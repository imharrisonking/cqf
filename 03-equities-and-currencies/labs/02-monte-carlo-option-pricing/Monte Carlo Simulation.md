# (3. <br> Monte Carlo Option Pricing <br> Kannan Singaravelu, CQF <br> June 2023 

## Monte Carlo Approach

Monte Carlo methods are any process that consumes random numbers. These are part of computational algorithms which are based on random sampling to obtain numerical results. Monte Carlo methods are proved to be a very valuable and flexible computational tool in finance and is one of the most widely used methods for optimization and numerical integration problems.

These methods are widely used in high dimensional problems; pricing exotics and complex derivatives where closed form solutions are not directly available. Monte Carlo methods are not just adapted in pricing complex derivatives, It is also extensively used in estimating the portfolio risk such as Value-at-Risk and Expected Shortfall and used in the calculation of worst-case scenarios in stress testing. The downside to that is, it is very computational intensive and demanding.

### Monte Carlo Simulation

A method of estimating the value of an unknown quantity using the principles of inferential statistics.

We take the population and then we sample it by drawing a proper subset. And then we make an inference about the population based upon some set of statistics we do on the sample.

And, the key fact that makes them work, that if we choose the sample at random, the sample will tend to exhibit the same properties as the population from which it is drawn.

### Option Pricing Techniques

As with other option pricing techniques Monte Carlo methods are used to price options using what is essentially a three step process.

Step 1: Simulate potential price paths of the underlying asset.

Step 2: Calculate the option payoff for each of these price paths.

Step 3: Average the payoff and discount back to today to determine the option price.

## Simulating Asset Prices

Next, we will simulate the asset price at maturity $S_{T}$. Following Black-Scholes-Merton where the underlying follows under risk neutrality, a geometric Brownian motion with a stochastic differential equation $(\mathrm{SDE})$ is given as

$$
d S_{t}=r S_{t} d t+\sigma S_{t} d W_{t}
$$

where $S_{t}$ is the price of the underlying at time $t, \sigma$ is constant volatility, $r$ is the constant risk-free interest rate and $W$ is the brownian motion.

Applying Euler discretization of SDE, we get

$$
S_{t+\delta t}=S_{t} *\left(1+r \delta t+\sigma \sqrt{\delta} t w_{t}\right)
$$

It is often more convenient to express in time stepping form

$$
S_{t+\delta t}=S_{t} \exp ^{\left(\left(r-\frac{1}{2} \sigma^{2}\right) \delta t+\sigma \sqrt{\delta} t w_{t}\right)}
$$

The variable $\mathrm{w}$ is a standard normally distributed random variable, $0<\delta t<\mathrm{T}$, time interval. It also holds $0<\mathrm{t} \mathrm{T}$ with $\mathrm{T}$ the final time horizon.

### Generate Price Paths

Simulating price paths plays an important role in the valuation of derivatives and it is always prudent to create a separate path function.

## Import Required Libraries

[ ] : \# Importing libraries

import pandas as pd

from numpy import *

import matplotlib.pyplot as plt

\# Set max row to 300

pd.set_option ('display.max_rows', 300)

### User Defined Function

![](https://cdn.mathpix.com/cropped/2023_08_20_7eb610d05150362331b5g-2.jpg?height=433&width=1316&top_left_y=2056&top_left_x=149)

![](https://cdn.mathpix.com/cropped/2023_08_20_7eb610d05150362331b5g-3.jpg?height=765&width=965&top_left_y=252&top_left_x=322)

### Histogram of Psuedo Random Numbers

[ ]: \# Assign simulated price path to dataframe for analysis and plotting price_path $=$ pd.DataFrame $($ simulate_path $(100,0.05,0.2,1,252,100000))$

\# Verify the generated price paths price_path.head()

### Histogram of Simulated Paths

[ ]: \# plt.hist(pd.DataFrame(random.standard_normal(100000)), bins=100);

$\# w=$ random.standard_normal (100000)

\#w.std (), w.mean()

[ ]: \# Plot the histogram of the simulated price path at maturity price_path.iloc [-1].hist(bins=100);

### Visualization of Simulated Paths

[ ]: \#Plot initial 100 simulated path using matplotlib plt.plot (price_path.iloc $[:,: 1000]$ )

plt.xlabel ('time steps')

plt.xlim $(0,252)$

plt.ylabel ('index levels')

plt.title('Monte Carlo Simulated Asset Prices');

## Risk-Neutral Valuation

A call option gives the holder of the option the right to buy the asset at a pre-defined price. A call buyer makes money if the price of the asset at maturity, denoted by $S_{T}$, is above the strike price $K$, otherwise it's worth nothing.

$$
C_{T}=\max \left(0, S_{T}-K\right)
$$

The price of an option using a Monte Carlo simulation is the expected value of its future payoff. So at any date before maturity, denoted by $t$, the option's value is the present value of the expectation of its payoff at maturity, $T$.

$$
C=P V\left(E\left[\max \left(0, S_{T}-K\right)\right]\right)
$$

Under the risk-neutral framework, we assume the asset is going to earn, on average, the risk-free interest rate. Hence, the option value at time $t$ would simply be the discounted value of the expected payoff.

$$
C=e^{r(T t)}\left(E\left[\max \left(0, S_{T}-K\right)\right]\right)
$$

### European Option

To price an option, we generate many possible price paths that the asset might take at maturity and then calculate option payoffs for each of those generated prices, average them to get the expected payoff and then discount it at risk free to arrive at the final value.

Given that Monte Carlo algorithms are computationaly heavy, it is necessary to implement efficiently. We'll use vectorization with NumPy for effective algorithm as NumPy syntax are more compact and are faster.

[ ]: \# Call the simulation function

$\mathrm{S}=$ simulate_path $(100,0.05,0.2,1,252,100000)$

\# Define parameters

$\mathrm{K}=100 ; \mathrm{r}=0.05 ; \mathrm{T}=1$

[ ]: \# Calculate the discounted value of the expeced payoff

$\mathrm{CO}=\exp (-\mathrm{r} * \mathrm{~T}) * \operatorname{mean}(\operatorname{maximum}(0, \mathrm{~S}[-1]-\mathrm{K}))$

$\mathrm{PO}=\exp (-\mathrm{r} * \mathrm{~T}) * \operatorname{mean}(\operatorname{maximum}(0, \mathrm{~K}-\mathrm{S}[-1]))$

\# Print the values

print (f"European Call Option Value is $\{\mathrm{CO}: 0.4 \mathrm{f}\} "$ )

print(f"European Put Option Value is $\{\mathrm{PO}: 0.4 \mathrm{f}\} "$ )

[ ]: \# range of spot prices

$\mathrm{sT}=$ linspace $(50,150,100)$

\# visualize call and put price for range of spot prices

![](https://cdn.mathpix.com/cropped/2023_08_20_7eb610d05150362331b5g-5.jpg?height=529&width=1591&top_left_y=256&top_left_x=251)

### Asian Call Option

An Asian option is an option where the payoff depends on the average price of the underlying asset over a certain period of time. Averaging can be either be Arithmetic or Geometric. There are two types of Asian options: fixed strike, where averaging price is used in place of underlying price; and fixed price, where averaging price is used in place of strike.

We'll now price a fixed strike arthmetic average option using Monte Carlo simulation.

The payoff of the options is given by

$$
\begin{gathered}
C_{T}=\max \left(0, \frac{1}{T} \sum_{i=1}^{T} S_{i}-K\right) \\
C_{T}=\max \left(0, S_{A v g}-K\right)
\end{gathered}
$$

where $S_{A v g}$ is the average price of the underlying asset over the life of the option. To price an option using a Monte Carlo simulation we use a risk-neutral valuation, where the fair value for a derivative is the expected value of its future payoff. So at any date before maturity, denoted by $t$, the option's value is the present value of the expectation of its payoff at maturity, $T$.

$$
C=P V\left(E\left[\max \left(0, S_{A v g}-K\right)\right]\right)
$$

Under the risk-neutral framework, we assume the asset is going to earn, on average, the risk-free interest rate. Hence, the option value at time $t$ would simply be the discounted value of the expected payoff.

$$
C=e^{r(T t)}\left(E\left[\max \left(0, S_{A v g}-K\right)\right]\right)
$$

![](https://cdn.mathpix.com/cropped/2023_08_20_7eb610d05150362331b5g-5.jpg?height=255&width=870&top_left_y=2195&top_left_x=149)

### Up-and-out Barrier Call Option

Barrier Options are path dependent exotic options whose payoff depends on whether the price of the underlying asset crosses a pre specified level (called the 'barrier') before the expiration. The four main types of barrier options are:

- Up-and-out
- Down-and-out
- Up-and-in
- Down-and-in

Refer Paul Wilmott on Quantitative Finance Chapter 23 - Barrier Options and Chapter 77 Finite Difference Methods for One-factor Models for further details on barriers.

Next, we will price a Up-Out-Call barrier with and without rebate using Monte Carlo simulation. Barrier options can be priced using analytical solutions if we assume continuous monitoring of the barrier. However, in reality many barrier contracts specify discrete monitoring.

In a paper titled A Continuity Correction for Discrete Barrier Option, Mark Broadie, Paul Glasserman and Steven Kou have shown us that the discrete barrier options can be priced using continuous barrier formulas by applying a simple continuity correction to the barrier. The correction shifts the barrier away from the underlying by a factor of

$$
\exp ^{(\beta \sigma \sqrt{\Delta t})}
$$

where $\beta \approx 0.5826$ and $\sigma$ is the underlying volatility, and $\Delta t$ is the time between monitoring instants. We will apply this continuity correction in our pricing method as well.

![](https://cdn.mathpix.com/cropped/2023_08_20_7eb610d05150362331b5g-6.jpg?height=816&width=1625&top_left_y=1629&top_left_x=152)

$\mathrm{CO}=\exp (-\mathrm{r} * \mathrm{~T}) *$ value $/ \mathrm{n}$

\# Print the values

print (f"Up-and-Out Barrier Call Option Value is $\{\mathrm{CO}: 0.4 \mathrm{f}\} "$ )

[ ]: figure, axes = plt.subplots $(1,3$, figsize=(20,6), constrained_layout=True)

title $=$ ['Visualising the Barrier Condition', 'Spot Touched Barrier', 'Spot $\sqcup$ $\hookrightarrow$ Below Barrier']

axes [0] plot $(\mathrm{S}[:,: 200])$

for $i$ in range(200): axes[1] plot $(S[:, i])$ if $S[:, i] \cdot \max ()>B_{-}$shift else axes [2] $\operatorname{plot}(S[:, i])$

for $i$ in range(3):

axes [i] . set_title(title [i])

axes [i].hlines(B_shift, 0, 252, colors='k', linestyles='dashed' )

figure.supxlabel ('time steps')

figure.supylabel ('index levels')

plt. show ()

## References

- Paul Glasserman (2004), Monte Carlo Methods in Financial Engineering
- Paul Wilmott (2007), Paul Wilmott introduces Quantitative Finance
- Python Resources
- Understanding Options

June 2023, Certificate in Quantitative Finance.

