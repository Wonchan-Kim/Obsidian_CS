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


![[Pasted image 20260303145822.png|left|400]]
![[Pasted image 20260303150022.png|right|400]]
Normal distributions: symmetric, mean and median lie together at the peak in the center of the curve. 
Normal curve- completely described by giving its mean and its sd. The mean determines the center of the distribution. It is located at the center of symmetry of the curve. Standard deviation determines the shape of the curve. sd is the distance from the mean to the change of curvature points on either side. 

68-95-99.7 rule: mean +- 1/2/3 sd. ![[Pasted image 20260303150708.png]]

standard scores - ex) mean is 500, sd is 100. For score 600, can be expressed as the one sd above the mean. Observations expressed in sd above or below the mean of a distribution are called standard scores, often referred to as z-scores. 
z = x - u / sd

Exclusively for normal distribution, standard scores translate directly into percentiles. 
The cth percentile of a distribution is a value such that c percent of the observations lie below it and the rest lie above. 
The median of any distribution is the 50th percentile, and the quartiles are the 25th and 75th percentiles. In any normal distribution, the point one sd above the mean is the 84th percentile. (50 + 34)

Scatterplot: relationship between two quantitative variables measured on the same individuals. The values of one variable appear on the horizontal axis, and the values of the other variable appear on the vertical axis. Each individual in the data appears as the point in the plot fixed by the values of both variables for that individual.  X -> explanatory variable Y -> response variable

Positively associated: above average values of one tend to accompany above average values of the other and below average values tend to occur together. ![[Pasted image 20260303210622.png]]
correlation describes the direction and strength of a straight line relationship between two quantitive variables, usually written as r. 

regression line: straight line describes how a response variable y changes as an explanatory variable x changes. Predict y for a given value of x.  (y-y^ / sy = r * x - x^/sx)
y = ax +b with b = r*sy/sx and a = y_ - bx_
x^ = x_ + rsx (y-y_)/sy


least squares regression line of y on x is the line that makes the sum of the squares of the vertical distances of the data points from the line as small as possible. 

extrapolation: prediction outside the range

r^2: square of correlation, r^2 is the proportion of the variation in the values of y that is explained by the least squares of y on x. 
usefulness of the regression line for prediction depends on the strength of the association(correlation). 
ex) r = 0.994 -> r^2 = 0.998, so the variation along the line accounts 98.8 of all variation. The scatter of the points about the line accounts for only the remaining 1.2 %. Little leftover scatter says that prediction will be accurate. 

1-r^2 -> unexplained variability. residual = actual y - predicted y. 
sum of squared resuduals: sigma(y_ - y)^2
r^2 = 1 - sigma(yi - y^)^2 / sigma(yi-y_)^2
Sum of squared residuals = (1 − r 2) times sum of squared deviations from average son’s height.

regression to the mean -> ex two midterm avg 70, sd 10, r = 0.5, z predicted y = r*zx
first midterm 90 -> 2sd -> estimate 0.5** 2 = 1 -> 70 + 10 = 80



lurking variable that influence both x and y can create high correlation -> common response: both the explanatory and reponse variable are responding to some lurking variable. 
![[Pasted image 20260305221953.png]]

index = value * 100 / base
fixed market basket price index is an index number for the total cost of a fixed collection of goods and services. 
CPI is a fixed market basket price index with several hundred items that represent all consumer purchases. Holding the market basket fixed allows a legitimate comparison of prices because we compare the prices of the same items at each time. 
ex) CPI for October 2018 was 252.9 means that we must spend 252.9 to buy goos and services that cost 100 in the 1982 and 1984 base period. 
Tables headings - median household income. restating all incomes in dollars that will but as much as the dollar would buy in 2017. Term constant dollars and real income/terms. Meaning that all dollar amounts represent the same buying power even though they may describe different years. 

adjusting the buying power: dollars at time B = dollars at time A * CPI at time B / CPI at time A

CPI covers urban consumers-80%. Different households buy different things, so categorize the purchase of huge sample. So market basket is not actually fixed. CPI can't really measure the changes in the cost of living as the buying items change. 

random: individual outcomes are uncertain but there is nonetheless a regular distribution of outcomes in a large number of repetitions. 
probability: number between 0 and 1 describes the proportions of times that outcome would occur in a very long series of repetitions. 

a personal probability of an outcome is a number between 0 and 1 that expresses an individual's judgement of how likely the outcome is. 

Probabiliyy: p [0,1], odds = p / 1 - p, p = odds / odds + 1 odds [0,inf)

