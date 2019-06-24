---
title: "Managing vector data with `ipyleaflet`"
teaching: 0
exercises: 0
questions:
- "How to add Markers to your map?"
- "What kind of Markers could I use?"
- "What is a Choropleth map?"
- "How to generate Choropleth map with `ipyleaflet`?"
objectives:
- "Understand how to add markers, circle on an `ipyleaflet` map"
- "Understand Chorophleth maps with `ipyleaflet`"
keypoints:
- "vector data in ipyleaflet"
- "markers, circles, choropleth maps"
---

# Add Markers, circles to an `ipyleaflet` map

Let's take a concrete example. We would like to reproduce the map of all Carpentries instructors.

<script  src="https://embed.github.com/view/geojson/carpentries/carpentries.org/gh-pages/files/geojson/all_instructors_by_airport.geojson"></script>

**Source**: [https://github.com/carpentries/carpentries.org/gh-pages/files/geojson/all_instructors_by_airport.geojson](https://github.com/carpentries/carpentries.org/gh-pages/files/geojson/all_instructors_by_airport.geojson)

We briefly mentioned it at the very beginning of the lesson but Github renders geojson file directly. However,
here we wish to cutomize this plot, so we will first learn how to add markers in `ipyleaflet`.

## View a map of all Carpentries instructors

- using https://raw.githubusercontent.com/carpentries/carpentries.org/gh-pages/files/geojson/all_instructors_by_airport.geojson


Let's first create a map, centred on Manchester (UK).

~~~
from ipyleaflet import Map, basemaps


# Define map centred on Manchester for CarpentryConnect 2019


# First is latitude and second is longitude; both in degrees
center = (53.483959, -2.244644)

map = Map(center=center, interpolation='nearest', basemap=basemaps.Stamen.Terrain)
~~~
{: .language-python}

## Download `geojson` file from Carpentries website


~~~
import urllib.request

url = 'https://raw.githubusercontent.com/carpentries/carpentries.org/gh-pages/files/geojson/all_instructors_by_airport.geojson'

# Download the file from `url` and save it locally under `data/all_instructors_by_airport.json`:
urllib.request.urlretrieve(url, 'all_instructors_by_airport.geojson')

~~~
{: .language-python}


## Read GeoJson file

The purpose of this episode is not to learn about geojson data format but we still need to be able to
read the data file to add it into our map. For this, we will be using another python package called `json`.
It can be installed from Anaconda conda-forge channel.


~~~
import json
# Open GeoJson file and load data
with open('all_instructors_by_airport.geojson') as f:
    geojson = json.load(f)
~~~
{: .language-python}

## Visualize geojson as a table

Let's have a look at our dataset:

~~~
from pandas.io.json import json_normalize

features = geojson['features']
json_normalize(features)
~~~
{: .language-python}

To use the function `json_normalize`, `pandas` python package is required. It should be available by default,
and is available from Anaconda conda-forge channel.


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>geometry.coordinates</th>
      <th>geometry.type</th>
      <th>properties.details</th>
      <th>properties.marker-color</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>[10.6190004, 56.2999992]</td>
      <td>Point</td>
      <td>[BAB, LWJ, AS, KT]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>1</th>
      <td>[-106.616704, 35.048665]</td>
      <td>Point</td>
      <td>[KB, AF, JMG, CJ, NM, TR, MS, JW]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>2</th>
      <td>[38.7993011, 8.97789]</td>
      <td>Point</td>
      <td>[YA, MDC, YAE, DYM]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>3</th>
      <td>[-2.8684399, 56.3728981]</td>
      <td>Point</td>
      <td>[IB, AK, PM, MT]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>4</th>
      <td>[174.791667, -37.008056]</td>
      <td>Point</td>
      <td>[VF, EJ, NJ, CMJT, CM, SS]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>5</th>
      <td>[-73.8016968, 42.7482986]</td>
      <td>Point</td>
      <td>[DT]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>6</th>
      <td>[4.767438, 52.309686]</td>
      <td>Point</td>
      <td>[KdH, MG, JH, VK, MK, PL, CMO, AS, RS]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>7</th>
      <td>[17.918611, 59.651944]</td>
      <td>Point</td>
      <td>[LDS, HG, WNkerstrm, OV, RVG, TW]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>8</th>
      <td>[-97.669722, 30.194444]</td>
      <td>Point</td>
      <td>[AD, VP, MS, MS, RT]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>9</th>
      <td>[2.078333, 41.296944]</td>
      <td>Point</td>
      <td>[AOC]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>10</th>
      <td>[-72.683056, 41.938889]</td>
      <td>Point</td>
      <td>[SBA, CD, JD, SK, AL, JM, TEM, SN, PN, SKO, RP...</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>11</th>
      <td>[26.3024006, -29.0926991]</td>
      <td>Point</td>
      <td>[FR]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>12</th>
      <td>[-68.8281021, 44.8073997]</td>
      <td>Point</td>
      <td>[SM]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>13</th>
      <td>[-86.753333, 33.562778]</td>
      <td>Point</td>
      <td>[VPL]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>14</th>
      <td>[-1.747778, 52.453611]</td>
      <td>Point</td>
      <td>[SB, MB, MH, AJ, SM, CS, LDW]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>15</th>
      <td>[-101.479347, 20.990772]</td>
      <td>Point</td>
      <td>[NS]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>16</th>
      <td>[11.289168, 44.536189]</td>
      <td>Point</td>
      <td>[GP]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>17</th>
      <td>[-86.66945, 36.131312]</td>
      <td>Point</td>
      <td>[SB, CF, JT]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>18</th>
      <td>[153.1175, -27.384167]</td>
      <td>Point</td>
      <td>[IB, MF, FG, SG, NH, PAM, AM, BM, KP, BW]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>19</th>
      <td>[-116.2229996, 43.5643997]</td>
      <td>Point</td>
      <td>[MC, MH, EJ]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>20</th>
      <td>[72.867897, 19.0886993]</td>
      <td>Point</td>
      <td>[SS]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>21</th>
      <td>[-71.005, 42.364167]</td>
      <td>Point</td>
      <td>[TB, DB, MF, JG, IG, KL, YL, EM, CM, MP, PR, S...</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>22</th>
      <td>[-2.718889, 51.3825]</td>
      <td>Point</td>
      <td>[CE, EH, DM, JM, DRB]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>23</th>
      <td>[4.487055, 50.902447]</td>
      <td>Point</td>
      <td>[LG, TOG, SVH]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>24</th>
      <td>[7.530066, 47.595156]</td>
      <td>Point</td>
      <td>[BB, GF]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>25</th>
      <td>[-73.1532974, 44.4719009]</td>
      <td>Point</td>
      <td>[SGC]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>26</th>
      <td>[-78.7322006, 42.9404984]</td>
      <td>Point</td>
      <td>[BTT]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>27</th>
      <td>[-118.3590012, 34.2006989]</td>
      <td>Point</td>
      <td>[GC, LS]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>28</th>
      <td>[-76.668333, 39.175278]</td>
      <td>Point</td>
      <td>[DK, PLL, DS]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>29</th>
      <td>[-111.1529999, 45.7775002]</td>
      <td>Point</td>
      <td>[SM, AST]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>163</th>
      <td>[-76.106111, 43.111111]</td>
      <td>Point</td>
      <td>[EM]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>164</th>
      <td>[-84.34527, 30.39537]</td>
      <td>Point</td>
      <td>[JK, DP]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>165</th>
      <td>[24.80074, 59.416582]</td>
      <td>Point</td>
      <td>[DF, LK, ES]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>166</th>
      <td>[34.876667, 32.009444]</td>
      <td>Point</td>
      <td>[BG]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>167</th>
      <td>[18.916328, 69.681079]</td>
      <td>Point</td>
      <td>[RB]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>168</th>
      <td>[-82.536413, 27.983099]</td>
      <td>Point</td>
      <td>[JM, KR]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>169</th>
      <td>[13.466389, 45.827778]</td>
      <td>Point</td>
      <td>[SC]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>170</th>
      <td>[146.766907, -19.248454]</td>
      <td>Point</td>
      <td>[DB]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>171</th>
      <td>[-110.941389, 32.116111]</td>
      <td>Point</td>
      <td>[GA, UKD, BJ, JO, TLS]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>172</th>
      <td>[13.2876997, 52.5597]</td>
      <td>Point</td>
      <td>[CCB, SC, TZ]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>173</th>
      <td>[-84, 35.81]</td>
      <td>Point</td>
      <td>[DEB, AS]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>174</th>
      <td>[25.28217, 54.640024]</td>
      <td>Point</td>
      <td>[MRC]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>175</th>
      <td>[20.9671001, 52.165699]</td>
      <td>Point</td>
      <td>[ZJS, AP]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>176</th>
      <td>[174.808049, -41.328932]</td>
      <td>Point</td>
      <td>[JdL, WH, JW]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>177</th>
      <td>[-117.636366, 49.296744]</td>
      <td>Point</td>
      <td>[JH]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>178</th>
      <td>[-113.579722, 53.309722]</td>
      <td>Point</td>
      <td>[CM, JS, AS]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>179</th>
      <td>[-66.527212, 45.871104]</td>
      <td>Point</td>
      <td>[JB]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>180</th>
      <td>[-76.595936, 44.226524]</td>
      <td>Point</td>
      <td>[GL]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>181</th>
      <td>[-63.508611, 44.880833]</td>
      <td>Point</td>
      <td>[RMD, RD]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>182</th>
      <td>[-75.669167, 45.3225]</td>
      <td>Point</td>
      <td>[CA, RC, KC, CM]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>183</th>
      <td>[-71.3975, 46.788333]</td>
      <td>Point</td>
      <td>[MB]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>184</th>
      <td>[-82.9448, 42.2749]</td>
      <td>Point</td>
      <td>[MW]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>185</th>
      <td>[-73.741389, 45.468056]</td>
      <td>Point</td>
      <td>[GAD, JG, DH, MJ, SR, PLSO]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>186</th>
      <td>[-123.181944, 49.195]</td>
      <td>Point</td>
      <td>[MHB, DL, MR, AR, YT, TT, LJW]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>187</th>
      <td>[-81.153889, 43.035556]</td>
      <td>Point</td>
      <td>[PB]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>188</th>
      <td>[-114.012405, 51.128778]</td>
      <td>Point</td>
      <td>[PP, AS]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>189</th>
      <td>[-123.425833, 48.646944]</td>
      <td>Point</td>
      <td>[AT]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>190</th>
      <td>[-52.8124826, 47.6211888]</td>
      <td>Point</td>
      <td>[IAA, OC, EG, DQ, OS]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>191</th>
      <td>[-79.630556, 43.677222]</td>
      <td>Point</td>
      <td>[SA, BB, MBF, BC, SEC, KC, HG, JG, TG, ARH, DH...</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
    <tr>
      <th>192</th>
      <td>[8.560132, 47.457767]</td>
      <td>Point</td>
      <td>[LD, SH, DLT, BMM, FP, SP, MR, FT, TT, LW]</td>
      <td>#2b3990</td>
      <td>Feature</td>
    </tr>
  </tbody>
</table>
<p>193 rows × 5 columns</p>
</div>



##  Add our json data as a new layer to the map

`ipyleaflet` (as many similar interactive map packages) can handle geojson data format using `GeoJSON`:


~~~
from ipyleaflet import GeoJSON

geo = GeoJSON(data=geojson)
map.add_layer(geo)
~~~
{: .language-python}

## Save map as HTML

As before with raster layers, we can save the resulting map in an HTML file:

~~~
from ipywidgets.embed import embed_minimal_html

embed_minimal_html('carpentries_instructors_basic.html', views=[map], title='The Carpentries Instructors')
~~~
{: .language-python}

<iframe width="800" height="400" src="../files/carpentries_instructors_basic.html" frameborder="0" allowfullscreen></iframe>

The map is not exactly similar to what is shown in Github yet. 

# Add a pop 

We added all the instructors locations (nearest airport) on our interactive map but it would be nice to add 
labels (using available information such as list of instructors):


~~~
from ipyleaflet import GeoJSON, Marker
from ipywidgets import HTML

features = geojson['features']
for i in range(len(features)):
    location=(features[i]['geometry']['coordinates'][1],features[i]['geometry']['coordinates'][0])
    instructors = features[i]['properties']['details']
    html = """
    <p>
      <h4><b>Instructors</b>:        """ + " ".join(instructors) + """</h4>
    </p>
    """
    marker = Marker(location=location)

    # Popup associated to a layer
    marker.popup = HTML(html)
    map.add_layer(marker)
~~~
{: .language-python}

<iframe width="800" height="400" src="../files/carpentries_instructors.html" frameborder="0" allowfullscreen></iframe>


## Customize markers

- When creating a marker, we can pass an additional argument to `Marker` to specify the *url* to a new icon



~~~
# First is latitude and second is longitude; both in degrees
center = (53.483959, -2.244644)

map = Map(center=center, interpolation='nearest', basemap=basemaps.Stamen.Terrain)
map
~~~
{: .language-python}

And now we will be using the Carpentries logo <img src="https://github.com/carpentries/carpentries.org/raw/gh-pages/assets/img/Badge_Carpentries.png" />

~~~
from ipyleaflet import Icon

features = geojson['features']
for i in range(len(features)):
    location=(features[i]['geometry']['coordinates'][1],features[i]['geometry']['coordinates'][0])
    instructors = features[i]['properties']['details']
    html = """
    <p>
      <h4><b>Instructors</b>:        """ + " ".join(instructors) + """</h4>
    </p>
    """
    icon = Icon(icon_url='https://github.com/carpentries/carpentries.org/raw/gh-pages/assets/img/Badge_Carpentries.png', 
                icon_size=[22, 22])
    marker = Marker(location=location, icon=icon)

    # Popup associated to a layer
    marker.popup = HTML(html)
    map.add_layer(marker)
~~~
{: .language-python}

<iframe width="800" height="400" src="../files/carpentries_instructors_with_logo.html" frameborder="0" allowfullscreen></iframe>

## Marker cluster

- The map starts to be a bit messy when we zoom out and our goal now is to cluster (group) markers together


~~~

from ipyleaflet import Icon, Marker, MarkerCluster

# First is latitude and second is longitude; both in degrees
center = (53.483959, -2.244644)

zoom = 2
map = Map(center=center, interpolation='nearest', zoom = zoom, basemap=basemaps.Stamen.Terrain)

markers = ()
features = geojson['features']
for i in range(len(features)):
    location=(features[i]['geometry']['coordinates'][1],features[i]['geometry']['coordinates'][0])
    instructors = features[i]['properties']['details']
    html = """
    <p>
      <h4><b>Instructors</b>:        """ + " ".join(instructors) + """</h4>
    </p>
    """
    marker = Marker(location=location)

    markers = markers + (marker,)
    # Popup associated to a layer
    marker.popup = HTML(html)

map.add_layer(MarkerCluster(markers = markers, name='Instructors'))
~~~
{: .language-python}

## Control map layers


~~~
from ipyleaflet import LayersControl
map.add_control(LayersControl())
~~~
{: .language-python}


<iframe width="800" height="400" src="../files/carpentries_instructors_with_marker_cluster.html" frameborder="0" allowfullscreen></iframe>


## Circle marker 

The main advantage is that 
- circle outline and fill colors can be chosen
- circle radius can be adjusted too; choose an HTML color (see https://www.rapidtables.com/web/color/html-color-codes.html)

We will be using it to visualize the Carpentries instructors:
- the radius of the circle marker will depend on the number of instructors on the corresponding location
- color also depend on the number of instructors: blue if one instructor only, orange if between 2 and 9, and
red if more or equal to 10.

~~~
from ipyleaflet import Icon, CircleMarker

# First is latitude and second is longitude; both in degrees
center = (53.483959, -2.244644)

zoom = 2
map = Map(center=center, interpolation='nearest', zoom = zoom, basemap=basemaps.Stamen.Terrain)

features = geojson['features']
for i in range(len(features)):
    location=(features[i]['geometry']['coordinates'][1],features[i]['geometry']['coordinates'][0])
    instructors = features[i]['properties']['details']
    html = """
    <p>
      <h4><b>Instructors</b>:        """ + " ".join(instructors) + """</h4>
    </p>
    """
    # set color from the number of instructors per location
    if len(instructors) == 1:
        color = "blue"
    elif len(instructors) < 10:
        color = "orange"
    else:
        color = "red"
    marker = CircleMarker(location=location, radius = len(instructors), 
                          color="darkblue", fill_color=color, 
                          fill_opacity=0.8, opacity=0.6)

    # Popup associated to a layer
    marker.popup = HTML(html)
    map.add_layer(marker)
~~~
{: .language-python}

## Choropleth maps



~~~
from rasterstats import zonal_stats
import geopandas as gpd
import pandas as pd
import json

country_file = gpd.datasets.get_path('naturalearth_lowres')
zs=zonal_stats(country_file, "../data/t2m_ERA5_25122018_shift_C.nc", stats="mean")
df_zonal_stats = pd.DataFrame(zs)

countries = gpd.read_file(country_file)

df = pd.concat([countries, df_zonal_stats], axis=1)
ddf = df.dropna()

ddf.head()
~~~
{: .language-python}


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>pop_est</th>
      <th>continent</th>
      <th>name</th>
      <th>iso_a3</th>
      <th>gdp_md_est</th>
      <th>geometry</th>
      <th>mean</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>53950935</td>
      <td>Africa</td>
      <td>Tanzania</td>
      <td>TZA</td>
      <td>150600.0</td>
      <td>POLYGON ((33.90371119710453 -0.950000000000000...</td>
      <td>27.045495</td>
    </tr>
    <tr>
      <th>2</th>
      <td>603253</td>
      <td>Africa</td>
      <td>W. Sahara</td>
      <td>ESH</td>
      <td>906.5</td>
      <td>POLYGON ((-8.665589565454809 27.65642588959236...</td>
      <td>20.714767</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35623680</td>
      <td>North America</td>
      <td>Canada</td>
      <td>CAN</td>
      <td>1674000.0</td>
      <td>(POLYGON ((-122.84 49.00000000000011, -122.974...</td>
      <td>-22.414753</td>
    </tr>
    <tr>
      <th>4</th>
      <td>326625791</td>
      <td>North America</td>
      <td>United States of America</td>
      <td>USA</td>
      <td>18560000.0</td>
      <td>(POLYGON ((-122.84 49.00000000000011, -120 49....</td>
      <td>-5.690464</td>
    </tr>
    <tr>
      <th>5</th>
      <td>18556698</td>
      <td>Asia</td>
      <td>Kazakhstan</td>
      <td>KAZ</td>
      <td>460700.0</td>
      <td>POLYGON ((87.35997033076265 49.21498078062912,...</td>
      <td>-16.670960</td>
    </tr>
  </tbody>
</table>
</div>

~~~
t2m =  dict(zip(ddf['name'].rename(columns={'name': 'id'}).tolist(), ddf['mean'].tolist()))
t2m
~~~
{: .language-python}



~~~
    {'Tanzania': 27.045494995406656,
     'W. Sahara': 20.71476744692734,
     'Canada': -22.41475295756037,
     'United States of America': -5.69046373681444,
     'Kazakhstan': -16.67096033269782,
     'Uzbekistan': 1.764118679849665,
     'Papua New Guinea': 24.81196305280159,
     'Indonesia': 25.875466322164467,
     'Argentina': 21.752232377674307,
     'Chile': 11.135730382916863,
     'Dem. Rep. Congo': 29.569072289446247,
     'Somalia': 30.30170728961632,
     'Kenya': 31.67995189114872,
     'Sudan': 27.486022838258766,
     'Chad': 26.692429239353384,
     'Dominican Rep.': 22.675683920426366,
     'Russia': -23.900154647163287,
     'Norway': -3.7497136369111415,
     'Greenland': -27.526973941152935,
     'South Africa': 33.44356093718852,
     'Mexico': 11.942822093680562,
     'Uruguay': 23.08714209232309,
     'Brazil': 24.905459109351693,
     'Bolivia': 20.032272190567276,
     'Peru': 17.16049273626683,
     'Colombia': 21.798475487111606,
     'Panama': 24.725506654058677,
     'Nicaragua': 21.889041330767355,
     'Honduras': 21.265986259753504,
     'El Salvador': 22.105261355411415,
     'Guatemala': 19.12085601525837,
     'Venezuela': 23.089038759205923,
     'Guyana': 23.260331487062643,
     'Suriname': 23.828620301834405,
     'France': 8.417135003615556,
     'Ecuador': 19.973881945816895,
     'Cuba': 23.046956462742855,
     'Zimbabwe': 32.16859650239843,
     'Botswana': 34.630019657943635,
     'Namibia': 31.81408359835583,
     'Senegal': 30.52788481539531,
     'Mali': 24.546538352410437,
     'Mauritania': 23.354622926381126,
     'Benin': 30.22631478102333,
     'Niger': 22.469154678136825,
     'Nigeria': 29.489976969825147,
     'Cameroon': 30.451354306672624,
     'Ghana': 31.08194862854576,
     "Côte d'Ivoire": 30.11038101780956,
     'Guinea': 29.309276343168847,
     'Guinea-Bissau': 28.614622097327697,
     'Liberia': 29.126722155695234,
     'Sierra Leone': 29.253324670124982,
     'Burkina Faso': 28.821453620901707,
     'Central African Rep.': 33.250194500675555,
     'Congo': 29.470682694898777,
     'Gabon': 28.80239211872915,
     'Eq. Guinea': 27.67434949015842,
     'Zambia': 26.890681219020223,
     'Malawi': 25.979440546978083,
     'Mozambique': 30.113032040839002,
     'Angola': 27.406082694876346,
     'Burundi': 24.782406660543984,
     'Israel': 18.70121846742944,
     'Madagascar': 28.40409207333217,
     'Gambia': 31.051364875059903,
     'Tunisia': 16.61156572925742,
     'Algeria': 17.589869296643716,
     'United Arab Emirates': 22.528455153645723,
     'Iraq': 13.12075033138536,
     'Oman': 24.151883463678914,
     'Vanuatu': 27.55201447621505,
     'Cambodia': 29.06413214856144,
     'Thailand': 26.60842270200081,
     'Laos': 22.496923066718466,
     'Myanmar': 20.29569889916325,
     'Vietnam': 22.37866588657321,
     'North Korea': -4.511850428322532,
     'South Korea': 2.0963740748619557,
     'Mongolia': -23.30693864195563,
     'India': 20.059997088207787,
     'Bangladesh': 20.210491139451506,
     'Nepal': 5.11491941890614,
     'Pakistan': 13.369403359726045,
     'Afghanistan': 5.918473954937194,
     'Tajikistan': -14.939487866831556,
     'Kyrgyzstan': -12.59378509947581,
     'Turkmenistan': 7.281742790873814,
     'Iran': 12.432886152984791,
     'Syria': 11.346418462485985,
     'Sweden': 0.217369902365661,
     'Belarus': -4.743480871389693,
     'Ukraine': -1.6838653944593165,
     'Poland': 2.0825046982811415,
     'Austria': 0.9910914488852995,
     'Hungary': 2.8823054144399123,
     'Romania': -0.029552417444435264,
     'Lithuania': 0.29406636944059983,
     'Latvia': -3.528902816289275,
     'Estonia': 0.4676113892207354,
     'Germany': 3.5804481725834227,
     'Bulgaria': 2.352424104045724,
     'Greece': 6.263588299827774,
     'Turkey': 5.532040235679036,
     'Albania': 4.457724344001122,
     'Switzerland': 1.1006239613694788,
     'Belgium': 5.082201915177109,
     'Portugal': 11.437221389502099,
     'Spain': 11.216733864371605,
     'Ireland': 10.672627552356118,
     'New Caledonia': 25.312999221019197,
     'New Zealand': 14.604062375448564,
     'Australia': 30.106313166468738,
     'Sri Lanka': 26.886284400337274,
     'China': -7.326497924451795,
     'Italy': 6.3752545625551535,
     'Denmark': 6.9727046306506395,
     'United Kingdom': 7.44461905943794,
     'Iceland': 4.226212442614155,
     'Azerbaijan': 0.2912213691163288,
     'Philippines': 26.56784757832856,
     'Malaysia': 25.98548617266715,
     'Slovenia': 3.1191516914348654,
     'Finland': -3.5334074001360327,
     'Slovakia': 0.9462826937781585,
     'Eritrea': 32.005862483850535,
     'Japan': 1.5481425123762391,
     'Paraguay': 26.81089189174427,
     'Yemen': 23.016799459305652,
     'Saudi Arabia': 21.604067131620234,
     'Antarctica': -19.436817208438438,
     'Morocco': 18.714198781408868,
     'Egypt': 20.496632518218867,
     'Libya': 17.524020555478355,
     'Ethiopia': 30.042507438644986,
     'Djibouti': 27.882034513829694,
     'Somaliland': 27.28102819532893,
     'Uganda': 27.406208209596514,
     'Bosnia and Herz.': 2.58286913031111,
     'Macedonia': 3.099236689165025,
     'Serbia': -0.2863136967092714,
     'S. Sudan': 35.95899043441552}
~~~
{: .output}


~~~
ddf.to_file("../data/output.json", driver="GeoJSON")
geojson_data = json.load(open("../data/output.json",'r'))
~~~
{: .language-python}


~~~
for feature in geojson_data['features']:
    properties = feature['properties']
    feature.update(id=properties['name'])
    print(feature['id'])
~~~
{: .language-python}

~~~
    Tanzania
    W. Sahara
    Canada
    United States of America
    Kazakhstan
    Uzbekistan
    Papua New Guinea
    Indonesia
    Argentina
    Chile
    Dem. Rep. Congo
    Somalia
    Kenya
    Sudan
    Chad
    Dominican Rep.
    Russia
    Norway
    Greenland
    South Africa
    Mexico
    Uruguay
    Brazil
    Bolivia
    Peru
    Colombia
    Panama
    Nicaragua
    Honduras
    El Salvador
    Guatemala
    Venezuela
    Guyana
    Suriname
    France
    Ecuador
    Cuba
    Zimbabwe
    Botswana
    Namibia
    Senegal
    Mali
    Mauritania
    Benin
    Niger
    Nigeria
    Cameroon
    Ghana
    Côte d'Ivoire
    Guinea
    Guinea-Bissau
    Liberia
    Sierra Leone
    Burkina Faso
    Central African Rep.
    Congo
    Gabon
    Eq. Guinea
    Zambia
    Malawi
    Mozambique
    Angola
    Burundi
    Israel
    Madagascar
    Gambia
    Tunisia
    Algeria
    United Arab Emirates
    Iraq
    Oman
    Vanuatu
    Cambodia
    Thailand
    Laos
    Myanmar
    Vietnam
    North Korea
    South Korea
    Mongolia
    India
    Bangladesh
    Nepal
    Pakistan
    Afghanistan
    Tajikistan
    Kyrgyzstan
    Turkmenistan
    Iran
    Syria
    Sweden
    Belarus
    Ukraine
    Poland
    Austria
    Hungary
    Romania
    Lithuania
    Latvia
    Estonia
    Germany
    Bulgaria
    Greece
    Turkey
    Albania
    Switzerland
    Belgium
    Portugal
    Spain
    Ireland
    New Caledonia
    New Zealand
    Australia
    Sri Lanka
    China
    Italy
    Denmark
    United Kingdom
    Iceland
    Azerbaijan
    Philippines
    Malaysia
    Slovenia
    Finland
    Slovakia
    Eritrea
    Japan
    Paraguay
    Yemen
    Saudi Arabia
    Antarctica
    Morocco
    Egypt
    Libya
    Ethiopia
    Djibouti
    Somaliland
    Uganda
    Bosnia and Herz.
    Macedonia
    Serbia
    S. Sudan
~~~
{: .output}    


~~~
import branca.colormap as cm
step = cm.StepColormap(['blue', 'cyan','yellow', 'red'],
                           vmin = int(ddf['mean'].min()), vmax = int(ddf['mean'].max()))
colors = step.to_linear()

colors
~~~
{: .language-python}

<svg height="50" width="500"><line x1="0" y1="0" x2="0" y2="20" style="stroke:#0000ff;stroke-width:3;" /><line x1="1" y1="0" x2="1" y2="20" style="stroke:#0001ff;stroke-width:3;" /><line x1="2" y1="0" x2="2" y2="20" style="stroke:#0003ff;stroke-width:3;" /><line x1="3" y1="0" x2="3" y2="20" style="stroke:#0004ff;stroke-width:3;" /><line x1="4" y1="0" x2="4" y2="20" style="stroke:#0006ff;stroke-width:3;" /><line x1="5" y1="0" x2="5" y2="20" style="stroke:#0007ff;stroke-width:3;" /><line x1="6" y1="0" x2="6" y2="20" style="stroke:#0009ff;stroke-width:3;" /><line x1="7" y1="0" x2="7" y2="20" style="stroke:#000aff;stroke-width:3;" /><line x1="8" y1="0" x2="8" y2="20" style="stroke:#000cff;stroke-width:3;" /><line x1="9" y1="0" x2="9" y2="20" style="stroke:#000dff;stroke-width:3;" /><line x1="10" y1="0" x2="10" y2="20" style="stroke:#000fff;stroke-width:3;" /><line x1="11" y1="0" x2="11" y2="20" style="stroke:#0010ff;stroke-width:3;" /><line x1="12" y1="0" x2="12" y2="20" style="stroke:#0012ff;stroke-width:3;" /><line x1="13" y1="0" x2="13" y2="20" style="stroke:#0014ff;stroke-width:3;" /><line x1="14" y1="0" x2="14" y2="20" style="stroke:#0015ff;stroke-width:3;" /><line x1="15" y1="0" x2="15" y2="20" style="stroke:#0017ff;stroke-width:3;" /><line x1="16" y1="0" x2="16" y2="20" style="stroke:#0018ff;stroke-width:3;" /><line x1="17" y1="0" x2="17" y2="20" style="stroke:#001aff;stroke-width:3;" /><line x1="18" y1="0" x2="18" y2="20" style="stroke:#001bff;stroke-width:3;" /><line x1="19" y1="0" x2="19" y2="20" style="stroke:#001dff;stroke-width:3;" /><line x1="20" y1="0" x2="20" y2="20" style="stroke:#001eff;stroke-width:3;" /><line x1="21" y1="0" x2="21" y2="20" style="stroke:#0020ff;stroke-width:3;" /><line x1="22" y1="0" x2="22" y2="20" style="stroke:#0021ff;stroke-width:3;" /><line x1="23" y1="0" x2="23" y2="20" style="stroke:#0023ff;stroke-width:3;" /><line x1="24" y1="0" x2="24" y2="20" style="stroke:#0024ff;stroke-width:3;" /><line x1="25" y1="0" x2="25" y2="20" style="stroke:#0026ff;stroke-width:3;" /><line x1="26" y1="0" x2="26" y2="20" style="stroke:#0028ff;stroke-width:3;" /><line x1="27" y1="0" x2="27" y2="20" style="stroke:#0029ff;stroke-width:3;" /><line x1="28" y1="0" x2="28" y2="20" style="stroke:#002bff;stroke-width:3;" /><line x1="29" y1="0" x2="29" y2="20" style="stroke:#002cff;stroke-width:3;" /><line x1="30" y1="0" x2="30" y2="20" style="stroke:#002eff;stroke-width:3;" /><line x1="31" y1="0" x2="31" y2="20" style="stroke:#002fff;stroke-width:3;" /><line x1="32" y1="0" x2="32" y2="20" style="stroke:#0031ff;stroke-width:3;" /><line x1="33" y1="0" x2="33" y2="20" style="stroke:#0032ff;stroke-width:3;" /><line x1="34" y1="0" x2="34" y2="20" style="stroke:#0034ff;stroke-width:3;" /><line x1="35" y1="0" x2="35" y2="20" style="stroke:#0035ff;stroke-width:3;" /><line x1="36" y1="0" x2="36" y2="20" style="stroke:#0037ff;stroke-width:3;" /><line x1="37" y1="0" x2="37" y2="20" style="stroke:#0038ff;stroke-width:3;" /><line x1="38" y1="0" x2="38" y2="20" style="stroke:#003aff;stroke-width:3;" /><line x1="39" y1="0" x2="39" y2="20" style="stroke:#003cff;stroke-width:3;" /><line x1="40" y1="0" x2="40" y2="20" style="stroke:#003dff;stroke-width:3;" /><line x1="41" y1="0" x2="41" y2="20" style="stroke:#003fff;stroke-width:3;" /><line x1="42" y1="0" x2="42" y2="20" style="stroke:#0040ff;stroke-width:3;" /><line x1="43" y1="0" x2="43" y2="20" style="stroke:#0042ff;stroke-width:3;" /><line x1="44" y1="0" x2="44" y2="20" style="stroke:#0043ff;stroke-width:3;" /><line x1="45" y1="0" x2="45" y2="20" style="stroke:#0045ff;stroke-width:3;" /><line x1="46" y1="0" x2="46" y2="20" style="stroke:#0046ff;stroke-width:3;" /><line x1="47" y1="0" x2="47" y2="20" style="stroke:#0048ff;stroke-width:3;" /><line x1="48" y1="0" x2="48" y2="20" style="stroke:#0049ff;stroke-width:3;" /><line x1="49" y1="0" x2="49" y2="20" style="stroke:#004bff;stroke-width:3;" /><line x1="50" y1="0" x2="50" y2="20" style="stroke:#004cff;stroke-width:3;" /><line x1="51" y1="0" x2="51" y2="20" style="stroke:#004eff;stroke-width:3;" /><line x1="52" y1="0" x2="52" y2="20" style="stroke:#0050ff;stroke-width:3;" /><line x1="53" y1="0" x2="53" y2="20" style="stroke:#0051ff;stroke-width:3;" /><line x1="54" y1="0" x2="54" y2="20" style="stroke:#0053ff;stroke-width:3;" /><line x1="55" y1="0" x2="55" y2="20" style="stroke:#0054ff;stroke-width:3;" /><line x1="56" y1="0" x2="56" y2="20" style="stroke:#0056ff;stroke-width:3;" /><line x1="57" y1="0" x2="57" y2="20" style="stroke:#0057ff;stroke-width:3;" /><line x1="58" y1="0" x2="58" y2="20" style="stroke:#0059ff;stroke-width:3;" /><line x1="59" y1="0" x2="59" y2="20" style="stroke:#005aff;stroke-width:3;" /><line x1="60" y1="0" x2="60" y2="20" style="stroke:#005cff;stroke-width:3;" /><line x1="61" y1="0" x2="61" y2="20" style="stroke:#005dff;stroke-width:3;" /><line x1="62" y1="0" x2="62" y2="20" style="stroke:#005fff;stroke-width:3;" /><line x1="63" y1="0" x2="63" y2="20" style="stroke:#0060ff;stroke-width:3;" /><line x1="64" y1="0" x2="64" y2="20" style="stroke:#0062ff;stroke-width:3;" /><line x1="65" y1="0" x2="65" y2="20" style="stroke:#0064ff;stroke-width:3;" /><line x1="66" y1="0" x2="66" y2="20" style="stroke:#0065ff;stroke-width:3;" /><line x1="67" y1="0" x2="67" y2="20" style="stroke:#0067ff;stroke-width:3;" /><line x1="68" y1="0" x2="68" y2="20" style="stroke:#0068ff;stroke-width:3;" /><line x1="69" y1="0" x2="69" y2="20" style="stroke:#006aff;stroke-width:3;" /><line x1="70" y1="0" x2="70" y2="20" style="stroke:#006bff;stroke-width:3;" /><line x1="71" y1="0" x2="71" y2="20" style="stroke:#006dff;stroke-width:3;" /><line x1="72" y1="0" x2="72" y2="20" style="stroke:#006eff;stroke-width:3;" /><line x1="73" y1="0" x2="73" y2="20" style="stroke:#0070ff;stroke-width:3;" /><line x1="74" y1="0" x2="74" y2="20" style="stroke:#0071ff;stroke-width:3;" /><line x1="75" y1="0" x2="75" y2="20" style="stroke:#0073ff;stroke-width:3;" /><line x1="76" y1="0" x2="76" y2="20" style="stroke:#0074ff;stroke-width:3;" /><line x1="77" y1="0" x2="77" y2="20" style="stroke:#0076ff;stroke-width:3;" /><line x1="78" y1="0" x2="78" y2="20" style="stroke:#0078ff;stroke-width:3;" /><line x1="79" y1="0" x2="79" y2="20" style="stroke:#0079ff;stroke-width:3;" /><line x1="80" y1="0" x2="80" y2="20" style="stroke:#007bff;stroke-width:3;" /><line x1="81" y1="0" x2="81" y2="20" style="stroke:#007cff;stroke-width:3;" /><line x1="82" y1="0" x2="82" y2="20" style="stroke:#007eff;stroke-width:3;" /><line x1="83" y1="0" x2="83" y2="20" style="stroke:#007fff;stroke-width:3;" /><line x1="84" y1="0" x2="84" y2="20" style="stroke:#0081ff;stroke-width:3;" /><line x1="85" y1="0" x2="85" y2="20" style="stroke:#0082ff;stroke-width:3;" /><line x1="86" y1="0" x2="86" y2="20" style="stroke:#0084ff;stroke-width:3;" /><line x1="87" y1="0" x2="87" y2="20" style="stroke:#0085ff;stroke-width:3;" /><line x1="88" y1="0" x2="88" y2="20" style="stroke:#0087ff;stroke-width:3;" /><line x1="89" y1="0" x2="89" y2="20" style="stroke:#0088ff;stroke-width:3;" /><line x1="90" y1="0" x2="90" y2="20" style="stroke:#008aff;stroke-width:3;" /><line x1="91" y1="0" x2="91" y2="20" style="stroke:#008cff;stroke-width:3;" /><line x1="92" y1="0" x2="92" y2="20" style="stroke:#008dff;stroke-width:3;" /><line x1="93" y1="0" x2="93" y2="20" style="stroke:#008fff;stroke-width:3;" /><line x1="94" y1="0" x2="94" y2="20" style="stroke:#0090ff;stroke-width:3;" /><line x1="95" y1="0" x2="95" y2="20" style="stroke:#0092ff;stroke-width:3;" /><line x1="96" y1="0" x2="96" y2="20" style="stroke:#0093ff;stroke-width:3;" /><line x1="97" y1="0" x2="97" y2="20" style="stroke:#0095ff;stroke-width:3;" /><line x1="98" y1="0" x2="98" y2="20" style="stroke:#0096ff;stroke-width:3;" /><line x1="99" y1="0" x2="99" y2="20" style="stroke:#0098ff;stroke-width:3;" /><line x1="100" y1="0" x2="100" y2="20" style="stroke:#0099ff;stroke-width:3;" /><line x1="101" y1="0" x2="101" y2="20" style="stroke:#009bff;stroke-width:3;" /><line x1="102" y1="0" x2="102" y2="20" style="stroke:#009cff;stroke-width:3;" /><line x1="103" y1="0" x2="103" y2="20" style="stroke:#009eff;stroke-width:3;" /><line x1="104" y1="0" x2="104" y2="20" style="stroke:#00a0ff;stroke-width:3;" /><line x1="105" y1="0" x2="105" y2="20" style="stroke:#00a1ff;stroke-width:3;" /><line x1="106" y1="0" x2="106" y2="20" style="stroke:#00a3ff;stroke-width:3;" /><line x1="107" y1="0" x2="107" y2="20" style="stroke:#00a4ff;stroke-width:3;" /><line x1="108" y1="0" x2="108" y2="20" style="stroke:#00a6ff;stroke-width:3;" /><line x1="109" y1="0" x2="109" y2="20" style="stroke:#00a7ff;stroke-width:3;" /><line x1="110" y1="0" x2="110" y2="20" style="stroke:#00a9ff;stroke-width:3;" /><line x1="111" y1="0" x2="111" y2="20" style="stroke:#00aaff;stroke-width:3;" /><line x1="112" y1="0" x2="112" y2="20" style="stroke:#00acff;stroke-width:3;" /><line x1="113" y1="0" x2="113" y2="20" style="stroke:#00adff;stroke-width:3;" /><line x1="114" y1="0" x2="114" y2="20" style="stroke:#00afff;stroke-width:3;" /><line x1="115" y1="0" x2="115" y2="20" style="stroke:#00b0ff;stroke-width:3;" /><line x1="116" y1="0" x2="116" y2="20" style="stroke:#00b2ff;stroke-width:3;" /><line x1="117" y1="0" x2="117" y2="20" style="stroke:#00b4ff;stroke-width:3;" /><line x1="118" y1="0" x2="118" y2="20" style="stroke:#00b5ff;stroke-width:3;" /><line x1="119" y1="0" x2="119" y2="20" style="stroke:#00b7ff;stroke-width:3;" /><line x1="120" y1="0" x2="120" y2="20" style="stroke:#00b8ff;stroke-width:3;" /><line x1="121" y1="0" x2="121" y2="20" style="stroke:#00baff;stroke-width:3;" /><line x1="122" y1="0" x2="122" y2="20" style="stroke:#00bbff;stroke-width:3;" /><line x1="123" y1="0" x2="123" y2="20" style="stroke:#00bdff;stroke-width:3;" /><line x1="124" y1="0" x2="124" y2="20" style="stroke:#00beff;stroke-width:3;" /><line x1="125" y1="0" x2="125" y2="20" style="stroke:#00c0ff;stroke-width:3;" /><line x1="126" y1="0" x2="126" y2="20" style="stroke:#00c1ff;stroke-width:3;" /><line x1="127" y1="0" x2="127" y2="20" style="stroke:#00c3ff;stroke-width:3;" /><line x1="128" y1="0" x2="128" y2="20" style="stroke:#00c5ff;stroke-width:3;" /><line x1="129" y1="0" x2="129" y2="20" style="stroke:#00c6ff;stroke-width:3;" /><line x1="130" y1="0" x2="130" y2="20" style="stroke:#00c8ff;stroke-width:3;" /><line x1="131" y1="0" x2="131" y2="20" style="stroke:#00c9ff;stroke-width:3;" /><line x1="132" y1="0" x2="132" y2="20" style="stroke:#00cbff;stroke-width:3;" /><line x1="133" y1="0" x2="133" y2="20" style="stroke:#00ccff;stroke-width:3;" /><line x1="134" y1="0" x2="134" y2="20" style="stroke:#00ceff;stroke-width:3;" /><line x1="135" y1="0" x2="135" y2="20" style="stroke:#00cfff;stroke-width:3;" /><line x1="136" y1="0" x2="136" y2="20" style="stroke:#00d1ff;stroke-width:3;" /><line x1="137" y1="0" x2="137" y2="20" style="stroke:#00d2ff;stroke-width:3;" /><line x1="138" y1="0" x2="138" y2="20" style="stroke:#00d4ff;stroke-width:3;" /><line x1="139" y1="0" x2="139" y2="20" style="stroke:#00d5ff;stroke-width:3;" /><line x1="140" y1="0" x2="140" y2="20" style="stroke:#00d7ff;stroke-width:3;" /><line x1="141" y1="0" x2="141" y2="20" style="stroke:#00d9ff;stroke-width:3;" /><line x1="142" y1="0" x2="142" y2="20" style="stroke:#00daff;stroke-width:3;" /><line x1="143" y1="0" x2="143" y2="20" style="stroke:#00dcff;stroke-width:3;" /><line x1="144" y1="0" x2="144" y2="20" style="stroke:#00ddff;stroke-width:3;" /><line x1="145" y1="0" x2="145" y2="20" style="stroke:#00dfff;stroke-width:3;" /><line x1="146" y1="0" x2="146" y2="20" style="stroke:#00e0ff;stroke-width:3;" /><line x1="147" y1="0" x2="147" y2="20" style="stroke:#00e2ff;stroke-width:3;" /><line x1="148" y1="0" x2="148" y2="20" style="stroke:#00e3ff;stroke-width:3;" /><line x1="149" y1="0" x2="149" y2="20" style="stroke:#00e5ff;stroke-width:3;" /><line x1="150" y1="0" x2="150" y2="20" style="stroke:#00e6ff;stroke-width:3;" /><line x1="151" y1="0" x2="151" y2="20" style="stroke:#00e8ff;stroke-width:3;" /><line x1="152" y1="0" x2="152" y2="20" style="stroke:#00e9ff;stroke-width:3;" /><line x1="153" y1="0" x2="153" y2="20" style="stroke:#00ebff;stroke-width:3;" /><line x1="154" y1="0" x2="154" y2="20" style="stroke:#00edff;stroke-width:3;" /><line x1="155" y1="0" x2="155" y2="20" style="stroke:#00eeff;stroke-width:3;" /><line x1="156" y1="0" x2="156" y2="20" style="stroke:#00f0ff;stroke-width:3;" /><line x1="157" y1="0" x2="157" y2="20" style="stroke:#00f1ff;stroke-width:3;" /><line x1="158" y1="0" x2="158" y2="20" style="stroke:#00f3ff;stroke-width:3;" /><line x1="159" y1="0" x2="159" y2="20" style="stroke:#00f4ff;stroke-width:3;" /><line x1="160" y1="0" x2="160" y2="20" style="stroke:#00f6ff;stroke-width:3;" /><line x1="161" y1="0" x2="161" y2="20" style="stroke:#00f7ff;stroke-width:3;" /><line x1="162" y1="0" x2="162" y2="20" style="stroke:#00f9ff;stroke-width:3;" /><line x1="163" y1="0" x2="163" y2="20" style="stroke:#00faff;stroke-width:3;" /><line x1="164" y1="0" x2="164" y2="20" style="stroke:#00fcff;stroke-width:3;" /><line x1="165" y1="0" x2="165" y2="20" style="stroke:#00fdff;stroke-width:3;" /><line x1="166" y1="0" x2="166" y2="20" style="stroke:#00ffff;stroke-width:3;" /><line x1="167" y1="0" x2="167" y2="20" style="stroke:#01fffe;stroke-width:3;" /><line x1="168" y1="0" x2="168" y2="20" style="stroke:#02fffd;stroke-width:3;" /><line x1="169" y1="0" x2="169" y2="20" style="stroke:#04fffb;stroke-width:3;" /><line x1="170" y1="0" x2="170" y2="20" style="stroke:#05fffa;stroke-width:3;" /><line x1="171" y1="0" x2="171" y2="20" style="stroke:#07fff8;stroke-width:3;" /><line x1="172" y1="0" x2="172" y2="20" style="stroke:#08fff7;stroke-width:3;" /><line x1="173" y1="0" x2="173" y2="20" style="stroke:#0afff5;stroke-width:3;" /><line x1="174" y1="0" x2="174" y2="20" style="stroke:#0bfff4;stroke-width:3;" /><line x1="175" y1="0" x2="175" y2="20" style="stroke:#0dfff2;stroke-width:3;" /><line x1="176" y1="0" x2="176" y2="20" style="stroke:#0efff1;stroke-width:3;" /><line x1="177" y1="0" x2="177" y2="20" style="stroke:#10ffef;stroke-width:3;" /><line x1="178" y1="0" x2="178" y2="20" style="stroke:#11ffee;stroke-width:3;" /><line x1="179" y1="0" x2="179" y2="20" style="stroke:#13ffec;stroke-width:3;" /><line x1="180" y1="0" x2="180" y2="20" style="stroke:#15ffea;stroke-width:3;" /><line x1="181" y1="0" x2="181" y2="20" style="stroke:#16ffe9;stroke-width:3;" /><line x1="182" y1="0" x2="182" y2="20" style="stroke:#18ffe7;stroke-width:3;" /><line x1="183" y1="0" x2="183" y2="20" style="stroke:#19ffe6;stroke-width:3;" /><line x1="184" y1="0" x2="184" y2="20" style="stroke:#1bffe4;stroke-width:3;" /><line x1="185" y1="0" x2="185" y2="20" style="stroke:#1cffe3;stroke-width:3;" /><line x1="186" y1="0" x2="186" y2="20" style="stroke:#1effe1;stroke-width:3;" /><line x1="187" y1="0" x2="187" y2="20" style="stroke:#1fffe0;stroke-width:3;" /><line x1="188" y1="0" x2="188" y2="20" style="stroke:#21ffde;stroke-width:3;" /><line x1="189" y1="0" x2="189" y2="20" style="stroke:#22ffdd;stroke-width:3;" /><line x1="190" y1="0" x2="190" y2="20" style="stroke:#24ffdb;stroke-width:3;" /><line x1="191" y1="0" x2="191" y2="20" style="stroke:#25ffda;stroke-width:3;" /><line x1="192" y1="0" x2="192" y2="20" style="stroke:#27ffd8;stroke-width:3;" /><line x1="193" y1="0" x2="193" y2="20" style="stroke:#29ffd6;stroke-width:3;" /><line x1="194" y1="0" x2="194" y2="20" style="stroke:#2affd5;stroke-width:3;" /><line x1="195" y1="0" x2="195" y2="20" style="stroke:#2cffd3;stroke-width:3;" /><line x1="196" y1="0" x2="196" y2="20" style="stroke:#2dffd2;stroke-width:3;" /><line x1="197" y1="0" x2="197" y2="20" style="stroke:#2fffd0;stroke-width:3;" /><line x1="198" y1="0" x2="198" y2="20" style="stroke:#30ffcf;stroke-width:3;" /><line x1="199" y1="0" x2="199" y2="20" style="stroke:#32ffcd;stroke-width:3;" /><line x1="200" y1="0" x2="200" y2="20" style="stroke:#33ffcc;stroke-width:3;" /><line x1="201" y1="0" x2="201" y2="20" style="stroke:#35ffca;stroke-width:3;" /><line x1="202" y1="0" x2="202" y2="20" style="stroke:#36ffc9;stroke-width:3;" /><line x1="203" y1="0" x2="203" y2="20" style="stroke:#38ffc7;stroke-width:3;" /><line x1="204" y1="0" x2="204" y2="20" style="stroke:#39ffc6;stroke-width:3;" /><line x1="205" y1="0" x2="205" y2="20" style="stroke:#3bffc4;stroke-width:3;" /><line x1="206" y1="0" x2="206" y2="20" style="stroke:#3dffc2;stroke-width:3;" /><line x1="207" y1="0" x2="207" y2="20" style="stroke:#3effc1;stroke-width:3;" /><line x1="208" y1="0" x2="208" y2="20" style="stroke:#40ffbf;stroke-width:3;" /><line x1="209" y1="0" x2="209" y2="20" style="stroke:#41ffbe;stroke-width:3;" /><line x1="210" y1="0" x2="210" y2="20" style="stroke:#43ffbc;stroke-width:3;" /><line x1="211" y1="0" x2="211" y2="20" style="stroke:#44ffbb;stroke-width:3;" /><line x1="212" y1="0" x2="212" y2="20" style="stroke:#46ffb9;stroke-width:3;" /><line x1="213" y1="0" x2="213" y2="20" style="stroke:#47ffb8;stroke-width:3;" /><line x1="214" y1="0" x2="214" y2="20" style="stroke:#49ffb6;stroke-width:3;" /><line x1="215" y1="0" x2="215" y2="20" style="stroke:#4affb5;stroke-width:3;" /><line x1="216" y1="0" x2="216" y2="20" style="stroke:#4cffb3;stroke-width:3;" /><line x1="217" y1="0" x2="217" y2="20" style="stroke:#4dffb2;stroke-width:3;" /><line x1="218" y1="0" x2="218" y2="20" style="stroke:#4fffb0;stroke-width:3;" /><line x1="219" y1="0" x2="219" y2="20" style="stroke:#51ffae;stroke-width:3;" /><line x1="220" y1="0" x2="220" y2="20" style="stroke:#52ffad;stroke-width:3;" /><line x1="221" y1="0" x2="221" y2="20" style="stroke:#54ffab;stroke-width:3;" /><line x1="222" y1="0" x2="222" y2="20" style="stroke:#55ffaa;stroke-width:3;" /><line x1="223" y1="0" x2="223" y2="20" style="stroke:#57ffa8;stroke-width:3;" /><line x1="224" y1="0" x2="224" y2="20" style="stroke:#58ffa7;stroke-width:3;" /><line x1="225" y1="0" x2="225" y2="20" style="stroke:#5affa5;stroke-width:3;" /><line x1="226" y1="0" x2="226" y2="20" style="stroke:#5bffa4;stroke-width:3;" /><line x1="227" y1="0" x2="227" y2="20" style="stroke:#5dffa2;stroke-width:3;" /><line x1="228" y1="0" x2="228" y2="20" style="stroke:#5effa1;stroke-width:3;" /><line x1="229" y1="0" x2="229" y2="20" style="stroke:#60ff9f;stroke-width:3;" /><line x1="230" y1="0" x2="230" y2="20" style="stroke:#61ff9e;stroke-width:3;" /><line x1="231" y1="0" x2="231" y2="20" style="stroke:#63ff9c;stroke-width:3;" /><line x1="232" y1="0" x2="232" y2="20" style="stroke:#65ff9a;stroke-width:3;" /><line x1="233" y1="0" x2="233" y2="20" style="stroke:#66ff99;stroke-width:3;" /><line x1="234" y1="0" x2="234" y2="20" style="stroke:#68ff97;stroke-width:3;" /><line x1="235" y1="0" x2="235" y2="20" style="stroke:#69ff96;stroke-width:3;" /><line x1="236" y1="0" x2="236" y2="20" style="stroke:#6bff94;stroke-width:3;" /><line x1="237" y1="0" x2="237" y2="20" style="stroke:#6cff93;stroke-width:3;" /><line x1="238" y1="0" x2="238" y2="20" style="stroke:#6eff91;stroke-width:3;" /><line x1="239" y1="0" x2="239" y2="20" style="stroke:#6fff90;stroke-width:3;" /><line x1="240" y1="0" x2="240" y2="20" style="stroke:#71ff8e;stroke-width:3;" /><line x1="241" y1="0" x2="241" y2="20" style="stroke:#72ff8d;stroke-width:3;" /><line x1="242" y1="0" x2="242" y2="20" style="stroke:#74ff8b;stroke-width:3;" /><line x1="243" y1="0" x2="243" y2="20" style="stroke:#75ff8a;stroke-width:3;" /><line x1="244" y1="0" x2="244" y2="20" style="stroke:#77ff88;stroke-width:3;" /><line x1="245" y1="0" x2="245" y2="20" style="stroke:#79ff86;stroke-width:3;" /><line x1="246" y1="0" x2="246" y2="20" style="stroke:#7aff85;stroke-width:3;" /><line x1="247" y1="0" x2="247" y2="20" style="stroke:#7cff83;stroke-width:3;" /><line x1="248" y1="0" x2="248" y2="20" style="stroke:#7dff82;stroke-width:3;" /><line x1="249" y1="0" x2="249" y2="20" style="stroke:#7fff80;stroke-width:3;" /><line x1="250" y1="0" x2="250" y2="20" style="stroke:#80ff7f;stroke-width:3;" /><line x1="251" y1="0" x2="251" y2="20" style="stroke:#82ff7d;stroke-width:3;" /><line x1="252" y1="0" x2="252" y2="20" style="stroke:#83ff7c;stroke-width:3;" /><line x1="253" y1="0" x2="253" y2="20" style="stroke:#85ff7a;stroke-width:3;" /><line x1="254" y1="0" x2="254" y2="20" style="stroke:#86ff79;stroke-width:3;" /><line x1="255" y1="0" x2="255" y2="20" style="stroke:#88ff77;stroke-width:3;" /><line x1="256" y1="0" x2="256" y2="20" style="stroke:#8aff75;stroke-width:3;" /><line x1="257" y1="0" x2="257" y2="20" style="stroke:#8bff74;stroke-width:3;" /><line x1="258" y1="0" x2="258" y2="20" style="stroke:#8dff72;stroke-width:3;" /><line x1="259" y1="0" x2="259" y2="20" style="stroke:#8eff71;stroke-width:3;" /><line x1="260" y1="0" x2="260" y2="20" style="stroke:#90ff6f;stroke-width:3;" /><line x1="261" y1="0" x2="261" y2="20" style="stroke:#91ff6e;stroke-width:3;" /><line x1="262" y1="0" x2="262" y2="20" style="stroke:#93ff6c;stroke-width:3;" /><line x1="263" y1="0" x2="263" y2="20" style="stroke:#94ff6b;stroke-width:3;" /><line x1="264" y1="0" x2="264" y2="20" style="stroke:#96ff69;stroke-width:3;" /><line x1="265" y1="0" x2="265" y2="20" style="stroke:#97ff68;stroke-width:3;" /><line x1="266" y1="0" x2="266" y2="20" style="stroke:#99ff66;stroke-width:3;" /><line x1="267" y1="0" x2="267" y2="20" style="stroke:#9aff65;stroke-width:3;" /><line x1="268" y1="0" x2="268" y2="20" style="stroke:#9cff63;stroke-width:3;" /><line x1="269" y1="0" x2="269" y2="20" style="stroke:#9eff61;stroke-width:3;" /><line x1="270" y1="0" x2="270" y2="20" style="stroke:#9fff60;stroke-width:3;" /><line x1="271" y1="0" x2="271" y2="20" style="stroke:#a1ff5e;stroke-width:3;" /><line x1="272" y1="0" x2="272" y2="20" style="stroke:#a2ff5d;stroke-width:3;" /><line x1="273" y1="0" x2="273" y2="20" style="stroke:#a4ff5b;stroke-width:3;" /><line x1="274" y1="0" x2="274" y2="20" style="stroke:#a5ff5a;stroke-width:3;" /><line x1="275" y1="0" x2="275" y2="20" style="stroke:#a7ff58;stroke-width:3;" /><line x1="276" y1="0" x2="276" y2="20" style="stroke:#a8ff57;stroke-width:3;" /><line x1="277" y1="0" x2="277" y2="20" style="stroke:#aaff55;stroke-width:3;" /><line x1="278" y1="0" x2="278" y2="20" style="stroke:#abff54;stroke-width:3;" /><line x1="279" y1="0" x2="279" y2="20" style="stroke:#adff52;stroke-width:3;" /><line x1="280" y1="0" x2="280" y2="20" style="stroke:#aeff51;stroke-width:3;" /><line x1="281" y1="0" x2="281" y2="20" style="stroke:#b0ff4f;stroke-width:3;" /><line x1="282" y1="0" x2="282" y2="20" style="stroke:#b2ff4d;stroke-width:3;" /><line x1="283" y1="0" x2="283" y2="20" style="stroke:#b3ff4c;stroke-width:3;" /><line x1="284" y1="0" x2="284" y2="20" style="stroke:#b5ff4a;stroke-width:3;" /><line x1="285" y1="0" x2="285" y2="20" style="stroke:#b6ff49;stroke-width:3;" /><line x1="286" y1="0" x2="286" y2="20" style="stroke:#b8ff47;stroke-width:3;" /><line x1="287" y1="0" x2="287" y2="20" style="stroke:#b9ff46;stroke-width:3;" /><line x1="288" y1="0" x2="288" y2="20" style="stroke:#bbff44;stroke-width:3;" /><line x1="289" y1="0" x2="289" y2="20" style="stroke:#bcff43;stroke-width:3;" /><line x1="290" y1="0" x2="290" y2="20" style="stroke:#beff41;stroke-width:3;" /><line x1="291" y1="0" x2="291" y2="20" style="stroke:#bfff40;stroke-width:3;" /><line x1="292" y1="0" x2="292" y2="20" style="stroke:#c1ff3e;stroke-width:3;" /><line x1="293" y1="0" x2="293" y2="20" style="stroke:#c2ff3d;stroke-width:3;" /><line x1="294" y1="0" x2="294" y2="20" style="stroke:#c4ff3b;stroke-width:3;" /><line x1="295" y1="0" x2="295" y2="20" style="stroke:#c6ff39;stroke-width:3;" /><line x1="296" y1="0" x2="296" y2="20" style="stroke:#c7ff38;stroke-width:3;" /><line x1="297" y1="0" x2="297" y2="20" style="stroke:#c9ff36;stroke-width:3;" /><line x1="298" y1="0" x2="298" y2="20" style="stroke:#caff35;stroke-width:3;" /><line x1="299" y1="0" x2="299" y2="20" style="stroke:#ccff33;stroke-width:3;" /><line x1="300" y1="0" x2="300" y2="20" style="stroke:#cdff32;stroke-width:3;" /><line x1="301" y1="0" x2="301" y2="20" style="stroke:#cfff30;stroke-width:3;" /><line x1="302" y1="0" x2="302" y2="20" style="stroke:#d0ff2f;stroke-width:3;" /><line x1="303" y1="0" x2="303" y2="20" style="stroke:#d2ff2d;stroke-width:3;" /><line x1="304" y1="0" x2="304" y2="20" style="stroke:#d3ff2c;stroke-width:3;" /><line x1="305" y1="0" x2="305" y2="20" style="stroke:#d5ff2a;stroke-width:3;" /><line x1="306" y1="0" x2="306" y2="20" style="stroke:#d6ff29;stroke-width:3;" /><line x1="307" y1="0" x2="307" y2="20" style="stroke:#d8ff27;stroke-width:3;" /><line x1="308" y1="0" x2="308" y2="20" style="stroke:#daff25;stroke-width:3;" /><line x1="309" y1="0" x2="309" y2="20" style="stroke:#dbff24;stroke-width:3;" /><line x1="310" y1="0" x2="310" y2="20" style="stroke:#ddff22;stroke-width:3;" /><line x1="311" y1="0" x2="311" y2="20" style="stroke:#deff21;stroke-width:3;" /><line x1="312" y1="0" x2="312" y2="20" style="stroke:#e0ff1f;stroke-width:3;" /><line x1="313" y1="0" x2="313" y2="20" style="stroke:#e1ff1e;stroke-width:3;" /><line x1="314" y1="0" x2="314" y2="20" style="stroke:#e3ff1c;stroke-width:3;" /><line x1="315" y1="0" x2="315" y2="20" style="stroke:#e4ff1b;stroke-width:3;" /><line x1="316" y1="0" x2="316" y2="20" style="stroke:#e6ff19;stroke-width:3;" /><line x1="317" y1="0" x2="317" y2="20" style="stroke:#e7ff18;stroke-width:3;" /><line x1="318" y1="0" x2="318" y2="20" style="stroke:#e9ff16;stroke-width:3;" /><line x1="319" y1="0" x2="319" y2="20" style="stroke:#eaff15;stroke-width:3;" /><line x1="320" y1="0" x2="320" y2="20" style="stroke:#ecff13;stroke-width:3;" /><line x1="321" y1="0" x2="321" y2="20" style="stroke:#eeff11;stroke-width:3;" /><line x1="322" y1="0" x2="322" y2="20" style="stroke:#efff10;stroke-width:3;" /><line x1="323" y1="0" x2="323" y2="20" style="stroke:#f1ff0e;stroke-width:3;" /><line x1="324" y1="0" x2="324" y2="20" style="stroke:#f2ff0d;stroke-width:3;" /><line x1="325" y1="0" x2="325" y2="20" style="stroke:#f4ff0b;stroke-width:3;" /><line x1="326" y1="0" x2="326" y2="20" style="stroke:#f5ff0a;stroke-width:3;" /><line x1="327" y1="0" x2="327" y2="20" style="stroke:#f7ff08;stroke-width:3;" /><line x1="328" y1="0" x2="328" y2="20" style="stroke:#f8ff07;stroke-width:3;" /><line x1="329" y1="0" x2="329" y2="20" style="stroke:#faff05;stroke-width:3;" /><line x1="330" y1="0" x2="330" y2="20" style="stroke:#fbff04;stroke-width:3;" /><line x1="331" y1="0" x2="331" y2="20" style="stroke:#fdff02;stroke-width:3;" /><line x1="332" y1="0" x2="332" y2="20" style="stroke:#feff01;stroke-width:3;" /><line x1="333" y1="0" x2="333" y2="20" style="stroke:#ffff00;stroke-width:3;" /><line x1="334" y1="0" x2="334" y2="20" style="stroke:#fffd00;stroke-width:3;" /><line x1="335" y1="0" x2="335" y2="20" style="stroke:#fffc00;stroke-width:3;" /><line x1="336" y1="0" x2="336" y2="20" style="stroke:#fffa00;stroke-width:3;" /><line x1="337" y1="0" x2="337" y2="20" style="stroke:#fff900;stroke-width:3;" /><line x1="338" y1="0" x2="338" y2="20" style="stroke:#fff700;stroke-width:3;" /><line x1="339" y1="0" x2="339" y2="20" style="stroke:#fff600;stroke-width:3;" /><line x1="340" y1="0" x2="340" y2="20" style="stroke:#fff400;stroke-width:3;" /><line x1="341" y1="0" x2="341" y2="20" style="stroke:#fff300;stroke-width:3;" /><line x1="342" y1="0" x2="342" y2="20" style="stroke:#fff100;stroke-width:3;" /><line x1="343" y1="0" x2="343" y2="20" style="stroke:#fff000;stroke-width:3;" /><line x1="344" y1="0" x2="344" y2="20" style="stroke:#ffee00;stroke-width:3;" /><line x1="345" y1="0" x2="345" y2="20" style="stroke:#ffed00;stroke-width:3;" /><line x1="346" y1="0" x2="346" y2="20" style="stroke:#ffeb00;stroke-width:3;" /><line x1="347" y1="0" x2="347" y2="20" style="stroke:#ffe900;stroke-width:3;" /><line x1="348" y1="0" x2="348" y2="20" style="stroke:#ffe800;stroke-width:3;" /><line x1="349" y1="0" x2="349" y2="20" style="stroke:#ffe600;stroke-width:3;" /><line x1="350" y1="0" x2="350" y2="20" style="stroke:#ffe500;stroke-width:3;" /><line x1="351" y1="0" x2="351" y2="20" style="stroke:#ffe300;stroke-width:3;" /><line x1="352" y1="0" x2="352" y2="20" style="stroke:#ffe200;stroke-width:3;" /><line x1="353" y1="0" x2="353" y2="20" style="stroke:#ffe000;stroke-width:3;" /><line x1="354" y1="0" x2="354" y2="20" style="stroke:#ffdf00;stroke-width:3;" /><line x1="355" y1="0" x2="355" y2="20" style="stroke:#ffdd00;stroke-width:3;" /><line x1="356" y1="0" x2="356" y2="20" style="stroke:#ffdc00;stroke-width:3;" /><line x1="357" y1="0" x2="357" y2="20" style="stroke:#ffda00;stroke-width:3;" /><line x1="358" y1="0" x2="358" y2="20" style="stroke:#ffd900;stroke-width:3;" /><line x1="359" y1="0" x2="359" y2="20" style="stroke:#ffd700;stroke-width:3;" /><line x1="360" y1="0" x2="360" y2="20" style="stroke:#ffd500;stroke-width:3;" /><line x1="361" y1="0" x2="361" y2="20" style="stroke:#ffd400;stroke-width:3;" /><line x1="362" y1="0" x2="362" y2="20" style="stroke:#ffd200;stroke-width:3;" /><line x1="363" y1="0" x2="363" y2="20" style="stroke:#ffd100;stroke-width:3;" /><line x1="364" y1="0" x2="364" y2="20" style="stroke:#ffcf00;stroke-width:3;" /><line x1="365" y1="0" x2="365" y2="20" style="stroke:#ffce00;stroke-width:3;" /><line x1="366" y1="0" x2="366" y2="20" style="stroke:#ffcc00;stroke-width:3;" /><line x1="367" y1="0" x2="367" y2="20" style="stroke:#ffcb00;stroke-width:3;" /><line x1="368" y1="0" x2="368" y2="20" style="stroke:#ffc900;stroke-width:3;" /><line x1="369" y1="0" x2="369" y2="20" style="stroke:#ffc800;stroke-width:3;" /><line x1="370" y1="0" x2="370" y2="20" style="stroke:#ffc600;stroke-width:3;" /><line x1="371" y1="0" x2="371" y2="20" style="stroke:#ffc500;stroke-width:3;" /><line x1="372" y1="0" x2="372" y2="20" style="stroke:#ffc300;stroke-width:3;" /><line x1="373" y1="0" x2="373" y2="20" style="stroke:#ffc100;stroke-width:3;" /><line x1="374" y1="0" x2="374" y2="20" style="stroke:#ffc000;stroke-width:3;" /><line x1="375" y1="0" x2="375" y2="20" style="stroke:#ffbe00;stroke-width:3;" /><line x1="376" y1="0" x2="376" y2="20" style="stroke:#ffbd00;stroke-width:3;" /><line x1="377" y1="0" x2="377" y2="20" style="stroke:#ffbb00;stroke-width:3;" /><line x1="378" y1="0" x2="378" y2="20" style="stroke:#ffba00;stroke-width:3;" /><line x1="379" y1="0" x2="379" y2="20" style="stroke:#ffb800;stroke-width:3;" /><line x1="380" y1="0" x2="380" y2="20" style="stroke:#ffb700;stroke-width:3;" /><line x1="381" y1="0" x2="381" y2="20" style="stroke:#ffb500;stroke-width:3;" /><line x1="382" y1="0" x2="382" y2="20" style="stroke:#ffb400;stroke-width:3;" /><line x1="383" y1="0" x2="383" y2="20" style="stroke:#ffb200;stroke-width:3;" /><line x1="384" y1="0" x2="384" y2="20" style="stroke:#ffb000;stroke-width:3;" /><line x1="385" y1="0" x2="385" y2="20" style="stroke:#ffaf00;stroke-width:3;" /><line x1="386" y1="0" x2="386" y2="20" style="stroke:#ffad00;stroke-width:3;" /><line x1="387" y1="0" x2="387" y2="20" style="stroke:#ffac00;stroke-width:3;" /><line x1="388" y1="0" x2="388" y2="20" style="stroke:#ffaa00;stroke-width:3;" /><line x1="389" y1="0" x2="389" y2="20" style="stroke:#ffa900;stroke-width:3;" /><line x1="390" y1="0" x2="390" y2="20" style="stroke:#ffa700;stroke-width:3;" /><line x1="391" y1="0" x2="391" y2="20" style="stroke:#ffa600;stroke-width:3;" /><line x1="392" y1="0" x2="392" y2="20" style="stroke:#ffa400;stroke-width:3;" /><line x1="393" y1="0" x2="393" y2="20" style="stroke:#ffa300;stroke-width:3;" /><line x1="394" y1="0" x2="394" y2="20" style="stroke:#ffa100;stroke-width:3;" /><line x1="395" y1="0" x2="395" y2="20" style="stroke:#ffa000;stroke-width:3;" /><line x1="396" y1="0" x2="396" y2="20" style="stroke:#ff9e00;stroke-width:3;" /><line x1="397" y1="0" x2="397" y2="20" style="stroke:#ff9c00;stroke-width:3;" /><line x1="398" y1="0" x2="398" y2="20" style="stroke:#ff9b00;stroke-width:3;" /><line x1="399" y1="0" x2="399" y2="20" style="stroke:#ff9900;stroke-width:3;" /><line x1="400" y1="0" x2="400" y2="20" style="stroke:#ff9800;stroke-width:3;" /><line x1="401" y1="0" x2="401" y2="20" style="stroke:#ff9600;stroke-width:3;" /><line x1="402" y1="0" x2="402" y2="20" style="stroke:#ff9500;stroke-width:3;" /><line x1="403" y1="0" x2="403" y2="20" style="stroke:#ff9300;stroke-width:3;" /><line x1="404" y1="0" x2="404" y2="20" style="stroke:#ff9200;stroke-width:3;" /><line x1="405" y1="0" x2="405" y2="20" style="stroke:#ff9000;stroke-width:3;" /><line x1="406" y1="0" x2="406" y2="20" style="stroke:#ff8f00;stroke-width:3;" /><line x1="407" y1="0" x2="407" y2="20" style="stroke:#ff8d00;stroke-width:3;" /><line x1="408" y1="0" x2="408" y2="20" style="stroke:#ff8c00;stroke-width:3;" /><line x1="409" y1="0" x2="409" y2="20" style="stroke:#ff8a00;stroke-width:3;" /><line x1="410" y1="0" x2="410" y2="20" style="stroke:#ff8800;stroke-width:3;" /><line x1="411" y1="0" x2="411" y2="20" style="stroke:#ff8700;stroke-width:3;" /><line x1="412" y1="0" x2="412" y2="20" style="stroke:#ff8500;stroke-width:3;" /><line x1="413" y1="0" x2="413" y2="20" style="stroke:#ff8400;stroke-width:3;" /><line x1="414" y1="0" x2="414" y2="20" style="stroke:#ff8200;stroke-width:3;" /><line x1="415" y1="0" x2="415" y2="20" style="stroke:#ff8100;stroke-width:3;" /><line x1="416" y1="0" x2="416" y2="20" style="stroke:#ff7f00;stroke-width:3;" /><line x1="417" y1="0" x2="417" y2="20" style="stroke:#ff7e00;stroke-width:3;" /><line x1="418" y1="0" x2="418" y2="20" style="stroke:#ff7c00;stroke-width:3;" /><line x1="419" y1="0" x2="419" y2="20" style="stroke:#ff7b00;stroke-width:3;" /><line x1="420" y1="0" x2="420" y2="20" style="stroke:#ff7900;stroke-width:3;" /><line x1="421" y1="0" x2="421" y2="20" style="stroke:#ff7800;stroke-width:3;" /><line x1="422" y1="0" x2="422" y2="20" style="stroke:#ff7600;stroke-width:3;" /><line x1="423" y1="0" x2="423" y2="20" style="stroke:#ff7400;stroke-width:3;" /><line x1="424" y1="0" x2="424" y2="20" style="stroke:#ff7300;stroke-width:3;" /><line x1="425" y1="0" x2="425" y2="20" style="stroke:#ff7100;stroke-width:3;" /><line x1="426" y1="0" x2="426" y2="20" style="stroke:#ff7000;stroke-width:3;" /><line x1="427" y1="0" x2="427" y2="20" style="stroke:#ff6e00;stroke-width:3;" /><line x1="428" y1="0" x2="428" y2="20" style="stroke:#ff6d00;stroke-width:3;" /><line x1="429" y1="0" x2="429" y2="20" style="stroke:#ff6b00;stroke-width:3;" /><line x1="430" y1="0" x2="430" y2="20" style="stroke:#ff6a00;stroke-width:3;" /><line x1="431" y1="0" x2="431" y2="20" style="stroke:#ff6800;stroke-width:3;" /><line x1="432" y1="0" x2="432" y2="20" style="stroke:#ff6700;stroke-width:3;" /><line x1="433" y1="0" x2="433" y2="20" style="stroke:#ff6500;stroke-width:3;" /><line x1="434" y1="0" x2="434" y2="20" style="stroke:#ff6400;stroke-width:3;" /><line x1="435" y1="0" x2="435" y2="20" style="stroke:#ff6200;stroke-width:3;" /><line x1="436" y1="0" x2="436" y2="20" style="stroke:#ff6000;stroke-width:3;" /><line x1="437" y1="0" x2="437" y2="20" style="stroke:#ff5f00;stroke-width:3;" /><line x1="438" y1="0" x2="438" y2="20" style="stroke:#ff5d00;stroke-width:3;" /><line x1="439" y1="0" x2="439" y2="20" style="stroke:#ff5c00;stroke-width:3;" /><line x1="440" y1="0" x2="440" y2="20" style="stroke:#ff5a00;stroke-width:3;" /><line x1="441" y1="0" x2="441" y2="20" style="stroke:#ff5900;stroke-width:3;" /><line x1="442" y1="0" x2="442" y2="20" style="stroke:#ff5700;stroke-width:3;" /><line x1="443" y1="0" x2="443" y2="20" style="stroke:#ff5600;stroke-width:3;" /><line x1="444" y1="0" x2="444" y2="20" style="stroke:#ff5400;stroke-width:3;" /><line x1="445" y1="0" x2="445" y2="20" style="stroke:#ff5300;stroke-width:3;" /><line x1="446" y1="0" x2="446" y2="20" style="stroke:#ff5100;stroke-width:3;" /><line x1="447" y1="0" x2="447" y2="20" style="stroke:#ff5000;stroke-width:3;" /><line x1="448" y1="0" x2="448" y2="20" style="stroke:#ff4e00;stroke-width:3;" /><line x1="449" y1="0" x2="449" y2="20" style="stroke:#ff4c00;stroke-width:3;" /><line x1="450" y1="0" x2="450" y2="20" style="stroke:#ff4b00;stroke-width:3;" /><line x1="451" y1="0" x2="451" y2="20" style="stroke:#ff4900;stroke-width:3;" /><line x1="452" y1="0" x2="452" y2="20" style="stroke:#ff4800;stroke-width:3;" /><line x1="453" y1="0" x2="453" y2="20" style="stroke:#ff4600;stroke-width:3;" /><line x1="454" y1="0" x2="454" y2="20" style="stroke:#ff4500;stroke-width:3;" /><line x1="455" y1="0" x2="455" y2="20" style="stroke:#ff4300;stroke-width:3;" /><line x1="456" y1="0" x2="456" y2="20" style="stroke:#ff4200;stroke-width:3;" /><line x1="457" y1="0" x2="457" y2="20" style="stroke:#ff4000;stroke-width:3;" /><line x1="458" y1="0" x2="458" y2="20" style="stroke:#ff3f00;stroke-width:3;" /><line x1="459" y1="0" x2="459" y2="20" style="stroke:#ff3d00;stroke-width:3;" /><line x1="460" y1="0" x2="460" y2="20" style="stroke:#ff3c00;stroke-width:3;" /><line x1="461" y1="0" x2="461" y2="20" style="stroke:#ff3a00;stroke-width:3;" /><line x1="462" y1="0" x2="462" y2="20" style="stroke:#ff3800;stroke-width:3;" /><line x1="463" y1="0" x2="463" y2="20" style="stroke:#ff3700;stroke-width:3;" /><line x1="464" y1="0" x2="464" y2="20" style="stroke:#ff3500;stroke-width:3;" /><line x1="465" y1="0" x2="465" y2="20" style="stroke:#ff3400;stroke-width:3;" /><line x1="466" y1="0" x2="466" y2="20" style="stroke:#ff3200;stroke-width:3;" /><line x1="467" y1="0" x2="467" y2="20" style="stroke:#ff3100;stroke-width:3;" /><line x1="468" y1="0" x2="468" y2="20" style="stroke:#ff2f00;stroke-width:3;" /><line x1="469" y1="0" x2="469" y2="20" style="stroke:#ff2e00;stroke-width:3;" /><line x1="470" y1="0" x2="470" y2="20" style="stroke:#ff2c00;stroke-width:3;" /><line x1="471" y1="0" x2="471" y2="20" style="stroke:#ff2b00;stroke-width:3;" /><line x1="472" y1="0" x2="472" y2="20" style="stroke:#ff2900;stroke-width:3;" /><line x1="473" y1="0" x2="473" y2="20" style="stroke:#ff2800;stroke-width:3;" /><line x1="474" y1="0" x2="474" y2="20" style="stroke:#ff2600;stroke-width:3;" /><line x1="475" y1="0" x2="475" y2="20" style="stroke:#ff2400;stroke-width:3;" /><line x1="476" y1="0" x2="476" y2="20" style="stroke:#ff2300;stroke-width:3;" /><line x1="477" y1="0" x2="477" y2="20" style="stroke:#ff2100;stroke-width:3;" /><line x1="478" y1="0" x2="478" y2="20" style="stroke:#ff2000;stroke-width:3;" /><line x1="479" y1="0" x2="479" y2="20" style="stroke:#ff1e00;stroke-width:3;" /><line x1="480" y1="0" x2="480" y2="20" style="stroke:#ff1d00;stroke-width:3;" /><line x1="481" y1="0" x2="481" y2="20" style="stroke:#ff1b00;stroke-width:3;" /><line x1="482" y1="0" x2="482" y2="20" style="stroke:#ff1a00;stroke-width:3;" /><line x1="483" y1="0" x2="483" y2="20" style="stroke:#ff1800;stroke-width:3;" /><line x1="484" y1="0" x2="484" y2="20" style="stroke:#ff1700;stroke-width:3;" /><line x1="485" y1="0" x2="485" y2="20" style="stroke:#ff1500;stroke-width:3;" /><line x1="486" y1="0" x2="486" y2="20" style="stroke:#ff1400;stroke-width:3;" /><line x1="487" y1="0" x2="487" y2="20" style="stroke:#ff1200;stroke-width:3;" /><line x1="488" y1="0" x2="488" y2="20" style="stroke:#ff1000;stroke-width:3;" /><line x1="489" y1="0" x2="489" y2="20" style="stroke:#ff0f00;stroke-width:3;" /><line x1="490" y1="0" x2="490" y2="20" style="stroke:#ff0d00;stroke-width:3;" /><line x1="491" y1="0" x2="491" y2="20" style="stroke:#ff0c00;stroke-width:3;" /><line x1="492" y1="0" x2="492" y2="20" style="stroke:#ff0a00;stroke-width:3;" /><line x1="493" y1="0" x2="493" y2="20" style="stroke:#ff0900;stroke-width:3;" /><line x1="494" y1="0" x2="494" y2="20" style="stroke:#ff0700;stroke-width:3;" /><line x1="495" y1="0" x2="495" y2="20" style="stroke:#ff0600;stroke-width:3;" /><line x1="496" y1="0" x2="496" y2="20" style="stroke:#ff0400;stroke-width:3;" /><line x1="497" y1="0" x2="497" y2="20" style="stroke:#ff0300;stroke-width:3;" /><line x1="498" y1="0" x2="498" y2="20" style="stroke:#ff0100;stroke-width:3;" /><line x1="499" y1="0" x2="499" y2="20" style="stroke:#ff0000;stroke-width:3;" /><text x="0" y="35">-27</text><text x="500" y="35" style="text-anchor:end;">35</text></svg>


~~~
from ipyleaflet import Map, Choropleth, basemaps, WidgetControl
import branca.colormap as cm
import matplotlib.pyplot as plt
from ipywidgets import widgets

map = Map(center=(52.3,8.0), zoom = 3, basemap= basemaps.Esri.WorldTopoMap)

geo_data = Choropleth(geo_data = geojson_data, choro_data = t2m,colormap = colors,
    border_color='black', hover_style={ 'fillOpacity': 0.4},
    style={'fillOpacity': 0.8},
                   name = 'Countries')


map.add_layer(geo_data)
out = widgets.Output(layout={'border': '1px solid black'})
out.append_stdout('Temperature (degrees C) 25th December 2018')
with out:
    display(colors)
widget_control = WidgetControl(widget=out, position='topright')
map.add_control(widget_control)

map
~~~

<iframe width="800" height="400" src="../files/temperature_per_country_no_zoom.html" frameborder="0" allowfullscreen></iframe>


## Add widget for zoom

~~~
from ipyleaflet import Map, Choropleth, basemaps, WidgetControl, Popup, Marker
import branca.colormap as cm
import matplotlib.pyplot as plt
from ipywidgets import widgets, IntSlider, jslink

map = Map(center=(52.3,8.0), zoom = 3, basemap= basemaps.Esri.WorldTopoMap)

geo_data = Choropleth(geo_data = geojson_data, choro_data = t2m,colormap = colors,
    border_color='black', hover_style={ 'fillOpacity': 0.4},
    style={'fillOpacity': 0.8},
                   name = 'Countries')


map.add_layer(geo_data)
out = widgets.Output(layout={'border': '1px solid black'})
out.append_stdout('Temperature (degrees C) 25th December 2018')
with out:
    display(colors)
widget_control = WidgetControl(widget=out, position='topright')
map.add_control(widget_control)
zoom_slider = IntSlider(description='Zoom level:', min=0, max=15, value=2)
jslink((zoom_slider, 'value'), (map, 'zoom'))
widget_control_zoom = WidgetControl(widget=zoom_slider, position='bottomright')
map.add_control(widget_control_zoom)

map
~~~
{: .language-python}

<iframe width="800" height="400" src="../files/temperature_per_country.html" frameborder="0" allowfullscreen></iframe>


## Save the resulting map

~~~
from ipywidgets.embed import embed_minimal_html

embed_minimal_html('temperature_per_country.html', views=[map], title='Temperature (degrees C), 25th December 2018')
~~~
{: .language-python}




{% include links.md %}

