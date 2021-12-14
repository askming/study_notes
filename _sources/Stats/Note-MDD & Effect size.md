# Power, **MDD**/**E** (minimum detectable difference/effect), and **Effect size** in clinical trials

## MDD[^MDD]

- MDD/MDE tell us what is the smallest true effect, *in standard deviations of the outcome* (not devided by std), that is detectable for a given level of power and statistical significance.
- Because the standard deviation is influenced by sample size, MDE incorporates all of the information of a power calculation but does so in a way that applies to all experiments. That is to say, as long as we can guess at the variance of our outcome, the same sample size considerations apply every time we conduct any experiment.
- We can envision the MDE as the threshold where, for power = .8, 80% of the sampling distribution of the observed effect would be larger than observed effect (<u>i.e. if we sort the observed effect, MDE is the cutoff value that splits the distribution into 20/80, it's the minimum value to declare a postive result</u>)

## Effect size[^ES]

- The <u>effect size</u> is just the standardised mean difference between the two groups.
  
- The '*standard deviation*' is a measure of the spread of a set of values. Here it refers to the standard deviation of the population from which the different treatment groups were taken. In practice, however, this is almost never known, so it must be estimated either from the standard deviation of the control group, or from a 'pooled' value from both groups
  
- Relationship with significance

  - Effect size quantifies the size of the difference between two groups, and may therefore be said to be a true measure of the significance of the difference. 

  - statistical significance (referes as *p-value*) does *not* tell you the most important thing: *the size of the effect*. One way to overcome this confusion is to report the effect size, together with an estimate of its likely 'margin for error' or 'confidence interval'.

  - CI of effect size ($d$):

    $$ d - 1.96*se(d), d + 1.96*se(d), $$ 
    
    where $se(d) = \sqrt{\frac{N_E + N_C}{N_E*N_C} + \frac{d^2}{2(N_E + N_C)}}$

- Factors that can influence effect size

  1. *Standard deviation*  

  - Ideally, the control group will provide the best estimate of standard deviation, since it consists of a representative group of the population who have not been affected by the experimental intervention. However, this may not always be feasible: e.g. control group is too small or there is not true control group

  - In reality, it is often better to use a '*pooled*' estimate of standard deviation, which is essentially an average of the standard deviations of the experimental and control groups

    $$SD_{pooled} = \sqrt{\frac{(N_E-1)SD^2_E + (N_C-1)SD^2_C}{N_E+N_C-2}}$$

    - The use of a pooled estimate of standard deviation depends on the assumption that the two calculated standard deviations are estimates of *the same* population value.

  2. *Non-Normal distributions*
     - If the normality assumption is not true then the interpretation may be altered, and in particular, it may be difficult to make a fair comparison between an effect-size based on Normal distributions and one based on non-Normal distributions.
  3. *Measurement reliability*

  

- Types of effective size [^es_type]

  1. Pearson *r* correlation
  2. Standardized means difference
  3. Cohen's $d$ effect size:  the difference of two population means and it is divided by the standard deviation from the data $S = \sqrt{\frac{(N_E-1)SD^2_E + (N_C-1)SD^2_C}{N_E+N_C}} $
  4. Glass's $\Delta$ method of effect size: the SD used is from the control group
  5. Hedges' g method of effect size: this uses the *pooled* SD mentioned above
  6. Cohen's $f^2$ method: measures the effect size when we use methods like ANOVA, multiple regression, etc. $$f^2 = \frac{R^2}{1-R^2}$$ where $R^2$ is the squared multiple correlation
  7. Cramer's $\phi$ or cramer's $V$ method: Chi-square is the best statistic to measure the effect size for nominal data. In nominal data, when a variable has two categories, then *Cramer’s phi* is the best statistic use. When these categories are more than two, then *Cramer’s V* statistics will give the best result for nominal data.
  8. Odds ratio

[^MDD]: https://thomasleeper.com/Rcourse/Tutorials/power.html
[^ES]: https://www.leeds.ac.uk/educol/documents/00002182.htm
[^es_type]: https://www.statisticssolutions.com/statistical-analyses-effect-size/