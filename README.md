# utl-recovering-a-sas-program-from-the-log-does-not-handle-wrapped-lines-logparse
Recovering a sas program from the log does not handle wrapped lines logparse   
    %let pgm=utl-recovering-a-sas-program-from-the-log-does-not-handle-wrapped-lines-logparse;

    Recovering a sas program from the log does not handle wrapped lines logparse

    Using the wayback time machine(SAS6.12)

    github
    https://tinyurl.com/4rscfswz
    https://github.com/rogerjdeangelis/utl-recovering-a-sas-program-from-the-log-does-not-handle-wrapped-lines-logparse

    I had ome difficulty with
    https://github.com/scottbass/SAS/blob/master/Macro/logparse.sas
    probably my thick skull

    /*
     _                   _
    (_)_ __  _ __  _   _| |_   _ __  _ __ ___   ___ ___  ___ ___
    | | `_ \| `_ \| | | | __| | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | | | | | |_) | |_| | |_  | |_) | | | (_) | (_|  __/\__ \__ \
    |_|_| |_| .__/ \__,_|\__| | .__/|_|  \___/ \___\___||___/___/
            |_|               |_|
    */

    /*----                                                                   ----*/
    /*---- Just paste your log after the cards4 statement below              ----*/
    /*---- If you are using the classic 1980s editor with                    ----*/
    /*---- the clean command line then you can send your log                 ----*/
    /*---- to c:/temp/log.txt by typying this on the log command line        ----*/
    /*----                                                                   ----*/
    /*---- Output is in the log                                              ----*/
    /*----                                                                   ----*/

    Command ===> file "d:/temp/log.txt"

    data _null_;
      * this is not 100 percent but should get you very close - wrapped lines are not parsed correcty;
      input ;
      if _n_>1;
      select;
          when ( index(_infile_,'The SAS System' ))  return;
          when ( _infile_ in: ('1' '2' '3' '4' '5' '6' '7' '8' '9' ) ) do;
            _infile_=substr(_infile_,index(_infile_,' '));
             put _infile_;
          end;
          when ( _infile_ =: 'MPRINT')  do;
            _infile_=left(substr(_infile_,index(_infile_,':')+1));
            put _infile_;
          end;
          when ( _infile_ =: 'MACROGEN')  do;
            _infile_=left(substr(_infile_,index(_infile_,':')+1));
            put _infile_;
          end;
          when ( _infile_ =: '+')  do;
            _infile_=left(substr(_infile_,index(_infile_,'+')+1));
            put _infile_;
          end;
          otherwise;
      end;
    cards4;
    1        The SAS System     09:12 Wednesday, September 20, 2000;
    NOTE: Copyright (c) 1989-1996 by SAS Institute Inc., Cary, NC, USA.;
    NOTE: SAS (r) Proprietary Software Release 6.12  TS055;
    This message is contained in the SAS news file, and is presented upon;
    initialization.  Edit the files 'news' in the 'misc/base' directory to;
    display site-specific news and information in the program log.;
    The command line option '-nonews' will prevent this display.;
    NOTE: SAS initialization used:;
    real time           0.160 seconds;
    cpu time            0.049 seconds;
    NOTE: AUTOEXEC processing completed.;
    MPRINT(UTLOPTS):   MERROR NOCENTER DETAILS SERROR NONUMBER FULLSTIMER NODATE DKRICOND=WARN DKROCOND=WARN NOSYNTAXCHECK ;
    MPRINT(UTLOPTS):   run;
    MPRINT(UTLOPTS):  quit;
    MLOGIC(UTLOPTS):  Ending execution.
    2685  %macro gencode(lower);
    2686    %global lwr;
    2687    %let lwr=&lower;
    2688  %mend gencode;
    2689  %macro test(inp=class,var=age,stat=min q1 mean median max);
    2690    %gencode(lower);
    2691    %put &=lwr;
    2692    %if %upcase(&inp) = CLASS %then %do;
    2693        %let lib=sashelp;
    2694        %let inp=&lib..&inp;
    2695    %end;
    2696    proc means data=&inp &stat ;
    2697      var &var;
    2698    run;quit;
    2699  %mend test;
    2700  %test(var=weight);
    MLOGIC(TEST):  Beginning execution.
    MLOGIC(TEST):  Parameter VAR has value weight
    MLOGIC(TEST):  Parameter INP has value class
    MLOGIC(TEST):  Parameter STAT has value min q1 mean median max
    MLOGIC(TEST.GENCODE):  Beginning execution.
    MLOGIC(TEST.GENCODE):  Parameter LOWER has value lower
    MLOGIC(TEST.GENCODE):  %GLOBAL  LWR
    MLOGIC(TEST.GENCODE):  %LET (variable name is LWR)
    SYMBOLGEN:  Macro variable LOWER resolves to lower
    MLOGIC(TEST.GENCODE):  Ending execution.
    MPRINT(TEST):  ;
    MLOGIC(TEST):  %PUT &=lwr
    SYMBOLGEN:  Macro variable LWR resolves to lower
    LWR=lower
    SYMBOLGEN:  Macro variable INP resolves to class
    MLOGIC(TEST):  %IF condition %upcase(&inp) = CLASS is TRUE
    MLOGIC(TEST):  %LET (variable name is LIB)
    MLOGIC(TEST):  %LET (variable name is INP)
    SYMBOLGEN:  Macro variable LIB resolves to sashelp
    SYMBOLGEN:  Macro variable INP resolves to class
    SYMBOLGEN:  Macro variable INP resolves to sashelp.class
    SYMBOLGEN:  Macro variable STAT resolves to min q1 mean median max
    MPRINT(TEST):   proc means data=sashelp.class min q1 mean median max ;
    SYMBOLGEN:  Macro variable VAR resolves to weight
    MPRINT(TEST):   var weight;
    MPRINT(TEST):   run;
    run;
    ;;;;
    run;quit;
    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    /*----  from the log                                                     ----*/

      %macro gencode(lower);
        %global lwr;
        %let lwr=&lower;
      %mend gencode;
      %macro test(inp=class,var=age,stat=min q1 mean median max);
        %gencode(lower);
        %put &=lwr;
        %if %upcase(&inp) = CLASS %then %do;
            %let lib=sashelp;
            %let inp=&lib..&inp;
        %end;
        proc means data=&inp &stat ;
          var &var;
        run;quit;
      %mend test;
      %test(var=weight);
    ;
    proc means data=sashelp.class min q1 mean median max ;
    var weight;
    run;

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
