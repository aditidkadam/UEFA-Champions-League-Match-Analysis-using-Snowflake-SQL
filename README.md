
# UEFA Champions League Match Analysis using Snowflake SQL

## Project Objective

This project analyzes UEFA Champions League match data stored in **Snowflake** to answer three clearly defined analytical questions for a sports media use case:

1. Identify the highest home-team scoring performances in the 2020 season.
2. Determine which team controlled possession most frequently in the 2021 season.
3. Detect matches where a team won more physical duels but still lost the game in the 2022 season.

Each query was written to extract measurable facts directly from match-level data rather than producing descriptive summaries.

---

## Environment & Tools

| Component | Usage |
|---|---|
| Snowflake | Cloud data warehouse used to store and query match tables |
| SQL | Data analysis and conditional logic implementation |
| DataCamp DataLab | Execution environment connected to Snowflake-style schema |
| GitHub | Documentation and project versioning |

Snowflake stores all database objects in uppercase by default, therefore all queries referenced tables and columns using uppercase naming conventions.

---

## Dataset Scope

| Attribute | Value |
|---|---|
| Schema | `SOCCER` |
| Tables Used | `TBL_UEFA_2020`, `TBL_UEFA_2021`, `TBL_UEFA_2022` |
| Seasons Covered | 2020, 2021, 2022 |
| Competition | UEFA Champions League |
| Teams per Season | 32 clubs |
| Observation Level | One row represents one completed match |
| Schema Consistency | All three tables share identical columns and data types |

Because schemas are identical, queries could be written independently per season without transformation or column remapping.

---

## Dataset Structure Understanding

Before querying, column relationships were reviewed to determine how performance comparisons must be constructed.

Each performance metric exists twice:

- Home team metric
- Away team metric

Example:

| Metric | Home Column | Away Column |
|---|---|---|
| Goals | TEAM_HOME_SCORE | TEAM_AWAY_SCORE |
| Possession | POSSESSION_HOME | POSSESSION_AWAY |
| Duels Won | DUELS_WON_HOME | DUELS_WON_AWAY |

This structure required conditional evaluation to determine which team performed better within each match row.

---

## Analytical Approach

### Step 1 — Question Definition

Queries were written only after defining measurable analytical outcomes:

- Ranking extreme scoring performances.
- Measuring repeated tactical dominance.
- Identifying contradictions between physical performance and match results.

### Step 2 — Row-Level Comparison Logic

Because Snowflake tables store both teams in the same row, `CASE WHEN` logic was used to dynamically select the better-performing team.

Example logic used:

```sql
CASE
WHEN POSSESSION_HOME > POSSESSION_AWAY THEN TEAM_NAME_HOME
WHEN POSSESSION_AWAY > POSSESSION_HOME THEN TEAM_NAME_AWAY
END
````

This approach ensured that results were determined by performance values rather than match location.

---

## Key Performance Indicators (KPIs)

| KPI                  | Exact Measurement                                                |
| -------------------- | ---------------------------------------------------------------- |
| Highest Home Goals   | Maximum TEAM_HOME_SCORE values in 2020 dataset                   |
| Possession Dominance | Count of matches where one team had higher possession percentage |
| Duel Dominance Loss  | Matches where DUELS_WON was higher but goals scored were lower   |
| Stage Context        | Tournament stage recorded in STAGE column                        |

Each KPI corresponds directly to a SQL condition used in filtering or ranking.

---

## Analysis Results

### 1. Highest Home Scoring Performances — Season 2020

Query:

* Ordered matches by `TEAM_HOME_SCORE` in descending order.
* Limited results to the top **3 rows** using `LIMIT 3`.

**Returned Teams**

| Rank | Team              |
| ---- | ----------------- |
| 1    | PSG               |
| 2    | Manchester United |
| 3    | Barcelona         |

**Fact:**
The dataset produced exactly **3 highest-scoring home performances** after sorting all matches in `TBL_UEFA_2020`.

**Interpretation:**
These matches represent the maximum offensive output achieved by home teams during the 2020 Champions League season.

---

### 2. Majority Possession Leader — Season 2021

Query logic:

* Compared `POSSESSION_HOME` and `POSSESSION_AWAY` for every match.
* Assigned the team with higher possession using conditional logic.
* Counted occurrences per team.
* Ranked counts in descending order.
* Returned the top result using `LIMIT 1`.

**Result**

| Team      | Result                                             |
| --------- | -------------------------------------------------- |
| Liverpool | Highest number of matches with majority possession |

**Fact:**
The aggregation returned **one team**, Liverpool, as the possession leader across all matches analyzed in `TBL_UEFA_2021`.

**Interpretation:**
Liverpool achieved majority ball possession more frequently than any other club recorded in that season’s dataset.

---

### 3. Duel Dominance but Match Loss — Season 2022

Query conditions:

* `DUELS_WON_HOME > DUELS_WON_AWAY` AND home team scored fewer goals
  OR
* `DUELS_WON_AWAY > DUELS_WON_HOME` AND away team scored fewer goals.

Rows meeting this condition were filtered using `WHERE TEAM_LOST IS NOT NULL`.

**Total Matches Identified:** **16 matches**

**Example Teams Returned**

Chelsea, Juventus, Barcelona, Atletico Madrid, Tottenham Hotspur, RB Leipzig, Rangers, Porto, Ajax, Marseille.

**Fact:**
Exactly **16 match records** satisfied the condition where duel superiority did not translate into victory.

**Interpretation:**
Physical contest success alone was insufficient to determine match outcomes within these games.

---

## Business Insights Derived from SQL Results

* The 2020 dataset confirms that PSG, Manchester United, and Barcelona produced the three highest home scoring outputs, quantifying offensive peaks rather than average performance.
* Liverpool’s repeated possession dominance in 2021 indicates consistent tactical control measurable across multiple matches.
* Sixteen matches in 2022 demonstrate a measurable disconnect between physical engagement metrics and final results, highlighting scoring efficiency as a stronger outcome driver than duel wins.

These insights provide measurable narratives suitable for match analysis and sports media reporting.

---

## Skills Demonstrated

* Writing analytical SQL queries in Snowflake
* Designing KPIs directly from dataset structure
* Implementing conditional comparisons across paired columns
* Ranking and filtering using ORDER BY and LIMIT
* Translating query output into factual analytical conclusions

---

## Project Outcome

This project demonstrates the ability to analyze structured sports datasets inside Snowflake, construct performance-based SQL logic, and produce fact-based insights grounded entirely in query results rather than assumptions.

---

## Author

**Aditi Kadam**
Data Analyst | SQL | Snowflake | Data Analytics



