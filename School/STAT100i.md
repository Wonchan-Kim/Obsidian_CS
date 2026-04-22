

### Conclusion template
> There is [weak / some / strong / very strong] evidence that the true [mean / proportion / probability] of ___ is [greater than / less than / different from] ___.

---
## Sampling methods: how to distinguish them

### Simple random sample (SRS)
Every sample of size \(n\) has an equal chance to be chosen.

Use when:
- population is treated as one whole group
- we randomly choose individuals directly from the full population

Key clue:
- “select \(n\) individuals at random from the whole population”

---

### Stratified random sampling
First divide the population into groups called **strata**, then take a random sample from **each stratum**.

Purpose:
- make sure each important subgroup is represented
- can reduce variability if members within each stratum are similar

Key clue:
- “divide into groups, then randomly sample from each group”

Example:
- divide students into first-year and others, then randomly sample from each group  
→ **stratified random sampling**

Important:
- strata are usually groups that differ from each other in an important way
- sample is taken from **every** stratum

---

### Cluster sampling
First divide population into clusters, then randomly choose **some clusters**, and sample everyone (or many people) in those chosen clusters.

Purpose:
- often cheaper / easier when population is naturally grouped

Key clue:
- “randomly choose some whole groups, then survey all individuals in those groups”

Example:
- randomly choose 3 classrooms and survey every student in those classrooms  
→ **cluster sampling**

Important difference from stratified:
- **stratified**: sample from every group
- **cluster**: choose only some groups

---

### Multi-stage sampling
Sampling done in several steps.

Key clue:
- “first choose larger groups, then choose smaller groups within them”

Example:
- choose schools, then classes within schools, then students within classes

Important:
- multi-stage sampling often includes cluster sampling as one stage

---

### Voluntary response sampling
People choose themselves whether to participate.

Key clue:
- “call in”, “vote online”, “respond if interested”

Problem:
- usually strongly biased because people with strong opinions are more likely to respond

---

### Quick comparison table

- **SRS**: sample individuals directly from whole population
- **Stratified**: divide into groups, sample from each group
- **Cluster**: divide into groups, randomly choose some groups
- **Multi-stage**: sampling in several random stages
- **Voluntary response**: people choose themselves

---

### Most common confusion: stratified vs cluster

#### Stratified sampling
- use **all strata**
- randomly sample **within each stratum**

#### Cluster sampling
- randomly choose **some clusters**
- then sample everyone or many people in chosen clusters

### Memory trick
- **stratified = from every type**
- **cluster = pick some groups**
</div>

<div class="stat-cheatsheet">

# STAT 100 FINAL CHEAT SHEET
## detailed version with definitions + confusing points

---

# 0. 먼저 기억할 시험 방향

- 시험 범위는 거의 전체지만, **midterm 이후 내용에 더 비중**
- 제외: **Ch. 7, 16, 19, 23, 24**
- 핵심:
  - sample surveys / observational studies / experiments
  - graphs / summary statistics
  - normal distribution / z-score / percentile
  - scatterplot / correlation / regression
  - probability / odds / expected value
  - confidence intervals
  - hypothesis tests

---

# 1. Population / Sample / Parameter / Statistic

## Population
우리가 알고 싶은 **전체 대상 집단**

예:
- SFU 학생 전체
- 캐나다 유권자 전체
- 특정 공장에서 생산한 제품 전체

## Sample
population 중에서 **실제로 관찰한 일부**

예:
- 학생 200명 조사
- 유권자 1000명 여론조사

## Parameter
**population을 설명하는 수치**
- 보통 고정되어 있지만 모름

예:
- 전체 학생의 평균 키
- 전체 유권자 중 찬성 비율

## Statistic
**sample을 설명하는 수치**
- sample마다 달라짐
- parameter를 추정하는 데 사용

예:
- 표본평균 \(\bar x\)
- 표본비율 \(\hat p\)

## 헷갈리는 포인트
- **parameter는 population**
- **statistic은 sample**
- 시험에서 “이 숫자가 sample 기반인지, population 전체 값인지” 구분해야 함

---

# 2. Individuals / Variables

## Individual / case / unit
데이터 한 줄에 해당하는 **한 개체**

예:
- 한 명의 학생
- 한 가구
- 한 실험 참가자
- 한 제품

## Variable
각 individual에 대해 측정하는 특성

예:
- 키
- 몸무게
- 성별
- 전공
- 시험점수

## Categorical variable
값이 **범주/그룹 이름**인 변수

예:
- 성별
- 혈액형
- 전공
- 찬성/반대

## Numerical variable
값이 **숫자**이고 산술적 의미가 있는 변수

예:
- 키
- 몸무게
- 소득
- 점수

## 헷갈리는 포인트
- 숫자로 적혀 있어도 진짜 수치형이 아닐 수 있음  
  예: 학번, 우편번호 → 숫자처럼 보여도 **식별용**, 수치형 아님

---

# 3. Bias / Variability / Sampling error

## Bias
표본조사를 반복했을 때 statistic이 true parameter를 **한쪽 방향으로 계속 벗어나는 경향**

즉,
- systematic overestimate
- 또는 systematic underestimate

예:
- 전화 없는 사람을 빼고 조사하면 특정 의견이 과대표집될 수 있음

## Variability
표본을 다시 뽑을 때 statistic이 **얼마나 흔들리는가**

즉,
- sample이 바뀌면 결과도 조금씩 바뀜
- 이건 정상적이고 unavoidable한 random variation

## Sampling error
sample만 보고 population을 추정하기 때문에 생기는 오차

## 핵심 차이
- **bias** = 한쪽으로 치우친 구조적 문제
- **variability** = random fluctuation
- 큰 sample size는 **variability를 줄여주지만 bias를 없애지는 못함**

## 시험 함정
- “sample size를 늘리면 bias가 사라진다” → 틀림
- “sample size를 늘리면 variability가 줄어든다” → 맞음

---

# 4. Sample survey / Census / Observational study / Experiment

## Sample survey
population에서 sample을 뽑아 조사하는 것

## Census
population 전체를 조사하는 것

## Observational study
처치를 하지 않고 그냥 관찰만 하는 연구

예:
- 흡연 여부와 암 발생률 관찰

## Experiment
연구자가 treatment를 **직접 부여**하고 response를 비교하는 연구

예:
- 어떤 약을 먹게 하고 효과 비교

## 가장 중요한 구분
- **observational study**: association는 볼 수 있음
- **experiment**: causation 주장에 더 강함

## 시험에서 자주 묻는 것
“이 연구로 인과관계를 말할 수 있나?”

답 기준:
- random assignment 있으면 causation 근거 강함
- observational study면 보통 causation 단정 못 함

---

# 5. Explanatory / Response / Confounding / Lurking variable

## Explanatory variable
결과를 설명하거나 예측하는 변수  
= independent variable

## Response variable
결과로 나오는 변수  
= dependent variable

예:
- 공부시간 \(x\), 시험점수 \(y\)
- \(x\): explanatory
- \(y\): response

## Lurking variable
분석에 직접 포함되지 않았지만 결과에 영향을 줄 수 있는 숨은 변수

예:
- 공부시간과 점수 사이에서 선행지식

## Confounding
두 변수의 효과가 섞여서 무엇이 진짜 원인인지 분리하기 어려운 상태

예:
- 운동량과 건강의 관계를 보는데 나이가 같이 작용함

## common response
\(x\)와 \(y\)가 서로 원인관계가 아니라, 둘 다 **같은 lurking variable**에 의해 움직일 수 있음

## 시험 포인트
- high correlation ≠ causation
- lurking variable / confounding 가능성 항상 생각

---

# 6. Sampling methods and bad samples

## SRS (simple random sample)
크기 \(n\)인 모든 sample이 똑같은 chance로 선택되는 표본

## Probability sample
random mechanism이 분명한 표본

## Convenience sample
구하기 쉬운 사람만 뽑은 표본  
→ 대표성 약함

## Voluntary response sample
응답하고 싶은 사람이 스스로 참여  
→ 강한 의견 가진 사람이 몰리기 쉬움

## Undercoverage
population의 일부 집단이 sample frame에서 빠짐

## Overcoverage
특정 집단이 과도하게 포함됨

## Nonresponse
뽑혔는데 응답 안 함

## Response bias
사람들이 진실하게 답하지 않거나 질문 방식 때문에 답이 왜곡됨

## Questionnaire wording effect
질문 문구 때문에 응답이 달라짐

## Interviewer bias
면접자/조사자가 응답에 영향을 줌

## Mode effects
전화, 온라인, 대면 등 조사 방식에 따라 응답이 달라짐

## 시험용 핵심 구분
- **random sampling** → population generalization
- **random assignment** → causation

---

# 7. Margin of error / Confidence level

## Margin of error
estimate가 true parameter에서 얼마나 벗어날 수 있는지에 대한 폭

예:
- \(52\% \pm 4\%\)

## Confidence level
이 방법을 같은 방식으로 아주 많이 반복했을 때,  
그 interval이 true parameter를 포함하는 비율

예:
- 95% confidence

## 올바른 해석
“이 방법으로 만든 interval들의 약 95%가 true parameter를 포함한다.”

## 잘못된 해석
“이 particular interval이 true parameter를 포함할 확률이 95%다.”  
→ 엄밀히 말하면 틀림  
parameter는 random이 아니라 fixed value로 봄

## sample size와의 관계
- bigger \(n\) → smaller margin of error
- confidence level 더 높이면 → interval wider

---

# 8. Graphs

## Pie chart
전체 중 각 범주의 비율을 원형으로 표현  
→ 전체가 100%인 상황에 적합

## Bar graph
범주별 크기 비교

## Line graph
시간 흐름에 따른 변화

## Histogram
수치형 변수의 분포를 구간별 막대로 표현

## Boxplot
five-number summary를 시각화

## 그래프 볼 때 항상 체크
1. shape
2. center
3. spread
4. outliers
5. scale 조작 여부

## 시험 함정
- bar graph와 histogram 헷갈리지 말기  
  - bar graph: categorical
  - histogram: numerical distribution

---

# 9. Distribution shape / center / spread

## Shape
- symmetric
- skewed right
- skewed left
- unimodal
- bimodal

## Center
- mean
- median

## Spread
- range
- IQR
- standard deviation

## Outlier
다른 값들에서 멀리 떨어진 unusually extreme observation

---

# 10. Mean / Median / Mode

## Mean
모든 값을 더해서 개수로 나눈 평균

## Median
정렬했을 때 한가운데 값

## Mode
가장 자주 나타나는 값 / peak

## Mean vs Median
- mean: 모든 값 반영, but outlier와 skewness에 민감
- median: resistant, skewed data에서 더 좋음

## 시험 포인트
- roughly symmetric, no outliers → mean and SD
- skewed or outliers → median and IQR / five-number summary

---

# 11. Median and quartiles: odd/even rules

데이터는 반드시 **작은 것부터 정렬**하고 시작

## Median

### If \(n\) is odd
middle observation

position:
$$
\frac{n+1}{2}
$$

### If \(n\) is even
middle two values의 average

positions:
$$
\frac{n}{2},\ \frac{n}{2}+1
$$

---

## Quartiles

### Rule used in intro stats
- 전체 median 구한 뒤
- **odd \(n\)**: overall median 제외하고 아래/위 half의 median을 각각 \(Q1, Q3\)
- **even \(n\)**: data를 정확히 둘로 나눠 각 half의 median을 \(Q1, Q3\)

## Example: odd
Data:
1, 3, 5, 8, 9, 12, 14

- median = 8
- lower half = 1, 3, 5 → \(Q1=3\)
- upper half = 9, 12, 14 → \(Q3=12\)

## Example: even
Data:
1, 3, 5, 8, 9, 12, 14, 20

- median:
$$
\frac{8+9}{2}=8.5
$$

- lower half = 1, 3, 5, 8
$$
Q1=\frac{3+5}{2}=4
$$

- upper half = 9, 12, 14, 20
$$
Q3=\frac{12+14}{2}=13
$$

---

# 12. Five-number summary / IQR / SD

## Five-number summary
- minimum
- Q1
- median
- Q3
- maximum

## IQR
$$
IQR = Q3 - Q1
$$

middle 50%의 spread

## Standard deviation (SD)
mean으로부터의 typical distance

## SD facts
- deviations from the mean sum to 0
- SD is not just average deviation; it squares deviations first
- large SD → values more spread out

## Which is better?
- skewed or outliers → five-number summary, IQR
- symmetric → mean, SD

---

# 13. Normal distribution

## Normal curve
- symmetric bell shape
- mean = median = center
- completely determined by **mean** and **SD**
- mean controls center
- SD controls spread

## 68–95–99.7 rule
- about 68% within 1 SD
- about 95% within 2 SD
- about 99.7% within 3 SD

## 시험 포인트
Normal이면 대칭이고 평균 주변으로 퍼짐을 SD가 결정

---

# 14. z-score / standard score / percentile

## z-score
관측값이 평균에서 몇 SD 떨어져 있는지

$$
z = \frac{x-\mu}{\sigma}
$$

실전에서는
$$
z = \frac{\text{observed} - \text{mean}}{\text{SD or SE}}
$$

## Interpretation
- \(z>0\): mean above
- \(z<0\): mean below
- \(|z|\) 클수록 평균에서 멀리 떨어짐

## Percentile
\(c\)th percentile = 전체 중 \(c\%\)가 그 값 아래에 있는 값

- median = 50th percentile
- Q1 = 25th percentile
- Q3 = 75th percentile

## Normal-specific facts
Normal distribution에서는 z-score와 percentile이 바로 연결됨

예:
- 1 SD above mean → 약 84th percentile
- 1 SD below mean → 약 16th percentile

## 헷갈리는 포인트
- percentile은 “below” 기준
- z-score는 “몇 SD 떨어졌는가”
- normal distribution이 아닐 때는 z-score와 percentile 연결이 그렇게 깔끔하지 않을 수 있음

---

# 15. Probability basics

## Random
개별 결과는 uncertain하지만, long run에서는 pattern이 regular

## Probability
아주 많이 반복했을 때 어떤 결과가 나오는 long-run proportion

## Personal probability
개인의 주관적 판단으로 보는 probability

## Event
outcomes의 집합

## Probability model
chance behavior를 수학적으로 표현한 모델

---

# 16. Probability rules

$$
P(A^c)=1-P(A)
$$

## Union rule
$$
P(A\cup B)=P(A)+P(B)-P(A\cap B)
$$

## If independent
$$
P(A\cap B)=P(A)P(B)
$$

## 헷갈리는 포인트
- \(A\cup B\): A or B or both
- \(A\cap B\): A and B
- independent라고 말 안 했으면 곱하면 안 됨

---

# 17. Odds

## odds in favor of \(A\)
일어날 가능성 : 안 일어날 가능성

$$
P(A):P(A^c)
$$

If \(p=P(A)\):
$$
\text{odds in favor of }A=\frac{p}{1-p}
$$

## odds against \(A\)
안 일어날 가능성 : 일어날 가능성

$$
P(A^c):P(A)
$$

$$
\text{odds against }A=\frac{1-p}{p}
$$

## convert odds to probability
If odds in favor = \(a:b\),
$$
P(A)=\frac{a}{a+b}
$$

If odds against = \(a:b\),
$$
P(A)=\frac{b}{a+b}
$$

## 헷갈리는 포인트
- odds ≠ probability
- probability는 0~1
- odds는 0부터 무한대 가능

---

# 18. Expected value

$$
E(X)=\sum xP(X=x)
$$

## Meaning
같은 상황을 아주 많이 반복했을 때의 long-run average outcome

## 헷갈리는 포인트
- expected value가 실제 가능한 값이 아닐 수도 있음
- 예: 평균 자녀 수 2.3명은 가능

---

# 19. Scatterplot

두 quantitative variables를 같은 individuals에 대해 찍은 그래프

- \(x\): explanatory
- \(y\): response

## 볼 것
- direction
- form
- strength
- outliers

## Positive association
한 변수가 평균보다 높으면 다른 변수도 높은 경향

## Negative association
한 변수가 높을수록 다른 변수는 낮아지는 경향

---

# 20. Correlation \(r\)

$$
-1 \le r \le 1
$$

## What it measures
두 quantitative variables 사이의 **straight-line relationship의 direction과 strength**

## Interpretation
- \(r>0\): positive
- \(r<0\): negative
- \(|r|\) closer to 1 → stronger linear relationship
- \(|r|\) near 0 → weak linear relationship

## Important cautions
- correlation measures **linear** relationship only
- correlation ≠ causation
- outliers can strongly affect \(r\)
- steep slope does **not** mean strong correlation
- 단위 바꿔도 correlation은 안 바뀜

---

# 21. Regression line

If raw data are given, the sample correlation can be computed by standardizing both variables and averaging the products of the z-scores:  
  
$$  
r = \frac{1}{n-1}\sum \left(\frac{x_i-\bar{x}}{s_x}\right)\left(\frac{y_i-\bar{y}}{s_y}\right)  
$$  
  
### Meaning  
- measures the **direction and strength of a linear relationship**  
- based on how \(x\)-z-scores and \(y\)-z-scores move together  
- positive \(r\): above-average \(x\) tends to go with above-average \(y\)  
- negative \(r\): above-average \(x\) tends to go with below-average \(y\)
- 
## Least squares regression line
$$
\hat y = a+bx
$$

- \(b\): slope
- \(a\): intercept

## Interpretation of slope
\(x\)가 1 unit 증가할 때 predicted \(y\)가 얼마나 변하는가

## Interpretation of intercept
\(x=0\)일 때 predicted \(y\)

단, \(x=0\)이 data range 밖이면 intercept 해석이 무의미할 수 있음

## Least squares meaning
vertical residuals의 squared sum을 최소로 하는 선

---

# 22. Regression by standardization

## Step 1
\(x\)를 z-score로
$$
z_x=\frac{x-\bar x}{s_x}
$$

## Step 2
predicted \(y\) z-score
$$
\hat z_y = rz_x
$$

## Step 3
원래 단위로 복원
$$
\hat y = \bar y + s_y\hat z_y
$$

## Equivalent form
$$
\hat y = \bar y + r\frac{s_y}{s_x}(x-\bar x)
$$

또는
$$
b=r\frac{s_y}{s_x}, \qquad a=\bar y-b\bar x
$$

---

# 23. Reverse prediction

\(y\)로부터 \(x\) 예측할 때

$$
z_y=\frac{y-\bar y}{s_y}
$$

$$
\hat z_x = rz_y
$$

$$
\hat x = \bar x + s_x\hat z_x
$$

## 헷갈리는 포인트
- \(y\) on \(x\) regression과 \(x\) on \(y\) regression은 다름
- 그냥 식 뒤집는 게 아님

---

# 24. Residual / \(r^2\) / unexplained variability

## Residual
$$
\text{residual}=y-\hat y
$$

actual minus predicted

- positive residual: actual > predicted
- negative residual: actual < predicted

## Coefficient of determination
$$
r^2
$$

\(y\) variation 중 regression line이 설명하는 proportion

## unexplained variability
$$
1-r^2
$$

## Another formula
$$
r^2 = 1-\frac{\sum (y_i-\hat y_i)^2}{\sum (y_i-\bar y)^2}
$$

## Interpretation
- 큰 \(r^2\): prediction more useful
- 작은 \(r^2\): line 주변에 scatter 큼

---

# 25. Extrapolation / regression to the mean

## Extrapolation
관측된 \(x\) 범위 밖에서 prediction하는 것  
→ 위험함

## Regression to the mean
extreme한 observation은 예측할 때 평균 쪽으로 덜 extreme해짐

왜냐하면
$$
\hat z = rz
$$
이고 보통 \(|r|<1\)

## 시험 포인트
“아주 높았던 첫 시험 점수가 다음 시험 predicted score에서는 덜 extreme하게 나오는 현상”

---

# 26. Common response / lurking variable and causation

## Common response
\(x\)와 \(y\) 둘 다 같은 lurking variable의 영향을 받음

예:
- 아이스크림 판매와 익사 사고  
  둘 다 더운 날씨 영향

## 시험 포인트
상관관계가 있어도
- direct causation?
- reverse causation?
- lurking variable?
를 꼭 따져야 함

---

# 27. Sampling distributions / standard error

## Sampling distribution
어떤 statistic을 많은 random samples에서 계속 계산했을 때 그 statistic의 distribution

## Standard error
그 sampling distribution의 SD

### sample proportion
$$
\hat p = \frac{x}{n}
$$

### sample mean
$$
\bar x
$$

## 헷갈리는 포인트
- SD는 individual data의 spread
- SE는 statistic의 spread

---

# 28. Confidence intervals

## General form
$$
\text{estimate} \pm (\text{critical value})(\text{SE})
$$

## CI for proportion
$$
\hat p \pm z^*\sqrt{\frac{\hat p(1-\hat p)}{n}}
$$

## CI for mean
$$
\bar x \pm z^*\frac{s}{\sqrt n}
$$

## Common critical values
- 90%: \(1.645\)
- 95%: \(1.96\)
- 99%: \(2.576\)

## Margin of error
$$
ME = z^* \cdot SE
$$

## Correct conclusion sentence
“I am __% confident that the true population [mean/proportion] of ___ is between ___ and ___.”

## Important confusion point
confidence interval is for the **parameter**, not for individual observations

---

# 29. Hypothesis testing

## Null hypothesis
기준값, 변화 없음, 차이 없음

예:
$$
H_0:p=p_0
$$
or
$$
H_0:\mu=\mu_0
$$

## Alternative hypothesis
우리가 evidence를 찾고 싶은 방향

$$
H_a:p>p_0,\quad p<p_0,\quad p\ne p_0
$$

$$
H_a:\mu>\mu_0,\quad \mu<\mu_0,\quad \mu\ne \mu_0
$$

## Test statistic

### For proportion
$$
z=\frac{\hat p-p_0}{\sqrt{\frac{p_0(1-p_0)}{n}}}
$$

### For mean
$$
z=\frac{\bar x-\mu_0}{s/\sqrt n}
$$

## Very important distinction
For CI:
$$
SE(\hat p)_{CI}=\sqrt{\frac{\hat p(1-\hat p)}{n}}
$$

For test:
$$
SE(\hat p)_{test}=\sqrt{\frac{p_0(1-p_0)}{n}}
$$

## P-value
\(H_0\)가 true라고 가정했을 때, observed result 이상으로 extreme한 결과가 나올 확률

### Tail rules
- right-tailed → area to right
- left-tailed → area to left
- two-sided → \(2 \times\) smaller tail area

## Evidence language
- big P-value → little evidence against \(H_0\)
- small P-value → strong evidence against \(H_0\)

## Significance
- \(P<0.05\): statistically significant
- \(P<0.01\): very strong evidence / highly significant

## Correct conclusion template
“There is [weak / some / strong / very strong] evidence that the true [mean / proportion / probability] of ___ is [greater than / less than / different from] ___.”

---

# 30. P-value mistakes / significance vs importance

## Wrong interpretations
- “P-value is the probability that \(H_0\) is true.” → 틀림
- “small P-value proves the alternative is true.” → 틀림

## Statistical significance ≠ practical importance
- sample size 크면 tiny effect도 significant 가능
- effect가 statistically significant라도 practically important 아닐 수 있음

---

# 31. Ultra-short formula box

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
P(A\cap B)=P(A)P(B)\quad \text{if independent}
$$

$$
\text{odds in favor of }A=\frac{p}{1-p}
$$

$$
\text{odds against }A=\frac{1-p}{p}
$$

$$
P(A)=\frac{a}{a+b}\quad \text{if odds in favor}=a:b
$$

$$
P(A)=\frac{b}{a+b}\quad \text{if odds against}=a:b
$$

$$
E(X)=\sum xP(X=x)
$$

$$
\hat y=a+bx
$$

$$
b=r\frac{s_y}{s_x}
$$

$$
a=\bar y-b\bar x
$$

$$
\text{residual}=y-\hat y
$$

$$
r^2=\text{proportion explained}
$$

$$
1-r^2=\text{unexplained}
$$

$$
SE(\hat p)_{CI}=\sqrt{\frac{\hat p(1-\hat p)}{n}}
$$

$$
SE(\hat p)_{test}=\sqrt{\frac{p_0(1-p_0)}{n}}
$$

$$
SE(\bar x)=\frac{s}{\sqrt n}
$$

$$
CI_p:\ \hat p\pm z^*\sqrt{\frac{\hat p(1-\hat p)}{n}}
$$

$$
CI_\mu:\ \bar x\pm z^*\frac{s}{\sqrt n}
$$

$$
z=\frac{\hat p-p_0}{\sqrt{\frac{p_0(1-p_0)}{n}}}
$$

$$
z=\frac{\bar x-\mu_0}{s/\sqrt n}
$$

---

 Odds
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

# 32. Final check list

문제 보면 바로 체크:

1. sample survey / observational study / experiment?
2. population / sample / parameter / statistic?
3. variable categorical or numerical?
4. bias / nonresponse / undercoverage / lurking variable?
5. graph면 shape, center, spread, outlier?
6. normal / z-score / percentile 문제인가?
7. scatterplot이면 direction, form, strength, outlier?
8. regression이면 slope, intercept, residual, \(r^2\)?
9. CI인지 hypothesis test인지?
10. one-sided인지 two-sided인지?
11. 마지막 문장을 context에 맞게 썼나?

</div>