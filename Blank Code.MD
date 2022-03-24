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

Taxon_Keys <- read.csv("Genus species.csv", sep = "\t")

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

Genus_species<- geodata2 %>% filter(species == "Genus species")

plot(Genus_species$decimalLongitude, Genus_species$decimalLatitude)

basemap <-  get_map(location = c(-140, -60, -32, 60), zoom = 3)
ggmap(basemap)

ggmap(basemap) + geom_point(data = Genus_species, aes(x=decimalLongitude, y=decimalLatitude, color=species))


basemap2 <-  get_map(location = c(-125, 20, -100, 40), zoom =5)
ggmap(basemap2)


Genus_species_LPLat <- quantile(Genus_species$decimalLatitude, c(0.005))
Genus_species_UPLat <- quantile(Genus_species$decimalLatitude, c(0.995))
Genus_species_1 <- Genus_species %>% filter(decimalLatitude > Genus_species_LPLat, decimalLatitude < Genus_species_UPLat)

Genus_species_LPLon <- quantile(Genus_species_1$decimalLongitude, c(0.005))
Genus_species_UPLon <- quantile(Genus_species_1$decimalLongitude, c(0.995))
Genus_species_2 <- Genus_species_1 %>% filter(decimalLongitude < Genus_species_UPLon, decimalLongitude > Genus_species_LPLon)

ggmap(basemap2) + geom_point(data = Genus_species_2, aes(x=decimalLongitude, y=decimalLatitude, color=species))

flags_Genus_erecta <- clean_coordinates(x = Genus_species, 
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

Genus_species_dat_fl <- Genus_species[!flags_Genus_species$.summary,]

Genus_species_dat_cl <- Genus_species[flags_Genus_species$.summary,]

plot(flags_Genus_species, lon = "decimalLongitude", lat = "decimalLatitude")

ggmap(basemap2) + geom_point(data = Genus_species_dat_cl, aes(x=decimalLongitude, y=decimalLatitude, color=species))

ggsave(filename = "Genus_species_distribuition.pdf")

Genus_species_Poly_1 <- getDynamicAlphaHull(Genus_species_dat_cl, fraction = 0.95, partCount = 4, buff = 10000, initialAlpha = 3,
                                    coordHeaders = c('decimalLongitude', 'decimalLatitude'), clipToCoast = 'terrestrial',
                                    proj = "+proj=longlat +datum=WGS84", alphaIncrement = 1, verbose = TRUE)

plot(Genus_species_Poly_1[[1]], col=transparentColor('dark green', 0.5), border = NA) 

points(Genus_species_dat_cl[,c('decimalLongitude','decimalLatitude')], cex = 0.5, pch = 3)

save(Genus_species_Poly_1, file = "Genus_species.Rdata")

library(leaflet)
PolygonMap <- leaflet()
PolygonMap <- addTiles(PolygonMap)
PolygonMap <- addPolygons(PolygonMap, data = Genus_species_Poly_1[[1]])
PolygonMap

Genus_species_Area <- area(Genus_species_Poly_1[[1]]) /1000000
Genus_species_Area
capture.output(Genus_species_Area, file = "Genus_species_area.txt")
```
## Step 2: Set working directory and load packages
## Step 3: Change "Genus_species" variables into your species and load your species dataset
#### Google Drive > Shared Drives > CFP_maps > Occurences by species > Download your species.csv
## Step 4: Run code and save the following outputs
###         -code for your species (.R)
###         -polygon (.Rdata)
###         -text file of area (.txt)
## Step 5: Upload all outputs to the Shared Drive
#### Google Drive > Shared Drives > CFP_maps > Outputs (code, polygon, text file) > *create* "Your Species" folder > upload outputs
