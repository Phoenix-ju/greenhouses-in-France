# Greenhouses-in-France
## Goal
The primary objective of this study is to derive an inventory of greenhouses and farm structures in Provence Alpes-Cote-d'Azure (***PACA***), France using remote sensing data and machine learning techniques. The study aims to train the algorithm to identify greenhouses in the area. 

Several data sources were used for the project. Their descriptions are provided below. In general, the data consists of satellite-derived information, which, after additional processing, will be utilized in a GIS program to identify greenhouses in the designated region. The use of multiple sources simultaneously is necessary for further result validation and for the increse of outcomes accuracy.

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

5. [A review of research on agrivoltaic systems](https://www.sciencedirect.com/science/article/abs/pii/S1364032122002635?via%3Dihub)
6. [High-spatiotemporal-resolution mapping of global urban change from 1985 to 2015](https://drive.google.com/file/d/1hADuFtGuqmRP1cgfFfVf9QrXjVxsk0pB/view)
7. [Mapping Plastic Greenhouses with Two-Temporal Sentinel-2 Images and 1D-CNN Deep Learning](https://www.mdpi.com/2072-4292/13/14/2820/htm)
A new fine- and coarse-scale mapping approach using two-temporal Sentinel-2 images with various seasonal characteristics and a one-dimensional convolutional neural network (1D-CNN) was proposed. Having applied this approach in a pilot area study, the results were summarized as follows: (1) A time-series analysis of Sentinel-2 images showed that the *reflectance of greenhouses changes during crop growth and development. In particular, the red-edge and near-infrared bands undergo a significant increase and then decrease during the whole crop growth period.* Thus, two critical period images, containing a substantial difference in greenhouse reflectance, were sufficient to carry out an accurate and efficient mapping result. (2) The 1D-CNN classifier was used to map greenhouses by capturing subtle details and the overall trend of the spectrum curve.
8. [Object Detection and Image Segmentation with Deep Learning on Earth Observation Data: A Review-Part I: Evolution and Recent Trends](https://www.mdpi.com/2072-4292/12/10/1667)


### Data collection. Source 1: 
* [OpenStreetMap (***OSM***)](https://download.geofabrik.de/europe/france.html) for Provence Alpes-Cote-d'Azur, France.
The meta data derived from shape files in GIS contains information about the buildings *(3 471 087 records )* located in this region of France.

### Attributes:
1. ***OSM ID*** - building identifier 
2. ***CODE*** and ***FCLASS*** - remains the same for all records
3. ***NAME*** - the type of building, its name, or its location (address/region) is described in French, additionally many designations are missing (NULL)
4. ***TYPE*** - the most important value among which following are encountered:

parking, roof, ruins, office, commercial, retail, public, apartments etc. including those relevant for the study such as greenhouse, glasshouse, farm, farm auxilliary.

![Attribute table.png](https://github.com/Phoenix-ju/greenhouses-in-France/blob/main/Screenshot/Attribute%20table.png)

Out of the ***3 471 087*** rows, only ***6 089*** are labeled with a specific building type. The remaining ***3 464 998*** IDs are missing and marked as NULL. 

#### A brief GIS data analysis.
Using GIS (filter tool), the data was sorted, and new shapefiles were created based on the available information about greenhouses. 
Categories such as farms, roofs, and glass houses have the potential to be greenhouses as well. Separate shapefiles were created for these categories. From the generated maps and metadata, it is evident that not all buildings are labeled as greenhouses. Considering the lack of data for a significant number (***3 464 998***) of entries, it is necessary to establish a specific classification that can help identify missing greenhouses based on patterns.

In the PACA region, there are a total of 2706 greenhouses, among which 718 belong to the Prealps de Provence. The remaining greenhouses, glass houses, and farms are scattered across other provinces in the region. Additionally, the greenhouses are densely clustered in the Prealpes de Provence, which potentially aids in accurately identifying their spectral signature for the detection of unmarked greenhouses.

It is evident that a large number of greenhouses are not labeled due to the absence of data in the "type" category. However, based on the available data, the following conclusions can be drawn:

* ***The majority of relevant data is marked in the south, specifically in the Prealpes de Provence region, therefore, this region will be used for further research.***
* The number of greenhouses is 718.
* The number of glasshouses is 15. (they look pretty simillar to greenhouses on Bing Satellite)
* The number of farmland is 1104. (it is important due to presence of the houses, which are simillar to greenhouses on this land)
* The number of farmyard is 87. (the same as above)
![Screenshot (20)](https://github.com/Phoenix-ju/greenhouses-in-France/blob/main/Screenshot/Province%20Cote%20d%20Azur.png)

![Prealpes de Provence map from QGIS](https://github.com/Phoenix-ju/greenhouses-in-France/blob/main/Screenshot/Prealpes%20de%20province.png)

##### Data collection. Source 2.
* [Farm boundary and field parcel data from Registre parcellaire graphique (Graphic parcel register, France)](https://geoservices.ign.fr/telechargement)

From this file, two shapefiles are extracted for QGIS (ILOTS_ANONYMES & PARCELLES_GRAPHIQUES), containing information about the graphic data of parcels (since 2015) and islets (2014 and earlier editions) with their main crop. 

These files were also cropped for the Préalpes de Provence in the south of France to facilitate faster data processing in GIS.

###### Data collection. Source 3.
* [Building Footprint from MS Satellite](https://github.com/microsoft/GlobalMLBuildingFootprints)

Among this data, building footprints were selected from three satellite imagies (3 squares) covering the whole southern region of PACA and Préalpes de Provence and including the whole Côte d'Azur region in France. The files were converted to the required format (from .csv.gz to GeoJSON) for further processing in GIS.
([code for json conversion](https://github.com/microsoft/GlobalMLBuildingFootprints/blob/main/scripts/make-gis-friendly.py))

###### The next step in GIS after data processing.
From the OSM data, using the Automatic Classification Plugin, greenhouse samples(GS) were extracted. Once GS have been identified, the intersection with "ILOTS_ANONYMES & PARCELLES_GRAPHIQUES" data was made. Then, i overlayed the building footprints on the intersected areas to identify buildings that overlap with the "ilots parcelles" fields. From these buildings i selected ones that match the greenhouse criteria based on spectral assessment from OSM.

###### [Mission Landsat 8-9 OLI/TIRS C2L2](https://earthexplorer.usgs.gov/)
Landsat 8 and Landsat 9 are satellite missions equipped with advanced sensors. OLI (Operational Land Imager) captures data in multiple spectral bands, including visible and near-infrared ranges, providing detailed imaging of Earth's surface. TIRS (Thermal Infrared Sensor) measures thermal emissions and surface temperature using two channels operating in the long-wave infrared range. These missions enable the study of environmental changes, land use, and temperature analysis.

* In this project, Landsat 8-9 images were used due to their distinct number of channels and detalization.
* To facilitate the identification of greenhouses, summer images were selected since they exhibit more vegetation. 
* Four images covering the Cote d'Azur region were chosen, with an additional criterion of cloud cover less than 10%. 
* Each image consists of 6 bands, and there was an accompanying mtl.text file that provides GIS information about the channels. 
* This results in 7 rasters that was mosaicked together so that each pixel contains information from all 7 bands. To accomplish this, preprocessing was conducted in GIS.
![Screenshot (31).png](https://github.com/Phoenix-ju/greenhouses-in-France/blob/main/Screenshot/Screenshot%20(31).png)

Each band in Landsat 8-9 images has a resolution of 30 meters and covers a different wavelength range. Here are the wavelength ranges for each band:
μm
* Band 1 (Coastal/Aerosol): 433 - 453 nanometers (two main uses: imaging shallow water, and tracking fine particles like dust&smoke, therefore band 1 might be not relevant)
* Band 2 (Blue): 450 - 515 nanometers
* Band 3 (Green): 525 - 600 nanometers
* Band 4 (Red): 630 - 680 nanometers
* Band 5 (Near-Infrared): 845 - 885 nanometers (its heat, which relevance is also debatable)
* Band 6 (Short-Wave Infrared 1): 156 - 166 nanometers (?)

###### Greenhouse signature identification in GIS Automatic classification plugin.

Only greenhouses from the OSM building data that belonged to the Prealps de Provence region were selected. Then, a spectral signature was assigned to the greenhouses and overlaid on the building footprints data, which represents all the buildings in the region. During the overlay, houses that matched the spectral signature indicative of greenhouses were displayed.

To select a spectral sample in GIS, there are following steps:

* Activate the ROI (Region of Interest) pointer in GIS software. This option helps identify areas with similar spectral characteristics.

* Choose a real greenhouse on the map that will be used as a sample. Mark it as a reference or standard sample.

* Apply this reference sample to the entire region of interes. The software will compare the spectral signatures of other areas with the reference sample.

* The resulting vector data can be analyzed further. Further, i intersect the vectorized result with the building footprints to extract the potential locations of greenhouses.

To obtain a precise classification, we identify materials, which are most commonly used for greenhouse roofs. Select the appropriate spectral signature for detection. To accomplish this:

* Determine the predominant roofing materials used in greenhouse construction. **Common materials include glass, polycarbonate, or plastic film**.

* Analyze the collected data to identify the most frequently used roofing material for greenhouses.

* Once the primary roofing material is identified, i search for the corresponding spectral signatures/spectral libraries. Many remote sensing or GIS software provide pre-defined spectral libraries that include various materials.

* Select the spectral signature that corresponds to the roofing material identified as most common. Use this spectral signature as a reference or standard for the spectral analysis to detect similar roofs in the region. 

*At the moment, i dentified areas with potential greenhouses and can extract their coordinates for further processing in Python. However, I have not done that yet because the preliminary results do not accurately reflect the greenhouse locations (for example, when compared to satellite imagery, many of the areas classified by the classifier do not even represent buildings). The classification needs to be modified.*

![Classification](https://github.com/Phoenix-ju/greenhouses-in-France/blob/main/Screenshot/Classification.png)

![Extract selected features](https://github.com/Phoenix-ju/greenhouses-in-France/blob/main/Screenshot/extract%20selected%20features.png)

![Extract by location](https://github.com/Phoenix-ju/greenhouses-in-France/blob/main/Screenshot/extraction%20by%20location.png)


[Importance of RGB in QGis for diffrent types of landuse](https://www.esri.com/arcgis-blog/products/product/imagery/band-combinations-for-landsat-8/)

1. Buildings ()
2. Roads ()
3. Greenhouses ()
4. Farms (6-5-2 or 4-5-6)


###### Spectral signatures

![Classification](https://github.com/Phoenix-ju/greenhouses-in-France/blob/main/Screenshot/signatures.png)

![Classification](https://github.com/Phoenix-ju/greenhouses-in-France/blob/main/Screenshot/sign1.png)

![Classification](https://github.com/Phoenix-ju/greenhouses-in-France/blob/main/Screenshot/sign2.png)

![Classification](https://github.com/Phoenix-ju/greenhouses-in-France/blob/main/Screenshot/sign3.png)














