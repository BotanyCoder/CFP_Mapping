**Step 1.** Creating the polygon # doesnt work atm
```
krameria_Poly_1 <- getDynamicAlphaHull(clean_krameria, fraction = 0.95, partCount = 4, buff = 1000, initialAlpha = 3,
                                      coordHeaders = c('decimalLongitude', 'decimalLatitude'), clipToCoast = 'terrestrial',
                                      proj = "+proj=longlat +datum=WGS84", alphaIncrement = 1, alphaCap = 100, verbose = TRUE)
```
**Step 2.** Plot the polygon  
```
PolygonMap <- leaflet()
PolygonMap <- addTiles(PolygonMap)
```
**Step 3.** Plot data points onto polygon 
```
PolygonMap <- addPolygons(PolygonMap, data = krameria_Poly_1[[1]])
PolygonMap <- addCircleMarkers(PolygonMap, lng=clean_krameria$decimalLongitude, lat = clean_krameria$decimalLatitude)
PolygonMap
```
**Step 4.** Calculate the area of our species distribution in Kilometers and export calculated area
```
krameria_Area <- area(krameria_Poly_1[[1]]) /1000000
krameria_Area     

```
**Step.** 5 Save Area as .txt
```
capture.output(Krameria_erecta_Area, file = "Krameria_erecta_area")   
```

> Remove the flag from your laptop if you have finsihed this tutorial ![image](https://user-images.githubusercontent.com/99222277/154882595-b2448b1c-473f-4e83-9d72-1d401ebcb5e6.png)