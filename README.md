# -Maji-Ndogo-Water-Services-Dashboard--Power-BI-Project-Part-2

# 💧 Maji Ndogo Water Services Dashboard — Power BI Project (Part 2: Visualising)

An end-to-end Power BI project analyzing water access, water quality, queue dynamics, infrastructure costs, and water-source-related crime across the fictional region of **Maji Ndogo**. The project transforms raw, messy survey data into a clean relational model and a set of interactive dashboards that surface actionable insights for improving water service delivery.

---

## 📋 Overview

The **MD Water Services** dataset contains operational and field survey data collected to assess water access, quality, infrastructure, and service delivery across different locations in Maji Ndogo. It combines information on water sources, water quality, customer visits, queue times, pollution levels, infrastructure costs, project progress, and water-related incidents.

The dataset is organized into several related tables:

| Table | Description |
|---|---|
| `visits` | Records of field visits and service observations |
| `location` | Geographic information for surveyed areas |
| `water_source` | Details of available water sources |
| `well_pollution` | Water quality and contamination records |
| `queue_composition` | Information on waiting times and user queues |
| `project_progress` | Status of water improvement projects |
| `infrastructure_cost` | Costs associated with water infrastructure |
| `water_source_related_crime` | Crime incidents linked to water access points |

This project uses the dataset for **data cleaning, transformation, modeling, and visualization in Power BI**, with the goal of identifying challenges in water service delivery and supporting data-driven decision-making across Maji Ndogo's five provinces: **Akatsi, Amanzi, Hawassa, Kilimani, and Sokoto**.

---

## 📊 Dashboard Analytics

The Power BI report is split across four interactive pages: **National Overview**, **Provincial Water Sources**, **Queue Analysis**, and **Pollution & Crime**.

### 🇰🇪 National Overview
- Of the people served, **59.87% live in rural areas** (23.74K) versus **40.13% in urban areas** (15.91K), highlighting the rural skew of water access challenges.
- Across water source types, **wells serve the most people (~17K)**, followed by `tap_in_home`, `tap_in_home_broken`, `shared_tap`, and lastly `river` sources, which serve the fewest people.
- At the town level, **Harare** has the highest count of `tap_in_home` sources, while **Zuri** leads in `shared_tap` records, and **Amina** has the most `tap_in_home_broken` records — indicating that infrastructure quality varies significantly even among towns with strong tap access.

### 🗺️ Provincial Water Sources
- When filtering for **rural areas only** across **Amanzi, Akatsi, and Hawassa**, there are a combined **6,331 wells** — a number that changes significantly depending on whether urban areas or other provinces are included, underscoring the importance of precise map/filter slicing in the report.
- **Dahabu** stands out as the town with the highest number of `tap_in_home` sources.

### ⏱️ Queue Analysis
- **Saturday** has the highest total queue time of any day (246 min total), making it the most congested day overall, while **Sunday has the lowest queue times**.
- Among **weekdays**, **Monday** consistently has the longest queues, while **Wednesday** has the shortest.
- Gender breakdown of queues: **69.06% female, 23.08% male, 7.86% child** on average — though this fluctuates by day. For example, the **average % of men in queues on a Saturday is 40%**, compared to an overall average of 24% male / 66% female across all days.
- **Optimal queue timing:** For a citizen restricted to Tuesday or Thursday, the **shortest queue time is on Thursday at 15:00**.
- At that optimal time (Thursday 15:00), only **~3% of the queue is made up of children** — much lower than the ~10–11% average seen on Sundays or across all days/times.

### 🦠 Pollution & Crime
- Well pollution status breaks down as: **40.8% Contaminated: Chemical, 30.92% Contaminated: Biological, 28.28% Clean**.
- **Akatsi** has the most chemically contaminated wells (2,056), making it the most affected province — well ahead of Kilimani, Sokoto, and Amanzi (Amanzi has the fewest).
- Crime victims are predominantly **female (64.39%)**, followed by male (25.42%) and child (10.2%) victims.
- **Akatsi** has the **lowest** number of crimes where the victim was male, compared to Kilimani, Amanzi, and Sokoto.
- Across a full week, the **lowest number of crimes against children occurs at 12:00 on Sunday**.
- **Friday** shows the **largest disparity between female and male victims** in sexual assault cases — larger than Saturday, Sunday, or Monday.

---

## 🧹 Data Cleaning

Before modeling, the raw tables were cleaned in Power Query to ensure analytical accuracy:

- **De-duplicated visits:** The `visits` table was filtered to handle instances where a single water source had been visited more than once, keeping the data point-in-time accurate and preventing inflated visit/queue counts.
- **Removed NaNs in infrastructure data:** Null/blank entries were removed from the `infrastructure_cost` table so that cost calculations and aggregations would not be skewed by missing values.
- Standardized column data types (dates, numeric fields, categorical text) across all eight tables.
- Trimmed whitespace and corrected inconsistent text casing in categorical fields such as `province_name`, `town_name`, and `type_of_water_source`.
- Validated referential integrity between tables (e.g., ensuring every `source_id` in `visits` exists in `water_source`).

---

## 🔧 Data Transformation (Extracted/New Columns)

Several calculated and extracted columns were created to enable deeper analysis:

- **`type_of_water_source`** — parsed/standardized from the raw source identifier to classify each source as `well`, `tap_in_home`, `tap_in_home_broken`, `shared_tap`, or `river`.
- **Day of week & hour-of-day fields** — extracted from visit/queue timestamps to power the queue-time-by-day and queue-time-by-hour visuals.
- **Queue duration calculations** — derived from start/end timestamps in `queue_composition` to compute total and average time spent in queue.
- **Gender/age composition percentages** — calculated from raw counts in `queue_composition` to express child/female/male shares as percentages.
- **Pollution result categories** — `Contaminated: Chemical`, `Contaminated: Biological`, and `Clean` flags derived from raw lab/test results in `well_pollution`.
- **Location-type flag** — `Rural` vs `Urban` classification extracted from the `location` table to enable the location-type breakdown visuals.
- **Crime victim gender grouping** — standardized `victim_gender` field (`F`, `M`, `C`) consolidated from raw crime records for consistent reporting.

---

## 🔗 Model View

The relational model establishes one-to-many relationships between the fact and dimension tables to allow filtering and slicing across the entire report:

- `location` (1) → (∞) `water_source` — each location can have multiple water sources.
- `water_source` (1) → (∞) `visits` — each water source can be visited multiple times.
- `water_source` (1) → (∞) `well_pollution` — pollution test records linked to individual wells.
- `visits` (1) → (∞) `queue_composition` — each visit can include detailed queue composition data.
- `location` (1) → (∞) `infrastructure_cost` — infrastructure spend tracked per location.
- `location` (1) → (∞) `project_progress` — project status tracked per location.
- `location` (1) → (∞) `water_source_related_crime` — crime incidents tied to specific locations/access points.

Relationships were rectified to remove circular and many-to-many ambiguities, ensuring every visual filters correctly across pages via slicers (province, town, location type, day of week).

📷 *See `model_view.png` in the resources for the full relationship diagram.*

---

## 📈 Key Visuals

| Visual Type | Example Use in This Report |
|---|---|
| **Pie / Donut Chart** | Location type split (Rural vs Urban); Queue composition (child/female/male); Well pollution status; Victim gender distribution |
| **Map (Choropleth/Shape)** | Province name (water sources); Gender distribution percentage; Pollution status by province; Crime distribution by province |
| **Simple Bar/Column Chart** | Number of people served by water source type; Total time in queue per province; Queue time across days of the week |
| **Comparative/Stacked Bar Chart** | Number of people served per water source in various towns (stacked by source type); Provincial distribution of crime victims per gender; Crime type per gender |
| **Line Chart** | Average queue time for shared taps by hour of the day (across days); Count of crime type per hour of day; Count of crime type per day of week |
| **Scatter Plot** | Used for exploring relationships between infrastructure cost, queue time, and population served per source (cost-efficiency analysis) |
| **Treemap** | Number of people served per water source type (national overview) |

---

## 🛠️ Tools & Technologies

- **Power BI Desktop** — data modeling, DAX measures, and dashboard design
- **Power Query (M language)** — data cleaning and transformation
- **DAX** — calculated columns and measures (percentages, queue durations, aggregations)
- **Excel (.xlsx)** — original source data format (`Md_water_services.xlsx`)
- **GitHub** — version control and project documentation

---

## ▶️ How to Use

1. **Clone or download** this repository.
   ```bash
   git clone https://github.com/<your-username>/maji-ndogo-water-services-dashboard.git
   ```
2. Open **`Md_water_services.xlsx`** to review the raw source tables (optional).
3. Open the **`.pbix`** Power BI file in **Power BI Desktop**.
4. In the **Power Query Editor**, review the applied steps under each table to see the cleaning and transformation logic.
5. Go to **Model View** to inspect the relationships between tables.
6. Explore the report pages:
   - **National Overview**
   - **Provincial Water Sources**
   - **Queue Analysis**
   - **Pollution & Crime**
7. Use the **map and slicers** (province, town, location type, day of week) to filter visuals interactively and answer your own questions about the data.

---

## 🌳 Project Structure

```
maji-ndogo-water-services-dashboard/
│
├── data/
│   └── Md_water_services.xlsx          # Raw source dataset (all 8 tables)
│
├── images/
│   ├── water_sources.png               # Provincial water source visuals
│   ├── queue_information.png           # Queue analysis visuals
│   ├── pollution_status.png            # Well pollution status visuals
│   ├── water_source_related_crimes.png # Crime analysis visuals
│   └── model_view.png                  # Power BI data model / relationships
│
├── Maji_Ndogo_Water_Services.pbix      # Main Power BI report file
│
└── README.md                           # Project documentation (this file)
```

---

## ✅ Project Challenge — Sample Analysis Findings

This project was validated against a 10-question analytical challenge (scored on accuracy and time-to-insight), confirming the dashboard correctly supports granular, filter-driven analysis such as:

- **Dahabu** is the town with the highest number of `tap_in_home` sources.
- There are **6,331 wells** in the rural areas of Amanzi, Akatsi, and Hawassa combined.
- The average percentage of men in queues on a Saturday is **40%**.
- **Monday** consistently has the longest queues among weekdays.
- The shortest queue time, when restricted to Tuesday or Thursday, occurs on **Thursday at 15:00**.
- Children make up **3%** of the queue at the shortest overall queue time.
- **Akatsi** is the province with the most chemically contaminated wells.
- **Akatsi** is the province with the fewest crimes involving male victims.
- The lowest number of crimes against children across the week is reported at **12:00 (Sunday)**.
- **Friday** shows the largest disparity between female and male victims in sexual assault cases.

These results demonstrate that the model and visuals are correctly built to support granular, filter-driven analysis.

---

## 👤 Author / Developer

**[Your Name]**
Data Analyst | Power BI Developer

- GitHub: [your-github-profile](https://github.com/your-username)
- LinkedIn: [your-linkedin-profile](https://linkedin.com/in/your-username)

If you found this project useful or interesting, please consider **starring ⭐ the repo** — it helps others discover it and supports more projects like this one!

---

## 📚 List of Resources

| Resource | Description |
|---|---|
| `Md_water_services.xlsx` | Original raw dataset containing all 8 source tables |
| `water_sources.png` | Dashboard view of water source distribution across towns/provinces |
| `queue_information.png` | Dashboard view of queue time and composition analysis |
| `pollution_status.png` | Dashboard view of well pollution status by province |
| `water_source_related_crimes.png` | Dashboard view of crime statistics linked to water access |
| `model_view.png` | Power BI data model showing table relationships |

---

⭐ *If this project helped you understand Power BI data modeling and dashboard storytelling, give it a star!*
