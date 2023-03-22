```
#### ==> SET PROPER DIRECTORY <==
# You can set your working directory by cliking on "session" and then "set working directory"
setwd("/Users/....") 
dir()

install.packages("rnaturalearth")

#install.packages("leaflet")
#install.packages("CoordinateCleaner")
#install.packages("dpylr")

library(leaflet)
library(CoordinateCleaner)
library(dplyr)

#Import data
#### ==> REPLACE FILE NAME <==
raw_data <- read.csv("SpeciesData.csv")
head(raw_data)

# remove data with out georreference
Example <- raw_data %>% filter(!is.na(decimalLatitude)) %>% filter(!is.na(decimalLongitude))

#Create a preliminary plot of the data
plot(Example$decimalLongitude, Example$decimalLatitude)

#Create an interactive map for points
PointMap <- leaflet()
PointMap <- addTiles(PointMap)
PointMap <- addCircleMarkers(PointMap, lng=Example$decimalLongitude, lat = Example$decimalLatitude)
PointMap

#### ==> LOOK CLOSELY AT THIS MAP <==
#### ==> DOES IT MAKES SENSE? <==
#### CALIFORNIA https://ucjeps.berkeley.edu/eflora/
#### USA http://www.efloras.org/
#### MEXICO https://efloramex.ib.unam.mx/

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

#### ==> CHECK THAT DISTRIBUTION MAKES SENSE <==
#### CALIFORNIA https://ucjeps.berkeley.edu/eflora/
#### USA http://www.efloras.org/
#### MEXICO https://efloramex.ib.unam.mx/

#If you need to do "manual cleaning" you can use dplyr to remove specific points from your dataset
#Just remove the # from the line and add the necessary parameters, once completed repeat creating PointMap2 to check that the point was removed
#For removing points based only on latitude:
#clean_Example <- clean_Example %>% filter(decimalLatitude < 1)

#For removing points based only on longitude:
#clean_Example <- clean_Example %>% filter(decimalLongitude < 1)

# save clean dataset
#### ==> REPLACE FILE NAME <==
write.csv(clean_Example, file = "SpeciesCode_Clean.csv")

# IMPORTANT: make sure the outputs make sense and are properly named

```
## Step 1: pick a species to work on, sign the `species list`, create a folder in your computer using its code
The species code (ex, KRER for Krameria_eracta) comprises the first two letters of the genus ans the fisrt two letters for the epithet. Your code should be used to name the outputs, for examle KRER.R and KRER.csv  
## Step 2: download the unfiltered dataset from the 'ocurrences by species' folder
Go to: Google Drive > Shared Drives > CFP_maps > Occurences by species > Download your intended species
## Step 3: Copy, paste, and save in a new file the code above. Make sure your working directory is properly set.
## Step 4: load your species dataset changing the name of the input file
## Step 5: Run code up to the filtering step
Check the Jepson and Flora of North America to make sure the filter is appropiate. Usually species with lots of ocurrences need the filtering process while species with only a handful can skip the filtering process.

- [Jepson](https://ucjeps.berkeley.edu/eflora/)

- [eFlora](http://www.efloras.org/)

- [efloramex] (https://efloramex.ib.unam.mx/)

## Step 6: save the following outputs
- code for your species (code.R)
- filtered georeferenced data (code_clean.csv)
## Step 7: Upload all outputs to the Shared Drive
#### Google Drive > Shared Drives > CFP_maps > Outputs (code, clean datset) > drag and drop your folder for an easy upload
