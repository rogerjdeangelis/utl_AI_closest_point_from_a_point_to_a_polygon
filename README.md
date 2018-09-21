# utl_AI_closest_point_from_a_point_to_a_polygon
AI closest point from a point to a polygon. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    AI_minimum_distance_from_a_poin_to_a_polygon

    Python is the best platform for this problem?
    You can do it in IML/R but Python is preferred?

    github
    https://tinyurl.com/y835udb5
    https://github.com/rogerjdeangelis/utl_AI_closest_point_from_a_point_to_a_polygon

    SAS Forum
    https://tinyurl.com/y7thqmnf
    https://communities.sas.com/t5/SAS-Programming/Distance-to-nearest-edge-of-polygon/m-p/497803

    Python has its own ARCGIS

    INPUT
    =====

      POLYGON ((1 1, 2 1, 2 2, 1 2, 1 1))
      POINT(0 0)


      |             (1,2)          (2,2)
    2 +               *--------------*
      |               |              |
      |               |              |
      |               |              |
      |               |              |
      |               |              |
      |               |              |
    1 +               *--------------*
      |            / (1,1)         (2,1)
      |          /
      |        /
      |      /
      |    /    sqrt(2)=1.4142
      |  /
    0 +* (0,0)
      -+--------------+--------------+
       0              1              2

                      x
    PROCESS
    ======

     %utl_submit_py64("
     from shapely import wkt;
     poly = wkt.loads('POLYGON ((1 1, 2 1, 2 2, 1 2, 1 1))');
     pt = wkt.loads('POINT(0 0)');
     dist=poly.distance(pt);
     print dist;
     ");

    OUTPUT
    ======

     1.41421356237

    *             __   _  _
     _ __  _   _ / /_ | || |
    | '_ \| | | | '_ \| || |_
    | |_) | |_| | (_) |__   _|
    | .__/ \__, |\___/   |_|
    |_|    |___/
    ;

    %macro utl_submit_py64(
          pgm
         ,return=  /* name for the macro variable from Python */
         )/des="Semi colon separated set of python commands - drop down to python";

      * write the program to a temporary file;
      filename py_pgm "%sysfunc(pathname(work))/py_pgm.py" lrecl=32766 recfm=v;
      data _null_;
        length pgm  $32755 cmd $1024;
        file py_pgm ;
        pgm=&pgm;
        semi=countc(pgm,';');
          do idx=1 to semi;
            cmd=cats(scan(pgm,idx,';'));
            if cmd=:'. ' then
               cmd=trim(substr(cmd,2));
             put cmd $char384.;
             putlog cmd $char384.;
          end;
      run;quit;
      %let _loc=%sysfunc(pathname(py_pgm));
      %put &_loc;
      filename rut pipe  "C:\Python_27_64bit/python.exe &_loc";
      data _null_;
        file print;
        infile rut;
        input;
        put _infile_;
      run;
      filename rut clear;
      filename py_pgm clear;

      * use the clipboard to create macro variable;
      %if "&return" ^= "" %then %do;
        filename clp clipbrd ;
        data _null_;
         length txt $200;
         infile clp;
         input;
         putlog "*******  " _infile_;
         call symputx("&return",_infile_,"G");
        run;quit;
      %end;

    %mend utl_submit_py64;

