# VOML 
## Overview of Objectives and Project
A generalized hydrothermal route to formation of metastable binary vanadium oxides has been systematically explored, resulting in initial isolation of seven distinct phases of VO. Navigation of synthetic landscapes involves mapping of potentially vast design spaces spanning the effects processing variables, reaction pathways, and chemical functionality in selected reagents. Each individual reaction parameter can be considered as providing an additional vector for exploration creating a high-dimensional reaction space. Traditional means which involve evaluation of the effects of reaction parameters one variable at a time are woefully inefficient in establishing meaningful process-structure-property relations and lack the ability to distinguish feature interactions. Coupling of design of experiment methodologies with accelerated synthesis and characterization approaches has enabled initial broad sampling of the hydrothermal synthesis of vanadium oxides from vanadic acid precursors where the effects of reaction time, temperature, concentration of vanadium precursors, pressure, and redox environment have been examined through forty-one experiments. Dirichlet regressors and decision tree algorithms were found to effectively map relations between reaction parameters and resulting product phase. Insights provided by evaluation of relative feature importance revealed that additional structural diversity would likely be found through further tuning of the redox environment. Initial reagent screening identified seventeen additional viable redox reagents, representing unchartered areas of an expanded design space. Current efforts involve exploration of this expanded reaction space through active learning to direct future sampling by leveraging models of the existing data to strategically target underexplored areas of the design space where model uncertainty is highest and to navigate towards isolation of pure products where phases were initially only isolated as minority fractions. This approach represents a generalizable methodology to provide deterministic pathways towards synthetic targets and discovery of key process-structure-property relations.

## A More Detailed Description of Data and Processes

### The initial sample space
41 initial samples of vanadium oxide were produced through a hydrothermal route. Samples were selected following a Box Behnken design of experiments where the solution pH, concentration of vanadium precursor, time under hydrothermal conditions, temperature of the hydrothermal oven, volume of solution in the hydrothermal vessel were varied. The resulting phase of vanadium oxide was represented as an output in several ways: categorical data where each phase and each phase mixture were separately designated as a categorical outputs, binary data where the presence or absence of a phase in a given sample was represented in a binary fashion, and a binary adjusted data where phases which were only present at less than 30% in a given sample were designated as being absent except in cases where the phase was only isolated as a minor fraction within the dataset.  

### Initial Modeling
Synthesis parameters were treated as inputs and phase fraction as outputs for fitting the dataset to machine learning models. The data was fitted to a variety of different scikit-learn models with error assessed based on leave one out accuracy. The decision tree algorithm demonstrated the highest initial accuracy of the scikit-learn models which was increased upon trimming the decision tree. Using an R package, a Dirichlet model performed adequately as well (code currently not present in the GitHub) as did fitting one regressor for each phase to predict the presence or absence of each phase (code in the Initial_modeling_part4_newbinaryadj.ipynb). A variety of addition error metrics were calculated in Excel (not available in GitHub at the moment) to compare the accuracy of these three models. Based on metrics from the three models, the pH was shown to play a key role in driving formation of new phases of VO. 

### A Note about pH as a Parameter
An issue with pH as a metric was that initially pH was altered by titrating in either pH or NaOH into samples to lower or raise the pH by 1. The samples containing NaOH incorporated Na into the structure, thus producing structures that were not pure VO. As we were targeting generalized routes to VO which could then be further tuned by incorporating additional metals after a reasonable model was produced, we wanted to avoid including samples in this model which contained other metals. Those samples were remade with the addition of isopropyl alcohol which produced a reducing environment and added the effects of redox into the reaction space instead. To further explore redox as a parameter, new redox reagents were explored and encoded in the unsampled space based on their bond dissociation free energies as input parameters to describe their presence. pH was then no longer a parameter that had three levels (low, unchanged, high) but instead had just two (low, unchanged). 
Defining the unsampled space for future sampling
An unsampled space was defined as including four factors (time, temperature, volume, and concentration of vanadium precursor) at three levels (low, medium, high) as well as pH at two levels (low and unchanged) and bond dissociation free energy for redox at 10 levels. This resulted in ((34) *2*10)=1620 samples in the whole space. The initial 41 samples were removed from this list leaving 1579 unsampled samples. 

### Active Learning Process
The Alipy package was used to predict the entropy for each of the unsampled samples based on the decision tree model using the QuerybyUncertainty package within Alipy. Since the entropy is based on the predict_proba output from the decision tree model, which is determined by the population at each terminal node in the decision tree, there were many samples calculated to have the same uncertainty in the unsampled space. To avoid sampling from a single terminal node in the decision tree, which would result in sampling more similar samples, samples with the highest three entropy values were selected. These were then separated into clusters using scikit-learn’s k-means clustering package. From each cluster a random sample was selected to produce 20 samples to generate next. 

## VOML File Descriptions 
### Code was created in Jupyter Notebook to allow for segmentation and annotation of sections of code.  
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


