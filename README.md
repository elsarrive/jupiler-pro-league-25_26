# Streamlit Dashboard – Jupiler Pro League Predictions 2025-2026 

[![Python](https://img.shields.io/badge/python-3.8%2B-blue.svg)](https://www.python.org/) [![Streamlit](https://img.shields.io/badge/streamlit-%3E%3D1.0-brightgreen)](https://streamlit.io/) 

Welcome to this project visualising predictions for the Jupiler Pro League!

[![Open in Streamlit](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://jupiler-pro-league-25-26.streamlit.app/)

## Context & Origin

Initial group project: <a href="https://github.com/becodeorg/football-prediction-olympiakos">here!</a>  
↳ Dashboard and live prediction algorithms via API. But...  
If no matches are scheduled, the interface remains empty!

Solution :   
↳ We embed a snapshot (new_predictions.csv) to ensure that something is displayed each time the application is launched. So, please note that the data aren't accurate!

## Features 
- Display of predictions (home win, draw, away win)  
- Detailed statistics for each team's last 18 matches  
- Donut charts for win/loss/draw rates  
- Mini tables showing the last 5 results (score, date)  
- Live season rankings  
- Responsive theme and layout via custom CSS

## Project structure 
<pre>
├── README.md  
├── main.py                  # Streamlit main entry point  
├── api.py                   # API call wrapper (disabled here)  
├── df_without_missings.csv  # Last season's history (frozen)  
├── new_predictions.csv      # Snapshot of upcoming matches  
├── requirements.txt         # Python dependencies  
├── style.css                # CSS styles for Streamlit  
├── .streamlit/  
│   └── config.toml          # Streamlit configuration (theme, port, sidebar, etc.)  
└── utils/  
    ├── functions.py         # Utility functions (stats, graphs, etc.)  
    └── lists_variables.py   # Constants: column lists, team mappings  
</pre>

## Installation
1. Clone the repository
```bash 
git clone https://github.com/elsarrive/jupiler-pro-league-25_26.git
```

2. Create and activate a virtual environment
```bash 
python -m venv venv
# macOS/Linux
source venv/bin/activate
# Windows PowerShell
.\venv\Scripts\activate
```

3. Install dependencies
```bash 
pip install -r requirements.txt
```

## Local launching 
```bash 
streamlit run main.py
```

## Component details
#### main.py
- Loads the frozen CSV files (df_without_missings.csv, new_predictions.csv)  
- Applies the team mapping (teamname_mapping)  
- Configures Streamlit (title, layout, CSS)  
- Generates dynamic tabs for each match to be predicted  
- Calls render_matches() to display the entire dashboard
```bash
# Example of a key block

tab_labels = [f"{m['HomeTeam']} - {m['AwayTeam']}" for m in matches]
tabs = st.tabs(tab_labels)

for idx, (tab, match) in enumerate(zip(tabs, matches)):
    with tab:
        render_matches(match, df, uid=idx)
```

#### utils/functions.py
| Function | Role |
|----------|------|
| former_18_matches()    | Selects a team's last 18 matches and calculates goals, % shots on target, cards, fouls, corners, etc. |
| stats()                | Based on the 18 matches: calculates win/loss/draw rates and total/average goals|
| create_side_table()    | Mini table of the last 5 results: Date, Score, Opponent      |
| create_side_stats()    | Details of stats (cards, fouls, corners) for these same 5 matches  |
| build_classement ()     | Generate the current season's standings: points, games played |
| make_donut()           | Create a ring chart (Altair) to represent a percentage |
| render_matches()       | Orchestrator: call all functions and structure the Streamlit UI (columns, tabs, tables, charts, etc.)

#### utils/lists_variables.py
- folder_matches_cols: columns for the mini-table (last 5 matches)  
- stats_cols: columns needed to calculate statistics (last 18 matches)  
- teamname_mapping: dictionary to standardise team names

## Update the data
- Regenerate your CSV new_predictions.csv with the API or manually.  
- Restart streamlit run main.py 
- That's it, your new predictions are now taken into account! 