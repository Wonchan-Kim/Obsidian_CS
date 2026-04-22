

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

## CI example for a proportion

Given \(x=34,\ n=400\),

$$
\hat{p}=\frac{34}{400}=0.085
$$

$$
SE=\sqrt{\frac{\hat{p}(1-\hat{p})}{n}}
=\sqrt{\frac{0.085(0.915)}{400}}
\approx 0.0139
$$

For 96.5% confidence:

$$
z^*\approx 2.1
$$

$$
CI=\hat{p}\pm z^*SE
=0.085\pm 2.1(0.0139)
$$

$$
(0.0557,\ 0.1143)
$$

Interpretation:  
I am 96.5% confident that the true probability is between 0.0557 and 0.1143.
### CI for proportion checklist
1. compute \(\hat p=x/n\)  
2. compute \(SE=\sqrt{\hat p(1-\hat p)/n}\)  
3. find \(z^*\) from confidence level  
4. do \(\hat p \pm z^*SE\)  
5. write sentence in context


---

# Past final examples / patterns

## 1. Cluster sample vs stratified sample
- divide into groups, then random sample from **each** group  
  → **stratified random sampling**
- randomly choose some whole classes / groups, then survey everyone there  
  → **cluster sampling**

Quick memory:
- stratified = sample from **every** group
- cluster = choose **some** groups

---

## 2. Observational study + probability sample
If people are randomly selected but **no treatment is imposed**, then this is

- an **observational study**
- based on a **probability sample**

---

## 3. Bigger sample size does what?
Increasing the size of a probability sample:

- **decreases variability**
- does **not** automatically reduce bias

Also, rough 95% margin of error for a proportion:
$$
MOE \approx \frac{1}{\sqrt{n}}
$$

---

## 4. Confounding / lurking variable examples
Always ask whether a third variable explains the association.

Examples:
- coffee and lung cancer → possible confounder: **smoking**
- tortilla chip sales and TV sales → possible confounder: **store size**
- women not working during pregnancy and healthier babies → possible confounder: **income**
- CO\(_2\) emissions and life expectancy → possible confounder: **national income / wealth**

---

## 5. Regression by standardization example
Given:
- correlation \(r=0.77\)
- explanatory z-score \(z_x=2.4\)

Predicted response z-score:
$$
\hat z_y = r z_x = 0.77(2.4)=1.848
$$

So the predicted standard score is about
$$
1.85
$$

Rule:
$$
\hat z_y = r z_x
$$

---

## 6. Slope of a regression line example
Given:
- \(r=0.77\)
- \(s_y=30.39\)
- \(s_x=22.26\)

The slope is
$$
b=r\frac{s_y}{s_x}
=0.77\cdot\frac{30.39}{22.26}
\approx 1.05
$$

Rule:
$$
b=r\frac{s_y}{s_x}
$$

---

## 7. Normal approximation from one cutoff
Suppose adult heights have mean \(166\) cm and SD \(10\) cm.  
Find the fraction under \(180\) cm.

Convert to z-score:
$$
z=\frac{180-166}{10}=1.4
$$

From the normal table:
$$
P(Z<1.4)\approx 0.9192
$$

So about
$$
91.9\%
$$
are under 180 cm.

Pattern:
- below one cutoff → convert to z, use left-tail area

---

## 8. Normal approximation between two cutoffs
Suppose BMI has mean \(22.02\) and SD \(3.132\).  
Find the fraction between 18 and 25.

Convert both cutoffs:
$$
z_{25}=\frac{25-22.02}{3.132}\approx 0.951
$$

$$
z_{18}=\frac{18-22.02}{3.132}\approx -1.28
$$

Use table and subtract:
$$
P(18<X<25)=P(Z<0.951)-P(Z<-1.28)
$$

Approximate answer:
about \(70\%\) to \(74\%\), depending on table rounding.

Pattern:
- between two cutoffs → convert both, subtract left-tail areas

---

## 9. Percentile from z-score example
A student has weight z-score
$$
z=1.646
$$

From the normal table, this is about the
$$
95\text{th percentile}
$$

Pattern:
- convert value to z-score
- use normal table to get percentile

---

## 10. Confidence interval for a proportion example
Suppose \(n=400\) trials give \(x=34\) successes.

Estimate:
$$
\hat p=\frac{34}{400}=0.085
$$

Estimated standard error:
$$
SE=\sqrt{\frac{\hat p(1-\hat p)}{n}}
=\sqrt{\frac{0.085(0.915)}{400}}
\approx 0.0139
$$

For 96.5% confidence:
$$
z^*\approx 2.1
$$

Confidence interval:
$$
\hat p \pm z^*SE
=0.085\pm 2.1(0.0139)
$$

$$
(0.0557,\ 0.1143)
$$

Interpretation:
I am 96.5% confident that the true probability is between 0.0557 and 0.1143.

Pattern:
1. compute \(\hat p=x/n\)
2. compute \(SE=\sqrt{\hat p(1-\hat p)/n}\)
3. find \(z^*\)
4. do \(\hat p\pm z^*SE\)
5. write sentence in context

---

## 11. Hypothesis test for a proportion example
If a deck is fair, the chance of a Heart is
$$
p_0=\frac14=0.25
$$

Suppose 1600 draws give 450 Hearts.

Hypotheses:
$$
H_0:p=0.25
$$

$$
H_a:p\ne 0.25
$$

Estimate:
$$
\hat p=\frac{450}{1600}=0.28125
$$

Standard error under \(H_0\):
$$
SE=\sqrt{\frac{0.25(0.75)}{1600}}=0.0108
$$

Test statistic:
$$
z=\frac{0.28125-0.25}{0.0108}\approx 2.89
$$

Two-sided P-value:
one tail at \(z\approx 2.9\) is about \(0.19\%\), so
$$
P\text{-value}\approx 2(0.19\%)=0.38\%
$$

Conclusion:
The P-value is very small, so there is very strong evidence that the probability is not \(1/4\).

Pattern:
- for tests of a proportion, use
$$
SE=\sqrt{\frac{p_0(1-p_0)}{n}}
$$
not \(\hat p\)

---

## 12. Confidence interval for a mean example
Suppose:
- \(n=1174\)
- \(\bar x=3386.7\)
- \(s=519.6\)

Standard error:
$$
SE=\frac{s}{\sqrt n}=\frac{519.6}{\sqrt{1174}}\approx 15.16
$$

For confidence level \(0.8064\), left-tail area is
$$
0.8064+\frac{1-0.8064}{2}=0.9032
$$

So
$$
z^*\approx 1.30
$$

Interval:
$$
\bar x \pm z^*SE
=3386.7\pm 1.3(15.16)
$$

$$
(3366.98,\ 3406.41)
$$

Interpretation:
I am 80.64% confident that the true mean is between 3366.98 and 3406.41.

---

## 13. Odds against example
If odds against an event are \(25:5\), then

$$
P(\text{event})=\frac{5}{25+5}=\frac16\approx 0.1667
$$

Rule:
If odds against are \(a:b\), then
$$
P(\text{event})=\frac{b}{a+b}
$$

---

## 14. Mean / median / mode in a skewed density
For a **left-skewed** density:
$$
\text{mean} < \text{median} < \text{mode}
$$

For a **right-skewed** density:
$$
\text{mode} < \text{median} < \text{mean}
$$

Reason:
the long tail pulls the mean farther into the tail.

---

## 15. Range restriction lowers correlation
If you cut off one side of a scatterplot and reduce the spread in the \(x\)-direction, the correlation \(r\) usually gets **smaller**.

Example pattern:
- select only students above some SAT cutoff
- then correlation between SAT and GPA tends to be **lower**

---

## 16. Regression to the mean
If first-year GPA and second-year GPA have positive correlation, students who are extremely high in year 1 are predicted to still be above average in year 2, but usually **less far above average**.

Why?
$$
\hat z = rz
$$
and usually
$$
|r|<1
$$

So extreme observations are predicted to be less extreme next time.

---

# Extra notes from class solution sets

## 1. Population vs population size
- **Population** = the actual group of people/objects we want information about
- **Population size** = the number of people/objects in that population

Example:
- population = students registered in STAT 100
- population size = 270

### Common mistake
- saying “the population is 270” is wrong
- 270 is the **population size**, not the population itself

---

## 2. Target sample vs respondents
- **Target sample** = the set of people you tried to survey
- **Respondents** = the people who actually replied

Example:
- if you tried to survey everyone in the class, the **target sample** is the whole class
- the 131 people who answered are just the **respondents**

### Common mistake
- calling the respondents “the sample” when the question is asking for the **desired / target sample**

---

## 3. Census can still fail
A survey can still be a **census attempt** even if not everyone responds, as long as you tried to survey the whole population.

### Key idea
- census = attempt to survey the entire population
- nonresponse can make it a **failed / incomplete census**, but it was still a census attempt

---

## 4. Frame, undercoverage, overcoverage
- **Frame** = the list or method used to identify who can be sampled
- **Undercoverage** = some people in the population are missing from the frame
- **Overcoverage** = the frame includes people who are not in the population

### Important point
You can have **both** undercoverage and overcoverage at the same time.

Example:
- Jan 5 class list used later
- students who dropped are still on the list → **overcoverage**
- students who joined later are missing → **undercoverage**

### Common mistake
- saying they “cancel out”
- they do **not** cancel out because they involve different people

---

## 5. Response bias can have a direction
If people systematically misreport answers, the estimate can be biased **up** or **down**.

Example:
- people may underreport weight
- heavier people may skip the weight question
- both effects make the estimated average weight **too low**

### Useful phrasing
- “biased down” = estimate tends to be too small
- “biased up” = estimate tends to be too large

---

## 6. Write estimates as estimates
Better wording:
- “We estimate the percentage to be 61%”

Avoid:
- “61% of Canadian adults think ...”

### Why
A sample result is an **estimate**, not exact truth about the population.

---

## 7. Quick 95% CI method from early course
For a proportion at 95% confidence:

$$
MOE \approx \frac{1}{\sqrt{n}}
$$

and

$$
\text{CI} \approx \hat p \pm MOE
$$

Example:
- \(n=1000\)
- \(\hat p=0.61\)

$$
MOE \approx \frac{1}{\sqrt{1000}} \approx 0.0316 \approx 3.2\%
$$

So the CI is about

$$
61\% \pm 3.2\% = (57.8\%, 64.2\%)
$$

---

## 8. Change + confidence interval for a difference
If estimate of change is \(-1800\) and the standard error of the change is \(5700\), then for a rough 95% CI:

$$
MOE \approx 2 \times SE = 2(5700)=11400
$$

So

$$
-1800 \pm 11400 = (-13200,\ 9600)
$$

### Interpretation
We are 95% confident that the true change is between a **decrease of 13,200** and an **increase of 9,600**.

### Useful note
- for these problems, the sentence matters
- just writing numbers loses marks

---

## 9. Experiment vs observational study: definition
A study is an **experiment** if the researcher **manipulates at least one explanatory variable**.

Even if the design is bad, it is still an experiment if the treatment variable was controlled.

Example:
- instructor changes exam time of day
- this is an **experiment**, though poorly designed

### Common mistake
- calling it observational just because it has flaws

---

## 10. Confounding variable: how to explain it
To explain confounding well, you must say how the confounder is related to:

1. the explanatory / treatment variable  
2. the response variable

Example:
- treatment variable = time of day of exam
- response variable = exam score
- possible confounder = exam difficulty

Why?
- harder exams lower scores
- if evening exam was harder, then time of day is tangled with difficulty

### Key exam tip
Do not just name a confounder.  
Explain how it connects to **both** variables.

---

## 11. Correlation does not imply causation: better criticism
If someone says:
- entrance test score is positively correlated with final GPA
- therefore test prep will improve final GPA

A better criticism is:
- correlation may be due to lurking variables such as **ability**, **study habits**, or **background preparation**
- improving score on the exam does not necessarily improve the underlying causes of success

### Stronger answer
Mention specific lurking variables, not just “correlation is not causation.”

---

## 12. Histogram bin counting detail
If histogram bars include the **lower limit** but not the **upper limit**, then values exactly on the boundary go into the bar on the **right**, not the left.

Example:
- bin \(1{:}59.0\) to \(2{:}00.0\): includes 1:59.4, but not 2:00.0
- bin \(2{:}00.0\) to \(2{:}01.0\): includes 2:00.0, 2:00.2, 2:00.4, ...

### Exam tip
Always check whether endpoints are included or excluded.

---

## 13. Misleading graph clue
A graph can be misleading if different series use **different y-axis scales**.

### Why this matters
Two lines may look comparable even when one is actually far larger than the other.

### Exam phrase
- “The graph is misleading because it uses different vertical scales.”

---

## 14. Quartiles from a normal distribution
To find quartiles of a Normal distribution:
- Q1 corresponds to about the **25th percentile**
- median corresponds to the **50th percentile**
- Q3 corresponds to about the **75th percentile**

Using the normal table:
- Q1 is about \(z=-0.7\)
- median is \(z=0\)
- Q3 is about \(z=0.7\)

Then convert back:

$$
x = \mu + z\sigma
$$

Example with mean \(68\) and SD \(2.5\):
$$
Q1 \approx 68-0.7(2.5)=66.25
$$

$$
Q2 = 68
$$

$$
Q3 \approx 68+0.7(2.5)=69.75
$$

---

## 15. Truncating data changes mean and SD
If you remove all observations above some cutoff:

- the **mean goes down**
- the **SD goes down**

### Why
- all remaining values are below the old mean, so the center shifts down
- the data are less spread out than before

Example:
- if you remove all sons taller than 69 inches, both mean and SD become smaller

---

## 16. Reverse prediction reminder
To predict \(x\) from \(y\), you do **not** use the same slope as predicting \(y\) from \(x\).

If predicting \(x\) from \(y\):

$$
b = r\frac{s_x}{s_y}
$$

Example:
- \(r=0.5\)
- \(s_x=12\)
- \(s_y=0.4\)

$$
b = 0.5 \cdot \frac{12}{0.4}=15
$$

### Common mistake
Using
$$
r\frac{s_y}{s_x}
$$
for the reverse direction.

---

## 17. Regression \(R^2\) as percent explained
If
$$
r=0.5
$$
then
$$
R^2 = r^2 = 0.25
$$

So the percentage of variation explained is:

$$
25\%
$$

### Exam wording
- “25% of the variation in \(y\) is explained by the regression on \(x\).”

---

## 18. Regression prediction example
If:
- mean entrance score = 72
- SD entrance score = 12
- mean GPA = 2.8
- SD GPA = 0.4
- \(r=0.5\)
- student score = 90

Then:

### Step 1. Convert to z-score
$$
z_x=\frac{90-72}{12}=1.5
$$

### Step 2. Predict response z-score
$$
\hat z_y = r z_x = 0.5(1.5)=0.75
$$

### Step 3. Convert back
$$
\hat y = 2.8 + 0.75(0.4)=3.1
$$

So the predicted GPA is

$$
3.1
$$

---

## 19. Undoing standardization example
If:
- explanatory mean = 19.4
- explanatory SD = 4.3
- response mean = 25
- response SD = 6.8
- \(r=0.62\)
- explanatory score = 27

### Step 1. Standardize explanatory score
$$
z_x=\frac{27-19.4}{4.3}=1.77
$$

### Step 2. Predict response z-score
$$
\hat z_y = 0.62(1.77)\approx 1.10
$$

### Step 3. Convert back
$$
\hat y = 25 + 1.10(6.8)\approx 32.45
$$

---

## 20. Reverse prediction example
Using the same setup, suppose a student has final score \(32.45\).

### Step 1. Standardize final score
$$
z_y=\frac{32.45-25}{6.8}\approx 1.096
$$

### Step 2. Predict explanatory z-score
$$
\hat z_x = 0.62(1.096)\approx 0.679
$$

### Step 3. Convert back
$$
\hat x = 19.4 + 4.3(0.679)\approx 22.3
$$

### Common mistake
Do **not** just go back to the original 27.

---

</div>