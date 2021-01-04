/*Creating Session_id */
%let Session_id='202101041513TST523';
%put &Session_id;


/*Creating Date variable with stated format*/
proc format;
	picture fordate
	other='%Y-%0m-%0d %0H:%0M' (datatype=datetime)
	;
run;

data _NULL_;
	a = datetime();
	call symput('Date', put(a,fordate.));
run;

%put &Date;




/**************************************************************/
/*The first method for setting input data. Won't be used later*/ 
/**************************************************************/

/*data input;*/
/*input id $14. Field1 Field2 Field3 Field4 Field5 Field6 Field7 Field8 Field9 Field10;*/
/*datalines;*/
/*11625484761584 52 1 25 5 2 0.5 3 36 58 0 */
/*11600000000001 30 2 10 10 4 0.7 5 48 75 0 */
/*11600000000002 '' 0.01 0.01 0.02 0.1 0.001 1 0.01 0.01 0.01*/
/*11900000000001 30 2 10 10 4 0.7 5 48 75 0 */
/*11900000000002 25 2 20 7 2 0.4 6 22 40 1 */
/*55900000000001 30 2 10 10 4 0.7 5 48 75 0 */
/*55900000000002 '' 1 20 7 2 0.5 3 25 30 1 */
/*;*/
/*run;*/



/*******************************************/
/*Macro that from input data creates output*/ 
/*******************************************/

%macro ModelSAS(nb, id, Field1, Field2, Field3 ,Field4, Field5, Field6 ,Field7 ,Field8 ,Field9 , Field10);
	%macro dummy;
	%mend dummy;

	data input_&nb.;
		Session_id = &Session_id;
		id= &id.;
		Date = "&Date";
		if substr(&id, 1,3) in ('116','119') then Model='A'; else Model = 'B';
		Field1=&Field1; 
		Field2=&Field2; 
		Field3=&Field3 ;
		Field4=&Field4;
		Field5=&Field5;
		Field6=&Field6 ;
		Field7=&Field7 ;
		Field8=&Field8 ;
		Field9=&Field9 ;
		Field10=&Field10;

		if &Field1. = . then Factor1=&Field2.; else Factor1=&Field1.;
		Factor2 = sum(&Field8.,&Field9.,&Field10.);
		Factor3 = &Field3.;
		Factor4 = &Field4./&Field5.;
		Factor6 = &Field6. * 100;
		if &Field7. in ('1','5','10') then Factor7 = 1; else Factor7=2;
		if Model='A' then LPT = sum(of Factor1-Factor3); else LPT = sum(Factor1, Factor2);
		if Model='A' then BEH = Factor4; else BEH = 0;
		FIN = sum(Factor6,Factor7);
		sum_of_factors = LPT + BEH + FIN;
		TOTAL=round(exp(sum_of_factors)/(exp(sum_of_factors)+1),0.01);

		/*if Model='A' then sum_of_factors = sum(of Factor1-Factor7); else sum_of_factors = sum(Factor1, Factor2, Factor6,Factor7);*/

		drop Factor5 sum_of_factors;
	run;


	data Output&nb.(keep=Session_id id Date TOTAL) ;
	set input_&nb.;
	run;

	proc append base = All_inputs data=input_&nb.;
	proc append base = All_clients data=Output&nb.;
	proc delete data = input_&nb. Output&nb.;

%mend;

%ModelSAS(1, '11625484761584' ,52, 1, 25, 5, 2, 0.5, 3, 36 ,58, 0) ;
%ModelSAS(2, '11600000000001' ,., 0.01, 0.01 ,0.02, 0.1 ,0.001, 1, 0.01 ,0.01, 0.01);
%ModelSAS(3, '11600000000001' ,52, 1, 25, 5, 2, 0.5, 3, 36 ,58, 0);
%ModelSAS(4, '55900000000001' ,52, 1, 25, 5, 2, 0.5, 3, 36 ,58, 0);
%ModelSAS(5, '55900000000001' ,., 0.01, 0.01 ,0.02, 0.1 ,0.001, 1, 0.01 ,0.01, 0.01);

