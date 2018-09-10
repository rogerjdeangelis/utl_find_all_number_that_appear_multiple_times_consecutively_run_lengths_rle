# utl_find_all_number_that_appear_multiple_times_consecutively_run_lengths_rle
Find all numbers that appear multiple times consecutively_run_lengths_rle.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    Find all numbers that appear multiple times consecutively run lengths rle

      Good Question

      Two Solutions
           1. Lag
           2, By num notsorted
           3. R rle function (run length)


    github
    https://tinyurl.com/yb9bnq7k
    https://github.com/rogerjdeangelis/utl_find_all_number_that_appear_multiple_times_consecutively_run_lengths_rle

    SAS forum
    https://communities.sas.com/t5/SAS-Programming/Consecutive-Numbers/m-p/494050

    profile RW9
    https://communities.sas.com/t5/user/viewprofilepage/user-id/45151

    INPUT
    =====

     SD1.HAVE total obs=9

      ID    NUM

       1     1
       2     1
       3     1
       4     1    pos=4 run length=4 value=1

       5     2
       6     2    pos=6 run length=2 value=2

       7     1
       8     1
       9     1    pos=9 run length=3 value=1


    EXAMPLE OUTPUTs
    --------------

      1. Lag
      ------
                                | Obs  RULES
       WORK.WANT total obs=3    |
                                |
         Obs    NUM             |  1 1
                                |  2 1  1
          1      1              |  3 1  1
          2      1           s  |  4    1 Two runs od 1s

                                |
          3      1              |  one run of 3

      2. By num notsorted
      --------------------

        WORK.WANT total obs=2

         NUM    CNT    POS | RULES
                           |
          1      4      4  | at ob 4 run of 4
          1      3      9  | at ob 9 run of 3


      3. R rle function (run length)

       RUN1    RUN2    RUN3

         1       2       1     Values
         4       2       3     RunLength

         1 for a run of 4
         2 for a run of 2
         1 for a run of 3


    PROCESS
    =======

      1. Lag  (user RW9)
      -------------------

        data want (drop=id);
          set sd1.have;
          if lag2(num)=num and lag(num)=num then output;
        run;

      2. By num notsorted
      --------------------


       * Additional Solution;
       * Provides count and only outputs 1 once per stream of 1s;

       data want(drop=id);
       set sd1.have;
       by num notsorted;
       cnt+1;
       pos=_n_;
       if last.num then do;
         if cnt ge 3 then output;
         cnt=0;
       end;
       run;quit;

      3. R rle function (run length)
      ------------------------------

       If you have name space issue just keey installing the requested packages;

       %utlfkil(d:/xpt/rxpt.xpt); * just in case it exists;

       * note haven fails with SASxport use package sas7bdat;
       %utl_submit_r64('
       library(haven);
       library(SASxport);
       have<-unlist(read_sas("d:/sd1/have.sas7bdat")[,2]);
       have;
       want<-rle(have);
       want<-as.data.frame(rbind(want$lengths,want$values));
       colnames(want)<-c("run1","run2","run3");
       write.xport(want,file="d:/xpt/rxpt.xpt");
       ');

       libname xpt xport "d:/xpt/rxpt.xpt";

       proc print data=xpt.want;
       run;quit;

       RUN1    RUN2    RUN3

         1*      2       1
         4       2       3

        1 for a run of 4

     *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
    input Id num ;
    cards4;
    1 1
    2 1
    3 1
    4 1
    5 2
    6 2
    7 1
    8 1
    9 1
    ;;;;
    run;quit;
