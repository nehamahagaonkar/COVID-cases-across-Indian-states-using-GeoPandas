# COVID-19 Spatial Analysis of India

## Overview

This project visualizes the spread of COVID-19 across Indian states using geospatial data analysis in Python. It combines geographic boundary data (shapefiles) with COVID-19 statistics retrieved from an API to create a choropleth map showing the distribution of confirmed cases across the country.

The goal of this project is to demonstrate how spatial data and statistical datasets can be integrated to reveal geographic patterns in real-world data.

---

## Project Objectives

* Collect COVID-19 data using an API
* Work with geospatial shapefiles representing Indian state boundaries
* Merge statistical data with geographic data
* Visualize the spread of COVID-19 across states using choropleth maps
* Demonstrate a complete data science workflow from data collection to visualization

---

## Technologies Used

* **Python**
* **Pandas** – data manipulation and cleaning
* **GeoPandas** – handling geospatial datasets
* **Matplotlib** – data visualization
* **Requests** – fetching data from API

---

## Data Sources

**1. Geographic Data**

Indian state boundary shapefiles from the GADM database.

Files used:

* `gadm41_IND_1.shp` – State level boundaries
* Additional shapefile components (.dbf, .shx, .prj, etc.)

**2. COVID-19 Data**

COVID-19 statistics were obtained from the COVID-19 India API which provides state-wise case data including:

* confirmed cases
* active cases
* recovered cases
* deaths
* daily updates

---

## Project Workflow

### 1. Load Geospatial Data

The shapefile containing Indian state boundaries is loaded using GeoPandas.

```python
import geopandas as gpd

india_map = gpd.read_file("gadm41_IND_1.shp")
```

---

### 2. Fetch COVID-19 Data from API

```python
import requests
import pandas as pd

url = "https://api.covid19india.org/data.json"
response = requests.get(url)
data = response.json()

state_data = pd.DataFrame(data["statewise"])
```

---

### 3. Data Cleaning and Preparation

The dataset is cleaned and relevant numeric fields are converted to appropriate types.

```python
state_data["confirmed"] = pd.to_numeric(state_data["confirmed"])
state_data["deaths"] = pd.to_numeric(state_data["deaths"])
state_data["recovered"] = pd.to_numeric(state_data["recovered"])
```

---

### 4. Merge Geospatial and COVID Data

The COVID dataset is merged with the geographic dataset using the state name.

```python
merged = india_map.merge(
    state_data,
    left_on="NAME_1",
    right_on="state"
)
```

This step combines the geographic boundaries with COVID statistics.

---

### 5. Create Choropleth Map

```python
import matplotlib.pyplot as plt

merged.plot(
    column="confirmed",
    cmap="Reds",
    legend=True,
    edgecolor="black",
    figsize=(10,10)
)

plt.title("COVID-19 Confirmed Cases Across Indian States")
plt.axis("off")
plt.show()
```

The result is a choropleth map where darker regions indicate higher numbers of confirmed cases.

---

##  Output

The visualization highlights geographic patterns in COVID-19 cases across India, making it easier to observe which states experienced higher infection counts.
<img width="809" height="813" alt="image" src="https://github.com/user-attachments/assets/0fa0321c-7115-475b-9bc2-ede831464aa0" />


---

## Key Concepts Demonstrated

* Geospatial data analysis
* Choropleth mapping
* API data collection
* Data cleaning and transformation
* Data merging using keys
* Visual analytics
---

## Future Improvements

Possible extensions to this project include:

* Visualizing **cases per million population**
* Adding **interactive maps using Folium or Plotly**
* Creating **time-series visualizations of case growth**
* Mapping **district-level COVID-19 data**
* Performing **spatial clustering analysis**

---

## Conclusion

This project demonstrates how geospatial data analysis can be used to explore and visualize real-world datasets. By combining geographic boundaries with COVID-19 statistics, the project provides a clear spatial perspective on the distribution of cases across Indian states.

Such techniques are widely used in epidemiology, public health research, and policy analysis to better understand the geographic dynamics of disease spread.

