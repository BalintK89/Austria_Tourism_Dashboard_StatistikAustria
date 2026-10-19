# 🇦🇹 Austria Tourism & Accommodation Analytics Dashboard (Power BI)

An interactive Power BI dashboard analyzing tourist arrivals (*Ankünfte*) and overnight stays (*Übernachtungen*) across Austrian federal states (*Bundesländer*), utilizing official Open Data provided by **Statistik Austria** via **Data.gv.at**.

---

## 📸 Dashboard Overview

<!--  IMAGES HERE  -->
<img width="2527" height="1215" alt="2" src="https://github.com/user-attachments/assets/fe94f5ec-4cef-4195-a0fe-28041b32147d" />
<img width="2527" height="1215" alt="3" src="https://github.com/user-attachments/assets/a5a58963-4033-45fb-ae50-0967d3496cfc" />




---

## 🎯 Project Overview & Business Value

Tourism is a key driver of Austria's economy. This project delivers an executive-level view of regional tourism dynamics, seasonality trends, and top international markets. Designed following professional BI standards, it translates raw public data into actionable insights for decision-makers.

### Key Capabilities:
* **Star Schema Data Modeling:** Built a robust data model with 1 Fact table (`Fact_Tourismus`) linked via 1-to-Many relationships to 3 Dimension tables (`Dim_Bundesland`, `Dim_Herkunftsland`, `Dim_Saison`).
* **Advanced DAX & Time Intelligence:** Formulated measures for Year-over-Year (YoY) metrics, percentage growth, and dynamic ranking (`TOPN`).
* **Interactive UI/UX:** Built-in cross-filtering between state map visual, seasonality trendlines, top market bar charts, and detailed matrix tables.

---

## 🛠️ Data Pipeline & Architecture

1. **Data Ingestion (ETL):** Ingested raw monthly OGD data files from *Statistik Austria*.
2. **Power Query (Data Transformation):** 
   * Constructed custom date keys from string values (`197312` -> `1/12/1973`) using M code.
   * Filtered out aggregate totals and redundant country/region codes.
   * Configured strict data typing and relationship keys.
3. **Data Modeling (Star Schema):** 
   * `Fact_Tourismus` (Measures: Arrivals, Nights)
   * `Dim_Bundesland` (9 Austrian States)
   * `Dim_Herkunftsland` (Origin Countries)
   * `Dim_Saison` (Calendar/Date Dimension)

---

## 📊 Core DAX Measures

```dax
// Total Overnight Stays
Gesamte Übernachtungen = SUM(Fact_Tourismus[F-UEB])

// Prior Year Overnight Stays (YoY)
Übernachtungen Vorjahr = 
CALCULATE(
    [Gesamte Übernachtungen],
    SAMEPERIODLASTYEAR(Dim_Saison[Date])
)

// Year-over-Year Growth %
Übernachtungen YoY % = 
DIVIDE(
    [Gesamte Übernachtungen] - [Übernachtungen Vorjahr],
    [Übernachtungen Vorjahr],
    0
)

// Dynamic Top Performing Bundesland
Top Bundesland = 
TOPN(
    1,
    VALUES(Dim_Bundesland[name]),
    [Gesamte Übernachtungen],
    DESC
)
