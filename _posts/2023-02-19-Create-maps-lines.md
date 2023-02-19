---
layout: post
title: Create maps and connect cities
tags: ["R coding","maps"]
comments: true
---

<h3 style="text-align: center;">How to draw connecting routes on a map in R</h3> 
<br><br>
  
In this tutorial, the goal is to create a map of the world, identify and pin our favorite cities in the world on the map and finally display connecting lines between them. Two packages are required, <font color="blue">maps</font> and <font color="blue">geosphere</font>. Let's go! <br><br>
  
    
<h4>Draw an empty map</h4>

---  
  

We can start by creating first an empty map of the world. For this we will use the package <font color="blue">maps</font>.<br> 

```r
# World map is available in the maps package
library(maps)


# World map
map('world',
    col="#f2f2f2", fill=TRUE, bg="white", lwd=0.05,
    mar=rep(0,4),border=0, ylim=c(-80,80) 
)
```

<p align="center">
<img width="60%" src="/static/empty_map.png" alt="Empty map">
</p>


<h4>Add the cities you want</h4>

---

Adding points to the map is easy. We need to obtain first the coordinates of the cities (longitute and lattitude). The package <font color="blue">maps</font> has this info for 43645 cities across the world. Since its possible that a city name can appear in multiple countries, its better to specify the country as well. <br>

```r
# here are the coordinates for 43645 cities across the world
world_cities_data = maps::world.cities

# choose the ones you want and get the longitute and lattitude
Athens = world_cities_data[world_cities_data$name=="Athens" & world_cities_data$country.etc=="Greece",c("long","lat")]
Sao_Paulo = world_cities_data[world_cities_data$name=="Sao Paulo" & world_cities_data$country.etc=="Brazil",c("long","lat")]
Tokyo = world_cities_data[world_cities_data$name=="Tokyo" & world_cities_data$country.etc=="Japan",c("long","lat")]

my_cities = rbind(Athens,Sao_Paulo,Tokyo)


# World map
map('world',
    col="#f2f2f2", fill=TRUE, bg="white", lwd=0.05,
    mar=rep(0,4),border=0, ylim=c(-80,80) )

# add the points in the city
points(x=my_cities$long, y=my_cities$lat, col="slateblue", cex=3, pch=20)
```

<p align="center">
<img width="60%" src="/static/map_points.png" alt="Map with points in the cities">
</p>


<h4>Connect the cities you want</h4>

---

In the last step we will connect the dots of the cities we want. For this we will use the package <font color="blue">geosphere</font>. <br>

```r
# Load geosphere
library(geosphere)

# World map
map('world',
    col="#f2f2f2", fill=TRUE, bg="white", lwd=0.05,
    mar=rep(0,4),border=0, ylim=c(-80,80) )

# add the points in the city
points(x=my_cities$long, y=my_cities$lat, col="slateblue", cex=3, pch=20)


# calculate the connection between Athes and Sao Paulo
inter <- gcIntermediate(Athens,  Sao_Paulo, n=50, addStartEnd=TRUE, breakAtDateLine=F)
lines(inter, col="slateblue", lwd=2)

# calculate the connection between Athes and Tokyo
inter <- gcIntermediate(Athens,  Tokyo, n=50, addStartEnd=TRUE, breakAtDateLine=F)
lines(inter, col="slateblue", lwd=2)
```


<p align="center">
<img width="60%" src="/static/map_points_lines.png" alt="Map with points in the cities and lines connecting them">
</p>


Thats it, we made it! We can always play around with the parameters of the package <font color="blue">maps</font>, to customize your map.<br>
