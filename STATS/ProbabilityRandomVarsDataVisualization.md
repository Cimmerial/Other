# Comprehensive Guide to Probability, Statistics, and Data Analysis

## Table of Contents
1. [Data Visualization and Descriptive Statistics](#visualization)
2. [Basic Probability Concepts](#basic-probability)
3. [Counting Methods](#counting)
4. [Conditional Probability and Independence](#conditional)
5. [Random Variables](#random-variables)
6. [Common Probability Distributions](#distributions)
7. [Expected Value and Variance](#moments)
8. [Special Topics](#special) 
9. [Distribution Functions](#distribution-functions) 

## 1. Data Visualization and Descriptive Statistics <a name="visualization"></a>

### Shape Characteristics
- **Skewness**
  - Left-skewed: Mean < Median
  - Right-skewed: Mean > Median
  - Symmetric: Mean ≈ Median
  - Identifying from histograms: Look at the "tail" direction

### Summary Statistics
- **Five-Number Summary**
  - Minimum
  - Q1 (25th percentile)
  - Median (50th percentile)
  - Q3 (75th percentile)
  - Maximum
- **IQR (Interquartile Range)**
  ```
  IQR = Q3 - Q1
  Outlier thresholds: [Q1 - 1.5*IQR, Q3 + 1.5*IQR]
  ```
- **Chebyshev's Inequality**
  - At least 1-1/k² of data lies within k standard deviations of mean
  - Works for ANY distribution

### Box Plots
- Shows five-number summary
- Outliers plotted as individual points
- Length of box = IQR
- Location of median line indicates skewness
- Can compare multiple groups side by side

## 2. Basic Probability Concepts <a name="basic-probability"></a>

### Fundamental Rules
1. **Addition Rule**
   ```
   P(A ∪ B) = P(A) + P(B) - P(A ∩ B)
   ```
2. **Multiplication Rule**
   ```
   P(A ∩ B) = P(A|B) * P(B)
   ```
3. **Complement Rule**
   ```
   P(A') = 1 - P(A)
   ```

### Set Operations
- Union (∪): OR
- Intersection (∩): AND
- Complement ('): NOT
- Mutually Exclusive: P(A ∩ B) = 0

## 3. Counting Methods <a name="counting"></a>

### Basic Principles
1. **Multiplication Principle**
   - If task A has m ways and task B has n ways
   - Total ways = m * n

2. **Addition Principle**
   - If task A can be done in m ways OR task B in n ways
   - Total ways = m + n

### Combinations and Permutations
1. **Permutations**
   ```
   P(n,r) = n!/(n-r)!
   ```
   - Order matters
   - Sampling without replacement

2. **Combinations**
   ```
   C(n,r) = n!/[r!(n-r)!]
   ```
   - Order doesn't matter
   - Sampling without replacement

3. **With Replacement**
   - Permutations: n^r
   - Combinations with repetition: C(n+r-1,r)

## 4. Conditional Probability and Independence <a name="conditional"></a>

### Conditional Probability
```
P(A|B) = P(A ∩ B)/P(B)
```

### Independence
1. **Two Events**
   ```
   P(A ∩ B) = P(A) * P(B)
   P(A|B) = P(A)
   ```

2. **Multiple Events**
   ```
   P(A ∩ B ∩ C) = P(A) * P(B) * P(C)
   ```

### Bayes' Theorem
```
P(A|B) = P(B|A) * P(A)/P(B)
```
Where:
```
P(B) = P(B|A)*P(A) + P(B|A')*P(A')
```

## 5. Random Variables <a name="random-variables"></a>

### Types
1. **Discrete**
   - Countable outcomes
   - Uses PMF (Probability Mass Function)
   - Example: number of successes

2. **Continuous**
   - Uncountable outcomes
   - Uses PDF (Probability Density Function)
   - Example: time, height

### Properties
1. **PMF Requirements**
   ```
   P(X = x) ≥ 0
   ΣP(X = x) = 1
   ```

2. **PDF Requirements**
   ```
   f(x) ≥ 0
   ∫f(x)dx = 1
   P(a ≤ X ≤ b) = ∫[a to b]f(x)dx
   ```

## 6. Common Probability Distributions <a name="distributions"></a>

### Discrete Distributions

1. **Bernoulli(p)**
   - Single trial, success probability p
   ```
   P(X = 1) = p
   P(X = 0) = 1-p
   E[X] = p
   Var(X) = p(1-p)
   ```

2. **Binomial(n,p)**
   - n independent Bernoulli trials
   ```
   P(X = k) = C(n,k) * p^k * (1-p)^(n-k)
   E[X] = np
   Var(X) = np(1-p)
   ```

3. **Poisson(λ)**
   - Rate of rare events
   ```
   P(X = k) = (λ^k * e^(-λ))/k!
   E[X] = λ
   Var(X) = λ
   ```

### Continuous Distributions

1. **Uniform(a,b)**
   ```
   f(x) = 1/(b-a) for a ≤ x ≤ b
   E[X] = (a+b)/2
   Var(X) = (b-a)^2/12
   ```

2. **Exponential(λ)**
   - Time between Poisson events
   ```
   f(x) = λe^(-λx) for x ≥ 0
   E[X] = 1/λ
   Var(X) = 1/λ^2
   ```

3. **Normal(μ,σ²)**
   ```
   f(x) = (1/√(2πσ²))e^(-(x-μ)²/(2σ²))
   E[X] = μ
   Var(X) = σ²
   ```

## 7. Expected Value and Variance <a name="moments"></a>

### Discrete Case
```
E[X] = Σ x*P(X = x)
Var(X) = E[(X-μ)²] = E[X²] - (E[X])²
```

### Continuous Case
```
E[X] = ∫ x*f(x)dx
Var(X) = ∫ (x-μ)²*f(x)dx
```

### Properties
1. **Linearity of Expectation**
   ```
   E[aX + b] = aE[X] + b
   E[X + Y] = E[X] + E[Y]
   ```

2. **Variance Properties**
   ```
   Var(aX + b) = a²Var(X)
   Var(X + Y) = Var(X) + Var(Y) [if independent]
   ```

## 8. Special Topics <a name="special"></a>

### Birthday Problem
- Probability of shared birthday in group of n people
- P(shared) = 1 - P(all different)
- P(all different) = 365!/(365-n)!/365^n

### Monty Hall Problem
- Switch increases winning probability from 1/3 to 2/3
- Key: Host's knowledge changes probability distribution

### Hypergeometric Distribution
- Sampling without replacement
- Different from binomial due to changing probabilities
```
P(X = k) = C(K,k)*C(N-K,n-k)/C(N,n)
```

### Law of Large Numbers
- Sample mean converges to expected value
- Basis for Monte Carlo simulation

### Central Limit Theorem
- Sum/average of many independent RVs ≈ Normal
- Key for statistical inference
- Works regardless of original distribution


## 9. Distribution Functions <a name="distribution-functions"></a>

### Cumulative Distribution Function (CDF)

#### Definition
- For any random variable X, the CDF F(x) = P(X ≤ x)
- Represents probability of X taking value less than or equal to x

#### Properties of CDF
1. **Monotonicity**
   - F(x) is non-decreasing
   - If x₁ < x₂, then F(x₁) ≤ F(x₂)

2. **Range**
   ```
   lim(x→-∞) F(x) = 0
   lim(x→∞) F(x) = 1
   ```

3. **Right-Continuity**
   - F(x) = F(x⁺)
   - Important for discrete distributions

4. **Probability Calculations**
   ```
   P(a < X ≤ b) = F(b) - F(a)
   P(X > a) = 1 - F(a)
   ```

#### Discrete vs Continuous CDFs
1. **Discrete**
   - Step function
   - Jumps at possible values
   - F(x) = Σ[t≤x] P(X = t)

2. **Continuous**
   - Smooth function
   - F(x) = ∫[-∞ to x] f(t)dt
   - No jumps

### Probability Density Function (PDF)

#### Definition
- For continuous random variables only
- f(x) = F'(x) (derivative of CDF)
- Not a probability, but a density

#### Properties of PDF
1. **Non-negativity**
   ```
   f(x) ≥ 0 for all x
   ```

2. **Total Area**
   ```
   ∫[-∞ to ∞] f(x)dx = 1
   ```

3. **Probability Calculations**
   ```
   P(a ≤ X ≤ b) = ∫[a to b] f(x)dx
   ```

4. **Expected Value**
   ```
   E[g(X)] = ∫[-∞ to ∞] g(x)f(x)dx
   ```

### Relationships Between PMF, PDF, and CDF

#### Discrete Case
1. **PMF to CDF**
   ```
   F(x) = Σ[t≤x] P(X = t)
   ```

2. **CDF to PMF**
   ```
   P(X = x) = F(x) - F(x⁻)
   ```

#### Continuous Case
1. **PDF to CDF**
   ```
   F(x) = ∫[-∞ to x] f(t)dt
   ```

2. **CDF to PDF**
   ```
   f(x) = d/dx F(x)
   ```

### Common Distribution Functions

#### Normal Distribution
```
PDF: f(x) = (1/√(2πσ²))e^(-(x-μ)²/(2σ²))
CDF: F(x) = Φ((x-μ)/σ)
```
Where Φ is the standard normal CDF

#### Exponential Distribution
```
PDF: f(x) = λe^(-λx), x ≥ 0
CDF: F(x) = 1 - e^(-λx), x ≥ 0
```

#### Uniform Distribution
```
PDF: f(x) = 1/(b-a), a ≤ x ≤ b
CDF: F(x) = (x-a)/(b-a), a ≤ x ≤ b
```

### Applications and Examples

#### Example 1: Normal Distribution Calculations
```python
from scipy import stats

# Standard Normal
z_score = (x - μ)/σ
probability = stats.norm.cdf(z_score)

# General Normal
probability = stats.norm.cdf(x, loc=μ, scale=σ)
```

#### Example 2: Reliability Analysis
For lifetime T ~ Exp(λ):
```
P(T > t) = 1 - F(t) = e^(-λt)
```

#### Example 3: Quantile Calculations
```
For probability p, quantile q satisfies:
F(q) = p
q = F⁻¹(p)
```

### Important Uses

1. **Risk Analysis**
   - Value at Risk (VaR) using quantiles
   - Probability of extreme events

2. **Quality Control**
   - Probability of defects
   - Process capability indices

3. **Hypothesis Testing**
   - P-values from test statistics
   - Critical values for tests

4. **Confidence Intervals**
   - Quantiles for interval endpoints
   - Coverage probabilities

[Rest of previous sections remain the same...]
