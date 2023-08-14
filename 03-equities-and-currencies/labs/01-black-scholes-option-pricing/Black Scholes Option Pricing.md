![](https://cdn.mathpix.com/cropped/2023_08_14_6246610c3a4432751e84g-1.jpg?height=174&width=480&top_left_y=382&top_left_x=687)

\title{
Black Scholes Option Pricing
}

Kannan Singaravelu, CQF

June 2023

\section*{Black Scholes Model}

The Black Scholes model was published in 1973 for non-dividend paying stocks and since then the model has created a revolution in quantitative finance and opened up derivatives pricing paradigm. Black Scholes model is based on number of assumptions about how financial markets operate and those are:

- Arbitrage Free Markets

- Frictionless and Continuous Markets

- Risk Free Rates

- Log-normally Distributed Price Movements

- Constant Volatility

These assumptions maynot hold true in reality, but are not particularly limiting. The generalized Black Scholes framework have been extended to price derivaties of other asset classes such as Black 76 (Commodity Futures) and Garman-Kohlhagen (FX Futures) that are currently used in derivative pricing and risk management.

\section*{Black Scholes Formula}

The Black-Scholes equation describes the price of the option over time as

$$
\frac{\partial V}{\partial t}+\frac{1}{2} \sigma^{2} S^{2} \frac{\partial^{2} V}{\partial S^{2}}+r S \frac{\partial V}{\partial S}-r V=0
$$

Solving the above equation, we know that the value of a call option for a non-dividend paying stock is:

$$
C=S N\left(d_{1}\right)-K e^{-r t} N\left(d_{2}\right)
$$

and, the corresponding put option price is:

$$
P=K e^{-r t} N\left(-d_{2}\right)-S N\left(-d_{1}\right)
$$

where, 

$$
\begin{gathered}
d_{1}=\frac{1}{\sigma \sqrt{t}}\left[\ln \left(\frac{S}{K}\right)+\left(r+\frac{\sigma^{2}}{2}\right) t\right] \\
d_{2}=d_{1}-\sigma \sqrt{t} \\
N(x)=\frac{1}{\sqrt{2 \pi}} \int_{-\infty}^{x} \mathrm{e}^{-\frac{1}{2} x^{2}} d x
\end{gathered}
$$

$S$ is the spot price of the underlying asset $K$ is the strike price $r$ is the annualized continuous compounded risk free rate $\sigma$ is the volatility of returns of the underlying asset $t$ is time to maturity (expressed in years) $N(x)$ is the standard normal cumulative distribution

\section*{Greeks}

\begin{tabular}{|l|l|l|l|}
\hline Description & & Greeks for Call Option & Greeks for Put Option \\
\hline Delta & $\frac{\partial V}{\partial S}$ Sensitivity of Value to changes in price & $N\left(d_{1}\right)$ & $-N\left(-d_{1}\right)$ \\
\hline Gamma & $\frac{\partial^{2} V}{\partial S^{2}}$ Sensitivity of Delta to changes in price & $\frac{N^{\prime}\left(d_{1}\right)}{S \sigma \sqrt{t}}$ & \\
\hline Vega & $\frac{\partial V}{\partial \sigma}$ Sensitivity of Value to changes in volatility & $S N^{\prime}\left(d_{1}\right) \sqrt{t}$ & \\
\hline Theta & $\frac{\partial V}{\partial t}$ Sensitivity of Value to changes in time & $-\frac{S N^{\prime}\left(d_{1}\right) \sigma}{2 \sqrt{t}}-r K e^{-r t} N\left(d_{2}\right)$ & $-\frac{S N^{\prime}\left(d_{1}\right) \sigma}{2 \sqrt{t}}+r K e^{-r t} N\left(-d_{2}\right)$ \\
\hline Rho & $\frac{\partial V}{\partial r}$ Sensitivity of Value to changes in risk-free & $K t e^{-r t} N\left(d_{2}\right)$ & $-K t e^{-r t} N\left(-d_{2}\right)$ \\
\hline
\end{tabular}

\section*{Import Required Libraries}

![](https://cdn.mathpix.com/cropped/2023_08_14_6246610c3a4432751e84g-2.jpg?height=588&width=867&top_left_y=1414&top_left_x=152)

\subsection*{Options Object}

Python is an object oriented programming language. Almost everything in Python is an object, with its properties and methods. There are two common programming paradigm in Python.

1. Procedural Programming

2. Object-oriented Programming (OOP)

The key difference between them is that in OOP, objects are at the center, not only representing the data, as in the procedural programming, but in the overall structure of the program as well. 

\section*{Class}

We use Classes to create user-defined data structures. Classes define functions called methods, which identify the characteristics and actions that an object created from the class can perform with its data. A Class is like an object constructor, or a "blueprint" for creating objects. To create a class, use the keyword class

_ init _ _

The properties of the class objects are defined in a method called __ init__

1. ___init__( ) sets the initial state of the object by assigning the values of the object's properties.

2. __ init__() initializes each new instance of the class

3. Attributes created in __ init__ () are called instance attributes, while class attributes are attributes that have the same value for all class instances.

self

The self is a parameter used to represent the instance of the class. With self, you can access the attributes and methods of the class in python. It binds the attributes with the given arguments. When a new class instance is created, the instance is automatically passed to the self parameter in __ init__() so that new attributes can be defined on the object.

1. __ init__() can any number of parameters, but the first parameter will always be a variable called self.

We will now construct a Black Scholes Options class

\section*{[ ] : class BS:}

![](https://cdn.mathpix.com/cropped/2023_08_14_6246610c3a4432751e84g-3.jpg?height=971&width=1618&top_left_y=1496&top_left_x=251)



![](https://cdn.mathpix.com/cropped/2023_08_14_6246610c3a4432751e84g-4.jpg?height=2182&width=1591&top_left_y=253&top_left_x=272)



![](https://cdn.mathpix.com/cropped/2023_08_14_6246610c3a4432751e84g-5.jpg?height=2234&width=1506&top_left_y=251&top_left_x=271)



![](https://cdn.mathpix.com/cropped/2023_08_14_6246610c3a4432751e84g-6.jpg?height=733&width=1550&top_left_y=244&top_left_x=239)

[ ]: \# Initialize option

option $=\operatorname{BS}(100,100,0.05,1,0.2)$

header $=$ ['Option Price', 'Delta', 'Gamma', 'Theta', 'Vega', 'Rho']

table $=$ [ [option.callPrice, option.callDelta, option.gamma, option.callTheta, $\lrcorner$ $\hookrightarrow$ option.vega, option.callRho]]

print (tabulate (table, header))

[ ]: \# Initialize option

option $=\operatorname{BS}(100,100,0.05,1,0.2)$

header $=$ ['Option Price', 'Delta', 'Gamma', 'Theta', 'Vega', 'Rho']

table $=$ [ [option.callPrice, option.callDelta, option.gamma, option.callTheta, $\lrcorner$

$\hookrightarrow$ option.vega, option.callRho] ]

print (tabulate (table, header))

\section*{SPY Option}

Let's now retrieve SPY option price from Yahoo Finance using yfinance library and manipulate the dataframe using the above Black Scholes option pricing model that we created.

https://finance.yahoo.com/quote/SPY/options?date=1692921600\&p=SPY

[ ]: \# Get SPY option chain

spy $=$ yf.Ticker ('SPY')

options = spy.option_chain('2023-08-25')

[ ]: from datetime import datetime

dte $=($ datetime $(2023,8,25)-\operatorname{datetime} \cdot \operatorname{today}()) \cdot \operatorname{days} / 365$ [ ]: \# August 2023450 SPY call option price

spot $=445 ;$ strike $=450 ;$ rate $=0.0 ;$ dte $=$ dte; vol $=0.1248$

spy_opt = BS (spot, strike, rate, dte, vol)

print(f'Option Price of SPY230825C00450000 with BS Model is sspy_opt.callPrice:0. $\left.\hookrightarrow 4 f\}^{\prime}\right)$

[ ]: \# Verify the options output

options.calls.head(2)

[ ]: \# Filter calls for strike at or above 450

$\mathrm{d} f=$ options.calls[(options.calls['strike'] $\rangle=450$ ) \& (options.

$\hookrightarrow$ calls ['strike']<=500)]

df.reset_index (drop=True, inplace=True)

\# Dataframe manipulation with selected fields

$\mathrm{df}=$ pd.DataFrame(\{'Strike': df['strike'],

'Price': df ['lastPrice'],

'ImpVol' : df ['impliedVolatility']\})

\# Derive greeks and assign to dataframe as columns

$\mathrm{df}\left[\right.$ 'Delta' $\left.^{\prime}\right]=\mathrm{df}\left[\right.$ 'Gamma' $\left.^{\prime}\right]=\mathrm{df}\left[\right.$ 'Vega' $\left.^{\prime}\right]=\mathrm{df}\left[\right.$ 'Theta' $\left.^{\prime}\right]=0$.

for $i$ in range(len(df)):

$\mathrm{df}[$ 'Delta'].iloc[i] = BS (spot, df ['Strike'].iloc[i],rate, dte, df ['ImpVol'] .

$\hookrightarrow$ iloc $[i])$. callDelta

$\mathrm{df}[$ 'Gamma'].iloc[i] = BS (spot, df ['Strike'].iloc[i],rate, dte, df ['ImpVol '] .

$\hookrightarrow$ iloc $[i])$. gamma

$\mathrm{df}[$ 'Vega'].iloc[i] = BS (spot, df ['Strike'].iloc[i], rate, dte, df ['ImpVol'] .

$\hookrightarrow$ iloc $[i])$. vega

$\mathrm{df}[$ 'Theta'].iloc[i] = BS (spot, $\mathrm{df}[$ 'Strike'].iloc[i],rate, dte, df ['ImpVol '] . $\hookrightarrow$ iloc $[i])$. callTheta

\# Check output

$\mathrm{df} \cdot \operatorname{head}(2)$

\section*{$2.1 \quad$ Visualize Data}

[ ]: \# Plot graph iteratively

fig, ax $=$ plt. $\operatorname{subplots}(2,2$, figsize $=(20,10))$

ax $[0,0] \cdot \operatorname{plot}(d f[' S t r i k e ']$, df ['Delta'], color='r', label='AUG 23')

ax $[0,1] \cdot \operatorname{plot}\left(d f[' S t r i k e '], \operatorname{df}\left[' G a m m a^{\prime}\right]\right.$, color='b', label='AUG 23')

ax [1,0].plot (df ['Strike'], df['Vega'], color='k', label='AUG 23')

ax [1,1].plot (df ['Strike'], df['Theta'], color='g', label='AUG 23') 

![](https://cdn.mathpix.com/cropped/2023_08_14_6246610c3a4432751e84g-8.jpg?height=528&width=1382&top_left_y=258&top_left_x=251)



\section*{References}

- Matplotlib

- Python Class Reference

- Python Resources

- YFinance

June 2023, Certificate in Quantitative Finance.