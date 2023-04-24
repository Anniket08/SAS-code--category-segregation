# SAS-code--category-segregation
This is the SAS code for category-wise segmentation using macros and proc sql

proc sql noprint;
select distinct Sex into:s separated by "@" from sasuser.admit;  */dataset name-Admit
select count(distinct sex) into:c from sasuser.admit;  */Librery name- sasuser/*
quit;

%put *&s*&c*;
%let d=%sysfunc(compress(&c));

%macro sex;

%do i=1 %to &D;

%let gender= %scan(&s,&i,"@");

proc sql noprint;
select count(*) into :n from sasuser.admit where sex= "&gender";
quit;
%let m=%sysfunc(compress(&n));
%put *&n*;

data &gender._&m;
set sasuser.admit;
where sex= "&gender";
run;

PROC EXPORT DATA= &gender._&m; 
            OUTFILE= "C:\Users\DELL\Desktop\ACD\Project1.XLXS" 
            DBMS=EXCEL REPLACE;
     SHEET="&gender._&m"; 
RUN;

%end;

%mend sex;
%sex;
