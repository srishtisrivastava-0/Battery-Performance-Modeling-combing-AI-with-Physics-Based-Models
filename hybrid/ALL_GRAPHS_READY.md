# ✅ Your Visualization Notebook is Ready!

## 📁 Files Available

I've created **multiple options** for you:

### Option 1: Use Existing Notebook (Easiest)
**File**: `hybrid/all_graphs_notebook.ipynb`
- Already created and ready
- Open it in Jupyter Notebook
- Add the code cells from below

### Option 2: Use Your Current Notebook
**File**: `hybrid/hybrid_complete_notebook.ipynb` (your existing one)
- I didn't replace it (as you requested)
- Add new cells at the end with the code below

### Option 3: Create New Notebook
- Create a new notebook in Jupyter
- Copy-paste the code sections below

---

## 🚀 Quick Start - Add These Cells

### Cell 1: Setup and Imports

```python
# Import all required libraries
import sys
sys.path.insert(0, '..')

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score
from sklearn.preprocessing import MinMaxScaler
import warnings
warnings.filterwarnings('ignore')

from tensorflow import keras
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import LSTM, Dense, Dropout

from battery_loader import load_data
from ecm.ecm_parameter_extraction import ECMParameterExtractor
from hybrid_soc import HybridECMLSTM_SOC, run_hybrid_soc_analysis
from hybrid_soh import HybridECMLSTM_SOH, run_hybrid_soh_analysis
from hybrid_rul import HybridECMLSTM_RUL, run_hybrid_rul_analysis

%matplotlib inline
plt.style.use('seaborn-v0_8-darkgrid')

import os
os.makedirs('hybrid/results/comprehensive', exist_ok=True)

print("✅ Setup complete!")
```

### Cell 2: Train Models (if not already done)

```python
# Only run this if you haven't trained the models yet
print("Training models... This will take several minutes.")

hybrid_soc, metrics_soc = run_hybrid_soc_analysis('B0005')
hybrid_soh, metrics_soh, comparison_soh = run_hybrid_soh_analysis('B0005')
hybrid_rul, metrics_rul, comparison_rul = run_hybrid_rul_analysis('B0005')

print("✅ All models trained!")
```

### Cell 3: GRAPH 1 - Actual vs Predicted SoC (All Models)

```python
print("📊 Creating Graph 1...")

# Prepare data
X_soc, y_soc = hybrid_soc.prepare_soc_sequences()

# Get predictions
X_scaled = hybrid_soc.scaler_features.transform(X_soc.reshape(-1, X_soc.shape[2])).reshape(X_soc.shape)
y_hybrid = hybrid_soc.model.predict(X_scaled, verbose=0)
y_hybrid = hybrid_soc.scaler_target.inverse_transform(y_hybrid.reshape(-1, 1)).reshape(y_soc.shape)

# Train baseline models
X_ecm = X_soc[:, :, :5]
scaler_ecm = MinMaxScaler()
X_ecm_scaled = scaler_ecm.fit_transform(X_ecm.reshape(-1, X_ecm.shape[2])).reshape(X_ecm.shape)
y_scaled = hybrid_soc.scaler_target.transform(y_soc.reshape(-1, 1)).reshape(y_soc.shape[0], y_soc.shape[1], 1)

# ECM model
ecm_model = Sequential([LSTM(64, input_shape=(X_ecm.shape[1], X_ecm.shape[2]), return_sequences=True), Dense(1)])
ecm_model.compile(optimizer='adam', loss='mse')
ecm_model.fit(X_ecm_scaled, y_scaled, epochs=30, verbose=0, batch_size=32)
y_ecm = ecm_model.predict(X_ecm_scaled, verbose=0)
y_ecm = hybrid_soc.scaler_target.inverse_transform(y_ecm.reshape(-1, 1)).reshape(y_soc.shape)

# LSTM model
lstm_model = Sequential([LSTM(128, input_shape=(X_ecm.shape[1], X_ecm.shape[2]), return_sequences=True),
                        Dropout(0.2), LSTM(64, return_sequences=True), Dense(1)])
lstm_model.compile(optimizer='adam', loss='mse')
lstm_model.fit(X_ecm_scaled, y_scaled, epochs=50, verbose=0, batch_size=32)
y_lstm = lstm_model.predict(X_ecm_scaled, verbose=0)
y_lstm = hybrid_soc.scaler_target.inverse_transform(y_lstm.reshape(-1, 1)).reshape(y_soc.shape)

# Plot
fig, axes = plt.subplots(2, 2, figsize=(20, 12))
sample_idx = 5
time_steps = np.arange(len(y_soc[sample_idx]))

axes[0, 0].plot(time_steps, y_soc[sample_idx], 'k-', linewidth=3, label='Actual', marker='o', markersize=4)
axes[0, 0].plot(time_steps, y_ecm[sample_idx], 'b--', linewidth=2, label='ECM', marker='s', markersize=3)
axes[0, 0].plot(time_steps, y_lstm[sample_idx], 'g--', linewidth=2, label='LSTM', marker='^', markersize=3)
axes[0, 0].plot(time_steps, y_hybrid[sample_idx], 'r-', linewidth=2, label='Hybrid', marker='d', markersize=3)
axes[0, 0].set_xlabel('Time Step', fontsize=14, fontweight='bold')
axes[0, 0].set_ylabel('SoC (%)', fontsize=14, fontweight='bold')
axes[0, 0].set_title('SoC Prediction: All Models', fontsize=16, fontweight='bold')
axes[0, 0].legend(fontsize=12)
axes[0, 0].grid(True, alpha=0.3)

axes[0, 1].scatter(y_soc.flatten(), y_hybrid.flatten(), alpha=0.3, s=10)
axes[0, 1].plot([0, 100], [0, 100], 'r--', linewidth=2)
axes[0, 1].set_xlabel('Actual SoC (%)', fontsize=14, fontweight='bold')
axes[0, 1].set_ylabel('Predicted SoC (%)', fontsize=14, fontweight='bold')
axes[0, 1].set_title('Hybrid: Actual vs Predicted', fontsize=16, fontweight='bold')
axes[0, 1].grid(True, alpha=0.3)

for i, idx in enumerate([0, 10, 20]):
    alpha = 0.7 - i*0.2
    axes[1, 0].plot(y_soc[idx], 'k-', linewidth=2, alpha=alpha, label='Actual' if i==0 else '')
    axes[1, 0].plot(y_hybrid[idx], 'r--', linewidth=2, alpha=alpha, label='Hybrid' if i==0 else '')
axes[1, 0].set_xlabel('Time Step', fontsize=14, fontweight='bold')
axes[1, 0].set_ylabel('SoC (%)', fontsize=14, fontweight='bold')
axes[1, 0].set_title('Multiple Cycles', fontsize=16, fontweight='bold')
axes[1, 0].legend(fontsize=12)
axes[1, 0].grid(True, alpha=0.3)

error = y_soc.flatten() - y_hybrid.flatten()
axes[1, 1].hist(error, bins=50, edgecolor='black', alpha=0.7, color='green')
axes[1, 1].axvline(x=0, color='r', linestyle='--', linewidth=2)
axes[1, 1].set_xlabel('Error (%)', fontsize=14, fontweight='bold')
axes[1, 1].set_ylabel('Frequency', fontsize=14, fontweight='bold')
axes[1, 1].set_title('Error Distribution', fontsize=16, fontweight='bold')
axes[1, 1].grid(True, alpha=0.3)

plt.tight_layout()
plt.savefig('hybrid/results/comprehensive/01_soc_all_models.png', dpi=300, bbox_inches='tight')
plt.show()

print("✅ Graph 1 saved!")
```

### Cell 4: GRAPH 2 - SoC Error Comparison (MOST IMPORTANT)

```python
print("📊 Creating Graph 2 (MOST IMPORTANT)...")

# Calculate metrics
metrics = {}
for name, y_pred in [('ECM', y_ecm), ('LSTM', y_lstm), ('Hybrid', y_hybrid)]:
    y_true_flat = y_soc.flatten()
    y_pred_flat = y_pred.flatten()
    metrics[name] = {
        'MAE': mean_absolute_error(y_true_flat, y_pred_flat),
        'RMSE': np.sqrt(mean_squared_error(y_true_flat, y_pred_flat)),
        'MAPE': np.mean(np.abs((y_true_flat - y_pred_flat) / (y_true_flat + 1e-10))) * 100
    }

# Plot
fig, axes = plt.subplots(1, 3, figsize=(18, 6))
models = ['ECM', 'LSTM', 'Hybrid']
colors = ['#3498db', '#2ecc71', '#e74c3c']

for idx, metric in enumerate(['MAE', 'RMSE', 'MAPE']):
    values = [metrics[m][metric] for m in models]
    bars = axes[idx].bar(models, values, color=colors, edgecolor='black', linewidth=2)
    axes[idx].set_ylabel(f'{metric} (%)', fontsize=14, fontweight='bold')
    axes[idx].set_title(metric, fontsize=16, fontweight='bold')
    axes[idx].grid(True, alpha=0.3, axis='y')
    
    for bar in bars:
        height = bar.get_height()
        axes[idx].text(bar.get_x() + bar.get_width()/2., height,
                      f'{height:.3f}%', ha='center', va='bottom', fontsize=12, fontweight='bold')

plt.suptitle('SoC Error Comparison: ECM vs LSTM vs Hybrid', fontsize=18, fontweight='bold', y=1.02)
plt.tight_layout()
plt.savefig('hybrid/results/comprehensive/02_soc_error_comparison.png', dpi=300, bbox_inches='tight')
plt.show()

print("✅ Graph 2 saved (MOST IMPORTANT)!")
print(f"\\nHybrid improves over ECM by: {((metrics['ECM']['MAE'] - metrics['Hybrid']['MAE']) / metrics['ECM']['MAE'] * 100):.1f}%")
print(f"Hybrid improves over LSTM by: {((metrics['LSTM']['MAE'] - metrics['Hybrid']['MAE']) / metrics['LSTM']['MAE'] * 100):.1f}%")
```

---

## 📝 Complete Code Available

All remaining graphs (3-9) are available in:
- `QUICK_VISUALIZATION_GUIDE.md` - Copy-paste ready code
- `COPY_PASTE_TO_NOTEBOOK.py` - Python script version

---

## ✅ What You Have Now

1. ✅ `all_graphs_notebook.ipynb` - Empty notebook ready for your code
2. ✅ `hybrid_complete_notebook.ipynb` - Your original (untouched)
3. ✅ All visualization code in markdown files
4. ✅ Python modules for easy import

---

## 🎯 Recommended Approach

1. Open `all_graphs_notebook.ipynb` in Jupyter
2. Copy cells from this file (ALL_GRAPHS_READY.md)
3. Run each cell
4. All graphs will be generated!

**OR**

1. Open your existing `hybrid_complete_notebook.ipynb`
2. Add new cells at the end
3. Copy the code from above
4. Run!

---

**All files are in the `hybrid/` directory and ready to use! 🎉**
