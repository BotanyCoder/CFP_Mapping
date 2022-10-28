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
data <- read.csv("SpeciesData.csv")
head(data)
Example <- data %>% filter(!is.na(decimalLatitude)) %>% filter(!is.na(decimalLongitude))

#Create a preliminary plot of the data
plot(Example$decimalLongitude, Example$decimalLatitude)

#Create an interactive map for points
PointMap <- leaflet()
PointMap <- addTiles(PointMap)
PointMap <- addCircleMarkers(PointMap, lng=Example$decimalLongitude, lat = Example$decimalLatitude)
PointMap

#This section will filter data that does not meet the parameters mentioned below, changing the value of outliers_mtp = 5, increases the value will encompass more data increase your area while decreasing the values will decrease your distribution area
#If your data is being unnecessarily removed, increase mtp or change outlier_mtp to NULL and work with outliers_td
report_Example <- clean_coordinates(x = Example, 
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
flags_Example <- Example[!report_Example$.summary,]

#Exclude flagged occurrences
clean_Example <- Example[report_Example$.summary,]

#Plot the flagged data set as well as the non-flagged species
plot(report_Example, lon = "decimalLongitude", lat = "decimalLatitude")

#Plot the filtered plant distribution 
PointMap2 <- leaflet()
PointMap2 <- addTiles(PointMap2)
PointMap2 <- addCircleMarkers(PointMap2, lng=clean_Example$decimalLongitude, lat = clean_Example$decimalLatitude, color= 'red')
PointMap2

#If you need to do "manual cleaning" you can use dplyr to remove specific points from your dataset
#Just remove the # from the line and add the necessary parameters, once completed repeat creating PointMap2 to check that the point was removed
#For removing points based only on latitude:
#clean_Example <- clean_Example %>% filter(decimalLatitude < 1)

#For removing points based only on longitude:
#clean_Example <- clean_Example %>% filter(decimalLongitude < 1)

# save clean dataset
write.csv(clean_Example, file = "SpeciesCode_Clean.csv")

# IMPORTANT: make sure the output match the imputs

#Create a polygon of the total plant distribution
Example_Poly_1 <- getDynamicAlphaHull(clean_Example, fraction = 0.95, partCount = 4, buff = 1000, initialAlpha = 3,
                                      coordHeaders = c('decimalLongitude', 'decimalLatitude'), clipToCoast = 'terrestrial',
                                      proj = "+proj=longlat +datum=WGS84", alphaIncrement = 1, alphaCap = 100, verbose = TRUE)

#Create an interactive map
PolygonMap <- leaflet()
PolygonMap <- addTiles(PolygonMap)
PolygonMap <- addPolygons(PolygonMap, data = Example_Poly_1[[1]])
PolygonMap <- addCircleMarkers(PolygonMap, lng=clean_Example$decimalLongitude, lat = clean_Example$decimalLatitude,color= 'red')
PolygonMap

#Save the polygon
save(Example_Poly_1, file = "Example_poly.Rdata")

#Calculate the area of the polygon
Example_Area <- area(Example_Poly_1[[1]]) /1000000
Example_Area
#Saves the area calculated as a text file
capture.output(Example_Area, file = "Example_area.txt")

```
## Step 1: Copy and paste the section of code above.
####  Go to: Google Drive > Shared Drives > CFP_maps > Occurences by species > Download your intended species
## Step 2: Change EXAMPLE variables into your species code name (first 2 letters of genus plus first two letters of species e.i. gesp) and load your species dataset
#### Variables that say "Genus species" should be replaced with the name of your species in this same format, you can use find and replace to replace EXAMPLE quickly
## Step 3: Run code up to the filtering step
#### Check the Jepson and Flora of North America to make sure the filter is appropiate. Usually species with lots of ocurrences need the filtering process while species with only a handful can skip the filtering process.

[Jepson](https://ucjeps.berkeley.edu/eflora/)

[eFlora](http://www.efloras.org/)

## Step 4: save the following outputs
####        -code for your species (code.R)
####        -polygon (code_poly.Rdata)
####        -text file of area (code.txt)
####        -filtered georeferenced data (code_clean.csv) 
## Step 5: Upload all outputs to the Shared Drive
#### Google Drive > Shared Drives > CFP_maps > Outputs (code, polygon, text file) > *create* "Your Species" folder > upload outputs
