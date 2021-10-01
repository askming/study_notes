
# Notes on ICH guidelines​

## ICH E9 (R1) - Addendum On **Estimands** And Sensitivity Analysis In Clinical Trials To The Guideline On Statistical Principles For Clinical Trials 

*(Adopted on 20 November 2019)*

### 1. Purpose and scope

- With the concept of **Intention-To-Treat (ITT)** introduced by ICH E9, the question remains
  whether estimating an effect in accordance with the ITT principle always represents the
  treatment effect of greatest relevance to regulatory and clinical decision making. 
- Secondly, issues considered generally under data handling and “**missing data**” (see Glossary) are re-visited. Two important distinctions are made. 
  - Firstly, the addendum distinguishes discontinuation of randomised treatment from study withdrawal. The former represents an intercurrent event, to be addressed in the precise specification of the trial objective through the estimand. The latter gives rise to missing data to be addressed in the statistical analysis.
  - Secondly, the addendum highlights the distinct consequences of different intercurrent events.
- Thirdly, issues related to the concept of analysis sets are considered in the framework.
  - (ICH E9) Section 5.2. strongly recommends that analysis of superiority trials be based on the full analysis set, defined to be as close as possible to including all randomised subjects. 
  - The meaning and role of an analysis of the **per protocol set** is also re-visited in this addendum; in particular whether the need to explore the impact of protocol violations and deviations can be addressed in a way that is less biased and more interpretable than naïve analysis of the per protocol set. 
- Finally, the concept of **robustness** (see 1.2.) is given expanded discussion under the heading of sensitivity analysis. 
- While the main focus is on randomised clinical trials, the principles are also applicable for
  single arm trials and observational studies.

### 2. A Framework to Align Planning, Design, Conduct, Analysis And Interpretation

- **Estimand**: trial objective, target of estimation

- **Estimator**: the method of estimation

- **Estimate**: the numerical result
- Steps:
  - Clear trial objectives should be translated into key clinical questions of interest by defining suitable estimands. 
  - A suitable method of estimation can then be selected and the main estimator will be underpinned by certain assumptions. 
  - To explore the robustness of inferences from the main estimator to deviations from its underlying assumptions, a sensitivity analysis should be conducted, in the form of one or more analyses, targeting the same estimand 

![image-20201029172037481](https://raw.githubusercontent.com/askming/picgo/master/image-20201029172037481.png)


### 3. Estimand

> An estimand is a precise description of the treatment effect reflecting the clinical question posed by a given clinical trial objective.

- The description of an estimand involves precise specifications of certain attributes, which should be developed based not only on clinical considerations but also on how *intercurrent events* are reflected in the clinical question of interest. 

#### 3.1 Intercurrent events

- **Intercurrent events** are events occurring after treatment initiation that affect either the interpretation or the existence of the measurements associated with the clinical question of interest.
- Unlike missing data, intercurrent events are not to be thought of as a drawback to be avoided in clinical trials. 
  - Examples of intercurrent events that can affect interpretation of the measurements include
    discontinuation of assigned treatment and use of an additional or alternative therapy.
- Because the estimand is to be defined in advance of trial design, neither study
  withdrawal nor other reasons for missing data (e.g. administrative censoring in trials with
  survival outcomes) are in themselves intercurrent events.

#### 3.2 Strategies for addressing intercurrent events when defining the clinical question of interest

- **Treatment policy strategy** 
  - The occurrence of the intercurrent event is considered irrelevant in defining the treatment effect of interest: the value for the variable of interest is used regardless of whether or not the intercurrent event occurs.
  - In general, the treatment policy strategy **cannot** be implemented for **intercurrent events that are terminal events**, since values for the variable after the intercurrent event do not exist. 

- **Hypothetical strategies**
  - A scenario is envisaged in which the intercurrent event would not occur: the value of the variable to reflect the clinical question of interest is the value which the variable would have taken in the hypothetical scenario defined. 

- **Composite variable strategies**
  - This relates to the variable of interest. An intercurrent event is considered in itself to be informative about the patient’s outcome and is therefore incorporated into the definition of the variable. 
  - Composite variable strategies can be viewed as implementing the intention-to-treat principle in some cases where the original measurement of the variable might not exist or might not be meaningful, but where the intercurrent event itself meaningfully describes the patient’s outcome, such as when the patient dies.

- **While on treatment strategies**
  - For this strategy, response to treatment prior to the occurrence of the intercurrent event is of interest.
  - Like the composite variable strategy, the while on treatment strategy can hence be thought of as impacting the definition of the variable, in this case by restricting the observation time of interest to the time before the intercurrent event. 
  - Particular care is required if the occurrence of the intercurrent event differs between the treatments being compared 

- **Principal Stratum strategies**
  - This relates to the population of interest. The target population might be taken to be the “principal stratum” in which an intercurrent event would/would not occur. 
  - The clinical question of interest relates to the treatment effect only within the principal stratum. 
  - It is important to distinguish “**principal stratification**”, which is based on potential intercurrent events (for example, subjects who would discontinue therapy if assigned to the test product), from subsetting based on actual intercurrent events (subjects who discontinue therapy on their assigned treatment). 
  - Treatment effects defined by comparing outcomes in these subsets confound the effects of the different treatments with the differences in outcomes possibly due to the differing characteristics of the subjects. 

#### 3.3. Estimand attributes

- The treatment and the alternative treatment condition to which comparison will be made
- The population of patients targeted by the clinical question
- The variable (or endpoint) to be obtained for each patient that is required to address the clinical question. 
- Intercurrent events
- A population-level summary for the variable should be specified, providing a basis for comparison between treatment conditions. 

#### 3.4 Considerations for constructing an estimand

- The construction of an estimand should consider what is of clinical relevance for the particular
  treatment in the particular therapeutic setting.

  - Considerations include the disease under study, the clinical context (e.g. the availability of alternative treatments), the administration of treatment (e.g. one-off dosing, short-term treatment or chronic dosing) and the goal of treatment (e.g. prevention, disease modification, symptom control). 

- When constructing the estimand it is necessary to have a clear understanding of the treatment
  to which the clinical question of interest pertains.

- Discussions should also consider whether specifications for the population and variable
  attributes should be used to reflect the clinical question of interest in respect of any intercurrent
  events.

- Where significant issues exist to develop an appropriate trial design or to derive an adequately reliable estimate for a particular estimand, an alternative estimand, trial design and method of analysis would need to be considered. 

  - Where significant issues exist to develop an appropriate trial design or to derive an adequately reliable estimate for a particular estimand, an alternative estimand, trial design and method of analysis would need to be considered. 

- Whilst an inability to derive a reliable estimate might preclude certain choices of strategy, it is important to proceed sequentially from the trial objective and an understanding of the clinical question of interest, and not for the choice of data collection and method of analysis to determine the estimand. 

- Characterising beneficial effects using estimands based on the *treatment policy strategy* might also be more generally acceptable to support regulatory decision making, specifically in settings where estimands based on alternative strategies might be considered of greater clinical interest, but main and sensitivity estimators cannot be identified that are agreed to support a reliable estimate or robust inference.

  - An estimand based on the treatment policy strategy might offer the possibility to obtain a reliable estimate of a treatment effect that is still relevant. In this situation, it is recommended to also include those estimands that are considered to be of greater clinical relevance and to present the resulting estimates along with a discussion of the limitations, in terms of trial design or statistical analysis, for that specific approach. 

- When constructing estimands based on the treatment policy strategy, inference can be complemented by defining an additional estimand and analysis pertaining to each intercurrent event for which the strategy is used

- An estimand using a *while on treatment strategy* should usually be accompanied by the additional information on the time to intercurrent event distributions 

- An estimand based on a *principal stratum* would usefully be accompanied by information on the proportion of patients in that stratum, if available.

- Estimands that are constructed with one or more intercurrent events accounted for using the treatment policy strategy present similar issues for non-inferiority and equivalence trials as those related to analysis of the FAS under the ITT principle.

- When selecting strategies, it might be important to distinguish between trials designed to detect whether differences exist between treatments containing the same or similar active substance (e.g. comparison of a biosimilar to a reference treatment) and trials where a non-inferiority or equivalence hypothesis is used in order to establish and quantify evidence of efficacy.

- An estimand can be constructed to target a treatment effect that prioritises sensitivity to detect differences between treatments, if appropriate for regulatory decision making. 

  

### 4. Impact on trial design and conduct

- Certainly, subjects cannot be retained in a trial against their will, and in some trials missing data for some subjects is inevitable by design, such as administrative censoring in trials with survival outcomes.  On the contrary, the occurrence of intercurrent events such as discontinuation of treatment, treatment switching, or use of additional medication, does not imply that the variable cannot be measured thereafter, though the measures may not be relevant.
- A prospective plan to collect informative reasons for why data intended for collection are missing may help to distinguish the occurrence of intercurrent events from missing data.
- Where all subjects contribute information to the analysis, and where the impact of the strategy to reflect intercurrent events is included in the effect size that is targeted and the expected variance, it is not usually necessary to additionally inflate the calculated sample size by the expected proportion of subject withdrawals from the trial.

### 5. Impact on trial analysis

#### 5. 1 Main estimation

- All methods of analysis rely on assumptions, and different methods may rely on different
  assumptions even when aligned to the same estimand.
- A composite variable strategy can avoid statistical assumptions about data after an intercurrent
  event by considering occurrence of the intercurrent event as a component of the outcome. The
  potential concern relates less to assumptions for estimation, and more to the interpretation of
  the estimated treatment effect.
- Failure to collect relevant data should not be confused with the choice not to collect, or to collect and not to use, data made irrelevant by an intercurrent event.

#### 5. 2 Sensitivity analysis

##### 5.2.1 Role of sensitivity analysis

- Sensitivity analysis should be planned for the main estimators of all estimands that will be important for regulatory decision making and labelling in the product information.
- This might be characterised as the extent of departures from assumptions that change the interpretation of the results in terms of their statistical or clinical significance (e.g. tipping point analysis).
- Distinct from sensitivity analysis, where investigations are conducted with the intent of exploring robustness of departures from assumptions, other analyses that are conducted in order to more fully investigate and understand the trial data can be termed “supplementary analysis”

##### 5.2.2 Choice of sensitivity analysis

- It is therefore desirable to adopt a structured approach, specifying the changes in assumptions that underlie the alternative analyses, rather than simply comparing the results of different analyses based on different sets of assumptions.

#### 5.3 Supplementary analysis

- (ICH E9) Section 5.2.3. indicates that it is usually appropriate to plan for analyses based on both the FAS and the Per Protocol Set (PPS) so that differences between them can be the subject of explicit discussion and interpretation.
- It is also described in Section 5.2.2. that results based on a PPS might be subject to severe bias.
- Notwithstanding the differences between violations and deviations from the protocol and intercurrent events, events likely to affect the interpretation or existence of measurements are considered in the description of the estimand.

### 6. Documenting estimand and sensitivity analysis

- Summaries of the number and timings of each intercurrent event in each treatment group should be reported.
- Changes to the estimand during the trial can be problematic and can reduce the credibility of
  the trial.
- A change to the estimand should usually be reflected through amendment to the protocol.