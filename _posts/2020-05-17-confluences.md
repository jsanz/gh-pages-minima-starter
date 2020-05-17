---
title: 🌐 Confluences
date: 2020-05-17
layout: post
---

Last week when reading Ken Jennings *Maphead* I discovered the [Degree Confluence project](http://confluence.org/) (people visiting the latitude and longitude degree integer intersections). So this weekend, I spent some time investigating where these intersections are and which is the closest one to my current location using GeoPandas. 

```python
import pandas as pd
import numpy as np
import geopandas as gpd
import matplotlib.pyplot as plt
from shapely.geometry import Point, LineString
from shapely.ops import nearest_points
import mapclassify
from scipy.spatial import cKDTree
```

### 🔴 Confluences

```python
lats = [lat for lat in range(-90, 100, 10)] # A 10 degree step was set to sample the research
lngs = [lng for lng in range(-180, 190, 10)]
coords = [(lat, lng) for lng in lngs for lat in lats]

points_df = pd.DataFrame(coords, columns=['lat', 'lng'])
points_geometry = [Point(xy) for xy in zip(points_df.lng, points_df.lat)]
crs = {'init': 'epsg:4326'}
points = gpd.GeoDataFrame(points_df, crs=crs, geometry=points_geometry)
```

```python
world = gpd.read_file(gpd.datasets.get_path('naturalearth_lowres'))

fig, ax = plt.subplots(figsize=(15, 15))
basemap = world.to_crs('+proj=robin').plot(ax=ax, color='#a6a6a6', edgecolor='white')
points.to_crs('+proj=robin').plot(ax=ax, marker='o', color='red', markersize=5)
plt.axis('off')
ax.set_title("Confluences", fontsize=20)
```

![confluences](https://github.com/ramiroaznar/blog/blob/master/assets/imgs/2020-05-17-confluences.png?raw=true)

### 🌐 Grid

```python
meridians = [
    (Point(lng, min(lats)), Point(lng, max(lats))) for lng in lngs
]
paralells = [
    (Point(min(lngs), lat), Point(max(lngs), lat)) for lat in lats
]

meridians_df = pd.DataFrame(meridians, columns=['origin', 'destination'])
paralells_df = pd.DataFrame(paralells, columns=['origin', 'destination'])
grid_df = pd.concat([meridians_df, paralells_df])
grid_df['lines'] = grid_df.apply(lambda row: LineString([row.origin, row.destination]), axis=1)
grid = gpd.GeoDataFrame(grid_df, crs=crs, geometry=grid_df['lines'])
```

```python
fig, ax = plt.subplots(figsize=(15, 15))
basemap = world.plot(ax=ax, color='#a6a6a6', edgecolor='white')
grid.plot(ax=ax)
plt.axis('off')
ax.set_title("Grid", fontsize=20)
```

![grid](https://github.com/ramiroaznar/blog/blob/master/assets/imgs/2020-05-17-grid.png?raw=true)

### 🌡️ Density

```python
world['count'] = world['geometry'].apply(
    lambda row: np.sum(points['geometry'].intersects(row))
)
world['density'] = world['count']/world['geometry'].area
```

```python
fig, ax = plt.subplots(figsize=(15, 15))

world.to_crs('+proj=robin').plot(column='density', scheme="fisher_jenks", figsize=(15,10), ax=ax, legend=True)
plt.axis('off')
ax.set_title("Confluences Density", fontsize=20)
```

![density](https://github.com/ramiroaznar/blog/blob/master/assets/imgs/2020-05-17-confluences-density.png?raw=true)

### 📏 Nearest

```python
url = 'https://www.naturalearthdata.com/http//www.naturalearthdata.com/download/10m/cultural/ne_10m_populated_places.zip'
cities = gpd.read_file(url)
```

```python
# from https://gis.stackexchange.com/a/301935/41250
def get_nearest(gdf_1, gdf_2):
    n_1 = np.array(list(zip(gdf_1.geometry.x, gdf_1.geometry.y)) )
    n_2 = np.array(list(zip(gdf_2.geometry.x, gdf_2.geometry.y)) )
    btree = cKDTree(n_2)
    dist, idx = btree.query(n_1, k=1)
    gdf = pd.concat(
        [gdf_1.reset_index(drop=True), gdf_2.loc[idx, gdf_2.columns != 'geometry'].reset_index(drop=True),
         pd.Series(dist, name='distance')], axis=1)
    return gdf
```

```python
nearest = get_nearest(points, cities)
nearest_df = nearest[['NAME', 'distance']].drop_duplicates(['NAME'])
nearest_df = nearest_df.merge(cities[['geometry', 'NAME', 'ADM0NAME']], on='NAME', how='left')
nearest_cities = gpd.GeoDataFrame(nearest_df, geometry='geometry')
```

```python
fig, ax = plt.subplots(figsize=(15, 15))

xlim = [5.42, 15.95]
ylim = [48.15, 55.18]
ax.set_xlim(xlim)
ax.set_ylim(ylim)

berlin = cities.loc[cities.NAME == 'Berlin']

basemap = world.plot(ax=ax, color='#a6a6a6', edgecolor='white')

grid.plot(ax=ax)

berlin.plot(ax=ax, marker='o', color='white', markersize=50)
berlin.plot(ax=ax, marker='o', color='black', markersize=5)
plt.text(berlin.iloc[0].geometry.x+0.07, berlin.iloc[0].geometry.y+0.07, berlin.iloc[0].NAME, fontsize=12)


nearest_cities.plot(ax=ax, marker='o', color='pink', markersize=40)
nearest_cities.plot(ax=ax, marker='o', color='red', markersize=5)

for x, y, country, label in zip(nearest_cities.geometry.x, nearest_cities.geometry.y, nearest_cities["ADM0NAME"], nearest_cities["NAME"]):
    if country == 'Germany':
        plt.text(x-0.75, y-0.25, label, fontsize=12)

plt.axis('off')        
ax.set_title("My nearest confluence", fontsize=20)
```

![nearest](https://github.com/ramiroaznar/blog/blob/master/assets/imgs/2020-05-17-nearest.png?raw=true)


The entire Jupyter notebook can be found [here](https://github.com/ramiroaznar/labs/blob/master/confluences.ipynb).
