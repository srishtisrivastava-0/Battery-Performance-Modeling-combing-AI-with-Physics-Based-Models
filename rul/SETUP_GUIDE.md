# Battery RUL (Remaining Useful Life) - Setup Guide

## ✅ Setup Complete!

All required libraries have been installed and verified. The RUL prediction system is ready to use.

## 📦 Installed Libraries

The following libraries are installed and ready:

### Core Data Science
- **numpy** (2.3.5) - Numerical computing
- **pandas** (3.0.3) - Data manipulation and analysis
- **scipy** (1.17.1) - Scientific computing
- **matplotlib** (3.10.9) - Data visualization
- **seaborn** (0.13.2) - Statistical data visualization

### Machine Learning
- **scikit-learn** (1.8.0) - Machine learning algorithms including:
  - RandomForestRegressor
  - AdaBoostRegressor
  - GradientBoostingRegressor
  - BaggingRegressor
  - SVR (Support Vector Regression)
  - DecisionTreeRegressor
  - ExtraTreeRegressor
  - LinearRegression
  - SGDRegressor
  - KNeighborsRegressor
  - StandardScaler (for feature scaling)

### Deep Learning
- **tensorflow** (2.21.0) - Deep learning framework
- **keras** (3.14.1) - High-level neural networks API
- **xgboost** (3.2.0) - Gradient boosting framework

### Jupyter Environment
- **jupyter** (1.1.1) - Jupyter notebook environment
- **notebook** (7.5.6) - Jupyter notebook server
- **ipykernel** (7.2.0) - IPython kernel for Jupyter
- **ipywidgets** (8.1.8) - Interactive widgets

### Visualization
- **plotly** (6.7.0) - Interactive plotting library

### Testing
- **pytest** (9.0.3) - Testing framework
- **hypothesis** (6.152.7) - Property-based testing

## 📊 Dataset Information

**Location:** `datasets/archive (1)/Battery_RUL.csv` or `datasets/archive (1)/Battery_dataset.csv`

**Dataset Details:**
- **Source:** Hawaii Natural Energy Institute
- **Batteries:** 14 NMC-LCO 18650 batteries
- **Nominal Capacity:** 2.8 Ah
- **Cycles:** Over 1000 cycles at 25°C
- **Charge Rate:** CC-CV at C/2 rate
- **Discharge Rate:** 1.5C

**Features:**
1. **Cycle_Index** - Number of cycles
2. **F1: Discharge Time (s)** - Time taken for discharge
3. **F2: Time at 4.15V (s)** - Duration at 4.15V
4. **F3: Time Constant Current (s)** - CC phase duration
5. **F4: Decrement 3.6-3.4V (s)** - Voltage drop time
6. **F5: Max. Voltage Discharge (V)** - Maximum discharge voltage
7. **F6: Min. Voltage Charge (V)** - Minimum charge voltage
8. **F7: Charging Time (s)** - Total charging time
9. **Total time (s)** - Total cycle time
10. **RUL** - Remaining Useful Life (TARGET)

## 🚀 How to Run

### Option 1: Jupyter Notebook (Recommended)

1. **Start Jupyter Notebook:**
   ```bash
   jupyter notebook
   ```

2. **Navigate to the RUL folder** and open:
   ```
   rul/battery_remaining_life_prediction.ipynb
   ```

3. **Update the dataset path** in the notebook:
   ```python
   # Change this line in the notebook:
   df = pd.read_csv("/content/drive/MyDrive/BatteryLifeDataset/Battery_RUL.csv")
   
   # To this (relative path):
   df = pd.read_csv("../datasets/archive (1)/Battery_dataset.csv")
   ```

4. **Run all cells** to execute the analysis

### Option 2: Python Script

Create a standalone Python script if needed:

```python
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
from sklearn.ensemble import RandomForestRegressor, BaggingRegressor
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error, mean_absolute_error, r2_score

# Load dataset
df = pd.read_csv("datasets/archive (1)/Battery_dataset.csv")

# Your analysis code here...
```

## 📈 What the Notebook Does

1. **Data Exploration**
   - Summary statistics
   - Data types and missing values
   - Basic dataset information

2. **Exploratory Data Analysis (EDA)**
   - RUL distribution visualization
   - Correlation heatmap
   - Feature relationships with RUL
   - Pair plots

3. **Data Preprocessing**
   - Remove Cycle_Index feature
   - Split into features (X) and target (y)
   - Train-test split
   - Feature standardization using StandardScaler

4. **Model Training & Evaluation**
   - Multiple regression models tested:
     - Random Forest
     - AdaBoost
     - Gradient Boosting
     - Bagging
     - SVR
     - Decision Tree
     - Extra Tree
     - Linear Regression
     - SGD
     - K-Neighbors
   - Performance metrics (MSE, MAE, R²)
   - Best model selection

5. **Feature Importance Analysis**
   - Identify most important features
   - Visualize feature contributions

6. **Ensemble Modeling**
   - Stack multiple models
   - Meta-model training (Lasso)
   - Final predictions

## 🔧 Troubleshooting

### Issue: Module not found
**Solution:** Reinstall requirements
```bash
pip install -r requirements.txt
```

### Issue: Dataset not found
**Solution:** Update the dataset path in the notebook to:
```python
df = pd.read_csv("../datasets/archive (1)/Battery_dataset.csv")
```

### Issue: Jupyter kernel not found
**Solution:** Install and register the kernel
```bash
python -m ipykernel install --user --name=battery-rul
```

### Issue: Memory error with large dataset
**Solution:** Use data chunking or reduce dataset size
```python
df = pd.read_csv("path/to/data.csv", nrows=10000)  # Load first 10k rows
```

## 📝 Expected Results

- **Best Model:** Typically BaggingRegressor performs best
- **Metrics:** 
  - R² Score: ~0.95-0.99 (excellent)
  - MSE: Low values indicate good fit
  - MAE: Average prediction error in cycles

## 🎯 Key Insights

1. **Most Important Features:**
   - Discharge Time
   - Time at 4.15V
   - Charging Time
   - Voltage characteristics

2. **Model Performance:**
   - Ensemble methods (Bagging, Random Forest) typically outperform single models
   - Stacking can further improve predictions

3. **Practical Applications:**
   - Predictive maintenance scheduling
   - Battery replacement planning
   - Resource optimization

## 📚 Additional Resources

- **Kaggle Dataset:** [Battery RUL Dataset](https://www.kaggle.com/datasets/ignaciovinuales/battery-remaining-useful-life-rul)
- **Hawaii Natural Energy Institute:** Original data source
- **Scikit-learn Documentation:** [sklearn.ensemble](https://scikit-learn.org/stable/modules/ensemble.html)

## ✨ Next Steps

1. Run the notebook and explore the results
2. Experiment with different models and hyperparameters
3. Try feature engineering to improve predictions
4. Compare with deep learning approaches (LSTM, CNN)
5. Deploy the best model for real-time predictions

---

**Status:** ✅ Ready to run!  
**Last Updated:** 2026-05-26
