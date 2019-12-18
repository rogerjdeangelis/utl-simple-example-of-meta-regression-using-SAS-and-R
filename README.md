# utl-simple-example-of-meta-regression-using-SAS-and-R
Simple example of meta regression using SAS and R
    Simple example of meta regression using SAS and R

    Adjusting event probabilities using regression.

    github
    https://tinyurl.com/tmekj2c
    https://github.com/rogerjdeangelis/utl-simple-example-of-meta-regression-using-SAS-and-R

    SAS Forum
    https://tinyurl.com/w3j7g9x
    https://communities.sas.com/t5/Statistical-Procedures/How-to-do-meta-regression-in-SAS/m-p/611084

    Up to 40 obs WORK.WANT total obs=5

    Obs    STUDY      YEAR    N_E    MEAN_E    SD_E    N_C    MEAN_C     SD_C

     1     Davis      1973     13      5.0     4.70     13      6.50     3.80
     2     Florell    1971     30      4.9     1.71     50      6.10     2.30
     3     Gruen      1975     35     22.5     3.44     35     24.90    10.65
     4     Hart       1975     20     12.5     1.47     20     12.30     1.66
     5     Wilson     1977      8      6.5     0.76      8      7.38     1.41


    *_                   _
    (_)_ __  _ __  _   _| |_
    | | '_ \| '_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    ;

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
      input STUDY$ REGION$ AGE YEAR N_E MEAN_E SD_E N_C MEAN_C SD_C;
    cards4;
     Davis Europe 55 1973 13 5.0 4.70 13 6.50 3.80
     Florell Europe 65 1971 30 4.9 1.71 50 6.10 2.30
     Gruen Asia 55 1975 35 22.5 3.44 35 24.90 10.65
     Hart Asia 65 1975 20 12.5 1.47 20 12.30 1.66
     Wilson Europe 55 1977 8 6.5 0.76 8 7.38 1.41
    ;;;;
    run;quit;

    SD1.HAVE total obs=5

    Obs    STUDY      REGION    AGE    YEAR    N_E    MEAN_E    SD_E    N_C    MEAN_C     SD_C

     1     Davis      Europe     55    1973     13      5.0     4.70     13      6.50     3.80
     2     Florell    Europe     65    1971     30      4.9     1.71     50      6.10     2.30
     3     Gruen      Asia       55    1975     35     22.5     3.44     35     24.90    10.65
     4     Hart       Asia       65    1975     20     12.5     1.47     20     12.30     1.66
     5     Wilson     Europe     55    1977      8      6.5     0.76      8      7.38     1.41

    *            _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| '_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    ;

    Mixed-Effects Model (k = 5; tau^2 estimator: DL)

    tau^2 (estimated amount of residual heterogeneity):     0.0088 (SE = 1.3220)
    tau (square root of estimated tau^2 value):             0.0939
    I^2 (residual heterogeneity / unaccounted variability): 0.67%
    H^2 (unaccounted variability / sampling variability):   1.01
    R^2 (amount of heterogeneity accounted for):            95.34%

    Test for Residual Heterogeneity:
    QE(df = 2) = 2.0134, p-val = 0.3654

    Test of Moderators (coefficients 2:3):
    QM(df = 2) = 3.4928, p-val = 0.1744

    Model Results:

                  estimate      se     zval    pval    ci.lb   ci.ub
    intrcpt        -0.4729  4.3338  -0.1091  0.9131  -8.9670  8.0213
    REGIONEurope   -1.0936  0.6455  -1.6942  0.0902  -2.3587  0.1715  .
    AGE             0.0078  0.0669   0.1162  0.9075  -0.1234  0.1390

    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1


    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;


    %utl_submit_r64('
    library(haven);
    library(SASxport);
    library(meta);
    have<-read_sas("d:/sd1/have.sas7bdat");
    m1 <- metacont(N_E, MEAN_E, SD_E, N_C, MEAN_C, SD_C,data = have, sm = "MD");
    mu1 <- update(m1, byvar = REGION);
    sink("d:/txt/want.txt");
    metareg(mu1, REGION + AGE);
    sink();
    ');

    data _null_;
      infile "d:/txt/want.txt";
      input;
      put _infile_;
    run;quit;


