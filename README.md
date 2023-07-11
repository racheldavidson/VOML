# VOML 
## Code was created in Jupyter Notebook to allow for segmentation and annotation of sections of code.  
## There are two datasets included currently which contain information about the same initial 41 samples formatted  slightly differently. 

real_data_take3b – CSV with inputs and outputs for 41 samples

real_data_take3d – CSV with inputs and outputs for 41 samples includes BDFE values. 

## Initial_modeling_part4_newbinaryadj.ipynb 
In this notebook the data was fit to different scikit-learn models and the accuracy of those models was evaluated.

Jupyter Notebook with the following sections of code: 
- Uploads the dataset
- Fits individual regressors for each phase
-	Multilabel machine learning –Fits the dataset to a variety of scikit-learn machine learning models. This section just looks at cross-validation scores 
-	In this section where models are marked down they did not function with the data as formatted
-	Leave one out (LOO) error estimates for all the models initially tested in the previous section
-	This fits the models for output data formatted as categorical, binary, binary adjusted data
-	Neural network hyperparameter tuning 

## Alipy_model_2023_lowaccuracy_witherrorcalcs_andBDFE_pH2lvl_acidredoxok.ipynb 
In this notebook the decision tree, which had the highest accuracy based on leave one out error analysis on the 
dataset was evaluated further, hyperparameters were tuned, and the model was used in active learning to select the #next samples to generate.

Jupyter Notebook with the following sections of code: 
-	Uploads the dataset
-	Defines the unsampled space
-	Fits the sample data to a decision tree and trims the tree
-	Predicts the output/identity of the unsampled space 
-	Uses Alipy to Query by Uncertainty to select the next samples to make from the unsampled sapce
-	Uses K-means clustering to create n clusters and look at ways to select samples from those clusters 


