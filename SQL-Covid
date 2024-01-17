
--SELECT  *
--FROM portfolio.covid_vac
--order by 3,4

SELECT  location, date, total_cases, new_cases, total_deaths, population
FROM portfolio.covid_deaths
order by location, date

-- Looking at Total Cases vs Total Deaths
-- Shows likelihood of dying if you contracted covid in your country
SELECT location, date, total_cases, total_deaths, ((total_deaths/total_cases) * 100) as DeathPercentage
FROM portfolio.covid_deaths
WHERE location like '%States'
order by location, date


-- Total Cases vs Population
-- Percentage of population that got covid
-- Can be filtered by country
SELECT location, date, population, total_cases, ((total_cases/population) * 100) as InfectionPercentage
FROM portfolio.covid_deaths
--WHERE location like '%States'
ORDER BY location, date

-- Countries with highest infection rate compared to population

SELECT location, population, MAX(total_cases) AS HighestInfectionCount, MAX((total_cases/population) * 100) as InfectionPercentage
FROM portfolio.covid_deaths
GROUP BY location, population
ORDER BY InfectionPercentage DESC


-- Countries with the highest death count per population

SELECT location, MAX(total_deaths) AS TotalDeathCount
FROM portfolio.covid_deaths
WHERE continent IS NOT null
GROUP BY location
ORDER BY TotalDeathCount DESC


-- Continents with the highest death count per population
SELECT continent, SUM(DISTINCT population) AS ContinentPopulation, SUM(new_deaths) AS SumOfDeaths, (SUM(new_deaths)/SUM(DISTINCT population)) * 100 AS DeathsOverTotalPop
FROM portfolio.covid_deaths
WHERE continent IS NOT NULL
GROUP BY continent
ORDER BY SumOfDeaths DESC


-- Global numbers

SELECT  SUM(new_cases) as total_cases, SUM(new_deaths) as total_deaths, SUM(new_deaths)/SUM(new_cases)*100 as DeathPercentage
FROM portfolio.covid_deaths
WHERE continent IS NOT NULL


-- Total population vs vaccinations

WITH Pop_vs_vac AS
(SELECT dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations, SUM(vac.new_vaccinations)
OVER (Partition by dea.Location ORDER BY dea.location, dea.date) AS RollingPeopleVaccinated
FROM portfolio.covid_deaths AS dea
JOIN portfolio.covid_vac AS vac
  ON dea.location = vac.location
  AND dea.date = vac.date
WHERE dea.continent IS NOT NULL
)
SELECT *, (RollingPeopleVaccinated/Population)*100 AS PercentVacc
FROM Pop_vs_vac


-- Creating View to store data for later visualizations

CREATE VIEW portfolio.Percent_Vaccinated as
WITH Pop_vs_vac AS
(SELECT dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations, SUM(vac.new_vaccinations)
OVER (Partition by dea.Location ORDER BY dea.location, dea.date) AS RollingPeopleVaccinated
FROM portfolio.covid_deaths AS dea
JOIN portfolio.covid_vac AS vac
  ON dea.location = vac.location
  AND dea.date = vac.date
WHERE dea.continent IS NOT NULL
)
SELECT *, (RollingPeopleVaccinated/Population)*100 AS PercentVacc
FROM Pop_vs_vac
