The program was written using SAS Enterprise Guide. The first step was to create "Session_id" and "Date" variables. In order to have "Date" variable in the needed format, I set up a "fordate" format that changes formats of date variables into 'YYYY-mm-dd HH:MM'.  The second step was to initialise input data. At first, I did this step with datalines, however later I changed this method into macro as it would be easier to run this program in the future. Then, using data step procedure I created all the remaining variables and final results. 

Steps of how to run this program:
1. Run the script via enterprise guide
2. To add another client you only need to run %ModelSAS(nb, 'id' ,field1, field2, field3 , field4, field5 , field6, field7, field8 ,field9, field10); where 'nb' shows the number of a client in a queue (it's best to choose a number that would be bigger by 1 than previous value of nb) and other variables are stated in "Main requirements" table.

Additional information:
  1. Because keeping "model" and "Date" variables before fields and factors seemed more meaningful to me, I changed the order of variables in a Database (OUTPUT INFORMATION point 2) that I called "All_inputs" table.
  2. Totals are mostly equal to 1 because of a formula: TOTAL=EXP(x)/(EXP(x)+1) and because of input data: for 1,3 and 4th cases I used the same data as shown in the example and still for these cases I get x=225.5, 225.5 and 198 respectively, thus EXP(x)/(EXP(x)+1 converges into 1, for another 2 cases I used small values and received Total < 1. Maybe the formula should be without exp?

Sorry, that I did this exercise not via REST API. I tried to do it with it and created an account for Jupyter in SAS.com, where I found a python script that creates an .authinfo file that is required for saspy to execute the SAS code. However, at first I received an error of "No such file or directory", when I changed my credentials into lowercase but the script has been running for almost 2 hours and still hasn't finished, thus I chosen to upload script that I wrote using SAS Enterprise guide.

