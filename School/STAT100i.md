
bias: consistent, repeated deviation of the sample statistic from the population parameter in the same direction when we take many samples. ->systemic overestimate or underestimate of the population parameter, use SRS 

variability: how the values of the sample statistic will vary when we take many samples. large variability-> result of sampling is not repeatable, use a larger sample. 

margin of error +- 4 % -> if we took many samples using the same method we used to get this one sample, 95% of the sample would give a result within a plus or minus 4 % of the truth about the population. 
Margin of error -> 1/size of SRS
A confidence statement has two parts: a margin of error and a level of confidence. The margin of
error says how close the sample statistic lies to the population parameter. The level of confidence says
what percentage of all possible samples satisfy the margin of error.


mean vs median: symmetry vs skewness
	mean and sd(standard deviations) are strongly affected by outliers or by the long tail of a skewed distribution. 
	five number summary(min, 3rd quartile, median, 1st quartile, max) is usually better than the mean an dsd for describing a skewed distribution or w outliers. 
measuring spread -> Q3 - Q1 interquartile range 
sd : root (sum of deviations from avg ^ 2 / n-1) : deviations always add up to 0


![[Pasted image 20260303145822.png|left|300]]
![[Pasted image 20260303150022.png|right|400]]
Normal distributions: symmetric, mean and median lie together at the peak in the center of the curve. 
Normal curve- completely described by giving its mean and its sd. The mean determines the center of the distribution. It is located at the center of symmetry of the curve. Standard deviation determines the shape of the curve. sd is the distance from the mean to the change of curvature points on either side. 

68-95-99.7 rule: mean +- 1/2/3 sd. ![[Pasted image 20260303150708.png|400]]

standard scores - ex) mean is 500, sd is 100. For score 600, can be expressed as the one sd above the mean. Observations expressed in sd above or below the mean of a distribution are called standard scores, often referred to as z-scores. 
z = x - u / sd

Exclusively for normal distribution, standard scores translate directly into percentiles. 
The cth percentile of a distribution is a value such that c percent of the observations lie below it and the rest lie above. 
The median of any distribution is the 50th percentile, and the quartiles are the 25th and 75th percentiles. In any normal distribution, the point one sd above the mean is the 84th percentile. (50 + 34)

Scatterplot: relationship between two quantitative variables measured on the same individuals. The values of one variable appear on the horizontal axis, and the values of the other variable appear on the vertical axis. Each individual in the data appears as the point in the plot fixed by the values of both variables for that individual.  X -> explanatory variable Y -> response variable

Positively associated: above average values of one tend to accompany above average values of the other and below average values tend to occur together. ![[Pasted image 20260303210622.png|400]]
correlation describes the direction and strength of a straight line relationship between two quantitive variables, usually written as r. 

regression line: straight line describes how a response variable y changes as an explanatory variable x changes. Predict y for a given value of x.  (y-y^ / sy = r * x - x^/sx)
y = a+bx with b = r*sy/sx and a = y_ - bx_
x^ = x_ + rsx (y-y_)/sy

zy^ = rzx
y^ = y_ +zy^ sy

correlation can be weak even if the slope is steep as a steep line does not guarantee the strong correlation if the points are widely scattered around it. 


least squares regression line of y on x is the line that makes the sum of the squares of the vertical distances of the data points from the line as small as possible. 

extrapolation: prediction outside the range

r^2: square of correlation, r^2 is the proportion of the variation in the values of y that is explained by the least squares of y on x. 
usefulness of the regression line for prediction depends on the strength of the association(correlation). 
ex) r = 0.994 -> r^2 = 0.998, so the variation along the line accounts 98.8 of all variation. The scatter of the points about the line accounts for only the remaining 1.2 %. Little leftover scatter says that prediction will be accurate. 

1-r^2 -> unexplained variability. residual = actual y - predicted y. 
sum of squared residuals: sigma(y_ - y)^2
r^2 = 1 - sigma(yi - y^)^2 / sigma(yi-y_)^2
Sum of squared residuals = (1 − r 2) times sum of squared deviations from average son’s height.

regression to the mean -> ex two midterm avg 70, sd 10, r = 0.5, z predicted y = r*zx
first midterm 90 -> 2sd -> estimate 0.5** 2 = 1 -> 70 + 10 = 80



lurking variable that influence both x and y can create high correlation -> common response: both the explanatory and reponse variable are responding to some lurking variable. 
![[Pasted image 20260305221953.png|400]]


random: individual outcomes are uncertain but there is nonetheless a regular distribution of outcomes in a large number of repetitions. 
probability: number between 0 and 1 describes the proportions of times that outcome would occur in a very long series of repetitions. 

a personal probability of an outcome is a number between 0 and 1 that expresses an individual's judgement of how likely the outcome is. 

Probabiliyy: p [0,1], odds = p / 1 - p, p = odds / odds + 1 odds [0,inf)

<div class="stat-cheatsheet">

# STAT 100 FINAL CHEAT SHEET
---

## 1. Basic terms
- Population: 알고 싶은 전체 집단
- Sample: 실제로 조사한 일부
- Individual (case/unit): 데이터 한 개체
- Variable: 개체의 특성
  - categorical
  - numerical
- Parameter: population 수치
- Statistic: sample 수치
- Bias: 체계적으로 한쪽으로 빗나감
- Variability: 표본마다 자연스럽게 달라짐

---

## 2. Study types
### Sample survey
표본을 뽑아 조사

### Census
전체 population 조사

### Observational study
처치 안 하고 관찰만 함

### Experiment
처치를 가하고 결과 관찰

### Cause and effect
- observational study → association
- randomized experiment → stronger causation evidence

---

## 3. Sampling / bias
- Voluntary response
- Convenience sample
- Undercoverage
- Overcoverage
- Nonresponse
- Selection bias
- Response bias
- Questionnaire wording effects
- Interviewer bias
- Mode effects
- Probability sample
- SRS

시험 포인트:
- random sampling → population inference
- random assignment → causation evidence

---

## 4. Graphs
- Pie chart
- Bar graph
- Line graph
- Histogram
- Boxplot

볼 때:
- shape
- center
- spread
- outliers
- misleading scale

---

## 5. Distributions
- symmetric
- skewed right / left
- unimodal / bimodal
- outlier
- center
- spread
- mode

### Five-number summary
- min, Q1, median, Q3, max

### IQR
$$
IQR = Q3 - Q1
$$

### Mean vs Median
- mean: outlier에 민감
- median: resistant

---

## 6. Normal distribution
### Standard score
$$
z = \frac{x-\mu}{\sigma}
$$

실전:
$$
z = \frac{\text{observed} - \text{mean}}{\text{SD or SE}}
$$

### 68–95–99.7 rule
- 68% within 1 SD
- 95% within 2 SD
- 99.7% within 3 SD

---

## 7. Probability
$$
P(A^c)=1-P(A)
$$

$$
P(A\cup B)=P(A)+P(B)-P(A\cap B)
$$

if independent:
$$
P(A\cap B)=P(A)P(B)
$$

---

## 8. Odds
### odds in favor of A
$$
P(A):P(A^c)
$$

### odds against A
$$
P(A^c):P(A)
$$

If $p=P(A)$:

$$
\text{odds in favor of }A=\frac{p}{1-p}
$$

$$
\text{odds against }A=\frac{1-p}{p}
$$

If odds in favor = $a:b$:
$$
P(A)=\frac{a}{a+b}
$$

If odds against = $a:b$:
$$
P(A)=\frac{b}{a+b}
$$

---

## 9. Expected value
$$
E(X)=\sum xP(X=x)
$$

의미:
- 반복 시 장기 평균 결과

---

## 10. Scatterplot / correlation
- direction
- form
- strength
- outliers

$$
-1 \le r \le 1
$$

- correlation measures linear relationship
- correlation ≠ causation

---

## 11. Regression
### Least squares line
$$
\hat y = a+bx
$$

### Coefficient of determination
$$
r^2
$$

= y variability 중 x가 설명하는 비율

### Regression prediction by standardization
$$
z_x = \frac{x-\bar x}{s_x}
$$

$$
\hat z_y = rz_x
$$

$$
\hat y = \bar y + s_y\hat z_y
$$

Equivalent form:
$$
\hat y = \bar y + r\frac{s_y}{s_x}(x-\bar x)
$$

Also:
$$
b=r\frac{s_y}{s_x}, \quad a=\bar y - b\bar x
$$

### Reverse prediction
$$
z_y = \frac{y-\bar y}{s_y}
$$

$$
\hat z_x = rz_y
$$

$$
\hat x = \bar x + s_x\hat z_x
$$

$$
\hat x = \bar x + r\frac{s_x}{s_y}(y-\bar y)
$$

Note:
- predicted standardized score = $r \times$ original standardized score
- prediction is usually less extreme
- regression to the mean

---

## 12. Sampling distributions
- statistic varies from sample to sample
- sampling distribution = distribution of a statistic
- standard error = SD of that distribution

### Sample proportion
$$
\hat p = \frac{x}{n}
$$

### Sample mean
$$
\bar x
$$

---

## 13. Confidence intervals
General form:
$$
\text{estimate} \pm (\text{critical value})(\text{SE})
$$

### Proportion CI
$$
\hat p \pm z^*\sqrt{\frac{\hat p(1-\hat p)}{n}}
$$

### Mean CI
$$
\bar x \pm z^*\frac{s}{\sqrt n}
$$

### Common critical values
- 90%: 1.645
- 95%: 1.96
- 99%: 2.576

### Margin of error
$$
ME = z^* \times SE
$$

Sentence:
> I am __% confident that the true population [mean/proportion] of ___ is between ___ and ___.

---

## 14. Hypothesis testing
### Hypotheses
$$
H_0:p=p_0 \quad \text{or} \quad H_0:\mu=\mu_0
$$

$$
H_a:p>p_0,\; p<p_0,\; p\ne p_0
$$

$$
H_a:\mu>\mu_0,\; \mu<\mu_0,\; \mu\ne \mu_0
$$

### Test statistic
For proportion:
$$
z=\frac{\hat p-p_0}{\sqrt{\frac{p_0(1-p_0)}{n}}}
$$

For mean:
$$
z=\frac{\bar x-\mu_0}{s/\sqrt n}
$$

### Important
$$
SE(\hat p)_{CI}=\sqrt{\frac{\hat p(1-\hat p)}{n}}
$$

$$
SE(\hat p)_{test}=\sqrt{\frac{p_0(1-p_0)}{n}}
$$

$$
SE(\bar x)=\frac{s}{\sqrt n}
$$

### P-value rules
- right-tailed → area right
- left-tailed → area left
- two-sided → 2 × smaller tail area

### Conclusion template
> There is [weak / some / strong / very strong] evidence that the true [mean / proportion / probability] of ___ is [greater than / less than / different from] ___.

---

## 15. P-value interpretation
Correct:
- assuming $H_0$ true, probability of a result at least as extreme as observed

Incorrect:
- P-value = probability $H_0$ is true

---

## 16. Statistically significant ≠ practically important
- small P-value does not automatically mean big real-world effect
- large n can make tiny differences significant

---

## 17. Ultra-short formula box
$$
\hat p=\frac{x}{n}
$$

$$
IQR=Q3-Q1
$$

$$
z=\frac{x-\mu}{\sigma}
$$

$$
P(A^c)=1-P(A)
$$

$$
P(A\cup B)=P(A)+P(B)-P(A\cap B)
$$

$$
P(A\cap B)=P(A)P(B) \text{ if independent}
$$

$$
E(X)=\sum xP(X=x)
$$

$$
\hat y=a+bx
$$

$$
r^2=\text{proportion of variability explained}
$$

</div>
