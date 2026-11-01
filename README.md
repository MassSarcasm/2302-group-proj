and where we are and what we have done so far
# North Carolina Housing Affordability and Recommendation Project

## Project Overview

Finding somewhere to live involves more than comparing housing prices. An affordable area may be far from work, grocery stores, recreation, or public transportation. This project brings those factors together to help people compare areas within North Carolina based on their budget and preferences.

We are building a geographic dataset that combines housing costs, income, neighborhood characteristics, and nearby amenities. This will support exploratory analysis and the development of a housing recommendation approach.

## Central Question

**Where in North Carolina can households with different budgets and preferences find affordable housing with access to the amenities and services they value?**

Supporting questions include:

- How do housing costs and local incomes vary across North Carolina?
- How do housing costs relate to walkability, transit, and nearby amenities?
- Which areas offer relatively affordable housing alongside access to everyday needs and recreation?
- How could recommendations change depending on a person's budget and priorities?

## Data Sources

| Source | Information used |
|---|---|
| American Community Survey (ACS), 2024 five-year estimates | Income, rent, home values, housing costs, population, and housing occupancy |
| Zillow | ZIP-level home values and a 12-month home-value measure |
| EPA Smart Location Database | Density, walkability, transit availability, job accessibility, and other built-environment characteristics |
| OpenStreetMap | Food and nightlife, shopping and daily needs, sports and fitness, entertainment and culture, and outdoor recreation |
| USGS PAD-US | Protected lands and areas classified as open access |
| NC coastal-access data | Beach and shoreline access locations |
| NCDOT rail facilities | Passenger-station records and Amtrak bus locations |
| U.S. Census Bureau geographic boundaries | Census tracts and ZIP Code Tabulation Areas (ZCTAs) |

Airport candidates were also selected from OpenStreetMap using a commercial passenger airport shortlist.

## Work Completed

### Data collection and cleaning

We collected and prepared the housing, income, built-environment, and amenity datasets.

The amenity preparation included:

- Grouping selected OSM features into five categories.
- Removing 103 likely duplicate point representations where a matching polygon existed.
- Retaining 60,273 cleaned OSM amenity records.
- Preparing 2,830 open-access PAD-US land records.
- Collecting 911 coastal-access records and separating Beach, Shoreline, and Unknown classifications.
- Separating 16 passenger-rail station records from 11 Amtrak bus locations.
- Identifying nine commercial passenger airport candidates.

### Geographic integration

The datasets use different geographic units, so they cannot all be joined directly by an ID.

We intersected ACS census tract boundaries with ZCTA boundaries to create **5,466 shared housing areas**. Each area retains its original tract and ZCTA identifiers.

These areas represent 2,661 ACS tracts. The 11 ACS tracts absent from the intersection all have zero reported population and zero occupied housing units.

EPA uses older, 2010 tract geography. We assigned each housing area the EPA tract covering the largest portion of it, keeping the overlap percentage to document the assignment.

| Source | Housing areas with a matching record |
|---|---:|
| ACS | 5,466 |
| Zillow | 5,279 |
| EPA | 5,442 |

For 60 areas, the selected EPA tract covers less than 90% of the area. These assignments are flagged.

### Initial accessibility features

We calculated mapped amenity counts within five kilometers of a representative point inside each housing area.

The counts vary substantially. For example, nearby food and nightlife records range from 0 to 742, with a median of 11. This is an initial descriptive result; we have not yet tested how these differences relate to housing costs.

## Repository Structure

| Notebook | Purpose |
|---|---|
| `acs_data_cleaning.ipynb` | Prepare ACS housing, income, and population data |
| `zillow_epa_data_cleaning.ipynb` | Prepare Zillow and EPA data |
| `amenities_and_accessibility.ipynb` | Collect, clean, and organize amenity and access datasets |
| `data_integration.ipynb` | Create shared housing areas, connect source attributes, and calculate initial accessibility features |

Large data files are excluded from Git and shared separately. Place the project data in the local `data/` folder using the paths expected by the notebooks.

## Current Status

Data collection, initial cleaning, geographic integration, and the first amenity-count features are complete. We have also prepared maps and progress visuals.

Exploratory analysis, modeling, and recommendation development are still ahead.

## Next Steps

- Review missing values and document the variables and source years.
- Explore housing costs, income, walkability, and amenity access.
- Complete and review additional accessibility features.
- Select modeling outcomes and build baseline models.
- Develop and evaluate a recommendation approach based on budget and preferences.

## Important Limitations

- ZCTAs approximate ZIP service areas and are not exact USPS ZIP boundaries.
- ACS, Zillow, and EPA values describe their original source areas. Creating smaller polygons does not create new measurements.
- Multiple housing areas share the same source statistics. Model evaluation must account for these shared geographies.
- Amenity counts represent mapped records, not necessarily unique venues or confirmed public access.
- Five-kilometer searches use straight-line distance from one reference point, not driving distance or access from every home.
- Airport coverage is incomplete because Piedmont Triad has not yet been added.
- The datasets describe different time periods, so relationships must be interpreted with those differences in mind.