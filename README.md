readme_content = """
# INTERNATIONAL NURSES AND HOUSING IN IRELAND ANALYSIS

## Project Overview
Ireland's healthcare system relies heavily on international nurses, yet their retention is challenged by the country's housing crisis. This project analyzes the impact of housing affordability on newly arrived international nurses. Despite a high per capita nurse count, many emigrate, and new recruits struggle to find affordable accommodation, impacting their settlement and mortgage access. This analysis highlights how the housing crisis directly affects the recruitment and retention of healthcare professionals.

## Data Sources and Cleaning

### CSO Population Data
- Downloaded from [data.gov.ie](https://data.gov.ie/dataset/g0420-population-per-county).
- Cleaned in Excel to retain county name and population.

### NMBI State of the Register 2025
- Downloaded PDF from [nmbi.ie](https://www.nmbi.ie/Registration/NMBI-state-of-the-Register).
- Manually extracted 'County' and 'Practicing 2025' from Table 3 and saved as a CSV.

### GeoHive (HSE Hospitals)
- Downloaded CSV from [geohive.ie](https://www.geohive.ie/datasets/feb34881088341bbbf80d86af6a4f333_0/explore?location=53.378980%2C-8.239397%2C7).
- **Excel Cleaning:** FullAddress concatenation, Dublin Eircode correction, de-duplication, County extraction, and manual addition of 'beds' column.
- **Python Cleaning:** Dropped unnecessary columns and created a GeoDataFrame for hospital locations, saving it as `hospital_locations.shp`.
  - *AI Help Used:* Gemini 2.5 Flash assisted in resolving an `AttributeError` when saving to `.shp` by guiding the conversion of a Pandas DataFrame to a GeoPandas GeoDataFrame.

### Residential Property Price Register (PPR)
- Raw CSV (`ppr_2025_raw.csv`) downloaded.
- Initial attempts to clean 'Price ()' in Python led to errors, so the raw file was formatted in Excel to remove currency symbols.
- Dropped irrelevant columns and calculated `Average House Price` per county.

### RTB Average Monthly Rent
- Raw CSV (`rtb_average_monthly_rent_report_raw.csv`) downloaded.
- Cleaned in Python, including dropping irrelevant columns and `NaN` values.
- Extracted 'County' from 'Location' string.
- **AI Help Used:** Gemini 2.5 Flash helped debug an issue where 'Westmeath' was not appearing in the average rent calculations by suggesting sorting the `irish_counties` list by length in descending order to prioritize longer county names during extraction.
- Calculated `Average Rent Price` per county.

## Data Transformation

All cleaned datasets were merged into a `master_df` for comprehensive analysis.

### New Variables Created:
- **Estimated International Nurses:** Calculated as 48% of 'Total Nurses' based on NMBI data (due to lack of county-level breakdown).
- **Hospital density per 10,000:** (`Hospitals / Population * 10000`)
- **Bed density per 10,000:** (`Total Beds / Population * 10000`)
- **Nurse density per 10,000:** (`Estimated Intl Nurses / Population * 10000`)
- **Rental Affordability Ratio:** (`(Average Rent Price * 12) / Nurse Salary`). Counties exceeding 0.30 (30%) are flagged as high-cost areas for newly recruited nurses, based on a starting nurse salary of €34,000.

## Key Visualizations and Findings

### Line Plots: Stats Overview by County
- Showed strong correlations between 'Population', 'Total Nurses', and 'Estimated Intl Nurses', indicating that higher population counties generally have more nurses.

### Bar Charts
- **Nurses per 10k Population by County:** Sligo showed significantly higher nurse density despite its lower population, exceeding Dublin's by over 25%.
- **Beds per 10k Population by County:** Sligo also had a notably higher bed density, surpassing Waterford by over 33%.
- **Hospitals per 10k Population by County:** Donegal had the highest hospital density, but this was attributed to many smaller community hospitals rather than large general/acute facilities.

### Scatter Plots
- **International Nurses per County vs. Rent Affordability by County:** Revealed a countrywide affordability crisis, with all counties exceeding the 30% rent burden threshold. Dublin, as expected, had the highest number of international nurses and the highest rent burden. The analysis noted that a majority of counties surpassed 40% rent burden, suggesting significant strain even with potential house sharing.

### Maps
- **Map of Ireland with County Boundaries:** Used GeoPandas to display county shapes.
- **Map of Ireland with Hospitals:** Plotted hospital locations on the county map. A clear concentration in Dublin was observed, with significant gaps along Galway, Roscommon, Mayo borders, and the South East, potentially impacting emergency response. *AI Help Used:* Gemini 2.5 Flash assisted in re-projecting hospital coordinates to match the county shapefile's CRS for accurate plotting.
- **Hospital Locations and Bed Count:** Interactive map showing hospital names and bed counts on hover.

## Limitations

Several factors limited the analysis:
- **Hospital Data Gaps:** No figures for nurses in private nursing homes or precise nurse counts per hospital. Inconsistencies in reported bed figures were noted, and Longford had no registered HSE hospitals. The analysis could have been enhanced by categorizing hospitals by type.
- **Workforce Context Missing:** Analysis focused solely on HSE hospitals, omitting nurses in private nursing homes. No specific data for newly arrived international nurses at a granular level.
- **Housing Cost Limitations:** Rental affordability ratio assumed one person paying rent for an entire property, which might overestimate the burden for those in shared accommodation. House price data was deemed less relevant for newly employed nurses due to mortgage access difficulties.

## Conclusion

The analysis highlighted correlations between population and nurse numbers, identified varying hospital and bed densities, and revealed a significant nationwide housing affordability crisis for nurses. All counties exceeded the 30% rent burden threshold, posing a barrier to recruitment and retention. Geographical gaps in hospital distribution were also noted, impacting access to care. Further analysis with more detailed nurse distribution and housing cost data would provide a clearer picture.
"""

with open('README.md', 'w') as f:
    f.write(readme_content)

print("README.md generated successfully!")
