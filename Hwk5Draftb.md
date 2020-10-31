Hwk 5
================
Patrick Sinclair, Kieran Yuen

For this lab, we improve some of our regression models to explain wages.

Work with a group to prepare a 4-min presentation by one of the group
members about their experiment process and results. You get 45 min to
prepare.

Build on the previous lab in creating useful models.

Concentrate on a smaller subset than previous. For instance if you
wanted to look at wages for Hispanic women with at least a college
degree, you might use

``` r
load("acs2017_ny_data.RData")
attach(acs2017_ny)
healthcare <- (IND == "7970" | IND == "7980" | IND == "7990" | IND == "8070" | IND == "8080" | IND == "8090" | IND == "8170" | IND == "8180" | IND == "8190" | IND == "8270")
use_varb <- (AGE >= 25) & (AGE <= 70) & (LABFORCE == 2) & (WKSWORK2 > 4) & (UHRSWORK >= 35) & (female == 1) & ((educ_college == 1) | (educ_advdeg == 1)) & healthcare
dat_med <- subset(acs2017_ny, use_varb) 
detach()
```

``` r
attach(dat_med)
summary(dat_med)
```

    ##       AGE            female    educ_nohs    educ_hs  educ_somecoll
    ##  Min.   :25.00   Min.   :1   Min.   :0   Min.   :0   Min.   :0    
    ##  1st Qu.:33.00   1st Qu.:1   1st Qu.:0   1st Qu.:0   1st Qu.:0    
    ##  Median :45.00   Median :1   Median :0   Median :0   Median :0    
    ##  Mean   :44.69   Mean   :1   Mean   :0   Mean   :0   Mean   :0    
    ##  3rd Qu.:55.00   3rd Qu.:1   3rd Qu.:0   3rd Qu.:0   3rd Qu.:0    
    ##  Max.   :70.00   Max.   :1   Max.   :0   Max.   :0   Max.   :0    
    ##                                                                   
    ##   educ_college     educ_advdeg                   SCHOOL    
    ##  Min.   :0.0000   Min.   :0.0000   N/A              :   0  
    ##  1st Qu.:0.0000   1st Qu.:0.0000   No, not in school:2431  
    ##  Median :1.0000   Median :0.0000   Yes, in school   : 195  
    ##  Mean   :0.5381   Mean   :0.4619   Missing          :   0  
    ##  3rd Qu.:1.0000   3rd Qu.:1.0000                           
    ##  Max.   :1.0000   Max.   :1.0000                           
    ##                                                            
    ##                         EDUC     
    ##  4 years of college       :1413  
    ##  5+ years of college      :1213  
    ##  N/A or no schooling      :   0  
    ##  Nursery school to grade 4:   0  
    ##  Grade 5, 6, 7, or 8      :   0  
    ##  Grade 9                  :   0  
    ##  (Other)                  :   0  
    ##                                             EDUCD     
    ##  Bachelor's degree                             :1413  
    ##  Master's degree                               : 719  
    ##  Professional degree beyond a bachelor's degree: 363  
    ##  Doctoral degree                               : 131  
    ##  N/A or no schooling                           :   0  
    ##  N/A                                           :   0  
    ##  (Other)                                       :   0  
    ##                                      DEGFIELD   
    ##  Medical and Health Sciences and Services:1068  
    ##  Biology and Life Sciences               : 262  
    ##  Psychology                              : 248  
    ##  Business                                : 240  
    ##  Social Sciences                         : 135  
    ##  Education Administration and Teaching   : 126  
    ##  (Other)                                 : 547  
    ##                                   DEGFIELDD   
    ##  Nursing                               : 730  
    ##  Psychology                            : 231  
    ##  Biology                               : 195  
    ##  Treatment Therapy Professions         :  89  
    ##  Business Management and Administration:  73  
    ##  Social Work                           :  65  
    ##  (Other)                               :1243  
    ##                                     DEGFIELD2   
    ##  N/A                                     :2358  
    ##  Medical and Health Sciences and Services: 102  
    ##  Social Sciences                         :  28  
    ##  Psychology                              :  21  
    ##  Business                                :  19  
    ##  Biology and Life Sciences               :  14  
    ##  (Other)                                 :  84  
    ##                                    DEGFIELD2D        PUMA            GQ       
    ##  N/A                                    :2358   Min.   : 100   Min.   :1.000  
    ##  Nursing                                :  63   1st Qu.:2202   1st Qu.:1.000  
    ##  Psychology                             :  19   Median :3211   Median :1.000  
    ##  Sociology                              :  10   Mean   :2935   Mean   :1.005  
    ##  Biology                                :   9   3rd Qu.:4004   3rd Qu.:1.000  
    ##  Health and Medical Preparatory Programs:   8   Max.   :4114   Max.   :5.000  
    ##  (Other)                                : 159                                 
    ##     OWNERSHP       OWNERSHPD        MORTGAGE        OWNCOST           RENT     
    ##  Min.   :0.000   Min.   : 0.00   Min.   :0.000   Min.   :    0   Min.   :   0  
    ##  1st Qu.:1.000   1st Qu.:13.00   1st Qu.:0.000   1st Qu.: 1479   1st Qu.:   0  
    ##  Median :1.000   Median :13.00   Median :3.000   Median : 2816   Median :   0  
    ##  Mean   :1.266   Mean   :15.21   Mean   :1.863   Mean   :28436   Mean   : 410  
    ##  3rd Qu.:2.000   3rd Qu.:22.00   3rd Qu.:3.000   3rd Qu.:99999   3rd Qu.: 550  
    ##  Max.   :2.000   Max.   :22.00   Max.   :4.000   Max.   :99999   Max.   :3800  
    ##                                                                                
    ##     COSTELEC       COSTGAS        COSTWATR       COSTFUEL       HHINCOME      
    ##  Min.   :   0   Min.   :   0   Min.   :   0   Min.   :   0   Min.   :   2600  
    ##  1st Qu.:1080   1st Qu.: 960   1st Qu.: 350   1st Qu.:9993   1st Qu.:  93400  
    ##  Median :1680   Median :2760   Median :1000   Median :9993   Median : 140000  
    ##  Mean   :2263   Mean   :5115   Mean   :4299   Mean   :8683   Mean   : 179328  
    ##  3rd Qu.:2490   3rd Qu.:9993   3rd Qu.:9993   3rd Qu.:9993   3rd Qu.: 205738  
    ##  Max.   :9997   Max.   :9997   Max.   :9997   Max.   :9997   Max.   :2030000  
    ##                                                              NA's   :2        
    ##     FOODSTMP        LINGISOL         ROOMS           BUILTYR2     
    ##  Min.   :1.000   Min.   :0.000   Min.   : 0.000   Min.   : 0.000  
    ##  1st Qu.:1.000   1st Qu.:1.000   1st Qu.: 4.000   1st Qu.: 1.000  
    ##  Median :1.000   Median :1.000   Median : 6.000   Median : 3.000  
    ##  Mean   :1.034   Mean   :1.038   Mean   : 6.169   Mean   : 4.013  
    ##  3rd Qu.:1.000   3rd Qu.:1.000   3rd Qu.: 8.000   3rd Qu.: 5.000  
    ##  Max.   :2.000   Max.   :2.000   Max.   :16.000   Max.   :22.000  
    ##                                                                   
    ##     UNITSSTR         FUELHEAT          SSMC            FAMSIZE     
    ##  Min.   : 0.000   Min.   :0.000   Min.   :0.00000   Min.   : 1.00  
    ##  1st Qu.: 3.000   1st Qu.:2.000   1st Qu.:0.00000   1st Qu.: 2.00  
    ##  Median : 3.000   Median :2.000   Median :0.00000   Median : 3.00  
    ##  Mean   : 4.716   Mean   :2.915   Mean   :0.01295   Mean   : 2.89  
    ##  3rd Qu.: 6.000   3rd Qu.:4.000   3rd Qu.:0.00000   3rd Qu.: 4.00  
    ##  Max.   :10.000   Max.   :9.000   Max.   :2.00000   Max.   :12.00  
    ##                                                                    
    ##      NCHILD           NCHLT5           RELATE          RELATED      
    ##  Min.   :0.0000   Min.   :0.0000   Min.   : 1.000   Min.   : 101.0  
    ##  1st Qu.:0.0000   1st Qu.:0.0000   1st Qu.: 1.000   1st Qu.: 101.0  
    ##  Median :0.0000   Median :0.0000   Median : 1.000   Median : 101.0  
    ##  Mean   :0.7986   Mean   :0.1424   Mean   : 2.275   Mean   : 229.8  
    ##  3rd Qu.:2.0000   3rd Qu.:0.0000   3rd Qu.: 2.000   3rd Qu.: 201.0  
    ##  Max.   :8.0000   Max.   :3.0000   Max.   :12.000   Max.   :1270.0  
    ##                                                                     
    ##      MARST           RACE          RACED           HISPAN      
    ##  Min.   :1.00   Min.   :1.00   Min.   :100.0   Min.   :0.0000  
    ##  1st Qu.:1.00   1st Qu.:1.00   1st Qu.:100.0   1st Qu.:0.0000  
    ##  Median :1.00   Median :1.00   Median :100.0   Median :0.0000  
    ##  Mean   :2.84   Mean   :2.19   Mean   :221.6   Mean   :0.2628  
    ##  3rd Qu.:6.00   3rd Qu.:2.00   3rd Qu.:200.0   3rd Qu.:0.0000  
    ##  Max.   :6.00   Max.   :9.00   Max.   :976.0   Max.   :4.0000  
    ##                                                                
    ##     HISPAND                      BPL                 BPLD     
    ##  Min.   :  0.00   New York         :1386   New York    :1386  
    ##  1st Qu.:  0.00   West Indies      : 174   Philippines : 128  
    ##  Median :  0.00   Philippines      : 128   India       :  71  
    ##  Mean   : 28.35   Other USSR/Russia: 111   Jamaica     :  58  
    ##  3rd Qu.:  0.00   India            :  97   Pennsylvania:  49  
    ##  Max.   :498.00   China            :  65   China       :  46  
    ##                   (Other)          : 665   (Other)     : 888  
    ##                      ANCESTR1                             ANCESTR1D   
    ##  Not Reported            : 281   Not Reported                  : 281  
    ##  Italian                 : 271   Italian (1990-2000, ACS, PRCS): 271  
    ##  Irish, various subheads,: 230   Irish                         : 223  
    ##  German                  : 159   German (1990-2000, ACS/PRCS)  : 159  
    ##  Filipino                : 117   Filipino                      : 117  
    ##  Polish                  : 113   Polish                        : 113  
    ##  (Other)                 :1455   (Other)                       :1462  
    ##          ANCESTR2                             ANCESTR2D       CITIZEN      
    ##  Not Reported:1859   Not Reported                  :1859   Min.   :0.0000  
    ##  Irish       : 142   Irish                         : 129   1st Qu.:0.0000  
    ##  German      : 124   German (1990-2000, ACS, PRCS) : 124   Median :0.0000  
    ##  English     :  69   English                       :  69   Mean   :0.7041  
    ##  Italian     :  55   Italian (1990-2000, ACS, PRCS):  55   3rd Qu.:2.0000  
    ##  Polish      :  33   Polish                        :  33   Max.   :3.0000  
    ##  (Other)     : 344   (Other)                       : 357                   
    ##     YRSUSA1          HCOVANY         HCOVPRIV         SEX          EMPSTAT     
    ##  Min.   : 0.000   Min.   :1.000   Min.   :1.000   Male  :   0   Min.   :1.000  
    ##  1st Qu.: 0.000   1st Qu.:2.000   1st Qu.:2.000   Female:2626   1st Qu.:1.000  
    ##  Median : 0.000   Median :2.000   Median :2.000                 Median :1.000  
    ##  Mean   : 7.823   Mean   :1.979   Mean   :1.946                 Mean   :1.002  
    ##  3rd Qu.:14.000   3rd Qu.:2.000   3rd Qu.:2.000                 3rd Qu.:1.000  
    ##  Max.   :68.000   Max.   :2.000   Max.   :2.000                 Max.   :2.000  
    ##                                                                                
    ##     EMPSTATD        LABFORCE      OCC            IND          CLASSWKR    
    ##  Min.   :10.00   Min.   :2   3255   : 782   8190   :1585   Min.   :1.000  
    ##  1st Qu.:10.00   1st Qu.:2   3060   : 257   7970   : 260   1st Qu.:2.000  
    ##  Median :10.00   Median :2   350    : 172   8090   : 202   Median :2.000  
    ##  Mean   :10.04   Mean   :2   2010   : 125   8270   : 165   Mean   :1.954  
    ##  3rd Qu.:10.00   3rd Qu.:2   3258   :  90   8170   : 159   3rd Qu.:2.000  
    ##  Max.   :20.00   Max.   :2   3600   :  86   8180   : 119   Max.   :2.000  
    ##                              (Other):1114   (Other): 136                  
    ##    CLASSWKRD        WKSWORK2        UHRSWORK         INCTOT      
    ##  Min.   :13.00   Min.   :5.000   Min.   :35.00   Min.   :  2600  
    ##  1st Qu.:22.00   1st Qu.:6.000   1st Qu.:40.00   1st Qu.: 52000  
    ##  Median :22.00   Median :6.000   Median :40.00   Median : 75000  
    ##  Mean   :22.59   Mean   :5.976   Mean   :42.84   Mean   : 93669  
    ##  3rd Qu.:23.00   3rd Qu.:6.000   3rd Qu.:45.00   3rd Qu.:104000  
    ##  Max.   :28.00   Max.   :6.000   Max.   :99.00   Max.   :967000  
    ##                                                                  
    ##     FTOTINC           INCWAGE          POVERTY         MIGRATE1    
    ##  Min.   :   2500   Min.   :     0   Min.   :  0.0   Min.   :1.000  
    ##  1st Qu.:  82390   1st Qu.: 50000   1st Qu.:466.2   1st Qu.:1.000  
    ##  Median : 130400   Median : 74000   Median :501.0   Median :1.000  
    ##  Mean   : 168766   Mean   : 88171   Mean   :457.1   Mean   :1.114  
    ##  3rd Qu.: 198075   3rd Qu.:100000   3rd Qu.:501.0   3rd Qu.:1.000  
    ##  Max.   :2030000   Max.   :638000   Max.   :501.0   Max.   :4.000  
    ##  NA's   :2                                                         
    ##    MIGRATE1D        MIGPLAC1         MIGCOUNTY1         MIGPUMA1     
    ##  Min.   :10.00   Min.   :  0.000   Min.   :  0.000   Min.   :   0.0  
    ##  1st Qu.:10.00   1st Qu.:  0.000   1st Qu.:  0.000   1st Qu.:   0.0  
    ##  Median :10.00   Median :  0.000   Median :  0.000   Median :   0.0  
    ##  Mean   :11.41   Mean   :  4.939   Mean   :  3.907   Mean   : 257.7  
    ##  3rd Qu.:10.00   3rd Qu.:  0.000   3rd Qu.:  0.000   3rd Qu.:   0.0  
    ##  Max.   :40.00   Max.   :900.000   Max.   :109.000   Max.   :9500.0  
    ##                                                                      
    ##     VETSTAT         VETSTATD        PWPUMA00       TRANWORK        TRANTIME    
    ##  Min.   :1.000   Min.   :11.00   Min.   :   0   Min.   : 0.00   Min.   :  0.0  
    ##  1st Qu.:1.000   1st Qu.:11.00   1st Qu.:2000   1st Qu.:10.00   1st Qu.: 15.0  
    ##  Median :1.000   Median :11.00   Median :3300   Median :10.00   Median : 30.0  
    ##  Mean   :1.007   Mean   :11.07   Mean   :2888   Mean   :18.26   Mean   : 34.5  
    ##  3rd Qu.:1.000   3rd Qu.:11.00   3rd Qu.:3800   3rd Qu.:33.00   3rd Qu.: 45.0  
    ##  Max.   :2.000   Max.   :20.00   Max.   :4100   Max.   :70.00   Max.   :138.0  
    ##                                                                                
    ##     DEPARTS           in_NYC          in_Bronx        in_Manhattan    
    ##  Min.   :   0.0   Min.   :0.0000   Min.   :0.00000   Min.   :0.00000  
    ##  1st Qu.: 642.0   1st Qu.:0.0000   1st Qu.:0.00000   1st Qu.:0.00000  
    ##  Median : 732.0   Median :0.0000   Median :0.00000   Median :0.00000  
    ##  Mean   : 806.7   Mean   :0.4117   Mean   :0.05484   Mean   :0.06474  
    ##  3rd Qu.: 825.8   3rd Qu.:1.0000   3rd Qu.:0.00000   3rd Qu.:0.00000  
    ##  Max.   :2345.0   Max.   :1.0000   Max.   :1.00000   Max.   :1.00000  
    ##                                                                       
    ##    in_StatenI      in_Brooklyn       in_Queens      in_Westchester   
    ##  Min.   :0.0000   Min.   :0.0000   Min.   :0.0000   Min.   :0.00000  
    ##  1st Qu.:0.0000   1st Qu.:0.0000   1st Qu.:0.0000   1st Qu.:0.00000  
    ##  Median :0.0000   Median :0.0000   Median :0.0000   Median :0.00000  
    ##  Mean   :0.0278   Mean   :0.1299   Mean   :0.1344   Mean   :0.06626  
    ##  3rd Qu.:0.0000   3rd Qu.:0.0000   3rd Qu.:0.0000   3rd Qu.:0.00000  
    ##  Max.   :1.0000   Max.   :1.0000   Max.   :1.0000   Max.   :1.00000  
    ##                                                                      
    ##    in_Nassau         Hispanic          Hisp_Mex           Hisp_PR       
    ##  Min.   :0.0000   Min.   :0.00000   Min.   :0.000000   Min.   :0.00000  
    ##  1st Qu.:0.0000   1st Qu.:0.00000   1st Qu.:0.000000   1st Qu.:0.00000  
    ##  Median :0.0000   Median :0.00000   Median :0.000000   Median :0.00000  
    ##  Mean   :0.1101   Mean   :0.08682   Mean   :0.006474   Mean   :0.03161  
    ##  3rd Qu.:0.0000   3rd Qu.:0.00000   3rd Qu.:0.000000   3rd Qu.:0.00000  
    ##  Max.   :1.0000   Max.   :1.00000   Max.   :1.000000   Max.   :1.00000  
    ##                                                                         
    ##    Hisp_Cuban         Hisp_DomR          white             AfAm       
    ##  Min.   :0.000000   Min.   :0.0000   Min.   :0.0000   Min.   :0.0000  
    ##  1st Qu.:0.000000   1st Qu.:0.0000   1st Qu.:0.0000   1st Qu.:0.0000  
    ##  Median :0.000000   Median :0.0000   Median :1.0000   Median :0.0000  
    ##  Mean   :0.001904   Mean   :0.0179   Mean   :0.6508   Mean   :0.1348  
    ##  3rd Qu.:0.000000   3rd Qu.:0.0000   3rd Qu.:1.0000   3rd Qu.:0.0000  
    ##  Max.   :1.000000   Max.   :1.0000   Max.   :1.0000   Max.   :1.0000  
    ##                                                                       
    ##     Amindian            Asian           race_oth        unmarried     
    ##  Min.   :0.000000   Min.   :0.0000   Min.   :0.0000   Min.   :0.0000  
    ##  1st Qu.:0.000000   1st Qu.:0.0000   1st Qu.:0.0000   1st Qu.:0.0000  
    ##  Median :0.000000   Median :0.0000   Median :0.0000   Median :0.0000  
    ##  Mean   :0.001904   Mean   :0.1641   Mean   :0.1721   Mean   :0.2673  
    ##  3rd Qu.:0.000000   3rd Qu.:0.0000   3rd Qu.:0.0000   3rd Qu.:1.0000  
    ##  Max.   :1.000000   Max.   :1.0000   Max.   :1.0000   Max.   :1.0000  
    ##                                                                       
    ##     veteran         has_AnyHealthIns has_PvtHealthIns  Commute_car    
    ##  Min.   :0.000000   Min.   :0.0000   Min.   :0.0000   Min.   :0.0000  
    ##  1st Qu.:0.000000   1st Qu.:1.0000   1st Qu.:1.0000   1st Qu.:0.0000  
    ##  Median :0.000000   Median :1.0000   Median :1.0000   Median :1.0000  
    ##  Mean   :0.007235   Mean   :0.9794   Mean   :0.9459   Mean   :0.6858  
    ##  3rd Qu.:0.000000   3rd Qu.:1.0000   3rd Qu.:1.0000   3rd Qu.:1.0000  
    ##  Max.   :1.000000   Max.   :1.0000   Max.   :1.0000   Max.   :1.0000  
    ##                                                                       
    ##   Commute_bus      Commute_subway    Commute_rail     Commute_other    
    ##  Min.   :0.00000   Min.   :0.0000   Min.   :0.00000   Min.   :0.00000  
    ##  1st Qu.:0.00000   1st Qu.:0.0000   1st Qu.:0.00000   1st Qu.:0.00000  
    ##  Median :0.00000   Median :0.0000   Median :0.00000   Median :0.00000  
    ##  Mean   :0.04417   Mean   :0.1542   Mean   :0.02704   Mean   :0.07388  
    ##  3rd Qu.:0.00000   3rd Qu.:0.0000   3rd Qu.:0.00000   3rd Qu.:0.00000  
    ##  Max.   :1.00000   Max.   :1.0000   Max.   :1.00000   Max.   :1.00000  
    ##                                                                        
    ##  below_povertyline  below_150poverty  below_200poverty    foodstamps     
    ##  Min.   :0.000000   Min.   :0.00000   Min.   :0.00000   Min.   :0.00000  
    ##  1st Qu.:0.000000   1st Qu.:0.00000   1st Qu.:0.00000   1st Qu.:0.00000  
    ##  Median :0.000000   Median :0.00000   Median :0.00000   Median :0.00000  
    ##  Mean   :0.005712   Mean   :0.01637   Mean   :0.03618   Mean   :0.03351  
    ##  3rd Qu.:0.000000   3rd Qu.:0.00000   3rd Qu.:0.00000   3rd Qu.:0.00000  
    ##  Max.   :1.000000   Max.   :1.00000   Max.   :1.00000   Max.   :1.00000  
    ## 

``` r
detach()
```

``` r
# creating different objects for work departure time
# want to create different objects within industry to subset office workers, hospitals, nursing facilities etc
attach(dat_med)
DEPARTS
```

    ##    [1]  802 1135  732  727  832  602  717  717  702  702  817 1415  612  622
    ##   [15]  702  732 1205  737  632  602  702  627  647  822  732  712    0  602
    ##   [29]  702  832  732  802  732  647  822  702  702  632  632  647  902  742
    ##   [43]  817  702  702 1035  732  722  802  732 1835  822  802  632  902  802
    ##   [57]  732  602  702  832  717  802  702  702  627  832  802  932  717  832
    ##   [71]  527  732  732  732  717  732  832  727  702  752 1435  702  702 1835
    ##   [85]  802  702  732  445  602 1715  617  747    0  145    0  902  747  632
    ##   [99]  717 1505    0  802  707  517  802  602  712  802  832  717  642  632
    ##  [113]  702    0  802  832  702  732 1835  517  702  902  732  732  732  802
    ##  [127]  902 1105  732  602 1335 1215  832  632  747  602  702  832  932 1915
    ##  [141]  712  717  902  647  802  632  802  632  647  647  647  617  702    0
    ##  [155]  602  747  747  732  702  632 1155  702  702  602  732  717  747  802
    ##  [169]  632  602  917  602  802  832  532  405 1805  702  802  732  802  902
    ##  [183]  802  702  632  702  612  702  507  647  647  632  702  742  642  747
    ##  [197]  717  647 1835  657  802  702  502  702  732  732 2215  802  747  602
    ##  [211]  632  602 1005  802 1005 1835  702  902  617  747 1805   15  802 1645
    ##  [225]  737  802  932 1635  702    0  617  602  802  732 1435  632  632  632
    ##  [239]  817  727  602  832  802  702  512  752  702  647  732  717 1505  832
    ##  [253]  802    0  727  717  732  702 1135  632  602 2315  702  802  517  702
    ##  [267]  822  902  902 1835  747  802  602  732  532  802  622  612  702  802
    ##  [281]  732 1105  812    0  702  732  627  702  802  632  245 1805  532  732
    ##  [295] 1805 1005  622  712  732  702 1805  647  802    0  702  802  702  822
    ##  [309]  702  802  832  747    0  602 2345  602    0  647  632  552  732  732
    ##  [323]  802  752  405  722  802  732  732  632  732 2105  632 1405  647  632
    ##  [337]  717  832  632  902  847  722  832  802 1755  547  632 1205  832  802
    ##  [351]  622  852    0 1805 1435  632  642 1855    0    0  532  602  732  602
    ##  [365]  702  817  722  832 1305  717  632  652  632  647  657  727  602  822
    ##  [379]  742  732  647    0  702  632  717  902  435  702  802  602  802  732
    ##  [393] 1835  552  632  622 1135  822  632 2245  802  612 1835  632  532  702
    ##  [407]  702  702  602  747  802  502  802 1305 1845  702  532  702  802  817
    ##  [421]  917  717  602  602  647  832  817  602 1835 2235  702  852  602  902
    ##  [435]  702  802  802  817  145 1445    0  612  902    0  602    0  747  702
    ##  [449]  532  717  647 1835  702 1835  732  902  717  717  702  732  657 1745
    ##  [463]  632  632 1805 1125  652  802  602  702  502  802 1745  732  732  732
    ##  [477]  832  712  702  732 1405  802  802  832  617  742  702  802 1915  732
    ##  [491]  602 1335  502 1005  802    0  602  802  647  702  702  802  717  807
    ##  [505]  702  732    0    0  602  617  602  527 1315  802 1305  802  632  702
    ##  [519] 1835  752   15  732  632  932    0  702  832  632 1805  642  832  707
    ##  [533]  802  707  517  802  732 1405  802  717  832   15  742  717  602  847
    ##  [547]  732  747  832  702  702  702  602  732  647  647  647  812  802  732
    ##  [561]  717  732  732  632  632  832  647 1505  917 1805  702  742  802  602
    ##  [575]  902  532  802  547  702  732    0  817    0 1735  732  802  632  547
    ##  [589]  802  642  742  802  632 1535  802  752  532  802  802 1815  732  722
    ##  [603]  822  932   15  732  702  702  602  722 1155  717  632  632 1335  612
    ##  [617]  602  632  832  527  817 1735  632  702  732  802  647  702  932  917
    ##  [631]  512  647 1005  732 1425  732  732  732  632  717  617  547  632 1005
    ##  [645]  802  932  717  702  602  802  802  602  747  502  842  632  602  802
    ##  [659]  902  702  802  802  647  632    0  747  702  822  502  717  702  802
    ##  [673]  902  642  702  817  652  547  817  752  847 1945  732  732  747  702
    ##  [687]  732  802  702  732  532  602  832  632  847  702  632 1035  847    0
    ##  [701]  932  717  732  702  732  732  702  702  722  547  832  747  702  832
    ##  [715]  807 1705 1515  717  832 1835  702  802  702  732  732  752 2105  752
    ##  [729]  737  812  632  827  732  717  717  747  702  732  732  732  717  802
    ##  [743]  617  832  547  802  802  602 1545  652  702  702 1915  702  832  802
    ##  [757]  702  732  702 1835  832  832  702  722  902  647  617  732 1105  652
    ##  [771]  702  832 1035 1845  832 1005  802  802  607  522  802 1235  632  732
    ##  [785]  702  547  702  602  747  802  517 1805  832  902  802  737 1405 2245
    ##  [799]  732  657  632  632 1805  602  632  702  647  647  702  602 1105  722
    ##  [813]  832  732  702  647  702  802  652  802  657  817 1005  732  502  607
    ##  [827]  702 2235  712  602 1735  602 1105    0  852  632  822  717 1105  832
    ##  [841]  622  652  612  802  842 1005  702  747    0  932  632  717  702  405
    ##  [855]  702 1005  832  817  632  602  547 1015  722  702  702  732 1405  802
    ##  [869]  802  802  732  717  812  747  632 1635  802  632  832  702  717  502
    ##  [883] 1535  717  802  445  557  702  802 1205  732  502  702  722  822  832
    ##  [897]  702  802  647  902  732 1535  602  732 2235  932  632  802 2045  647
    ##  [911]  802  817 1915  632  902  902  802    0  702  732  802 1005 1205  732
    ##  [925]  642  802  832  532  732  702 1815  732  622  802  822 1005  702  802
    ##  [939]  702  832  702  722  902  732  722  932  732  602  842  702  742  732
    ##  [953]  802  532    0  832  832  532  512  632  832  632  632  802  532  732
    ##  [967]  632  652  802 1005  802  757  552  732  722  832  747  832  747  702
    ##  [981]  712  847  547  732  802  832  622 1845  747  632  702  702  702  732
    ##  [995] 1815 1435  435  817  652  632  732  717 1005 1605  732  617 1805 1835
    ## [1009]  532  747  727  647  502 1915  652  632  802  902  632  622  832  732
    ## [1023] 1405    0  802  717  702  552 2045  642  632  832    0  732 2235  802
    ## [1037]  702  832  632  717  802  802  902  802 1825  732  802  602  802 1405
    ## [1051]    0    0  732  802 1835    0  732 1915  832 1505  732  722  702  732
    ## [1065]  717 1005  832  752  732  822  802  612 1805  717  802  702  752  732
    ## [1079]  717  602 1855  547  702  632  917  802  732    0 1805  802  802  642
    ## [1093]  547  717  747  642  832  747 1305  702  732  657 1405  732  702 1835
    ## [1107]  757  632  802  842 1525  557  657 1105  752  847  802 1105  622 1505
    ## [1121]  802  902  832  602  732  732  732    0  627  732 1745 1745  802  732
    ## [1135]  707 1845  405  802  702  602  532 1915  802  902  802  802  802  602
    ## [1149]  832  657  602  632  512  702  702  802  617  632  622    0    0  932
    ## [1163]  702  632  702  902  902  702  802  752  902  702  702  752  547  617
    ## [1177]   15  702 1835  702  902  702  632  602  612  517  757 1035    0  702
    ## [1191]    0  702  722    0  602  802  802  802  702  732  632  602  847  802
    ## [1205]  732  752  732  502  602  702  632  632  632  832  702  732  632  802
    ## [1219]  627  732  602  732  602  602  602  702  632  947  632  742 2205  902
    ## [1233]  632  732  712  702    0  632 1445  802  602  732  802  602 1005  702
    ## [1247]  602  832  617  732    0  702  702  732  802  802  842  932  647  732
    ## [1261] 1005  947  732  802  602  732  802  802  632  732  802  802  647 1805
    ## [1275]  732  602 1035  717  602  622  642  747  802  722  702  732  702  435
    ## [1289]  702  702  637 1835  712 1805  647  602  532 2345 1815  902  932 1405
    ## [1303]  802  902  802 1215  847  702    0  532  757  802  802  642 1835 1405
    ## [1317] 1705  822  717  747  802  617 1005  527  742  932  702  732  732  832
    ## [1331]  717  752  832  547  435  902  617  902  702 2345  802  717  702  602
    ## [1345]  632  617 1005  932 1435 1435  832  817  832  632  732  802  832  702
    ## [1359]  802    0  612 1235  747  742  717  632 1745  802 1205  832  617  702
    ## [1373]  902  632  602  747    0  702 1535  627  732  802  747 1745 1705  832
    ## [1387] 1235  742 1825  817  722  802  502  702  602  602  702  902  802  632
    ## [1401]  747  732  802  702  802  742  817 1335  602  902  802  802 1755  622
    ## [1415]  732  802  602  702  902  932  602  632  902  732 1635  435  917  647
    ## [1429]  732  822  732 1005  602  647  802  717    0  647  832 2345  902  802
    ## [1443]  902  802  732  717  702  727  847 1005  832  602 1005  632  642  832
    ## [1457]  732  802  602  702  622  702  802  602  817  732  832  702  542    0
    ## [1471]  902  732  732  752  617 1705 1805  802  532  702  702  902  732 1835
    ## [1485]  817  702  902  602  602  647  717  732  732  612  632    0  752  832
    ## [1499] 1805  802  602  832  602  747   15  702  702  832  902  717  632  632
    ## [1513]  802  802  732  722  802  902 1915  902  752  602  802 1805  832  702
    ## [1527]  532 1735  612  702  802  702  702  717  717  632  817  642  732  732
    ## [1541]  702 1005  602  732  902  732  632  602  732  732    0  732  732 2235
    ## [1555]  712  547  732  502  742  847  602  702  702  632  435  717  632    0
    ## [1569] 1815  532  732  617 1855  717  702  832  802  632  702 1735 1105  802
    ## [1583]  732    0  847  732  647  817 2235 1035  802  702  717  602  817  802
    ## [1597] 2015 1835 1435    0  602  832  702  732    0  602  502  617 1205  602
    ## [1611]  802  702  802  617  632    0 1735  832  717  802 1605 1035  902  702
    ## [1625]  747  702  717  637  732  817  902  702  622  832  747  802 1005  532
    ## [1639]  702  802  747  532  612 1705  702  817  702    0 1135 1005  802  747
    ## [1653]  732  932  802  647  802  702  702  932  517  722  732  702 1805 1335
    ## [1667]  602  932  717 1505  702  632  622  712  617  802  702  732  612  902
    ## [1681] 1135  542  747 1705 1835  435  602    0  832  632  702  902  647 1635
    ## [1695] 1635  732 2345  917  802  532 1705  802  932  532  732 1005  622  817
    ## [1709]  902  602  727  602  702  532  732  802  702  802  722  902 1435  632
    ## [1723] 1915  732  702 1845 2015  802  732  652  747  632  802  602  802    0
    ## [1737] 1505  802  532  747  722  817  702  632  732 1505  847  802 1035  502
    ## [1751]  902    0  632  602  602  722  802  732 1835  702  832  702  647  922
    ## [1765]  702  647  702  617 1335  817  722    0  802  712  732  817 1915  702
    ## [1779]  702  732  657  702    0  517  932  632 2235  902  732  732  722    0
    ## [1793]  617 1735  702 1845 1805  802 1605  702  732 1915 1815  702  832  747
    ## [1807]  832  802  802  712  802  802  702  532  742    0  702  622  702  617
    ## [1821]   15  502  832  455  702  532  632  832  547  702  612  802  802  602
    ## [1835]  732  532  802  717  732  717  742  602  632  832  742 1205  702  902
    ## [1849]  832  727  632  847  622  737  902 2015  807  802  747  617  632  732
    ## [1863]  532  602  902  602    0  647  607  617  802  817 1815  732  947  522
    ## [1877]  732  832   15  832 1405  732  802  632  617  405  832  717 1435  802
    ## [1891]  532  702  702  647  702  647  832  612  742  632  632  847  802    0
    ## [1905]  717 1335  632  802  532  632  817  802  902  702 1445  812  832  902
    ## [1919]  732  832  732  832    0  747  732  547  832  732 1105  547  632  632
    ## [1933]  802  602  932  817  802  832 1805  802  727  802  817  802  702  802
    ## [1947]  802  502  747    0  702 1805  802  502  752  802  802  732  602 1835
    ## [1961] 1835  702  652  832  632  532  732  632  717  747  742  737  722  747
    ## [1975]  847  742  632  932  802  747  632  742  802  747  832  802 1005  702
    ## [1989]  532    0  632 1835  642  802  702 2015 1815  802    0  647  802  902
    ## [2003]  632  702  632 1605    0  632 1445  802  732  832  657   45  502  702
    ## [2017]  732 1405  602 1915 1915  847  832  632  702  632  632    0  722 1005
    ## [2031]  602    0    0  602 1735  802  732  517  702  902  832  622  612  702
    ## [2045]  717  717  722  732 1425  502  532  702  702  702  742  802  807  702
    ## [2059]  902  632  832 2235  702  822 1435  747  732  902  702  702  702  932
    ## [2073] 1945  702  832  632  837  517  802 1335  902  632  632  802  717  647
    ## [2087]  617  602  622  632  602  632  702 1005  732  802  802  802  552  647
    ## [2101]  717  902  802  602  732  822 1035  632  702  632 1835  732 1305  632
    ## [2115]  802  717  732  702  712 1825  617 1705  832  642  832  832  732  702
    ## [2129]  617  602  752 2205  647 1405  812 1825 1315 1815  802  822  722 2245
    ## [2143]  837  802  802  702  832  832  802    0  542  747  732  532  617  532
    ## [2157]  802  617  732  702  902  832  802  732  522  802 1835  702  747  732
    ## [2171] 1205  532  632 2105  832  732  632    0  602  732  732  732  732  732
    ## [2185]  732 1305  532  832 1535  547  802  802  732  702  902  532  832  802
    ## [2199]  802  622  802  632  612  732  742  702  702  902  817  702  917  702
    ## [2213]  722  802 1345  802 1705 1705 1805  712    0  532  802  802  747 2235
    ## [2227]  817  717  832  702  902  817  642  702  802 1835  732  802  702  847
    ## [2241]  747  612  702  802 1805  822  902  702  632  802  617  612 1135  602
    ## [2255]  802  847  732  802 1915  742  742 1045  657  832  732  732  732  832
    ## [2269]  902  732  802  747  832  732  832 1105 1005  902  717  732  802 1405
    ## [2283]  532  702  717  602  732  502  802  632  602  632  602  652  832  847
    ## [2297] 1135 1005  717  702  857  802  602 1735 1805  802  702  817  802  802
    ## [2311]  717  435  612  747  532 1915  602  802  817  822  832 1825  802  902
    ## [2325]    0  647  702 1735  602  732  632  802  832  832  632  802  702    0
    ## [2339]  717  717  742  702  732  802  602  802  722  902  607  732 1735  702
    ## [2353]  802  732  702  832 1805  917  647  617  747  632  702  802  702  632
    ## [2367]  617  842  702  732  702  702  632  532  732  802  552 2125  932 2235
    ## [2381]  602  617 1745    0  702    0  902  542 1605  847  932  702  802  602
    ## [2395]  812  602  917  647  632  902  752 1545 1605  712  732 1105  802 1415
    ## [2409]  947  802  742    0  717  632  802    0  632  435  632  752  847  717
    ## [2423]  902 1005  602  602  717  702  902  852  832  632  612  832  732  532
    ## [2437]  752 1435  717  632  802  602  702    0  802  802  702  802  802  717
    ## [2451]  732  702  702 1635  632  702  732  817  817 1235  547  617  802  702
    ## [2465]  502 2215  802 1525  732  732  742  702  802  802  722  702  947  802
    ## [2479]  702  742  732  727  752  547  832  802  552  617  812  832  612    0
    ## [2493]  832 1845 1805  732 2015  832  602 1805  832  732 1305  557  832  917
    ## [2507]  802  932  702 1305  702  757  802  527  647  632  747  802  902 1005
    ## [2521] 1505 1835 1815  547  732  832  832  802    0  832  632  632    0  747
    ## [2535]  842  732 2135  732 1305  602  802  832  737  847 1205  702  732  732
    ## [2549]  832 2205  647  732  747  707  502  732  702  702  832  847  602  435
    ## [2563] 2205  832  802  902  702  717  512 1035  455  717  702  602  702 2315
    ## [2577]  602  732  802  632 1735  642  702  717  932 1005  702  737 1805 1835
    ## [2591]  802  832 1305  602  702  702  817  722  542 1705  702  802  702  647
    ## [2605]  632  747  532  832  802  602  647  647 1005  822  502  702  717  732
    ## [2619]  822  732  602 1535  632  802    0 1535

``` r
length(DEPARTS)
```

    ## [1] 2626

``` r
graveyard <- ((DEPARTS >= 0) & (DEPARTS <= 459))
morning <- ((DEPARTS >= 500) & (DEPARTS <=930))
daytime <- ((DEPARTS >= 931) & (DEPARTS <=1700))
evening <- ((DEPARTS >= 1701) & (DEPARTS <=2359))
summary(graveyard)
```

    ##    Mode   FALSE    TRUE 
    ## logical    2506     120

``` r
summary(morning)
```

    ##    Mode   FALSE    TRUE 
    ## logical     501    2125

``` r
summary(daytime)
```

    ##    Mode   FALSE    TRUE 
    ## logical    2420     206

``` r
summary(evening)
```

    ##    Mode   FALSE    TRUE 
    ## logical    2451     175

``` r
table(morning)
```

    ## morning
    ## FALSE  TRUE 
    ##   501  2125

``` r
deptime <- factor((1*evening + 2*morning + 3*daytime + 4*graveyard), levels = c(1, 2, 3, 4), labels = c( "Night", "Grave", "Morn", "Mid"))
table(deptime)
```

    ## deptime
    ## Night Grave  Morn   Mid 
    ##   175  2125   206   120

``` r
sum(morning+daytime)
```

    ## [1] 2331

``` r
detach()
```

``` r
attach(dat_med)
summary(INCWAGE)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##       0   50000   74000   88171  100000  638000

Try a regression with age and age-squared in addition to your other
controls, something like this:

``` r
attach(dat_med)
```

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

``` r
lma <- lm((INCWAGE ~ AGE + I(AGE^2) + educ_college))
lmdep <- lm((INCWAGE ~ AGE + I(AGE^2) + educ_college + deptime))
# summary(lma)
summary(lmdep)
```

    ## 
    ## Call:
    ## lm(formula = (INCWAGE ~ AGE + I(AGE^2) + educ_college + deptime))
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -120804  -35204  -10321   16892  574221 
    ## 
    ## Coefficients:
    ##               Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  -14079.71   21906.40  -0.643  0.52046    
    ## AGE            5236.23     986.15   5.310 1.19e-07 ***
    ## I(AGE^2)        -47.38      10.81  -4.383 1.22e-05 ***
    ## educ_college -35638.10    2917.99 -12.213  < 2e-16 ***
    ## deptimeGrave  -9765.31    5861.19  -1.666  0.09581 .  
    ## deptimeMorn  -22567.52    7616.93  -2.963  0.00308 ** 
    ## deptimeMid   -21508.33    8785.78  -2.448  0.01443 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 73810 on 2619 degrees of freedom
    ## Multiple R-squared:  0.08587,    Adjusted R-squared:  0.08378 
    ## F-statistic:    41 on 6 and 2619 DF,  p-value: < 2.2e-16

``` r
plot(lma)
```

![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-5-2.png)<!-- -->![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-5-3.png)<!-- -->![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-5-4.png)<!-- -->

``` r
require(stargazer)
```

    ## Loading required package: stargazer

    ## 
    ## Please cite as:

    ##  Hlavac, Marek (2018). stargazer: Well-Formatted Regression and Summary Statistics Tables.

    ##  R package version 5.2.2. https://CRAN.R-project.org/package=stargazer

``` r
suppressWarnings(stargazer(lma, type = "text"))
```

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

``` r
NNobs <- length(INCWAGE)
set.seed(12345)
graph_obs <- (runif(NNobs) < 0.5)
med_graph <-subset(dat_med, graph_obs) 
plot(INCWAGE ~ jitter(AGE, factor = 0.1), pch = 16, col = rgb(0.5, 0.5, 0.5, alpha = 0.2), ylim = c(0,300000), data = med_graph)
require(AER)
```

    ## Loading required package: AER

    ## Loading required package: car

    ## Loading required package: carData

    ## Loading required package: lmtest

    ## Loading required package: zoo

    ## 
    ## Attaching package: 'zoo'

    ## The following objects are masked from 'package:base':
    ## 
    ##     as.Date, as.Date.numeric

    ## Loading required package: sandwich

    ## Loading required package: survival

    ## 
    ## Attaching package: 'survival'

    ## The following object is masked from 'dat_med':
    ## 
    ##     veteran

``` r
medpredict <- data.frame(AGE = 25:70, female = 1, educ_college = 1, educ_advdeg = 1)
medpredict$yhat <- predict(lma, newdata = medpredict)
lines(yhat ~ AGE, data = medpredict)
```

![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

``` r
max(medpredict$yhat) # peak predicted wage
```

    ## [1] 84317.64

``` r
plot(INCWAGE ~ jitter(AGE, factor = 0.1), pch = 16, col = rgb(0.5, 0.5, 0.5, alpha = 0.2), ylim = c(0,300000), data = med_graph)
require(AER)
medpredictb <- data.frame(AGE = 25:70, female = 1, educ_college = 0, educ_advdeg = 1)
medpredictb$yhat <- predict(lma, newdata = medpredictb)
lines(yhat ~ AGE, data = medpredictb, col = "blue")
```

![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

``` r
max(medpredictb$yhat) # peak predicted wage
```

    ## [1] 119489.4

What is the peak of predicted wage? What if you add higher order
polynomials of age, such as \(Age^3\) or \(Age^4\)?

``` r
# sets up lm with polynomials included
attach(dat_med)
```

    ## The following object is masked from package:survival:
    ## 
    ##     veteran

    ## The following objects are masked from dat_med (pos = 11):
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

    ## The following objects are masked from dat_med (pos = 12):
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

``` r
lmpolys <- lm((INCWAGE ~ AGE + I(AGE^2) + I(AGE^3) + I(AGE^4) + I(AGE^5) + educ_college))
# summary(lma)
plot(lmpolys)
```

![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-8-2.png)<!-- -->![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-8-3.png)<!-- -->![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-8-4.png)<!-- -->

``` r
require(stargazer)
suppressWarnings(stargazer(lmpolys, type = "text"))
```

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

``` r
# plots line on plot - peak wages AREN'T particularly different from those of the Aged^2 lm
plot(INCWAGE ~ jitter(AGE, factor = 1), pch = 16, col = rgb(0.5, 0.5, 0.5, alpha = 0.2), ylim = c(0,300000), data = med_graph)
medpredict$yhatploys <- predict(lmpolys, newdata = medpredict)
lines(yhatploys ~ AGE, data = medpredict)
```

![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

``` r
max(medpredict) # peak predicted wage
```

    ## [1] 85445.97

``` r
# Hypothesis test of higher order polynomials - jointly significant
coeftest(lmpolys,vcovHC)
```

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

``` r
require(car)
# or
linearHypothesis(lmpolys, "I(AGE^2) + I(AGE^3) + I(AGE^4) + I(AGE^5) = 0", test="F")
```

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

``` r
# The pattern of predicted wage trends upward until age reaches the mid 50s, plateaus until approximately 57 years old then trends down towards 70 years old.
```

What if you used \(log(Age)\)? (And why would polynomials in
\(log(Age)\) be useless? Experiment.)

``` r
lmlog <- lm((INCWAGE ~ AGE + log(AGE) + educ_college))
# plot(lmlog)
plot(INCWAGE ~ jitter(AGE, factor = 1), pch = 16, col = rgb(0.5, 0.5, 0.5, alpha = 0.2), ylim = c(0,300000), data = med_graph)
medpredict$yhatlog <- predict(lmlog, newdata = medpredict)
lines(yhatlog ~ AGE, data = medpredict)
```

![](Hwk5Draftb_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

``` r
max(medpredict) # peak predicted wage
```

    ## [1] 85445.97

``` r
# using log(AGE) smooths the curve towards the upper end of the age range. We shouldn't use polynomials as they will undo the impact of the log function.
```

Recall about how dummy variables work. If you added educ\_hs in a
regression using the subset given above, what would that do?
(Experiment, if you aren’t sure.) What is interpretation of coefficient
on *educ\_college* in that subset? What would happen if you put both
*educ\_college* and *educ\_advdeg* into a regression? Are your other
dummy variables in the regression working sensibly with your selection
criteria?

``` r
# Adding educ_hs in our particular subset will render it as omitted (we haven't included it as a variable in the subset). If we add the educ_advdeg variable, it will also be rendered as omitted, so R can run a comparison.
lmedu <- lm((INCWAGE ~ AGE + I(AGE^2) + educ_college + educ_advdeg))
lmedu
```

    ## 
    ## Call:
    ## lm(formula = (INCWAGE ~ AGE + I(AGE^2) + educ_college + educ_advdeg))
    ## 
    ## Coefficients:
    ##  (Intercept)           AGE      I(AGE^2)  educ_college   educ_advdeg  
    ##    -22289.85       5121.62        -46.25     -35171.72            NA

``` r
lmedua <- lm((INCWAGE ~ AGE + I(AGE^2) + educ_advdeg + educ_college))
lmedua
```

    ## 
    ## Call:
    ## lm(formula = (INCWAGE ~ AGE + I(AGE^2) + educ_advdeg + educ_college))
    ## 
    ## Coefficients:
    ##  (Intercept)           AGE      I(AGE^2)   educ_advdeg  educ_college  
    ##    -57461.57       5121.62        -46.25      35171.72            NA

``` r
# if we change the position of the educ_advdeg variable to place it BEFORE the educ_college variable, R drops the LAST variable from the regression. What is interesting is that whichever variable is included, the coefficient is the absolute same value but has a negative value when educ_college is the included variable.
```

Why don’t we use polynomial terms of dummy variables? Experiment.

``` r
# We don't use polynomials of dummy variables as they have no effect on the regression - 1^x = 1; 0^x = 0.
```

What is the predicted wage, from your model, for a few relevant cases?
Do those seem reasonable?

``` r
# check wage as a function of age for lma and medpredict
# we can also change the medpredict objects to use the poly regression or the log regression
medpredictc <- data.frame(AGE = 25:70, female = 1, educ_college = 1, educ_advdeg = 0)
medpredictc$yhat <- predict(lma, newdata = medpredictc)
medpredict
```

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

``` r
medpredictb
```

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

``` r
medpredictc
```

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

``` r
# included all 3 regressions into medpredict in prior chunks to allow for easy comparison in the table
medpredict
```

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

``` r
mean(medpredict$yhat)
```

    ## [1] 73309.33

``` r
mean(medpredict$yhatlog)
```

    ## [1] 73658.94

Try some interactions, like this,

``` r
# interactions
lminter <- lm(INCWAGE ~ AGE + I(AGE^2) + educ_college + I(educ_college*AGE) + I(educ_college*(AGE^2)))
lminter
```

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

``` r
lminterb <- lm(INCWAGE ~ AGE + I(AGE^2) + educ_college + I(educ_college*AGE) + educ_advdeg + I(educ_advdeg*AGE))
lminterb
```

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
