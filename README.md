# MIST 4610 Group Project 2

## Team Name
**Group 6**

## Team Members

| Name | GitHub |
|------|--------|
| Elher Zemihret | [Ezemihret](https://github.com/Ezemihret/SQLGROUPPPROJECT) |
| Allison Davis | [AllisonDavis149](https://github.com/AllisonDavis149/GP1) |
| Coen Cardelli | [CoenCardelli](https://github.com/CoenCardelli/SQL) |
| Charlotte Holzapfel | [charlotteholzapfel](https://github.com/charlotteholzapfel/Group-Project-1) |

---

## Dataset Description

**Dataset:** Fatality Analysis Reporting System (FARS) 2022 Final Release – Fatal Motor Vehicle Accidents

**Source:** [U.S. Department of Transportation / National Highway Traffic Safety Administration (NHTSA)](https://catalog.data.gov/dataset/fatality-analysis-reporting-system-fars-2023-accidents) via the U.S. Data.gov open data catalog. The dataset is part of the Bureau of Transportation Statistics (BTS) National Transportation Atlas Database (NTAD).

**Dimensions:** 39,681 rows × 84 columns

**About the Dataset:**

The FARS dataset is a nationwide census of fatal motor vehicle traffic crashes compiled annually by NHTSA. To qualify for inclusion, a crash must involve a motor vehicle on a public roadway and result in the death of at least one person (vehicle occupant or non-motorist) within 30 days of the crash. Each record represents one unique crash event. Data is gathered by trained state analysts from police crash reports, death certificates, state vehicle registration files, and hospital records.

**Key Columns and Data Types:**

| Column | Type | Description |
|--------|------|-------------|
| ST_CASE | Integer | Unique crash case number |
| STATE / STATENAME | Integer / String | State code and name where crash occurred |
| COUNTY / COUNTYNAME | Integer / String | County code and name |
| CITY / CITYNAME | Integer / String | City code and name |
| MONTH / MONTHNAME | Integer / String | Month of crash |
| DAY / DAYNAME | Integer / String | Day of month |
| DAY_WEEK / DAY_WEEKNAME | Integer / String | Day of week (1=Sunday through 7=Saturday) |
| YEAR | Integer | Year of crash (2022) |
| HOUR / HOURNAME | Integer / String | Hour of crash (0–23) |
| MINUTE | Integer | Minute of crash |
| FATALS | Integer | Number of fatalities in the crash |
| PEDS | Integer | Number of pedestrians involved |
| VE_TOTAL | Integer | Total number of vehicles involved |
| PERSONS | Integer | Total number of persons involved |
| DRUNK_DR | Integer | Number of drunk drivers involved |
| RUR_URB / RUR_URBNAME | Integer / String | Rural vs. urban classification |
| LGT_COND / LGT_CONDNAME | Integer / String | Lighting conditions (daylight, dark, dusk, etc.) |
| WEATHER / WEATHERNAME | Integer / String | Atmospheric conditions (clear, rain, snow, fog, etc.) |
| HARM_EV / HARM_EVNAME | Integer / String | First harmful event in the crash |
| MAN_COLL / MAN_COLLNAME | Integer / String | Manner of collision |
| FUNC_SYS / FUNC_SYSNAME | Integer / String | Functional road classification |
| RD_OWNER / RD_OWNERNAME | Integer / String | Road ownership (state, federal, local, etc.) |
| NHS / NHSNAME | Integer / String | Whether road is part of National Highway System |
| WRK_ZONE / WRK_ZONENAME | Integer / String | Work zone involvement |
| REL_ROAD / REL_ROADNAME | Integer / String | Crash location relative to roadway |
| RELJCT1 / RELJCT2 | Integer | Relation to junction |
| ROUTE / ROUTENAME | Integer / String | Route type (interstate, US highway, state road, etc.) |
| SP_JUR / SP_JURNAME | Integer / String | Special jurisdiction (national park, military, etc.) |
| LATITUDE / LONGITUD | Double | Geographic coordinates of crash |
| SCH_BUS / SCH_BUSNAME | Integer / String | School bus involvement |
| RAIL / RAILNAME | String | Railroad crossing involvement |
| ARR_HOUR / ARR_MIN | Integer | Time of EMS/emergency arrival |
| HOSP_HR / HOSP_MN | Integer | Time of hospital admission |

---

## Questions

### Question 1
**How do lighting conditions and time of day interact to influence the frequency and severity of fatal crashes across the United States?**

**Why It Is Interesting and Important:**

Traffic fatalities represent one of the leading causes of preventable death in the United States. Understanding the relationship between lighting conditions (captured in `LGT_COND`) and time of day (`HOUR`) is critical for informing infrastructure investment decisions such as roadway lighting expansion, reflective signage upgrades, and speed limit adjustments during nighttime hours. From a public policy standpoint, identifying which hours and lighting environments produce the most fatal crashes enables targeted safety campaigns and funding allocation by state and federal transportation agencies. This question also has a direct economic dimension — fatal crashes carry enormous societal costs including emergency response, lost productivity, and healthcare expenditure, all of which can be reduced with better-targeted interventions.

**Connection to Dataset:** Uses `LGT_COND`, `LGT_CONDNAME`, `HOUR`, `HOURNAME`, `FATALS`, and `STATENAME` columns.

---

### Question 2
**What combination of environmental and behavioral factors — including weather conditions, alcohol involvement, and rural vs. urban setting — is most strongly associated with higher fatality counts in fatal crashes?**

**Why It Is Interesting and Important:**

Fatal crash severity is rarely the result of a single factor. This question examines the interaction of weather (`WEATHER`), drunk driving (`DRUNK_DR`), and rural/urban context (`RUR_URB`) to understand which combination of conditions drives the deadliest outcomes. Socially, this matters because rural communities face disproportionately higher fatality rates due to longer emergency response times and road design differences — analyzing this alongside behavioral factors like alcohol provides a more complete picture. Economically, the findings can guide where sobriety checkpoint funding, rural road safety improvements, and adverse weather driving campaigns would have the greatest impact. For policymakers and public health officials, understanding these compound risk factors is essential for designing multi-layered interventions rather than single-cause solutions.

**Connection to Dataset:** Uses `WEATHERNAME`, `DRUNK_DR`, `RUR_URBNAME`, `FATALS`, `STATENAME`, and `PERSONS` columns.

---

## Manipulations Applied to the Dataset

The following manipulations and calculated fields were applied in Tableau to support the analysis:

**1. Calculated Field – Average Fatalities per Crash:**
A calculated field was created by dividing `FATALS` by the number of crash records (`ST_CASE` count) to produce an average fatalities per crash metric. This allows for comparison of crash severity across different conditions independent of raw crash volume.

**2. Grouping Hour into Time-of-Day Bins (Question 1):**
The `HOUR` field (0–23) was grouped into four time-of-day categories — Early Morning (12am–5am), Morning (6am–11am), Afternoon (12pm–5pm), and Evening/Night (6pm–11pm) — using a calculated field to make the time dimension more interpretable in visualizations.

**3. Filter on Known/Valid Values:**
Records where `HOUR` was coded as 99 ("Unknown") or `WEATHER` was coded as 98/99 (unknown/not reported) were excluded from the relevant visualizations to ensure analytical accuracy and avoid misleading patterns from missing data.

**4. Drunk Driver Flag:**
The `DRUNK_DR` field (number of drunk drivers per crash) was used as both a quantitative measure and converted to a binary dimension (0 = No drunk driver, 1+ = Drunk driver involved) for certain views to simplify comparison across rural/urban contexts and weather conditions.

---

## Analysis and Results

### Question 1 – Lighting Conditions and Time of Day

**Visualization:** Heatmap of crash frequency by hour of day (x-axis) and lighting condition (y-axis), colored by total number of fatal crashes. A secondary bar chart shows average fatalities per crash by lighting condition.

**Findings:**

The analysis reveals that dark roadways without lighting are consistently the most dangerous environment for fatal crashes, particularly during the late-night and early-morning hours (10pm–3am). Despite lower overall traffic volume at night, fatality rates are significantly elevated compared to daytime hours. Daylight crashes are far more frequent in absolute terms due to higher traffic volume but produce fewer fatalities per crash on average. Dawn and dusk represent transitional risk periods where lighting is inconsistent and driver visibility is compromised, producing fatality spikes disproportionate to their traffic share.

*[Insert Tableau visualization here]*

**Implications:** States with high proportions of rural roads and limited street infrastructure should prioritize lighting upgrades on high-speed corridors. Nighttime speed limit reductions and increased patrols during late-night hours may also reduce severity in unlit environments.

---

### Question 2 – Environmental and Behavioral Risk Factors

**Visualization:** A stacked bar chart and Tableau dashboard showing average fatalities per crash broken down by weather condition and rural/urban setting, with drunk driver involvement as a filter/color dimension.

**Findings:**

Rural crashes consistently produce higher average fatalities per crash than urban crashes across all weather conditions and regardless of alcohol involvement. The highest-severity crashes cluster around the intersection of rural setting, clear weather (which may promote higher speeds), and drunk driver involvement. Notably, adverse weather (rain, snow, fog) does not independently drive the highest fatality counts — likely because drivers reduce speed in poor conditions. Instead, the riskiest crashes tend to occur in conditions where drivers feel a false sense of safety, such as dry roads at night.

*[Insert Tableau visualization here]*

**Implications:** Rural sobriety enforcement and DUI checkpoint programs represent high-impact interventions. Public health messaging campaigns should target clear-weather nighttime driving rather than focusing exclusively on adverse weather scenarios, which are already associated with cautious behavior.

---

## Tableau Packaged Workbook

The Tableau packaged workbook (.twbx) file is included in this repository.

---

## References

- National Highway Traffic Safety Administration. (2022). *Fatality Analysis Reporting System (FARS) 2022 Final Release.* U.S. Department of Transportation. https://catalog.data.gov/dataset/fatality-analysis-reporting-system-fars-2023-accidents
- Bureau of Transportation Statistics. *National Transportation Atlas Database (NTAD).* https://www.bts.gov/ntad
