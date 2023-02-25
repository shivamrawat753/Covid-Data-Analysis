--1.Global - Total confirmed cases, Total deaths, Mortality rate

select sum(new_cases) as Total_Cases, sum(cast(new_deaths as int)) as Total_Deaths, 
	(sum(cast(new_deaths as int))/sum(new_cases))*100 as Mortality_rate
from Portfolio_Projects..covid_data

--2.Total confirmed cases and deaths in each country

select location, MAX(total_cases) as TotalCases, Max(cast(total_deaths as int)) as TotalDeaths
from Portfolio_Projects..covid_data
where continent is not null
group by location
order by 1

select location, sum(new_cases) as TC, sum(cast(new_deaths as int)) as TD
from Portfolio_Projects..covid_data
where continent is not null
group by location
order by TC desc

--3.Daily % change in confirmed cases

select date, sum(total_cases) as TotalCases, sum(new_cases) as NewCases, (sum(new_cases)/sum(total_cases))*100 as Daily_PercentageChange
from Portfolio_Projects..covid_data
group by date
order by date desc

--4.Last 7 days - new confirmed cases, deaths and vaccinations

select Top(7) date, sum(new_cases) as TotalCases, sum(cast(new_deaths as int)) as TotalDeath, 
	sum(cast(new_vaccinations as int)) as TotalVaccinations
from Portfolio_Projects..covid_data
where continent is not null
group by date
order by date desc

--5.Countries with highest rate of vaccination

Select Top(10) location, Max(cast(total_vaccinations as bigint)) as Total_vaccination
from Portfolio_Projects..covid_data
where continent is not null
group by location
order by Total_vaccination desc

Select Top(10) location, SUM(cast(new_vaccinations as bigint)) as Total_vaccination
from Portfolio_Projects..covid_data
where continent is not null
group by location
order by Total_vaccination desc


--6. Percentage of population vaccinated and infected in each country

select location, population, MAX(total_cases) as Total_Infected, Max(cast(new_vaccinations as bigint)) as Total_Vaccinated,
	MAX((cast(new_vaccinations as bigint)/population))*100 as PercentPopulationVaccinated,
	MAX((total_cases)/population)*100 as PercentPopulationInfected
from Portfolio_Projects..covid_data
where continent is not null
group by location, population
order by location

--.Percentage of population infected in each country

select location, population, MAX(total_cases) as Total_Infected, MAX((total_cases)/population)*100 as PercentPopulationInfected
from Portfolio_Projects..covid_data
where continent is not null
group by location, population
order by location


-- TEMP TABLE
DROP TABLE if exists PercentagePeopleVaccinated
CREATE TABLE PercentagePeopleVaccinated
(
	location nvarchar(255),
	date datetime,
	population numeric,
	new_vaccination numeric,
	People_Vaccinated numeric,
)
Insert into PercentagePeopleVaccinated
select location, date, population, new_vaccinations, 
	SUM(cast(new_vaccinations as bigint)) OVER (partition by location order by location,date) as People_Vaccinated
from Portfolio_Projects..covid_data
where continent is not null
order by 1,2

Select *, (People_vaccinated/population)*100 as PercentPeopleVaccinated
from PercentagePeopleVaccinated


---

*  ```
	SELECT *
    ->FROM Portfolio_Projects..covid_data
    ->ORDER BY 3,4 ;
    ```
	
*  ```
	select location, date, total_cases, new_cases, total_deaths, population
    ->from Portfolio_Projects..covid_data
    ->order by 1,2
   ```

*  ```
	select continent, MAX(total_deaths) as TD
	->from Portfolio_Projects..covid_data
	->where continent is not null
	->group by continent
	->order by TD DESC
   ```

*  ```
	select location, date, total_cases, new_cases, total_deaths, new_deaths
	->from Portfolio_Projects..covid_data
	->where location = 'India'
	->order by date desc
   ```

*  ```
	select Distinct(location), population
	->from Portfolio_Projects..covid_data
	->where continent is not null
	->order by population DESC
   ```
   
*  ```   
	select sum(Distinct(population))
	->from Portfolio_Projects..covid_data
	->where continent is not null
    ```
	
---1. Global - Total confirmed cases, Total deaths, Mortality rate
   ```
	select sum(new_cases) as Total_Cases, sum(cast(new_deaths as int)) as Total_Deaths, 
	(sum(cast(new_deaths as int))/sum(new_cases))*100 as Mortality_rate
	->from Portfolio_Projects..covid_data
    ```
	
4.  ```
	SELECT * FROM emp WHERE name LIKE '%Jyoti%' AND sal > 900000.00 ;
    ```
	
5.  ```
	CREATE VIEW emp_dept AS ( SELECT emp.enum AS enum, emp.name AS name, dept.name AS dept_name, dept.area AS area
    -> FROM emp, dept WHERE emp.dnum = dept.dnum ) ;
    ```
	
6.  ```
	SELECT * FROM emp_Dept GROUP BY dnum ASC ;
    ```
	
7.  ```
	SELECT enum, name FROM emp
    -> WHERE sal BETWEEN  140000.00 AND 500000.00
    -> AND tax = 0 ;
    ```
	
8.  ```
	SELECT AVG( tax ) AS avg_tax FROM emp GROUP BY dnum ;
    ```
	
9.  ```
	INSERT INTO dept VALUES
    -> ( 25, 'Administration', 'S', 25 ) ;
	
	 INSERT INTO supplier VALUES
    -> ( 125, 'Eureka Solutions', 'DLH' ) ;
	
	INSERT INTO supply VALUES
    -> ( 'ST99', 125, 25, 800 ) ;
    ```
	
10. ```
	 -> SELECT * FROM emp ;
	 -> SELECT * FROM dept ;
	 -> SELECT * FROM supply ;
	 -> SELECT * FROM supplier ;
     ```
	
  ---
