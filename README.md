# Data-Pipeline

### Report :
1.	Computation Strategy
•	brazil_covid19.csv have date,region,state,cases,deaths columns while brazil_covid19_cities.csv have date,state,name,code,cases,deaths columns
•	Select  only date,state,cases,deaths columns from brazil_covid19_cities.csv and group based on date and state to find aggregate of cases and deaths
•	brazil_covid19_cities.csv do not have region field. Select distinct state and region from brazil_covid19.csv and join with brazil_covid19_cities.csv(which is now aggregated based on date and state) based on state to get region column.
•	Save above dataframe to  new_brazil_covid19.csv. Now this dataset will have same structure as  brazil_covid19.csv.



2.	strategy of comparison of new_brazil_covid19.csv and brazil_covid19.csv : by assuming following -> Source: brazil_covid19.csv , Destination: new_brazil_covid19.csv


•	load above 2 datasets and add 1 addition column “date_state” which is a concatination of date and state. Say we have dataset as readSource and readDestination
•	To get source count and destination count,  use below 
•	readSource.count and readDestination.count
•	To get number of rows present in source but not in destination, use below
•	do left join readSource and  readDestination based on date_state and filter date_state from readDestination df as null i.e. readSource.join(readDestination,readSource("date_state") === readDestination("date_state"),"left").filter(readDestination("date_state").isNull)
•	To get number of rows present in destination but not in source , use below
•	do left join readDestination and  readSource based on date_state and filter date_state from readSource df as null i.e. readDestination.join(readSource,readSource("date_state") === readDestination("date_state"),"left").filter(readSource("date_state").isNull)
•	To get number of rows identical in source and destination - sameCasesAndDeaths
•	Do inner join  between readDestination and  readSource based on date_state,cases,deaths
•	To get number of rows with different values in source and destination
•	Do inner join  between readDestination and  readSource based on date_state
•	subtract its output with above output(sameCasesAndDeaths)

instructions 

    •  clone the github 
     
        git clone "copied git clone hyperlink"
        
    • how to run the batch using the maven/mill command locally
      For development, maven is used insted of mill. To run code from intellij using maven, follow below instructions.
        1. Add below parameters in intellij Config argument
          1. For generating new_brazil_covid19.csv
            path for brazil_covid19_cities.csv
            path for brazil_covid19.csv
            path where new_brazil_covid19.csv has to be saved
          2. For generating report
            path for brazil_covid19.csv
            path for new_brazil_covid19.csv
            path where report-diff.json to be saved
            
    • package automatically the jar file 
      1.  Import project using Intellij and use maven "mvn clean compile package" command to generate jar file
      
    • how to run the batch using the spark-submit command locally (considering spark home is configured in path)
      1. for new_brazil_covid19.csv
         spark-submit  \
         --class com.spark.scala.job.MainDriver \
         path_to_jar/MAIN_DRIVER.jar \
         path for brazil_covid19_cities.csv \
         path for brazil_covid19.csv \
         path where new_brazil_covid19.csv has to be saved
         
    • how to run the batch using the spark-submit command with AWS 
    • how to generate the diff report locally (considering spark home is configured in path)
         spark-submit  \
         --class com.spark.scala.job.Report \
         path_to_jar/MAIN_DRIVER.jar \
         path for brazil_covid19.csv
         path for new_brazil_covid19.csv
         path where report-diff.json to be saved
    
    
    
    
   