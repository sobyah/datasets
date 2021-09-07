Source data attributed to:
-------------------------

Australian Bureau of Meteorology (2019), ACORN-SAT v2, Snapshot v.2.1.0.1 ( Australian Climate Observations Reference Network - Surface Air Temperature ), 2019. { https://doi.org/10.25941/5d28a5d352de7 } Downloaded from ftp://ftp.bom.gov.au/anon/home/ncc/www/change/ACORN_SAT_daily/ on 16-Apr-2021.

Please refer to [ http://www.bom.gov.au/other/disclaimer.shtml ], for disclaimer details.

Data details:
------------
**Scope**: Australian States Surface air temperature data from BOM (www.bom.gov.au).

**Date Range**: Year 2000 - 2019

**Granularity**: 
-  Year
-  State
(Season-level measures)

**Measures**:
-  Autumn_tminavg: Min Daily Temperature, Averaged for Autumn
-  Autumn_tmaxavg: Max Daily Temperature, Averaged for Autumn
-  Spring_tminavg: Min Daily Temperature, Averaged for Spring
-  Spring_tmaxavg: Max Daily Temperature, Averaged for Spring
-  Summer_tminavg: Min Daily Temperature, Averaged for Summer
-  Summer_tmaxavg: Max Daily Temperature, Averaged for Summer
-  Winter_tminavg: Min Daily Temperature, Averaged for Winter
-  Winter_tmaxavg: Max Daily Temperature, Averaged for Winter
  
Season Definition taken from: http://www.bom.gov.au/climate/glossary/seasons.shtml

Spring - the three transition months September, October and November.
Summer - the three hottest months December, January and February.
Autumn - the transition months March, April and May.
Winter - the three coldest months June, July and August.

Wrangling Process / Steps:
-------------------------

1.	Extract Temperature Data
-	The 112 CSV files per Zipped files were extracted and processed using bash scripts. Each CSV file contained data from Sep 1941 to Dec 2019 which was filtered to get data for years 2000 to 2019 in CSV format.
-	Two intermediate delimited files were generated from above step containing fields Location, Date and Tmin or Tmax (Minimum or Maximum Temperate) based on corresponding file.
-	These file were loaded in R  and joined together based on location and date to get one file with minimum and maximum temperature data
2.	Reduce Granularity from Date to Season/Year
-	Year and Month was extracted from the date field
-	Season was determined based on month using Australia’s season information i.e.: “December to February is summer; March to May is autumn; June to August is winter; and September to November is spring” (ref: [LINK](https://www.australia.com/en/facts-and-planning/weather-in-australia.html#:~:text=Australia's%20seasons%20are%20at%20opposite,rainfall%20in%20Australia's%20capital%20cities.))
3.	Add Station/Location
-	This data is then combined with Stations information file to add Station/Location Name and Geographical information (Latitude and Longitude)
4.	Map data to States
-	This information is then merged with the postcode file to get corresponding States (Since water market data is on state level). 
-	This presented a challenge where there are localities with same name in multiple states. To cater this, the join condition was expanded to geographical coordinates (latitude and longitude). If one location was being mapped to multiple states, latitude for the two locations was compared with +/- 0.8 threshold. This mapped 99 out of 112 localities correctly. 13 localities were ignored since the overall data was checked to have good representation for states.
-	State codes were now mapped to State Names since Water Market data has state names instead of codes.
5.	Aggregate and Calculate average temperatures
-	This data is then aggregated to calculate the average minimum and average maximum temperature by Season, Year and State.
-	Missing values in temperature are removed in order for them to not effect the average. Since we are aggregating on 3-month data, a few daily observations missing do not effect the overall measurement
6.	Reshape Data
-	Data is then reshaped to have one row per state and year, columns are created for minimum and maximum temperature for the four Seasons

