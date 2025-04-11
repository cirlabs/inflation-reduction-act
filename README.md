# Inflation Reduction Act

## Methodology

We performed a [spatial join](https://en.wikipedia.org/wiki/Spatial_join) on the U.S. Census Bureau district polygons and the coordinates published in the energy.gov dataset.

Where those did not produce a match, we instead used the coordinates for the city as designated by the U.S. Census Bureau (referred to as Place).

If this was not possible:

- and the project investment exceeded $1 billion, we determined the project location from published announcements.
- and the project did not exceed $1 billion, we excluded the data from the analysis.

## Sources

### Investments

<details open>
<summary>
Data associated with the Inflation Reduction Act regarding investments in clean energy announced by private and governmental organizations since January 21, 2021.
</summary>

[`IRA_data.csv`](01_inputs/IRA_data.csv)

**Source:** The U.S. Department of Energy website, specifically [energy.gov/invest](https://www.energy.gov/invest#:~:text=Download%20Data)

> ​​​​​​​The data was gathered from public announcements that were made on or after January 21, 2021: directly from the company, from governmental sources, from news outlets, or from other reputable sources. Dollar investment amounts and employment totals are only included if they are reported in the public announcements; many facilities therefore lack investment and employment estimates. Many facilities are conditional on financing, funding, site control, and other facts, and inclusion [...] does not imply that these facilities will come online as reported.
-- <cite>https://www.energy.gov/invest/faq</cite>
</details>

### Geographies



<details open>
<summary>
119th Congressional District shapefiles for all states and territories
Not uploaded because they're large files
</summary>

**Source:** U.S. Census Bureau TIGER/Line Shapefiles 2024

  - 119th Congressional Session districts
    - [FTP directory](https://www2.census.gov/geo/tiger/TIGER2024/CD/)
    - Documentation [PDF](https://www2.census.gov/geo/pdfs/maps-data/data/tiger/tgrshp2024/TGRSHP2024_TechDoc.pdf)
  - cities (referred to as Place)
    - [FTP directory](https://www2.census.gov/geo/tiger/TIGER2024/PLACE/)

</details>


### Legislators

<details open>
<summary>Data on the districts composing the U.S. and territories
</summary>

[`119th_congress_members.csv`](01_inputs/119th_congress_members.csv)

**Source:** [LegiScan API](https://legiscan.com/legiscan).

#### Example entry

| field             | value           |
|-------------------|-----------------|
| people_id         | 9434            |
| person_hash       | tz8vuv7x        |
| party_id          | 3               |
| state_id          | 52              |
| party             | I               |
| role_id           | 2               |
| role              | Sen             |
| name              | Bernard Sanders |
| first_name        | Bernard         |
| middle_name       |                 |
| last_name         | Sanders         |
| suffix            |                 |
| nickname          |                 |
| district          | SD-VT           |
| ftm_eid           | 9960190         |
| votesmart_id      | 27110           |
| opensecrets_id    | N00000528       |
| knowwho_pid       | 159116          |
| ballotpedia       | Bernie_Sanders  |
| bioguide_id       | S000033         |
| committee_sponsor | 0               |
| committee_id      | 0               |
| state_federal     | 0               |

</details>

## Output

### [`processed_data.csv`](04_outputs/processed_data.csv)

Fields marked like <mark>this</mark> were added by Reveal; others reflect data in the original dataset.

| `Field`              | Description                                                                                                 | Example                  |
|--------------------|-------------------------------------------------------------------------------------------------------------|--------------------------|
| <mark>`row_id`</mark>             | Arbitrary identifier based on index in raw data.                                                            | 1596                     |
| <mark>`GEOIDFQ`</mark>            | Fully Qualified GEOID                                                                                       | 5001900US2613            |
| <mark>`geocoded_state`</mark>     | State to which geocoded coordinates correspond                                                              | Michigan                 |
| <mark>`geocoded_latitude`</mark>  | Output of geocoding via U.S. Census Bureau                                                                  | 42.28                    |
| <mark>`geocoded_longitude`</mark> | Output of geocoding via U.S. Census Bureau                                                                                                         | -83.4                    |
| <mark>`join_type`</mark>          | 123                                                                                                         | geometry and state name³ |
| `original_latitude`  | Latitude submitted in original data                                                                         | 42.28                    |
| `original_longitude` | Longitude submitted in original data                                                                                               | -83.4                    |
| `project`            | Name of project                                                                                             | Cabot                    |
| `city`               | City in which project is to be located                                                                      | Van Buren Township       |
| `state`              | State in which project is to be located                                                                                                   | Michigan                 |
| `priinvest`          | Investment amount announced by a private entity                                                             | 181000000                |
| `pubinvest`          | Investment amount announced by a federal entity                                                             | 0                        |
| `totinvest`          | Total amount announced                                                                                      | 181000000                |
| `jobs`               | Total jobs announced                                                                                        | 85                       |
| `tech`               | The technology to which investments will apply                                                              | Batteries                |
| `company_name`       | The organization that made the announcement                                                                 | Cabot                    |
| `category`           | Whether the announcement was related to manufacturing.<br>Possible values:`All`, `All - Manufacturing` | All - Manufacturing      |
| `public`             | Whether announced by a public entity.<br>Possible values: `Yes`, `No`                                                           | No                       |
| `private`            | Whether announced by a private entity.<br>Possible values: `Yes`, `No`                                                         | Yes                      |
| <mark>`Representative`</mark>     | Name of 119th Congressional District representative                                                         | Undetermined             |
| <mark>`Party`</mark>              | Party of 119th Congressional District representative.<br>Possible values: `R`, `D`                                                                                                   | Democrat                 |
| <mark>`District`</mark>           | Succinct label for district comprising the state ANSI code and the district’s numerical ID                  |                          |
| <mark>`Census Region`</mark>      | One of nine regions to which states and DC are assigned.                                                    | Midwest                  |