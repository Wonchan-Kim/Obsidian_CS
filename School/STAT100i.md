<div class="stat-cheatsheet">

# STAT 100 FINAL CHEAT SHEET

## 0. Scope
- Emphasis on post-midterm material
- Excluded: Ch. 7, 16, 19, 23, 24
- Core: surveys / experiments / graphs / summary stats / normal / regression / correlation / probability / expected value / CI / tests

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
- Bigger sample: reduces variability, not bias
- Probability sample: random mechanism known
- SRS: every sample of size \(n\) has equal chance
- Convenience sample: easy to reach, weak representativeness
- Voluntary response: people choose themselves, strong opinions overrepresented
- Undercoverage: some population members missing from frame
- Overcoverage: frame includes people not in population
- Nonresponse: selected people do not respond
- Response bias: inaccurate answers due to wording / sensitivity / mode
- Questionnaire effects / interviewer bias / mode effects all can bias answers
- Random sampling \(\to\) supports population inference
- Random assignment \(\to\) supports causal claims

---

## 3. Population / target sample / frame / census
- Population = actual set of people/objects of interest
- Population size = number in that set
- Target sample = people you tried to survey
- Respondents = people who actually replied
- Frame = list/method used to identify who can be sampled
- Census = attempt to survey entire population; can still be an incomplete/failed census if not everyone responds
- Same frame can have both undercoverage and overcoverage; they do not “cancel out”

---

## 4. Study types
- Sample survey: collect data from sample of population
- Census: collect data from whole population
- Observational study: observe, no imposed treatment; association only
- Experiment: researcher manipulates at least one explanatory variable
- A bad design can still be an experiment if treatment was controlled
- Explanatory = predictor / independent / \(x\)
- Response = outcome / dependent / \(y\)
- Lurking variable: omitted variable affecting relationship
- Confounding: effects tangled, cannot separate clearly
- Common response: \(x\) and \(y\) both respond to same lurking variable
- To explain confounding well, connect confounder to both explanatory and response variables

---

## 5. Sampling methods: distinguish
- SRS: choose individuals directly from whole population
- Stratified: divide into strata, sample from each stratum
- Cluster: divide into clusters, choose some whole clusters, survey everyone/many there
- Multi-stage: sample in stages, e.g. schools \(\to\) classes \(\to\) students
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
- Misleading graph clue: different series may use different \(y\)-axis scales

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
- Left-skewed density: \(\text{mean}<\text{median}<\text{mode}\)
- Right-skewed density: \(\text{mode}<\text{median}<\text{mean}\)

---

## 8. Median / quartiles / five-number summary
- Sort data first
- Median:
  - odd \(n\): middle value, position \((n+1)/2\)
  - even \(n\): average of middle two, positions \(n/2\) and \(n/2+1\)
- Quartiles (intro rule):
  - odd \(n\): remove overall median, then find medians of lower/upper halves
  - even \(n\): split into two equal halves, find medians of halves
- Five-number summary: min, \(Q1\), median, \(Q3\), max
- \(IQR=Q3-Q1\)
- SD facts: deviations from mean sum to 0; SD measures typical distance from mean; larger SD = more spread
- If remove all large values above a cutoff, both mean and SD usually decrease

---

## 9. Normal distribution / z / percentile
- Normal curve: symmetric, bell-shaped, fully determined by mean and SD
- Mean controls center, SD controls spread
- 68–95–99.7 rule: about 68%, 95%, 99.7% within 1, 2, 3 SDs
- Standard score: \(z=(x-\mu)/\sigma\) or observed minus mean over SD/SE
- \(z>0\): above mean; \(z<0\): below mean
- Percentile = percent below a value
- Median = 50th percentile; \(Q1=25\)th; \(Q3=75\)th
- In Normal distributions, z-scores map directly to percentiles
- Approx: \(z=1\) is 84th percentile, \(z=-1\) is 16th percentile
- Quartiles of Normal: \(Q1\approx z=-0.7\), median \(z=0\), \(Q3\approx z=0.7\); convert back with \(x=\mu+z\sigma\)

---

## 10. Probability / odds / EV
- Random: individual outcomes uncertain, long-run pattern regular
- Probability: long-run proportion
- Personal probability: subjective judgment
- Event: set of outcomes
- Probability model: mathematical description of chance behavior
- Rules: \(P(A^c)=1-P(A)\), \(P(A\cup B)=P(A)+P(B)-P(A\cap B)\)
- If independent: \(P(A\cap B)=P(A)P(B)\)
- Odds in favor of \(A\): \(P(A):P(A^c)=p:(1-p)\), value \(p/(1-p)\)
- Odds against \(A\): \(P(A^c):P(A)=(1-p):p\), value \((1-p)/p\)
- If odds in favor are \(a:b\), then \(P(A)=a/(a+b)\)
- If odds against are \(a:b\), then \(P(A)=b/(a+b)\)
- Expected value: \(E(X)=\sum xP(X=x)\)
- EV is long-run average, may be a value not actually attainable

---

## 11. Scatterplot / correlation
- Scatterplot shows two numerical variables measured on same individuals
- Put explanatory variable on horizontal axis
- Describe by direction, form, strength, outliers
- Positive association: above-average \(x\) with above-average \(y\)
- Negative association: above-average \(x\) with below-average \(y\)
- Correlation \(r\): direction and strength of linear relationship; \(-1\le r\le 1\)
- Correlation measures linear relationship only
- Correlation unaffected by unit changes
- Correlation \(\neq\) causation
- Steep slope does not imply strong correlation
- Outliers can change \(r\) a lot
- Restricting/truncating \(x\)-range usually makes \(r\) smaller

---

## 12. Regression / residual / \(r^2\)
- Least-squares line: \(\hat y=a+bx\)
- \(b\): predicted change in \(y\) for 1-unit increase in \(x\)
- \(a\): predicted \(y\) at \(x=0\) (may be meaningless if outside data range)
- Least squares minimizes sum of squared vertical residuals
- Residual: \(y-\hat y\); positive = point above line, negative = below
- Sample correlation from raw data:
  \[
  r=\frac{1}{n-1}\sum \left(\frac{x_i-\bar x}{s_x}\right)\left(\frac{y_i-\bar y}{s_y}\right)
  \]
- Slope:
  \[
  b=r\frac{s_y}{s_x}
  \]
- Intercept:
  \[
  a=\bar y-b\bar x
  \]
- \(r^2\): proportion of variation in \(y\) explained by regression on \(x\)
- Unexplained variation: \(1-r^2\)
- Also:
  \[
  r^2=1-\frac{\sum(y_i-\hat y_i)^2}{\sum(y_i-\bar y)^2}
  \]

---

## 13. Regression by standardization
- Standardize explanatory:
  \[
  z_x=\frac{x-\bar x}{s_x}
  \]
- Predict response z-score:
  \[
  \hat z_y=rz_x
  \]
- Convert back:
  \[
  \hat y=\bar y+s_y\hat z_y
  \]
- Equivalent:
  \[
  \hat y=\bar y+r\frac{s_y}{s_x}(x-\bar x)
  \]
- Reverse prediction is different:
  \[
  z_y=\frac{y-\bar y}{s_y},\quad \hat z_x=rz_y,\quad \hat x=\bar x+s_x\hat z_x
  \]
- If predicting \(x\) from \(y\), slope is \(r(s_x/s_y)\), not \(r(s_y/s_x)\)
- Extrapolation = predict outside observed \(x\)-range; dangerous
- Regression to the mean: if \(|r|<1\), extreme observations are predicted to be less extreme next time because \(\hat z=rz\)

---

## 14. Sampling distributions / SE / MOE
- Sampling distribution = distribution of statistic over repeated random samples
- SE = SD of sampling distribution
- SD = spread of individual data; SE = spread of statistic
- \(\hat p=x/n\), \(\bar x\) = sample mean
- Rough 95% MOE for a proportion at early-course level:
  \[
  MOE\approx \frac{1}{\sqrt n}
  \]
- Rough 95% CI for change estimate with known SE:
  \[
  \text{estimate}\pm 2(SE)
  \]

---

## 15. Confidence intervals
- General form: estimate \(\pm\) critical value \(\times\) SE
- Proportion CI:
  \[
  \hat p\pm z^*\sqrt{\frac{\hat p(1-\hat p)}{n}}
  \]
- Mean CI:
  \[
  \bar x\pm z^*\frac{s}{\sqrt n}
  \]
- Common \(z^*\): 90% 1.645, 95% 1.96, 99% 2.576
- MOE \(=z^*\cdot SE\)
- Confidence level = long-run success rate of method, not probability the fixed parameter is random
- CI is for parameter, not individual observations
- Write estimates as estimates: “We estimate …”, not “The population is definitely …”
- Good CI sentence: “I am __% confident that the true population [mean/proportion] of ___ is between ___ and ___.”

---

## 16. Hypothesis testing
- Null: \(H_0:p=p_0\) or \(H_0:\mu=\mu_0\)
- Alternative: \(>\), \(<\), or \(\ne\)
- Proportion test statistic:
  \[
  z=\frac{\hat p-p_0}{\sqrt{\frac{p_0(1-p_0)}{n}}}
  \]
- Mean test statistic:
  \[
  z=\frac{\bar x-\mu_0}{s/\sqrt n}
  \]
- Important:
  - CI for \(p\) uses \(\hat p\) in SE
  - test for \(p\) uses \(p_0\) in SE
- P-value = probability, assuming \(H_0\) true, of result at least as extreme as observed
- Right-tailed: area right; left-tailed: area left; two-sided: double smaller tail
- Small P-value = strong evidence against \(H_0\)
- \(P<0.05\): statistically significant; \(P<0.01\): very strong evidence
- Do not say P-value is probability \(H_0\) is true
- Statistical significance \(\neq\) practical importance
- Conclusion template: “There is [weak/some/strong/very strong] evidence that the true [mean/proportion/probability] of ___ is [greater than/less than/different from] ___.”

---

## 17. Compact worked examples

### Regression z-score prediction
Given \(r=0.77\), \(z_x=2.4\):
\[
\hat z_y=0.77(2.4)=1.848\approx 1.85
\]

### Slope example
Given \(r=0.77\), \(s_y=30.39\), \(s_x=22.26\):
\[
b=0.77\cdot \frac{30.39}{22.26}\approx 1.05
\]

### One-cutoff Normal example
Heights mean 166, SD 10; fraction under 180:
\[
z=\frac{180-166}{10}=1.4,\quad P(Z<1.4)\approx 0.9192
\]
So about 91.9%.

### Between-two-cutoffs Normal example
BMI mean 22.02, SD 3.132; fraction between 18 and 25:
\[
z_{25}\approx \frac{25-22.02}{3.132}=0.951,\quad z_{18}\approx \frac{18-22.02}{3.132}=-1.28
\]
Then \(P(18<X<25)=P(Z<0.951)-P(Z<-1.28)\), about 70–74%.

### Percentile from z
If \(z=1.646\), percentile is about 95th.

### Proportion CI example
If \(x=34,n=400\):
\[
\hat p=34/400=0.085,\quad SE=\sqrt{0.085(0.915)/400}\approx 0.0139
\]
For 96.5% confidence, \(z^*\approx 2.1\):
\[
CI=0.085\pm 2.1(0.0139)=(0.0557,0.1143)
\]

### Proportion test example
If \(p_0=0.25\), \(n=1600\), \(x=450\):
\[
\hat p=450/1600=0.28125,\quad SE=\sqrt{0.25(0.75)/1600}=0.0108
\]
\[
z=\frac{0.28125-0.25}{0.0108}\approx 2.89
\]
Two-sided P-value \(\approx 2(0.19\%)=0.38\%\), so very strong evidence against \(p=0.25\).

### Mean CI example
If \(n=1174,\bar x=3386.7,s=519.6\):
\[
SE=\frac{519.6}{\sqrt{1174}}\approx 15.16
\]
For confidence 0.8064, \(z^*\approx 1.30\):
\[
3386.7\pm 1.3(15.16)=(3366.98,3406.41)
\]

### Quick 95% CI using \(1/\sqrt n\)
If \(n=1000,\hat p=0.61\):
\[
MOE\approx \frac{1}{\sqrt{1000}}\approx 0.0316\approx 3.2\%
\]
So:
\[
61\%\pm 3.2\%=(57.8\%,64.2\%)
\]

### Change + CI example
If estimate of change is \(-1800\) and \(SE=5700\):
\[
MOE\approx 2(5700)=11400
\]
\[
-1800\pm 11400=(-13200,9600)
\]

### Odds against example
If odds against are \(25:5\):
\[
P(\text{event})=\frac{5}{25+5}=\frac16\approx 0.1667
\]

### Regression forward example
Entrance score mean 72, SD 12; GPA mean 2.8, SD 0.4; \(r=0.5\); score 90:
\[
z_x=\frac{90-72}{12}=1.5,\quad \hat z_y=0.5(1.5)=0.75,\quad \hat y=2.8+0.75(0.4)=3.1
\]

### Regression reverse example
If \(r=0.5,s_x=12,s_y=0.4\), reverse slope:
\[
b=r\frac{s_x}{s_y}=0.5\cdot \frac{12}{0.4}=15
\]

### Undoing standardization example
If explanatory mean 19.4, SD 4.3; response mean 25, SD 6.8; \(r=0.62\); score 27:
\[
z_x=\frac{27-19.4}{4.3}=1.77,\quad \hat z_y=0.62(1.77)\approx 1.10
\]
\[
\hat y=25+1.10(6.8)\approx 32.45
\]

### Reverse from final score
If final score is 32.45:
\[
z_y=\frac{32.45-25}{6.8}\approx 1.096,\quad \hat z_x=0.62(1.096)\approx 0.679
\]
\[
\hat x=19.4+4.3(0.679)\approx 22.3
\]

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
- Predicting \(y\) from \(x\), or \(x\) from \(y\)?
- CI or hypothesis test?
- One-sided or two-sided?
- Did I write the final sentence in context?


---
cssclasses:
  - stat-cheatsheet
---

# Test

- Complement: \(P(A^c)=1-P(A)\)
- Union: \(P(A\cup B)=P(A)+P(B)-P(A\cap B)\)
- If independent: \(P(A\cap B)=P(A)P(B)\)

$$
z=\frac{x-\mu}{\sigma}
$$

$$
\hat p=\frac{x}{n}
$$

$$
SE=\sqrt{\frac{\hat p(1-\hat p)}{n}}
$$

</div>