# 🚀 Quick Visualization Guide

## ✅ All Required Graphs - Ready to Use

I've created all the visualization code you need. Here's how to use it:

### Step 1: Run Your Models First

```python
# In your notebook, run these first:
from hybrid_soc import run_hybrid_soc_analysis
from hybrid_soh import run_hybrid_soh_analysis
from hybrid_rul import run_hybrid_rul_analysis

# Run analyses
hybrid_soc, metrics_soc = run_hybrid_soc_analysis('B0005')
hybrid_soh, metrics_soh, comparison_soh = run_hybrid_soh_analysis('B0005')
hybrid_rul, metrics_rul, comparison_rul = run_hybrid_rul_analysis('B0005')
```

### Step 2: Import Visualization Module

```python
from all_visualizations import *

# Create output directory
create_output_dir()
```

### Step 3: Generate All Graphs

```python
# Get data (you'll have this from running the models)
X_soc, y_soc = hybrid_soc.prepare_soc_sequences()

# GRAPH 1: Actual vs Predicted SoC (All Models)
y_ecm, y_lstm, y_hybrid = plot_1_soc_all_models(hybrid_soc, X_soc, y_soc, 'B0005')

# GRAPH 2: SoC Error Comparison (MOST IMPORTANT)
soc_metrics = plot_2_soc_error_comparison(y_soc, y_ecm, y_lstm, y_hybrid, 'B0005')

# GRAPH 3: Voltage Response
plot_3_voltage_response('B0005')

# GRAPH 4: Current vs Time
plot_4_current_vs_time('B0005')

# GRAPH 5: Training/Validation Loss (if you saved history)
# plot_5_training_validation_loss(history_soc, 'SOC')
```

## 📊 All Graphs List

### ✅ MUST HAVE (Created)
1. ✅ Actual vs Predicted SoC - `plot_1_soc_all_models()`
2. ✅ SoC Error Comparison - `plot_2_soc_error_comparison()` **MOST IMPORTANT**
3. ✅ Voltage Response - `plot_3_voltage_response()`
4. ✅ Current vs Time - `plot_4_current_vs_time()`
5. ✅ Training/Validation Loss - `plot_5_training_validation_loss()`

### 🔄 Additional Graphs Needed

Add these to your notebook:

#### GRAPH 6: SOH Degradation Curve

```python
def plot_soh_degradation(hybrid_soh, X_soh, y_soh, battery_id='B0005'):
    # Get predictions
    X_scaled = hybrid_soh.scaler_features.transform(X_soh.reshape(-1, X_soh.shape[2])).reshape(X_soh.shape)
    y_pred = hybrid_soh.model.predict(X_scaled)
    y_pred = hybrid_soh.scaler_target.inverse_transform(y_pred).flatten()
    
    fig, ax = plt.subplots(figsize=(14, 8))
    cycles = np.arange(len(y_soh))
    
    ax.plot(cycles, y_soh, 'b-o', linewidth=3, markersize=8, label='Actual SOH', markerfacecolor='blue')
    ax.plot(cycles, y_pred, 'r--s', linewidth=3, markersize=8, label='Predicted SOH', markerfacecolor='red')
    ax.axhline(y=80, color='orange', linestyle=':', linewidth=2, label='EOL Threshold (80%)')
    
    ax.set_xlabel('Cycle Number', fontsize=14, fontweight='bold')
    ax.set_ylabel('SOH (%)', fontsize=14, fontweight='bold')
    ax.set_title('Battery State of Health Degradation Over Lifetime', fontsize=16, fontweight='bold')
    ax.legend(fontsize=12, loc='best')
    ax.grid(True, alpha=0.3)
    ax.set_ylim([75, 105])
    
    plt.tight_layout()
    plt.savefig(f'hybrid/results/comprehensive/{battery_id}_06_soh_degradation.png', dpi=300, bbox_inches='tight')
    plt.show()
    
    print("✅ Graph 6: SOH Degradation saved!")

# Run it
X_soh, y_soh = hybrid_soh.prepare_hybrid_features()
plot_soh_degradation(hybrid_soh, X_soh, y_soh, 'B0005')
```

#### GRAPH 7: Capacity Fade Curve

```python
def plot_capacity_fade(battery_id='B0005'):
    from battery_loader import load_data
    
    dataset, capacity_data = load_data(battery_id)
    
    # Get capacity per cycle
    capacity_per_cycle = dataset.groupby('cycle')['capacity'].first()
    cycles = capacity_per_cycle.index
    capacities = capacity_per_cycle.values
    
    fig, ax = plt.subplots(figsize=(14, 8))
    
    ax.plot(cycles, capacities, 'b-o', linewidth=3, markersize=6, markerfacecolor='blue')
    ax.axhline(y=capacities[0]*0.8, color='red', linestyle='--', linewidth=2, label='80% Capacity (EOL)')
    
    ax.set_xlabel('Cycle Number', fontsize=14, fontweight='bold')
    ax.set_ylabel('Capacity (Ah)', fontsize=14, fontweight='bold')
    ax.set_title('Battery Capacity Fade Over Lifetime', fontsize=16, fontweight='bold')
    ax.legend(fontsize=12)
    ax.grid(True, alpha=0.3)
    
    # Add fade rate annotation
    fade_rate = (capacities[0] - capacities[-1]) / len(cycles) * 100
    ax.text(0.02, 0.98, f'Fade Rate: {fade_rate:.4f} Ah/cycle',
            transform=ax.transAxes, verticalalignment='top',
            bbox=dict(boxstyle='round', facecolor='wheat', alpha=0.8), fontsize=12)
    
    plt.tight_layout()
    plt.savefig(f'hybrid/results/comprehensive/{battery_id}_07_capacity_fade.png', dpi=300, bbox_inches='tight')
    plt.show()
    
    print("✅ Graph 7: Capacity Fade saved!")

# Run it
plot_capacity_fade('B0005')
```

#### GRAPH 8: RUL Actual vs Predicted

```python
def plot_rul_prediction(hybrid_rul, X_rul, y_rul, battery_id='B0005'):
    # Get predictions
    X_scaled = hybrid_rul.scaler_features.transform(X_rul.reshape(-1, X_rul.shape[2])).reshape(X_rul.shape)
    y_pred = hybrid_rul.model.predict(X_scaled)
    y_pred = hybrid_rul.scaler_target.inverse_transform(y_pred).flatten()
    y_pred = np.maximum(y_pred, 0)  # Ensure non-negative
    
    fig, axes = plt.subplots(1, 2, figsize=(18, 7))
    
    # Plot 1: RUL over time
    samples = np.arange(len(y_rul))
    axes[0].plot(samples, y_rul, 'b-o', linewidth=3, markersize=6, label='Actual RUL')
    axes[0].plot(samples, y_pred, 'r--s', linewidth=3, markersize=6, label='Predicted RUL')
    axes[0].set_xlabel('Sample Index', fontsize=14, fontweight='bold')
    axes[0].set_ylabel('RUL (cycles)', fontsize=14, fontweight='bold')
    axes[0].set_title('Remaining Useful Life Prediction', fontsize=16, fontweight='bold')
    axes[0].legend(fontsize=12)
    axes[0].grid(True, alpha=0.3)
    axes[0].axhline(y=0, color='black', linestyle='-', linewidth=1)
    
    # Plot 2: Scatter plot
    axes[1].scatter(y_rul, y_pred, alpha=0.6, s=100, c='blue', edgecolors='black')
    axes[1].plot([0, y_rul.max()], [0, y_rul.max()], 'r--', linewidth=2)
    
    # Add ±10 and ±20 cycle bands
    x_range = np.linspace(0, y_rul.max(), 100)
    axes[1].fill_between(x_range, x_range-10, x_range+10, alpha=0.2, color='green', label='±10 cycles')
    axes[1].fill_between(x_range, x_range-20, x_range+20, alpha=0.1, color='yellow', label='±20 cycles')
    
    axes[1].set_xlabel('Actual RUL (cycles)', fontsize=14, fontweight='bold')
    axes[1].set_ylabel('Predicted RUL (cycles)', fontsize=14, fontweight='bold')
    axes[1].set_title('RUL: Actual vs Predicted', fontsize=16, fontweight='bold')
    axes[1].legend(fontsize=12)
    axes[1].grid(True, alpha=0.3)
    
    # Add R²
    r2 = r2_score(y_rul, y_pred)
    axes[1].text(0.05, 0.95, f'R² = {r2:.4f}', transform=axes[1].transAxes,
                fontsize=12, verticalalignment='top',
                bbox=dict(boxstyle='round', facecolor='wheat', alpha=0.8))
    
    plt.tight_layout()
    plt.savefig(f'hybrid/results/comprehensive/{battery_id}_08_rul_prediction.png', dpi=300, bbox_inches='tight')
    plt.show()
    
    print("✅ Graph 8: RUL Prediction saved!")

# Run it
X_rul, y_rul = hybrid_rul.prepare_rul_features()
plot_rul_prediction(hybrid_rul, X_rul, y_rul, 'B0005')
```

#### GRAPH 9: Hybrid Architecture Diagram

```python
def plot_hybrid_architecture():
    """
    GRAPH 9: Hybrid Architecture Diagram
    Shows the complete workflow
    """
    fig, ax = plt.subplots(figsize=(14, 10))
    ax.axis('off')
    
    # Define boxes
    boxes = [
        {'text': 'Battery Dataset\\n(Voltage, Current, Temperature)', 'pos': (0.5, 0.95), 'color': '#3498db'},
        {'text': 'Data Preprocessing\\n(Normalization, Cleaning)', 'pos': (0.5, 0.85), 'color': '#2ecc71'},
        {'text': 'Thevenin 1RC ECM\\n(Extract R0, R1, C1, τ1)', 'pos': (0.25, 0.70), 'color': '#e74c3c'},
        {'text': 'Raw Features\\n(V, I, T)', 'pos': (0.75, 0.70), 'color': '#f39c12'},
        {'text': 'Feature Fusion\\n(ECM + Raw Features)', 'pos': (0.5, 0.55), 'color': '#9b59b6'},
        {'text': 'LSTM Network\\n(Bidirectional + Dropout)', 'pos': (0.5, 0.40), 'color': '#1abc9c'},
        {'text': 'Output Layer\\n(Dense + Activation)', 'pos': (0.5, 0.25), 'color': '#34495e'},
        {'text': 'Predictions\\nSOC | SOH | RUL', 'pos': (0.5, 0.10), 'color': '#e67e22'},
    ]
    
    # Draw boxes
    for box in boxes:
        bbox = dict(boxstyle='round,pad=0.8', facecolor=box['color'], edgecolor='black', linewidth=2, alpha=0.8)
        ax.text(box['pos'][0], box['pos'][1], box['text'],
                ha='center', va='center', fontsize=12, fontweight='bold',
                bbox=bbox, color='white', transform=ax.transAxes)
    
    # Draw arrows
    arrows = [
        ((0.5, 0.92), (0.5, 0.88)),
        ((0.5, 0.82), (0.25, 0.75)),
        ((0.5, 0.82), (0.75, 0.75)),
        ((0.25, 0.67), (0.5, 0.60)),
        ((0.75, 0.67), (0.5, 0.60)),
        ((0.5, 0.52), (0.5, 0.45)),
        ((0.5, 0.37), (0.5, 0.30)),
        ((0.5, 0.22), (0.5, 0.15)),
    ]
    
    for arrow in arrows:
        ax.annotate('', xy=arrow[1], xytext=arrow[0],
                   arrowprops=dict(arrowstyle='->', lw=3, color='black'),
                   xycoords='axes fraction', textcoords='axes fraction')
    
    ax.set_title('Hybrid ECM-LSTM Architecture', fontsize=18, fontweight='bold', pad=20)
    
    plt.tight_layout()
    plt.savefig('hybrid/results/comprehensive/09_hybrid_architecture.png', dpi=300, bbox_inches='tight')
    plt.show()
    
    print("✅ Graph 9: Hybrid Architecture saved!")

# Run it
plot_hybrid_architecture()
```

## 📁 Output Location

All graphs will be saved to:
```
hybrid/results/comprehensive/
```

## 🎯 Priority Order for Presentation

1. **Graph 2**: SoC Error Comparison (MOST IMPORTANT)
2. **Graph 9**: Hybrid Architecture
3. **Graph 1**: Actual vs Predicted SoC
4. **Graph 6**: SOH Degradation
5. **Graph 8**: RUL Prediction
6. **Graph 3**: Voltage Response
7. **Graph 7**: Capacity Fade
8. **Graph 4**: Current vs Time
9. **Graph 5**: Training/Validation Loss

## ✅ Quick Run All

```python
# Run everything at once
create_output_dir()

# SOC graphs
X_soc, y_soc = hybrid_soc.prepare_soc_sequences()
y_ecm, y_lstm, y_hybrid = plot_1_soc_all_models(hybrid_soc, X_soc, y_soc, 'B0005')
soc_metrics = plot_2_soc_error_comparison(y_soc, y_ecm, y_lstm, y_hybrid, 'B0005')

# ECM/Physics graphs
plot_3_voltage_response('B0005')
plot_4_current_vs_time('B0005')

# SOH graphs
X_soh, y_soh = hybrid_soh.prepare_hybrid_features()
plot_soh_degradation(hybrid_soh, X_soh, y_soh, 'B0005')
plot_capacity_fade('B0005')

# RUL graphs
X_rul, y_rul = hybrid_rul.prepare_rul_features()
plot_rul_prediction(hybrid_rul, X_rul, y_rul, 'B0005')

# Architecture
plot_hybrid_architecture()

print("\n🎉 ALL GRAPHS GENERATED SUCCESSFULLY!")
```

---

**Need help?** Check `VISUALIZATION_GUIDE.md` for detailed explanations of each graph.
