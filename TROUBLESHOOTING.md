# 🔧 Troubleshooting Guide

## Common Issues and Solutions

---

## ❌ Issue 1: ModuleNotFoundError

### **Error:**
```
ModuleNotFoundError: No module named 'ecm_model'
ModuleNotFoundError: No module named 'battery_loader'
ModuleNotFoundError: No module named 'hybrid_ecm_lstm'
```

### **✅ Solution:**

#### **Quick Fix:**
The import path in `ecm/ecm_parameter_extraction.py` has been fixed. Just restart your Jupyter kernel:

1. In Jupyter: **Kernel** → **Restart & Clear Output**
2. Run all cells again

#### **If Still Not Working:**

Add this to the **first cell** of your notebook:

```python
import sys
import os

# Add parent directory to Python path
sys.path.insert(0, os.path.abspath('..'))

# Now imports will work
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from hybrid_ecm_lstm import HybridECMLSTM, run_hybrid_analysis
```

#### **Verify Your Directory Structure:**

```bash
# You should be in the hybrid/ directory
pwd  # Should show: .../hybrid

# Check parent directory has the files
ls ..
# Should show: battery_loader.py, ecm/, hybrid/, soh/, rul/, etc.
```

---

## ❌ Issue 2: Dataset Not Found

### **Error:**
```
FileNotFoundError: [Errno 2] No such file or directory: 'datasets/battery_data/B0005.mat'
```

### **✅ Solution:**

#### **Check Dataset Location:**

```bash
# From project root
ls datasets/battery_data/
# Should show: B0005.mat, B0006.mat, etc.
```

#### **Fix Path in Code:**

If running from `hybrid/` directory, the path should be:

```python
# In battery_loader.py, line 23:
mat = loadmat('../datasets/battery_data/' + battery + '.mat')
# Note the '../' to go up one directory
```

#### **Or Copy battery_loader.py:**

```bash
# Copy to hybrid directory
cp battery_loader.py hybrid/
```

Then in notebook, import becomes:
```python
from battery_loader import load_data  # No path needed
```

---

## ❌ Issue 3: TensorFlow/Keras Import Error

### **Error:**
```
ImportError: cannot import name 'Sequential' from 'tensorflow.keras.models'
ModuleNotFoundError: No module named 'tensorflow'
```

### **✅ Solution:**

#### **Install TensorFlow:**

```bash
pip install tensorflow==2.21.0
# Or latest version:
pip install tensorflow
```

#### **Verify Installation:**

```python
import tensorflow as tf
print(tf.__version__)
# Should print: 2.21.0 or similar
```

#### **If Using Apple Silicon (M1/M2 Mac):**

```bash
# Use tensorflow-metal for GPU acceleration
pip install tensorflow-macos
pip install tensorflow-metal
```

---

## ❌ Issue 4: Memory Error

### **Error:**
```
MemoryError: Unable to allocate array
ResourceExhaustedError: OOM when allocating tensor
```

### **✅ Solution:**

#### **Reduce Batch Size:**

```python
# In hybrid_ecm_lstm.py, line ~200
history = hybrid.train(X, y, epochs=100, batch_size=8)  # Reduce from 16 to 8
```

#### **Reduce Sequence Length:**

```python
# When initializing
hybrid = HybridECMLSTM(battery_id='B0005', sequence_length=25)  # Reduce from 50
```

#### **Use Fewer Cycles:**

```python
# Extract parameters less frequently
ecm_params = hybrid.extract_ecm_features(cycle_step=10)  # Increase from 5 to 10
```

---

## ❌ Issue 5: Jupyter Notebook Won't Start

### **Error:**
```
jupyter: command not found
```

### **✅ Solution:**

#### **Install Jupyter:**

```bash
pip install jupyter notebook
```

#### **Or Use JupyterLab:**

```bash
pip install jupyterlab
jupyter lab
```

#### **Check Installation:**

```bash
jupyter --version
# Should show version numbers
```

---

## ❌ Issue 6: Plots Not Showing

### **Error:**
Plots don't appear in notebook

### **✅ Solution:**

#### **Add Magic Command:**

```python
%matplotlib inline
import matplotlib.pyplot as plt
```

#### **Or Use:**

```python
%matplotlib notebook  # For interactive plots
```

#### **Explicitly Show Plots:**

```python
plt.plot(data)
plt.show()  # Add this line
```

---

## ❌ Issue 7: Slow Training

### **Error:**
Training takes too long (>30 minutes)

### **✅ Solution:**

#### **Reduce Epochs:**

```python
history = hybrid.train(X, y, epochs=50, batch_size=16)  # Reduce from 100
```

#### **Use GPU (if available):**

```python
# Check GPU availability
import tensorflow as tf
print("GPU Available:", tf.config.list_physical_devices('GPU'))
```

#### **Reduce Data:**

```python
# Use fewer cycles
ecm_params = hybrid.extract_ecm_features(cycle_step=10)  # More sparse
```

---

## ❌ Issue 8: Poor Model Performance

### **Error:**
R² score is low (<0.8)

### **✅ Solution:**

#### **Check Data Quality:**

```python
# Verify data loaded correctly
print(f"Dataset shape: {dataset.shape}")
print(f"Missing values: {dataset.isnull().sum()}")
```

#### **Increase Training Epochs:**

```python
history = hybrid.train(X, y, epochs=150, batch_size=16)  # Increase epochs
```

#### **Try Different Battery:**

```python
# Some batteries have better data quality
hybrid = HybridECMLSTM(battery_id='B0006')  # Try B0006 or B0007
```

---

## ❌ Issue 9: Kernel Keeps Dying

### **Error:**
Jupyter kernel crashes during execution

### **✅ Solution:**

#### **Increase Memory Limit:**

```bash
# Start Jupyter with more memory
jupyter notebook --NotebookApp.max_buffer_size=1000000000
```

#### **Restart Kernel:**

In Jupyter: **Kernel** → **Restart**

#### **Run Cells Individually:**

Don't run all cells at once. Run them one by one to identify the problematic cell.

---

## ❌ Issue 10: Import Error After Installation

### **Error:**
```
ImportError: cannot import name 'X' from 'Y'
```

### **✅ Solution:**

#### **Reinstall Requirements:**

```bash
pip uninstall -y numpy pandas scipy matplotlib seaborn scikit-learn tensorflow keras
pip install -r requirements.txt
```

#### **Check Python Version:**

```bash
python --version
# Should be 3.8 or higher
```

#### **Use Virtual Environment:**

```bash
# Create virtual environment
python -m venv venv

# Activate (Windows)
venv\Scripts\activate

# Activate (Mac/Linux)
source venv/bin/activate

# Install requirements
pip install -r requirements.txt
```

---

## 🆘 Still Having Issues?

### **Debug Checklist:**

1. **Check Python Version:**
   ```bash
   python --version  # Should be 3.8+
   ```

2. **Check Installed Packages:**
   ```bash
   pip list | grep -E "numpy|pandas|tensorflow|scikit-learn"
   ```

3. **Check Current Directory:**
   ```bash
   pwd  # Should be in hybrid/ or project root
   ```

4. **Check File Exists:**
   ```bash
   ls ../battery_loader.py  # Should exist
   ls ../ecm/ecm_model.py   # Should exist
   ```

5. **Test Simple Import:**
   ```python
   import sys
   print(sys.path)  # Check Python search paths
   ```

---

## 📝 Quick Test Script

Save this as `test_setup.py` and run it:

```python
#!/usr/bin/env python3
"""
Test script to verify project setup
"""

import sys
import os

print("="*60)
print("Project Setup Test")
print("="*60)

# Test 1: Python version
print(f"\n1. Python Version: {sys.version}")
assert sys.version_info >= (3, 8), "Python 3.8+ required"
print("   ✅ Python version OK")

# Test 2: Required packages
print("\n2. Testing package imports...")
try:
    import numpy as np
    print(f"   ✅ numpy {np.__version__}")
except ImportError as e:
    print(f"   ❌ numpy: {e}")

try:
    import pandas as pd
    print(f"   ✅ pandas {pd.__version__}")
except ImportError as e:
    print(f"   ❌ pandas: {e}")

try:
    import tensorflow as tf
    print(f"   ✅ tensorflow {tf.__version__}")
except ImportError as e:
    print(f"   ❌ tensorflow: {e}")

try:
    import sklearn
    print(f"   ✅ scikit-learn {sklearn.__version__}")
except ImportError as e:
    print(f"   ❌ scikit-learn: {e}")

# Test 3: Project files
print("\n3. Testing project files...")
files_to_check = [
    'battery_loader.py',
    'ecm/ecm_model.py',
    'ecm/ecm_parameter_extraction.py',
    'hybrid/hybrid_ecm_lstm.py',
    'requirements.txt'
]

for file in files_to_check:
    if os.path.exists(file):
        print(f"   ✅ {file}")
    else:
        print(f"   ❌ {file} NOT FOUND")

# Test 4: Dataset
print("\n4. Testing dataset...")
if os.path.exists('datasets/battery_data/B0005.mat'):
    print("   ✅ Dataset found")
else:
    print("   ❌ Dataset NOT FOUND")
    print("      Please download NASA battery dataset")

print("\n" + "="*60)
print("Setup test complete!")
print("="*60)
```

Run it:
```bash
python test_setup.py
```

---

## 📞 Need More Help?

### **Check Documentation:**
- `README.md` - Project overview
- `GETTING_STARTED.md` - Quick start guide
- `hybrid/README.md` - Hybrid model guide
- `hybrid/QUICK_FIX.md` - Import error fix

### **Common Solutions:**
1. Restart Jupyter kernel
2. Reinstall requirements: `pip install -r requirements.txt`
3. Check you're in correct directory
4. Add parent directory to path: `sys.path.insert(0, '..')`

---

**Most issues are solved by restarting the Jupyter kernel!** 🔄
