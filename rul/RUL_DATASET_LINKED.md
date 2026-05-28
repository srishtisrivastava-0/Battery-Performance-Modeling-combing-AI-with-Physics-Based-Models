# RUL Dataset Successfully Linked! ✅

## Changes Made

The `battery_remaining_life_prediction.ipynb` notebook has been updated to work with your local RUL dataset.

### What Was Changed:

1. **Removed Google Colab dependency**
   - Commented out the `google.colab` import and drive mount code
   - This was causing the `ModuleNotFoundError`

2. **Updated data path**
   - **Old path:** `/content/drive/MyDrive/BatteryLifeDataset/Battery_RUL.csv`
   - **New path:** `../datasets/rul_dataset/Battery_RUL.csv`
   - The path is relative to the `rul/` directory where the notebook is located

### Dataset Location:
```
datasets/rul_dataset/Battery_RUL.csv
```

### How to Run:

1. **Open the notebook:**
   ```bash
   cd rul
   jupyter notebook battery_remaining_life_prediction.ipynb
   ```

2. **Or use the provided scripts:**
   - Windows: `start_notebook.bat`
   - Linux/Mac: `./start_notebook.sh`

3. **Run all cells** - The notebook should now execute without errors!

### What the Notebook Does:

- Loads the Battery RUL (Remaining Useful Life) dataset
- Performs exploratory data analysis (EDA)
- Trains multiple regression models:
  - RandomForestRegressor
  - BaggingRegressor
  - KNeighborsRegressor
  - ExtraTreeRegressor
  - DecisionTreeRegressor
  - And more...
- Evaluates model performance using RMSE
- Creates visualizations and feature importance plots

### Dataset Information:

The dataset contains features from 14 NMC-LCO 18650 batteries:
- **Cycle Index**: Number of cycle
- **F1**: Discharge Time (s)
- **F2**: Time at 4.15V (s)
- **F3**: Time Constant Current (s)
- **F4**: Decrement 3.6-3.4V (s)
- **F5**: Max. Voltage Discharge (V)
- **F6**: Min. Voltage Charge (V)
- **F7**: Charging Time (s)
- **Total time** (s)
- **RUL**: Target variable (Remaining Useful Life)

### Next Steps:

Run the notebook and it should work perfectly with your local dataset! 🚀
