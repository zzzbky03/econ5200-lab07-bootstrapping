# Diagnosing Broken Confidence Intervals

## Objective
I checked when bootstrap and normal confidence intervals fail, measured how often they actually cover the truth, and fixed one of them with a cluster bootstrap.

## Methodology
- Bootstrapped the median age of 32,561 people from the UCI Adult census data and compared the percentile interval with scipy's BCa method
- Ran 400 simulated experiments (n = 50, skewed lognormal data) to measure the real coverage of nominal 95% parametric and percentile bootstrap intervals for the mean and the median
- Simulated wages for workers in 25 firms and compared a naive bootstrap (resampling workers) with a cluster bootstrap (resampling firms), then checked coverage over many new panels
- Used a bootstrap_utils.py module with percentile, i.i.d. and cluster bootstrap functions
- Had an AI write a coverage function, revised my prompt once, and checked its results against my own coverage numbers

## Key Findings
- The median age bootstrap had an SE of 0.000000 because ages are whole numbers with many ties at the median. BCa returned nan with a warning instead of an interval.
- For the mean, the parametric interval covered 92.5% and the bootstrap 92.0%, so the bootstrap did not fix the skew. For the median, the parametric interval covered only 66.8% because it is centered on the mean, while the bootstrap covered 92.8%.
- Treating clustered workers as independent gave an interval 5.7x too narrow with only 19.5% coverage. Resampling whole firms brought coverage to 91.0%.
- The AI's first coverage function picked random seeds and gave 24.0% and 91.5%, which is within noise but not an exact match. After I told it to use seed + t, it matched my numbers exactly (19.5% and 91.0%).
