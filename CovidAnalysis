-- Total Cases vs. Total Deaths
SELECT 
  location, 
  date, 
  total_cases, 
  total_deaths,
  (total_deaths / total_cases) * 100 AS death_percentage
FROM 
  `covid-393715.Covid.CovidDeaths` 
WHERE 
  continent IS NOT NULL
ORDER BY 1,2;

-- Total Cases vs. Population
-- Shows percentage of population who got Covid

SELECT 
  location, 
  date, 
  population,
  total_cases, 
  (total_cases / population) * 100 AS covid_percentage
FROM 
  `covid-393715.Covid.CovidDeaths` 
WHERE 
  continent is not null
ORDER BY 1,2;

-- Countries with highest infection rate compared to population

SELECT 
  location, 
  population,
  MAX(total_cases) AS highest_covid_count, 
  (MAX(total_cases) / population) * 100 AS covid_percentage
FROM 
  `covid-393715.Covid.CovidDeaths` 
GROUP BY 
  location, 
  population
ORDER BY covid_percentage DESC;

-- Countries with highest death count per population
SELECT 
  location, 
  MAX(total_deaths) AS total_death_count
FROM 
  `covid-393715.Covid.CovidDeaths` 
WHERE 
  continent is not null
GROUP BY 
  location, 
  population
ORDER BY total_death_count DESC;

-- Break down above by continent:

SELECT 
  location, 
  MAX(total_deaths) AS total_death_count
FROM 
  `covid-393715.Covid.CovidDeaths` 
WHERE 
  continent is null AND
  location not like '%income%' AND
  location !='World' AND
  location != "European Union"
GROUP BY 
  location
ORDER BY 
  total_death_count DESC;

-- GLOBAL NUMBERS

SELECT
  date,
  SUM(new_cases) AS total_cases,
  SUM(new_deaths) AS total_deaths,
  SUM(new_deaths) / SUM(New_Cases) * 100 as death_percentage
FROM 
  `covid-393715.Covid.CovidDeaths`
WHERE
  continent is not null AND
  new_deaths > 0 AND
  new_cases > 0
GROUP BY
  date
order by
  1,2;

-- Total Population vs Vaccinations
-- Count restarts for each country

WITH PopvsVac AS
(
  SELECT
    dea.continent,
    dea.location,
    dea.date,
    dea.population,
    vac.new_vaccinations,
    SUM(vac.new_vaccinations) OVER (PARTITION BY dea.location ORDER BY dea.location, dea.date) AS rolling_people_vaccinated
  FROM
    `covid-393715.Covid.CovidDeaths` AS dea
  JOIN 
    `covid-393715.Covid.CovidVaccinations` AS vac
  ON
    dea.location = vac.location AND
    dea.date = vac.date
  WHERE
    dea.continent is not null
)
SELECT 
  *, 
  (rolling_people_vaccinated / population) * 100 AS rolling_percent_vaccinated
FROM 
  PopvsVac;

-- Common Table Expressions (CTEs)

WITH PercentPopulationVaccinated AS (
  SELECT
    dea.continent,
    dea.location,
    dea.date,
    dea.population,
    vac.new_vaccinations,
    SUM(vac.new_vaccinations) OVER (PARTITION BY dea.location ORDER BY dea.location, dea.date) AS rolling_people_vaccinated
  FROM
    `covid-393715.Covid.CovidDeaths` AS dea
  JOIN 
    `covid-393715.Covid.CovidVaccinations` AS vac
  ON
    dea.location = vac.location AND
    dea.date = vac.date
  WHERE
    dea.continent IS NOT NULL
)

-- Main Query

SELECT 
  *, 
  (rolling_people_vaccinated / population) * 100 AS percent_population_vaccinated
FROM 
  PercentPopulationVaccinated;


CREATE VIEW covid-393715.Covid.RollingGlobalVaccinationsPercentPopulation AS
WITH PercentPopulationVaccinated AS (
  SELECT
    dea.continent,
    dea.location,
    dea.date,
    dea.population,
    vac.new_vaccinations,
    SUM(vac.new_vaccinations) OVER (PARTITION BY dea.location ORDER BY dea.location, dea.date) AS rolling_people_vaccinated
  FROM
    `covid-393715.Covid.CovidDeaths` AS dea
  JOIN 
    `covid-393715.Covid.CovidVaccinations` AS vac
  ON
    dea.location = vac.location AND
    dea.date = vac.date
  WHERE
    dea.continent IS NOT NULL
)

-- Main Query

SELECT 
  *, 
  (rolling_people_vaccinated / population) * 100 AS percent_population_vaccinated
FROM 
  PercentPopulationVaccinated;

