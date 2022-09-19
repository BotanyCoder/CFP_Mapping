```
setwd("/Users/....") 
getwd()

#install.packages("leaflet")
#install.packages("CoordinateCleaner")
#install.packages("dpylr")
#install.packages("rangeBuilder")
#install.packages("raster")

library(leaflet)
library(CoordinateCleaner)
library(dplyr)
library(rangeBuilder)
library(raster)

#Import data
data <- read.csv("Krameria_erecta_geo.csv")
head(data)
krameria <- data

#Create a preliminary plot of the data
plot(krameria$decimalLongitude, krameria$decimalLatitude)

#Create an interactive map for points
PointMap <- leaflet()
PointMap <- addTiles(PointMap)
PointMap <- addCircleMarkers(PointMap, lng=krameria$decimalLongitude, lat = krameria$decimalLatitude)
PointMap

#This section will filter data that does not meet the parameters mentioned below, changing the value of outliers_mtp = 5, increases the value will encompass more data increase your area while decreasing the values will decrease your distribution area
report_krameria <- clean_coordinates(x = krameria, 
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
flags_krameria <- krameria[!report_krameria$.summary,]

#Exclude flagged occurrences
clean_krameria <- krameria[report_krameria$.summary,]

#Plot the flagged data set as well as the non-flagged species
plot(report_krameria, lon = "decimalLongitude", lat = "decimalLatitude")

#Plot the filtered plant distribution 
PointMap2 <- leaflet()
PointMap2 <- addTiles(PointMap2)
PointMap2 <- addCircleMarkers(PointMap2, lng=clean_krameria$decimalLongitude, lat = clean_krameria$decimalLatitude, color= 'red')
PointMap2

# save clean dataset
write.csv(clean_krameria, file = "Krameria_erecta_geo_cl.csv")

#If you need to do "manual cleaning" you can use dplyr to remove specific points from your dataset
#Just remove the # from the line and add the necessary parameters
#For removing points based only on latitude:
#EXAMPLE_dat_cl2 <- EXAMPLE_dat_cl %>% filter(decimalLatitude < 1)

#For removing points based only on longitude:
#EXAMPLE_dat_cl2 <- EXAMPLE_dat_cl %>% filter(decimalLongitude < 1)

# IMPORTANT: if you use "manual filtering" replace EXAMPLE_dat_cl by EXAMPLE_dat_cl2 in the code below

#Create a polygon of the total plant distribution
krameria_Poly_1 <- getDynamicAlphaHull(clean_krameria, fraction = 0.95, partCount = 4, buff = 1000, initialAlpha = 3,
                                      coordHeaders = c('decimalLongitude', 'decimalLatitude'), clipToCoast = 'terrestrial',
                                      proj = "+proj=longlat +datum=WGS84", alphaIncrement = 1, alphaCap = 100, verbose = TRUE)

#Create an interactive map
PolygonMap <- leaflet()
PolygonMap <- addTiles(PolygonMap)
PolygonMap <- addPolygons(PolygonMap, data = krameria_Poly_1[[1]])
PolygonMap <- addCircleMarkers(PolygonMap, lng=clean_krameria$decimalLongitude, lat = clean_krameria$decimalLatitude,color= 'red')
PolygonMap

#Save the polygon
save(krameria_Poly_1, file = "Krameria_erecta_poly.Rdata")

#Calculate the area of the polygon
krameria_Area <- area(krameria_Poly_1[[1]]) /1000000
krameria_Area
#Saves the area calculated as a text file
capture.output(krameria_Area, file = "krameria_area.txt")

```
## Step 1: Copy asd paste the section of code above.
####  Go to: Google Drive > Shared Drives > CFP_maps > Occurences by species > Download your intended species
## Step 2: Change EXAMPLE variables into your species code name (first 2 letters of genus plus first two letters of species e.i. gesp) and load your species dataset
#### Variables that say "Genus species" should be replaced with the name of your species in this same format, you can use find and replace to replace EXAMPLE quickly
## Step 3: Run code up to the filtering step, check the Jepson and Flora of North America to make sure the filter is appropiate. Usually species with lots of ocurrences need the filtering process while species with only a handful can skip the filtering process.

[Jepson](https://ucjeps.berkeley.edu/eflora/)

[eFlora](http://www.efloras.org/)

## Step 4: save the following outputs
#           -code for your species (EXAMPLE_code.R)
#           -polygon (.Rdata)
#           -text file of area (.txt)
#           -filtered georeferenced data (.csv)
## Step 5: Upload all outputs to the Shared Drive
#### Google Drive > Shared Drives > CFP_maps > Outputs (code, polygon, text file) > *create* "Your Species" folder > upload outputs
