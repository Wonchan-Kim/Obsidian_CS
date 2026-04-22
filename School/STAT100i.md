---
cssclasses:
  - stat-cheatsheet
---
---
cssclasses:
  - stat-cheatsheet
---

# STAT 100 FINAL CHEAT SHEET
---

## 1. Basic objects
- Population: whole group of interest
- Sample: observed subset
- Parameter: number describing population
- Statistic: number describing sample
- Individual/case/unit: one object in data
- Variable: characteristic measured on each individual
- Categorical: labels/groups
- Numerical: arithmetic meaning
- Warning: ZIP code / student ID look numeric but are labels, not numerical variables

---

## 2. Bias / variability / sampling
- Bias: repeated samples miss the true parameter in the same direction; systematic over/underestimate
- Variability: statistic changes from sample to sample due to random fluctuation
- Sampling error: error from using sample instead of whole population
- Bigger sample reduces variability, not bias
- Probability sample: random mechanism known
- SRS: every sample of size $n$ has equal chance
- Convenience sample: easy to reach, weak representativeness
- Voluntary response: people choose themselves, strong opinions overrepresented
- Undercoverage: some population members missing from frame
- Overcoverage: frame includes people not in population
- Nonresponse: selected people do not respond
- Response bias: inaccurate answers due to wording / sensitivity / mode
- Random sampling $\to$ supports population inference
- Random assignment $\to$ supports causal claims

---

## 3. Population / target sample / frame / census
- Population = actual set of people/objects of interest
- Population size = number in that set
- Target sample = people you tried to survey
- Respondents = people who actually replied
- Frame = list/method used to identify who can be sampled
- Census = attempt to survey entire population; can still be incomplete if not everyone responds
- Same frame can have both undercoverage and overcoverage; they do not cancel out

---
## 4. Study types
- Sample survey: collect data from sample of population
- Census: collect data from whole population
- Observational study: observe, no imposed treatment; association only
- Experiment: researcher manipulates at least one explanatory variable
- A bad design can still be an experiment if treatment was controlled
- Explanatory = predictor / independent / $x$
- Response = outcome / dependent / $y$
- Lurking variable: omitted variable affecting relationship
- Confounding: effects tangled, cannot separate clearly
- Common response: $x$ and $y$ both respond to same lurking variable
- To explain confounding well, connect confounder to both explanatory and response variables

---

## 5. Sampling methods
- SRS: choose individuals directly from whole population
- Stratified: divide into strata, sample from each stratum
- Cluster: divide into clusters, choose some whole clusters, survey everyone/many there
- Multi-stage: sample in stages, e.g. schools $\to$ classes $\to$ students
- Memory: stratified = from every type; cluster = pick some groups

---

## 6. Graphs
- Pie chart: parts of whole
- Bar graph: compare categories
- Line graph: change over time
- Histogram: distribution of numerical variable
- Boxplot: five-number summary
- Always check: shape, center, spread, outliers, scale
- Histogram bin detail: if bins include lower limit but not upper, boundary values go in right-hand bin
- Misleading graph clue: different series may use different $y$-axis scales

---

## 7. Distribution shape / center / spread
- Shape: symmetric, skewed right, skewed left, unimodal, bimodal
- Center: mean, median
- Spread: range, IQR, SD
- Outlier: unusually extreme observation
- Mean uses all values, sensitive to skew/outliers
- Median is resistant, better for skew/outliers
- Use mean + SD for roughly symmetric data without outliers
- Use median + IQR / five-number summary for skewed data or outliers
- Left-skewed density: $\text{mean}<\text{median}<\text{mode}$
- Right-skewed density: $\text{mode}<\text{median}<\text{mean}$

---

## 8. Median / quartiles / five-number summary
- Sort data first
- Median:
  - odd $n$: middle value, position $(n+1)/2$
  - even $n$: average of middle two, positions $n/2$ and $n/2+1$
- Quartiles:
  - odd $n$: remove overall median, then find medians of lower/upper halves
  - even $n$: split into two equal halves, find medians of halves
- Five-number summary: min, $Q1$, median, $Q3$, max
- $IQR=Q3-Q1$
- SD facts: deviations from mean sum to 0; SD measures typical distance from mean; larger SD = more spread
- If remove all large values above a cutoff, both mean and SD usually decrease

---

## 9. Normal distribution / z / percentile
- Normal curve: symmetric, bell-shaped, fully determined by mean and SD
- Mean controls center, SD controls spread
- 68–95–99.7 rule: about 68%, 95%, 99.7% within 1, 2, 3 SDs
- Standard score: $z=(x-\mu)/\sigma$ or observed minus mean over SD/SE
- $z>0$: above mean; $z<0$: below mean
- Percentile = percent below a value
- Median = 50th percentile; $Q1=25$th; $Q3=75$th
- In Normal distributions, z-scores map directly to percentiles
- Approx: $z=1$ is 84th percentile, $z=-1$ is 16th percentile
- Quartiles of Normal: $Q1\approx z=-0.7$, median $z=0$, $Q3\approx z=0.7$; convert back with $x=\mu+z\sigma$

---

## 10. Probability / odds / EV
- Random: individual outcomes uncertain, long-run pattern regular
- Probability: long-run proportion
- Personal probability: subjective judgment
- Event: set of outcomes
- Probability model: mathematical description of chance behavior
- Rules: $P(A^c)=1-P(A)$, $P(A\cup B)=P(A)+P(B)-P(A\cap B)$
- If independent: $P(A\cap B)=P(A)P(B)$
- Odds in favor of $A$: $P(A):P(A^c)=p:(1-p)$, value $p/(1-p)$ means success:failure 
- Odds against $A$: $P(A^c):P(A)=(1-p):p$, value $(1-p)/p$ means success:failure ex)odds of 35 to 1 against landing on 22 P(22) = 35:1 = 1/36
- If odds in favor are $a:b$, then $P(A)=a/(a+b)$
- If odds against are $a:b$, then $P(A)=b/(a+b)$
- Expected value: $E(X)=\sum xP(X=x)$
- EV is long-run average, may be a value not actually attainable

---

## 11. Scatterplot / correlation
- Scatterplot shows two numerical variables measured on same individuals
- Put explanatory variable on horizontal axis
- Describe by direction, form, strength, outliers
- Positive association: above-average $x$ with above-average $y$
- Negative association: above-average $x$ with below-average $y$
- Correlation $r$: direction and strength of linear relationship; $-1\le r\le 1$
- Correlation measures linear relationship only
- Correlation unaffected by unit changes
- Correlation $\neq$ causation
- Outliers can change $r$ a lot
- Restricting/truncating $x$-range usually makes $r$ smaller
$r=\frac{1}{n-1}\sum \left(\frac{x_i-\bar{x}}{s_x}\right)\left(\frac{y_i-\bar{y}}{s_y}\right)$  
  $r$: sample correlation   $\bar{x}, \bar{y}$: sample means  $s_x, s_y$: sample standard deviations  Measures direction and strength of the linear relationship between $x$ and $y$  Always satisfies $-1 \le r \le 1$

---

## 12. Regression / residual / $r^2$
- Least-squares line: $\hat y=a+bx$
- $b$: predicted change in $y$ for 1-unit increase in $x$
- $a$: predicted $y$ at $x=0$
- Residual: $y-\hat y$; positive = above line, negative = below
- Sample correlation from raw data: $r=\frac{1}{n-1}\sum \left(\frac{x_i-\bar x}{s_x}\right)\left(\frac{y_i-\bar y}{s_y}\right)$
- Slope: $b=r\frac{s_y}{s_x}$
- Intercept: $a=\bar y-b\bar x$
- $r^2$: proportion of variation in $y$ explained by regression on $x$
- Unexplained variation: $1-r^2$
- Also: $r^2=1-\frac{\sum(y_i-\hat y_i)^2}{\sum(y_i-\bar y)^2}$

---

## 13. Regression by standardization
- Standardize explanatory: $z_x=\frac{x-\bar x}{s_x}$
- Predict response z-score: $\hat z_y=rz_x$
- Convert back: $\hat y=\bar y+s_y\hat z_y$
- Equivalent: $\hat y=\bar y+r\frac{s_y}{s_x}(x-\bar x)$
- Reverse prediction: $z_y=\frac{y-\bar y}{s_y}$, $\hat z_x=rz_y$, $\hat x=\bar x+s_x\hat z_x$
- If predicting $x$ from $y$, slope is $r\frac{s_x}{s_y}$, not $r\frac{s_y}{s_x}$
- Extrapolation = predict outside observed $x$-range
- Regression to the mean: if $|r|<1$, extreme observations are predicted to be less extreme next time because $\hat z=rz$

---

## 14. Sampling distributions / SE / MOE
- Sampling distribution = distribution of statistic over repeated random samples
- SE = SD of sampling distribution
- SD = spread of individual data; SE = spread of statistic
- $\hat p=x/n$, $\bar x$ = sample mean
- Rough 95% MOE for a proportion: $MOE\approx \frac{1}{\sqrt n}$
- Rough 95% CI for change estimate with known SE: estimate $\pm 2(SE)$

---

## 15. Confidence intervals
- General form: estimate $\pm$ critical value $\times$ SE
- Proportion CI: $\hat p\pm z^*\sqrt{\frac{\hat p(1-\hat p)}{n}}$
- Mean CI: $\bar x\pm z^*\frac{s}{\sqrt n}$
- Common $z^*$: 90% $1.645$, 95% $1.96$, 99% $2.576$
- MOE $=z^*\cdot SE$
- Confidence level = long-run success rate of method
- CI is for parameter, not individual observations
- Good CI sentence: “I am __% confident that the true population [mean/proportion] of ___ is between ___ and ___.”

---

## 16. Hypothesis testing 
- first define the parameter, then state the estimate from the sample, the compute SE, find the multiplier of confidence level, compute estimate $\pm$ multiplier $\times$ standard error 
- Null: $H_0:p=p_0$ or $H_0:\mu=\mu_0$
- Alternative: $>$, $<$, or $\ne$
- Proportion test statistic: $z=\frac{\hat p-p_0}{\sqrt{\frac{p_0(1-p_0)}{n}}}$
- Mean test statistic: $z=\frac{\bar x-\mu_0}{s/\sqrt n}$
- Important:
  - CI for $p$ uses $\hat p$ in SE
  - test for $p$ uses $p_0$ in SE
- P-value = probability, assuming $H_0$ true, of result at least as extreme as observed
- Right-tailed: area right; left-tailed: area left; two-sided: double smaller tail
- Small P-value = strong evidence against $H_0$
- $P<0.05$: statistically significant; $P<0.01$: very strong evidence
- Do not say P-value is probability $H_0$ is true
- Statistical significance $\neq$ practical importance
- Conclusion template: “There is [weak/some/strong/very strong] evidence that the true [mean/proportion/probability] of ___ is [greater than/less than/different from] ___.”

---

## 17. Worked examples

- Regression z-score prediction: given $r=0.77$, explanatory z-score $z_x=2.4$. Predict response z-score with $\hat z_y=rz_x=0.77(2.4)=1.848\approx1.85$.
- Slope example: given $r=0.77$, $s_y=30.39$, $s_x=22.26$. Use $b=r\frac{s_y}{s_x}=0.77\cdot\frac{30.39}{22.26}\approx1.05$.
- One-cutoff Normal example: heights have mean $166$, SD $10$. Fraction under $180$: first standardize, $z=\frac{180-166}{10}=1.4$. Then use table: $P(Z<1.4)\approx0.9192$. So about $91.9\%$ are under $180$.
- Between-two-cutoffs Normal example: BMI has mean $22.02$, SD $3.132$. Fraction between $18$ and $25$: standardize both cutoffs, $z_{18}=\frac{18-22.02}{3.132}\approx-1.28$, $z_{25}=\frac{25-22.02}{3.132}\approx0.951$. Then subtract left-tail areas: $P(18<X<25)=P(Z<0.951)-P(Z<-1.28)$, about $70\%-74\%$ depending on table rounding.
- Percentile from z: if $z=1.646$, look up the left-tail area in the normal table. That is about the $95$th percentile.
- Proportion CI example: if $x=34$, $n=400$, first estimate $\hat p=\frac{34}{400}=0.085$. Then compute $SE=\sqrt{\frac{\hat p(1-\hat p)}{n}}=\sqrt{\frac{0.085(0.915)}{400}}\approx0.0139$. For $96.5\%$ confidence, $z^*\approx2.1$. So $CI=\hat p\pm z^*SE=0.085\pm2.1(0.0139)=(0.0557,0.1143)$.
- Proportion test example: if $p_0=0.25$, $n=1600$, $x=450$, start with $\hat p=\frac{450}{1600}=0.28125$. For a test, use $SE=\sqrt{\frac{p_0(1-p_0)}{n}}=\sqrt{\frac{0.25(0.75)}{1600}}=0.0108$. Then $z=\frac{\hat p-p_0}{SE}=\frac{0.28125-0.25}{0.0108}\approx2.89$. For a two-sided test, double the smaller tail area: P-value $\approx2(0.19\%)=0.38\%$. Very small P-value $\Rightarrow$ very strong evidence against $p=0.25$.
- Mean CI example: if $n=1174$, $\bar x=3386.7$, $s=519.6$, first compute $SE=\frac{s}{\sqrt n}=\frac{519.6}{\sqrt{1174}}\approx15.16$. For confidence level $0.8064$, the multiplier is $z^*\approx1.30$. Then $\bar x\pm z^*SE=3386.7\pm1.3(15.16)=(3366.98,3406.41)$.
- Quick 95% CI using $1/\sqrt n$: if $n=1000$, $\hat p=0.61$, use rough MOE $=\frac{1}{\sqrt{1000}}\approx0.0316\approx3.2\%$. Then CI is $61\%\pm3.2\%=(57.8\%,64.2\%)$.
- Change + CI example: if estimated change is $-1800$ and $SE=5700$, use rough $95\%$ rule $MOE\approx2(SE)=11400$. Then CI is $-1800\pm11400=(-13200,9600)$.
- Odds against example: if odds against are $25:5$, probability of the event is $\frac{5}{25+5}=\frac16\approx0.1667$.
- Regression forward example: entrance score mean $72$, SD $12$; GPA mean $2.8$, SD $0.4$; $r=0.5$; score $90$. Step 1: standardize predictor, $z_x=\frac{90-72}{12}=1.5$. Step 2: predict response z-score, $\hat z_y=0.5(1.5)=0.75$. Step 3: convert back, $\hat y=2.8+0.75(0.4)=3.1$.
- Regression reverse slope example: if predicting $x$ from $y$ and $r=0.5$, $s_x=12$, $s_y=0.4$, the slope is $b=r\frac{s_x}{s_y}=0.5\cdot\frac{12}{0.4}=15$. Use this, not $r\frac{s_y}{s_x}$.
- Undoing standardization example: explanatory mean $19.4$, SD $4.3$; response mean $25$, SD $6.8$; $r=0.62$; explanatory score $27$. Standardize: $z_x=\frac{27-19.4}{4.3}=1.77$. Predict response z-score: $\hat z_y=0.62(1.77)\approx1.10$. Convert back: $\hat y=25+1.10(6.8)\approx32.45$.
- Reverse from final score example: using same setup, if final score is $32.45$, standardize response: $z_y=\frac{32.45-25}{6.8}\approx1.096$. Predict explanatory z-score: $\hat z_x=0.62(1.096)\approx0.679$. Convert back: $\hat x=19.4+4.3(0.679)\approx22.3$.
---

## 18. Final checklist
- Population or sample?
- Parameter or statistic?
- Observational study or experiment?
- Random sampling or random assignment?
- Any bias / nonresponse / undercoverage / overcoverage / confounding?
- Graph: shape, center, spread, outlier, misleading scale?
- Use mean/SD or median/IQR?
- Normal? one cutoff or two cutoffs?
- Correlation or causation?
- Predicting $y$ from $x$, or $x$ from $y$?
- CI or hypothesis test?
- One-sided or two-sided?
- Did I write the final sentence in context?
---
19. 
- **Population** = actual group of people/objects of interest; **population size** = number in that group. Do not say “the population is 270”; 270 is the size, not the population.
- **Target sample** = people you tried to survey; **respondents** = people who actually answered. If asked for the intended sample, do not give only the respondents.
- **Census** = attempt to survey the entire population. Even with nonresponse, it can still be a census attempt, just incomplete.
- **Frame** = list/system used to identify who can be sampled. **Undercoverage** = people in population missing from frame. **Overcoverage** = people in frame who are not in population. Both can happen at the same time and do not cancel out.
- **Response bias can have direction**: estimates can be **biased down** or **biased up**. Example: self-reported weight may be biased down if people underreport weight or heavier people skip the question.
- Write estimates as estimates: better to say **“We estimate the percentage to be 61%”** than **“61% of Canadian adults think...”**
- Quick 95% rule for a proportion: $MOE \approx \frac{1}{\sqrt{n}}$, so rough CI is $\hat p \pm MOE$. Example: if $n=1000$, $\hat p=0.61$, then $MOE\approx0.0316\approx3.2\%$, so CI $\approx 61\%\pm3.2\%=(57.8\%,64.2\%)$.
- If a change estimate and its standard error are given, a rough 95% CI is **estimate $\pm 2(SE)$**. Example: estimate $=-1800$, $SE=5700$, so CI $\approx -1800\pm11400=(-13200,9600)$.
- **Experiment** = researcher manipulates at least one explanatory variable. Even if the design is bad, it is still an experiment if the treatment was controlled. If nothing is imposed, it is observational.
- To explain **confounding** well, do not just name a confounder; explain how it is related to both the explanatory/treatment variable and the response variable.
- Better criticism of causal claims from correlation: do not just write **“correlation is not causation”**; name likely lurking variables such as **ability**, **study habits**, **background preparation**, **income**, **store size**, etc.
- **Histogram endpoint rule**: if bins include the lower limit but not the upper limit, boundary values go into the bar on the **right**.
- A graph can be misleading if different series use **different vertical scales**.
- Quartiles of a Normal distribution can be approximated using z-scores: $Q1\approx z=-0.7$, median $=z=0$, $Q3\approx z=0.7$, then convert back using $x=\mu+z\sigma$.
- If you **truncate** data by removing large values above a cutoff, both the **mean** and the **SD** usually decrease.
- Reverse regression is different from ordinary regression. Predicting $y$ from $x$: $b=r\frac{s_y}{s_x}$. Predicting $x$ from $y$: $b=r\frac{s_x}{s_y}$. Do not reuse the wrong slope.
- If $r=0.5$, then $R^2=r^2=0.25$, so **25% of the variation in $y$ is explained by the regression on $x$**.
- Regression example: if mean entrance score $=72$, SD $=12$, mean GPA $=2.8$, SD $=0.4$, $r=0.5$, and score $=90$, then $z_x=\frac{90-72}{12}=1.5$, $\hat z_y=0.5(1.5)=0.75$, and predicted GPA is $\hat y=2.8+0.75(0.4)=3.1$.
- Reverse prediction example: if explanatory mean $=19.4$, SD $=4.3$, response mean $=25$, SD $=6.8$, $r=0.62$, and explanatory score $=27$, then $z_x=\frac{27-19.4}{4.3}=1.77$, $\hat z_y=0.62(1.77)\approx1.10$, and predicted response is $\hat y=25+1.10(6.8)\approx32.45$. Reversing from final score $32.45$: $z_y=\frac{32.45-25}{6.8}\approx1.096$, $\hat z_x=0.62(1.096)\approx0.679$, so $\hat x=19.4+4.3(0.679)\approx22.3$. Do not just go back to the original 27.
- To compute **sample correlation from raw standardized values**, average the products of the paired z-scores: if the six products are $0.94, 0.57, 0.01, -0.52, 1.22, -0.33$, then $r=\frac{0.94+0.57+0.01-0.52+1.22-0.33}{5}\approx0.378$. If using raw data, use $r=\frac{1}{n-1}\sum \left(\frac{x_i-\bar x}{s_x}\right)\left(\frac{y_i-\bar y}{s_y}\right)$.
- **Regression effect / regression to the mean** can explain why children of low-IQ birth mothers may have average IQs closer to the population mean even without any environmental effect. Example: if mothers average $85$ when population mean is $100$ and SD is $15$, then mothers are $1$ SD below average; if $r=0.5$, children are predicted to be only $0.5$ SD below average, i.e. about $100-0.5(15)=92.5$.
- For **equally likely outcomes**, probability = $1/(\text{number of outcomes})$. Example: European roulette has 37 equally likely slots, so $P(22)=1/37$.
- For **odds against** $35:1$, probability of the event is $1/(35+1)=1/36$. More generally, if odds against are $a:b$, then $P(\text{event})=\frac{b}{a+b}$.
- If several probabilities are given and must sum to 1, find the missing one by subtraction. Example: for a tetrahedral die, if $P(1)=0.3$, $P(2)=0.4$, $P(3)=0.2$, then $P(4)=1-0.3-0.4-0.2=0.1$.
- For the probability of **either A or B** when outcomes are disjoint, add: $P(A\text{ or }B)=P(A)+P(B)$. Example: $P(1\text{ or }3)=0.3+0.2=0.5$.
- **Expected value** of a fair tetrahedral die: $E(X)=1(1/4)+2(1/4)+3(1/4)+4(1/4)=2.5$.
- For two fair tetrahedral dice, there are **16 equally likely ordered outcomes**: $(1,1),(1,2),\dots,(4,4)$, each with probability $1/16$.
- If a random variable is the **difference** Red − Green for two fair tetrahedral dice, possible values are $-3,-2,-1,0,1,2,3$ with probabilities $1/16,2/16,3/16,4/16,3/16,2/16,1/16$, so the expected value is $0$.
- Multi-stage gambling / roulette EV problems: identify the **possible final values** and their probabilities first, then compute expected value. Example: if you leave with either $0$ or $1296$ dollars and $P(1296)=(1/38)^2$, then $E(\text{final amount})=1296(1/38)^2+0(1-(1/38)^2)\approx0.898$. If you started with \$1, expected **winnings** are $0.898-1=-0.102$.
- Confidence interval for a probability with many trials: estimate $\hat p=x/n$, compute $SE=\sqrt{\frac{\hat p(1-\hat p)}{n}}$, then do $\hat p\pm z^*SE$. Example: roulette red results $4915/10000$ give $\hat p=0.4915$, $SE\approx0.00500$, and for confidence level $80.64\%$, $z^*\approx1.3$, so CI is $0.4915\pm1.3(0.00500)\approx(0.485,0.498)$.
- Confidence interval for a **population mean**: use sample mean $\bar x$, sample SD $s$, and $SE=s/\sqrt n$. Example: $n=2500$, $\bar x=1.7$, $s=2.7$, so $SE=\frac{2.7}{\sqrt{2500}}=0.054$. For a 98% CI, use $z^*\approx2.05$, so CI is $1.7\pm2.05(0.054)\approx(1.59,1.81)$.
- A population may **not** be approximately Normal if the SD is too large relative to the mean and Normality would imply impossible negative values. Example: if mean papers published is $1.7$ and SD is $2.7$, a Normal model would put noticeable probability below $0$, which is impossible.
- One-sided p-value from a z-statistic: if alternative is $p>p_0$, use the **right-tail** area. Example: with $\hat p=180/900=0.2$, $p_0=1/6$, $SE=\sqrt{\frac{p_0(1-p_0)}{900}}\approx0.0124$, and $z\approx2.68$, the p-value is the area to the right, about $0.0035$. Very small p-value $\Rightarrow$ strong evidence that $p>p_0$.
- If given a **one-sided p-value** and asked for the z-statistic, reverse the tail logic. Example: if right-tail p-value is $0.0019$, then the left-tail area is $0.9981$, which corresponds to about $z=2.9$.
- In tests, always match the **direction of the alternative** to the tail: if the treatment is supposed to increase the response, then the p-value is in the **right-hand tail**.
- Two-sided mean test example: if $\bar x=299054.2$, $\mu_0=299792.5$, $s=54.2$, $n=20$, then $SE=\frac{54.2}{\sqrt{20}}\approx12.1$ and $z=\frac{299054.2-299792.5}{12.1}\approx-61.0$. This is so extreme that the p-value is essentially $0$. A practical interpretation is that the method is **badly biased**, and the bias dominates the random variation.
- Good exam answers usually need **words, not just formulas**. In probability/CI/test questions, explain the setup briefly: what parameter is being estimated or tested, why the formula applies, and what the final number means.


---
## 21. Finding critical values / multipliers from confidence level
- If confidence level is $C$: middle area $=C$, total outside area $=1-C$, each tail area $=\frac{1-C}{2}$, left cumulative area for Table B $=1-\frac{1-C}{2}=\frac{1+C}{2}$; use this left cumulative area to find the critical value $z^*$.
- Common examples: 90% confidence $\to$ left cumulative area $=0.95$ $\to$ $z^*\approx1.645$; 95% confidence $\to$ left cumulative area $=0.975$ $\to$ $z^*\approx1.96$; 99% confidence $\to$ left cumulative area $=0.995$ $\to$ $z^*\approx2.58$.
- Exact 95% CI for a proportion: $\hat p \pm 1.96\sqrt{\frac{\hat p(1-\hat p)}{n}}$; rough 95% MOE rule: $MOE\approx\frac{1}{\sqrt n}$. Why the rough rule works: $1.96\approx2$ and if $\hat p$ is near $0.5$, then $\sqrt{\hat p(1-\hat p)}\approx0.5$, so $1.96\sqrt{\frac{\hat p(1-\hat p)}{n}}\approx2\sqrt{\frac{0.25}{n}}=\frac{1}{\sqrt n}$. Works best when $\hat p$ is between about $0.4$ and $0.6$; if $\hat p$ is far from $0.5$, the exact SE using $\hat p(1-\hat p)$ is better. The rough rule $\frac{1}{\sqrt n}$ is conservative, so it usually gives a slightly wider interval than the exact 95% proportion CI.

## Ecological fallacy / regression fallacy
- Ecological fallacy: making a claim about individuals using only group-level data; a relationship seen for groups does not have to hold for individuals.
- Example: if cities with higher income have higher crime rates, it does not follow that richer individuals commit more crime.
- Regression fallacy: incorrectly explaining regression to the mean as a real causal change.
- Regression to the mean: if correlation is imperfect ($|r|<1$), an extreme first measurement is usually followed by a less extreme second measurement.
- Example: students with very low first scores often improve next time partly because of regression to the mean, not necessarily because of a special treatment. Regression to the mean prediction: Step 1: standardize first score: $z_1=\frac{x_1-\mu_1}{\sigma_1}$ Step 2: predict second z-score: $\hat z_2 = r z_1$ Step 3: convert back: $\hat y_2=\mu_2+\hat z_2\sigma_2$ If $|r|<1$, an extreme first score is predicted to be less extreme on the second measurement.