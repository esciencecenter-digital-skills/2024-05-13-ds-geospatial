![](https://i.imgur.com/iywjz8s.png)


# Day 4 - Collaborative Document

2024-05-16 Introduction to Geospatial Raster and Vector Data with Python

Welcome to The Workshop Collaborative Document.

This Document is synchronized as you type, so that everyone viewing this page sees the same text. This allows you to collaborate seamlessly on documents.

----------------------------------------------------------------------------

This is the Document for today: [link](https://tinyurl.com/2024-05-16-geospatial-python)

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


## 👩‍🏫👩‍🙋💻🎓 Instructors & Helpers

Maurice de Kleijn, Francesco Nattino, Ou Ku

## 🗓️ Agenda

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
| 13:00 | **END of Day 4 WORKSHOP** |


## 🎓 Certificate of attendance
If you attend the full workshop you can request a certificate of attendance by emailing to training@esciencecenter.nl .

## 🔧 Exercises

### Exercise 1: searching satellite scenes with a time filter

Add a time filter to the search in order to select the only scenes recorded between 1 July and 31 August 2023. You can find the input argument and the required syntax in the documentation of `client.search` (which you can access from Python or [online](https://pystac-client.readthedocs.io/en/stable/api.html#pystac_client.Client.search)). How many scenes do now match our search?


### Solution
We want to add a daytime filter. pystac documentation is not perfect. Can be a bit messy. Community behind it is very big. 

So we use the client search and add a datetime
```python
client.search(
    collections=[collection],
    intersects=point,
    datetime="2023-07-01/2023-08-31"
    )
```


```python
search.matched()

```

Result should be "12".



### Exercise 2: searching satellite scenes using metadata filters

Let's add a filter on the cloud cover to select the only scenes with less than 1% cloud coverage. How many scenes do now match our search?

Hint: generic metadata filters can be implemented via the `query` input argument of `client.search`, which requires the following syntax (see [docs](https://pystac-client.readthedocs.io/en/stable/usage.html#query-extension)): `query=['<property><operator><value>']`.


### Solution
We do the same as above, but now include the cloud coverage: 

```python
client.search(
    collections=[collection],
    intersects=point,
    datetime="2023-07-01/2023-08-31"
    query=["eo:cloud_cover<1]"#is a list, since you can add multiple queries
) 
```


```python
search.matched()

```
11 

Rhodes in summer had little clouds.


## 🧠 Collaborative Notes


## Access data 

Getting Sentinel 2 images.Copernicus ESA program

Data is often provided in 2 ways. 

Main access point is Copernics dataspace: https://dataspace.copernicus.eu/

Relatively intuitive to get your data.

A login is required, but the registration is for free. Slow and error prone. If you have a pipeline you can get data based on a script using the API.

https://dataspace.copernicus.eu/analyse/apis

The main challenge is different API´s for different data providers. Every API can is different. 

This was the motivation for some people from the data providers perspective and user community to develop catalogue  to load data from these platforms. 

STAC: https://stacspec.org/en

Datasets: https://stacspec.org/en/about/datasets/

We will look at Earth Search catalogue
https://stacindex.org/catalogs/earth-search
Data hosted at the Amazon Cloud as open data.

We will be looking at Sentinel2 level 2a data. 

https://stacindex.org/catalogs/earth-search#/43bjKKcJQfxYaT1ir3Ep6uENfjEoQrjkzhd2?t=3

In contians scenes and links to assets from a scene. 

We will interact with this catalogue with python.

First we need to get the link to the api. 



```python
api_url = "https://earth-search.aws.element84.com/v1/"
```

We use the stac library for python "pystac". pystac_client is specifically for accessing the stac client.

```python
import pystac_client
```

create the client object

```python
client = pystac_client.Client.open(api_url)
```
Let us print it.

```python
client
```
We are going to have a look a the data from the api

```python
client.search?

```

As said we are focussing at Sentinel 2 level 2a collection. We specify that as follows.

```python
list(client.get_collections())

```
We want to focus at sentinel 2 l2a
```python
collection = "sentinel-2-l2a"
```
We want to specify a geometry point to get data from a specific location. To do so we use shapely

```python
from shapely import Point
point = Point(27.95, 36.20) # lon lat
```
Now we use this point to search images for this specific location. We put this in our search.

```python
client.search(
    collections=[collection],
    intersects=point
)

```

You can also include more complex geometries, but be aware that there is probably a limit to the service

To look at how many match the search you can use the matched function

```python
search.matched()
```
The outcome (in our case 619, which can change since it is dynamic) represents the number of scenes (which all have multiple assets, like red, nir, visual etc.)

An alternative browser. 
https://radiantearth.github.io/stac-browser/#/

Exercise

From the [exercise](https://codimd.carpentries.org/4eP3B6M3RBKMIHopaJXX-g?both#%F0%9F%94%A7-Exercises)

```python
client.search(
    collections=[collection],
    intersects=point,
    datetime="2023-07-01/2023-08-31"
    )
```

We now access the item collection from our search.

```python
items= search.item_collection()
```

to list them you can see the various scenes


```python
list(items)
```
To get the first from the list.

```python
item = items[0]
```

To get the datetime you can do. There are all kind of ways to print information about this item.

```python
item.datetime 
```


```python
item.properties
```
To get all kind of information. eo: cloud_cover tells something about the cloud coverage. The projection is often standardized through the EPSG codes.

Very often the cloud coverage is a condition. 

```python
client.search(
    collections=[collection],
    intersects=point,
    datetime="2023-07-01/2023-08-31"
    query=["eo:cloud_cover<1]"#is a list, since you can add multiple queries
) 
```

We can save this image search as a json file, this allows you to reuse it in a different context.


```python
items.save_object("rhodes_sentinel-2.json")
```
If you want to load the file you will need to use the pystac library.


```python
import pystac
```

```python
_ = pystac.ItemCollection.from_file("rhodes_sentinel-2.json")
```
Let us now see how we can access different assets from the scene (item). 

Let is select the first one in the item list (which is the most recent image)
```python
item = items[0] 
```

Explore it you will notice it is an dictionary

```python
item.assets # is actually a dictionary
```


```python
item.assets.keys() # are links to real data. 
```

Let is look at the thumbnail for instance

```python
item.assets["thumbnail"].href # are links to real data. 
```
You get a clickable link which shows the thumbnail.


To get the oldest image in the list.

```python
items[-1].assets["thumbnail"].href # are links to real data. 
```
Get the link to a specific file

```python
red_href = item.assets["red"].href
```

To open that. We do exactly the same as yesterday

```python
import rioxarray
```

```python
red =rioxarray.open_rasterio(red_href)
```
In case you do a clip Rioxarray will only look at the clipped part.


## Parallel Computation 

We create a new notebook. We will be using Dask for this. 

 - Less memory usage
 - Speed up the process

https://www.dask.org/

For parallel compution but also for multiple machines. 

We will look at dask arrays.

![](https://codimd.carpentries.org/uploads/upload_cc072e0294468012195631da244e593e.png)

Let us do this on the one red band we downloaded locally. (we can also use the STAC api which we di above)

```python
red =rioxarray.open_rasterio("data/sentinel2/red.tif", chunks={"x": 4000, "y":4000})

```
Let see how that looks like
```python
red
```
![](https://codimd.carpentries.org/uploads/upload_0e724d5dc6b5921f0c507f511a64b4ad.png)

You will see we have 9 chunks. You will see 9 blocks in the image, which correspond with what we did.

What would be a good chunk size?

1 Let us check the block size of the COG.
For this we can check the gdal information 

! --> not python but command line

```python
!gdalinfo data/sentinel2/red.tif

```
You can see the Blocks for the cloud optimalized geotifs. (which are blocked themselves) It would make sense to use these to to chuck your raster file.

2 Rule of thumb is ~100mb of the chuck.


```python
6*1024
```
So: 

```python
red =rioxarray.open_rasterio("data/sentinel2/red.tif", chunks={"x": 6144, "y":6144})
```
provides 72 mb chunks


You can also allow dask to do this on its own. 
```python
red =rioxarray.open_rasterio("data/sentinel2/red.tif", chunks="auto"})
```

But let us stick with the 6144

```python
red =rioxarray.open_rasterio("data/sentinel2/red.tif", chunks={"x": 6144, "y":6144})
```


Let us first do this in serial.

```python
red =rioxarray.open_rasterio("data/sentinel2/red.tif", masked = True)
nir =rioxarray.open_rasterio("data/sentinel2/nir.tif", masked = True)
```

```python
%%time #times the time of execution
ndvi = (nir - red) / (nir + red)
```

It took 9 seconds on Francesco´s machine.

Now let us do this in parallel.

```python
red_dask =rioxarray.open_rasterio("data/sentinel2/red.tif", masked = True, chunks={"x": 6144, "y":6144}, lock=False)
nir_dask =rioxarray.open_rasterio("data/sentinel2/nir.tif", masked = True, chunks={"x": 6144, "y":6144}, lock=False)
```
Let us now do the ndvi calculation as we did before

```python
%%time 
ndvi_dask = (nir_dask - red_dask) / (nir_dask + red_dask)
```
Result is 233 ms. Is is really possible to go from 9 seconds to 233 ms? Not really probably. It did not actually performs them. It set them up.

The full array size actually increases when chunking it.

So now we actually want to run it. So first let us have a look at the Dask Graph to see what is happening. We do that by importing dask and create an overview on the various processes that are scheduled.


```python
import dask
dask.visualize(ndvi_dask)
```

Now we actually want to run the calculation. How do we do that?

There are 2 ways of triggering the calculation.

.persist() and .compute()


```python
ndvi_dask.persist()
```
You do as intermediate step

```python
ndvi_dask.compute()
```
You do if you are running it at the end.
On a local machine this will not have much effect. On servers it will.


```python
%%time
ndvi_dask.persist(scheduler="threads", num_workers=4)
```
3.26 on Francesco´s machine.
serial time /parallel time
9.38/3.26

Speedup of 2.8 

If you want to know more about parallel python have a look here:

https://www.esciencecenter.nl/digital-skills/

Here is the material for the parallel workshop:

https://carpentries-incubator.github.io/lesson-parallel-python/


```python
%%time
ndvi_dask_persisted = ndvi_dask.persist(scheduler="threads", num_workers=4)
```

You can save the result in a raster.

```python
import threading import Lock
ndvi_dask_persisted.rio.ro_raster("ndvi.tif", tiled=True, lock= Lock())
```

We are locking it since we are creating a file which uses multiple threads. We have 4 things that are writing to the same file. That is important, to avoid that they are overwriting each other. 



In this last part: we will be focussing on data cubes

## Data cubes
First we need to install the (Open Data Cube) ODC stac library

```python
!pip installl odc-stac
```

We are going to use the metadata without using the actual data to construct the framework of our datacube.

```python
import odc.stac
```
Let us define the area of interest. We use the rhodes.gpkg
You know how at this moment ;-)

```python
import geopandas as gpd
rhodes = gpd.read_file("rhodes_130524.gpkg")
bbox = rhodes.total_bounds
bbox
```

Access to the api


```python
api_url = "https://earth-search.aws.element84.com/v1"
```

import pystac
```python
import pystac_client
```
define the client

```python
client = pystac_client.Client.open(api_url)
```


```python
collection = "setinel-2-l2a"
```

Q: how would you know which one to select?
A: Exploring, Francesco knew by asking to the developers (who are very active!). 


```python
client.search(
    collections=[collection],
    datetime="2023-07-01/2023-08-31",
    bbox= bbox,
)

```

```python
search.matched()
```
73 is much higher, since we are not focussed on a point, but now a whole bounding box.

```python
items = search.item_collection()
```


We take the metadata and construct a datacube, with links to the data.
We want to tile images that have same condition (solar day in this case)

```python
ds = odc.stac.load(
    items,
    groupby="solar_day",
    chunks ={"x":2048, "y": 2028},
    resolution= 20, #the bands have different resolutions, therefore we assign 20 as cell size since that is the highest cell size
    use_overviews = True,
    bbox = bbox,   
)
```
It is acually pretty fast, since it fully works on the metadata and creates a mosaiced data array


```python
ds
```

All the bands have a different attribute. This structure comes from the NetCDF files. You can save it as that format as well

```python
ds.nbytes / 2**30
```
It is actually 12 GB, but we have done this lazy, so none has been loaded yet. 

```python
ds["red"]
```
![](https://codimd.carpentries.org/uploads/upload_c2583e3f9d91d2060072ee4b736381f0.png)

scene classification 

https://custom-scripts.sentinel-hub.com/custom-scripts/sentinel-2/scene-classification/

Let us distinguish 
![](https://codimd.carpentries.org/uploads/upload_266221857523fe12ada89112dc5c8884.png)

3,6,8,9 and 10

Let us first take out the layers that we want

```python
red = ds["red"]
nir = ds["nir"]
scl = ds["scl"]
```

For the scl select the values 3,6,8,9 and 10 


```python
mask = scl.isin([3,6,8,9,10])
red_masked = red.where(~mask)
nir_masked = nir.where(~mask)
```
Now we do the NDVI again.

```python
ndvi = (nir_masked - red_masked)/ (nir_masked + red_masked)
```

Through this you can let the calculations perform on only that day. 

Let us select a date before the fire
```python
ndvi.sel(time="2023-07-13").plot()
```
Now let us look at the period after the fire.


```python
ndvi.sel(time="2023-08-27").plot()
```

We can also do this for a different area.

```python
x = 585_000
y = 3_995_000

```

```python
ndvi_xy = ndvi.sel(x=x, y=y, method= "nearest")
```

Now we compute this quantity

```python
%%time
ndvi_xy = ndvi_xy.compute(scheduler="threads", num_workers=4)
```
Q: what happens when a scene is next to the UTM zone?
A: you can set that as parameters


```python
ndvi_xy.dropna(dim="time").plot()
```
You can see for that point the NDVI drop.




