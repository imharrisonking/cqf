<div class="center">

<img src="2023_07_29_7d6ca5e0312d2c7eed33g-1" alt="image" />

</div>

Kannan Singaravelu, CQF

June 2023

There are several methods to evaluate risk for an individual stock or a
portfolio, such as variance, standard deviation of returns, et al. But,
those measures do not consider a probability distribution. However, many
risk managers prefer a simple measure called Value at Risk (VaR). VaR is
one of the most important metrics that is used to measure the risk
associated with a financial position or a portfolio of financial
instruments and can be defined as the maximum loss with a confidence
level over a predetermined period.

Let’s say that the 1-day 95% VaR of our portfolio is $100. This means
that 95% of the time, it is expected that - under normal market
conditions - we will not lose more than $100 by holding our portfolio
over one day.

Three approaches that are commonly used in the industry are

-   Parametric

-   Historical

-   Monte Carlo

# Import Libraries

We’ll import the required libraries that we’ll use in this example.

<div class="center">

<img src="2023_07_29_7d6ca5e0312d2c7eed33g-1(1)" alt="image" />

</div>

import warnings

warnings.filterwarnings (’ignore’)

pd.set_option (’display.precision’, 4)

# Retrieve Data

We will use the Indian stocks as before to build for calculation of VaR.

<div class="center">

<img src="2023_07_29_7d6ca5e0312d2c7eed33g-2" alt="image" />

</div>

\[ \]: \# Calculate daily returns

returns = df ⋅ pct_change () ⋅ dropna ()

returns.head()

\[ \]: \#Plot histogram

px.histogram(returns,

 histnorm=’probability density’, 

title=’Histogram of Returns’,

barmode=’relative’)

## Parametric VaR

The Variance-covariance is a parametric method which assumes (almost
always) that the returns are normally distributed. In this method, we
first calculate the mean and standard deviation of the returns to derive
the risk metric. Based on the assumption of normality, we can
generalise, *V**a**R*= position  \* (*μ*−*z*\**σ*)

<div class="center">

| Confidence Level | Value At Risk     |
|:-----------------|:------------------|
| 90%              | *μ* − 1.29 \* *σ* |
| 95%              | *μ* − 1.64 \* *σ* |
| 99%              | *μ* − 2.33 \* *σ* |

</div>

where, *μ* is the return, *σ* is the volatility and *z* is the number of
standard deviation from the mean.

<div class="center">

<img src="2023_07_29_7d6ca5e0312d2c7eed33g-2(1)" alt="image" />

</div>

\[ \]: \# number of stdev from the mean

stats.norm.ppf (0.01)

\[ \]: \# Ouput results in tabular format

<div class="center">

<img src="2023_07_29_7d6ca5e0312d2c7eed33g-3" alt="image" />

</div>

header = \[’Confidence Level’, ’Value At Risk’\]

print (tabulate (table, headers=header))

### Normality Test

In the Parametric VaR, we assumed that the returns are normally
distributed. However, in the real world, we know that stock / portfolio
returns do not necessarily follow a normal distribution. Let’s perform a
quick check to determine the normality of the underlying returns and see
whether we need to modify our approach in deriving the VaR numbers.

Shapiro The Shapiro-Wilk test is a test of normality and is used to
determine whether or not a sample comes from a normal distribution.

\[ \]: \# normality test

stats.shapiro(stockreturn)

Our null hypothesis is that HDFCBank stock daily returns follows a
normal distribution. Since the p-value is less than 0.05 , we reject the
null hypothesis. We have sufficient evidence to say that the sample data
does not come from a normal distribution. This result shouldn’t be
surprising as the data comes from an empirical distribution.

Anderson-Darling Alternatively, we can perform an Anderson-Darling Test.
It is a goodness of fit test that measures how well the data fit a
specified distribution. This test is most commonly used to determine
whether or not the data follow a normal distribution.

\[ \]: \# normality test

stats. anderson (stockreturn)

Based on the above result, the null hypothesis is rejected since the
test statistic value is much higher than the critical value of 1.09 even
at 1% significance level.

## Modified VaR

Standard normal distribution have a zero mean, unit variance, zero
skewness, and its kurtosis of 3. However, we now know the distribution
is not normal and in such scenario, the skewness and excess kurtosis of
many stock returns are not zero. As a consequence, the modified VaR was
developed to utilize those four moments instead of the first two
moments.

*m**V**a**R* =  position  \* (*μ*−*t*\**σ*)

where,

$$t=z+\\frac{1}{6}\\left(z^{2}-1\\right) s+\\frac{1}{24}\\left(z^{3}-3 z\\right) k-\\frac{1}{36}\\left(2 z^{3}-5 z\\right) s^{2}$$

*μ* is the return, *σ* is the volatility, *s* is the skewness, *k* is
the kurtosis and *z* is the absolute number of standard deviation from
the mean.

\[ \]: \# First four moments

dist = OrderedDict ({

’Mean’ : np. mean (returns \[’HDFCBANK ’\]) ),

’Variance’ : np.std (returns \[’HDFCBANK’\]),

’Skew’ : stats. skew (returns \[’HDFCBANK ’\]),

’Kurtosis’: stats.kurtosis (returns \[’HDFCBANK’\])

})

pprint (dist)

\[ \]: \# Specify params

**z**= abs (stats.norm.ppf (0.01))

s= stats.skew(stockreturn)

k= stats.kurtosis (stockreturn)

t = **z** + 1/6 \* (**z**\*\*2−1) \*  s + 1/24 \* (**z**\*\*3−3\***z**) \* k − 1/36 \* (2\***z**\*\*3−5\***z**) \* **s** \*  \* 2

\# Calculate VaR at difference confidence level

mVaR_99 = (mean-t\*stdev)

mVaR_99

## Historical VaR

Asset returns do not necessarily follow a normal distribution. An
alternative is to use sorted returns to evaluate a VaR. This method uses
historical data where returns are sorted in ascending order to calculate
maximum possible loss for a given confidence level.

\[ \]: \#Use quantile function for Historical VaR

hVaR_90 = returns \[’HDFCBANK’\] . quantile (0.10)

hVaR_95 = returns \[’HDFCBANK’\] . quantile (0.05)

hVaR_99 = returns \[’HDFCBANK’\] . quantile (0.01)

\[ \]: \# Ouput results in tabular format

htable = \[ ’90%’, hVaR_90\], \[’95%’, hVaR_95\], \[’99%’, hVaR_99\]\]

print (tabulate (htable, headers=header))

## MonteCarlo VaR

The Monte Carlo simulation approach has a number of similarities to
historical simulation. It allows us to use actual historical
distributions rather than having to assume normal returns. As returns
are assumed to follow a normal distribution, we could generate *n*
simulated returns with the same mean and standard deviation (derived
from the daily returns) and then sorted in ascending order to calculate
maximum possible loss for a given confidence level. \[ \]: \# Set seed
for reproducibility

np.random.seed (42)

\# Number of simulations

n<sub>−</sub>sims  = 5000

\# Simulate returns and sort

sim_returns  = *n**p*⋅ random.normal (mean, stdev, n_sims)

\# Use percentile function for MCVaR

MCVaR_90 = np.percentile (sim_returns, 10)

MCVaR_95 = np.percentile (sim_returns, 5)

MCVaR_99 = np.percentile ( sim_returns, 1)

\[ \]: \# Ouput results in tabular format

mctable  = \[\[′90%′, MCVaR_90\], \[’95%’, MCVaR_95\], \[’99%’,
MCVaR_99\]\]

print (tabulate (mctable, headers=header))

## Scaling VaR

Now, let’s calculate VaR over a 5-day period. To scale it, multiply by
square root of time.

$$V a R=\\text { position } \*(\\mu-z \* \\sigma) \* \\sqrt{T}$$

where, *T* is the horizon or forecast period.

\[ \]: \# VaR Scaling

forecast_days  = 5

f_VaR_90 = VaR_90\*np.sqrt (forecast_days)

f_VaR_95 = VaR_95\*np.sqrt (forecast_days)

f_VaR_99 = VaR_99\*np.sqrt (forecast_days)

\[ \]: \# Ouput results in tabular format

ftable = \[ ’90%’, f_VaR_90\], \[’95%’, f_VaR_95\], \[’99%’, f_VaR_99\]
\]

fheader = \[’Confidence Level’, ’5-Day Forecast Value At Risk’\]

print (tabulate (ftable, headers = fheader ) )

\[ \]: \# Plot Scaled VaR

sVaR= pd.DataFrame (\[−100\*VaR\_99\*np⋅sqrt(x) for x in range(100)\], ⊔

↪ columns  = \[ ’ScaledVaR’\])

px.scatter (sVaR, sVaR. index, ’ScaledVaR’, title=’Scaled VaR’, labels
 = { ’index’:

↪ ’Horizon’})

## Expected Short Fall

VaR is a reasonable measure of risk if assumption of normality holds.
Else, we might underestimate the risk if we observe a fat tail or
overestimate the risk if tail is thinner. Expected shortfall or
Conditional Value at Risk - CVaR - is an estimate of expected shortfall
sustained in the worst 1 - *x*% of scenarios. It is defined as the
average loss based on the returns that are lower than the VaR threshold.
Assume that we have *n* return observations, then the expected shortfall
is

$$C V a R=\\frac{1}{n} \* \\sum\_{i=1}^{n} R\_{i}\\left\[R \\leq h V a R\_{c l}\\right\]$$

where, *R* is returns, *h**V**a**R* is historical VaR and *c**l* is the
confidence level.

\[ \]: \# Calculate CVar

CVaR_90 = returns \[’HDFCBANK’ \] \[returns \[’HDFCBANK ’
\]\<=*h**V**a**R*<sub>−</sub>90\].mean ()

CVaR_95 = returns \[’HDFCBANK ’\] \[returns \[’HDFCBANK ’
\]\<=*h**V**a**R*\_95\]⋅ mean ()

CVaR_99 = returns \[’HDFCBANK’\] \[returns \[’HDFCBANK ’
\]\<=*h**V**a**R*\_99\].*m**e**a**n*()

\[ \]: \# Ouput results in tabular format

ctable  = \[\[′90%′, CVaR_90\], \[’95%’, CVaR_95\], \[’99%’, CVaR_99\]
\]

cheader = \[’Confidence Level’, ’Conditional Value At Risk’\]

print (tabulate (ctable, headers=cheader) )

## Portfolio VaR

If we know the returns and volatilities of all the assets in the
portfolio, we can derive portfolio VaR. We will now derive VaR of
minimum variance portfolio consisting of Indian stocks.

\[28\]: \# Weights from Minimum Variance Portfolio

wts = np. array (\[1.928e−01,2.367e−01,2.099e−01,7.286e−02,2.878e−01\])

\# Portfolio mean returns and volatility

port_mean = wts.T (c) returns.mean ()

port_stdev  = *n**p*.*s**q**r**t*( multi_dot (\[ wts.T, returns.cov(),
wts \]))

pVaR = stats.norm.ppf (1-0.99, port_mean, port_stdev)

print (f"Mean: {port_mean}, Stdev: {port_stdev}, pVaR: {pVaR}")

# GARCH

Asset price volatility is central to derivatives pricing. It is defined
as measure of price variability over certain period of time. In essence,
it describes standard deviation of returns. There are different types of
volatility: Historical, Implied, Forward. In most cases, we assume
volatility to be constant, which is clearly not true and numerous
studies have been dedicated to estimate this variable, both in academia
and industry.

# Volatility

Volatility estimation by statistical means assume equal weights to all
returns measured over the period. We know that over 1-day, the mean
return is small as compared to standard deviation. If we consider a
simple *m*-period moving average, where *σ*<sub>*n*</sub> is the
volatility of return on day n, then with *ū* ≈ 0, we have

$$\\sigma\_{n}^{2}=\\frac{1}{m} \\sum\_{i=1}^{m} u\_{n-i}^{2}$$

where, *u* is return and *σ*<sup>2</sup> is the variance.

# ARCH

However, any large return within this *n* period will elevate the
volatility until it drops out of the sample. Further, we observe
volatility is mean reverting and tends to vary about a long term mean.
To address this effect, we adopt to the weighting schemes.

$$\\begin{gathered}
\\sigma\_{n}^{2}=\\gamma \\bar{\\sigma}^{2}+\\sum\_{i=1}^{m} \\alpha\_{i} u\_{n-i}^{2} \\\\
\\sigma\_{n}^{2}=\\omega+\\sum\_{i=1}^{m} \\alpha\_{i} u\_{n-i}^{2}
\\end{gathered}$$

where, *ω* = *γ**σ̄*<sup>2</sup> and weights must sum to 1 .

This is known as Autoregressive Conditional Heteroscedastic model.
Autoregressive models are a statistical technique involving a regression
of lagged values where the model suggests that past values can help
forecast future values of the same variable. Within the model, a time
series is the dependent variable and lagged values are the independent
variables.

The ARCH model, was originally developed by Robert Engle in 1982 to
measure the dynamics of inflation uncertainty. Conditional
heteroskedasticity refers to the notion that the next period’s
volatility is conditional on the volatility in the current period as
well as to the time varying nature of volatility. However, given the
volatility dynamics, this model fail to fully capture the persistence of
volatility.

# GARCH

To address the shortcoming, ARCH has been extended to a generalised
framework where we add volatility as a forecasting feature by adding
previous variance. This method is popularly known as Generalized ARCH or
GARCH model.

$$\\sigma\_{n}^{2}=\\omega+\\sum\_{i=1}^{p} \\alpha\_{i} u\_{n-i}^{2}+\\sum\_{i=1}^{q} \\beta\_{i} \\sigma\_{n-i}^{2}$$

where, *p* and *q* are lag length.

GARCH (1,1) is then represented as,

*σ*<sub>*n*</sub><sup>2</sup> = *ω* + *α**u*<sub>*n* − 1</sub><sup>2</sup> + *β**σ*<sub>*n* − 1</sub><sup>2</sup>

where, *α* + *β* \< 1 and *γ* + *α* + *β* = 1 as weight applied to long
term variance cannot be negative and
$\\frac{\\omega}{(1-\\alpha-\\beta)}$ is the long-run variance.

The GARCH model is a way of specifying the dependence of the time
varying nature of volatility. The model incorporates changes in the
fluctuations in volatility and tracks the persistence of volatility as
it fluctuates around its long-term average and are exponentially
weighted. To model GARCH or the conditional volatility, we need to
derive *ω*, *α*, *β* by maximizing the likelihood function.

## ARCH Toolbox

ARCH is one of the popular tools used for financial econometrics,
written in Python - with Cython and/or Numba used to improve
performance. We will now use arch_model to fit our GARCH model using
this package.

<div class="center">

<img src="2023_07_29_7d6ca5e0312d2c7eed33g-8" alt="image" />

</div>

<div class="center">

<img src="2023_07_29_7d6ca5e0312d2c7eed33g-9" alt="image" />

</div>

# References

-   Arch

-   Scipy

-   Tabulate

June 2023, Certificate in Quantitative Finance.
