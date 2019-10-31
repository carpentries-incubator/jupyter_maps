---
title: "Managing Raster layers with ipyleaflet"
teaching: 0
exercises: 0
questions:
- "How to overlay a tile layer with `ipyleaflet`"
- "What is a velocity map?"
- "How to generate Velocity map with `ipyleaflet`?"
- "How to save the resulting map?"
objectives:
- "Understand tile layers with `ipyleaflet`"
- "Understand Velocity maps with `ipyleaflet`"
- "Learn to save `ipyleaflet` maps"
keypoints:
- "raster data in ipyleaflet"
- "tile layers"
- "velocity maps"
---

### Adding a tile layer to an existing map

We will see very little about how to overlay Raster data on an `ipyleaflet` but here is a concrete example with
satellite imagery.

Satellite data can be huge so instead of having one single image or having millions of points, data is split by
*tile*. So depending on how much you zoom, you see more or less details. The picture below explains the process:

<img src="https://www.researchgate.net/profile/Ian_Church/publication/267399124/figure/fig3/AS:295705924128790@1447513194903/Tile-creation-for-Google-Maps-each-tile-was-subsequently-divided-into-four-new-tiles.png" />
**Source**: *Image from [Seamless Online Distribution of Amundsen Multibeam Data](https://www.researchgate.net/publication/267399124_Seamless_Online_Distribution_of_Amundsen_Multibeam_Data/figures?lo=1)*


Let's add an image from [NASA Global Imagery Browse Service (GIBS)](https://earthdata.nasa.gov/eosdis/science-system-description/eosdis-components/gibs):

~~~
from ipyleaflet import basemap_to_tiles

nasa_layer = basemap_to_tiles(basemaps.NASAGIBS.ModisTerraTrueColorCR, "2019-06-24");
map.add_layer(nasa_layer);
map
~~~
{: .language-python}


<iframe width="800" height="400" src="{% link files/simple_map_NASAGIBS.ModisTerraTrueColorCR_20190623.html %}" frameborder="0" allowfullscreen></iframe>

As you can see the new layer is on top of the original map, thus hiding the original basemap.

To remove a layer:
~~~
map.remove_layer(nasa_layer)
~~~
{: .language-python}

To be able to control (remove or change its specification), it is important to store it in a variable (here 
*nasa_layer*).


## Adding layer control

As we are adding or removing layers from Python interface, it would be nice to have the same functionality from
the map itself:

~~~
from ipyleaflet import LayersControl
map.add_control(LayersControl())

~~~
{: .language-python}

<iframe width="800" height="400" src="{% link files/simple_map_NASAGIBS.ModisTerraTrueColorCR_20190623_control.html %}" frameborder="0" allowfullscreen></iframe>

On the top right of our map, we now have the possibility to add/remove our layers.

> ## Exercise
> Add a new raster tile layer on the map; for instance overlay the same kind of imagery but for another date.
>
{: .challenge}

Bear in mind that many "tile" providers require registration and data may not all be freely available. In addition,
you can also create your own tiles and then use `LocalTileLayer` but it is out of scope.

# Velocity map

## Visualize winds with ipyleaflet

Winds (velocity and direction) are usually difficult to represent on a map. `ipyleaflet` provide a simple
way to do it, using `Velocity`. `Velocity` requires both:

- U the zonal velocity, i.e. the component of the horizontal wind towards east;
- V is the meridional velocity, i.e. the component of the horizontal wind towards north

The meteorological convention for winds is that U component is positive for a west to east flow (eastward wind) and the V component is positive for south to north flow (northward wind).

The wind data we will be using for this episode is from [Copernicus Climate Data Store](https://cds.climate.copernicus.eu/#!/home) 
and is freely available (but registration is required).
Data corresponds to [ECMWF re-analysis 5 (ERA5)](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-single-levels?tab=overview) 
daily data for 25th December 2018 at 12 UTC.

> > ## How to retrieve data from Copernicus Data Store
> > - Apart from the registration, you would need to accept
> > the corresponding data license and install [`cdsapi`](https://anaconda.org/conda-forge/cdsapi python
> > package. This package is available in `conda-forge` channel.
> > - Then the corresponding request looks like:
> > 
> > ~~~
> > import cdsapi
> > 
> > c = cdsapi.Client()
> > 
> > c.retrieve(
> >     'reanalysis-era5-single-levels',
> >     {
> >         'product_type':'reanalysis',
> >         'variable':[
> >             '10m_u_component_of_wind','10m_v_component_of_wind'
> >         ],
> >         'year':'2018',
> >         'month':'12',
> >         'day':'25',
> >         'time':'12:00',
> >          'grid': "2.5/2.5",
> >         'format':'netcdf'
> >     },
> >     'uv_ERA5_20181224_1200.nc')
> > 
> > ~~~
> > {: .language-python}
> > 
> >
> > - Dataset were retrieved from [Copernicus Climate Data Store](https://cds.climate.copernicus.eu/#!/home) using the following python code:
> > with `cdsapi` a python package (Climate Data Store API), available from [conda](https://anaconda.org/conda-forge/cdsapi).
> > 
> > **Note**: 
> > - the resolution has been reduced (grid set to 2 x 2 e.g. 2 degrees resolution) to get smaller data files for the workshop. The original resolution of the ERA5 dataset is 0.25 x 0.25 degrees. 
> > - Copernicus Climate data is free but [registration](https://cds.climate.copernicus.eu/user/login?destination=%2F%23!%2Fhome) is requested to download it. 
> > 
> > 
> {: .solution}
{: .challenge}

The raster data we have for winds is in [netCDF](https://www.unidata.ucar.edu/software/netcdf/docs/netcdf_introduction.html) format,
a binary format that is commonly used in environmental sciences such as meteorology, oceanography and climate.
Details on this data format is out of scope.

The main advantage of this fairly standard data format is that we can use `xarray` python package to read it.

## Get Wind data for this episode

And make sure the file [uv_ERA5_25122018_small.nc](https://github.com/annefou/jupyter_maps/raw/master/data/uv_ERA5_25122018_small.nc) is in the data folder. 

~~~
from ipyleaflet import Map, basemaps, Velocity
import xarray as xr

# Open Dataset containing U and V wind components

ds = xr.open_dataset('data/uv_ERA5_25122018_small.nc')

# print metadata

print(ds)
~~~
{: .language-python}

~~~
    <xarray.Dataset>
    Dimensions:  (lat: 72, lon: 144)
    Coordinates:
      * lon      (lon) float32 0.0 2.5 5.0 7.5 10.0 ... 350.0 352.5 355.0 357.5
      * lat      (lat) float32 -88.75 -86.25 -83.75 -81.25 ... 83.75 86.25 88.75
    Data variables:
        time     datetime64[ns] ...
        u10      (lat, lon) float64 ...
        v10      (lat, lon) float64 ...
    Attributes:
        CDI:                       Climate Data Interface version 1.9.3 (http://m...
        Conventions:               CF-1.6
        history:                   Fri Jun 21 13:35:05 2019: ncwa -d time,0 -a ti...
        CDO:                       Climate Data Operators version 1.9.3 (http://m...
        NCO:                       netCDF Operators version 4.7.5 (Homepage = htt...
        nco_openmp_thread_number:  1

~~~
{: .output}

As all raster data, it is gridded data and here we have a regular latitude-longitude grid with 72 latitudes
and 144 latitudes. The two variables are:
- u10
- v10

These are the variables we will pass to `ipyleaflet` to plot winds.

> ## Note
> Why U10 and V10?
> We never get winds exactly at the surface but close to the surface of the Earth and by convention, we 
> use 10 metre winds (10 metres above the surface).
>
{: .callout}

Let's create our velocity map:

~~~
#  Define a map centred over Oslo (Latitude: 59.9127 Longitude: 10.7461)

# First is latitude and second is longitude; both in degrees
center = (59.9127, 10.7461)
zoom = 4

map = Map(center=center, zoom=zoom, interpolation='nearest', basemap=basemaps.Esri.WorldImagery)

# Add winds to the map

wind = Velocity(data=ds,
                zonal_speed='u10',
                meridional_speed='v10',
                latitude_dimension='lat',
                longitude_dimension='lon',
                velocity_scale=0.01,
                max_velocity=20)
map.add_layer(wind)

map
~~~
{: .language-python}

<iframe width="800" height="400" src="{% link files/uv_ERA5_25122018_small.html %}" frameborder="0" allowfullscreen></iframe>


# Save to HTML


~~~
from ipywidgets.embed import embed_minimal_html

embed_minimal_html('uv_ERA5_25122018_small.html', views=[map], title='Velocity map ERA5, 25 December 2018 12:00')
~~~
{: .language-python}


> ## Exercise:
>
> - Overlay NASA GIBS imagery from the same date (NASAGIBS.ModisTerraTrueColorCR)
>
> > ## Solution
> > ~~~
> > from ipyleaflet import basemap_to_tiles
> >
> > nasa_layer = basemap_to_tiles(basemaps.NASAGIBS.ModisTerraTrueColorCR, "2018-12-25");
> > map.add_layer(nasa_layer);
> > ~~~
> {: .solution}
{: .language-python}

# Split your map

It may be useful to get two different maps (instead of adding layers one on top of the other) and the
possibility to control (zoom in/out) the two maps together:

~~~
from ipyleaflet import Map, basemaps, basemap_to_tiles, SplitMapControl
center = (59.9127, 10.7461)
zoom = 4

map = Map(center=center, zoom=zoom, interpolation='nearest', basemap= basemaps.CartoDB.DarkMatter)

right_layer = basemap_to_tiles(basemaps.NASAGIBS.ModisTerraTrueColorCR, "2018-12-25")

control = SplitMapControl(left_layer=wind, right_layer=right_layer)
map.add_control(control)

map
~~~
{: .language-python}


{% include links.md %}

