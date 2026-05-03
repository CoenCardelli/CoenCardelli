# MIST 4610 Group Project 2

## Team Name
**Group 1**

## Team Members

| Name | GitHub |
|------|--------|
| Elher Zemihret | [Ezemihret](https://github.com/Ezemihret/SQLGROUPPPROJECT) |
| Allison Davis | [AllisonDavis149](https://github.com/AllisonDavis149/Group-Project-2) |
| Coen Cardelli | [CoenCardelli](https://github.com/CoenCardelli/CoenCardelli) |
| Charlotte Holzapfel | [charlotteholzapfel](https://github.com/charlotteholzapfel/Group-Project-1) |

---

## Dataset Description

**Dataset:** Fatality Analysis Reporting System (FARS) 2022 Final Release – Fatal Motor Vehicle Accidents

**Source:** [U.S. Department of Transportation / National Highway Traffic Safety Administration (NHTSA)](https://catalog.data.gov/dataset/fatality-analysis-reporting-system-fars-2023-accidents) via the U.S. Data.gov open data catalog. The dataset is part of the Bureau of Transportation Statistics (BTS) National Transportation Atlas Database (NTAD).

**Dimensions:** 39,681 rows × 83 columns

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

The table above covers the primary analytical columns used in this project. The full dataset contains 83 fields total, including supporting label/name fields for each coded variable and geographic coordinates in both decimal degree (LATITUDE/LONGITUD) and projected Web Mercator (x/y) formats.

---

## Questions

### Question 1
**How do lighting conditions and time of day interact to influence the frequency and severity of fatal crashes across the United States?**

**Why It Is Interesting and Important:**

Traffic fatalities represent one of the leading causes of preventable death in the United States, and the FARS dataset's combination of `LGT_COND` and `HOUR` fields makes it uniquely suited to examine how visibility and time of day compound crash risk. Understanding this relationship is critical for informing infrastructure investment decisions such as roadway lighting expansion, reflective signage upgrades, and speed limit adjustments during nighttime hours. From a public policy standpoint, identifying which hours and lighting environments produce the most fatal crashes — using the 39,681 crash records in this dataset — enables targeted safety campaigns and funding allocation by state and federal transportation agencies. This question also has a direct economic dimension: fatal crashes carry enormous societal costs including emergency response, lost productivity, and healthcare expenditure, all of which can be reduced with better-targeted interventions derived from the patterns this data reveals.

**Connection to Dataset:** Uses `LGT_COND`, `LGT_CONDNAME`, `HOUR`, `HOURNAME`, `FATALS`, and `STATENAME` columns.

---

### Question 2
**How do weather conditions and rural vs. urban classification interact to influence the severity of fatal crashes, and which combinations of these factors produce the highest average fatality counts?**

**Why It Is Interesting and Important:**

Fatal crash severity is shaped by a complex interplay of environmental and geographic factors, and the FARS dataset captures both through its `WEATHER` and `RUR_URB` fields across nearly 40,000 crash records — providing sufficient depth to analyze these interactions meaningfully. This question examines how atmospheric conditions and road setting combine to drive the deadliest outcomes. Socially, rural communities face disproportionately higher fatality rates due to longer emergency response times, higher speed limits, and less-forgiving road design — understanding how weather compounds these risks provides a more complete picture of where lives are most at risk. Economically, the findings can guide state and federal decisions around rural road safety improvements, adverse weather driving campaigns, and emergency response infrastructure investment. The question is also significant because it challenges the intuitive assumption that adverse weather is always the primary driver of severity — the interaction between weather and rural context captured in this dataset tells a more nuanced story that single-variable analysis would miss.

**Connection to Dataset:** Uses `WEATHERNAME`, `RUR_URBNAME`, `FATALS`, `ST_CASE` (for crash count), `STATENAME`, and `PERSONS` columns.

---

## Manipulations Applied to the Dataset

The following manipulations and calculated fields were applied in Tableau to support the analysis:

**1. Calculated Field – Average Fatalities per Crash:**
A calculated field was created by dividing the sum of `FATALS` by the count of distinct crash records (`ST_CASE`) to produce an average fatalities per crash metric. This allows for fair comparison of crash severity across different conditions independent of raw crash volume — for example, rural areas have fewer total crashes than urban areas but may produce higher average fatalities per crash.

**2. Calculated Field – Time of Day:**
A calculated field named "Time of Day" was created to categorize the `HOUR` field (0–23) into four readable time periods: Early Morning (12am–5am), Morning (6am–11am), Afternoon (12pm–5pm), and Evening/Night (6pm–11pm). This field was used as a dimension in the analysis to make the time-of-day patterns more interpretable in visualizations.

**3. Hour Axis Range Adjustment:**
The `HOUR` field was initially displaying a default axis range of -2 to 25 in Tableau due to automatic padding. The axis was manually fixed to a custom range of 0 to 23 to accurately reflect the valid hour values in the dataset. Additionally, the aggregation for the Hour filter was changed from SUM to Count to ensure the field was being treated as a category rather than a numeric measure, producing the correct heatmap grid.

**4. Rural/Urban Filter:**
The `RUR_URBNAME` field contains several classification values beyond the core Rural and Urban categories, including "Not Reported", "Trafficway Not in System", and "Unknown". These non-meaningful categories were filtered out in the Question 2 visualizations to ensure the analysis focused only on the valid Rural vs. Urban comparison, avoiding skewed results from incomplete or inapplicable records.

---

## Analysis and Results

### Question 1 – Lighting Conditions and Time of Day

**Visualization:** Heatmap of crash frequency by hour of day (x-axis) and lighting condition (y-axis), colored by total number of fatal crashes. A secondary bar chart shows average fatalities per crash by lighting condition.

![Q1 Dashboard](https://raw.githubusercontent.com/CoenCardelli/CoenCardelli/main/4610_Q1.png)

**Findings:**

The heatmap reveals that Dark - Unknown Lighting and Dark - Not Lighted conditions produce the highest crash concentrations during late night and early morning hours (roughly 10pm–3am), despite significantly lower overall traffic volume during those hours. Daylight crashes are more frequent in absolute terms during peak afternoon hours (12pm–5pm) due to higher traffic volume, but the bar chart shows that Dark - Not Lighted crashes produce the highest average fatalities per crash — meaning when crashes happen in unlit dark conditions, they tend to be deadlier. Dawn crashes, while fewer in number, show a disproportionately high average fatality rate relative to their traffic share, suggesting that transitional lighting at sunrise creates particularly dangerous visibility conditions.

**Implications:** States with high proportions of rural roads and limited street infrastructure should prioritize lighting upgrades on high-speed corridors. Nighttime speed limit reductions and increased patrols during late-night hours may also reduce severity in unlit environments. The dawn spike suggests that driver awareness campaigns targeting early morning commuters could also reduce fatalities during this overlooked risk window.

---

### Question 2 – Weather Conditions and Rural vs. Urban Setting

**Visualization:** A heatmap cross-tabulating weather condition against rural/urban setting, colored by average fatalities per crash. A secondary bar chart shows average fatalities per crash broken down by weather condition with rural vs. urban classification as a color dimension.

![Q2 Dashboard](https://raw.githubusercontent.com/CoenCardelli/CoenCardelli/main/4610_Q2.png)

**Findings:**

Rural crashes consistently produce higher average fatalities per crash than urban crashes across every weather condition. The heatmap shows that Blowing Sand, Soil, or Dirt in rural settings produces the highest average fatality rate — a finding that points to visibility impairment on high-speed rural roads with no urban infrastructure to slow traffic. The bar chart reinforces that the Rural bar extends further right than Urban across all weather types, confirming the pattern is not weather-specific but structural. Notably, Clear weather in rural settings produces higher average fatalities than many adverse weather conditions in urban settings — directly challenging the assumption that bad weather is the primary crash severity driver. Drivers appear to exercise more caution in visibly poor weather, while clear conditions on rural roads encourage higher speeds and reduced vigilance.

**Implications:** Rural road safety campaigns should not focus exclusively on adverse weather scenarios. Public messaging targeting clear-weather, high-speed rural driving — particularly at night — could have significant impact. Infrastructure investments such as rumble strips, improved signage, and guardrails on rural highways would address the structural factors that make rural crashes so deadly regardless of weather. Federal highway safety funding should prioritize rural corridors where the combination of high speeds, limited infrastructure, and longer emergency response times compounds fatality risk across all weather conditions.

---

## Tableau Packaged Workbook

The Tableau packaged workbook (.twbx) file is included in this repository: [FARS_Group1.twbx](https://github.com/CoenCardelli/CoenCardelli/blob/main/FARS_Group1.twbx)

---

## References

- National Highway Traffic Safety Administration. (2022). *Fatality Analysis Reporting System (FARS) 2022 Final Release.* U.S. Department of Transportation. https://catalog.data.gov/dataset/fatality-analysis-reporting-system-fars-2023-accidents
- Bureau of Transportation Statistics. *National Transportation Atlas Database (NTAD).* https://www.bts.gov/ntad
