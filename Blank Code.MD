## Step 1: Load entire code into RStudio

```
setwd("~/CFP_Mapping") 
dir()

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

Taxon_Keys <- read.csv("0000436-210914110416597.csv", sep = "\t")

head(Taxon_Keys)

Taxon_Keys_Spp <- c(Taxon_Keys$species)
Taxon_Keys_Spp_Tally <- Taxon_Keys_Spp %>% group_by(species) %>% tally() 

Taxon_Keys_Species_List <- Taxon_Keys %>% group_by(species) %>% tally()

geodata <- Taxon_Keys %>% filter(!is.na(decimalLatitude)) 

geodata2 <- geodata %>% filter(!is.na(decimalLongitude))

geodata2_Species_List <- geodata2 %>% group_by(species) %>% tally()

geodata2$decimalLatitude
plot(geodata$decimalLatitude)
hist(geodata$decimalLatitude, breaks=60)

NLat <- geodata2 %>% filter(decimalLatitude < 1) 

Species_NLat <- NLat %>% group_by(species) %>% tally()

plot(geodata2$decimalLongitude, geodata2$decimalLatitude)

Krameria_erecta<- geodata2 %>% filter(species == "Krameria erecta")

plot(Krameria_erecta$decimalLongitude, Krameria_erecta$decimalLatitude)

basemap <-  get_map(location = c(-140, -60, -32, 60), zoom = 3)
ggmap(basemap)

ggmap(basemap) + geom_point(data = Krameria_erecta, aes(x=decimalLongitude, y=decimalLatitude, color=species))


basemap2 <-  get_map(location = c(-125, 20, -100, 40), zoom =5)
ggmap(basemap2)


Krameria_erecta_LPLat <- quantile(Krameria_erecta$decimalLatitude, c(0.005))
Krameria_erecta_UPLat <- quantile(Krameria_erecta$decimalLatitude, c(0.995))
Krameria_erecta_1 <- Krameria_erecta %>% filter(decimalLatitude > Krameria_erecta_LPLat, decimalLatitude < Krameria_erecta_UPLat)

Krameria_erecta_LPLon <- quantile(Krameria_erecta_1$decimalLongitude, c(0.005))
Krameria_erecta_UPLon <- quantile(Krameria_erecta_1$decimalLongitude, c(0.995))
Krameria_erecta_2 <- Krameria_erecta_1 %>% filter(decimalLongitude < Krameria_erecta_UPLon, decimalLongitude > Krameria_erecta_LPLon)

ggmap(basemap2) + geom_point(data = Krameria_erecta_2, aes(x=decimalLongitude, y=decimalLatitude, color=species))

flags_Krameria_erecta <- clean_coordinates(x = Krameria_erecta, 
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
Krameria_erecta_dat_fl <- Krameria_erecta[!flags_Krameria_erecta$.summary,]

Krameria_erecta_dat_cl <- Krameria_erecta[flags_Krameria_erecta$.summary,]

plot(flags_Krameria_erecta, lon = "decimalLongitude", lat = "decimalLatitude")

ggmap(basemap2) + geom_point(data = Krameria_erecta_dat_cl, aes(x=decimalLongitude, y=decimalLatitude, color=species))

ggsave(filename = "Krameria_erecta_distribuition.pdf")

Krameria_erecta_Poly_1 <- getDynamicAlphaHull(Krameria_erecta_dat_cl, fraction = 0.95, partCount = 4, buff = 10000, initialAlpha = 3,
                                    coordHeaders = c('decimalLongitude', 'decimalLatitude'), clipToCoast = 'terrestrial',
                                    proj = "+proj=longlat +datum=WGS84", alphaIncrement = 1, verbose = TRUE)

plot(Krameria_erecta_Poly_1[[1]], col=transparentColor('dark green', 0.5), border = NA) 

points(Krameria_erecta_dat_cl[,c('decimalLongitude','decimalLatitude')], cex = 0.5, pch = 3)

library(rgdal)
writeOGR(Krameria_erecta_Poly_1, dsn = '.', layer = 'poly', driver = "ESRI Shapefile")
library(sp)
save(Krameria_erecta_Poly_1, file = "Krameria_erecta.Rdata")

library(leaflet)
PolygonMap <- leaflet()
PolygonMap <- addTiles(PolygonMap)
PolygonMap <- addPolygons(PolygonMap, data = Krameria_erecta_Poly_1[[1]])
PolygonMap

ggmap(basemap2) + geom_polygon(data = Krameria_erecta_Poly_1[[1]])

st_write(Classmaps01, "Krameria_erecta.shp")

Krameria_erecta_Area <- area(Krameria_erecta_Poly_1[[1]]) /1000000
Krameria_erecta_Area
capture.output(Krameria_erecta_Area, file = "Krameria_erecta_area")
```
## Step 2: Set working directory and load packages
## Step 3: Change example "Genus_species" variables into your species and load your species dataset
### Google Drive > Shared Drives > CFP_maps > Occurences by species > Download your species.csv
## Step 4: Run code and save the following outputs
###         -code for your species (.R)
###         -polygon (.Rdata)
###         -text file of area (.txt)
## Step 5: Upload all outputs to the Shared Drive
### Google Drive > Shared Drives > CFP_maps > Outputs (code, polygon, text file) > *create* "Your Species" folder > upload outputs
