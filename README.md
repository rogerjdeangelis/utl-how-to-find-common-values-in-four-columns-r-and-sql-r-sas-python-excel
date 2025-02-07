# utl-how-to-find-common-values-in-four-columns-r-and-sql-r-sas-python-excel
how to find common values in four columns r and sql r sas python excel 
    %let pgm=utl-how-to-find-common-values-in-four-columns-r-and-sql-r-sas-python-excel;

    %stop_submission;

    how to find common values in four columns r and sql r sas python excel

           SOLUTIONS

              1 r generate sql code
              2 r sas python excel sql hardcoded using 1 (see https://tinyurl.com/z9srrxbr)
              3 sas dynamic code
              4 r dynamic code
              5 r purrr language
                reduce, intersect

    /*         _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __  ___
    / __|/ _ \| | | | | __| |/ _ \| `_ \/ __|
    \__ \ (_) | | |_| | |_| | (_) | | | \__ \
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|___/

    */

    /**************************************************************************************************************************/
    /*                           |                                                            |                               */
    /*                           |                                                            |                               */
    /*        INPUT              |             PROCESS                                        |        OUTPUT                 */
    /*                           |       (SQL IS SELF EXPLANTORY)                             |                               */
    /*                           |                                                            |                               */
    /*                           |                                                            |                               */
    /* X1    X2    X3    X4      | 1 R GENERATE SQL QUERY TEXT (see 4 for example)            |  colx                         */
    /*                           | ===========================                                |  ----                         */
    /*  1*    3*    2     3*     |                                                            |     1                         */
    /*  3*    1*    3*    2      | %utl_rbeginx;                                              |     3                         */
    /*  5     8     1*    4      | parmcards4;                                                |     9                         */
    /*  7     9*    5     1*     | library(haven)                                             |                               */
    /*  9*    1     7     9*     | have<-read_sas("d:/sd1/have.sas7bdat")                     |                               */
    /*  1     1     9*    3      | cols<-names(have)                                          |                               */
    /*                           | phrases <- sprintf("select %s As colx from have",cols)     |                               */
    /*                           | genquery <- paste(phrases,collapse = "\n Union All\n")     |                               */
    /*                           | cat(genquery)                                              |                               */
    /*                           | ;;;;                                                       |                               */
    /*                           | %utl_rendx;                                                |                               */
    /*                           |                                                            |                               */
    /*                           | select x1 as colx from have                                |                               */
    /*                           |  intersect                                                 |                               */
    /*  options                  | select x2 as colx from have                                |                               */
    /*   validvarname=upcase;    |  intersect                                                 |                               */
    /*  libname sd1 "d:/sd1";    | select x3 as colx from have                                |                               */
    /*  data sd1.have;           |  intersect                                                 |                               */
    /*  input                    | select x4 as colx from have                                |                               */
    /*     x1 x2 x3 x4;          |                                                            |                               */
    /*  cards4;                  |                                                            |                               */
    /*  1 3 2 3                  | 2 SAS R PYTHON INSERT TEXT (ONLY SAS SHOWN)                |  colx                         */
    /*  3 1 3 2                  | ===========================================                |  ----                         */
    /*  5 8 1 4                  |                                                            |     1                         */
    /*  7 9 5 1                  | proc sql;                                                  |     3                         */
    /*  9 1 7 9                  |   select x1 as colx from sd1.have                          |     9                         */
    /*  1 1 9 3                  |    intersect                                               |                               */
    /*  ;;;;                     |   select x2 as colx from sd1.have                          |                               */
    /*  run;quit;                |    intersect                                               |                               */
    /*                           |   select x3 as colx from sd1.have                          |                               */
    /*                           |    intersect                                               |                               */
    /*                           |   select x4 as colx from sd1.have                          |                               */
    /*                           | ;quit;                                                     |                               */
    /*                           |                                                            |                               */
    /*                           | 3 SAS DYNAMIC SQL CODE                                     |  colx                         */
    /*                           | ==================                                         |  ----                         */
    /*                           |                                                            |     1                         */
    /*                           | %array(_idx,values=1-4)                                    |     3                         */
    /*                           |                                                            |     9                         */
    /*                           | proc sql;                                                  |                               */
    /*                           |   %do_over(_idx,phrase=%str(                               |                               */
    /*                           |    select x? as colx from sd1.have)                        |                               */
    /*                           |   ,between=intersect                                       |                               */
    /*                           |   )                                                        |                               */
    /*                           | ;quit;                                                     |                               */
    /*                           |                                                            |                               */
    /*                           | 4 R SQL DYNAMIC CODE                                       |                               */
    /*                           | ====================                                       |  colx                         */
    /*                           |                                                            |  ----                         */
    /*                           | %utl_rbeginx;                                              |     1                         */
    /*                           | parmcards4;                                                |     3                         */
    /*                           | library(haven)                                             |     9                         */
    /*                           | library(sqldf)                                             |                               */
    /*                           | source("c:/oto/fn_tosas9x.R")                              |                               */
    /*                           | have<-read_sas("d:/sd1/have.sas7bdat")                     |                               */
    /*                           | cols<-names(have)                                          |  ROWNAMES COLX                */
    /*                           | phrases <- sprintf("select %s As colx from have",cols)     |                               */
    /*                           | phrases <- gsub("\\s+", " ", phrases)                      |     1      1                  */
    /*                           | genquery <- paste(phrases,collapse = "\n intersect\n")     |     2      3                  */
    /*                           | want<-sqldf(genquery)                                      |     3      9                  */
    /*                           | want                                                       |                               */
    /*                           | fn_tosas9x(                                                |                               */
    /*                           |       inp    = want                                        |                               */
    /*                           |      ,outlib ="d:/sd1/"                                    |                               */
    /*                           |      ,outdsn ="want"                                       |                               */
    /*                           |      )                                                     |                               */
    /*                           | ;;;;                                                       |                               */
    /*                           | %utl_rendx;                                                |                               */
    /*                           |                                                            |                               */
    /*                           |                                                            |                               */
    /*                           | 5 R PURR LANGUAGE                                          |  > res                        */
    /*                           | =================                                          |    values                     */
    /*                           |                                                            |  1      1                     */
    /*                           | %utl_rbeginx;                                              |  2      3                     */
    /*                           | parmcards4;                                                |  3      9                     */
    /*                           | library(haven)                                             |                               */
    /*                           | library(sqldf)                                             |                               */
    /*                           | library(purrr)                                             |  ROWNAMES VALUES              */
    /*                           | source("c:/oto/fn_tosas9x.R")                              |                               */
    /*                           | have<-read_sas("d:/sd1/have.sas7bdat")                     |        1     1                */
    /*                           | print(have)                                                |        2     3                */
    /*                           | common_values<-reduce(have[,1:ncol(have)],intersect)       |        3     9                */
    /*                           | common_values                                              |                               */
    /*                           | res <- data.frame(values = common_values)                  |                               */
    /*                           | res                                                        |                               */
    /*                           | fn_tosas9x(                                                |                               */
    /*                           |       inp    = res                                         |                               */
    /*                           |      ,outlib ="d:/sd1/"                                    |                               */
    /*                           |      ,outdsn ="want"                                       |                               */
    /*                           |      )                                                     |                               */
    /*                           | ;;;;                                                       |                               */
    /*                           | %utl_rendx;                                                |                               */
    /*                           |                                                            |                               */
    /*                           | proc print data=sd1.want;                                  |                               */
    /*                           | run;quit;                                                  |                               */
    /*                           |                                                            |                               */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
