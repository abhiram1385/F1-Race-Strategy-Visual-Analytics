# F1 Race Strategy Visual Analytics System

An interactive, web-based visual analytics platform for exploring Formula 1 race strategy, driver performance, and tire degradation patterns across seven F1 seasons (2018–2024).

> Course Project — CS-661: Big Data Visual Analytics, IIT Kanpur (Prof. Saumya Dutta) · Status: 🚧 Work in Progress

---

## Table of Contents

- [Project Overview](#project-overview)
- [Features](#features)
- [How It Works](#how-it-works)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [Data Pipeline](#data-pipeline)
- [Contributing](#contributing)
- [License](#license)

---

## Project Overview

Formula 1 generates over a million telemetry data points per car per race weekend, yet no publicly available tool lets fans, analysts, or researchers visually explore this data in a coherent, interactive way. This project builds a narrative-driven visual analytics system that takes users from season-level championship context down to lap-by-lap strategic detail — surfacing how races are actually won and lost through pace, tire strategy, and execution.

The system draws on two complementary data sources: the **Ergast Developer API** for 75 years of historical F1 results (1950–2024), and **FastF1** for high-resolution lap and telemetry data across the 2018–2024 seasons.

**Key Use Cases:**

- Analysts can trace how a championship battle unfolds round by round.
- Fans can replay any race lap-by-lap with tire strategy overlaid.
- Researchers can cluster tire stints to uncover strategic archetypes.
- Engineers can decompose lap times into pace, fuel, tire-age, and traffic components.

**Dataset at a Glance:**

| Metric | Value |
|---|---|
| Seasons covered (telemetry) | 2018–2024 (7 seasons) |
| Races | 140 |
| Driver stints | 6,500+ |
| Individual lap records | 450,000+ |
| Historical race weekends (Ergast) | 1,000+ |

---

## Features

- **Season Overview & Championship Progression:** Animated cumulative points trajectories, constructor performance heatmaps, and race outcome grids across a full season.
- **Race Deep Dive & Lap-by-Lap Dynamics:** Animated position charts with tire compound encoding, gap-to-leader area charts, and Gantt-style pit stop timelines.
- **Driver & Team Performance Comparison:** Radar charts across six performance dimensions, qualifying-vs-race scatter plots, and per-circuit heatmaps.
- **High-Dimensional Stint Clustering:** Parallel coordinates plots and PCA-based 2D projections revealing strategic archetypes (tire conservation, short-stint attacks, safety-car recovery stints).
- **Tire Degradation & Lap Time Decomposition:** Fitted degradation curves per compound with 95% confidence intervals, and toggleable overlays isolating fuel-burn, tire-age, and traffic effects.
- **Linked, Cross-View Filtering:** A shared global filter bar (season, race, driver) propagates selections across all views.

*(All features above are planned/in progress — see [Status](#status).)*

---

## How It Works

1. **Data Acquisition:** Lap, stint, and session data are bulk-downloaded via FastF1 and the Ergast API for all race weekends from 2018–2024.
2. **Preprocessing:** Non-representative laps (in-laps, out-laps, safety car laps) are removed, missing telemetry is handled, tire compound naming is normalized across regulation eras, and DNFs are filtered appropriately.
3. **Transformation:** Lap-level records are aggregated into stint- and race-level summaries, with derived features such as tire age deltas, gap-to-leader trajectories, and position-change metrics — stored as Parquet.
4. **Statistical Modelling:** Regression models decompose lap times into base pace, fuel-burn degradation, tire-age degradation, and traffic penalty components, with confidence intervals for uncertainty quantification.
5. **Visualization:** Users explore the processed dataset through five linked, interactive views spanning season-level to lap-level detail.

---

## Tech Stack

- **Frontend:** React.js, D3.js (custom visualizations), Recharts & Plotly (standard interactive charts)
- **Backend:** Python (Flask / FastAPI), Pandas (data processing), scikit-learn (regression modelling & PCA)
- **Database:** Parquet files, queried via DuckDB
- **Data Sources:** [FastF1](https://docs.fastf1.dev/), [Ergast Developer API](http://ergast.com/mrd/)

---

## Getting Started

### Prerequisites

- Python 3.10+
- Node.js (v16+) and npm
- pip / a Python virtual environment tool

### Installation

1. **Clone the repository:**

   ```
   git clone https://github.com/<your-username>/f1-race-strategy-visual-analytics.git
   cd f1-race-strategy-visual-analytics
   ```

2. **Backend setup:**

   ```
   cd server
   python -m venv venv
   source venv/bin/activate
   pip install -r requirements.txt
   python app.py
   ```

3. **Frontend setup:**

   ```
   cd ../client
   npm install
   npm run dev
   ```

4. **Access the app:**
   Open your browser at `http://localhost:3000`

> Note: since the project is still in development, exact setup steps will be finalized as the backend/frontend structure stabilizes.

---

## Project Structure

```
f1-race-strategy-visual-analytics/
├── client/                 # Frontend (React)
│   ├── src/
│   │   ├── components/     # Chart & view components
│   │   └── pages/          # Season Overview, Race Deep Dive, etc.
│   └── ...
├── server/                  # Backend (Flask/FastAPI)
│   ├── data/                 # Raw & processed Parquet datasets
│   ├── pipeline/              # Acquisition, cleaning, transformation scripts
│   ├── models/                 # Lap time decomposition & regression models
│   └── app.py                   # Server entry point
└── README.md
```

---

## Data Pipeline

| Stage | Description |
|---|---|
| Acquisition | Bulk-download via FastF1 & Ergast for 2018–2024 |
| Preprocessing | Clean laps, normalize compounds, filter DNFs |
| Transformation | Aggregate to stint/race level, derive features, save as Parquet |
| Modelling | Decompose lap times (pace, fuel, tire-age, traffic) with confidence intervals |
| Visualization | Five linked interactive views across the React frontend |

---

## Status

This project is under active development as part of the CS-661 course at IIT Kanpur. Current focus is on data acquisition, cleaning, and the statistical decomposition model, with visualization views being built out incrementally per the roadmap above.

## Team

| Area | Members |
|---|---|
| Data Processing, Cleaning & Statistical Modelling | Abhiram Pande (240038), Rahul Buraniya (240829) |
| Backend Development | Aman Ahuja (240099), Vaibhav Vishwakarma (241129) |
| Visualization Development | Yogesh Rewar (241213), Prathu Agarwal (240782) |
| Frontend Development | Daksh Saijwal (240318), Bhav Agarwal (240266) |

---

## Contributing

This is a course project, but suggestions and issue reports are welcome. Feel free to open an issue or submit a pull request.

---

## License

This project is intended for academic purposes as part of CS-661, IIT Kanpur, and is not affiliated with Formula 1, FIA, or any F1 team.
