# Covid Data SQL Queries
---

*  ```
	SELECT *
    -> FROM Portfolio_Projects..covid_data
    -> ORDER BY 3,4 ;
    ```
	
*  ```
	SELECT location, date, total_cases, new_cases, total_deaths, population
    -> FROM Portfolio_Projects..covid_data
    -> ORDER BY 1,2
   ```

*  ```
	SELECT continent, MAX(total_deaths) as TD
	-> FROM Portfolio_Projects..covid_data
	-> WHERE continent is not null
	-> GROUP BY continent
	-> ORDER BY TD DESC
   ```

*  ```
	SELECT location, date, total_cases, new_cases, total_deaths, new_deaths
	-> FROM Portfolio_Projects..covid_data
	-> WHERE location = 'India'
	-> ORDER BY date desc
   ```

*  ```
	SELECT Distinct(location), population
	-> FROM Portfolio_Projects..covid_data
	-> WHERE continent is not null
	-> ORDER BY population DESC
   ```
   
*  ```   
	SELECT sum(Distinct(population))
	-> FROM Portfolio_Projects..covid_data
	-> WHERE continent is not null
   ```
	
1. Global - Total confirmed cases, Total deaths, Mortality rate
   ```
	SELECT sum(new_cases) as Total_Cases, sum(cast(new_deaths as int)) as Total_Deaths, 
	->	(sum(cast(new_deaths as int))/sum(new_cases))*100 as Mortality_rate
	-> FROM Portfolio_Projects..covid_data
   ```
	
2.  Total confirmed cases and deaths in each country
   ```
	SELECT location, MAX(total_cases) as TotalCases, Max(cast(total_deaths as int)) as TotalDeaths
	-> FROM Portfolio_Projects..covid_data
	-> WHERE continent is not null
	-> GROUP BY location
	-> ORDER BY 1
   ```
   ```
	SELECT location, sum(new_cases) as TC, sum(cast(new_deaths as int)) as TD
	-> FROM Portfolio_Projects..covid_data
	-> WHERE continent is not null
	-> GROUP BY location
	-> ORDER BY TC desc
   ```
	
3. Daily % change in confirmed cases
   ```
	SELECT date, sum(total_cases) as TotalCases, sum(new_cases) as NewCases, 
	->	(sum(new_cases)/sum(total_cases))*100 as Daily_PercentageChange
	-> FROM Portfolio_Projects..covid_data
	-> GROUP BY date
	-> ORDER BY date desc
   ```
	
4.  Last 7 days - new confirmed cases, deaths and vaccinations
   ```
	SELECT Top(7) date, sum(new_cases) as TotalCases, sum(cast(new_deaths as int)) as TotalDeath, 
	->	sum(cast(new_vaccinations as int)) as TotalVaccinations
	-> FROM Portfolio_Projects..covid_data
	-> WHERE continent is not null
	-> GROUP BY date
	-> ORDER BY date desc
   ```
	
5.  Countries with highest rate of vaccination
    ```
	SELECT Top(10) location, Max(cast(total_vaccinations as bigint)) as Total_vaccination
	-> FROM Portfolio_Projects..covid_data
	-> WHERE continent is not null
	-> GROUP BY location
	-> ORDER BY Total_vaccination desc
    ```
    ```
	SELECT Top(10) location, SUM(cast(new_vaccinations as bigint)) as Total_vaccination
	-> FROM Portfolio_Projects..covid_data
	-> WHERE continent is not null
	-> GROUP BY location
	-> ORDER BY Total_vaccination desc
    ```
	
6.  Percentage of population vaccinated and infected in each country
    ```
	SELECT location, population, MAX(total_cases) as Total_Infected, Max(cast(new_vaccinations as bigint)) as Total_Vaccinated,
	->	MAX((cast(new_vaccinations as bigint)/population))*100 as PercentPopulationVaccinated,
	->	MAX((total_cases)/population)*100 as PercentPopulationInfected
	-> FROM Portfolio_Projects..covid_data
	-> WHERE continent is not null
	-> GROUP BY location, population
	-> ORDER BY location
    ```
	
7.  Percentage of population infected in each country
    ```
	SELECT location, population, MAX(total_cases) as Total_Infected, MAX((total_cases)/population)*100 as PercentPopulationInfected
	-> FROM Portfolio_Projects..covid_data
	-> WHERE continent is not null
	-> GROUP BY location, population
	-> ORDER BY location
    ```
	
8.  Temp Table
    ```
	DROP TABLE if exists PercentagePeopleVaccinated
	-> CREATE TABLE PercentagePeopleVaccinated
	-> (
	->	location nvarchar(255),
	->	date datetime,
	->	population numeric,
	->	new_vaccination numeric,
	->	People_Vaccinated numeric,
	-> )
	-> Insert into PercentagePeopleVaccinated
	-> SELECT location, date, population, new_vaccinations, 
	->	SUM(cast(new_vaccinations as bigint)) OVER (partition by location order by location,date) as People_Vaccinated
	-> FROM Portfolio_Projects..covid_data
	-> WHERE continent is not null
	-> ORDER BY 1,2

	-> SELECT *, (People_vaccinated/population)*100 as PercentPeopleVaccinated
	-> FROM PercentagePeopleVaccinated
    ```
	
  ---
