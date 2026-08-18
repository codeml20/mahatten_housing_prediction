# Manhattan Housing Prediction - Datasets Documentation

## Table of Contents
- [Overview](#overview)
- [1. NYC Citywide Annualized Sales](#1-nyc-citywide-annualized-sales-w2pb-icbu)
- [2. MapPLUTO](#2-mappluto-64uk-42ks)
- [3. Census Bureau ACS5](#3-census-bureau-acs5-2022)
- [4. MTA Subway Stations](#4-mta-subway-stations-39hk-dx4f)
- [Data Integration](#data-integration)
- [Data Quality & Missing Values](#data-quality--missing-values)
- [Notes for Analysis](#notes-for-analysis)

---

## Overview
This project uses three primary data sources from NYC Open Data and the Census Bureau to predict Manhattan housing prices. The datasets are merged using the BBL (Borough, Block, Lot) identifier and geographic coordinates.

---

## 1. NYC Citywide Annualized Sales (w2pb-icbu)

**Source:** NYC Department of Finance  
**Description:** Historical property sales data for Manhattan from 2021-2025, filtered for arm's length transactions (sales >= $100,000) to exclude foreclosures and family transfers.

### Fields:

| Field Name | Data Type | Description |
|---|---|---|
| `borough` | string | Borough code (1 = Manhattan) |
| `neighborhood` | string | Specific neighborhood name within Manhattan |
| `building_class_category` | string | Category describing building type (e.g., "13 CONDOS - ELEVATOR APARTMENTS") |
| `tax_class_as_of_final_roll` | string | Tax class of property as of final assessment roll |
| `block` | string | Tax block number of the property |
| `lot` | string | Tax lot number within the block |
| `building_class_as_of_final` | string | More specific building classification (e.g., "R4", "D4") |
| `address` | string | Street address of the property |
| `apartment_number` | string | Apartment number (for condos/co-ops) |
| `zip_code` | string | Postal ZIP code |
| `residential_units` | string | Number of residential units in building |
| `commercial_units` | string | Number of commercial units in building |
| `total_units` | string | Total units (residential + commercial) |
| `land_square_feet` | string | Land area in square feet |
| `gross_square_feet` | string | Gross building area in square feet |
| `year_built` | string | Year the building was constructed |
| `tax_class_at_time_of_sale` | string | Tax class at time of sale |
| `building_class_at_time_of_sale` | string | Building class at time of sale |
| `sale_price` | string | Sale price of the property (TARGET VARIABLE) |
| `sale_date` | string | Date of property sale (ISO 8601 format) |
| `latitude` | string | Geographic latitude coordinate |
| `longitude` | string | Geographic longitude coordinate |
| `community_board` | string | Community board district code |
| `council_district` | string | City council district code |
| `bin` | string | Building Identification Number |
| `bbl` | string | Borough, Block, and Lot number (unique property identifier) |
| `census_tract_2020` | string | Census tract from 2020 census |
| `nta` | string | Neighborhood Tabulation Area code |

**Key Statistics:**
- Records: 86,147 cleaned residential Manhattan sales
- Date Range: 2021-01-01 to 2025-07-01
- Residential Categories Included:
  - 13 CONDOS - ELEVATOR APARTMENTS (39,254)
  - 10 COOPS - ELEVATOR APARTMENTS (34,928)
  - 17 CONDO COOPS (5,272)
  - 09 COOPS - WALKUP APARTMENTS (4,140)
  - 15 CONDOS - 2-10 UNIT RESIDENTIAL (1,532)
  - 01 ONE FAMILY DWELLINGS (804)
  - 02 TWO FAMILY DWELLINGS (674)
  - 03 THREE FAMILY DWELLINGS (355)
  - 12 CONDOS - WALKUP APARTMENTS (585)

**Data Quality Notes:**
- `latitude` & `longitude`: 47,646 non-null (94.6%)
- `year_built`: Missing in some records
- `apartment_number`: Only 11,877 non-null (23.8%) - most applicable to condos/co-ops
- Deduplication applied on: `bbl`, `sale_date`, `sale_price`, `apartment_number`

---

## 2. MapPLUTO (64uk-42ks)

**Source:** NYC Department of City Planning  
**Description:** Property Land Use and Tax information database for Manhattan. Provides detailed zoning, land use classification, building characteristics, and property assessment data.

**Records:** 42,544 Manhattan properties  
**Join Key:** BBL (Borough, Block, Lot) number

### Selected Fields (96 total columns):

| Field Name | Description |
|---|---|
| `borough` | Borough designation ("MN" for Manhattan) |
| `block` | Tax block number |
| `lot` | Tax lot number |
| `bbl` | Complete BBL identifier |
| `address` | Property street address |
| `zipcode` | ZIP code |
| `bldgclass` | Building class code |
| `landuse` | Land use classification |
| `lotarea` | Lot area in square feet |
| `bldgarea` | Building area in square feet |
| `comarea` | Commercial area |
| `resarea` | Residential area |
| `officearea` | Office space area |
| `retailarea` | Retail space area |
| `garagearea` | Garage area |
| `strgearea` | Storage area |
| `factryarea` | Factory area |
| `numbldgs` | Number of buildings on lot |
| `numfloors` | Number of floors |
| `unitsres` | Number of residential units |
| `unitstotal` | Total number of units |
| `lotfront` | Lot frontage in feet |
| `lotdepth` | Lot depth in feet |
| `bldgfront` | Building frontage in feet |
| `bldgdepth` | Building depth in feet |
| `yearbuilt` | Year building was constructed |
| `yearalter1` | Year of first alteration |
| `yearalter2` | Year of second alteration |
| `assessland` | Assessed land value |
| `assesstot` | Total assessed value |
| `exempttot` | Tax exempt value |
| `latitude` | Geographic latitude |
| `longitude` | Geographic longitude |
| `cd` | Community District |
| `ct2010` | Census tract (2010) |
| `cb2010` | Census block (2010) |
| `council` | City council district |
| `schooldist` | School district |
| `firecomp` | Fire company |
| `policeprct` | Police precinct |
| `healtharea` | Health area |
| `landmark` | Landmark designation status |
| `histdist` | Historic district designation |
| `zonedist1` | Primary zoning district |
| `zonedist2` | Secondary zoning district |
| `spdist1` | Special purpose district |
| `builtfar` | Built floor area ratio |
| `residfar` | Residential FAR |
| `commfar` | Commercial FAR |

**Data Quality Notes:**
- Comprehensive property characteristics database
- Better coverage of structural/zoning features
- Some fields have significant null values (zoning overlays, landmarks)

---

## 3. Census Bureau ACS5 (2022)

**Source:** US Census Bureau American Community Survey 5-Year Data  
**Description:** Socioeconomic and demographic data at the census tract level for Manhattan (New York County).

**Key Variables:**
- Population and housing characteristics
- Income and poverty statistics
- Education attainment
- Employment data
- Housing unit characteristics
- Demographic breakdown by race/ethnicity

**Coverage:** 2022 ACS5 data aligned with 2021-2025 sales window

**Join Method:** Census tract linkage (`census_tract_2020` field from sales data)

---

## 4. MTA Subway Stations (39hk-dx4f)

**Source:** MTA/data.ny.gov  
**Description:** NYC subway station locations with latitude/longitude and served routes.

**Fields:**
- Station name and complex ID
- Latitude/longitude coordinates
- Route designations (1-A, 2-5, etc.)

**Join Method:** Spatial join using KDTree for nearest subway station to each property

---

## Data Integration

### Join Strategy:
1. **Primary Join:** NYC Sales ↔ MapPLUTO on `bbl` (BBL identifier)
2. **Secondary Join:** Sales ↔ Census data on `census_tract_2020`
3. **Spatial Join:** Properties ↔ MTA Stations using KDTree on coordinates

### Filtering Criteria:
- **Geography:** Manhattan only (borough = 1/"MN")
- **Date Range:** 2021-01-01 onwards
- **Property Type:** Residential only (9 specific building class categories)
- **Sale Price:** ≥ $100,000 (arms-length transactions)
- **Deduplication:** Removed duplicate sales on same property

### Final Dataset Characteristics:
- **Records:** 86,147 residential sales with complete geographic data
- **Time Period:** 4.5+ years of transaction history
- **Geographic Coverage:** All Manhattan neighborhoods
- **Feature Count:** 100+ engineered features from merged sources

---

## Data Quality & Missing Values

### Sales Data (w2pb-icbu):
| Field | Non-Null % | Notes |
|---|---|---|
| sale_price | 100% | Target variable |
| latitude/longitude | 94.6% | Sufficient for spatial analysis |
| year_built | ~93% | Important for age-based features |
| gross_square_feet | Partial | Many commercial properties lack this |
| apartment_number | 23.8% | Only relevant for condos/co-ops |

### PLUTO Data:
- Generally comprehensive for structural features
- Some null values in optional zoning overlays
- Landmark/historic district designations are sparse but valuable

---

## Notes for Analysis

1. **Sales Price Outliers:** Pre-filtered to remove non-market transactions (<$100k), but extreme luxury transactions (>$20M) may require separate handling
2. **Time Effects:** 4.5 years of data includes COVID period (2020-2021) and post-pandemic recovery
3. **Neighborhood Variation:** Wide range of property types and prices across Manhattan's diverse neighborhoods
4. **Feature Engineering:** Derived features include distance to subway, age of building, price per sq ft, etc.
5. **Collinearity:** Multiple size measures (land_sqft, gross_sqft, units) are correlated and require careful selection

[Back to Table of Contents](#table-of-contents)
