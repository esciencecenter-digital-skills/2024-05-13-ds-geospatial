![](https://i.imgur.com/iywjz8s.png)


# Day 1 - Collaborative Document

2024-05-13 Introduction to Geospatial Raster and Vector Data with Python

Welcome to The Workshop Collaborative Document.

This Document is synchronized as you type, so that everyone viewing this page sees the same text. This allows you to collaborate seamlessly on documents.

----------------------------------------------------------------------------

This is the Document for today: [link](https://tinyurl.com/2024-05-13-geospatial-python)

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

Zoom link: [link](https://us02web.zoom.us/j/82696360872?pwd=SmJaMFdxcE83UUNGL3dMRHl2QWZMUT09)

## 👩‍🏫👩‍🙋💻🎓 Instructors & Helpers

Maurice de Kleijn, Francesco Nattino, Ou Ku


## 🗓️ Agenda
**Day 1**
 
| Time  | Topic                                   | Instructors             |
| ----- | --------------------------------------- | ----------------------- |
| 09:00 | Welcome and icebreaker, setup check     | Maurice - Francesco- Ou |
| 09:15 | Introduction to raster, vector, and CRS | Maurice - Francesco     |
| 10:15 | **Coffee break**                        |                         |
| 10:25 | Read and visualize raster data          | Francesco - Maurice     |
| 11:30 | **Coffee break**                        |                         |
| 11:45 | Vector data in Python                   | Maurice - Francesco       |
| 12:50 | Wrap up & feedback                      | Maurice - Francesco     |
| 13:00 | **END of Day 1**                        |                         |
 
**Day 2**

| Time  | Topic                                         | Instructors  |
| ----- | --------------------------------------------- | ------------ |
| 09:00 | Welcome and icebreaker                        | Maurice - Ou |
| 09:10 | Vector data in Python                         | Maurice - Ou |
| 10:15 | **Coffee break**                              |              |
| 10:25 | Crop raster data with Rioxarray and geopandas | Maurice - Ou |
| 11:30 | **Coffee break**                              |              |
| 11:40 | Crop raster data with Rioxarray and geopandas | Maurice - Ou |
| 12:50 | Wrap up & feedback                            | Maurice - Ou |
| 13:00 | **END of Day 2**                              |              |

**Day 3**

| Time  | Topic                                   | Instructors         |
| ----- | --------------------------------------- | ------------------- |
| 09:00 | Welcome and icebreaker                  | Maurice - Francesco |
| 09:10 | Raster calculations in python           | Francesco - Maurice |
| 10:15 | **Coffee break**                        |                     |
| 10:25 | Raster calculations in python           | Francesco - Maurice |
| 11:30 | **Coffee break**                        |                     |
| 11:40 | Calculating Zonal Statistics on Rasters | Maurice - Francesco |
| 12:50 | Wrap up & feedback                      | Maurice - Francesco |
| 13:00 | **END of Day 3**                        |                     |


**Day 4**

| Time  | Topic                                   | Instructors         |
| ----- | --------------------------------------- | ------------------- |
| 09:00 | Welcome and icebreaker                  | Maurice - Francesco |
| 09:10 | Access satellite imagery using Python   | Francesco - Maurice |
| 10:15 | **Coffee break**                        |                     |
| 10:25 | Parallel raster Computations using Dask | Francesco - Maurice |
| 11:30 | **Coffee break**                        |                     |
| 11:40 | Parallel raster Computations using Dask | Francesco - Maurice |
| 12:50 | Post-workshop survey                    | Maurice - Francesco |
| 13:00 | **END of Day 4 & WORKSHOP**             |                     |


## 🎓 Certificate of attendance
If you attend the full workshop you can request a certificate of attendance by emailing to training@esciencecenter.nl .

## 🔧 Exercises

## 🧠 Collaborative Notes




In this workshop, we will have a wilf fire event happend in Rhodes Island as a usecase. This event happended in the summer of 2023.

Geodata covered in this workshop:
- **vector data**, typically used for discrete variables (buildings, roads, etc.). Includes features associated to a geometry (a point, a line, or a polygon)
- **raster data**, typically used for continuous variables (temperature, elevation, etc.). Values assigned to 'pixels' (or grid cells).


Try importing the following packages to test your Python environment is working:

```python
import rioxarray
import geopandas
import pystac
```

Trouble shooting setting up the Python env
```py
python -m venv venv
source venv/bin/activate
```

```bash
pip install jupyterlab geopandas rioxarray  
```


# Introduction to Raster data for python

Give your notebook a name that makes sense. In this case raster_intro would make sense


# Day 1 - Part 1 Introduction Raster data - loading data

```python
import rioxarray

```

Rioxarray is a powerfull library it exists of 2 other other libraries
 - [Rasterio](https://rasterio.readthedocs.io/en/stable/#) - Under the hood it uses GDAL
 - [Xarray](https://docs.xarray.dev/en/stable/#)
   
To see which input arguments you use with a description on what the function does

```python
rioxarray.open_rasterio? 
```

Now we are going to open the red.tif file. All the bands that have been taken from a satellite scene. 13 bands are provided. We will open one. Let us first make a variable to the file we want to load in our case *red.tif* (which is useful in our analysis or analysisin scorched areas) 

```python
rhodes_red_path = "data/sentinel2/red.tif"
```


Now we use this to load the file.
```python
rhodes_red = rioxarray.open_rasterio(rhodes_red_path)
```


To explore the data:

```python
rhodes_red
```

You can explore the number of bands, the range of the values and information about the crs etc. 

Let´s see what you can do with this object. Pressing tab (in jupyter lab) will allow you to explore the options.

![](https://codimd.carpentries.org/uploads/upload_11503384bb4322b2dc4c75d28d418a06.png)


```python
rhodes_red.rio.crs
```

Will return the[epsg code](https://spatialreference.org/ref/epsg/) of the object. 

```python
rhodes_red.rio.nodata
```

To see which values are used for nodata (with data this is 0).

```python
rhodes_red.rio.bounds()
```

Will generate show the outer coordinates of the dataset

The data itself is not loaded in the memory. It was relatively fast.

You can load the values as an array. They are part of the xarray data.

```python
rhodes_red.values
```

You can also visualize the data. A method of xarray not rio specific.

```python
rhodes_red.plot()
```

It took some time to plot the image. 


For performance purposes we load a lower resolution of the file. 
These datasets are stored as cloud optimalized geotifs (COG) format. Sometimes you do not need the whole area of the image. The file contains multiple zoom images of the data. 

Therefore we are going to load only the part that we want in a lower resolution. 

So let us open the same file again.

Overview of resolution levels

0, 1st , 20x20 
1, 2st , 40x40
2, 3st , 80x80
etc. 

Let us go for the 2

```python
rhodes_red_80 = rioxarray.open_rasterio(rhodes_red_path, overview_level=2)
```

Explore the data:

```python
rhodes_red
```
You will see a different number of cells.


![](https://codimd.carpentries.org/uploads/upload_ed18c254ec982e295c40ebfb60eb43db.png)

You can also directly call the resolution

The resolution of the first one.
```python
rhodes_red.rio.resoltion() #Eddo: spelling
```

The resolution of the otehr one.
```python
rhodes_red_80.rio.resoltion() #Eddo: spelling
```

In plot function you can add an optional option to exclude outliers.

```python
rhodes_red_80.plot(robust=True)
```

We now have an idea how to load the data and how to visualize it. 

# Coordinate Reference System

Now we want to understand something about the coordinate reference systems.


To see the CRS of our file.

```python
rhodes_red_80.rio.crs
```
CRS.from_epsg(32635)


pyproj is the library to use to work with CRS-s


```python
import pyproj
```

Assign epsg code to a variable:
```python
epsg_code = rhodes_red_80.rio.crs.to_epsg()
```

```python
crs = pyproj.CRS(epsg_code)
```

To read it. 

Well known text format is a standardized way to define the coordinate system.  

```python
crs.to_wkt()
```

Let us open the raster and calculate some statistics.

The minimum values
```python
rhodes_red_80.min()
```

The maximum values
```python
rhodes_red_80.max()
```

We can specific also the dimension where we want to caculate the max for.
```python
rhodes_red_80.max(dim='x')
```

To get the mean value or standard deviation etc.

```python
rhodes_red_80.std()
```

```python
rhodes_red_80.mean()
```

etc.


To understand which values are used as nodata you can access them as follows.

```python
rhodes_red_80.rio.nodata()
```

```output
0
```
We are now going to convert the no data values into nan, otherwise the statistics makes no sense (since it has 0 for nodata)

```python
rhodes_red_80_mask= rioxarray.open_rasterio(rhodes_red_path, overview_level=2, masked= True)
```
See the difference between rhodes_red_80_mask and rhodes_red_80
![](https://codimd.carpentries.org/uploads/upload_a791b54e17d1019b462ab88a83e24444.png)

Now the statistics will be different, since the 0 values are not included in the calculation anymore.

You will notice that once it is masked you will get foating point datatype in order to fit the nan values (which cannot be represented as integers). Be aware of this when you calculate data. 

![](https://codimd.carpentries.org/uploads/upload_66370644615d06152a36085eb773b611.png)

# Let us now focus on the Multiband raster files. 

RGB bands. 


```python
visual_path = "data/sentinel2/visual.tif"

```
Let us load the data again:

```python
visual= rioxarray.open_rasterio(visual_path, overview_level=2)
```


```python
visual.plot()
```

Will not generate an image, it will generate a histogram since it does not know what it needs to do. 

We therefore help it by using the imshow method. This shows the image in RGB values of the dataset.

```python
visual.plot.imshow()
```

Q1: How do you select only one band. that is done through the sel method to select label based indices.

```python
visual.sel(band=1)
```

```python
visual.sel(x=500_000, method="nearest")
```

Q2: How do I create nice plots?

[hvplot](https://hvplot.holoviz.org/) is suggested by Francesco as suggestion. 




Tomorrow we'll continue with the same notebook!


### Resources
 - Check what are the actual sizes of the countries: [link](https://www.thetruesize.com/)

