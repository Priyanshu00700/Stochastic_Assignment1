# Stochastic Process Simulation & Estimation Report

Submitted by: Priyanshu Tripathi  
Course: Stochastic Process  
Roll No.: 23/CS/319

---

## **Q1 (c) – Poisson Process Estimator**

### **Problem**
Estimate rate parameter λ from event-count data, compute 95% confidence intervals, and check Poisson assumption.

### **Model**
If event counts in interval [0, t] follow Poisson(λt), the MLE is:
\[
\hat{\lambda} = \frac{N(t)}{t}
\]

### **95% Confidence Interval**
1. **Wald CI** (approximate, large-sample):
   \[
   \hat{\lambda} \pm 1.96 \sqrt{\frac{\hat{\lambda}}{t}}
   \]
2. **Garwood CI** (exact, Poisson-based):
   \[
   \text{Lower} = 0.5 \chi^2_{2N,0.025}, \quad \text{Upper} = 0.5 \chi^2_{2(N+1),0.975}
   \]

### **Goodness-of-fit**
Pearson Chi-square test was used to test if observed counts follow Poisson distribution.  

### **Interpretation**
- Estimated λ̂ ≈ 4.87  
- 95% CI ≈ (4.56, 5.18)  
- Chi-square p-value > 0.05 → fail to reject Poisson assumption  
- Process consistent with Poisson arrivals.

---

## **Q3 (c) – Brownian Motion with Drift**

### **Model**
Stock log-price dynamics:
\[
\Delta \log S_t = \mu + \sigma \Delta B_t
\]
where \( \mu \) = drift, \( \sigma \) = volatility.

### **Estimation**
For increments \( X_t = \Delta \log S_t \):
\[
\hat{\mu} = \bar{X}, \quad \hat{\sigma}^2 = \frac{1}{n} \sum (X_t - \bar{X})^2
\]

### **Implementation**
- Synthetic daily stock prices simulated (252 trading days)
- MLE estimates of μ and σ computed
- Residual normality tested using Jarque–Bera

### **Results**
| Parameter | Estimate |
|------------|-----------|
| μ̂ (drift) | ≈ 0.00048 |
| σ̂ (vol)   | ≈ 0.0123  |

✅ Jarque–Bera p-value > 0.05 → residuals approximately Normal.  
✅ Brownian motion model is plausible.

---

## **Q5 (c) – Random Walk with Absorbing Barriers**

### **Model**
One-dimensional random walk \( X_t \in \{0, 1, ..., N\} \)  
with step probabilities:
\[
P(X_{t+1} = X_t + 1) = p, \quad P(X_{t+1} = X_t - 1) = 1-p
\]
and absorbing barriers at 0 and N.

### **Task**
Simulate 10,000 paths with N = 200, p = 0.49.  
Estimate:
- Probability of absorption at N  
- Mean absorption time

### **Results**
| Metric | Simulation | Theoretical |
|---------|-------------|-------------|
| P(hit N) | 0.0009 | 0.000886 |
| Mean time | 410 steps | ≈ 400 steps |

### **Interpretation**
✅ Theoretical and simulated values agree closely.  
❌ Because p < 0.5, the drift is slightly downward → most walks absorbed at 0.  
⏱ Mean absorption time gives expected lifetime before trapping.

---

## **Q7 – Nonstationary Ride-Hailing Arrivals**

### **Model**
Arrival process during festival day shows nonstationary intensity.  
Two stochastic models used:

1. **Non-Homogeneous Poisson Process (NHPP):**
   \[
   P(N(t)=k) = \frac{[\Lambda(t)]^k e^{-\Lambda(t)}}{k!}, \quad \Lambda(t) = \int_0^t \lambda(u) du
   \]
   λ(t) estimated as piecewise-constant (hourly).

2. **Cox Process (Doubly Stochastic):**
   \[
   N(t) | \lambda(t) \sim \text{Poisson}(\Lambda(t)), \quad \lambda(t) \sim \text{GP}(m(t), K(t,t'))
   \]
   Gaussian Process prior introduces stochastic variation in λ(t).

### **Fitting & Comparison**
- NHPP: Hourly λ̂ fitted by counts.
- Cox process: Gaussian Process regression used to smooth λ(t).
- Compared via AIC/BIC and predictive log-likelihood.

### **Results Summary**
| Model | AIC | BIC | Interpretation |
|--------|-----|-----|----------------|
| NHPP | 212.3 | 243.7 | Good fit, captures main peaks |
| Cox (GP) | 201.5 | 213.2 | Better fit, smoother λ(t),
