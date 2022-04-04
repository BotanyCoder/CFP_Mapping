## Step 1: Load entire code into RStudio

```
setwd("~/CFP_Mapping") 
getwd()

#install.packages("dpylr")
#install.packages("purrr")
#install.packages("readr")
#install.packages("mapproj")
#install.packages("raster")
#install.packages("elevatr")
#install.packages("rgdal")
#install.packages("ggplot2")
#install.packages("ggmap")
#install.package("CoordinateCleaner")
#install.packages("rnaturalearthdata")
#install.packages("rangeBuilder")
#install.packages("leaflet")


library(dplyr)
library(purrr)
library(readr)  
library(magrittr)
library(mapproj)
library(raster)
library(elevatr)
library(rgdal)
library(ggplot2)
library(ggmap)
library(CoordinateCleaner)
library(rnaturalearth)
library(rangeBuilder)
library(rlang)

#Import data
data <- read.csv("Genus species.csv")
head(data)
EXAMPLE <- data


#Create a preliminary plot of the data
plot(EXAMPLE$decimalLongitude, EXAMPLE$decimalLatitude)

#Create a large base map showing the entirety of North America
basemap <-  get_map(location = c(-140, -60, -32, 60), zoom = 3)
ggmap(basemap)
ggmap(basemap) + geom_point(data = EXAMPLE, aes(x=decimalLongitude, y=decimalLatitude, color=species))

#Create a secondary map, we recommend changing the values (-140, 20, -32, 60), zoom=3 to better fit your plant distribution 
basemap2 <-  get_map(location = c(-140, 20, -40, 60), zoom =3)
ggmap(basemap2) + geom_point(data = EXAMPLE, aes(x=decimalLongitude, y=decimalLatitude, color=species))

#This section will filter data that does not meet the parameters mentioned below, changing the value of outliers_mtp = 5, increases the value will encompass more data increase your area while decreasing the values will decrease your distribution area
flags_EXAMPLE <- clean_coordinates(x = EXAMPLE, 
                                        lon = "decimalLongitude", 
                                        lat = "decimalLatitude",
                                        countries = "countryCode",
                                        species = "species",
                                        tests = c("capitals", "centroids", "equal","gbif", "institutions",
                                                  "zeros", "outliers","seas"),
                                        outliers_method = "quantile",
                                        outliers_mtp = 5,
                                        outliers_td = 60,
                                        outliers_size = 100)

#Isolate flagged occurrences

EXAMPLE_dat_fl <- EXAMPLE[!flags_EXAMPLE$.summary,]
#Exclude flagged occurrences

EXAMPLE_dat_cl <- EXAMPLE[flags_EXAMPLE$.summary,]

#Plot the flagged data set as well as the non-flagged species

#Plot the filtered plant distribution 
plot(flags_EXAMPLE, lon = "decimalLongitude", lat = "decimalLatitude")
ggmap(basemap2) + geom_point(data = EXAMPLE_dat_cl, aes(x=decimalLongitude, y=decimalLatitude, color=species))

#(Opitional saves data as a pdf)
#ggsave(filename = "EXAMPLE_distribuition.pdf")

#Create a polygon of the total plant distribution
EXAMPLE_Poly_1 <- getDynamicAlphaHull(EXAMPLE_dat_cl, fraction = 0.95, partCount = 4, buff = 10000, initialAlpha = 3,
                                    coordHeaders = c('decimalLongitude', 'decimalLatitude'), clipToCoast = 'terrestrial',
                                    proj = "+proj=longlat +datum=WGS84", alphaIncrement = 1, verbose = TRUE)

#Plot the polygon
plot(EXAMPLE_Poly_1[[1]], col=transparentColor('dark green', 0.5), border = NA) 

#Add points to polygon for a visual representation of occurrences 
points(EXAMPLE_dat_cl[,c('decimalLongitude','decimalLatitude')], cex = 0.5, pch = 3)

#Save the polygon
save(EXAMPLE_Poly_1, file = "EXAMPLE_poly.Rdata")

#Create an interactive map
library(leaflet)
PolygonMap <- leaflet()
PolygonMap <- addTiles(PolygonMap)
PolygonMap <- addPolygons(PolygonMap, data = EXAMPLE_Poly_1[[1]])
PolygonMap

#Calculate the area of the polygon
EXAMPLE_Area <- area(EXAMPLE_Poly_1[[1]]) /1000000
EXAMPLE_Area
#Saves the area calcuated as a text file
capture.output(EXAMPLE_Area, file = "EXAMPLE_area.txt")
```
## Step 2: Set working directory and load packages
## Step 3: Change EXAMPLE variables into your species code name (first 2 letters of genus plus first two letters of species e.i. gesp) and load your species dataset
#### Variables that say "Genus species" should be replaced with the name of your species in this same format, you can use find and replace to replace EXAMPLE quickly
#### Google Drive > Shared Drives > CFP_maps > Occurences by species > Download your species.csv
## Step 4: Run code up to the filtering step, check the Jepson and Flora of North America to make sure the filter is appropiate. Usually species with lots of ocurrences need the filtering process while species with only a handful can skip the filtering process.

[Jepson](https://ucjeps.berkeley.edu/eflora/)

[eFlora](http://www.efloras.org/)

## Step 5: save the following outputs
###         -code for your species (EXAMPLE_code.R)
###         -polygon (.Rdata)
###         -text file of area (.txt)
## Step 6: Upload all outputs to the Shared Drive
#### Google Drive > Shared Drives > CFP_maps > Outputs (code, polygon, text file) > *create* "Your Species" folder > upload outputs
