# utl_rolling_means_by-quarter_semiannual_and_yearly
    Rolling means by quarter semi-annual and yearly

    github
    https://goo.gl/iyKsqS
    https://github.com/rogerjdeangelis/utl_rolling_means_by-quarter_semiannual_and_yearly

    INPUT
    =====                                             RULES

     SD1.HAVE total obs=27          INTERPOLATE                        Rolling Quarters

       MONTH    X |   MONTH     X   Delta=(LAST-FIRST)/(NLAST-NFIRST)   QUARTER
                  |
          1     1 |      1    1.00                                         .
          2     1 |      2    1.00  Delta = (3-1)/3 = 0.67                 .
     not in       |      3    1.67  1 + delta   = 1.67                    1.22   (1 + 1    + 1.67)/3 = 1.22
     input        |      4    2.33  1 + 2*delta = 2.33                    1.67   (1 + 1.67 + 2.33)/3 = 1.67
          5     3 |      5    3.00  1 + 3*delta = 1+3*(2/3) =3            2.33
          6     0 |      6    0.00                                        1.77
          7     2 |      7    2.00                                        1.67
          8     4 |      8    4.00                                        2.00
          9     2 |      9    2.00                                        2.67
         10     0 |     10    0.00                                        2.00
         11     4 |     11    4.00                                        2.00
         12     3 |     12    3.00                                        2.33
         13     4 |     13    4.00                                        3.67
         14     1 |     14    1.00                                        2.67
         15     0 |     15    0.00                                        1.67
                  |     16    1.33                                        0.77
                  |     17    2.67                                        1.33
         18     4 |     18    4.00                                        2.67
         19     0 |     19    0.00                                        2.22
         20     3 |     20    3.00                                        2.33
         21     2 |     21    2.00                                        1.67
         22     3 |     22    3.00                                        2.67
         23     0 |     23    0.00  delta = (1-0)/5 = .2                  1.67
    not in        |     24    0.20  0 +   .2 = .2                         1.06
    input         |     25    0.40  0 + 2*.2 = .4                         0.20
                  |     26    0.60  0 + 3*.2 = .6                         0.40   (.2 + .4 + .6)/3  = .4
                  |     27    0.80  0 + 4*.2 = .8                         0.60
         28     1 |     28    1.00  0 + 5*.2 =  1                         0.80
         29     2 |     29    2.00                                        1.26
         30     2 |     30    2.00                                        1.67
                  |     31    3.00                                        2.33
         32     4 |     32    4.00                                        3.00
         33     1 |     33    1.00                                        2.67
         34     3 |     34    3.00                                        2.67
         35     4 |     35    4.00                                        2.67
         36     2 |     36                                                3.00


    WORKING CODE
    ============

         want        <- complete(have, MONTH = full_seq(MONTH, 1)); * complete the months;
         want$X      <- na.approx(want$X);                          * linear interpolation;
         want$QUARTER<-c(rep(NA,2),roll_mean(want$X,   3));         * various rolling averages;
         want$SEMI   <-c(rep(NA,5),roll_mean(want$X,   6));
         want$ANNUAL <-c(rep(NA,11),roll_mean(want$X, 12));


    OUTPUT
    ======

        WORK.WANT total obs=36

         MONTH       X       QUARTER      SEMI      ANNUAL

            1     1.00000     .          .          .
            2     1.00000     .          .          .
            3     1.66667    1.22222     .          .
            4     2.33333    1.66667     .          .
            5     3.00000    2.33333     .          .
            6     0.00000    1.77778    1.50000     .
            7     2.00000    1.66667    1.66667     .
            8     4.00000    2.00000    2.16667     .
            9     2.00000    2.66667    2.22222     .
           10     0.00000    2.00000    1.83333     .
           11     4.00000    2.00000    2.00000     .
           12     3.00000    2.33333    2.50000    2.00000
           13     4.00000    3.66667    2.83333    2.25000
           14     1.00000    2.66667    2.33333    2.25000
           15     0.00000    1.66667    2.00000    2.11111
           16     1.33333    0.77778    2.22222    2.02778
           17     2.66667    1.33333    2.00000    2.00000
           18     4.00000    2.66667    2.16667    2.33333
           19     0.00000    2.22222    1.50000    2.16667
           20     3.00000    2.33333    1.83333    2.08333
           21     2.00000    1.66667    2.16667    2.08333
           22     3.00000    2.66667    2.44444    2.33333
           23     0.00000    1.66667    2.00000    2.00000
           24     0.20000    1.06667    1.36667    1.76667
           25     0.40000    0.20000    1.43333    1.46667
           26     0.60000    0.40000    1.03333    1.43333
           27     0.80000    0.60000    0.83333    1.50000
           28     1.00000    0.80000    0.50000    1.47222
           29     2.00000    1.26667    0.83333    1.41667
           30     2.00000    1.66667    1.13333    1.25000
           31     3.00000    2.33333    1.56667    1.50000
           32     4.00000    3.00000    2.13333    1.58333
           33     1.00000    2.66667    2.16667    1.50000
           34     3.00000    2.66667    2.50000    1.50000
           35     4.00000    2.66667    2.83333    1.83333
           36     2.00000    3.00000    2.83333    1.98333

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;
    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
     input MONTH X;
    cards4;
     1 1
     2 1
     5 3
     6 0
     7 2
     8 4
     9 2
     10 0
     11 4
     12 3
     13 4
     14 1
     15 0
     18 4
     19 0
     20 3
     21 2
     22 3
     23 0
     28 1
     29 2
     30 2
     32 4
     33 1
     34 3
     35 4
     36 2
    ;;;;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;


    %utl_submit_wps64('
    options set=R_HOME "C:/Program Files/R/R-3.3.2";
    libname wrk sas7bdat "%sysfunc(pathname(work))";
    proc r;
    submit;
    source("c:/Program Files/R/R-3.3.2/etc/Rprofile.site",echo=T);
    library(tidyr);
    library(zoo);
    library(haven);
    library(RcppRoll);
    have<-read_sas("d:/sd1/have.sas7bdat");
    want<- complete(have, MONTH = full_seq(MONTH, 1));
    want$X <- na.approx(want$X);
    want$QUARTER<-c(rep(NA,2),roll_mean(want$X, 3));
    want$SEMI   <-c(rep(NA,5),roll_mean(want$X, 6));
    want$ANNUAL <-c(rep(NA,11),roll_mean(want$X, 12));
    endsubmit;
    import r=want data=wrk.want;
    run;quit;
    ');

