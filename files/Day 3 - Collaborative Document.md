![](https://i.imgur.com/iywjz8s.png)


# Day 3 - Collaborative Document

2024-05-15 Introduction to Geospatial Raster and Vector Data with Python

Welcome to The Workshop Collaborative Document.

This Document is synchronized as you type, so that everyone viewing this page sees the same text. This allows you to collaborate seamlessly on documents.

----------------------------------------------------------------------------

This is the Document for today: [link](https://tinyurl.com/2024-05-15-geospatial-python)

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

Files created in the past days:

* Rhodes boundaries: [`rhodes_130524.gpkg`](https://github.com/esciencecenter-digital-skills/2024-05-13-ds-geospatial/raw/main/files/rhodes_130524.gpkg)
* Assets: [`assets.gpkg`](https://github.com/esciencecenter-digital-skills/2024-05-13-ds-geospatial/raw/main/files/assets.gpkg)
* Burned area classification mask: [`burned.tif`](https://github.com/esciencecenter-digital-skills/2024-05-13-ds-geospatial/raw/main/files/burned.tif)

## 👩‍🏫👩‍🙋💻🎓 Instructors & Helpers

Maurice de Kleijn, Francesco Nattino, Ou Ku

## 🗓️ Agenda

**Day 3**

| Time  | Topic                                   |
| ----- | --------------------------------------- |
| 09:00 | Welcome and icebreaker                  |
| 09:10 | Raster calculations in python           |
| 10:15 | **Coffee break**                        |
| 10:25 | Raster calculations in python           |
| 11:30 | **Coffee break**                        |
| 11:40 | Raster calculations in python & Calculating Zonal Statistics on Rasters  |
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

### Exercise: NDWI and custom index to detect burned areas

Calculate the other two indices required to compute the burned area classification mask, specifically:

* The [normalized difference water index (NDWI)](https://en.wikipedia.org/wiki/Normalized_difference_water_index), derived from the **green** and **NIR** bands (with band ID "green" and "nir", respectively):

$NDWI=\frac{green - NIR}{green + NIR}$

* A custom index derived from the  1600 nm and the 2200 nm **short-wave infrared (SWIR)** bands ( "swir16" and "swir22", respectively):

$INDEX = \frac{SWIR_{16} - SWIR_{22}}{SWIR_{16} + SWIR_{22}}$

What challenge do you foresee in combining the data from the two indices?


Please fill in the challenges you identified:

| name | challenges |
| ---- | ---------- |
| Kevin     |NDVI/NDWI and INDEX have different shape |
| Eddo     |  I made a mistake in my code, so a wrong conclusistaktey  
| Sandeep     |            |
| Mahaut     | Should we reduce the resolution of the nir and red bands to fit with the swir16 and 22? |
| Frithjof |  The last index is at coarser resolution due to the lower resolution of the bands. Also, while all indices fall between -1 and 1, any linear combination will scale differently.|


## Solution


```python 
#Create a function
def get_band_and_clip(path,bbox):
    band = rioxarray.open_raterio(path, masked = True)
    return band.rio.clip_box(*bbox)

green = get_band_and_clip("data/sentiel2/green.tif", bbox)
swir16_clip = get_band_and_clip("data/sentiel2/swir16.tif", bbox)
swir22_clipgreen = get_band_and_clip("data/sentiel2/swir22.tif", bbox)
#calculate the NDWI
ndwi = (green_clip - nir_clip) / (green_clip + nir_clip)
index = (swir16_clip - swir22_clip) / (swir16_clip + swir22_clip)
```

```python
ndwi.shape, index.shape
```

the resolutions are different.


```python
ndwi.rio.resolution(), index.rio.resolution()
```

We thus need to bring the two rasters to the same shape.

approach 1. to bring the higher resolution to the one with the lower resolution. 

using the ´reproject_match´ will allow you to match the two.

```python
index_match = index.rio.reproject_match(ndwi)
```

Now the resolution is the same, allowing you to perform the calculation.

Another option would have been to work with the rasterio overview level functionality. rioxarray.open_rasterio("path/to.tif", overview_level= 2) etc. 

There are multiple ways to convert the data from the one to the other resolution with rasterio you can assign the alogithm that is used. 





## Question from Day 2
Q: Is it possible to add multiple layers using geopandas´ explore function?

A: Yes it is, but not as straight forward. Have a look at this [link](https://geopandas.org/en/stable/docs/user_guide/interactive_mapping.html) in the Geopandas Documentation. Option to develop interactive maps you could have a look at.

- [openlayers](https://openlayers.org/)
- [leaflet](https://leafletjs.com/)
- [cesium](https://cesium.com/) - In case you are interested in 3D mapping
- [hvplot](https://hvplot.holoviz.org)
- [lonboard](https://developmentseed.org/lonboard/latest/)

## Question from Day 3 

Q: deal with 3rd dimesion
Not a clear answer, but some guidance
- [PDAL](https://pdal.io/en/2.7-maintenance/)
- [DuckDB](https://duckdb.org/)
- [Polars](https://docs.pola.rs/py-polars/html/reference/dataframe/index.html) A geopolar is under development.


## 🧠 Collaborative Notes

### Raster calculations

We will reuse files we created yesterday and on monday.

Files created in the past days:

* Rhodes boundaries: [`rhodes_130524.gpkg`](https://github.com/esciencecenter-digital-skills/2024-05-13-ds-geospatial/raw/main/files/rhodes_130524.gpkg)
* Assets: [`assets.gpkg`](https://github.com/esciencecenter-digital-skills/2024-05-13-ds-geospatial/raw/main/files/assets.gpkg)

We will create a new notebook.

03-Raster Caculations

What are we going to do? We are going to perform some calculations to estimate burned areas vs non burned areas. 

We will use the method from the following script:

https://custom-scripts.sentinel-hub.com/sentinel-2/burned_area_ms/

We are going to reimplement this script to generate a binary classification mask to distinguish burned vs non burned areas. 

We are going to perform local statistics for raster data.

A very oftenly used method is caculating the Normalized Difference Vegetation Index(NDVI)

![](https://codimd.carpentries.org/uploads/upload_ddc1fcc38342da272fb3c963d96fb46d.png)

This is a method to distinguish Vegetated areas vs non vegetation layers. 


Let us load the red band and the Near infrared band

```python
import rioxarray
red = rioxarray.open_rasterio("data/sentinel2/red.tif", masked= True)
nir = rioxarray.open_rasterio("data/sentinel2/nir.tif", masked= True)
```

Now let us load the vector data

```python
import geopandas as gpd
rhodes = gpd.read_file("rhodes_130524.gpkg")
```

Let us look at the geodataframe

```python
rhodes
```

First challenge is to check the crs´s and allign them.

```python
rhodes_reprojected = rhodes.to_crs(red.rio.crs)
```

When you plot it you will see that the coordinates are updated with the crs of the red band

We are going to use the extend of the rhodes geodataframe to clip the satellite image
```python
bbox = rhodes_reprojected.total_bounds
```


```python
red_clip = red.rio.clip_box(*bbox)
nir_clip = nir.rio.clip_box(*bbox)
```

To see how the clip_box function works.
```python
red.rio.clip_box?
```

red_clip and nir_clip objects are not loaded yet. We are first reducing the loading. We just load what we need. 

```python
red_clip.plot(robust=True)
```
(remember robust for visualisation purposes)

```python
nir_clip.plot(robust=True)
```
To have an idea on where the fire took place. Have a look at the picture published by ESA.
![](https://codimd.carpentries.org/uploads/upload_817704c10b47b8f6ddb9d0f47f182a6e.png)

From https://www.esa.int/ESA_Multimedia/Images/2023/07/Rhodes_wildfire_forces_thousands_to_flee
![](https://codimd.carpentries.org/uploads/upload_b2007507f0c1ecbe5b0a207e35e21a36.png)

We have these two rasters. Now we want calculate the index:

![](https://codimd.carpentries.org/uploads/upload_e8922b1c0d56d5a168ac99ce8b0492ef.png)

In order to do this, we need the rasters to have the same shape.

```python
red_clip.shape, nir_clip.shape
```

You will also need to check the crs of the rasters you want to use in your calculations

```python
red_clip.rio.crs, nir_clip.rio.crs
```

Now we perform the calculation

```python
ndvi = (nir_clip - red_clip) / (nir_clip + red_clip)
```

If the shape and CRS are not matching, then Xarray is trying to help you and will try to combine coordinate values and dimensions. 

The ´reproject_match´ function from the rio accessor is not exclusively to reproject, but also to match even if they have the same projection.

Let us have a look at the outcome.

```python
ndvi.plot()
```
If you want to do a specific plot you do a plot through the attribute ´plot´. For instance to do the histogram.

```python
ndvi.plot.hist()
```

```python
ndvi.plot(vmin=0, vmax=0)
```

Have a look at matplot lib to customize your plot. 
https://matplotlib.org/

Now it is your turn to work on this [exercise](https://codimd.carpentries.org/iMYSoxG_Tu2_KSaeXD2jSQ?view#%F0%9F%94%A7-Exercises)! 


```python
swir16_clip_match = swir16_clip.rio.reproject_match(ndwi)
blue_clip = get_band_and_clip("data/sentinel/blue.tif", bbox)
```
*Note that we are using the answers (i.e. the function we created) to the exercise here as well*


```python
mask = ndvi <= 0.3
```

This creates a binary raster that meets the condition as 1 and the ones that do not meet the condition as 0


Now we are going to creat a burned binary raster. We are implementing the formula from:

We are using 
![](https://codimd.carpentries.org/uploads/upload_c3f22cfc22f215d0991724ac1c436f33.png)

From https://custom-scripts.sentinel-hub.com/sentinel-2/burned_area_ms/

To ensure the unit size is matching have a look at the
units for sentinel data documentation: https://docs.sentinel-hub.com/api/latest/data/sentinel-2-l2a/#units

We create a new raster with a specific condition.

```python
burned = (
    (ndvi <= 0.3) &
    (ndvi <= 0.1) &
    ((index_match + nir_clip/10_000) <= 0.1) &
    ((blue_clip/10_000) <=0.1) &
    ((swir16_clip_match/10_000) >= 0.1)
    )
```

Now let us have a look at the outcome raster.

```python
burned.rio.resolution()
burned.plot()
```
Now we need to save this file.

```python
burned.rio.to_raster("burned.tif", dtype="int8")
```

Now we want to confront the image with the visual layer

```python
visual = rioxarray.open_rasterio("data/sentinel2/visual.tif")
```

Let us crop it with the bounding box we created before

```python
visual_clip = visual.rio.clip_box(*bbox)
visual_clip.rio.resolution()
```

Let us have a look a the min and max

```python
visual_clip.min(), visual_clip.max()
```

```python
burned 
```
Has only one dimension

"squeeze" function is used to drop all dimensions that have a single element in it. With squeeze you drop them. Then the useless dimensions are dropped.

```python
burned = burned.squeeze()
```

Now we want to take this visual image and take the red channel and for all the pixels that are burned. Set the maximum value to 0. 

visual_clip has 3 bands. We take the first band and convert all the values 

With "~" you are inverting your mask. We are overwriting the red band in our channel

Where burned is true RGB channel becomes RGB code to (255,0,0) it will turn red.

```python
visual_clip[0, :, :] = visual_clip[0, :, :].where(~burned,255)
visual_clip[1:3, :, :] = visual_clip[1:3, :, :].where(~burned,0)
```

Q: *The band are 1, 2, 3 no?*
A: *true, however accessing the index is 0,1,2 (python specific)*

```python
visual_clip.plot.imshow()
```

Compare it with:

https://www.esa.int/ESA_Multimedia/Images/2023/07/Rhodes_wildfire_forces_thousands_to_flee


### ZONAL statistics

If you don't have the `burned.tif` file, you can download it from [here](https://github.com/esciencecenter-digital-skills/2024-05-13-ds-geospatial/raw/main/files/burned.tif). Same for the [`assets.gpkg`](https://github.com/esciencecenter-digital-skills/2024-05-13-ds-geospatial/raw/main/files/assets.gpkg) file (created yesterday).

We will use it to calculate zonal statistics!

Let's import the relevant libraries to load raster and vector data:
```python=
import rioxarray
import geopandas as gpd
```

Load the binary classification mask:
```python=
burned = rioxarray.open_rasterio("burned.tif")
burned.plot()  # verify
```

Also load all the assets:
```python=
assets = gpd.read_file("assets.gpkg")
assets.explore() # visualize
```

Vector and raster data are in different CRSs!

```python
print(assets.crs, burned.rio.crs)
```

EPSG:4326 and EPSG:32635! We need to reproject:

```python=
assets = assets.to_crs(burned.rio.crs)
```

Let's rasterize the vector data, so that we can overlay it with the raster part. Let's get the geometries with assigned the corresponding code (0 fo 1 for infrastructure, 2 for builtup):

```python=
geom = assets[['geomemtry', 'code']].values.tolist()
```

Drop the band dimension by using `.squeeze()`:

```python=
burned_squeeze = burned.squeeze()
```

Let's rasterize the geometries:
```python=
from rasterio import features
assets_rasterized = features.rasterize(
    geom,
    out_shape=burned_squeeze.shape,
    transform=burned.rio.transform()
)
```

`assets_rasterized` is now a numpy array - not a Xarray object. We need to convert the array to a DataArray object. We create a template using the `burned_squeeze` raster and replace the data:

```python=
assets_rasterized_xarr = burned_squeeze.copy()
assets_rasterized_xarr.data = assets_rasterized
```

We can now visualize it using the usual xarray plotting tools:
```python=
assets_rasterized_xarr.plot()
```

Zonal statistics are calculated using xarray-spatial:

```python=
from xrspatial import zonal_stats

stats = zonal_stats(assets_rasterized_xarr, burned_squeeze)
```

`stats` is now a table that includes statistics computed over the zones (infrastructures, builtup regions, remaining area).

