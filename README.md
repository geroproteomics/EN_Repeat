# EN_Repeat
Function that validates features chosen via elastic net modeling by identifying out-of-sample prediction ability.

**R Function Name:**  
EN_Repeat

**Overview:**  
Elastic net is a form of penalized linear regression that performs feature selection by shrinking beta coefficients of predictor variables using a penalty term, 
the stength of which is determined using a hyperperamater lambda, with a smaller lambda selecting more predictor terms, and larger selecting fewer. A range of values are used for lambda,
and ultimately the model using the lambda value and selected features that produces the lowest mean squared error (MSE) are found. In this way, Elastic Net can be used to identify important 
features, for example gene expression levels, implicated in clinical parameters (age, bmi, etc.).

One downside of elastic net modeling using common R functions such as cv.glmnet is the variability of the output, as train and test sets are selected randomly. This function reduces variability 
and increases the robustness of results by repeating elastic net modeling in R using the glmnet package a user-specified number of times, averages the output, and produces a lambda value and set 
of selected features that produce the lowest MSE across many trials. This function also auto-scales features, uses parallel processing for faster output, and automates useful visualizations of the selected features.


**Usage:**  
EN_Repeat_Results <- EN_Repeat(clin_df, protein_list, control_list, trait_list, alpha, iterations, heatmap=FALSE)

**Arguments**  
| Parameter       | Type        | Description                                                                                             |
|----------------|-------------|---------------------------------------------------------------------------------------------------------|
| `clin_df `     | `dataframe` | Table of microarray data containing gene expression values and covariates by columns, sample IDs by row |
| `protein_list` | `vector `   | Vector of genes from which to perform feature selection, and any continuous covariates                  |
| `control_list` | `vector `   | Vector of categorical covariates                                                                        |
| `trait_list`   | `vector `   | Vector of continuous traits for which to select implicated features                                     |
| `alpha`        | `numeric `  | Number indicating hyperparameter alpha (0 for ridge, 1 for lasso, in-between for Elastic Net)           |
| `interations`  | `numeric `  | Number indicating the number of times the analysis will be run                                          |
| `heatmap`      | `boolean `  | Boolean value determining if a heatmap displaying effect size by feature and trait will be returned     |

**Return Values:**  
- Coef is a table showing the penalized effect size of each feature for each supplied trait.
- Lambda is a table showing the supplied alpha and optimized lambda across many runs for each parameter of the trait_list.
- Heatmap is a graphical representation of the penalized effect size of each feature for each supplied trait.

<p align="center">
  <img src="images/Example_heatmap2.JPG" alt="Example Image of Selected Features" width="500">
</p>

- IVSum is a bar graph respresenting the number of selected features for each supplied parameter. Example image shows 100 possible features and 2 covariates.

<p align="center">
  <img src="images/Example_ivsum.JPG" alt="Example Image of Selected Features" width="500">
</p>

**Dependencies:**  
glmnet  
ggplot2  
heatmaply  
doParallel

**Notes**  
This function will produce an error if the heatmap functionality is set to true and any of the parameters have 0 selected features. If this error occurs, set heatmap to false.

**Author**  
Bradley Olinger, PhD  
b.a.olinger@gmail.com




