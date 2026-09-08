# Indonesia Electric Vehicle Analysis

An interactive **Power BI decision-support dashboard** for analyzing electric vehicle (EV) market positioning, vehicle performance, value-for-money, and budget suitability.

> **Portfolio case study:** from data preparation and validation to DAX-driven analysis and interactive Power BI storytelling.

---

## Project Overview

The dashboard brings multiple EV specifications into one analytical view so users can compare vehicles across:

- Price
- Driving Range
- Battery Capacity
- Motor Power
- Charging Time
- Seating Capacity
- Value-for-money
- Budget suitability

The project is designed to answer a practical business question:

> **Which EV models offer the best balance between price, range, and performance, and which options fit a given budget?**

---

## Business Problem

EV specifications can be difficult to compare when price, range, battery, performance, and charging information are viewed separately.

This dashboard addresses three key challenges:

1. **Fragmented specifications**  
   Multiple vehicle attributes need to be evaluated together rather than individually.

2. **No integrated value comparison**  
   A consistent metric is needed to compare driving range relative to vehicle price.

3. **No interactive budget-fit analysis**  
   Users need to identify which EV models are eligible under a selected price ceiling.

---

## Objectives

The dashboard is designed to answer five analytical questions:

1. What does the EV market landscape look like across price segments?
2. Which EV models offer the strongest value-for-money?
3. How do price, battery capacity, motor power, and driving range relate?
4. Which EV models provide the longest driving range?
5. Which EV models fit a selected budget?

---

## Dashboard Structure

### Main Analytical Pages

| Page | Purpose |
|---|---|
| **Dashboard** | Executive overview of the EV catalog, price/performance relationships, and headline rankings |
| **Market Analysis** | Price-segment distribution, charging-time landscape, and market-level comparisons |
| **Vehicle Analysis** | Vehicle-level comparison of price, range, battery, power, charging time, and value |
| **Budget Simulation** | Interactive budget scenario analysis and eligible EV recommendations |
| **Business Insight** | Converts analytical findings into business implications and recommendations |

### Supporting Pages

| Page | Purpose |
|---|---|
| **Vehicle Detail** | Supporting vehicle-level detail / drill-through experience |
| **Tootlip Vehicle** | Tooltip page used to provide additional vehicle context |

---

# Dashboard Preview

The dashboard screenshots are available in the `screenshots/` folder.
---

## Key Dashboard Capabilities

### Executive Overview

The main dashboard combines:

- Total EV
- Average valid price
- Average driving range
- Average battery capacity
- Price vs. motor power
- Battery capacity vs. driving range
- Top 5 value-for-money EVs
- Top 5 longest-range EVs
- Interactive vehicle, seating, price, and charging-time filters
- Budget simulation

### Market Analysis

Provides a market-level view using:

- Price Segment distribution
- Motor Power vs. Driving Range
- Charging Time distribution
- Price-segment filtering
- Valid-price analysis

### Vehicle Analysis

Allows users to compare individual vehicles across:

- Price
- Driving Range
- Battery Capacity
- Motor Power
- Charging Time
- Value Score

The page also supports richer vehicle-level tooltips and comparison.

### Budget Simulation

Uses a **Power BI What-if Parameter** to evaluate vehicle eligibility under a selected budget.

The flow is:

```text
Selected Budget
      ↓
Validate Vehicle Price
      ↓
Filter Eligible EV Models
      ↓
EV Within Budget
      ↓
Avg Range Within Budget
      ↓
Best Value Within Budget
```

This turns the dashboard from a static report into an interactive decision-support tool.

### Business Insight

The final page translates the analysis into business implications and recommendations across:

- Portfolio / market positioning
- Performance
- Charging
- Budget opportunity

---

## Data & Data Quality

The dashboard contains **60 EV models**.

A key data-quality issue was identified during validation:

> **12 vehicle models have price values flagged as invalid.**

These records are **not treated as globally invalid vehicles**. Instead, the invalid price values are excluded from analyses that depend on price, such as:

- Price-based comparisons
- Value Score
- Price segmentation
- Budget eligibility

Performance-related information such as driving range and battery capacity can still be used where appropriate.

This approach prevents invalid price values from distorting downstream analysis while preserving useful non-price attributes.

---

## Data Validation Logic

The dashboard uses a validation layer to distinguish price values that are suitable for price-based analysis.

Conceptually:

```text
Raw Price
   ↓
Price Validation
   ↓
Valid / Invalid
   ↓
Valid Price
   ↓
Price-based Analysis
```

The validation layer is especially important because an invalidly low price can artificially inflate the value metric:

```text
Value Score = Driving Range / Valid Price
```

Without validation, a very small price can produce a misleadingly high score.

---

## Key Analytical Calculations

The project uses both **calculated columns** and **measures**, depending on the analytical requirement.

### Calculated Columns

#### Price Segment

Used to categorize vehicles into price tiers for market analysis.

Example conceptual segmentation:

```text
Entry    → < 300M
Mid      → 300–600M
Premium  → > 600M
```

Invalid price records are identified separately rather than being treated as normal price observations.

#### Value Scores

The current value metric is based on valid vehicle pricing:

```DAX
Value Scores =
DIVIDE(
    EV_Data[Range Avg (km)],
    EV_Data[Valid Price]
)
```

This is a **calculated column** because the score is evaluated at vehicle/row level.

### Measures

Important measures include:

- `Total EV`
- `Avg Price (Valid)`
- `Avg Range`
- `Avg Battery`
- `EV Within Budget`
- `Avg Range Within Budget`
- Budget-driven insight measures
- Business insight measures

Measures are used where results need to respond dynamically to filter context and user selections.

---

## Budget Analysis Logic

The Budget Simulation page uses the selected What-if budget to identify eligible vehicles.

Conceptually:

```text
Valid Price <= Selected Budget
```

Only vehicles satisfying the condition are treated as budget-eligible.

This supports dynamic outputs such as:

- Number of eligible EV models
- Average range within budget
- Best-value vehicle within budget
- Budget insight text

---

## Why Measures vs. Calculated Columns?

A core modeling decision in this project is choosing between calculated columns and measures based on analytical behavior.

### Calculated Column

Used when a value should exist at the vehicle/row level.

Example:

```text
Value Scores
Price Segment
```

### Measure

Used when a result should react dynamically to:

- Slicers
- Filters
- What-if parameters
- Page context

Example:

```text
EV Within Budget
Avg Range Within Budget
```

---

## Key Insights

The dashboard's Business Insight page highlights four major themes:

### 1. Portfolio Position

The validly priced EV catalog is weighted toward the higher price segment, with the Premium tier representing a large share of price-valid models.

**Business implication:**  
There may be an opportunity to investigate under-served lower-price segments.

### 2. Performance Pattern

Higher motor power tends to be associated with longer driving range.

**Business implication:**  
Power and range can be considered together when comparing performance-oriented vehicle positioning.

### 3. Charging Landscape

Charging time varies substantially between EV models.

**Business implication:**  
Charging time can act as a meaningful vehicle-selection and differentiation factor.

### 4. Budget Opportunity

The Budget Simulation page shows how the available vehicle set changes as the user's price ceiling changes.

**Business implication:**  
Budget-based scenario analysis can provide a more decision-oriented view than a static vehicle price list.

---

## Example Decision Flow

The dashboard is designed to support this journey:

```text
Market Understanding
        ↓
Vehicle Comparison
        ↓
Budget Simulation
        ↓
Business Insight
        ↓
Decision Support
```

---

## Technology Stack

- **Power BI Desktop**
- **DAX**
- **Power Query**
- Interactive slicers
- What-if Parameters
- Bookmarks & Buttons
- Page Navigation
- Tooltips
- Data validation logic

---

## Project Deliverables

Recommended repository structure:

```text
indonesia-ev-powerbi-dashboard/
│
├── README.md
│
├── dashboard/
│   └── EV Dashboard.pbix
│
├── report/
│   ├── EV Dashboard Report.pptx
│   └── EV Dashboard Report.pdf
│
└── screenshots/
    ├── dashboard-overview.png
    ├── market-analysis.png
    ├── vehicle-analysis.png
    ├── budget-simulation.png
    └── business-insight.png
```

---

## How to Use

### Option 1 — Interactive Power BI

Open:

```text
dashboard/EV Dashboard.pbix
```

with **Power BI Desktop**.

### Option 2 — Quick Review

Open the PDF/PPT report in:

```text
report/
```

This is recommended for recruiters or stakeholders who do not have Power BI Desktop.

### Option 3 — Portfolio Preview

Use the images in `screenshots/` for the project preview on GitHub or your personal portfolio website.

---

## Portfolio Highlights

This project demonstrates the ability to:

- Translate a business question into an analytical dashboard
- Prepare and validate messy data
- Distinguish valid vs. invalid price observations
- Build calculated columns and dynamic measures in DAX
- Create ranking and value-based analysis
- Build What-if budget simulations
- Design interactive navigation
- Turn analysis into business insights and recommendations
- Communicate analytical results through Power BI and presentation storytelling

---

## Data Quality & Limitations

The price-validation issue is intentionally documented rather than hidden.

The 12 flagged price records should be treated as **data-quality exceptions** until their source values are verified.

As a result:

- Price-dependent analysis should use validated price values.
- Non-price attributes can still be used when their quality is acceptable.
- Business conclusions based on pricing should be interpreted in the context of the validated subset.

This separation helps prevent data-quality problems from silently affecting business decisions.

---

## Final Takeaway

> **The dashboard transforms fragmented EV specifications into an interactive analytical tool for market comparison, vehicle evaluation, and budget-aware decision making.**

It combines descriptive analysis with interactive scenario modeling so users can move from:

**“What does the EV market look like?”**

to:

**“Which vehicle makes the most sense for this budget?”**

---

## Author

**Brian Naufal**  
Data Analyst / Business Intelligence

**Tools:** Power BI • DAX • Power Query

- Portfolio: https://briannaufal-portfolio.netlify.app/
- LinkedIn: add your LinkedIn URL
- Email: add your professional email
