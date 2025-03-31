# Comprehensive Statistics Guide

## Table of Contents
1. [Joint Random Variables](#joint-random-variables)
2. [Sampling and the Central Limit Theorem](#sampling-and-the-central-limit-theorem)
3. [Hypothesis Testing](#hypothesis-testing)
4. [Confidence Intervals](#confidence-intervals)

## Joint Random Variables

### Joint Probability Mass Functions (PMFs)
Joint PMFs describe the probability distribution of two or more random variables together.

**Definition**: For discrete random variables X and Y, the joint PMF P(X=x, Y=y) gives the probability that X takes value x and Y takes value y simultaneously.

**Example**: 
If X can take values 0, 1, 2, 3 and Y can take values 0, 1, 2, we can represent their joint distribution in a table:

| X\Y | 0 | 1 | 2 |
|-----|---|---|---|
| 0   |0.1|0.1|0.05|
| 1   |0.15|0.1|0.05|
| 2   |0.05|0.1|0.1|
| 3   |0.05|0.05|0.1|

From this joint PMF, we can calculate:
- Joint probabilities: P(X=1, Y=0) = 0.15
- Marginal probabilities: P(X=1) = P(X=1, Y=0) + P(X=1, Y=1) + P(X=1, Y=2) = 0.15 + 0.1 + 0.05 = 0.3
- Conditional probabilities: P(X=1|Y=0) = P(X=1, Y=0)/P(Y=0) = 0.15/(0.1+0.15+0.05+0.05) = 0.15/0.35 = 0.43

### Multinomial Random Variables
Multinomial random variables are generalizations of binomial random variables to more than two outcomes.

**Definition**: A multinomial distribution models the probability of counts for each possible outcome in a fixed number of independent trials, where each trial has a categorical outcome with k possible values.

**PMF**: If X = (X₁, X₂, ..., Xₖ) follows a multinomial distribution with parameters n (number of trials) and p = (p₁, p₂, ..., pₖ) (probabilities of each outcome), then:

P(X₁=x₁, X₂=x₂, ..., Xₖ=xₖ) = (n!/(x₁!×x₂!×...×xₖ!)) × p₁^x₁ × p₂^x₂ × ... × pₖ^xₖ

where x₁ + x₂ + ... + xₖ = n

**Example**: 
When rolling a die 10 times, the number of times each face appears follows a multinomial distribution with n=10 and p=(1/6, 1/6, 1/6, 1/6, 1/6, 1/6).

```python
# Simulating multinomial random variables in Python
import numpy as np
from numpy.random import multinomial

# 10 trials, equal probability for 6 outcomes
sample = multinomial(10, [1/6, 1/6, 1/6, 1/6, 1/6, 1/6])
print(sample)  # Example output: [1 2 2 1 2 2]
```

### Independence of Random Variables

**Definition**: Two random variables X and Y are independent if and only if:

P(X=x, Y=y) = P(X=x) × P(Y=y) for all possible values of x and y

To check if variables are independent:
1. Calculate joint probabilities P(X=x, Y=y)
2. Calculate marginal probabilities P(X=x) and P(Y=y)
3. Check if P(X=x, Y=y) = P(X=x) × P(Y=y) for all x, y pairs

**Example**: 
Using the joint PMF table above, to check if X and Y are independent:
- P(X=1, Y=0) = 0.15
- P(X=1) = 0.3
- P(Y=0) = 0.35
- P(X=1) × P(Y=0) = 0.3 × 0.35 = 0.105

Since 0.15 ≠ 0.105, X and Y are not independent.

### Standard Units, Covariance, and Correlation

**Standard Units**: Converting a random variable to standard units:
Z = (X - μₓ)/σₓ

Where μₓ is the mean and σₓ is the standard deviation.

```python
def convert_to_standard_units(data):
    mean = np.mean(data)
    sd = np.std(data, ddof=1)  # Using sample standard deviation
    return (data - mean) / sd
```

**Covariance**: Measures how two random variables vary together.

Cov(X, Y) = E[(X - μₓ)(Y - μᵧ)] = E[XY] - E[X]E[Y]

**Correlation**: Normalized measure of association between variables.

ρ(X, Y) = Cov(X, Y)/(σₓσᵧ)

Correlation ranges from -1 to 1:
- ρ = 1: Perfect positive linear relationship
- ρ = -1: Perfect negative linear relationship
- ρ = 0: No linear relationship

**Important Note**: Association and correlation do not imply causation.

### Sums of Random Variables

For any random variables X and Y:
- E[X + Y] = E[X] + E[Y]
- If X and Y are independent, then Var(X + Y) = Var(X) + Var(Y)
- If X and Y are not independent, then Var(X + Y) = Var(X) + Var(Y) + 2 × Cov(X, Y)

For a sum of n random variables:
- E[X₁ + X₂ + ... + Xₙ] = E[X₁] + E[X₂] + ... + E[Xₙ]
- If all variables are independent, Var(X₁ + X₂ + ... + Xₙ) = Var(X₁) + Var(X₂) + ... + Var(Xₙ)

### IID Random Variables

**Definition**: Random variables are Independent and Identically Distributed (IID) if:
1. They are mutually independent
2. They all have the same probability distribution

The IID assumption is crucial for many statistical methods, including the Central Limit Theorem.

## Sampling and the Central Limit Theorem

### Population, Sampling Frame, and Sample

- **Target Population**: The complete set of subjects we want to study
- **Sampling Frame**: The list of subjects from which the sample is actually drawn
- **Sample**: The subset of the sampling frame that is selected for analysis

### Sampling Bias

Three common types of sampling bias:

1. **Selection Bias**: Systematic error due to non-random sample selection
2. **Non-response Bias**: Bias when some subjects are less likely to respond
3. **Volunteer Bias**: People who volunteer for studies may differ from the general population

### Simple Random Sample

A simple random sample (SRS) gives each possible sample of size n from the population an equal probability of being selected.

**IID Samples**: Samples are IID when each observation is independent of others and all observations come from the same distribution.

**10% Rule**: If the sample size is less than 10% of the population size, we can treat the sample as approximately independent (even when sampling without replacement).

### Parameters vs. Statistics

- **Parameter**: A numerical characteristic of the population (fixed, unknown value)
- **Statistic**: A numerical characteristic calculated from a sample (random variable)

### Estimators

An **estimator** is a rule for calculating an estimate of a population parameter from sample data.

**Unbiased Estimator**: An estimator is unbiased if its expected value equals the true parameter value.

E[θ̂] = θ, where θ̂ is the estimator and θ is the parameter

Examples:
- Sample mean (X̄) is an unbiased estimator of the population mean (μ)
- Sample variance with Bessel's correction (n-1 denominator) is an unbiased estimator of population variance

### Distribution Types

- **Population Distribution**: Distribution of the variable in the entire population
- **Sample Distribution**: Distribution of the variable in a single sample
- **Sampling Distribution**: Distribution of a statistic calculated from many different samples

### Central Limit Theorem (CLT)

**Statement**: For a large sample size n, the sampling distribution of the sample mean is approximately normal, regardless of the population distribution.

If X₁, X₂, ..., Xₙ are IID random variables with mean μ and variance σ², then as n→∞:

X̄ ~ N(μ, σ²/n)

In other words, the sampling distribution of X̄ approaches a normal distribution with mean μ and variance σ²/n.

**Conditions for CLT to apply**:
1. The sample size n is sufficiently large (typically n ≥ 30)
2. The samples are independent
3. The population variance is finite

**Standard Error**: The standard deviation of a statistic's sampling distribution.

For the sample mean: SE(X̄) = σ/√n

If the population standard deviation σ is unknown, we can estimate it using the sample standard deviation s:

SE(X̄) ≈ s/√n

**Example**:
If a population has mean μ = 70 and standard deviation σ = 10:
- For samples of size n = 100, the sampling distribution of the mean will be approximately normal with mean 70 and standard deviation 10/√100 = 10/10 = 1
- About 95% of sample means will fall within 2 standard errors of the population mean: 70 ± 2(1) = (68, 72)

```python
# Simulating the Central Limit Theorem
import numpy as np
import matplotlib.pyplot as plt

# Generate a non-normal population (e.g., exponential distribution)
population = np.random.exponential(scale=1.0, size=100000)

# Take many samples and calculate their means
sample_size = 30
num_samples = 1000
sample_means = [np.mean(np.random.choice(population, size=sample_size)) 
                for _ in range(num_samples)]

# Plot histogram of sample means
plt.hist(sample_means, bins=30, density=True)
plt.title('Sampling Distribution of the Mean')
plt.xlabel('Sample Mean')
plt.ylabel('Density')
plt.show()
```

## Hypothesis Testing

### Purpose and Framework

Hypothesis testing is a formal procedure for making statistical decisions based on data.

**When to use**: To determine if observed effects are statistically significant or could be due to random chance.

### Null and Alternative Hypotheses

- **Null Hypothesis (H₀)**: Statement of no effect or no difference (status quo)
- **Alternative Hypothesis (H₁ or Hₐ)**: Statement that contradicts the null hypothesis

**Guidelines for defining hypotheses**:
1. The null hypothesis is a statement of no effect or no difference
2. The alternative hypothesis is what you're trying to demonstrate
3. Hypotheses should be mutually exclusive
4. At least one hypothesis should be falsifiable

**Example**:
Testing if a coin is fair:
- H₀: The coin is fair (p = 0.5)
- Hₐ: The coin is not fair (p ≠ 0.5)

### Test Statistics

A test statistic is a value calculated from sample data that is used to determine how likely the observed data is under the null hypothesis.

**Guidelines for selecting test statistics**:
1. For one-sided alternatives (greater than), use the raw statistic
2. For one-sided alternatives (less than), use the negative of the raw statistic
3. For two-sided alternatives, use the absolute difference or distance measure

**Common test statistics**:
- For proportions: number of successes, proportion, or z-score
- For means: t-statistic
- For distributions: chi-square statistic or Total Variation Distance (TVD)

**Total Variation Distance (TVD)**: Half the sum of absolute differences between observed and expected proportions.

TVD = (1/2) × Σ|observed_proportion - expected_proportion|

### Significance Level (α)

The significance level (α) is the probability of rejecting the null hypothesis when it's actually true (Type I error probability).

Common values: 0.05 (5%), 0.01 (1%), 0.10 (10%)

### P-value

The p-value is the probability of observing a test statistic as extreme as, or more extreme than, the one observed, assuming the null hypothesis is true.

**Interpreting p-values**:
- p-value < α: Reject null hypothesis
- p-value ≥ α: Fail to reject null hypothesis

**Visual interpretation**: Area in the tail(s) of the null distribution beyond the observed test statistic.

```python
def compute_p_value(empirical_dist, observed_ts, alternative='greater'):
    """
    Calculate p-value from empirical distribution
    
    Parameters:
    empirical_dist: Array of test statistics under the null hypothesis
    observed_ts: Observed test statistic
    alternative: 'greater', 'less', or 'two-sided'
    
    Returns:
    p_value: Probability of observing a test statistic as extreme as observed_ts
    """
    if alternative == 'greater':
        return np.mean(empirical_dist >= observed_ts)
    elif alternative == 'less':
        return np.mean(empirical_dist <= observed_ts)
    elif alternative == 'two-sided':
        return np.mean(np.abs(empirical_dist) >= np.abs(observed_ts))
```

### Types of Hypothesis Tests

#### 1. One-Sample Tests
Compare a sample to a theoretical model or known population parameter.

**Example**: Testing if a coin is fair
```python
# Observed: 60 heads in 100 tosses
observed_proportion = 0.6
expected_proportion = 0.5
n = 100

# Simulate under the null hypothesis (fair coin)
simulated_props = [np.mean(np.random.choice([0, 1], size=n, p=[0.5, 0.5])) 
                   for _ in range(10000)]

# Calculate p-value (two-sided test)
p_value = np.mean(np.abs(np.array(simulated_props) - 0.5) >= 
                  np.abs(observed_proportion - 0.5))
```

#### 2. Chi-Square Tests for Multinomial Distributions
Test if observed frequencies match expected frequencies across multiple categories.

**Example**: Testing distribution of dice rolls
```python
# Observed counts for dice faces 1-6
observed = [10, 15, 12, 9, 20, 14]
n = sum(observed)
expected = [n/6] * 6  # Equal probability for fair die

# Chi-square statistic
chi_square = sum((observed[i] - expected[i])**2 / expected[i] for i in range(6))
```

#### 3. A/B Tests
Compare outcomes between two groups to detect differences.

**Example**: Testing if treatment A produces different results than treatment B
```python
# Group A results
group_A = [25, 30, 28, 32, 27, 31]
# Group B results
group_B = [20, 22, 21, 24, 23, 25]

# Test statistic: difference in means
observed_diff = np.mean(group_A) - np.mean(group_B)

# Permutation test
combined = np.array(group_A + group_B)
n_A = len(group_A)
n_B = len(group_B)

def one_permutation():
    np.random.shuffle(combined)
    new_A = combined[:n_A]
    new_B = combined[n_A:]
    return np.mean(new_A) - np.mean(new_B)

# Simulate under null hypothesis
simulated_diffs = [one_permutation() for _ in range(10000)]

# Calculate p-value (two-sided)
p_value = np.mean(np.abs(simulated_diffs) >= np.abs(observed_diff))
```

### Permutation Tests

Permutation tests simulate the null hypothesis by randomly reassigning observations to groups.

**Steps**:
1. Calculate observed test statistic
2. Combine all data
3. Repeatedly:
   - Randomly shuffle data and reassign to groups
   - Calculate test statistic for each shuffled data
4. Compare observed statistic to the distribution of simulated statistics

### Statistical Power and Errors

**Type I Error**: Rejecting a true null hypothesis (false positive)
- Probability = α (significance level)

**Type II Error**: Failing to reject a false null hypothesis (false negative)
- Probability = β

**Statistical Power**: Probability of correctly rejecting a false null hypothesis
- Power = 1 - β

**Factors affecting power**:
1. Effect size: Larger effects are easier to detect
2. Sample size: Larger samples increase power
3. Significance level: Higher α increases power but also increases Type I error rate

### P-hacking and Multiple Testing

**P-hacking**: Manipulating data or analysis to achieve statistically significant results.

**Guidelines to avoid p-hacking**:
1. Pre-specify hypotheses before seeing the data
2. Report all tests performed, not just significant ones
3. Adjust for multiple comparisons
4. Use consistent stopping rules for data collection

## Confidence Intervals

### Definition and Interpretation

A confidence interval is a range of values that is likely to contain the true population parameter with a specified confidence level.

**Interpretation**: If we were to take many samples and compute a 95% confidence interval for each sample, then approximately 95% of the intervals would contain the true population parameter.

**Incorrect interpretation**: There is a 95% probability that the true parameter is in the confidence interval.

### Bootstrap Percentile Method

The bootstrap method resamples from the observed data to estimate the sampling distribution of a statistic.

**Steps**:
1. Take a random sample from the population
2. Resample with replacement from the sample (same size as original sample)
3. Calculate the statistic of interest for each resample
4. Repeat steps 2-3 many times to create a bootstrap distribution
5. Use percentiles of this distribution to create a confidence interval

```python
def bootstrap_ci(data, statistic, confidence=0.95, n_resamples=10000):
    """
    Calculate bootstrap confidence interval
    
    Parameters:
    data: Original sample data
    statistic: Function that computes the statistic of interest
    confidence: Confidence level (e.g., 0.95 for 95% CI)
    n_resamples: Number of bootstrap resamples
    
    Returns:
    lower, upper: Lower and upper bounds of the confidence interval
    """
    bootstrap_stats = []
    n = len(data)
    
    for _ in range(n_resamples):
        # Resample with replacement
        resample = np.random.choice(data, size=n, replace=True)
        # Calculate statistic
        bootstrap_stats.append(statistic(resample))
    
    # Calculate percentiles
    alpha = (1 - confidence) / 2
    lower_percentile = alpha * 100
    upper_percentile = (1 - alpha) * 100
    
    lower = np.percentile(bootstrap_stats, lower_percentile)
    upper = np.percentile(bootstrap_stats, upper_percentile)
    
    return lower, upper
```

**Key assumptions and limitations**:
1. The original sample is representative of the population
2. The bootstrap distribution approximates the true sampling distribution
3. Resampling is done with replacement
4. The sample size should be reasonably large

### CLT-Based Confidence Intervals

For large samples, we can use the Central Limit Theorem to create confidence intervals for means and proportions.

#### For Population Mean (μ)

95% CI for μ = X̄ ± 1.96 × (s/√n)

Where:
- X̄ is the sample mean
- s is the sample standard deviation
- n is the sample size
- 1.96 is the z-score for 95% confidence level

**Example**:
Sample mean = 170 cm, sample standard deviation = 10 cm, n = 400
95% CI = 170 ± 1.96 × (10/√400) = 170 ± 1.96 × 0.5 = 170 ± 0.98 = (169.02, 170.98)

#### For Population Proportion (p)

95% CI for p = p̂ ± 1.96 × √[p̂(1-p̂)/n]

Where:
- p̂ is the sample proportion
- n is the sample size

### Sample Size Determination

To determine the required sample size for a desired confidence interval width:

**For proportions**:
n = (z²p̂(1-p̂)) / E²

Where:
- z is the z-score for the desired confidence level
- p̂ is the estimated proportion (use 0.5 if unknown)
- E is the desired margin of error (half the width of the CI)

**For means**:
n = (z²σ²) / E²

Where:
- z is the z-score for the desired confidence level
- σ is the population standard deviation (use sample SD if unknown)
- E is the desired margin of error

**Example**:
For a 95% confidence interval for a proportion with margin of error 0.025:
n = (1.96²×0.5×0.5) / 0.025² = 3.8416 × 0.25 / 0.000625 = 1537

### Using Confidence Intervals for Hypothesis Testing

A confidence interval can be used for hypothesis testing:
- If a value is outside the CI, we can reject the null hypothesis that the parameter equals that value
- If a value is inside the CI, we fail to reject the null hypothesis

**Example**:
If the 95% CI for a proportion is (0.45, 0.55) and we're testing if p = 0.5:
- Since 0.5 is inside the interval, we fail to reject H₀: p = 0.5
- If we're testing if p = 0.6, we would reject H₀ since 0.6 is outside the interval
