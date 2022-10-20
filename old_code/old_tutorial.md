
# CFP_Mapping

## Set working directory 
Setting up your workplace is important when it comes to organization and accessing necessary files. 
setwd() will set your working director

**Example: setwd("C:/Users/Cam/Desktop/BOT_499")**
```
setwd("~/CFP_Mapping") 
```
getwd() will show you which directory you are currently using
```
getwd()
```
## Packages
To install the package we neet to type:
```
install.packages("dpylr")
install.packages("purrr")
install.packages("readr")
install.packages("mapproj")
install.packages("raster")
install.packages("elevatr")
install.packages("rgdal")
install.packages("ggplot2")
install.packages("ggmap")
install.packages("CoordinateCleaner")
install.packages("rnaturalearthdata")
install.packages("rangeBuilder")
install.packages("leaflet")
```
## Load Library
```
library(dplyr)
library(purrr)
library(readr)  
library(mapproj)
library(raster)
library(elevatr)
library(rgdal)
library(ggplot2)
library(ggmap)
library(CoordinateCleaner)
library(rnaturalearth)
library(rangeBuilder)
library(leaflet)
```
### If you ran into an issue with the package "CoordinateCleaner" and "terra" not being update, follow these steps.
The error seems to occur due to a older verison of "terra" being installed.
<details><summary> CLCK ME </summary>
  <p>

**Step 1.** Download and install [RTools](https://cran.r-project.org/bin/windows/Rtools/rtools40.html)

**Step 2.** Create a txt. file named " .Renviron ".

**Step 3.** Save the file to your documents.

**Step 4.** Restart R 


![Rest_R](https://user-images.githubusercontent.com/99222277/153778610-77351921-c65c-48a7-bbe5-70b3447fb129.png)


**Step 5.** Run the following lines in order and one at a time. This may take several minutes.
```
write('PATH="${RTOOLS40_HOME}\\usr\\bin;${PATH}"', file = "~/.Renviron", append = TRUE)
Sys.which("make")
install.packages("terra", type = "source")
```
This should have updated your "terra" packages, which we can check by loading the packages
```
library(CoordinateCleaner)
```
The package should be updated and no error message should appear
    
  </p>
  </details>



# Beginning of individual spp cleaning
The name of the file may vary, if so just replace "krameria_erecta_geo.csv" with the name of the file located on your computer.

*Step 1.*  Check list of species for a speices which has not be used already

[Species list](https://docs.google.com/spreadsheets/d/1b-dYWAtMH2d__rlOIhvbReTYXqSH9xj67lmR0LMz5B4/edit#gid=1238335591)

*Step 2.* Sign name next to species


*Step 3.* Download Occurences data set

[Species Occurrences](https://drive.google.com/drive/u/0/folders/1syFvdxNsxo3eVK3BVlx5bYX7WoynWeYp)
```
data <- read.csv("Genus_species.csv") 
```
Set Data as an varible
Genus_species <- data
## Plotting out uncleaned data to get coordinate frame
```
plot(Genus_species$decimalLongitude, Genus_species$decimalLatitude)
```
## Creating a initial base map 
Now we can set our basemap along with the bounding box, the following figure shows how the bounding box is set
![smaller_xy](https://user-images.githubusercontent.com/99222277/153784782-0c2c3247-f20d-4d8b-8191-0742a47a721f.png)

We suggest changing the values to encompass the range of the species and its proximity to the CFP.


```
basemap <-  get_map(location = c(-140, -60, -32, 60), zoom = 3)
ggmap(basemap)
```
Plot data over basemap of CFP

**Example:(ggmap(basemap) + geom_point(data = Krameria_erecta_2, aes(x=decimalLongitude, y=decimalLatitude, color=species))**

```
ggmap(basemap) + geom_point(data = Genus_speies_2, aes(x=decimalLongitude, y=decimalLatitude, color=species))
```
## Building second base map of average range of data.
Changing the bounding box may be neccesary to encompass a plant species distrubtion.

Ploting the data on to the Basemap
```
ggmap(basemap2) + geom_point(data = Krameria_erecta_2, aes(x=decimalLongitude, y=decimalLatitude, color=species))
```
## Beginning of coordinate cleaning

When are are looking at a plant disturbiton we should reference a site ejepson or eflora to gain a better understanding of the plant species in questions distrubtion.

[Jepson](https://ucjeps.berkeley.edu/eflora/)

[eFlora](http://www.efloras.org/)


Example: 
![Example_Flagged](https://user-images.githubusercontent.com/99222277/153942636-3dc69b93-e386-4b8b-9015-d2736e7deb8c.png)

<details><summary> How coordinate cleaning works </summary>
  <p>
    outliers_mtp, seems to determine how far a data point can be away from the majority of the data set and still count as being "in range"
    For instance and "mtp" value of 5 means that a point with 5 kilometers will still be considered apart of the distubtion.
    When we lower the value of "mtp" we lower what coordinate cleaner will consider within range.
    
    
   ![MTP_Guide](https://user-images.githubusercontent.com/99222277/155865824-0b2b5ffe-8b84-4b52-a7c2-cd247696cc70.png)

    
  </p>  
  </details>

```
Genus_species_example <- clean_coordinates(x = Genus_species, 
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
```
Removing flagged occurence
<details><summary> How to adjust coordinate cleaner </summary>
  <p>
  Outliers_method = "quantile"'
  
  outliers_mtp seems to be the easiest way to adjust what the coordnaites, as we lower the value of "5" which seems to represent 5 kilometers (5,000 meters), as we lower this value outliers which are at the edge of our data beging to be removed
  
  add photos, digrams ect
  
  
  </p>
  </details>

Isolate the flagged obs. (need to figure what these lines do, lines in skelton key)
```
eample_dat_fl <- example[!flags_example$.summary,]
```
Exclude flagged obs.
```
example_dat_cl <- example[flags_example$.summary,]
```
Plotting data with flags removed over basemap2
```
ggmap(basemap2) + geom_point(data = Genus_species_example, aes(x=decimalLongitude, y=decimalLatitude, color=species, size = 9))
```
**More filtering if Neccesary**

    
```
example_1 <- example %>% filter(decimalLatitude > example_LPLat, decimalLatitude < example_UPLat)
LPLon <- quantile(example_1$decimalLongitude, c(0.005))
UPLon <- quantile(example_1$decimalLongitude, c(0.995))
example_2 <- example_1 %>% filter(decimalLongitude < example_UPLon, decimalLongitude > example_LPLon)
```


    
    
Opitional Download
```
ggsave(filename = "Genus_species_distribuition.pdf")
```
    
# Beginning on Polygon work
(short explaination of why we are creating polygon/ shapefiles)

```
Genus_species_Poly_1 <- getDynamicAlphaHull(Genus_species_dat_cl, fraction = 0.95, partCount = 4, buff = 10000, initialAlpha = 3,
                                    coordHeaders = c('decimalLongitude', 'decimalLatitude'), clipToCoast = 'terrestrial',
                                    proj = "+proj=longlat +datum=WGS84", alphaIncrement = 1, verbose = TRUE)
```
Step 1. Plot polygon  

```
plot(Genus_species_Poly_1[[1]], col=transparentColor('dark green', 0.5), border = NA) 
```
Step 2. Plot data points onto polygon
```
points(Genus_species_dat_cl[,c('decimalLongitude','decimalLatitude')], cex = 0.5, pch = 3)
```

Step 3. Calculate the area of our species distribution in Kilometers
```
Genus_species_Area <- area(Genus_species_Poly_1[[1]]) /1000000
Genus_species_Area

capture.output(Genus_species_Area, file = "Genus_species_Area")
```

Step 4. View the polygon on top of an interactive map using Leaflet
```
PolygonMap <- leaflet()
PolygonMap <- addTiles(PolygonMap)
PolygonMap <- addPolygons(PolygonMap, data = Krameria_erecta_Poly_1[[1]])
PolygonMap
```
Step 5.

```
save(Krameria_erecta_Poly_1,file = "Krameria_erecta_Poly.Rdata")
```
### Uploading the data to Google Drive

*Step 1.* Create folder named after your species code

*Step 2.* Add polygon, area txt file and R script used

*Step 3.* Add area calcualted to Google Sheets and add additional comments if needed.

