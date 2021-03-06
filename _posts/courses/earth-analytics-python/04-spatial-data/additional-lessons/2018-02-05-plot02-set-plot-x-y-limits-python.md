---
layout: single
title: "Customize your Maps in Python: GIS in Python"
excerpt: "In this lesson you will learn how to adjust the x and y limits of your matplotlib and geopandas map to change the spatial extent.."
authors: ['Chris Holdgraf', 'Leah Wasser']
modified: 2019-02-18
category: [courses]
class-lesson: ['hw-custom-maps-python']
permalink: /courses/earth-analytics-python/spatial-data-vector-shapefiles/python-change-spatial-extent-of-map-matplotlib-geopandas/
nav-title: 'Adjust Map Extent'
course: 'earth-analytics-python'
week: 4
sidebar:
  nav:
author_profile: false
comments: true
order: 2
---
{% include toc title="In This Lesson" icon="file-text" %}

<div class='notice--success' markdown="1">

## <i class="fa fa-graduation-cap" aria-hidden="true"></i> Learning Objectives

After completing this tutorial, you will be able to:

* Clip a spatial vector point and line layer to the spatial extent of a polygon layer in `Python`
* Visually "clip" or zoom in to a particular spatial extent in a plot.

## <i class="fa fa-check-square-o fa-2" aria-hidden="true"></i> What You Need

You will need a computer with internet access to complete this lesson and the
spatial-vector-lidar data subset created for the course.

{% include/data_subsets/course_earth_analytics/_data-spatial-lidar.md %}


</div>


In this lesson, you will learn how to spatially clip data for easier plotting and analysis of smaller spatial areas. You will use the `geopandas` library and the `box` module from the `shapely` library. 

{:.input}
```python
# import libraries
import os
import numpy as np
import geopandas as gpd
import os
import matplotlib.pyplot as plt
plt.ion()
```



## Change the Spatial Extent of a Plot in Python

Above you modified your data by clipping it. This is useful when you want to 

1. Make your data smaller to speed up processing and reduce file size
2. Make analysis simpler and faster given less data to work with.

However, if you just want to plot the data, you can consider adjusting the spatial extent of a plot to "zoom in". Note that zooming in on a plot **does not change your data in any way** - it just changes how your plot renders!

To zoom in on a region of your plot, you first need to grab the spatial extent of the object 

{:.input}
```python
# get spatial extent  - to zoom in on the map rather than clipping
bounds = sjer_aoi.geometry.total_bounds
bounds
```

{:.input}
```python
The `total_bounds` attribute represents the total spatial extent for the aoi layer. This is teh total external boundary of the layer - thus if there are multiple polygons in the layer it will take the furtherst edge in the north, south, east and west directions to create the spatial extent box. 

<figure>
    <a href="{{ site.baseurl }}/images/courses/earth-analytics/spatial-data/spatial-extent.png">
    <img src="{{ site.baseurl }}/images/courses/earth-analytics/spatial-data/spatial-extent.png" alt="the spatial extent represents the spatial area that a particular dataset covers."></a>
    <figcaption>The spatial extent of a shapefile or `Python` spatial object like a `geopandas` `geodataframe` represents
    the geographic "edge" or location that is the furthest north, south east and
    west. Thus is represents the overall geographic coverage of the spatial object.
    Image Source: National Ecological Observatory Network (NEON)
    </figcaption>
</figure>

The object that is returned is a tuple - a non editable object containing 4 values:
(xmin, ymin, xmax, ymax). If you want you can assign each value to a new variable as follows
```

{:.input}
```python
# create x and y min and max objects to use in the plot boundaries
xmin, xmax, ymin, ymax = sjer_aoi.total_bounds

#xmin, ymin, xmax, ymax = bounds[0:4]
#xmax
```

{:.input}
```python
fig, ax = plt.subplots(figsize  = (14, 6))
country_boundary_us.plot(alpha = .5, ax = ax)
ne_roads.plot(color='purple', ax=ax, alpha=.5)
ax.set(title='Natural Earth Global Roads Layer')
ax.set_axis_off()
plt.axis('equal');
```

You can set the x and **ylim**its of the plot using the x and y min and max values from your bounds object that you created above to zoom in your map. 

{:.input}
```python
# use the country boundary to set the min and max values for the plot
country_boundary_us.total_bounds
```

Notice in the plot below, you can still see roads that fall outside of the US Boundary area but are within the rectangular spatial extent of the boundary layer. Hopefully this helps you better understand the difference between clipping the data to a polygon shape vs simply plotting a small geographic region. 

{:.input}
```python
# plot the data with a modified spatial extent
fig, ax = plt.subplots(figsize = (10,6))
xlim = ([country_boundary_us.total_bounds[0],  country_boundary_us.total_bounds[2]])
ylim = ([country_boundary_us.total_bounds[1],  country_boundary_us.total_bounds[3]])

ax.set_xlim(xlim)
ax.set_ylim(ylim)

country_boundary_us.plot(alpha = .5, ax = ax)
ne_roads.plot(color='purple', ax=ax, alpha=.5)

ax.set(title='Natural Earth Global Roads \n Zoomed into the United States')
ax.set_axis_off();


```
