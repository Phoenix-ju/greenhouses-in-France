# Greenhouses-in-France
## Goal
The primary objective of this study is to derive an inventory of greenhouses and farm structures in Provence Alpes-Cote-d'Azure (***PACA***), France using remote sensing data and machine learning techniques. The study aims to train the algorithm to identify greenhouses in the area. 

Several data sources were used for the project. Their descriptions are provided below. In general, the data consists of satellite-derived information, which, after additional processing, will be utilized in a GIS program to identify greenhouses in the designated region. The use of multiple sources simultaneously is necessary for further result validation and for the outcomes accuracy increase.

**__Literature overview (to be continued..)__**
#### 1. [Object-Based Greenhouse Mapping Using Very High Resolution Satellite Data and Landsat 8 Time Series](https://www.mdpi.com/144558)
Remarks: Mapping greenhouses using remote sensing poses challenges due to the significant variations in the spectral signature of plastic-covered(or any other roof material)structures.

Methods: 

i. Segmentation. The segmentation process involved analyzing two different datasets: a time series of Landsat 8 images from 2014 and a WorldView-2 multispectral image from 2013. The images were geometrically and atmospherically corrected. A multi-resolution segmentation algorithm in eCognition software was used, considering factors such as homogeneity criteria, color and shape weights, and compactness and smoothness criteria. The best segmentation was determined through a supervised evaluation using the Euclidean Distance 2 (ED2) method, which takes into account both geometric and arithmetic discrepancies between the segmented objects and manually delineated reference polygons representing greenhouses.

ii. The features used for the object-based classification included spectral information (mean and standard deviation values), spectral and vegetation indices, Moment Distance Index (MDI) for analyzing the shape of reflectance spectra, GLCM texture features (homogeneity, dissimilarity, and entropy), and statistical seasonal features (maximum, minimum, difference, and average values). These features were derived from the L8 time series and WV2 single image datasets to carry out the object-based classification.

iii. Decision Tree Modeling and Classification Accuracy Assessment. Decision tree modeling algorithm was used for classification in this study. The decision tree classifier, implemented in eCognition, uses rule sets to determine feature thresholds and provides a clear and interpretable structure. The classification accuracy was assessed using both object-based ground truth datasets and pixel-based ground truth datasets obtained from manual digitization, allowing the evaluation of the classification performance and the determination of the relative importance of different features.

Overall, the MDI was found to be the most important feature for greenhouse detection, and a stable threshold value was identified. The proposed approach achieved high overall accuracies of 93.0% and 93.3% for the 2014 and 2015 datasets, respectively.

2. [Evaluation of different classification techniques for the detection of glass and plastic greenhouses from WorldView-2 satellite imagery](https://www.spiedigitallibrary.org/journals/journal-of-applied-remote-sensing/volume-7/issue-1/073553/Evaluation-of-different-classification-techniques-for-the-detection-of-glass/10.1117/1.JRS.7.073553.short)
3. [Object-based classification approach for greenhouse mapping using Landsat-8 imagery](http://www.ijabe.org/index.php/ijabe/article/view/1414)
4. [Liu, Jian Guo, and Philippa J. Mason. Essential image processing and GIS for remote sensing. John Wiley & Sons, 2013.](https://books.google.de/books?hl=en&lr=&id=a9_CLn4FWEMC&oi=fnd&pg=PT13&ots=M6JDtXOypP&sig=SpAGtOUkVEul2MqiWBxo_wmbc4A&redir_esc=y#v=onepage&q&f=false)




### Data collection. Source 1: 
* [OpenStreetMap (***OSM***)](https://download.geofabrik.de/europe/france.html) for Provence Alpes-Cote-d'Azur, France.
The meta data derived from shape files in GIS contains information about the buildings *(3 471 087 records )* located in this region of France.

### Attributes:
1. ***OSM ID*** - building identifier 
2. ***CODE*** and ***FCLASS*** - remains the same for all records
3. ***NAME*** - the type of building, its name, or its location (address/region) is described in French, additionally many designations are missing (NULL)
4. ***TYPE*** - the most important value among which following are encountered:

parking, roof, ruins, office, commercial, retail, public, apartments etc. including those relevant for the study such as greenhouse, glasshouse, farm, farm auxilliary.

![Attribute table.png](https://github.com/Phoenix-ju/greenhouses-in-France/blob/main/Attribute%20table.png)

Out of the ***3 471 087*** rows, only ***6 089*** are labeled with a specific building type. The remaining ***3 464 998*** IDs are missing and marked as NULL. 

#### A brief GIS data analysis.
Using GIS (filter tool), the data was sorted, and new shapefiles were created based on the available information about greenhouses. 
Categories such as farms, roofs, and glass houses have the potential to be greenhouses as well. Separate shapefiles were created for these categories. From the generated maps, it is visually evident that not all buildings are labeled as greenhouses. Considering the lack of data for a significant number (***3 464 998***) of entries, it is necessary to establish a specific classification that can help identify missing greenhouses based on patterns.

In the PACA region, there are a total of 2706 greenhouses, among which 718 belong to the Prealps de province. The remaining greenhouses, glass houses, and farms are scattered across other provinces in the region. Additionally, in the Prealpes de Provence, the greenhouses are densely clustered, which potentially aids in accurately identifying their spectral signature for the detection of unmarked greenhouses.

It is evident that a large number of greenhouses are not labeled due to the absence of data in the "type" category. However, based on the available data, the following conclusions can be drawn:

* ***The majority of relevant data is marked in the south, specifically in the Prealpes de Provence region, therefore, this region will be used for further research.***
* The number of greenhouses is 718.
* The number of glasshouses is 15. (they look pretty simillar to greenhouses)
* The number of farmland is 1104. (it is important due to presence of the houses, which are simillar to greenhouses on this land)
* The number of farmyard is 87. (the same as above)
![Screenshot (20)](https://github.com/Phoenix-ju/greenhouses-in-France/blob/main/Province%20Cote%20d%20Azur.png)

![Prealpes de Province map from QGIS](https://github.com/Phoenix-ju/greenhouses-in-France/blob/main/Prealpes%20de%20province.png)

##### Data collection. Source 2.
* [Farm boundary and field parcel data from Registre parcellaire graphique (Graphic parcel register, France)](https://geoservices.ign.fr/telechargement)

From this file, two shapefiles are extracted for QGIS (ILOTS_ANONYMES & PARCELLES_GRAPHIQUES), containing information about the graphic data of parcels (since 2015) and islets (2014 and earlier editions) with their main crop. 

These files were also cropped for the Préalpes de Provence province in the south of France to facilitate faster data processing in GIS.

###### Data collection. Source 3.
* [Building Footprint from MS Satellite](https://github.com/microsoft/GlobalMLBuildingFootprints)

Among this data, building footprints were selected from three satellite imagies (3 squares) covering the southern region of Préalpes de Provence and including the whole Côte d'Azur region in France. The files were converted to the required format (from .csv.gz to GeoJSON) for further processing in GIS.
([code for json conversion](https://github.com/microsoft/GlobalMLBuildingFootprints/blob/main/scripts/make-gis-friendly.py))

###### The next step in GIS after data processing.
From the OSM data, using the Automatic Classification plugin, we extract greenhouse samples. Once we have identified the greenhouse samples, we intersect them with "ILOTS_ANONYMES & PARCELLES_GRAPHIQUES". Then, we overlay the building footprints on the intersected areas to identify buildings that overlap with the "ilots parcelles" fields. From these buildings, we select the ones that match the greenhouse criteria based on spectral assessment from OSM.

###### Greenhouse signature identification in GIS Automatic classification plugin. (will be written here)
















