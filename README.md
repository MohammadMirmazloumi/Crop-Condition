# Crop Condition

Monthly Vegetation indexes from Sentinel-2 images are used as input to classify regions affected by drought.
Thie repository includes two codes:
- crop_condition_model
- model_implementation

The idea is following the below workflow:


![workflow](workflow.PNG)
 

In detail, the procedure is as follows:
-	Vegetation indexes time series: The monthly mosaics of Sentinel-2 images were first produced for the period of 2017-2022. Based on the yield data and additional information regarding drought occurrence, the years 2018 and 2022 were selected for training as examples of non-drought and drought conditions, respectively. The images were processed to exclude images with high cloud coverage and tag cloud contaminated pixels. Afterwards, 3 indices, namely NDVI, NDMI, and GVI, were computed for each monthly composite.
-	Growing season’s stratification: The Kenyan agricultural systems are classified into two short and long rainy growing seasons. To clearly investigate the start, maximum and end of the two growing seasons, we utilized the WaPOR Phenology (Seasonal) product. The method is based on a time series of dekadal NDVI composites. Hence, the timing of each growing season for training years were derived for cropland pixels, masked by WaPOR land cover classification maps. The stratification correctness was also confirmed by local partners to reliably and accurately identify the two growing seasons.  
-	Labeling pixels: Following the stratification and identification of two seasons, the time series were filtered by identifying local temporal anomalies based on consecutive VI observations. This step is needed for filtering the time series used for training by identifying the time series that have abnormally low values during the growing season, which can indicate the drought signal. Finally, two classes of drought affected and unaffected were separated for each season considering the drought years and the stratification.
-	Classification and performance assessment: In this step, a 2D matrix of labeled time series is generated using the data of the previous step, which is the input of the machine learning classifier. The input data is separated to train and test portions by the ratio of 0.3. The random forest is trained by 70% of labeled time series to provide a classifier for predicting the most accurate class label. Finally, the performance of the trained model is assessed by the confusion matrix and overall accuracy over the test data.
The trained model was used to predict the crop condition in the years that were not used for training.
