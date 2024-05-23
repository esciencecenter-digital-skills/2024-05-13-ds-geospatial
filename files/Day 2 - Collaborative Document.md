![](https://i.imgur.com/iywjz8s.png)


# Day 2 - Collaborative Document

2024-05-14 Introduction to Geospatial Raster and Vector Data with Python

Welcome to The Workshop Collaborative Document.

This Document is synchronized as you type, so that everyone viewing this page sees the same text. This allows you to collaborate seamlessly on documents.

----------------------------------------------------------------------------

This is the Document for today: [link](https://tinyurl.com/2024-05-14-geospatial-python)

Collaborative Document day 1: [link](https://tinyurl.com/2024-05-13-geospatial-python)

Collaborative Document day 2: [link](https://tinyurl.com/2024-05-14-geospatial-python)

Collaborative Document day 3: [link](https://tinyurl.com/2024-05-15-geospatial-python)

Collaborative Document day 4: [link](https://tinyurl.com/2024-05-16-geospatial-python)


## 👮Code of Conduct

Participants are expected to follow these guidelines:
* Use welcoming and inclusive language.
* Be respectful of different viewpoints and experiences.
* Gracefully accept constructive criticism.
* Focus on what is best for the community.
* Show courtesy and respect towards other community members.
 
## ⚖️ License

All content is publicly available under the Creative Commons Attribution License: [creativecommons.org/licenses/by/4.0/](https://creativecommons.org/licenses/by/4.0/).

## 🙋Getting help

To ask a question just raise your hand or put it in the chat. 


## 🖥 Links

Workshop website: [link](https://esciencecenter-digital-skills.github.io/2024-05-13-ds-geospatial/)

🛠 Setup: [link](https://carpentries-incubator.github.io/geospatial-python/index.html)

🔧 Download files: [link](https://figshare.com/ndownloader/articles/25721754/versions/3)

💻 Zoom link: [link](https://us02web.zoom.us/j/82696360872?pwd=SmJaMFdxcE83UUNGL3dMRHl2QWZMUT09)

## 👩‍🏫👩‍🙋💻🎓 Instructors & Helpers

Maurice de Kleijn, Francesco Nattino, Ou Ku

## 🗓️ Agenda
 
**Day 2**

| Time  | Topic                                   |
| ----- | --------------------------------------- |
| 09:00 | Welcome and icebreaker   |
| 09:10 | Vector data in Python        |
| 10:15 | **Coffee break**                        |
| 10:30 | Crop raster data with Rioxarray and geopandas     |
| 11:30 | **Coffee break**                        |
| 11:50 | Crop raster data with Rioxarray and geopandas         |
| 12:50 | Wrap up & feedback|
| 13:00 | **END of Day 2** |

**Day 3**

| Time  | Topic                                   |
| ----- | --------------------------------------- |
| 09:00 | Welcome and icebreaker                  |
| 09:10 | Raster calculations in python           |
| 10:15 | **Coffee break**                        |
| 10:25 | Raster calculations in python           |
| 11:30 | **Coffee break**                        |
| 11:40 | Calculating Zonal Statistics on Rasters  |
| 12:50 | Wrap up & feedback|
| 13:00 | **END of Day 3** |


**Day 4**

| Time  | Topic                                   |
| ----- | --------------------------------------- |
| 09:00 | Welcome and icebreaker   |
| 09:10 | Access satellite imagery using Python    |
| 10:15 | **Coffee break**                        |
| 10:25 | Parallel raster Computations using Dask     |
| 11:30 | **Coffee break**                        |
| 11:40 | Parallel raster Computations using Dask         |
| 12:50 | Post-workshop survey |
| 13:00 | **END of Day 4 & WORKSHOP** |


## 🎓 Certificate of attendance
If you attend the full workshop you can request a certificate of attendance by emailing to training@esciencecenter.nl .

## 🔧 Exercises

### Exercise 1: Get built-up regions from Open Street Map (OSM)



Now that we have a buffered dataset for the key infrastructure of Rhodes, our next step is to create a dataset with all the built-up areas. To do so we will use the land use data from OSM, which we prepared for you in the file data_workshop/osm_landuse.gpkg. This file includes the land use data for the entire Greece. We assume the built-up regions to be the union of three types of land use: "commercial", "industrial", and "residential".

Note that for the simplicity of this course, we limit the built-up regions to these three types of land use. In reality, the built-up regions can be more complex also there is definately more high quality (e.g. local government).

Now it will be up to you to create a dataset with valueable assets. You should be able to complete this task by yourself with the knowledge you have gained from the previous steps and links to the documentation we provided.
Exercise: Get the built-up regions

Create a builtup_buffer from the file /osm/osm_landuse.gpkg by the following steps:

   - Load the land use data from osm/osm_landuse.gpkg and mask it with the administrative boundary of Rhodes Island (gdf_rhodes).
   - Select the land use data for "commercial", "industrial", and "residential".
   - Create a 10m buffer around the land use data.
   - Visualize the results.

After completing the exercise, answer the following questions:

    - How many unique land use types are there in osm_landuse.gpkg?
    - After selecting the three types of land use, how many entries (rows) are there in the results?

Hints:

    - data/osm_landuse.gpkg contains the land use data for the entire Greece. Use the administrative boundary of Rhodes Island (gdf_rhodes) to select the land use data for Rhodes Island.
    - The land use attribute is stored in the fclass column.
    - Reuse buffer_crs function to create the buffer.


#### Solution Exercise 1

Below is one potential solution:

```python
gdf_landuse = gpd.read_file("./data/osm/osm_landuse.gpkg", mask=gdf_rhodes)

# visualize
gdf_landuse.explore()

# Check how many unique land uses are there
print(len(gdf_landuse["fclass"].unique()))

# Make a list of the labels we want to select
builtup_labels = ["commercial", "industrial", "residential"]

# select the three attributes we set 
builtup = gdf_landuse.loc[gdf_landuse["fclass"].isin(builtup_labels)]

# Use the function we already created
builtup_buffer = buffer_crs(builtup, 10)

# Visualize the built-up region again
builtup.explore()

# check how many polygons are there
print(len(builtup_buffer))
```

### Exercise 2

Now that you have seen how clip a raster using a polygon, we want you to do this for the red band of the sattelite image. Use the shape of Rhodes from GADM and clip the red band with it. Furthermore, make sure to transform the no data values to Not a Number values. 

Data to be used:
"./data/gadm/ADM_ADM_3.gpkg"
"./data/sentinel2/red.tif"

#### Solution Exercise 2

```python
# In case the packages haven't been inported
import geopandas as gpd
import rioxarray

gdf_greece = gpd.read_file("./data/gadm/ADM_ADM_3.gpkg")

gdf_rhodes = gdf_greece.loc[gdf_greece["NAME_3"]=="Rhodos"]

gdf_rhodes.explore()

path_red = "./data/sentinel2/red.tif"

red = rioxarray.open_rasterio(path_red, overview_level=1, masked=True)

gdf_rhodes = gdf_rhodes.to_crs(red.rio.crs)

red_clip = red.rio.clip(gdf_rhodes["geometry"])

red.plot()
```



## 🧠 Collaborative Notes


### Intro to Vector in python [FROM DAY 1]

We create a new notebook to work with vector data

Package to work with vector data: [`geopandas`](https://geopandas.org/en/stable/). It's the geo- extension of pandas. In pandas, we have DataFrames and Series, Geopandas adds the concept of GeoSeries, i.e. a Series with geometry information.

We start by importing the library:

```python=
import geopandas as gpd  # gpd is the standard alias for geopandas 
```

#### Administrative boundaries

Load data:
```python=
# the data path might be different
gdf_greece = gpd.read_file("./data/gadm/ADM_ADM_3.gpkg")
```

In Jupyter, (Geo)DataFrames have a nice tabular representation (you can try with `gdf_greece`). You can also plot it to get a quick representation:

```python=
gdf_greece.plot()
```

.. or for an interactive representation: 

```python=
gdf_greece.explore()
```

You can explore dataset, including hovering on shapes to see attributes. Hovering over Rhodes, shows that we can select its boundaries with the feature "NAME_3" is "Rhodos"  

```python=
gdf_rhodes = gdf_greece.loc[gdf_greece["NAME_3"] == "Rhodos"]
```

Save output of filtering, i.e. Rhodes' boundaries:
```python=
gdf_rhodes.to_file("rhodes_130524.gpkg")
```

#### Roads

Now we load the roads on Rhodes, from OSM - it takes few seconds:
```python=
gdf_roads = gpd.read_file("data/osm/osm_roads.gpkg")
```

Show the first few lines of the table:
```python=
gdf_roads.head(10) # change 10 to the number of desired rows
```

We can plot it and explore it using:
```
gdf_roads.plot()
gdf_roads.explore()
```

The dataset includes all roads and paths for many islands of Greece, we want to limit our focus to Rhodes and the main roads. 

Select only roads of Rhodes:
```python=
gdf_roads = gpd.read_file("data/osm/osm_roads.gpkg", mask=gdf_rhodes)
```

By providing a geodataframe as argument to `mask`, we select only the geometries intersecting the mask.

Now we want to select only the main roads. First let's have a look at the values available in `fclass`:
```python
gdf_roads['fclass'].unique()
```

We see all the values in the `fclass` column. Let's select only rows with the following values: 
```python=
key_infra_labels = ["primary", "secondary", "tertiary"]
key_infra = gdf_roads.loc[gdf_roads["fclass"].isin(key_infra_labels)]
```

Let's visualize it again to see the remaining lines:
```python
key_infra.explore()
```

### Intro to Vector in python [START DAY 2]

Some basic operations of GIS analysis

1. Intersection: Points in Polygon; Line in Polygon
2. Overlay: UNION, COMBINE/DESSOLVE, INTERSECT/CLIP
3. Buffer

Today we are going to combine vector and raster data, and investigate which assest(s) on Rhodes Island are vulnerable to wild fire

Now we have a subseltion of roads `key_infra`, let lay a buffer of 100 meters

```python
# This will get into an error
key_infra_meters_buffer = key_infra.buffer(100)
```

The previous script will get into the following error

```output
/tmp/ipykernel_2486/1673672606.py:2: UserWarning: Geometry is in a geographic CRS. Results from 'buffer' are likely incorrect. Use 'GeoSeries.to_crs()' to re-project geometries to a projected CRS before this operation.

  key_infra_meters_buffer = key_infra.buffer(100)
```

This is because `key_infra` is in a degree CRS. `.buffer(100)` will grow a buffer of 100 degrees.

We need to first reproject `key_infra` to a CRS with meters. This can be done by coverting between two EPSG codes.

Let's first define a variable with the target EPSG code to project

```python=
epsg_code = 32631
```

Make a new variable project the `key_infra` variable to a new CRS, this can be done with the `to_crs()` function:

```python
key_infra_meters = key_infra.to_crs(epsg_code)
```

let's plot out the reprojected roads

```python
key_infra_meters.plot()
```

Now let's grow the buffer again

```python
key_infra_meters_buffer = key_infra_meters.buffer(100)
```

And plot the buffer out

```python
key_infra_meters_buffer.plot()
```

Since the buffer is small, we can also use the `explore()` funtion to make the visualization more informative.

```python
key_infra_meters_buffer.explore()
````

Covert the buffer back into the original CRS as the same as `key_infra`. This is to keep most of our data into the same CRS

```python
key_infra_buffer = key_infra_meters_buffer.to_crs(key_infra.crs)
```

Print out the variable `key_infra_buffer

```python
key_infra_buffer
```

Compare the two CRS, the original roads and the buffer

```python
print(key_infra.crs)
```

```python
print(key_infra_buffer.crs)
```

The above two should be `EPSG:4326`, which should be different from the buffer in the meter system, which is `EPSG:32631`

```python
print(key_infra_meters_buffer.crs)
```

In a python workflow, if the buffering and CRS conversion operations need to be done manytimes, one can create a function to avoid dupicating the code:

```python
def buffer_crs(gdf, size, meter_crs=32631, target_crs=4326):
    return gdf.to_crs(meter_crs).buffer(size).to_crs(target_crs)
```

Let's use the function we just created and again grow a 50m buffer of `key_infra`

```python
key_infra_buffer_50 = buffer_crs(key_infra, 50)
```

```python
key_infra_buffer_50.explore()
```

And a 500m buffer can simply be created by changing the `50` to `500`:

```python
key_infra_buffer_500 = buffer_crs(key_infra, 500)
```

Discussion: What if I am making a buffer under a CRS which is not suitable for my area of interest (AoI)

> The recommendation would be using a local projection close to your AoI.


Merge the infrastructure (roads) and the built-up area:

First we setup a dictionary to record our geoDataFrame for infrastructure:

```python
data = {"geometry": key_infra_buffer, "type": "infrastructure", "code": 1}
```

We they covert the dictionary we just created to a GeoDataFram

```python
gdf_infra = gpd.GeoDataFrame(data)
```

We can do the same to the built-up region. first create a dictionary, and convert it to a `GeoDataFrame`

```python
data = {"geometry": builtup_buffer, "type": "builtup", "code": 2}
```

```python
gdf_builtup = gpd.GeoDataFrame(data)
```

There is current no easy function from `geopandas` to merge two `GeoDataFrame`. We will use the `concat` function from `pandas` to merge them together.

```python
import pandas as pd
gdf_assets = pd.concat([gdf_infra, gdf_builtup]).reset_index(drop=True)
```

We use `reset_index(drop=True)` here to drop the original index from  `gdf_infra` and `gdf_builtup` and giving them a new index.

```python=
gdf_assets.plot(column="type", legend=True)
```

```python
gdf_assets.to_file("assets.gpkg")
```


### Crop raster data with the polygons

Let's create a new notebook: `Workshop_day2_cropping_raster.ipynb`

```python
import rioxarray
```

```python
path_visual = "../data/sentinel2/visual.tif"
```

Let's load the COG image with an overview level 1

```python
visual=rioxarray.open_rasterio(path_visual, overview_level=1)
```

```python
visual.plot.imshow()
```


Let's also read the assets file we just created, and visualize it

```python
import geopandas as gpd
assets = gpd.read_file("assets.gpkg")
```

```python
assets.explore()
```

Let's check the extent (bounding box) of the vector datasets

```python
assets.total_bounds # in xmin, ymin, xmax, ymax
```

```output
array([27.71628289, 35.89024659, 28.24128748, 36.4569273 ])
```


Let's compare the CRS of the raster and vector dataset

```python
print(visual.rio.crs)
print(assets.crs)
```

```output
EPSG:32635
EPSG:4326
```

Here we convert the CRS of the assets vectore data to the CRS of the visual image.

```python
assets = assets.to_crs(visual.rio.crs)
```

```python
print(assets.crs)
```

And we can clip the visual image by the bounding boxes to the vector data:

```python
# We use and "*" symbol to unpack the array: assets.total_bounds
visual_clipbox = visual.rio.clip_box(*assets.total_bounds)
```

we visualize the image cropped by the bounding box:

```python
visual_clipbox.plot.imshow()
```

Let's further clip the `visual_clipbox` to the buffers we just created.

```python
visual_clip = visual_clipbox.rio.clip(assets["geometry"])
```

and visualize the clip version

```python
visual_clip.plot.imshow()
```

### Match two rasters

We will now match the DEM raster data to the sentinel 2 image

First we load the DEM tiff file:

```python
dem = rioxarray.open_rasterio("./data/dem/rhodes_dem.tif")
```

Visualize the DEM

```
dem.plot()
```

Check the CRS of the DEM and the Sentinel-2 image:

```python
print(dem.rio.crs)
print(visual_clip.rio.crs)
```

```output
EPSG:4326
EPSG:32635
```

Now we match the two rasters together. `reproject_match()` function can do the reprojection and raster match in one go 

```python
dem_matched = dem.rio.reproject_match(visual_clip)
```

```python
dem_matched.plot()
```

`reproject_match` does the following in one go:

- reproject
- match the extent
- match the resolutions

Now we save the matched DEM into Cloud Coverage Geotiff (COG) format

```python
dem_matched.rio.to_raster("dem_rhodes_match.tif", drive="COG")
```

### Raster Calculation

Tomorrow we will derive data products which can reflect the burned area from the Sentinel-2 image.

Different calculations you can perform on a raster:

- Local: calculation applied on a single pixel. E.g. multiply a value to all pixels, or pixel-wise add/multiplication
- Focal: calculations on pixels and their neighbors. E.g. moving window operations.
- Zonal: operations on pixels marked with the same value ("zones").
- Global: operations on the entire grid. E.g. average value a raster. 


## Resources

- An exmaple of how maps can go wrong: [How to lie with maps](https://press.uchicago.edu/ucp/books/book/chicago/H/bo27400568.html)
- To check the CRS details: https://spatialreference.org/
- One Focal calculation example implemented by `xarray-spatial`: [the slope calclulation function](https://xarray-spatial.readthedocs.io/en/stable/_modules/xrspatial/slope.html)

