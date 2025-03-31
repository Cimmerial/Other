# Joint Random Variables (Lesson 15)

## (a) Using joint PMFs for calculating probabilities
- **Explanation**: A joint probability mass function (PMF) gives the probability that two discrete random variables take specific values simultaneously.
- **Equations**: 
  - Joint PMF: P(X=x, Y=y)
  - Conditional probability: P(X=x|Y=y) = P(X=x, Y=y)/P(Y=y)
  - Marginal probability: P(X=x) = ∑y P(X=x, Y=y)
- **Summary**: The joint PMF allows us to find joint, conditional, and marginal probabilities for discrete random variables.

## (b) Multinomial random variables
- **Explanation**: Multinomial random variables represent outcomes from experiments with multiple possible results, extending the binomial distribution to more than two categories.
- **Equation**: P(X₁=x₁, X₂=x₂, ..., Xₖ=xₖ) = (n!/(x₁!x₂!...xₖ!)) × p₁^x₁ × p₂^x₂ × ... × pₖ^xₖ
- **Summary**: Multinomial distributions model the probability of counts for multiple categories with fixed probabilities and total number of trials.

## (c) Simulating multinomial distributions with Python
- **Explanation**: Python libraries like NumPy and SciPy can generate random samples from multinomial distributions.
- **Code example**: `numpy.random.multinomial(n, pvals, size)` where n is the number of trials, pvals are the probabilities of each outcome, and size is the number of samples.
- **Summary**: Simulation allows empirical verification of theoretical properties and approximation of complex scenarios.

## (d) Independence definition for random variables
- **Explanation**: Two random variables are independent if knowledge about one variable provides no information about the other.
- **Equation**: X and Y are independent if P(X=x, Y=y) = P(X=x) × P(Y=y) for all x and y
- **Summary**: Independence means the joint probability equals the product of the marginal probabilities.

## (e) Testing independence for discrete random variables
- **Explanation**: To determine if discrete random variables are independent, verify the mathematical definition for all possible values.
- **Method**: Calculate P(X=x, Y=y) and compare with P(X=x) × P(Y=y) for all x, y pairs
- **Summary**: If the joint probability equals the product of marginals for all value combinations, the variables are independent.

## (f) Converting to standard units
- **Explanation**: Standardization transforms random variables to have mean 0 and standard deviation 1.
- **Equation**: Z = (X - μ)/σ, where μ is the mean and σ is the standard deviation
- **Summary**: Standardization enables comparison between variables with different scales and units.

## (g) Covariance and correlation
- **Explanation**: Covariance measures how two variables vary together; correlation normalizes this to range from -1 to 1.
- **Equations**: 
  - Covariance: Cov(X,Y) = E[(X-μₓ)(Y-μᵧ)] = E[XY] - E[X]E[Y]
  - Correlation: ρ = Cov(X,Y)/(σₓσᵧ)
- **Summary**: Correlation provides a standardized measure of linear relationship strength between variables.

## (h) Association, correlation, and causation
- **Explanation**: Association is any relationship between variables; correlation is a specific measure of linear relationship; causation implies one variable directly affects another.
- **Distinctions**: 
  - Association: Variables are related in some way
  - Correlation: Specific measure of linear relationship
  - Causation: Change in one variable causes change in another
- **Summary**: Correlation does not imply causation; other factors or reverse causality may explain correlations.

## (i) Expectation and variance for sums of random variables
- **Explanation**: These formulas allow calculation of the expected value and variance when random variables are added.
- **Equations**: 
  - E[X + Y] = E[X] + E[Y]
  - Var(X + Y) = Var(X) + Var(Y) + 2Cov(X,Y)
  - For independent variables: Var(X + Y) = Var(X) + Var(Y)
- **Summary**: These properties are essential for analyzing sums of random variables in many applications.

## (j) IID random variables
- **Explanation**: Independent and Identically Distributed (IID) random variables follow the same probability distribution and are mutually independent.
- **Definition**: Variables X₁, X₂, ..., Xₙ are IID if:
  1. They are independent
  2. They all follow the same probability distribution
- **Summary**: The IID assumption is fundamental in many statistical methods, particularly in sampling.

# Sampling and The Central Limit Theorem (Lessons 16-17)

## (a) Target population, sampling frame, and sample
- **Explanation**: These terms describe different aspects of the sampling process.
- **Definitions**:
  - Target population: The entire group about which inferences are made
  - Sampling frame: The list of individuals from which the sample is actually selected
  - Sample: The subset of individuals actually observed
- **Summary**: Discrepancies between these can lead to biased inferences.

## (b) Three common types of sampling bias
- **Explanation**: Sampling biases systematically distort the representativeness of a sample.
- **Types**:
  1. Selection bias: Non-random selection of participants
  2. Voluntary response bias: Self-selection of participants
  3. Nonresponse bias: Systematic differences between responders and non-responders
- **Summary**: These biases can invalidate inference from samples to populations.

## (c) Simple random sample
- **Explanation**: A sampling method where each possible sample of size n has an equal probability of being selected.
- **Definition**: Each individual in the population has an equal probability of selection, and selections are independent.
- **Summary**: Simple random sampling is the foundation for many statistical methods and helps ensure representativeness.

## (d) IID sampling and the 10% rule
- **Explanation**: IID sampling requires independent observations from the same distribution.
- **10% rule**: If the sample size is less than 10% of the population size, we can treat observations as approximately independent even when sampling without replacement.
- **Summary**: The 10% rule provides a practical guideline for when to consider sampling with replacement vs. without replacement.

## (e) Parameters vs. statistics
- **Explanation**: Parameters describe population characteristics, while statistics describe sample characteristics.
- **Definitions**:
  - Parameter: A numerical characteristic of a population (fixed, unknown value)
  - Statistic: A numerical characteristic of a sample (random variable)
- **Summary**: Statistics are random because they depend on which sample is drawn; parameters are fixed but typically unknown.

## (f) Estimators and unbiasedness
- **Explanation**: An estimator is a rule for calculating an estimate of a population parameter from a sample.
- **Definition of unbiased**: An estimator θ̂ is unbiased for θ if E[θ̂] = θ
- **Summary**: Unbiased estimators provide estimates that are correct on average over many samples.

## (g) Population, sample, and sampling distributions
- **Explanation**: These distributions represent different aspects of the statistical inference process.
- **Definitions**:
  - Population distribution: Distribution of the variable across the entire population
  - Sample distribution: Distribution of the variable in a specific sample
  - Sampling distribution: Distribution of a statistic across all possible samples
- **Summary**: Understanding these different distributions is crucial for statistical inference.

## (h) Simulating empirical sampling distributions
- **Explanation**: Simulation involves generating many random samples and calculating the statistic for each.
- **Method**: 
  1. Draw many random samples from a population
  2. Calculate the statistic for each sample
  3. Plot the distribution of the statistic
- **Summary**: Simulation helps visualize and understand theoretical properties of sampling distributions.

## (i) Central Limit Theorem (CLT)
- **Explanation**: The CLT states that the sampling distribution of the sample mean approaches a normal distribution as sample size increases.
- **Equation**: For large n, X̄ ~ N(μ, σ²/n) approximately
- **Conditions**: The sample must be IID and have finite variance; sample size should be "large enough" (usually n ≥ 30)
- **Usage**: The CLT allows normal approximation for inferences about means from large samples, even for non-normal populations.

## (j) Standard error
- **Explanation**: The standard error is the standard deviation of a statistic's sampling distribution.
- **Equation**: For the sample mean, SE(X̄) = σ/√n
- **Summary**: Standard error measures the typical deviation between a statistic and the parameter it estimates.

# Hypothesis Tests (Lessons 18-20)

## (a) When to use hypothesis tests
- **Explanation**: Hypothesis tests are used to determine if sample data provide sufficient evidence against a specified model or claim.
- **Use cases**: Comparing samples to hypothesized models, comparing different groups, testing for relationships between variables
- **Summary**: Hypothesis tests provide a formal framework for making statistical decisions under uncertainty.

## (b) Implementing hypothesis tests comparing samples to models
- **Explanation**: These tests determine if observed data are consistent with a hypothesized model.
- **Steps**:
  1. Define null and alternative hypotheses
  2. Choose and calculate a test statistic
  3. Determine the p-value
  4. Make a decision based on significance level
- **Summary**: The process provides a structured way to assess evidence against a model.

## (c) Testing consistency with a multinomial distribution
- **Explanation**: These tests determine if observed categorical counts match expected probabilities.
- **Test statistic**: Often chi-square: χ² = ∑ (Observed - Expected)²/Expected
- **Summary**: These tests are useful for categorical data when comparing observed frequencies to theoretical expectations.

## (d) Null and alternative hypotheses
- **Explanation**: The null hypothesis (H₀) represents the status quo or no effect; the alternative hypothesis (H₁) represents what we're looking for evidence to support.
- **Guidelines**:
  - H₀ should be a specific claim (e.g., parameter equals specific value)
  - H₁ is typically what the researcher wants to demonstrate
  - For scientific questions, H₀ often represents "no effect" or "no difference"
- **Summary**: Clear specification of hypotheses is essential for meaningful hypothesis testing.

## (e) Test statistics and their selection
- **Explanation**: A test statistic quantifies how far sample data deviate from what's expected under H₀.
- **Guidelines for selection**:
  - For means: t-statistic or z-statistic
  - For proportions: z-statistic
  - For categorical data: chi-square statistic
  - For comparing distributions: Total Variation Distance (TVD)
- **Summary**: The test statistic should be sensitive to deviations that matter for the alternative hypothesis.

## (f) Significance level
- **Explanation**: The significance level (α) is the threshold for deciding when to reject H₀.
- **Selection guidelines**: Typically 0.05, but can be 0.01 for more stringent tests or 0.10 for more exploratory research
- **Usage**: Reject H₀ if p-value < α
- **Summary**: The significance level controls the false positive rate (Type I error rate).

## (g) P-value interpretation
- **Explanation**: The p-value is the probability of observing a test statistic at least as extreme as the one calculated, assuming H₀ is true.
- **Mathematical definition**: p = P(Test statistic ≥ observed value | H₀ true)
- **Visual interpretation**: Area in the tail of the sampling distribution beyond the observed test statistic
- **Summary**: Smaller p-values indicate stronger evidence against H₀.

## (h) Possible conclusions in hypothesis testing
- **Explanation**: Hypothesis tests lead to one of two decisions, with specific interpretations.
- **Types of conclusions**:
  1. Reject H₀: Evidence suggests H₀ is false; supports H₁
  2. Fail to reject H₀: Insufficient evidence against H₀; does not prove H₀ is true
- **Summary**: Hypothesis tests never "prove" hypotheses but provide evidence for or against them.

## (i) A/B hypothesis tests
- **Explanation**: A/B tests compare outcomes between two groups (A and B) that receive different treatments.
- **Implementation**:
  1. Randomly assign subjects to groups
  2. Apply different treatments
  3. Measure outcomes
  4. Test for significant differences
- **Summary**: A/B tests allow statistical comparison between treatment options.

## (j) Using permutations to simulate the null
- **Explanation**: Permutation tests shuffle observed data to simulate the null hypothesis of no difference between groups.
- **Method**:
  1. Combine data from all groups
  2. Randomly reassign data to groups many times
  3. Calculate test statistic for each permutation
  4. Compare observed test statistic to permutation distribution
- **When to use**: When assumptions for parametric tests aren't met or for complex test statistics

## (k) Causal conclusions from A/B tests
- **Explanation**: Causal conclusions require random assignment and proper experimental design.
- **Requirements**:
  1. Random assignment to groups
  2. Sufficient sample size
  3. Control of confounding variables
  4. Proper blinding where appropriate
- **Summary**: Well-designed randomized experiments can support causal conclusions.

## (l) Empirical vs. theoretical p-values
- **Explanation**: Empirical p-values are calculated from simulations; theoretical p-values use mathematical formulas.
- **Differences**:
  - Empirical: Based on simulated sampling distribution, approximates true p-value
  - Theoretical: Based on known sampling distribution (e.g., normal, t, chi-square)
- **Summary**: Empirical p-values are useful when theoretical distributions aren't applicable.

## (m) Statistical power and influencing factors
- **Explanation**: Statistical power is the probability of correctly rejecting a false null hypothesis.
- **Mathematical definition**: Power = P(Reject H₀ | H₁ is true)
- **Factors affecting power**:
  1. Sample size (larger samples increase power)
  2. Effect size (larger effects are easier to detect)
  3. Significance level (less stringent α increases power but also increases Type I error risk)
- **Summary**: Higher power increases the ability to detect true effects.

## (n) Type I and Type II errors
- **Explanation**: These are the two types of errors possible in hypothesis testing.
- **Definitions**:
  - Type I error: Rejecting H₀ when it's true (false positive)
  - Type II error: Failing to reject H₀ when it's false (false negative)
- **Minimization**:
  - Decrease α to reduce Type I errors
  - Increase sample size or power to reduce Type II errors
- **Summary**: There's a trade-off between these error types; reducing one typically increases the other.

## (o) P-hacking and prevention
- **Explanation**: P-hacking refers to manipulating data or analyses until statistically significant results appear.
- **Prevention guidelines**:
  1. Pre-register hypotheses and analysis plans
  2. Report all analyses performed, not just significant results
  3. Use appropriate corrections for multiple comparisons
  4. Validate findings with independent replication
- **Summary**: P-hacking inflates false positive rates and reduces scientific credibility.

# Confidence Intervals (Lessons 21-22)

## (a) Confidence interval definition and interpretation
- **Explanation**: A confidence interval provides a range of plausible values for a population parameter.
- **Interpretation**: If we were to repeat our sampling process many times, approximately (confidence level)% of the resulting intervals would contain the true parameter value.
- **Summary**: Confidence intervals quantify uncertainty in parameter estimates.

## (b) Bootstrap percentile method
- **Explanation**: The bootstrap method resamples with replacement from the original sample to estimate sampling distributions.

### (i) Resample size
- **Explanation**: Bootstrap resamples should be the same size as the original sample.
- **Reason**: This preserves the same level of sampling variability as in the original sampling process.

### (ii) Resampling with replacement
- **Explanation**: Resampling with replacement is necessary to simulate the sampling process.
- **Reason**: Without replacement, all bootstrap samples would be identical to the original sample.

### (iii) Bootstrap reasoning
- **Explanation**: The bootstrap treats the sample as a proxy for the population.
- **Reasoning**: The relationship between sample and population is similar to the relationship between bootstrap resamples and the original sample.
- **Why it works**: As sample size increases, the sample distribution approaches the population distribution.

### (iv) Bootstrap assumptions and limitations
- **Explanation**: The bootstrap has specific requirements and limitations.
- **Assumptions**:
  - The original sample is representative of the population
  - Observations are IID
  - Sample size is sufficiently large
- **Limitations**:
  - Less reliable for small samples
  - May not work well for extreme percentiles
  - Cannot overcome fundamental sampling bias

## (c) CLT-based confidence intervals for the mean
- **Explanation**: These intervals use the normal approximation from the Central Limit Theorem.
- **Equation**: x̄ ± z₍α/₂₎ × (σ/√n), where z₍α/₂₎ is the critical value from the standard normal distribution
- **Summary**: For large samples, these intervals provide good approximations for means.

## (d) Confidence intervals for proportions
- **Explanation**: These intervals estimate population proportions from sample proportions.
- **Equation**: p̂ ± z₍α/₂₎ × √(p̂(1-p̂)/n)
- **Summary**: These intervals are useful for binary data and use the normal approximation to the binomial.

## (e) Sample size determination
- **Explanation**: This involves calculating the sample size needed for a desired confidence interval width.
- **Equation**: n = (z₍α/₂₎ × σ/E)², where E is the margin of error
- **Summary**: Larger sample sizes produce narrower confidence intervals, increasing precision.

## (f) Using confidence intervals for hypothesis testing
- **Explanation**: Confidence intervals can be used to perform hypothesis tests.
- **Method**: If a hypothesized parameter value falls outside the confidence interval, we reject the null hypothesis at the corresponding significance level.
- **When to use**: Confidence intervals provide more information than hypothesis tests alone by showing the range of plausible values.
- **Summary**: A 95% confidence interval not containing a value is equivalent to rejecting a hypothesis test with that value at α = 0.05.
