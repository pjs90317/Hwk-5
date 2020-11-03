Homework 5
================
Patrick Sinclair, Kieran Yuen

For our subset, we decided to examine how different variables impact
wages for people who identify as female working in the various aspects
of the medical industry. We did this by creating an object that houses
different industry codes pertaining to the medical field, including
physician and dentist offices, hospitals, nursing facilities and other
health-care practitioners.

For our initial regression, we elected to use the
[INCWAGE](https://cps.ipums.org/cps-action/variables/INCWAGE#description_section)
observations as our measure of wage. We made this decision as we believe
it is reasonable to assume that most of the data observations in our
subset (females in the medical field with a Bachelor or Advanced degree)
would only hold one job. INCWAGE is ‘the total pre-tax and salary
income’ each of the respondent’s earned as an employee in the previous
12 months prior to the survey.

    ## 
    ## ==========================================================
    ##                              Dependent variable:          
    ##                     --------------------------------------
    ##                     INCWAGE ~ AGE + I(AGE2) + educ_college
    ## ----------------------------------------------------------
    ## AGE                              5,121.620***             
    ##                                   (987.179)               
    ##                                                           
    ## I(AGE2)                           -46.251***              
    ##                                    (10.823)               
    ##                                                           
    ## educ_college                    -35,171.720***            
    ##                                  (2,897.363)              
    ##                                                           
    ## Constant                         -22,289.850              
    ##                                  (21,349.910)             
    ##                                                           
    ## ----------------------------------------------------------
    ## Observations                        2,626                 
    ## R2                                  0.082                 
    ## Adjusted R2                         0.081                 
    ## Residual Std. Error         73,930.770 (df = 2622)        
    ## F Statistic                77.788*** (df = 3; 2622)       
    ## ==========================================================
    ## Note:                          *p<0.1; **p<0.05; ***p<0.01

The regressor coefficients from the initial regression all appear to be
statistically significant, the only exception being the intercept
coefficient. What is curious is that holding a Bachelor’s degree has a
*negative* correlation with wages within the subset, as does \(Age^2\).
However, for our observations this makes sense; we limited our
observations to people who hold Bachelor’s or Advanced degrees. From an
outsider’s perspective of the industry, those whole hold advanced
degrees or extra certifications within the medical field would be those
who have specialized in particular fields, commanding higher wages for
their specialized knowledge. A Bachelor’s degree is closer to an entry
level requirement in the field, so the comparative wages are
significantly lower.

From this regression, we created our prediction model using 50% of the
2626 observations in our subset in order to give the model a sizable
enough sample from which to draw its comparisons. For easy
visualization, the jitter has been set to 0.1, to demonstrate the trends
at different ages and the range of the INCWAGE axis has been capped at
300,000.

To draw some comparison from the initial regression, we set the
prediction model to predict wages for those with advanced degrees.

![Initial Regression Plot](./initlmplot.png)

The plot for the predicted values shows us a gently sloped concave curve
that has a peak predicted value of 119489.4.

When we added high polynomials, \(Age^3\), \(Age^4\) and \(Age^5\), to
the regression, the absolute values of the corresponding coefficients
get progressively smaller.

    ## 
    ## ========================================================================================
    ##                                             Dependent variable:                         
    ##                     --------------------------------------------------------------------
    ##                     INCWAGE ~ AGE + I(AGE2) + I(AGE3) + I(AGE4) + I(AGE5) + educ_college
    ## ----------------------------------------------------------------------------------------
    ## AGE                                              6,701.787                              
    ##                                                (143,188.500)                            
    ##                                                                                         
    ## I(AGE2)                                           282.990                               
    ##                                                 (6,578.649)                             
    ##                                                                                         
    ## I(AGE3)                                           -16.410                               
    ##                                                  (147.274)                              
    ##                                                                                         
    ## I(AGE4)                                            0.276                                
    ##                                                   (1.609)                               
    ##                                                                                         
    ## I(AGE5)                                            -0.002                               
    ##                                                   (0.007)                               
    ##                                                                                         
    ## educ_college                                   -35,144.240***                           
    ##                                                 (2,909.189)                             
    ##                                                                                         
    ## Constant                                        -106,948.300                            
    ##                                               (1,213,726.000)                           
    ##                                                                                         
    ## ----------------------------------------------------------------------------------------
    ## Observations                                       2,626                                
    ## R2                                                 0.082                                
    ## Adjusted R2                                        0.080                                
    ## Residual Std. Error                        73,953.420 (df = 2619)                       
    ## F Statistic                               39.103*** (df = 6; 2619)                      
    ## ========================================================================================
    ## Note:                                                        *p<0.1; **p<0.05; ***p<0.01

![Polynomial Regression](./polyplot.png)

When plotted, the curves follow a similar slope and shape. Adding the
extra polynomials increases the maximum predicted wage slightly to
120590.2 but we notice a steepening of the “Polynomials” curve as age
approaches 70. Running a joint hypothesis shows us that the higher-order
polynomial terms are not significant; i.e. they have next to
relationship with how wages are determined within the dataset.

    ## Linear hypothesis test
    ## 
    ## Hypothesis:
    ## I(AGE^3)  + I(AGE^4)  + I(AGE^5) = 0
    ## 
    ## Model 1: restricted model
    ## Model 2: INCWAGE ~ AGE + I(AGE^2) + I(AGE^3) + I(AGE^4) + I(AGE^5) + educ_college
    ## 
    ##   Res.Df        RSS Df Sum of Sq      F Pr(>F)
    ## 1   2620 1.4324e+13                           
    ## 2   2619 1.4324e+13  1  67104124 0.0123 0.9118

In both regression, wages as a function of age trend upwards. Wage peaks
at the age of 55 in the initial regression model but peaks later, at 59,
in the polynomial model. The range of predicted wages is larger in the
polynomial model by 4549.33

If we use \(log(Age)\) in our regression instead, we can clearly see
that the log function flattens the predicted values towards the upper
end of our age range.  
![log regression plot](./logplot.png)  
The log function demonstrates that the percentage changes in wage
relative to the increase in age are small compared to wage changes for
younger workers. The log regression curve also shows us the impact of
outliers on the higher-order polynomials regression.

**Dummy Variables**  
Were we to add the *educ\_hs* dummy variable into this regression it
would have zero effect because our dataset only includes those with a
Bachelor or Advanced degree. However, if we add both the *educ\_college*
and *educ\_advdeg*, we fall into the dummy variable trap - all
observations will satisfy one or the other category. To draw relevant
insights from the regression, we need to exclude one category from the
regression. If we had these dummy variables coded into a factor, we can
have R exclude any particular category by changing the order in which
the categories are included in the factor. Similarly, if both dummy
variables are included in the regression, we can make sure R excludes
the relevant dummy by changing their order.

We don’t apply polynomials or logs to the dummy variables as they will
not have any effect on the regression. \(1^x = 1\); \(0^x = 0\).

Here are some predicted values for each of the three regression models
that we have created.

    ##    AGE      yhat yhatpolys   yhatlog
    ## 1   30  89732.67  90835.37  90170.61
    ## 2   31  92032.97  93491.93  92776.40
    ## 3   32  94240.76  95905.31  95201.80
    ## 4   33  96356.05  98091.90  97457.92
    ## 5   34  98378.84 100068.70  99554.87
    ## 6   57 119372.33 120234.18 118342.81
    ## 7   58 119175.06 120485.35 118280.71
    ## 8   59 118885.29 120590.21 118167.10
    ## 9   60 118503.02 120514.80 118003.71
    ## 10  61 118028.24 120220.65 117792.18

The predicted values demonstrate the upward trend of wages towards the
late 50s of those working in the medical field. The predicted values
seem to make sense. Younger professionals earn less than their more
experienced counterparts. The gap between more experience physicians and
their contemporaries working in other hospitals jobs, such as janitors
or caterers, would be greater. Glied, Ma and Pearlsteins’ 2015
[article](https://www.healthaffairs.org/doi/full/10.1377/hlthaff.2014.1367)
has a succinct depiction of the differences between Physicians, Nurses
and other health industry workers in the first results table. Their data
comes from the 2014 CPS data and is reported in 2013 dollars.

**Elasticity**  
By changing the dependent variable to log(INCWAGE), we encountered an
error as there are 45 observations in the dataset that have INCWAGE = 0.
To get around this issue, we asked the regression to take
\(log(INCWAGE + 1)\) to remove the issue of the 0 values whilst
minimizing the impact on the other observations. The observed values are
generally big enough that the addition of 1 does not impact the log of
the observations with any significance.
![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

Here we compare INCWAGE \~ log(AGE) and log(INCWAGE + 1) \~ log(AGE)

![log plot](./solologplot.png) ![log of log plot](./logfunlogplot.png)

    ##   AGE  yhatlog yhatlogfun
    ## 1  25 73958.43   10.74424
    ## 2  26 77678.45   10.79046
    ## 3  27 81141.96   10.83278
    ## 4  28 84367.65   10.87147
    ## 5  29 87372.20   10.90679
    ## 6  30 90170.61   10.93897

Changing our dependent variable to log(INCWAGE + 1) depicts the
percentage change in wages in response to a percentage change in age.
While the pattern of predicted values in the regression with INCWAGE
trends upwards until age reaches the mid 50s, then begins to trend down,
the predicted values for log(INCWAGE + 1) stay consistent at
approximately 11.0375274. If we take the log of this value, we see that
a 1% increase in age has a corresponding average increase of 2.4013011
in wages.

**Interactions**  
For our interactions, we add a factor based on the times workers leave
their homes to get to work. The factor is split into 4 time periods,
Morn, Mid, Grave, Night, corresponding to (5am-9:30am), (9:31am-5:00pm),
(5:01pm-11:59pm) and (12:00-4:59am) respectively.

We chose departure time because we believe the time someone leaves for
work would be a strong indicator of how much they make in wages. We take
the assumption that most people prefer to work during daytime hours.
Research has shown that human biology follows a [circadian
rhythm](https://www.sleepfoundation.org/circadian-rhythm#:~:text=The%20sleep%2Dwake%20cycle%20is,keep%20us%20awake%20and%20active)
to allow the brain and body to be awake and alert during the sunny hours
and rest at nighttime. It is hard to purposefully adjust circadian
rhythms so we can be more alert during nighttime hours *(insert research
link here)*. Therefore, we expect that those who have departure times
during the day time would have higher incomes than those who have “less
desirable” departure times, such as a graveyard shift at a hospital.

**Are your other dummy variables in the regression working sensibly with
your selection criteria?**

    ## 
    ## Call:
    ## lm(formula = INCWAGE ~ AGE + I(AGE^2) + deptime + educ_college + 
    ##     (deptime * AGE) + (deptime * (AGE^2)) + (deptime * educ_college), 
    ##     data = dat_med)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -123025  -35288  -10221   16683  576257 
    ## 
    ## Coefficients:
    ##                            Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)               -25859.72   21598.79  -1.197   0.2313    
    ## AGE                         5369.32     992.19   5.412 6.81e-08 ***
    ## I(AGE^2)                     -48.40      10.84  -4.465 8.35e-06 ***
    ## deptimeMid                -35954.03   21526.14  -1.670   0.0950 .  
    ## deptimeGrave              -12252.80   28910.39  -0.424   0.6717    
    ## deptimeNight               22627.20   24160.88   0.937   0.3491    
    ## educ_college              -38997.79    3205.88 -12.164  < 2e-16 ***
    ## AGE:deptimeMid               241.78     442.60   0.546   0.5849    
    ## AGE:deptimeGrave            -171.82     582.60  -0.295   0.7681    
    ## AGE:deptimeNight            -593.86     463.27  -1.282   0.2000    
    ## deptimeMid:educ_college    21894.38   10876.21   2.013   0.0442 *  
    ## deptimeGrave:educ_college  15521.92   13915.71   1.115   0.2648    
    ## deptimeNight:educ_college  17294.45   13870.83   1.247   0.2126    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 73770 on 2613 degrees of freedom
    ## Multiple R-squared:  0.08886,    Adjusted R-squared:  0.08468 
    ## F-statistic: 21.24 on 12 and 2613 DF,  p-value: < 2.2e-16

    ## 
    ## =====================================================
    ##                               Dependent variable:    
    ##                           ---------------------------
    ##                                     INCWAGE          
    ## -----------------------------------------------------
    ## AGE                              5,369.325***        
    ##                                    (992.194)         
    ##                                                      
    ## I(AGE2)                           -48.405***         
    ##                                    (10.841)          
    ##                                                      
    ## deptimeMid                       -35,954.030*        
    ##                                  (21,526.140)        
    ##                                                      
    ## deptimeGrave                      -12,252.800        
    ##                                  (28,910.390)        
    ##                                                      
    ## deptimeNight                      22,627.200         
    ##                                  (24,160.880)        
    ##                                                      
    ## educ_college                    -38,997.790***       
    ##                                   (3,205.877)        
    ##                                                      
    ## AGE:deptimeMid                      241.784          
    ##                                    (442.596)         
    ##                                                      
    ## AGE:deptimeGrave                   -171.822          
    ##                                    (582.602)         
    ##                                                      
    ## AGE:deptimeNight                   -593.855          
    ##                                    (463.267)         
    ##                                                      
    ## deptimeMid:educ_college          21,894.380**        
    ##                                  (10,876.210)        
    ##                                                      
    ## deptimeGrave:educ_college         15,521.920         
    ##                                  (13,915.710)        
    ##                                                      
    ## deptimeNight:educ_college         17,294.440         
    ##                                  (13,870.830)        
    ##                                                      
    ## Constant                          -25,859.720        
    ##                                  (21,598.790)        
    ##                                                      
    ## -----------------------------------------------------
    ## Observations                         2,626           
    ## R2                                   0.089           
    ## Adjusted R2                          0.085           
    ## Residual Std. Error         73,769.790 (df = 2613)   
    ## F Statistic                21.237*** (df = 12; 2613) 
    ## =====================================================
    ## Note:                     *p<0.1; **p<0.05; ***p<0.01

and explain those outputs (different peaks for different groups).

What are the other variables you are using in your regression? Do they
have the expected signs and patterns of significance? Explain if there
is a plausible causal link from X variables to Y and not the reverse.
Explain your results, giving details about the estimation, some
predicted values, and providing any relevant graphics. Impress.

### Bibliography

<https://cps.ipums.org/cps-action/variables/INCWAGE#description_section>

Glied, Sherry A., Ma, Stephanie and Pearlstein, Ivanna, (2015).
“Understanding Pay Differentials Among Health Professionals,
Nonprofessionals, And Their Counterparts In Other Sectors”
<https://www.healthaffairs.org/doi/full/10.1377/hlthaff.2014.1367>

Our Regression Model: Departure Time and Health Insurance
