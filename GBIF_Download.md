## First you want to go to [GBIF](https://www.gbif.org/) and login or create an account

![Screenshot 2023-03-01 161433](https://user-images.githubusercontent.com/99222277/223763880-3de1b69a-90d1-476a-8270-09a2bd188573.png)


### Once your account is created return to the home page and search your species. 

![Screenshot 2023-03-01 162005](https://user-images.githubusercontent.com/99222277/223766381-ddb5d3f7-8ff9-4136-a4cd-29b7027652ca.png)


### When the next page loads click on the results which best fit your search



![Ery_GBIF](https://user-images.githubusercontent.com/99222277/223766412-c3d296ee-4bb6-440a-8e78-d47fe41ab73d.png)

### Click on the download tab towards the center of the screen

![Screenshot 2023-03-01 162706](https://user-images.githubusercontent.com/99222277/223766468-2e7229fa-d1d5-48e2-be83-a05b34174f88.png)

### Click Simple download and wait between 5- 30 mins for a email from GBIF. Simple data includes Lat and Long which will be used to map your speices

![Screenshot 2023-03-01 162202](https://user-images.githubusercontent.com/99222277/223766521-9122418e-4023-43ef-958a-dd718ed3ef36.png)

### With the data download you can now import the data into R! With  data downloaded try to call on the first 10 lines using:

```
head(data)

```
### You should notice that the data is very clutuered, this is beacuase R is recognizing the "," in the CSV as what seperates each data entry

### To fix this just run the following line (varible "species" can be term)

```

species <- read.csv("ENTER GBIF FILE HERE.csv", head=TRUE sep = "\t")

```
Your data should be seperated now, to check this run:

```
head(species) 

```

If you want to now map your speices please follow this [Tutorial](https://github.com/BotanyCoder/CFP_Mapping/tree/main/blank_code)
