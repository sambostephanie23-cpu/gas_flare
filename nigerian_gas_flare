with global_gas_flare as(
  select Country, Latitude, Longitude, bcm,Year ,MMscfd, `Field  Type`, `Field Name`, `Field  Operator`, Location as state, `Flare Level`,`Flaring Vol _million m3_` as flaring_vol_m3,
  (`Flaring Vol _million m3_` * 2400) as carbon_emissions_ton,
  (`Flaring Vol _million m3_` * 1000000 * 0.0035) AS estimated_regulatory_fine_usd,
  rank() over (partition by `Field  Operator` order by Country) as operators_per_country
  from `gas_flare.global`
  where `Flaring Vol _million m3_` is not null
  and MMscfd is not null
  and bcm is not null
  and Year is not null

),
country_level as(
  select country,
  sum(flaring_vol_m3) as Total_flaring_country_level,
  avg(flaring_vol_m3) as avg_flaring_country,
   count(*) as number_of_active_flaring_sites,
  RANK() OVER (ORDER BY SUM(flaring_vol_m3) DESC) AS country_global_rank
  from global_gas_flare
GROUP BY country
),
operator_level as(
  select `Field  Operator`,
   sum(flaring_vol_m3) as total_flaring_operator,
  COUNT(*) AS records_count,
   RANK() OVER (ORDER BY SUM(flaring_vol_m3) DESC) AS operator_global_rank
   from global_gas_flare
   group by `Field  Operator`),
 location_level as (
  select State,
   COUNT(*) AS records_count,
  RANK() OVER (ORDER BY SUM(flaring_vol_m3) DESC) AS location_global_rank
FROM global_gas_flare
group by State
),
annual_metics as (
  select Year,
  sum(flaring_vol_m3) as current_year_flaring,
  from global_gas_flare
  group by year
),
TIME_TREND_ANALYSIS as(
  select year,
  current_year_flaring,
  round(lag( current_year_flaring,1 ) over (order by Year),5) as previous_year_flare,
 round((current_year_flaring - lag( current_year_flaring,1 ) over (order by Year))/nullif(lag( current_year_flaring,1 ) over (order by Year),0) * 100,2)as yoy_percentage_change
 from annual_metics
 order by year
)
 
select global_gas_flare.*,
operator_level.operator_global_rank,
location_level.location_global_rank,
TIME_TREND_ANALYSIS.previous_year_flare,
TIME_TREND_ANALYSIS.yoy_percentage_change
from global_gas_flare
left join operator_level
on global_gas_flare.`Field  Operator` = operator_level.`Field  Operator`
left join location_level
on global_gas_flare.state = location_level.state
left join TIME_TREND_ANALYSIS
on global_gas_flare.year = TIME_TREND_ANALYSIS.year
where country = 'Nigeria'
;


