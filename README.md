# PortfolioProjects

--SELECT DATA THAT WE ARE GOING TO BE USING

SELECT location, date, total_cases, new_cases, total_deaths, population 
FROM `my-project-covid-368601.covid_deaths.covid_deaths` 
WHERE continent is not NULL
ORDER BY location ASC;

--LOOKING AT TOTAL CASES vs TOTAL DEATHS
--Shows likelihood  of dying if you contract Covid in your country

SELECT location, date, total_cases, total_deaths, (total_deaths/total_cases)*100 AS death_percentage 
FROM `my-project-covid-368601.covid_deaths.covid_deaths` 
WHERE location = 'United States'
ORDER BY location ASC;

--LOOKING AT TOTAL CASES vs POPULATION
--Shows what percentage  of population got Covid

SELECT location, date, population, total_cases, (total_cases/population)*100 AS percent_population_infected 
FROM `my-project-covid-368601.covid_deaths.covid_deaths` 
WHERE location = 'United States'
ORDER BY location ASC;

--LOOKING AT COUNTRIES WITH HIGHEST INFECTION RATE COMPARED TO POPULATION

SELECT location, population, MAX(total_cases) AS HighestInfectionCount, MAX((total_cases/population))*100 AS percent_population_infected
FROM `my-project-covid-368601.covid_deaths.covid_deaths` 
GROUP BY location, population
ORDER BY percent_population_infected DESC;

--SHOWING COUNTRIES WITH HIGHEST DEATH COUNT PER POPULATION

SELECT location, MAX(cast(total_deaths AS int)) AS TotalDeathCount
FROM `my-project-covid-368601.covid_deaths.covid_deaths` 
WHERE continent is not NULL
GROUP BY location
ORDER BY TotalDeathCount DESC;

--LET'S BREAK DOWN THINGS BY CONTINENT
--SHOWING CONTINENTS WITH HIGHEST DEATH COUNT PER POPULATION

SELECT continent, MAX(cast(total_deaths AS int)) AS TotalDeathCount
FROM `my-project-covid-368601.covid_deaths.covid_deaths` 
WHERE continent is not NULL
GROUP BY continent
ORDER BY TotalDeathCount DESC;

--other option, data more accured
SELECT location, MAX(cast(total_deaths AS int)) AS TotalDeathCount
FROM `my-project-covid-368601.covid_deaths.covid_deaths` 
WHERE continent is NULL
GROUP BY location
ORDER BY TotalDeathCount DESC;

--GLOBAL NUMBERS BY DATE
SELECT date, SUM(new_cases)AS total_cases, SUM(cast(new_deaths AS int))AS total_deaths, SUM(CAST(new_deaths AS int))/SUM(new_cases)*100 AS death_percentage 
FROM `my-project-covid-368601.covid_deaths.covid_deaths` 
WHERE continent IS NOT null
GROUP BY date
ORDER BY date;

--GLOBAL NUMBERS TOTAL
SELECT  SUM(new_cases)AS total_cases, SUM(cast(new_deaths AS int))AS total_deaths, SUM(CAST(new_deaths AS int))/SUM(new_cases)*100 AS death_percentage 
FROM `my-project-covid-368601.covid_deaths.covid_deaths` 
WHERE continent IS NOT null

SELECT * 
FROM `my-project-covid-368601.covid_deaths.covid_deaths` dea
JOIN `my-project-covid-368601.covid_deaths.covid_vaccinations` vac
  ON dea.location=vac.location
  AND dea.date=vac.date

--LOOKING AT TOTAL POPULATION VS VACCINATIONS

  SELECT dea.continent, dea.location, dea.date, dea. population, vac.new_vaccinations
FROM `my-project-covid-368601.covid_deaths.covid_deaths` dea
JOIN `my-project-covid-368601.covid_deaths.covid_vaccinations` vac
  ON dea.location=vac.location
  AND dea.date=vac.date
WHERE dea.continent IS NOT NULL
ORDER BY dea.location, dea.date
