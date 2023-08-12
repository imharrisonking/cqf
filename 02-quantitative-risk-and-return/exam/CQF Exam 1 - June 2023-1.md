# CQF Exam One 

June 2023 Cohort

## $\underline{\text { Instructions }}$

All questions must be attempted. Requested mathematical and full computational workings must be provided to obtain maximum credit. Books and lecture notes may be referred to. Help from others is not permitted.

Upload the report E1_YOURNAME_REPORT.pdf and one zip file E1_YOURNAME_CODE.zip. All other files, data, code and signed declaration must be inside that zip file.

- You must prepare a report in ONE PDF FILE that provides workings, numerical answers and plots in the order of Questions.
- If your work is a Python notebook - remove unnecessary output and generate a PDF (you can save Python notebook to HTML first, and then print as PDF).
- Handwriten derivation workings are acceptable - you need to scan them and insert into the relevant places (eg Q1, Q3) of the final PDF report file.
- Submission of report as Excel/unformatted Python notebook will receive a deduction in marks for rough quality of work. Absence of requested tables and plots will receive a deduction, even if numbers are generated by the code. It is not acceptable to write 'refer to Excel' or 'refer to Python code'.
- If adopting the code from elsewhere/CQF labs, make sure its understood and properly applied. No permission or notification needed to for coding in Python, Excel (VBA), Matlab, R, and C++, Java.

Clarifying questions only but not asking to explain exam task - please submit a support ticket. Understanding and implementing exam questions is part of the task - tutors should not be asked to guide you through and/or not provide 'on the right track' reassurance.

This distant learning exam is set as a collection of take-home exercises. Please make a good use of lecture material and exercise solutions. Tutor is unable to discuss your numerical answers or provide hints beyond ones given on the exam paper.

Please follow the instructions on file naming. It will facilitate the faster turnaround of exam scripts. Example: E1_JOHN SMITH_REPORT.pdf. Use the same personal name as registered on CQF Portal.

Marking Scheme: Q1 16\% Q2 24\% Q3 8\% Q4 16\% Q5 18\% $\quad$ Q6 18\%. Total is 100\%.

## Optimal Portfolio Allocation

An investment universe of the following risky assets with a dependence structure (correlation) applies to all questions below as relevant:

$\begin{array}{cccc}\text { Asset } & \boldsymbol{\mu} & \boldsymbol{\sigma} & \boldsymbol{w} \\ A & 0.05 & 0.07 & w_{1} \\ B & 0.07 & 0.28 & w_{2} \\ C & 0.15 & 0.25 & w_{3} \\ D & 0.22 & 0.31 & w_{4}\end{array}$

$$
\text { Corr }=\left(\begin{array}{cccc}
1 & 0.4 & 0.3 & 0.3 \\
0.4 & 1 & 0.27 & 0.42 \\
0.3 & 0.27 & 1 & 0.5 \\
0.3 & 0.42 & 0.5 & 1
\end{array}\right)
$$

Question 1. Global Minimum Variance portfolio is obtained subject to the budget constraint:

$$
\underset{\boldsymbol{w}}{\operatorname{argmin}} \frac{1}{2} \boldsymbol{w}^{\prime} \boldsymbol{\Sigma} \boldsymbol{w} \quad \text { s.t. } \boldsymbol{w}^{\prime} 1=1
$$

- Derive the analytical solution for optimal allocations $\boldsymbol{w}^{*}$. Provide full mathematical workings.
- In the derivation, include the formula derivation for the Lagrangian multiplier.
- Compute optimal allocations (Global MV portfolio) for the given investment universe.

Question 2. Consider the optimization for a target return $m$. There is no risk-free asset.

$$
\begin{aligned}
& \underset{\boldsymbol{w}}{\operatorname{argmin}} \frac{1}{2} \boldsymbol{w}^{\prime} \boldsymbol{\Sigma} \boldsymbol{w} \\
& \boldsymbol{w}^{\prime} \mathbf{1}=1 \\
& \boldsymbol{w}^{\prime} \boldsymbol{\mu}=m
\end{aligned}
$$

- Compute $\mathbf{w}^{*}$ and portfolio risk $\sigma_{\Pi}=\sqrt{\boldsymbol{w}^{\prime} \boldsymbol{\Sigma} \boldsymbol{w}}$ for $m=7 \%$ for three levels of correlation.
- Correlation matrix $\times 1, \times 1.3, \times 1.8$, subject to individual correlation upper limit of 0.99 , if the scaling results in correlation value above 1 . Provide all results in a single table.

Hint: negative and non-robust allocations (into \pm 100 s $\%$ ) are possible, particularly for high correlations. Please do not reconfirm your numerical results via support.

All optimal allocations (weights) in these sections have to be computed explicitly via analytical formulae. Computation via numerical optimisation only will result in a deduction.

## Understanding Risk

Question 3. "Evaluating the P\\&L more frequently make it appear more risky than it actually is." Make the following simple computations to demonstrate this statement.

- Write down the formula for Sharpe Ratio and identify main parameter scaled with time.
- Compute Daily, Monthly, and Quarterly Sharpe Ratio, for Annualised SR of 0.53. No other inputs.
- Convert each Sharpe Ratio into Loss Probability (daily, monthly, quarterly, annual), using

$$
\operatorname{Pr}(\mathrm{P} \& \mathrm{~L}<0)=\operatorname{Pr}(x<-\mathrm{SR}) .
$$

where $x$ is a standard Normal random variable.

Question 4. Instead of computing the optimal allocations analytically, let's conduct an experiment. Generate above 700 random allocation sets: $4 \times 1$ vectors. Those will not be optimal and can be negative.

- Standardise each set to satisfy $w^{\prime} \mathbf{1}=1$. In fact, generate 3 allocations and compute the 4 th.
- For each set, compute $\mu_{\Pi}=\boldsymbol{w}^{\prime} \boldsymbol{\mu}$ and $\sigma_{\Pi}=\sqrt{\boldsymbol{w}^{\prime} \boldsymbol{\Sigma} \boldsymbol{w}}$.
- Plot the cloud of points, $\mu_{\Pi}$ vertically on $\sigma_{\Pi}$ horizontally. Explain this plot.

Hint: use the investment universe settings above, but the task is not optimisation, and no other formulae involved. Compute only what is asked in the question. Question 5. NASDAQ100 data provided (2017-2023) for you to implement the backtesting of 99\%/10day Value at Risk and report the following:

(a) The count and percentage of VaR breaches.

(b) The count of consecutive VaR breaches. (1, 1, 1 indicates two consecutive occurrences)

(c) Provide a plot which: identifies the breaches visually (crosses or other marks) and properly labels axis $\mathrm{X}$ with at least years.

(d) In your own words, describe the sequence of breaches caused by COVID pandemic news in 2020-Feb versus 2020-Mar.

$$
\operatorname{VaR}_{10 D, t}=\text { Factor } \times \sigma_{t} \times \sqrt{10}
$$

- Compute the rolling standard deviation $\sigma_{t}$ from 21 daily returns.
- Timescale of that $\sigma_{t}$ remains 'daily' regardless of how many returns are in the sample. To make projection, use the additivity of variance $\sigma_{10 D}=\sqrt{\sigma_{t}^{2} \times 10}$.
- A breach occurs when the forward realised 10-day return is below the $\mathrm{VaR}_{t}$ quantity.

$r_{10 D, t+10}<\operatorname{VaR}_{10 D, t} \quad$ means breach, given both numbers are negative.

VaR is fixed at time $t$ and compared to the return realised from $t$ to $t+10$, computed $\ln \left(S_{t+10} / S_{t}\right)$. Alternatively, you can compare to $\ln \left(S_{t+11} / S_{t+1}\right)$ but state this assumption in your report upfront.

Question 6. Re-implement backtesting using the method above, recompute $\mathrm{VaR}_{10 D, t}$ but, with the input of EWMA $\sigma_{t+1}^{2}$. Use the variance for the entire dataset to initialise the scheme.

$$
\sigma_{t+1 \mid t}^{2}=\lambda \sigma_{t \mid t-1}^{2}+(1-\lambda) r_{t}^{2}
$$

with $\lambda=0.72$ value set to minimise out of sample forecasting error.

Hint: computation of EWMA $\sigma_{t+1}^{2}$ is not sufficient, proceed to compute $\operatorname{VaR}_{10 D, t}$ and count breaches in VaR.

(a-c) Provide the same deliverables (a), (b) and (c) as in the previous Question.

(d) Briefly (3-4 lines) discuss the impact of $\lambda$ on smoothness of EWMA-predicted volatility.

## END OF EXAM
