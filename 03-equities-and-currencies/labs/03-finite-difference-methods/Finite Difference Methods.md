![](https://cdn.mathpix.com/cropped/2023_08_25_8e3a8c54d4fcc061b7c3g-01.jpg?height=281&width=823&top_left_y=380&top_left_x=651)

Kannan Singaravelu, CQF

June 2023

# 1 Finite Difference Methods 

Finite Difference Methods (FDM) are one of the popular numerical methods used in computational finance. FDM are discretizations used for solving differential equations by approximating them with difference equations. It is one of the simplest and the oldest methods to solve differential equations. These techniques were applied to numerical applications as early as 1950s.

FDM are similar in approach to the (binomial) tress. However, instead of discretizing asset prices and the passage of time in a tree structure, it discretizes in a grid - with time and price steps - by calculating the value at every possible grid points.

Explicit, Implicit and Crank-Nicolson are the three popular approaches of FDM. The explicit methods are simple to implement, but it does not always converge and largely depends on the size of the time and asset step. Explicit methods are unstable when compared to other two methods. Finite Difference approach is peferred for low dimensional problem, usually upto 4 dimensions.

## Differentiation Using The Grid

The Binomial method contains the diffusion - the volatility - in the tree structure whereas in FDM, the 'tree' is fixed and we change the parameters to reflect the changing diffusion. We will now define the grid by specifying the time step $\delta t$ and asset step $\delta s$ and discretize $S$ and $t$ as

$$
S=i \delta s
$$

and time to maturity as

$$
t=T-k \delta t
$$

where $0 \leq i \leq I$ and $0 \leq k \leq K$

Here $i$ and $k$ are respective steps in the grid and we can write the value of the option at each grid points as

$$
V_{i}^{k}=(i \delta S, T-k \delta t)
$$

## Approximating Greeks

The greeks terms, the Black-Scholes equation can be written as

$$
\Theta+\frac{1}{2} \sigma^{2} S^{2} \Gamma+r S \Delta-r V=0
$$

Assume that we know the option value at each grid points, we can extract the derivatives of the option using Taylor series expansion.

## Approximating $\Theta$

We know that the first derivative of option as,

$$
\frac{\partial V}{\partial t}=\lim _{h \rightarrow 0} \frac{V(S, t+h)-V(S, t)}{h}
$$

We can then approximate the time derivative from our grid using

$$
\frac{\partial V}{\partial t}(S, t) \approx \frac{V_{i}^{k}-V_{i}^{k+1}}{\delta t}
$$

## Approximating $\Delta$

From the lecture, we know that the central difference has much lower error when compared to forward and backward differences. Accordingly, we can approximate the first derivative of option with respect to the underlying as

$$
\frac{\partial V}{\partial S}(S, t) \approx \frac{V_{i+1}^{k}-V_{i-1}^{k}}{2 \delta S}
$$

## Approximating $\Gamma$

The gamma of the option is the second derivative of option with respective to the underlying and approximating it we have,

$$
\frac{\partial V^{2}}{\partial S^{2}}(S, t) \approx \frac{V_{i+1}^{k}-2 V_{i}^{k}+V_{i-1}^{k}}{\delta S^{2}}
$$

## Import Required Libraries

![](https://cdn.mathpix.com/cropped/2023_08_25_8e3a8c54d4fcc061b7c3g-02.jpg?height=496&width=747&top_left_y=2005&top_left_x=151)

![](https://cdn.mathpix.com/cropped/2023_08_25_8e3a8c54d4fcc061b7c3g-03.jpg?height=245&width=920&top_left_y=255&top_left_x=251)

## Example

Suppose that we know the value of the option on the below grid points, we can then easily evaluate the greeks as follows

![](https://cdn.mathpix.com/cropped/2023_08_25_8e3a8c54d4fcc061b7c3g-03.jpg?height=1252&width=1358&top_left_y=740&top_left_x=275)

From the grid, we can estimate the

$$
\begin{gathered}
\Theta=\frac{12-13}{0.1}=-10 \\
\Delta=\frac{15-10}{2 x 2}=1.25 \\
\Gamma=\frac{15-2 x 12+10}{2 x 2}=0.25
\end{gathered}
$$

Black Scholes Equation is a relationship between the option value and greeks. If we know the option value at the expiration, we can step back to get the values prior to it such that $V_{k}^{i}=V_{k-1}^{i}-\Theta * d t$. This approach is called Explicit Finite Difference Method because the relationship between the option values at time step $k$ is a simple function of the option values at time step $k-1$.

## Option Pricing Techniques

As with other option pricing techniques Explicit Finite Difference methods are used to price options using what is essentially a three step process.

Step 1: Generate the grid by specifying grid points.

Step 2: Specify the final or initial conditions.

Step3: Use boundary conditions to calculate option values and step back down the grid to fill it.

## European Option

To price an option, we generate a finite grid of a specified asset and time steps for a given maturity. Next, we specify the initial and boundary conditions to calculate payoff when $\mathrm{S}$ and $\mathrm{T}$ equals zero. We then step back to fill the grid with newer values derived from the earlier values.

## Specify Parameters

[ ]: \# Specify the parameters for FDM

![](https://cdn.mathpix.com/cropped/2023_08_25_8e3a8c54d4fcc061b7c3g-04.jpg?height=623&width=1576&top_left_y=1285&top_left_x=248)

### Generate Grid

Build the grid with the above input parameters

![](https://cdn.mathpix.com/cropped/2023_08_25_8e3a8c54d4fcc061b7c3g-04.jpg?height=341&width=664&top_left_y=2106&top_left_x=149)

[ ]: \#Verify the steps size

s.shape, t.shape

[ ]: \# Initialize the grid with zeros

grid $=\operatorname{zeros}((\operatorname{len}(s), \operatorname{len}(t)))$

\# Subsume the grid points into a dataframe

\# with asset price as index and time steps as columns

grid $=$ pd.DataFrame (grid, index $=\mathbf{s}, \operatorname{columns}=\operatorname{around}(t, 3))$

grid

### Specify Condition

Specify final condition and payoffs

$$
V_{i}^{0}=\max (i \delta s-E, 0)
$$

[ ]: \# Set Final or Initial condition at Expiration

if $\mathrm{Flag}==1$ :

$\operatorname{grid} . \operatorname{iloc}[:, 0]=\operatorname{maximum}(\mathrm{s}-\mathrm{E}, 0)$ else:

grid.iloc $[:, 0]=\operatorname{maximum}(E-\mathrm{s}, 0)$

[ ]: \#Verify the grid grid

### Fill the Grid

Specify boundary condition at $\mathrm{S}=0$

$$
V_{0}^{k}=(1-r \delta t) V_{0}^{k-1}
$$

Specify boundary condition at $\mathrm{S}=\infty$

$$
V_{i}^{k}=2 V_{i-1}^{k}-V_{i-2}^{k}
$$

[ ]: \# $k$ is counter

for $k$ in range $(1$, len $(t))$ :

for $i$ in range $(1, \operatorname{len}(s)-1)$ :

delta $=(\operatorname{grid} . i l o c[\mathrm{i}+1, \mathrm{k}-1]-\operatorname{grid} . i l o c[\mathrm{i}-1, \mathrm{k}-1]) /(2 * \mathrm{ds})$

$\hookrightarrow(\mathrm{ds} * * 2)$

gamma $=(\operatorname{grid} \cdot i l o c[i+1, k-1]-2 * \operatorname{grid} \cdot \mathrm{iloc}[\mathrm{i}, \mathrm{k}-1]+\operatorname{grid} \cdot \mathrm{iloc}[\mathrm{i}-1, \mathrm{k}-1]) / \sqcup$

theta $=(-0.5 * \operatorname{vol} * * 2 * \mathrm{~s}[\mathrm{i}] * * 2 *$ gamma $)-(\mathrm{r} * \mathrm{~s}[\mathrm{i}] *$ delta $)+(\mathrm{r} *$ grid.

$\hookrightarrow$ iloc $[i, k-1])$

$\operatorname{grid} . i l o c[i, k]=\operatorname{grid} . i l o c[i, k-1]-($ theta $* d t)$

![](https://cdn.mathpix.com/cropped/2023_08_25_8e3a8c54d4fcc061b7c3g-06.jpg?height=489&width=1591&top_left_y=256&top_left_x=252)

[ ]: \# Output the option values

\# grid.iloc $[0: 15,:]$

grid

## Visualize the payoff

[ ]: \# Plot Call Option Payoff

grid.iloc $[0: 15,:]$.iplot(kind = 'surface', title='Call Option values by Explicit॰ $\hookrightarrow$ FDM $^{\prime}$ )

## Bilinear Interpolation

We have generated the grid and filled it with the possible option values. However, if we have to estimate option value or its derivatives on the mesh points, how can we estimate the value at points in between? The simplest way is to do a two-dimensional interpolation method called Bilinear Interpolation.

![](https://cdn.mathpix.com/cropped/2023_08_25_8e3a8c54d4fcc061b7c3g-07.jpg?height=973&width=1227&top_left_y=272&top_left_x=384)

The option value can be estimated using the values from the nearest neighbouring values. Assume $V_{1}, V_{2}, V_{3} a n d V_{4}$ are the option values from the nearest neighbour and $A_{1}, A_{2}, A_{3}$ and $A_{4}$ are the areas of the rectanges made by the four corners and the interior points, we can approximate the option value at the interior points as

$$
\frac{\sum_{i=1}^{4} A_{i} V_{i}}{\sum_{i=1}^{4} A_{i}}
$$

[ ]: def bilinear_interpolation(asset_price, ttm, df):

\# Find relevant rows and columns

$\operatorname{col} 1=\mathrm{df}$. columns $[\mathrm{df}$. columns $<\mathrm{ttm}][-1]$

col2 $=$ df.columns $[\mathrm{df}$. columns $>=\mathrm{ttm}][0]$

row1 $=\mathrm{df}$.index $[\mathrm{df}$.index $<$ asset_price, $][-1]$

row2 $=\mathrm{df}$.index $[\mathrm{df}$.index $>=$ asset_price, $][0]$

\# Define points and areas

$\mathrm{V}=[\mathrm{df} . \operatorname{loc}[$ row1, col1], df.loc [row1, col2],

df.loc [row2, col2], df.loc[row2, col1]]

$A=[($ row2 - asset_price $) *($ col2 - ttm $)$,

(row2 - asset_price) * (ttm - col1),

(asset_price - row1) * (ttm - col1),

(asset_price - row1) * (col2 - ttm)] [ ]: \# Option value, approximated

bilinear_interpolation(105, 0.25 ,grid)

## User Defined Function

Let's subsume above grid calculation into a function for ease of use.

[ ]: def efdm_grid(Strike, Volatility, Rate, TTM, NAS, Flag=1):

\# Specify Flag as 1 for calls and -1 for puts

$\mathrm{d} \mathbf{s}=2 *$ Strike/NAS \# asset step size

$\mathrm{dt}=0.9 /$ Volatility $* 2 / \mathrm{NAS} * * 2 \quad$ \# for stability

NTS $=\operatorname{int}($ TTM $/ \mathrm{dt})+1 \quad$ \# time step size, alternatively use

$\hookrightarrow$ fixed size 10 on stability issue

$\mathrm{dt}=\mathrm{TTM} / \mathrm{NTS} \quad$ \# time step

$\mathbf{s}=\operatorname{arange}(0,(\mathrm{NAS}+1) * \mathrm{ds}, \mathrm{ds})$

$t=\mathrm{TTM}-$ arange $(\mathrm{NTS} * \mathrm{dt},-\mathrm{dt},-\mathrm{dt})$

\# Initialize the grid with zeros

grid $=\operatorname{zeros}((\operatorname{len}(s), \operatorname{len}(t)))$

grid $=$ pd.DataFrame $($ grid, index $=\mathbf{s}, \operatorname{columns}=\operatorname{around}(t, 2))$

\# Set boundary condition at Expiration

grid.iloc $[:, 0]=\operatorname{abs}(\operatorname{maximum}(F l a g *(s-\operatorname{Strike}), 0))$

for $k$ in range $(1$, len( $t))$ :

for $i$ in range $(1, \operatorname{len}(s)-1)$ :

delta $=(\operatorname{grid} . i l o c[\mathrm{i}+1, \mathrm{k}-1]-\operatorname{grid} . \mathrm{iloc}[\mathrm{i}-1, \mathrm{k}-1]) /(2 * \mathrm{ds})$

$\hookrightarrow(\mathrm{ds} * * 2)$

gamma $=(\operatorname{grid} \cdot \operatorname{iloc}[\mathrm{i}+1, \mathrm{k}-1]-2 * \operatorname{grid} \cdot \mathrm{iloc}[\mathrm{i}, \mathrm{k}-1]+\operatorname{grid} . \mathrm{iloc}[\mathrm{i}-1, \mathrm{k}-1]) / \sqcup$ $\hookrightarrow \operatorname{iloc}[\mathrm{i}, \mathrm{k}-1])$

theta $=(-0.5 * \operatorname{vol} * * 2 * \mathrm{~s}[\mathrm{i}] * * 2 *$ gamma $)-(\mathrm{r} * \mathrm{~s}[\mathrm{i}] * \operatorname{delta})+(\mathrm{r} * \mathrm{grid}$.

$\operatorname{grid} \cdot \mathrm{iloc}[\mathrm{i}, \mathrm{k}]=\operatorname{grid} \cdot \mathrm{iloc}[\mathrm{i}, \mathrm{k}-1]-\mathrm{dt} *$ theta

\# Set boundary condition at $S=0$

$\operatorname{grid} . i l o c[0, \mathrm{k}]=\operatorname{grid} . i l o c[0, \mathrm{k}-1] *(1-\mathrm{r} * \mathrm{dt})$

\# Set boundary condition at $S=$ infinity

$\operatorname{grid} \cdot i \operatorname{loc}[\operatorname{len}(\mathrm{s})-1, \mathrm{k}]=\operatorname{abs}(2 *(\operatorname{grid} \cdot i \operatorname{loc}[\operatorname{len}(\mathrm{s})-2, \mathrm{k}])-\operatorname{grid}$.

$\hookrightarrow \operatorname{iloc}[\operatorname{len}(\mathrm{s})-3, \mathrm{k}])$ \# round grid values to 4 decimals

return around (grid,2)

[ ]: \# Call the function for put option

fdm_puts $=$ efdm_grid $(100,0.2,0.05,1,20, \mathrm{Flag}=-1)$

fdm_puts

[ ]: \# Visualize the plot for put option

fdm_puts.iplot (kind='surface', title='Put Option values by Explicit FDM')

## Convergence Analysis

Let's now compare option pricing for various asset steps (NAS) with black scholes price.

[ ]: \# Iterate over asset steps (NAS)

nas_list $=[10,20,30,40,50,60]$

fdmoption $=[]$

for $i$ in nas_list:

fdmoption.append (efdm_grid $(100,0.2,0.05,1, i) .10 c[100,1])$

fdmoption

[ ]: \# Call black scholes class

from src.blackscholes import BS

\# Instantiate black scholes object

option $=$ BS $(100,100,0.05,1,0.20)$

bsoption = round (option. callPrice, 2 )

bsoption = bsoption.repeat(len(nas_list))

\# Range of option price

bsoption

[ ]: \# Subsume into dataframe

$\mathrm{df}=$ pd.DataFrame(list(zip(bsoption,fdmoption)), columns=['BS', 'FDM'], $\hookrightarrow$ index=nas_list)

$\operatorname{df}\left[{ }^{\prime} \mathrm{dev}{ }^{\prime}\right]=\operatorname{df}\left[\mathrm{IFDM}^{\prime}\right]-\mathrm{df}\left[\right.$ 'BS' $\left.^{\prime}\right]$

$\mathrm{df}\left[{ }^{\prime} \% \mathrm{dev}{ }^{\prime}\right]=\operatorname{round}\left(\mathrm{df}\left[\mathrm{Idev}^{\prime}\right] / \mathrm{df}\left[\mathrm{BSS}^{\prime}\right] * 100 ., 2\right)$

\# Output

print("BS - FDM Convergence over NAS")

df

## $9 \quad$ References

- Python Resources
- Paul Wilmott (2007), Paul Wilmott introduces Quantitative Finance

June 2023, Certificate in Quantitative Finance.

