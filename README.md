# yield-prediction-CVs
Spatial Block Cross-Validation with Iterative Grid Size for XGBoost
This repository contains Python scripts to evaluate wheat yield prediction performance using spatial block cross-validation with XGBoost regression.
The scripts demonstrate two complementary cases:
•	script1.py — Iterative grid-size evaluation across the entire study area (SA).
•	script2.py — Fixed grid size (~500 m) across SA with XGBoost hyperparameter tuning.
Study Context
This work is part of the study:
"Agricultural landscape heterogeneity matters for yield prediction: insights on using remote sensing, machine learning and adaptive spatial cross-validation for wheat yield prediction in Morocco"
The full study was conducted across both:
•	Entire study area (SA)
•	Landform-based classes
In this repository:
•	script1.py runs the spatial cross-validation analysis across the entire SA with varying grid sizes.
•	script2.py applies the same SA case but at the initial 500 m grid size and includes grid search hyperparameter tuning for XGBoost.
Scripts Overview
script1.py — Iterative Grid Size Evaluation (SA-wide)
Evaluates how grid size affects model performance when applying spatial block cross-validation across the SA.
Steps:
1.	Load dataset (Excel with longitude, latitude, features, and yield target).
2.	For each grid size (500 m to 1,250 km):
o	Create point shapefile (SA.shp)
o	Generate grid shapefile for the given cell size
o	Assign points to grid cells (FID)
o	Perform GroupKFold CV (3 folds) using grid IDs as groups
3.	Train and evaluate XGBoost for each size.
4.	Store R², MAE, and RMSE for all iterations.
script2.py — Fixed 500 m Grid + Hyperparameter Tuning (SA-wide)
Runs the same spatial blocking setup as script1.py but at a fixed 500 m grid size and performs grid search over XGBoost hyperparameters.
Hyperparameter ranges (per Zhang et al., 2022):
•	max_depth: 1–5
•	n_estimators: 100–1000 (step 100)
•	learning_rate: 0.1, 0.2, 0.3
•	colsample_bytree: 0.1–0.9 (step 0.1)
Selects the best combination by:
1.	Highest mean R² across folds
2.	Tie-breaker: lowest RMSE
3.	Tie-breaker: lowest MAE
Why Spatial Block Cross-Validation?
Predictive models in spatial datasets often suffer from spatial autocorrelation, leading to overestimated accuracy when using standard random cross-validation.
Spatial block CV mitigates this by:
1.	Partitioning the study area into grid cells
2.	Ensuring all samples from a cell are in the same fold
3.	Reducing spatial leakage between training and testing sets
 General Workflow
1.	Load dataset
o	Excel file with geographic coordinates, predictors, and yield values.
2.	Spatial partitioning
o	Create grid polygons over the study area (variable or fixed size)
o	Assign each sample point to a grid cell ID
3.	Cross-validation
o	Use GroupKFold with grid IDs as grouping variables
o	3 folds (or 2 if fewer than 3 unique cells)
4.	Model training and evaluation
o	Model: xgboost.XGBRegressor
o	Metrics:
	R² — Coefficient of determination
	MAE — Mean Absolute Error
	RMSE — Root Mean Squared Error

100.0	-0.35	950	1100

