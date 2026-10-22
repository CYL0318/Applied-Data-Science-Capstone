# Applied Data Science Capstone — SpaceX Falcon 9 First Stage Landing Prediction

This repository contains the complete project for the **IBM Applied Data Science Capstone**. SpaceX advertises Falcon 9 rocket launches at a cost of 62 million dollars, while other providers cost upward of 165 million dollars each — much of the saving comes from SpaceX's ability to reuse the first stage. This project collects Falcon 9 launch data, explores it with visual analytics and SQL, and builds machine learning models to **predict whether the Falcon 9 first stage will land successfully**, which in turn allows the cost of a launch to be estimated.

## Contents

| File | Description |
|---|---|
| `jupyter-labs-spacex-data-collection-api.ipynb` | Collect launch data from the SpaceX REST API, resolve rocket/payload/launchpad/core IDs, filter Falcon 9 launches and export `dataset_part_1.csv`. |
| `jupyter-labs-webscraping.ipynb` | Scrape the Wikipedia page "List of Falcon 9 and Falcon Heavy launches" with BeautifulSoup and export `spacex_web_scraped.csv`. |
| `labs-jupyter-spacex-Data_wrangling.ipynb` | Exploratory data analysis and creation of the binary landing-outcome label (`Class`), exporting `dataset_part_2.csv`. |
| `jupyter-labs-eda-sql-coursera_sqllite.ipynb` | Load the dataset into SQLite and answer business questions with SQL queries on `SPACEXTBL`. |
| `jupyter-labs-eda-dataviz.ipynb` | Visual EDA with seaborn/matplotlib: launch sites, payload mass, orbits and the yearly success trend. |
| `lab_jupyter_launch_site_location.ipynb` | Interactive Folium maps of launch sites, launch outcomes and distances to coastlines, highways, railways and cities. |
| `spacex_dash_app.py` | Interactive Plotly Dash dashboard with a launch-site dropdown, success pie chart, payload range slider and payload-vs-class scatter chart. |
| `SpaceX_Machine_Learning_Prediction_Part_5.ipynb` | Machine learning pipeline: standardization, train/test split, GridSearchCV tuning of Logistic Regression, SVM, Decision Tree and KNN, and model comparison. |
| `SpaceX_Falcon9_Capstone_Presentation.pdf` | Final presentation deck covering data collection, wrangling, EDA, SQL, maps, dashboard and predictive modelling. |

## Datasets

The notebooks read the course datasets directly from IBM Cloud Object Storage:

- `dataset_part_1.csv`, `dataset_part_2.csv`, `dataset_part_3.csv` — [IBM-DS0321EN-SkillsNetwork/datasets](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DS0321EN-SkillsNetwork/datasets/dataset_part_1.csv)
- `Spacex.csv` (SQL lab) — [IBM-DS0321EN-SkillsNetwork/labs/Coursera_3/Spacex.csv](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DS0321EN-SkillsNetwork/labs/Coursera_3/Spacex.csv)
- `spacex_launch_geo.csv` (Folium lab) and `spacex_launch_dash.csv` (Dash app) — same Skills Network bucket
- Live data: [SpaceX REST API](https://api.spacexdata.com/v4/launches/past) and the [Wikipedia launch list](https://en.wikipedia.org/wiki/List_of_Falcon_9_and_Falcon_Heavy_launches)

## Tools Used

- **Python 3** with **pandas** and **NumPy** for data manipulation
- **requests** + **BeautifulSoup** for API data collection and web scraping
- **SQL / SQLite** (`ipython-sql`) for relational exploration
- **Matplotlib** and **seaborn** for static visualizations
- **Folium** for interactive geospatial maps
- **Plotly Dash** for the interactive dashboard
- **scikit-learn** for machine learning (GridSearchCV, LogisticRegression, SVC, DecisionTreeClassifier, KNeighborsClassifier)

## How to Run

1. Install the dependencies:
   ```bash
   pip install pandas numpy requests beautifulsoup4 matplotlib seaborn folium plotly dash scikit-learn ipython-sql jupyter
   ```
2. Run the notebooks in order (data collection → web scraping → wrangling → EDA/SQL → visualization → machine learning):
   ```bash
   jupyter notebook
   ```
3. Start the dashboard:
   ```bash
   python spacex_dash_app.py
   ```
   Then open http://127.0.0.1:8050/ in your browser.

## Results

On the 90 Falcon 9 flights in the dataset, 60 first stages landed successfully (66.7%), so every model is judged against that majority-class baseline.

| Model | Best 10-fold CV accuracy | Held-out accuracy |
|---|---|---|
| Logistic Regression | 0.846 | 0.833 |
| Support Vector Machine | 0.863 | 0.833 |
| Decision Tree | **0.886** | 0.778 |
| K Nearest Neighbors | 0.848 | 0.833 |

The Decision Tree wins cross-validation but generalises worst, while the simpler linear models hold 0.833 on the test set — a sign that the tree's extra depth is fitting quirks of a 72-row training set. Orbit type and booster generation carry most of the predictive signal, while launch-site identity matters far less once those two are known.
