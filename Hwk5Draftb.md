Hwk 5
================
Patrick Sinclair, Kieran Yuen

    ## The following object is masked from package:survival:
    ## 
    ##     veteran

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

    ## The following object is masked from package:survival:
    ## 
    ##     veteran

![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-6-2.png)<!-- -->![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-6-3.png)<!-- -->![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-6-4.png)<!-- -->

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

![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

    ## [1] 84317.64

![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

    ## [1] 119489.4

What is the peak of predicted wage? What if you add higher order
polynomials of age, such as \(Age^3\) or \(Age^4\)?

    ## The following objects are masked from dat_med (pos = 3):
    ## 
    ##     AfAm, AGE, Amindian, ANCESTR1, ANCESTR1D, ANCESTR2, ANCESTR2D,
    ##     Asian, below_150poverty, below_200poverty, below_povertyline, BPL,
    ##     BPLD, BUILTYR2, CITIZEN, CLASSWKR, CLASSWKRD, Commute_bus,
    ##     Commute_car, Commute_other, Commute_rail, Commute_subway, COSTELEC,
    ##     COSTFUEL, COSTGAS, COSTWATR, DEGFIELD, DEGFIELD2, DEGFIELD2D,
    ##     DEGFIELDD, DEPARTS, EDUC, educ_advdeg, educ_college, educ_hs,
    ##     educ_nohs, educ_somecoll, EDUCD, EMPSTAT, EMPSTATD, FAMSIZE,
    ##     female, foodstamps, FOODSTMP, FTOTINC, FUELHEAT, GQ,
    ##     has_AnyHealthIns, has_PvtHealthIns, HCOVANY, HCOVPRIV, HHINCOME,
    ##     Hisp_Cuban, Hisp_DomR, Hisp_Mex, Hisp_PR, HISPAN, HISPAND,
    ##     Hispanic, in_Bronx, in_Brooklyn, in_Manhattan, in_Nassau, in_NYC,
    ##     in_Queens, in_StatenI, in_Westchester, INCTOT, INCWAGE, IND,
    ##     LABFORCE, LINGISOL, MARST, MIGCOUNTY1, MIGPLAC1, MIGPUMA1,
    ##     MIGRATE1, MIGRATE1D, MORTGAGE, NCHILD, NCHLT5, OCC, OWNCOST,
    ##     OWNERSHP, OWNERSHPD, POVERTY, PUMA, PWPUMA00, RACE, race_oth,
    ##     RACED, RELATE, RELATED, RENT, ROOMS, SCHOOL, SEX, SSMC, TRANTIME,
    ##     TRANWORK, UHRSWORK, UNITSSTR, unmarried, veteran, VETSTAT,
    ##     VETSTATD, white, WKSWORK2, YRSUSA1

    ## The following object is masked from package:survival:
    ## 
    ##     veteran

![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-9-2.png)<!-- -->![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-9-3.png)<!-- -->![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-9-4.png)<!-- -->

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

![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

    ## [1] 85445.97

    ## 
    ## t test of coefficients:
    ## 
    ##                 Estimate  Std. Error  t value Pr(>|t|)    
    ## (Intercept)  -1.0695e+05  1.0105e+06  -0.1058   0.9157    
    ## AGE           6.7018e+03  1.2162e+05   0.0551   0.9561    
    ## I(AGE^2)      2.8299e+02  5.6859e+03   0.0498   0.9603    
    ## I(AGE^3)     -1.6410e+01  1.2913e+02  -0.1271   0.8989    
    ## I(AGE^4)      2.7566e-01  1.4267e+00   0.1932   0.8468    
    ## I(AGE^5)     -1.5706e-03  6.1468e-03  -0.2555   0.7983    
    ## educ_college -3.5144e+04  3.0778e+03 -11.4186   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    ## Linear hypothesis test
    ## 
    ## Hypothesis:
    ## I(AGE^2)  + I(AGE^3)  + I(AGE^4)  + I(AGE^5) = 0
    ## 
    ## Model 1: restricted model
    ## Model 2: INCWAGE ~ AGE + I(AGE^2) + I(AGE^3) + I(AGE^4) + I(AGE^5) + educ_college
    ## 
    ##   Res.Df        RSS Df Sum of Sq      F Pr(>F)
    ## 1   2620 1.4324e+13                           
    ## 2   2619 1.4324e+13  1   9410454 0.0017 0.9669

Do a hypothesis test of whether all of those higher-order polynomial
terms are jointly significant. Describe the pattern of predicted wage as
a function of age.

What if you used \(log(Age)\)? (And why would polynomials in
\(log(Age)\) be useless? Experiment.)

![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

    ## [1] 85445.97

Recall about how dummy variables work. If you added educ\_hs in a
regression using the subset given above, what would that do?
(Experiment, if you aren’t sure.) What is interpretation of coefficient
on *educ\_college* in that subset? What would happen if you put both
*educ\_college* and *educ\_advdeg* into a regression? Are your other
dummy variables in the regression working sensibly with your selection
criteria?

    ## 
    ## Call:
    ## lm(formula = (INCWAGE ~ AGE + I(AGE^2) + educ_college + educ_advdeg))
    ## 
    ## Coefficients:
    ##  (Intercept)           AGE      I(AGE^2)  educ_college   educ_advdeg  
    ##    -22289.85       5121.62        -46.25     -35171.72            NA

    ## 
    ## Call:
    ## lm(formula = (INCWAGE ~ AGE + I(AGE^2) + educ_advdeg + educ_college))
    ## 
    ## Coefficients:
    ##  (Intercept)           AGE      I(AGE^2)   educ_advdeg  educ_college  
    ##    -57461.57       5121.62        -46.25      35171.72            NA

Why don’t we use polynomial terms of dummy variables? Experiment.

What is the predicted wage, from your model, for a few relevant cases?
Do those seem reasonable?

    ##    AGE female educ_college educ_advdeg     yhat yhatploys  yhatlog
    ## 1   25      1            1           1 41671.93  38250.94 38951.67
    ## 2   26      1            1           1 44434.74  42335.88 42671.68
    ## 3   27      1            1           1 47105.05  46110.97 46135.20
    ## 4   28      1            1           1 49682.85  49586.86 49360.89
    ## 5   29      1            1           1 52168.15  52775.74 52365.44
    ## 6   30      1            1           1 54560.95  55691.13 55163.84
    ## 7   31      1            1           1 56861.25  58347.69 57769.63
    ## 8   32      1            1           1 59069.04  60761.07 60195.04
    ## 9   33      1            1           1 61184.34  62947.66 62451.16
    ## 10  34      1            1           1 63207.12  64924.46 64548.10
    ## 11  35      1            1           1 65137.41  66708.85 66495.10
    ## 12  36      1            1           1 66975.20  68318.42 68300.61
    ## 13  37      1            1           1 68720.48  69770.80 69972.37
    ## 14  38      1            1           1 70373.26  71083.42 71517.53
    ## 15  39      1            1           1 71933.53  72273.36 72942.66
    ## 16  40      1            1           1 73401.31  73357.18 74253.84
    ## 17  41      1            1           1 74776.58  74350.67 75456.70
    ## 18  42      1            1           1 76059.35  75268.72 76556.46
    ## 19  43      1            1           1 77249.62  76125.09 77557.96
    ## 20  44      1            1           1 78347.38  76932.26 78465.74
    ## 21  45      1            1           1 79352.65  77701.22 79284.00
    ## 22  46      1            1           1 80265.41  78441.26 80016.68
    ## 23  47      1            1           1 81085.66  79159.82 80667.46
    ## 24  48      1            1           1 81813.42  79862.30 81239.78
    ## 25  49      1            1           1 82448.67  80551.83 81736.89
    ## 26  50      1            1           1 82991.42  81229.13 82161.82
    ## 27  51      1            1           1 83441.67  81892.28 82517.44
    ## 28  52      1            1           1 83799.42  82536.58 82806.42
    ## 29  53      1            1           1 84064.66  83154.32 83031.32
    ## 30  54      1            1           1 84237.40  83734.59 83194.53
    ## 31  55      1            1           1 84317.64  84263.13 83298.31
    ## 32  56      1            1           1 84305.38  84722.11 83344.81
    ## 33  57      1            1           1 84200.61  85089.94 83336.04
    ## 34  58      1            1           1 84003.34  85341.11 83273.95
    ## 35  59      1            1           1 83713.57  85445.97 83160.33
    ## 36  60      1            1           1 83331.30  85370.56 82996.94
    ## 37  61      1            1           1 82856.52  85076.41 82785.42
    ## 38  62      1            1           1 82289.24  84520.36 82527.32
    ## 39  63      1            1           1 81629.46  83654.38 82224.15
    ## 40  64      1            1           1 80877.18  82425.34 81877.31
    ## 41  65      1            1           1 80032.39  80774.90 81488.17
    ## 42  66      1            1           1 79095.10  78639.22 81058.02
    ## 43  67      1            1           1 78065.31  75948.86 80588.09
    ## 44  68      1            1           1 76943.02  72628.55 80079.56
    ## 45  69      1            1           1 75728.22  68597.01 79533.55
    ## 46  70      1            1           1 74420.93  63766.75 78951.15

    ##    AGE female educ_college educ_advdeg      yhat
    ## 1   25      1            0           1  76843.65
    ## 2   26      1            0           1  79606.46
    ## 3   27      1            0           1  82276.77
    ## 4   28      1            0           1  84854.57
    ## 5   29      1            0           1  87339.87
    ## 6   30      1            0           1  89732.67
    ## 7   31      1            0           1  92032.97
    ## 8   32      1            0           1  94240.76
    ## 9   33      1            0           1  96356.05
    ## 10  34      1            0           1  98378.84
    ## 11  35      1            0           1 100309.13
    ## 12  36      1            0           1 102146.92
    ## 13  37      1            0           1 103892.20
    ## 14  38      1            0           1 105544.98
    ## 15  39      1            0           1 107105.25
    ## 16  40      1            0           1 108573.03
    ## 17  41      1            0           1 109948.30
    ## 18  42      1            0           1 111231.07
    ## 19  43      1            0           1 112421.34
    ## 20  44      1            0           1 113519.10
    ## 21  45      1            0           1 114524.37
    ## 22  46      1            0           1 115437.13
    ## 23  47      1            0           1 116257.38
    ## 24  48      1            0           1 116985.14
    ## 25  49      1            0           1 117620.39
    ## 26  50      1            0           1 118163.14
    ## 27  51      1            0           1 118613.39
    ## 28  52      1            0           1 118971.14
    ## 29  53      1            0           1 119236.38
    ## 30  54      1            0           1 119409.12
    ## 31  55      1            0           1 119489.36
    ## 32  56      1            0           1 119477.10
    ## 33  57      1            0           1 119372.33
    ## 34  58      1            0           1 119175.06
    ## 35  59      1            0           1 118885.29
    ## 36  60      1            0           1 118503.02
    ## 37  61      1            0           1 118028.24
    ## 38  62      1            0           1 117460.96
    ## 39  63      1            0           1 116801.18
    ## 40  64      1            0           1 116048.90
    ## 41  65      1            0           1 115204.11
    ## 42  66      1            0           1 114266.82
    ## 43  67      1            0           1 113237.03
    ## 44  68      1            0           1 112114.74
    ## 45  69      1            0           1 110899.94
    ## 46  70      1            0           1 109592.64

    ##    AGE female educ_college educ_advdeg     yhat
    ## 1   25      1            1           0 41671.93
    ## 2   26      1            1           0 44434.74
    ## 3   27      1            1           0 47105.05
    ## 4   28      1            1           0 49682.85
    ## 5   29      1            1           0 52168.15
    ## 6   30      1            1           0 54560.95
    ## 7   31      1            1           0 56861.25
    ## 8   32      1            1           0 59069.04
    ## 9   33      1            1           0 61184.34
    ## 10  34      1            1           0 63207.12
    ## 11  35      1            1           0 65137.41
    ## 12  36      1            1           0 66975.20
    ## 13  37      1            1           0 68720.48
    ## 14  38      1            1           0 70373.26
    ## 15  39      1            1           0 71933.53
    ## 16  40      1            1           0 73401.31
    ## 17  41      1            1           0 74776.58
    ## 18  42      1            1           0 76059.35
    ## 19  43      1            1           0 77249.62
    ## 20  44      1            1           0 78347.38
    ## 21  45      1            1           0 79352.65
    ## 22  46      1            1           0 80265.41
    ## 23  47      1            1           0 81085.66
    ## 24  48      1            1           0 81813.42
    ## 25  49      1            1           0 82448.67
    ## 26  50      1            1           0 82991.42
    ## 27  51      1            1           0 83441.67
    ## 28  52      1            1           0 83799.42
    ## 29  53      1            1           0 84064.66
    ## 30  54      1            1           0 84237.40
    ## 31  55      1            1           0 84317.64
    ## 32  56      1            1           0 84305.38
    ## 33  57      1            1           0 84200.61
    ## 34  58      1            1           0 84003.34
    ## 35  59      1            1           0 83713.57
    ## 36  60      1            1           0 83331.30
    ## 37  61      1            1           0 82856.52
    ## 38  62      1            1           0 82289.24
    ## 39  63      1            1           0 81629.46
    ## 40  64      1            1           0 80877.18
    ## 41  65      1            1           0 80032.39
    ## 42  66      1            1           0 79095.10
    ## 43  67      1            1           0 78065.31
    ## 44  68      1            1           0 76943.02
    ## 45  69      1            1           0 75728.22
    ## 46  70      1            1           0 74420.93

What is difference in regression from using log wage as the dependent
variable? Compare the pattern of predicted values from the two models
(remember to take exp() of the predicted value, where the dependent is
log wage). Discuss.

    ##    AGE female educ_college educ_advdeg     yhat yhatploys  yhatlog
    ## 1   25      1            1           1 41671.93  38250.94 38951.67
    ## 2   26      1            1           1 44434.74  42335.88 42671.68
    ## 3   27      1            1           1 47105.05  46110.97 46135.20
    ## 4   28      1            1           1 49682.85  49586.86 49360.89
    ## 5   29      1            1           1 52168.15  52775.74 52365.44
    ## 6   30      1            1           1 54560.95  55691.13 55163.84
    ## 7   31      1            1           1 56861.25  58347.69 57769.63
    ## 8   32      1            1           1 59069.04  60761.07 60195.04
    ## 9   33      1            1           1 61184.34  62947.66 62451.16
    ## 10  34      1            1           1 63207.12  64924.46 64548.10
    ## 11  35      1            1           1 65137.41  66708.85 66495.10
    ## 12  36      1            1           1 66975.20  68318.42 68300.61
    ## 13  37      1            1           1 68720.48  69770.80 69972.37
    ## 14  38      1            1           1 70373.26  71083.42 71517.53
    ## 15  39      1            1           1 71933.53  72273.36 72942.66
    ## 16  40      1            1           1 73401.31  73357.18 74253.84
    ## 17  41      1            1           1 74776.58  74350.67 75456.70
    ## 18  42      1            1           1 76059.35  75268.72 76556.46
    ## 19  43      1            1           1 77249.62  76125.09 77557.96
    ## 20  44      1            1           1 78347.38  76932.26 78465.74
    ## 21  45      1            1           1 79352.65  77701.22 79284.00
    ## 22  46      1            1           1 80265.41  78441.26 80016.68
    ## 23  47      1            1           1 81085.66  79159.82 80667.46
    ## 24  48      1            1           1 81813.42  79862.30 81239.78
    ## 25  49      1            1           1 82448.67  80551.83 81736.89
    ## 26  50      1            1           1 82991.42  81229.13 82161.82
    ## 27  51      1            1           1 83441.67  81892.28 82517.44
    ## 28  52      1            1           1 83799.42  82536.58 82806.42
    ## 29  53      1            1           1 84064.66  83154.32 83031.32
    ## 30  54      1            1           1 84237.40  83734.59 83194.53
    ## 31  55      1            1           1 84317.64  84263.13 83298.31
    ## 32  56      1            1           1 84305.38  84722.11 83344.81
    ## 33  57      1            1           1 84200.61  85089.94 83336.04
    ## 34  58      1            1           1 84003.34  85341.11 83273.95
    ## 35  59      1            1           1 83713.57  85445.97 83160.33
    ## 36  60      1            1           1 83331.30  85370.56 82996.94
    ## 37  61      1            1           1 82856.52  85076.41 82785.42
    ## 38  62      1            1           1 82289.24  84520.36 82527.32
    ## 39  63      1            1           1 81629.46  83654.38 82224.15
    ## 40  64      1            1           1 80877.18  82425.34 81877.31
    ## 41  65      1            1           1 80032.39  80774.90 81488.17
    ## 42  66      1            1           1 79095.10  78639.22 81058.02
    ## 43  67      1            1           1 78065.31  75948.86 80588.09
    ## 44  68      1            1           1 76943.02  72628.55 80079.56
    ## 45  69      1            1           1 75728.22  68597.01 79533.55
    ## 46  70      1            1           1 74420.93  63766.75 78951.15

    ## [1] 73309.33

    ## [1] 73658.94

Try some interactions, like this,

    ## 
    ## Call:
    ## lm(formula = INCWAGE ~ AGE + I(AGE^2) + educ_college + I(educ_college * 
    ##     AGE) + I(educ_college * (AGE^2)))
    ## 
    ## Coefficients:
    ##               (Intercept)                        AGE  
    ##                 -98495.49                    8058.95  
    ##                  I(AGE^2)               educ_college  
    ##                    -71.89                   92301.75  
    ##     I(educ_college * AGE)  I(educ_college * (AGE^2))  
    ##                  -4863.83                      41.81

    ## 
    ## Call:
    ## lm(formula = INCWAGE ~ AGE + I(AGE^2) + educ_college + I(educ_college * 
    ##     AGE) + educ_advdeg + I(educ_advdeg * AGE))
    ## 
    ## Coefficients:
    ##           (Intercept)                    AGE               I(AGE^2)  
    ##             -54253.84                5971.18                 -49.01  
    ##          educ_college  I(educ_college * AGE)            educ_advdeg  
    ##              12721.32               -1071.56                     NA  
    ##  I(educ_advdeg * AGE)  
    ##                    NA

and explain those outputs (different peaks for different groups).

What are the other variables you are using in your regression? Do they
have the expected signs and patterns of significance? Explain if there
is a plausible causal link from X variables to Y and not the reverse.
Explain your results, giving details about the estimation, some
predicted values, and providing any relevant graphics. Impress.

### Bibliography

<https://cps.ipums.org/cps-action/variables/INCWAGE#description_section>
