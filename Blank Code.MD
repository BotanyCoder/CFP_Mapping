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

data <- read.csv("Genus species.csv")
head(data)
EXAMPLE <- data


plot(EXAMPLE$decimalLongitude, EXAMPLE$decimalLatitude)

basemap <-  get_map(location = c(-140, -60, -32, 60), zoom = 3)
ggmap(basemap)

ggmap(basemap) + geom_point(data = EXAMPLE, aes(x=decimalLongitude, y=decimalLatitude, color=species))


basemap2 <-  get_map(location = c(-140, 20, -40, 60), zoom =3)
ggmap(basemap2) + geom_point(data = EXAMPLE, aes(x=decimalLongitude, y=decimalLatitude, color=species))

flags_EXAMPLE <- clean_coordinates(x = EXAMPLE, 
                                        lon = "decimalLongitude", 
                                        lat = "decimalLatitude",
                                        countries = "countryCode",
                                        species = "species",
                                        tests = c("capitals", "centroids", "equal","gbif", "institutions",
                                                  "zeros", "outliers","seas"),
                                        outliers_method = "quantile",
                                        outliers_mtp = 2,
                                        outliers_td = 60,
                                        outliers_size = 100)

EXAMPLE_dat_fl <- EXAMPLE[!flags_EXAMPLE$.summary,]

EXAMPLE_dat_cl <- EXAMPLE[flags_EXAMPLE$.summary,]

plot(flags_EXAMPLE, lon = "decimalLongitude", lat = "decimalLatitude")

ggmap(basemap2) + geom_point(data = EXAMPLE_dat_cl, aes(x=decimalLongitude, y=decimalLatitude, color=species))

ggsave(filename = "EXAMPLE_distribuition.pdf")

EXAMPLE_Poly_1 <- getDynamicAlphaHull(EXAMPLE_dat_cl, fraction = 0.95, partCount = 4, buff = 10000, initialAlpha = 3,
                                    coordHeaders = c('decimalLongitude', 'decimalLatitude'), clipToCoast = 'terrestrial',
                                    proj = "+proj=longlat +datum=WGS84", alphaIncrement = 1, verbose = TRUE)

plot(EXAMPLE_Poly_1[[1]], col=transparentColor('dark green', 0.5), border = NA) 

points(EXAMPLE_dat_cl[,c('decimalLongitude','decimalLatitude')], cex = 0.5, pch = 3)

save(EXAMPLE_Poly_1, file = "EXAMPLE_poly.Rdata")

library(leaflet)
PolygonMap <- leaflet()
PolygonMap <- addTiles(PolygonMap)
PolygonMap <- addPolygons(PolygonMap, data = EXAMPLE_Poly_1[[1]])
PolygonMap

EXAMPLE_Area <- area(EXAMPLE_Poly_1[[1]]) /1000000
EXAMPLE_Area
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
