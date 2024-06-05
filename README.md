# Causal-Analysis
Causal Analysis of Physical Activity on BMI using NHANES Data

## Introduction and Data Description

The National Health and Nutrition Examination Survey (NHANES), conducted
by the U.S. National Center for Health Statistics (NCHS), is an
invaluable resource for assessing the health and nutritional status of
the American population. Starting in the 1960s, NHANES has been pivotal
in providing data that guide public health policies and medical
guidelines. The 2009-2012 NHANES dataset, specifically its raw version
(NHANESraw), is utilized in this study. It offers a detailed and
representative cross-sectional view of the non-institutionalized
civilian resident population of the United States.

NHANESraw is distinct for its comprehensive coverage of health,
nutritional, and demographic variables. It uses a complex survey design
that includes oversampling of specific subpopulations, like racial
minorities, to ensure their adequate representation. This design
complexity requires rigorous analytical approaches to avoid biased
conclusions.

## Objective

This study aims to determine the causal impact of physical activity on
Body Mass Index (BMI) among the U.S. population, using the NHANESraw
2009-2012 dataset. The analysis employs the backdoor criterion for
causal inference while controlling for potential confounders and the
intricate survey design inherent in NHANES.

## Data Preparation

For this analysis, I am considering six potential confounders - a
variable that could affect both the treatment and the outcome. First,
let’s examine the types of the variables of interest in the NHANES raw
dataset.

-PhysActive (Treatment): A binary factor variable indicating the
physical activity status of individuals, categorized as ‘Yes’ or ‘No’.  
-BMI (Outcome): Representing the Body Mass Index, a numerical measure
calculated from weight and height.  
-Age (Integer): Age, recorded in years, is a fundamental demographic
variable.  
-Gender (Factor): Categorized as ‘female’ or ‘male’.  
-Race1 (Factor): This variable, with categories like ‘Black’,
‘Hispanic’, etc., reflects the racial diversity of the dataset.  
-Education (Factor): With levels indicating educational attainment, such
as ‘8th Grade’, ‘High School’, ‘College Grad’, etc.  
-HHIncome (Factor): Representing household income in various brackets.  
-MaritalStatus (Factor): Including categories like ‘Married’,
‘Divorced’, ‘Single’, etc.

## Potential Outcomes and Their Interpretation

In the context of this study, the potential outcomes refer to the
different Body Mass Index (BMI) levels that individuals in the NHANES
dataset might have under varying scenarios of physical activity.
Specifically, the two key potential outcomes are: 1) the BMI level of an
individual if they engage in regular physical activity, and 2) the BMI
level of the same individual if they do not engage in physical activity.
These outcomes allow us to compare the BMI levels across these two
scenarios, thus providing insight into the effect of physical activity
on BMI.

## Causal Estimand(s) of Interest and Their Interpretations

The primary causal estimand in this analysis is the average treatment
effect (ATE) of physical activity on BMI. This refers to the average
difference in BMI between the two potential outcomes: when individuals
are physically active versus when they are not. The interpretation of
this ATE is pivotal as it quantifies the expected change in BMI that
could be attributed to physical activity. If the ATE is significantly
negative, it implies that engaging in physical activity is associated
with a lower BMI on average, suggesting potential health benefits of
being physically active.

## Checking for Duplicates in the Dataset

An essential step in data preparation is to ensure the uniqueness of
each observation. Duplicates can skew the analysis and lead to
misleading conclusions. Therefore, a thorough check for duplicate rows
in the NHANESraw dataset was conducted.

    ## Total Rows in the Dataset: 20293

    ## Unique Rows in the Dataset: 20293

The results indicate that there are no duplicate rows in the NHANESraw
dataset. Each row represents a unique observation, which is crucial for
maintaining the accuracy of the analysis.

## Handling Missing Data

In the next phase of data preparation for the NHANESraw dataset, I
address the issue of missing values in key variables. Proper handling of
missing data is critical for the robustness of the analysis, as missing
values can introduce bias and potentially invalidate the results.

An assessment of missing values in the NHANESraw dataset was conducted
for the selected variables.

    ##    PhysActive           BMI           Age        Gender         Race1 
    ##          6015          2279             0             0             0 
    ##     Education      HHIncome MaritalStatus 
    ##          8535          2076          8526

Given the nature of the NHANESraw dataset, a strategic approach to
handle missing values is implemented:

BMI (Outcome Variable): Special attention is given to handling missing
values in BMI. Given its importance as the outcome variable, an
imputation strategy, such as median imputation, will be employed. This
approach is justified by the need to maintain a complete dataset for a
robust analysis of the outcome.

PhysActive (Treatment) and Confounders: For the treatment variable
(PhysActive) and other confounders (Age, Gender, Race1, Education,
HHIncome, MaritalStatus), and listwise deletion will be utilized. Records
with missing values in these variables will be excluded from the
analysis. This decision is based on the principle of maintaining data
integrity for variables directly influencing the treatment and
confounders in the causal model.

## Descriptive Analysis

After addressing the missing values in the dataset, the next step in the
analysis involves creating visualizations for the key variables. These
visualizations are important for understanding the distribution and
characteristics of the data, particularly focusing on non-missing values
to ensure accuracy.

<div class="figure" style="text-align: center">

<img src="Project_files/figure-gfm/unnamed-chunk-4-1.png" alt="Histogram of BMI (Non-Missing Values)" width="100%" />
<p class="caption">
Histogram of BMI (Non-Missing Values)
</p>

</div>

According to the histogram (Figure 01), we can observe that the BMI
values exhibit a right skewed distribution with a mean of 24.92. Due to
the skewness of this distribution, I will use the median as a summary
measure for the BMI variable.

<div class="figure" style="text-align: center">

<img src="Project_files/figure-gfm/unnamed-chunk-5-1.png" alt="Bar Plot of Physical Activity (Non-Missing Values)" width="80%" />
<p class="caption">
Bar Plot of Physical Activity (Non-Missing Values)
</p>

</div>

According to the bar chart (Figure 02), a slightly higher proportion of
individuals in the dataset engage in physical activity compared to those
who do not. While this difference is noticeable, it is not markedly
substantial, indicating a relatively balanced distribution of physical
activity status among the participants.

## Causal Analysis

**Identification Conditions of the Causal Model**

The causal inference relies on several key assumptions:

1.  Stable Unit Treatment Value Assumption (SUTVA): This assumption
    posits that the potential outcome for any individual is unaffected
    by the treatment status of others. In this context, it implies that
    an individual’s BMI is influenced solely by their physical activity
    level, independent of others’ activity levels.

2.  Positivity: This condition assumes that every individual in the
    population has a non zero probability of experiencing each treatment
    level. For this study, this translates to all individuals having
    some chance of being physically active or inactive.

3.  Consistency: Under this assumption, the potential outcome of an
    individual under the treatment they receive is equal to the observed
    outcome. In simpler terms, if an individual is observed as
    physically active, their BMI corresponds to this state of activity.

4.  No Unobserved Confounding: This assumption suggests that all
    confounders affecting both the treatment (physical activity) and the
    outcome (BMI) are observed and accounted for in the model. This
    assumption is critical for asserting that the observed differences
    in BMI are due to physical activity levels and not other unmeasured
    factors.

**Assessment of Identification Conditions**

SUTVA is generally reasonable in observational studies like NHANES,
where individual outcomes (BMI levels) are not likely to be directly
influenced by others’ physical activity levels. Positivity is a
plausible assumption in our analysis, given the diverse representation
of physical activity levels across various demographic groups.
Consistency is maintained, as physical activity is a well defined and
measurable exposure in NHANES.

The potential for unobserved confounding exists, especially considering
factors like genetic predispositions or unrecorded lifestyle habits that
might influence both physical activity and BMI. While my model accounts
for many known confounders, the possibility of residual confounding
cannot be entirely ruled out.

**Statistical Estimands and Estimator**

The primary estimand in this study is the average treatment effect of
physical activity on BMI, adjusting for age, gender, race, education,
income and Marital Status. The aim is to estimate the difference in mean
BMI between individuals who are physically active and those who are not.

The estimator employed is a survey weighted regression model. This model
inherently assumes linearity in parameters and that the residuals are
normally distributed. I also assume that observations are independent
within each cluster defined by the survey’s complex design. This
approach allows to accurately reflect the NHANES sampling strategy and
provide unbiased estimates that are representative of the U.S.
population.

**Initial Model Analysis (Without BMI Imputation)**

The initial model aims to assess the relationship between physical
activity and BMI while accounting for confounders such as age, gender,
race, education, income and Marital Status. This model is based on the
NHANES dataset with unique entries and no imputation for missing BMI
values.

Defining the Survey Design:

-I defined the survey design using the svydesign function. This step
involved specifying the primary sampling units (PSUs), strata, and
survey weights.  
-ids = ~SDMVPSU: The ids argument denotes the clustering within the
dataset, with SDMVPSU representing the cluster variable.  
-strata = ~SDMVSTRA: The strata argument helps in acknowledging the
stratified sampling design, with SDMVSTRA being the stratification
variable.  
-weights = ~WTMEC2YR: The weights argument incorporates the sampling
weights (WTMEC2YR).  
-nest = TRUE: This parameter is set to account for the nesting of
clusters within strata.

Survey Weighted Regression Model:

After establishing the survey design, I conducted a survey weighted
regression analysis using the svyglm function. This approach allows for
the estimation of relationships between variables while respecting the
survey’s complex design.

    ## 
    ## Call:
    ## svyglm(formula = BMI ~ PhysActive + Age + Gender + Race1 + Education + 
    ##     HHIncome + MaritalStatus, design = nhanes_design)
    ## 
    ## Survey design:
    ## svydesign(ids = ~SDMVPSU, strata = ~SDMVSTRA, weights = ~WTMEC2YR, 
    ##     data = NHANESraw, nest = TRUE)
    ## 
    ## Coefficients:
    ##                            Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)               30.945811   0.853164  36.272 2.93e-08 ***
    ## PhysActiveYes             -1.347517   0.212742  -6.334 0.000725 ***
    ## Age                        0.014733   0.006702   2.198 0.070274 .  
    ## Gendermale                -0.010540   0.133521  -0.079 0.939646    
    ## Race1Hispanic             -2.165946   0.328682  -6.590 0.000587 ***
    ## Race1Mexican              -1.522003   0.367859  -4.137 0.006096 ** 
    ## Race1White                -2.568737   0.298429  -8.608 0.000135 ***
    ## Race1Other                -4.633499   0.362880 -12.769 1.42e-05 ***
    ## Education9 - 11th Grade    0.058326   0.332593   0.175 0.866558    
    ## EducationHigh School       0.672219   0.311630   2.157 0.074373 .  
    ## EducationSome College      0.809490   0.267883   3.022 0.023343 *  
    ## EducationCollege Grad     -0.265851   0.366999  -0.724 0.496089    
    ## HHIncome10000-14999        0.721083   0.594063   1.214 0.270425    
    ## HHIncome15000-19999        0.645450   0.607961   1.062 0.329242    
    ## HHIncome20000-24999        0.730401   0.635457   1.149 0.294133    
    ## HHIncome25000-34999        0.929418   0.660835   1.406 0.209219    
    ## HHIncome35000-44999        0.287628   0.563528   0.510 0.627995    
    ## HHIncome45000-54999        0.248743   0.629534   0.395 0.706417    
    ## HHIncome5000-9999          0.260061   0.563103   0.462 0.660470    
    ## HHIncome55000-64999        0.127118   0.690512   0.184 0.860005    
    ## HHIncome65000-74999        1.406634   0.841466   1.672 0.145626    
    ## HHIncome75000-99999        0.448351   0.747287   0.600 0.570474    
    ## HHIncomemore 99999        -0.376329   0.629216  -0.598 0.571648    
    ## MaritalStatusLivePartner  -0.831417   0.429298  -1.937 0.100904    
    ## MaritalStatusMarried      -0.160926   0.308545  -0.522 0.620659    
    ## MaritalStatusNeverMarried -1.244754   0.352678  -3.529 0.012376 *  
    ## MaritalStatusSeparated    -0.720419   0.515805  -1.397 0.211986    
    ## MaritalStatusWidowed      -0.928892   0.403148  -2.304 0.060758 .  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for gaussian family taken to be 56.39504)
    ## 
    ## Number of Fisher Scoring iterations: 2

    ## Coefficient for Physical Activity (PhysActiveYes): -1.347517

    ## 95% Confidence Interval for Physical Activity (PhysActiveYes): [ -1.764492 , -0.930542 ]

Physical Activity (PhysActiveYes): The coefficient for physical activity
is -1.3, with a 95% confidence interval of \[-1.8, -0.9\]. This result
indicates a statistically significant negative association between
physical activity and BMI. It implies that, on average, individuals who
are physically active have a lower BMI compared to those who are not
active, with the estimated difference in BMI being about 1.3 units lower
for physically active individuals. This finding aligns with the general
understanding that physical activity can contribute to a healthier BMI.

**Model with Imputed BMI**

In order to accommodate for the missing BMI values in the NHANESraw
dataset, I employed median imputation. This approach is particularly
appropriate given the right skewed distribution of BMI, where the median
serves as a more representative measure of central tendency than the
mean.

The survey design and the Survey Weighted Regression Model remained same
as the initial model.

    ## 
    ## Call:
    ## svyglm(formula = BMI ~ PhysActive + Age + Gender + Race1 + Education + 
    ##     HHIncome + MaritalStatus, design = nhanes_design_imputed)
    ## 
    ## Survey design:
    ## svydesign(ids = ~SDMVPSU, strata = ~SDMVSTRA, weights = ~WTMEC2YR, 
    ##     data = NHANES_imputed, nest = TRUE)
    ## 
    ## Coefficients:
    ##                            Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)               30.975124   0.864188  35.843 3.14e-08 ***
    ## PhysActiveYes             -1.311896   0.208680  -6.287 0.000754 ***
    ## Age                        0.013082   0.006618   1.977 0.095459 .  
    ## Gendermale                -0.018841   0.134163  -0.140 0.892912    
    ## Race1Hispanic             -2.126581   0.332790  -6.390 0.000691 ***
    ## Race1Mexican              -1.526423   0.365525  -4.176 0.005840 ** 
    ## Race1White                -2.564207   0.297946  -8.606 0.000135 ***
    ## Race1Other                -4.603946   0.361900 -12.722 1.45e-05 ***
    ## Education9 - 11th Grade    0.061280   0.332070   0.185 0.859669    
    ## EducationHigh School       0.675244   0.308414   2.189 0.071133 .  
    ## EducationSome College      0.798156   0.262766   3.038 0.022876 *  
    ## EducationCollege Grad     -0.263808   0.358061  -0.737 0.489060    
    ## HHIncome10000-14999        0.677593   0.575193   1.178 0.283388    
    ## HHIncome15000-19999        0.665642   0.612424   1.087 0.318806    
    ## HHIncome20000-24999        0.706436   0.626280   1.128 0.302401    
    ## HHIncome25000-34999        0.912670   0.663526   1.375 0.218125    
    ## HHIncome35000-44999        0.290628   0.561055   0.518 0.622995    
    ## HHIncome45000-54999        0.290351   0.632748   0.459 0.662479    
    ## HHIncome5000-9999          0.207776   0.557994   0.372 0.722421    
    ## HHIncome55000-64999        0.168040   0.689605   0.244 0.815599    
    ## HHIncome65000-74999        1.437543   0.839549   1.712 0.137685    
    ## HHIncome75000-99999        0.473151   0.743619   0.636 0.548082    
    ## HHIncomemore 99999        -0.355184   0.627484  -0.566 0.591889    
    ## MaritalStatusLivePartner  -0.857835   0.432375  -1.984 0.094498 .  
    ## MaritalStatusMarried      -0.181103   0.312664  -0.579 0.583513    
    ## MaritalStatusNeverMarried -1.275978   0.353113  -3.614 0.011184 *  
    ## MaritalStatusSeparated    -0.786336   0.520164  -1.512 0.181362    
    ## MaritalStatusWidowed      -0.975445   0.398248  -2.449 0.049835 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for gaussian family taken to be 54.31049)
    ## 
    ## Number of Fisher Scoring iterations: 2

    ## Coefficient for Physical Activity (PhysActiveYes) in Imputed Model: -1.311896

    ## 95% Confidence Interval for Physical Activity (PhysActiveYes) in Imputed Model: [ -1.72091 , -0.9028826 ]

This enhanced model similarly demonstrated a significant negative
relationship between physical activity and BMI. The findings are
consistent with the initial model, suggesting a robust association.

## Comparative Analysis and Conclusions

The comparison of the initial and imputed models shows consistency in
the primary findings. Physical activity maintains a negative association
with BMI across both models.

This study underscores the potential benefits of physical activity in
managing BMI and provides insights into the impact of demographic,
socio-economic, and marital factors on BMI. It is important to note,
however, that the observational nature of the analysis limits our
ability to establish definitive causality. Future studies using
longitudinal or experimental designs could provide a more detailed
understanding of these relationships.
