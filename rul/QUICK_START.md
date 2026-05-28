# 🚀 Quick Start - Battery RUL Prediction

## ✅ Setup Status: COMPLETE

All libraries are installed and the dataset is ready!

## 📝 Important: Update Dataset Path

Before running the notebook, you need to update the dataset path:

### In the notebook cell that loads data:

**CHANGE FROM:**
```python
df = pd.read_csv("/content/drive/MyDrive/BatteryLifeDataset/Battery_RUL.csv")
```

**CHANGE TO:**
```python
df = pd.read_csv("../datasets/archive (1)/Battery_dataset.csv")
```

## 🎯 Run the Notebook

### Method 1: Jupyter Notebook
```bash
jupyter notebook
```
Then navigate to: `rul/battery_remaining_life_prediction.ipynb`

### Method 2: JupyterLab
```bash
jupyter lab
```
Then open: `rul/battery_remaining_life_prediction.ipynb`

### Method 3: VS Code
1. Open the notebook file in VS Code
2. Select Python kernel
3. Run cells

## 📊 Dataset Info

- **Location:** `datasets/archive (1)/Battery_dataset.csv`
- **Rows:** 680
- **Columns:** 11
- **Features:**
  - battery_id, cycle, chI, chV, chT
  - disI, disV, disT, BCt, SOH
  - **RUL** (target variable)

## 🔍 Verification

To verify your setup anytime:
```bash
cd rul
python verify_setup.py
```

## 📚 Models Included

The notebook trains and compares these models:
1. ✅ Random Forest Regressor
2. ✅ AdaBoost Regressor
3. ✅ Gradient Boosting Regressor
4. ✅ Bagging Regressor (typically best performer)
5. ✅ Support Vector Regressor (SVR)
6. ✅ Decision Tree Regressor
7. ✅ Extra Tree Regressor
8. ✅ Linear Regression
9. ✅ SGD Regressor
10. ✅ K-Neighbors Regressor

Plus ensemble stacking with Lasso meta-model!

## 💡 Tips

1. **First time?** Read through the notebook cells before running
2. **Slow execution?** Some models (SVR, Gradient Boosting) take longer
3. **Want faster results?** Comment out slower models
4. **Customize?** Adjust hyperparameters in model definitions

## 🎨 Visualizations Included

- RUL distribution plots
- Correlation heatmaps
- Feature importance charts
- Model performance comparisons
- Prediction vs actual plots

## ⚡ Expected Runtime

- Data loading: < 1 second
- EDA & visualization: 5-10 seconds
- Model training: 1-3 minutes (depends on your CPU)
- Total: ~5 minutes

## 🆘 Need Help?

Check these files:
- `SETUP_GUIDE.md` - Detailed setup instructions
- `verify_setup.py` - Run diagnostics
- `README.md` - Project overview

## 🎉 You're All Set!

Everything is configured and ready to go. Just update the dataset path and start exploring!

---
**Last Verified:** 2026-05-26  
**Status:** ✅ All systems operational
