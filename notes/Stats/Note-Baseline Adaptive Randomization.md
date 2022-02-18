# Baseline adaptive randomization

Steps to conduct baseline adaptive randomization

1. Assume there are $k$ baseline covariates chosen as the stratification factors, each of which would have several levels
2. At the beginning of the randomization, since there isn't any patients yet, simple randomization will be used for the first $m$ patients ($m$ = No. of treatment groups)
3. After Step 2, for every new incoming patient, calculate the measure of imbalance for assigning that patient to every possible treatment
4. Generate the probability of biased coin for treatment assignment based on the measure of imbalance from Step 3: group that leads to larger imbalance score should receive smaller probability of having a patient being assigned

**Measure of imbalance**

i. 
$$
B(t) =\sum_{f=1}^F w_f*\text{range}(X_{f_1}^t-X_{f_2}^t), t=1, 2, \cdots 
$$
this measure works only for two treatment groups

ii. max $\chi$-square statistic among all the stratification factors


## Sample code
```r
library(arsenal)
# N: total sample size for randomization
# treatment groups: LETTERS[1:K]
# baseline stratification factors (and levels within each factor)

# function to generate a sample for randomization
gen_sample = function(n = 200, prob_age = c(0.2, 0.3, 0.5), prob_sex = 0.5, prob_occ = 0.6, prob_site = 0.6){
	SEX = as.factor(rbinom(n, 1, prob = prob_sex) + 1)
	OCCLUTION = as.factor(rbinom(n, 1, prob = prob_occ) + 1)
	AGE = as.factor(sample(length(prob_age), n, prob_age, replace = TRUE))
	SITE = as.factor(rbinom(n, 1, prob = prob_site) + 1)

	data.frame(AGE = AGE, SEX = SEX, OCCLUTION = OCCLUTION, SITE = SITE)
}

# function to assign group for new patients based on biased coin probability calculated from all assigned pats
assign = function(trt_grp, prob){
	return(sample(trt_grp, 1, replace = FALSE, prob))
}

# Baseline Adaptive Randomization using biased coin method
BAR = function(df, n_trt = 2, prob_bc = 2/3){
	# df: a sample to be randomized
	# n_trt: number of treatment arms 
	# prob_bc: biased coin probability when there is imbalance between two treatment arms
	trt_grp = LETTERS[1:n_trt]
	df$trt = NA
	i = 1
	max_x_level = max(sapply(df, function(x) length(levels(x)))) # find the maximum length of levels in all x variables
	while(i <= 1){
		new_trt = assign(trt_grp, prob = c(0.5, 0.5))
		df$trt[i] = new_trt
		i = i + 1
	}
	while(i <= nrow(df)){
		# print(i)
		chisq_stat = range = rep(Inf, length(trt_grp))
		for (j in seq_along(trt_grp))
		{
			df$trt[i] = trt_grp[j]
			# chisq_stat[j] = max(sapply(df[, names(df) != 'trt'], function(x) chisq.test(table(x, df$trt))$statistic))
			range[j] = sum(sapply(df[, names(df) != 'trt'], function(x) sum(apply(table(x, df$trt), 1, function(y) max(y) - min(y))))) # measure the absolute difference in terms of cell frequencies between two arms for all potential treatment assignment (a.k.a range method)
			# print(range)
		}
		# prob_assign = ifelse(chisq_stat[1] == chisq_stat[2], 0.5, ifelse(chisq_stat[1] < chisq_stat[2], prob_bc, 1-prob_bc))
		prob_assign = ifelse(range[1] == range[2], 0.5, ifelse(range[1] < range[2], prob_bc, 1 - prob_bc)) # assign higher probability to assignment leads to smaller imbalance
		df$trt[i] = assign(trt_grp, c(prob_assign, 1-prob_assign)) # do random draw of the treatment for the patient to be randomized
		i = i + 1
	}
	return(df)
}

# a sample simulation to check the algorithm
test_sample = gen_sample(200, prob_age = c(0.3, 0.3, 0.4), prob_sex = 0.5, prob_site = 0.8, prob_occ = 0.7)
temp = BAR(test_sample, prob_bc = 0.8) # 0.8/0.2 biased coin probability split for two-arm study 
summary(tableby(trt ~ ., data = temp), text = TRUE)

# the program works pretty well on balancing equal allocation of treatment overall and within each factor
# parameters that we can tune in this program: prob_bc, the probability for levels within each stratification factor; or may be the measure of difference between two arms (so far, frequency difference works better than chisq statistic for the simulation observation)
```