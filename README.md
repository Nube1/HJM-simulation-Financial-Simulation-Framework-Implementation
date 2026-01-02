# ZARONIA Paradox Simulation: Theorem 4.5 Proof

## Overview

This repository contains a quantitative simulation engine designed to numerically validate **Theorem 4.5 (Stability Violation)** within the context of the ZARONIA (South African Rand Overnight Index Average) benchmark transition. 

The simulation demonstrates a specific regulatory paradox where rapid monetary tightening (rate shocks) creates a false signal of financial stability. This occurs when the mechanical reduction in Expected Credit Losses (ECL)—caused by higher discount rates—outpaces the recognition of rising default probabilities due to regulatory reporting lags.

**Key Outcome:** The model proves that under specific lag conditions, $\frac{\partial \text{Capital}}{\partial r} > 0$ (Capital Relief) while $\frac{\partial \text{Risk}}{\partial r} > 0$ (Risk Increase), creating a "Whipsaw Effect" in capital adequacy.

## Features

*   **Stochastic Simulation:** Models ZARONIA rates using mean-reverting processes with jump diffusion for policy shocks.
*   **Lag Modeling:** Explicitly separates **Economic Transmission Lags** (how fast borrowers react to rates) from **Regulatory Reporting Lags** (how fast banks update PD models).
*   **Paradox Detection:** Automated calculation of partial derivatives ($\partial E/\partial r$ and $\partial Risk/\partial r$) to formally detect Theorem 4.5 violations.
*   **Whipsaw Quantification:** Measures the magnitude of capital destruction following the false stability signal.
*   **Visual Proofs:** Generates high-resolution plotting of the "Paradox Window" and mathematical decomposition of ECL.
*   **Robustness & Sensitivity:** Includes Monte Carlo robustness checks and sensitivity analysis on parameters like default sensitivity and reporting lags.

## Mathematical Context

The simulation validates the following conditions defined in Theorem 4.5:

1.  **Monetary Shock:** $\Delta r > 0$ (Tightening).
2.  **Discounting Dominance:** The discount factor ($e^{-rt}$) drops instantaneously, reducing the present value of ECL.
3.  **Reporting Friction:** Regulatory Probability of Default ($PD_{reg}$) remains fixed during the reporting interval $[t, t+\Delta t]$.
4.  **The Violation:**
    $$ \frac{\partial \text{Capital}}{\partial r} > 0 \quad \text{AND} \quad \frac{\partial \text{Economic Risk}}{\partial r} > 0 $$

This results in banks releasing capital exactly when economic risk is rising, violating the principle of counter-cyclical buffering.

## Prerequisites

The simulation requires Python 3.8+ and the following scientific computing libraries:

```bash
pip install numpy matplotlib pandas scipy
```

## Usage

Save the script as `zaronia_paradox.py` and run it directly:

```bash
python zaronia_paradox.py
```

## Outputs

The script generates three types of output:

### 1. Console Analysis
Detailed statistical proof of the paradox, including:
*   Magnitude of the monetary shock.
*   Calculated derivatives for Capital and Risk.
*   Confirmation of stability violation.
*   Quantification of the "Whipsaw" magnitude.

### 2. Visualizations
*   **`theorem45_paradox.png`**: A 3-panel composite showing the Rate Shock, Discounting effects, and the divergence between Economic and Reported Capital.
*   **`theorem45_proof.png`**: A visual representation of the mathematical proof, focusing on derivative signs and ECL component decomposition.
*   **`sensitivity_analysis.png`**: (Conditional) Graphs showing how the paradox frequency changes with default sensitivity and lag times.

### 3. Data
*   **`paradox_simulation.csv`**: Time-series data containing the full simulation paths for rates, PDs (Economic vs Regulatory), and Capital requirements for further analysis.

## Configuration

The simulation parameters can be adjusted in the `ParadoxConfig` class within the script:

```python
class ParadoxConfig:
    def __init__(self):
        self.reporting_lag = 0.5   # Regulatory update frequency (years)
        self.theta_shock = 0.07    # Magnitude of rate shock (7%)
        self.default_sensitivity = 3.0  # How strongly PD reacts to rates
        # ...
```

## Interpretation of Results

When the simulation runs successfully, you will observe a **"Paradox Window"** (highlighted in yellow in the plots).

*   **Before the window:** Rates are stable.
*   **Inside the window:** Rates spike. The Discount Factor crashes. Because Reporting PD is frozen, calculated ECL drops significantly. **Capital requirements decrease (Capital Relief).**
*   **After the window:** The Reporting Lag expires. The high Economic PD is finally recognized. ECL spikes massively. **Capital requirements jump drastically (The Whipsaw).**

## Disclaimer

This code is for theoretical and educational purposes regarding financial stability modeling and benchmark transitions. It is not financial advice and should not be used for production capital planning without rigorous validation against specific institutional portfolios.
