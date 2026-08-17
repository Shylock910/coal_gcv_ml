# Coal Quality Estimation from Geophysical Well Logs using Machine Learning

Predicting coal quality parameters - **ash content, moisture, and Gross Calorific Value (GCV)** - directly from geophysical well log data, using a chained machine learning pipeline.

## Problem

Coal quality (graded G1 to G17 for non-coking coals per Indian Standard Practice, 2022) is conventionally determined through laboratory analysis of borehole samples. This approach is:
- Limited to point data at borehole locations
- Time and cost intensive
- Reliant on lateral interpolation between boreholes, introducing spatial uncertainty

This project addresses these limitations by linking geophysical log responses (density, natural gamma, resistivity, caliper, P and S wave velocities) directly to lab-calibrated proximate parameters, enabling **continuous, depth-wise prediction of coal quality**.

## Data

- **LAS files** (Well 17 & Well 22): density, natural gamma, resistivity, caliper, P-velocity, S-velocity
- **XLSX files** (Well 17 & Well 22): lab-calibrated moisture and ash content at specific depths (ground truth)

## Methodology

1. **Preprocessing** - removal of missing/undefined values, standardization, scaling
2. **Feature engineering** - Pearson correlation analysis, feature importance ranking, derived log features
3. **Chained modeling approach**:
   - Stage 1: Ash content predicted from density, natural gamma, resistivity
   - Stage 2: Moisture predicted using logs + predicted ash
   - Stage 3: GCV predicted using logs + predicted ash and moisture
4. **Models trained**: XGBoost, Random Forest, Linear Regression, Multi-Layer Perceptron (MLP), with hyperparameters tuned via GridSearchCV
5. **Validation**: Trained on Well 17, tested on Well 22, evaluated using 5-fold cross-validation
6. **Deployment**: Best models applied to raw LAS files for continuous depth-wise prediction of ash, moisture, and GCV

Density emerged as the dominant predictor across all target parameters, followed by natural gamma and resistivity.

## Results

| Parameter | Best Model | R² | MAE |
|---|---|---|---|
| Ash content | MLP | 0.869 | 4.184% |
| Moisture | MLP | 0.442 | 1.570% |
| GCV | Random Forest | 0.847 | 373.4 Kcal/kg |

- All four models exceeded the R² ≥ 0.80 target threshold for GCV prediction.
- Predicted GCV logs for both wells closely tracked lab-measured values across full logged depth intervals, correctly delineating coal seams within the G10–G16 grade range.
- Moisture prediction was comparatively weaker across all models, indicating it is harder to constrain from geophysical logs alone.
- Minor discrepancies at high-GCV intervals were attributed to thin seam effects and lithological heterogeneity not fully captured by the log curves.

## Future Scope

- Incorporate seismic data to extend predictions beyond borehole coverage
- Test additional models: CatBoost, Extra Trees, LightGBM, SVR, stacking ensembles
- Add spontaneous potential, neutron porosity, and photoelectric factor logs to improve moisture prediction

## Team

- Utkarsh Sharma, M.Tech (Geo-exploration), IIT Bombay
- Sayandip Das, M.Tech (Petroleum Geosciences), IIT Bombay
- Vivek Krishnamurthy Iyer, M.Tech (Petroleum Geosciences), IIT Bombay
