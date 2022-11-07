# FitBit-Tracking-Data-Analysis
The objective of this analysis is to observe current trends in Fitbit tracking data and apply them to Bellabeat’s Time device to improve product development and marketability.

## Bellabeat_Analysis_Jetty2022

![image](https://user-images.githubusercontent.com/71042140/200430998-67416e74-c49c-44b9-9599-2941a85d0e00.png)

### INTRODUCTION
Founded in 2013 by Urska Srsen and Sando Mur, Bellabeat is a high-tech organization that manufactures smart-devices to help you take your health into your own hands. Bellabeat’s rapid success can be attributed, but not limited to, its devices’ beautiful designs, state-of-the-art engineering, and ease of use for customers. The efficient designs and smart technology encourage wellness by allowing individuals to track their daily health habits automatically. Bellabeat is constantly working to improve product efficacy so that users get the most out of their products, and make healthier decisions. Through this analysis, we hope to gain insights into smart-technology use and implement findings to maximize growth for the company.

### MEET THE STAKEHOLDERS
Urska Srsen: Founder Sando Mur: Co-Founder

KEY PRODUCT

![image](https://user-images.githubusercontent.com/71042140/200431048-2a068922-ae30-490f-b5d7-d6e38eda0cbf.png)


Time is Bellabeat’s very own wellness-tracking device. This wellness watch allows for easy wearability and uses smart technology to track sleep, stress, and user activity. Once activated, Time automatically connects to your account on the Bellabeat app, allowing for continuity and easy health tracking for each user. Why should this matter? Well, we noticed an increase in demand for health-tracking devices that automatically collect data without users having to manually input it. Wearable devices are also becoming increasingly popular because they can be worn as jewelry, and customized. This gives wearable health-tracking devices an advantage and encourages use.

### OBJECTIVE
The objective of this analysis is to observe current trends in Fitbit tracking data and apply them to Bellabeat’s Time device to improve product development and marketability. Time is Bellabeat’s wellness-tracking device that works similar to the FitBit. They are both wearable and customized smart-watches that track heart rate, sleep, stress, and calorie data; they are both worn and utilized in almost-identical fashions. Due to this, the FitBit Tracking data will provide insights that are similar to Time. Utilizing this data can help improve Time’s development and guide future marketing strategies to increase user-satisfaction and maximize profit for Bellabeat investors.

### RESOURCES

The data we will use for this project is called “FitBit Fitness Tracker Data”, a public domain, Kaggle dataset made available through Mobius. This dataset focuses on 30 approved FitBit users who have consented to the submission and use of their data in our data analysis. Some parameters this dataset tracks from its 30 users are but not limited to:

  - Tracking Distance

  - Calories burned

  - Minutes of Activity

  - Sedentary Minutes

  - Date of activity

  - Intensity of Activity

  - User ID

The FitBit Fitness Tracking Dataset contains 18 related files, however, this analysis will focus on 3 subsets: Daily activity, calories daily, and weight log info merged.The following files have been used for the project:

dailyActivity_merged

weightLogInfo_merged

dailyCalories_merged

### DATA QUALITY
The FitBit Fitness-Tracking Data is second-party, external data. This data is an open-source dataset and free for use. FitBit is automatically recording and storing this data after collection, therefore, users were not able to manually manipulate or change their health data. This dataset does not include any personal identifiable information, therefore, data anonymization is not required by us. This dataset’s small sample size of 30 individuals meets the minimum requirements of the Central Limit Theorem for a good statistical analysis, however, it is not a large enough sample to accurately reflect the 10 million+ population of FitBit users. Our incredibly small sample size would lead to a margin of error of over 15%. Furthermore, the data in this dataset has been collected in 2016, which can set back our analysis by 8 years. The data is organized in a long format; this will be manipulated for use as fit later on.

#### DATA PREPARATION & CLEANING

Loading necessary packages

library("lubridate")
library("dplyr")
library("ggplot2")
library("tidyr")
library("skimr")
library("janitor")
library("data.table")

#### File Naming

daily_activity_merged <- dailyActivity_merged

merged_weight <- weightLogInfo_merged

daily_calories <- dailyCalories_merged

#### View summary statistics

head(daily_activity_merged)

# A tibble: 6 × 15
          Id ActivityDate TotalSteps TotalDistance TrackerDistance LoggedAct…¹ VeryA…² Moder…³ Light…⁴ Seden…⁵ VeryA…⁶ Fairl…⁷ Light…⁸ Seden…⁹ Calor…˟
       <dbl> <chr>             <dbl>         <dbl>           <dbl>       <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
1 1503960366 4/12/2016         13162          8.5             8.5            0    1.88   0.550    6.06       0      25      13     328     728    1985
2 1503960366 4/13/2016         10735          6.97            6.97           0    1.57   0.690    4.71       0      21      19     217     776    1797
3 1503960366 4/14/2016         10460          6.74            6.74           0    2.44   0.400    3.91       0      30      11     181    1218    1776
4 1503960366 4/15/2016          9762          6.28            6.28           0    2.14   1.26     2.83       0      29      34     209     726    1745
5 1503960366 4/16/2016         12669          8.16            8.16           0    2.71   0.410    5.04       0      36      10     221     773    1863
6 1503960366 4/17/2016          9705          6.48            6.48           0    3.19   0.780    2.51       0      38      20     164     539    1728
# … with abbreviated variable names ¹LoggedActivitiesDistance, ²VeryActiveDistance, ³ModeratelyActiveDistance, ⁴LightActiveDistance,
#   ⁵SedentaryActiveDistance, ⁶VeryActiveMinutes, ⁷FairlyActiveMinutes, ⁸LightlyActiveMinutes, ⁹SedentaryMinutes, ˟Calories
head(merged_weight)

# A tibble: 6 × 8
          Id Date                  WeightKg WeightPounds   Fat   BMI IsManualReport         LogId
       <dbl> <chr>                    <dbl>        <dbl> <dbl> <dbl> <lgl>                  <dbl>
1 1503960366 5/2/2016 11:59:59 PM      52.6         116.    22  22.6 TRUE           1462233599000
2 1503960366 5/3/2016 11:59:59 PM      52.6         116.    NA  22.6 TRUE           1462319999000
3 1927972279 4/13/2016 1:08:52 AM     134.          294.    NA  47.5 FALSE          1460509732000
4 2873212765 4/21/2016 11:59:59 PM     56.7         125.    NA  21.5 TRUE           1461283199000
5 2873212765 5/12/2016 11:59:59 PM     57.3         126.    NA  21.7 TRUE           1463097599000
6 4319703577 4/17/2016 11:59:59 PM     72.4         160.    25  27.5 TRUE           1460937599000
head(daily_calories)

# A tibble: 6 × 3
          Id ActivityDay Calories
       <dbl> <chr>          <dbl>
1 1503960366 4/12/2016       1985
2 1503960366 4/13/2016       1797
3 1503960366 4/14/2016       1776
4 1503960366 4/15/2016       1745
5 1503960366 4/16/2016       1863
6 1503960366 4/17/2016       1728

colnames(daily_activity_merged)
 [1] "Id"                       "ActivityDate"             "TotalSteps"               "TotalDistance"            "TrackerDistance"         
 [6] "LoggedActivitiesDistance" "VeryActiveDistance"       "ModeratelyActiveDistance" "LightActiveDistance"      "SedentaryActiveDistance" 
[11] "VeryActiveMinutes"        "FairlyActiveMinutes"      "LightlyActiveMinutes"     "SedentaryMinutes"         "Calories" 

colnames(merged_weight)
[1] "Id"             "Date"           "WeightKg"       "WeightPounds"   "Fat"            "BMI"            "IsManualReport" "LogId"     

colnames(daily_calories)
[1] "Id"          "ActivityDay" "Calories"  
n_distinct(daily_activity_merged$Id)

[1] 33
n_distinct(merged_weight$Id)

[1] 8
n_distinct(daily_calories$Id)

[1] 33


From this initial analysis, it appears that not everyone has provided data for each data set. Though the number of distinct participants in both the daily_activity_merged & daily_calories datasets are equal (33 participants), merged_weight only has 8.From this initial analysis, it appears that not everyone has provided data for each data set. The total 33 participants from this data set seems to differ by 3 individuals from the metadata on the Fitbit Tracking data itself, which claims to have only 30 participants. This calls into further question the credibility and accuracy of this data set.

While looking through the data sets, I noticed there were some zero values that added nothing to the analysis due to the insufficient or non existing data. This would skew the results, therefore, I removed them.

#### Filtering for only non-zero values

new_daily_activity <- daily_activity_merged %>%

filter(TotalSteps != 0)

view(new_daily_activity)

new_weight <- merged_weight %>%

filter(WeightKg != 0)

view(new_weight)

new_calories <- daily_calories %>%

filter(Calories != 0)

view(new_calories)

I noticed that in the Weight data set, the date and time were meshed into one column. An updated weight data set was created to fix this.

weight_new <- new_weight %>%

separate(Date, c("Date", "Time"), " ")

view(weight_new)

Warning message:
Expected 2 pieces. Additional pieces discarded in 67 rows [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, ...]. 
n_distinct(weight_new$Date)

[1] 31
n_distinct(weight_new$Time)

[1] 26
Cleaning up duplicate values

In order to be safe and ensure there are no skewed results in the analysis, I cleaned the data by checking if there are any duplicates.

nrow(new_daily_activity)

[1] 863
nrow(weight_new)

[1] 67
nrow(new_calories)

[1] 936
It looks like the values for each respective data set match up.

nrow(unique(new_daily_activity))

[1] 863
nrow(unique(weight_new))

[1] 67
nrow(unique(new_calories))

[1] 936


The data cleaning process is now complete. All the values in each of the 3 data sets are unique and viable values that are ready for analysis. It is also worth mentioning that there are only 30 non-zero values in the LoggedActivitiesDistance column of the unique_daily_activity data set. This will greatly skew results as most of the values in this column were not logged by the individual.

### Analysis
Now that the data is cleaned, I will now begin the analysis process. By identifying patterns and relationships between data sets and their values within, I hope to gain insights about the data to help improve BellaBeat’s business model and increase profits.

Overview of data sets to ensure data is ready:

skim_without_charts(new_daily_activity)

── Data Summary ────────────────────────
                           Values            
Name                       new_daily_activity
Number of rows             863               
Number of columns          15                
_______________________                      
Column type frequency:                       
  character                1                 
  numeric                  14                
________________________                     
Group variables            None              

── Variable type: character ──────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  skim_variable n_missing complete_rate min max empty n_unique whitespace
1 ActivityDate          0             1   8   9     0       31          0

── Variable type: numeric ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   skim_variable            n_missing complete_rate    mean      sd         p0           p25     p50     p75    p100
 1 Id                               0             1 4.86e+9 2.42e+9 1503960366 2320127002    4.45e+9 6.96e+9 8.88e+9
 2 TotalSteps                       0             1 8.32e+3 4.74e+3          4       4923    8.05e+3 1.11e+4 3.60e+4
 3 TotalDistance                    0             1 5.98e+0 3.72e+0          0          3.37 5.59e+0 7.90e+0 2.80e+1
 4 TrackerDistance                  0             1 5.96e+0 3.70e+0          0          3.37 5.59e+0 7.88e+0 2.80e+1
 5 LoggedActivitiesDistance         0             1 1.18e-1 6.46e-1          0          0    0       0       4.94e+0
 6 VeryActiveDistance               0             1 1.64e+0 2.74e+0          0          0    4.10e-1 2.27e+0 2.19e+1
 7 ModeratelyActiveDistance         0             1 6.18e-1 9.05e-1          0          0    3.10e-1 8.65e-1 6.48e+0
 8 LightActiveDistance              0             1 3.64e+0 1.86e+0          0          2.34 3.58e+0 4.89e+0 1.07e+1
 9 SedentaryActiveDistance          0             1 1.75e-3 7.65e-3          0          0    0       0       1.10e-1
10 VeryActiveMinutes                0             1 2.30e+1 3.36e+1          0          0    7   e+0 3.5 e+1 2.1 e+2
11 FairlyActiveMinutes              0             1 1.48e+1 2.04e+1          0          0    8   e+0 2.1 e+1 1.43e+2
12 LightlyActiveMinutes             0             1 2.10e+2 9.68e+1          0        146.   2.08e+2 2.72e+2 5.18e+2
13 SedentaryMinutes                 0             1 9.56e+2 2.80e+2          0        722.   1.02e+3 1.19e+3 1.44e+3
14 Calories                         0             1 2.36e+3 7.03e+2         52       1856.   2.22e+3 2.83e+3 4.9 e+3
skim_without_charts(weight_new)

── Data Summary ────────────────────────
                           Values    
Name                       weight_new
Number of rows             67        
Number of columns          9         
_______________________              
Column type frequency:               
  character                2         
  logical                  1         
  numeric                  6         
________________________             
Group variables            None      

── Variable type: character ──────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  skim_variable n_missing complete_rate min max empty n_unique whitespace
1 Date                  0             1   8   9     0       31          0
2 Time                  0             1   7   8     0       26          0

── Variable type: logical ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  skim_variable  n_missing complete_rate  mean count           
1 IsManualReport         0             1 0.612 TRU: 41, FAL: 26

── Variable type: numeric ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  skim_variable n_missing complete_rate    mean            sd      p0     p25     p50     p75    p100
1 Id                    0        1      7.01e 9 1950321944.   1.50e 9 6.96e 9 6.96e 9 8.88e 9 8.88e 9
2 WeightKg              0        1      7.20e 1         13.9  5.26e 1 6.14e 1 6.25e 1 8.50e 1 1.34e 2
3 WeightPounds          0        1      1.59e 2         30.7  1.16e 2 1.35e 2 1.38e 2 1.88e 2 2.94e 2
4 Fat                  65        0.0299 2.35e 1          2.12 2.2 e 1 2.28e 1 2.35e 1 2.42e 1 2.5 e 1
5 BMI                   0        1      2.52e 1          3.07 2.15e 1 2.40e 1 2.44e 1 2.56e 1 4.75e 1
6 LogId                 0        1      1.46e12  782994784.   1.46e12 1.46e12 1.46e12 1.46e12 1.46e12
skim_without_charts(new_calories)

── Data Summary ────────────────────────
                           Values      
Name                       new_calories
Number of rows             936         
Number of columns          3           
_______________________                
Column type frequency:                 
  character                1           
  numeric                  2           
________________________               
Group variables            None        

── Variable type: character ──────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  skim_variable n_missing complete_rate min max empty n_unique whitespace
1 ActivityDay           0             1   8   9     0       31          0

── Variable type: numeric ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  skim_variable n_missing complete_rate        mean          sd         p0        p25        p50         p75       p100
1 Id                    0             1 4849840870. 2421440107. 1503960366 2320127002 4445114986 6962181067  8877689391
2 Calories              0             1       2313.        704.         52       1834       2144       2794.       4900
The data is all in the correct format for use, however, I want to narrow my data sets down to only include the columns and rows that are more specific to my analysis and the questions I am asking.

#### Condensing data sets

Condense Daily Activity data set

v4final_daily_activity <- new_daily_activity %>%

select(Id, ActivityDate, TotalSteps, TotalDistance, VeryActiveMinutes, FairlyActiveMinutes,     LightlyActiveMinutes, SedentaryMinutes, Calories) %>%

rename(Date = ActivityDate)

I renamed the ActivityDate column to “Date” to make it easier to read.

Condense Weight data set

final_weight <- weight_new %>%

select(Id, Date, WeightPounds, BMI, IsManualReport)

view(final_weight)

Condense Calories data set

final_calories <- new_calories %>%

select(Id, ActivityDay, Calories) %>%

rename(Date = ActivityDay)

view(final_calories)

I renamed the ActivityDay column to “Date” to make it easier to read and consistent with other data sets in this study.

#### Now that I have created new data sets with only pertinent data, I can now look at summary statistics to gain better insights into these data sets.

summary(v4final_daily_activity)

      Id                Date             TotalSteps    TotalDistance   VeryActiveMinutes ModeratelyActiveDistance LightlyActiveMinutes
 Min.   :1.504e+09   Length:863         Min.   :    4   Min.   : 0.00   Min.   :  0.00    Min.   :0.0000           Min.   :  0.0       
 1st Qu.:2.320e+09   Class :character   1st Qu.: 4923   1st Qu.: 3.37   1st Qu.:  0.00    1st Qu.:0.0000           1st Qu.:146.5       
 Median :4.445e+09   Mode  :character   Median : 8053   Median : 5.59   Median :  7.00    Median :0.3100           Median :208.0       
 Mean   :4.858e+09                      Mean   : 8319   Mean   : 5.98   Mean   : 23.02    Mean   :0.6182           Mean   :210.0       
 3rd Qu.:6.962e+09                      3rd Qu.:11092   3rd Qu.: 7.90   3rd Qu.: 35.00    3rd Qu.:0.8650           3rd Qu.:272.0       
 Max.   :8.878e+09                      Max.   :36019   Max.   :28.03   Max.   :210.00    Max.   :6.4800           Max.   :518.0       
 SedentaryMinutes    Calories   
 Min.   :   0.0   Min.   :  52  
 1st Qu.: 721.5   1st Qu.:1856  
 Median :1021.0   Median :2220  
 Mean   : 955.8   Mean   :2361  
 3rd Qu.:1189.0   3rd Qu.:2832  
 Max.   :1440.0   Max.   :4900  
summary(final_weight)

Id                Date            WeightPounds        BMI        IsManualReport 
 Min.   :1.504e+09   Length:67          Min.   :116.0   Min.   :21.45   Mode :logical  
 1st Qu.:6.962e+09   Class :character   1st Qu.:135.4   1st Qu.:23.96   FALSE:26       
 Median :6.962e+09   Mode  :character   Median :137.8   Median :24.39   TRUE :41       
 Mean   :7.009e+09                      Mean   :158.8   Mean   :25.19                  
 3rd Qu.:8.878e+09                      3rd Qu.:187.5   3rd Qu.:25.56                  
 Max.   :8.878e+09                      Max.   :294.3   Max.   :47.54 
summary(final_calories)

Id                Date              Calories   
 Min.   :1.504e+09   Length:936         Min.   :  52  
 1st Qu.:2.320e+09   Class :character   1st Qu.:1834  
 Median :4.445e+09   Mode  :character   Median :2144  
 Mean   :4.850e+09                      Mean   :2313  
 3rd Qu.:6.962e+09                      3rd Qu.:2794  
 Max.   :8.878e+09                      Max.   :4900  



Before I can look into the summary statistics, I noticed the Date columns in the final_daily_activity and final_calories data sets were not interpreted as numeric values. To confirm this, I checked the class type of the date column.

class(v4final_daily_activity)

[1] "character"
class(final_calories)

[1] "character"


As suspected, the Date columns are in “character” when they should be in numeric date formats. I then created new data sets to fix this change. The new date format in daily activity will be in the YYYY-MM-DD format.

Fixing date in daily activity data set:

fDate <- as.character.Date(final_daily_activity$Date)

final_daily_activity$fDate <- fDate

view(final_daily_activity)

New summary for updated final_daily_activity data set:

summary(v4final_daily_activity)

Id                Date             TotalSteps    TotalDistance   VeryActiveMinutes FairlyActiveMinutes LightlyActiveMinutes
 Min.   :1.504e+09   Length:863         Min.   :    4   Min.   : 0.00   Min.   :  0.00    Min.   :  0.00      Min.   :  0.0       
 1st Qu.:2.320e+09   Class :character   1st Qu.: 4923   1st Qu.: 3.37   1st Qu.:  0.00    1st Qu.:  0.00      1st Qu.:146.5       
 Median :4.445e+09   Mode  :character   Median : 8053   Median : 5.59   Median :  7.00    Median :  8.00      Median :208.0       
 Mean   :4.858e+09                      Mean   : 8319   Mean   : 5.98   Mean   : 23.02    Mean   : 14.78      Mean   :210.0       
 3rd Qu.:6.962e+09                      3rd Qu.:11092   3rd Qu.: 7.90   3rd Qu.: 35.00    3rd Qu.: 21.00      3rd Qu.:272.0       
 Max.   :8.878e+09                      Max.   :36019   Max.   :28.03   Max.   :210.00    Max.   :143.00      Max.   :518.0       
 SedentaryMinutes    Calories   
 Min.   :   0.0   Min.   :  52  
 1st Qu.: 721.5   1st Qu.:1856  
 Median :1021.0   Median :2220  
 Mean   : 955.8   Mean   :2361  
 3rd Qu.:1189.0   3rd Qu.:2832  
 Max.   :1440.0   Max.   :4900 
Observed Trends

        - From our summary statistics, I noticed the following trends:

        - The average Total Steps and individual takes is 8319 steps.

        - The average Total Distance an individual travels is 5.98 miles.

        - The average of an individual’s Very Active Minutes is 23.02 minutes.

        - The average of an individual’s Fairly Active Minutes is 14.78 minutes.

        - The average of an individual’s Lightly Active Minutes is 210.0 minutes.

        - The average of an individual’s Sedentary Minutes is 955.8 minutes.

        - The average calories an individual burns daily is 2361 kCal.

        - The average Weight in pounds of an individual is 158.8 lbs.

        - The average BMI of an indivudal is 25.19.

        - The average number of Calories an individual burns is 2313 kCal.

#### Analyzing Trends
According to the Cleveland Health Clinic, the recommended healthy BMI of a male should be between 18.5 to 24.9 The recommended healthy BMI of a female should be between 18.2 - 23.9. From our data, we cannot conclude whether the individuals we are studying from the FitBit Tracking data fall within that range, because the individual’s sex was not recorded in any of the used data sets. However, the average BMI for all individuals reported was 25.9, which falls above the recommended healthy range for both men and women. It is also important to note that BMI is not the best indicator of obesity or health.

According to the National Agriculture Library, the ideal number of calories to burn per day to lose 1 pound of fat is around 3500 calories. From our summary statistics, we can see that the average calories an individual burns was 2313. Therefore, it is safe to assume that our individuals are burning below the recommended caloric count to burn 1 pound of fat. A limitation of this data is that each individual has different goals for health an exercise; some people may not need to burn 3500 calories per day, some may need to burn more depending on their goals. Therefore, this information on calories burned cannot give us deeper insights into whether or not burning the average 2313 calories is sufficient or not.

It is also important to note that perhaps the individuals in this study are buying and using the FitBit because they are overweight; generally, individuals who are aiming tot rack their progress and have larger weight goals will buy health-tracking smart devices to track their progress. It can be seen as a positive thing that “overweight” individuals according to our study will be more likely to purchase these smart devices. It could be beneficial to target ads for BellaBeat Time device for those who are overweight or above the recommended range for BMI.

#### Data Visualization
Now that we have drawn certain conclusions based on our data summaries, I created plots to visualize our data.

I want to see how much of the day is spent active, and how active that time is. I created a pie chart to help visualize this.

#Creating Pie chart
VeryActive <- sum(v4final_daily_activity$VeryActiveMinutes)
FairlyActive <- sum(v4final_daily_activity$FairlyActiveMinutes)
LightActive <- sum(v4final_daily_activity$LightlyActiveMinutes)
Sedentary <- sum(v4final_daily_activity$SedentaryMinutes)
TotalMin <- VeryActive + FairlyActive + LightActive + Sedentary
 
#adding percentages
slices <- c(VeryActive, FairlyActive, LightActive, Sedentary)
titles <- c("Very Active Minutes", "Fairly Active Minutes", "Lightly Active Minutes", "Sedentary Minutes")
pct <- round(slices/sum(slices)*100)
titles <- paste(titles, pct) # add percents to labels
titles <- paste(titles,"%",sep="") # ad % to labels
 
pie(slices,labels = titles, col=rainbow(length(titles)),
+     main="Percentage of Activity in Minutes")

![image](https://user-images.githubusercontent.com/71042140/200431587-5f0e875b-97cb-47fc-9cd9-61d34b0b66e0.png)


From this chart, it can be seen that most of the user’s minutes are spent sedentary. The FitBit is showing that users, despite purchasing smart devices to track their health, are not nearly active enough as 79% of their minutes are spent inactive.

I want to see if calories burned has any relationship with Very Active Minutes and Total distance traveled. To visualize this, I created scatter plots:

ggplot(data = v4final_daily_activity) +
     geom_point(mapping = aes(x = Calories, y = TotalSteps), alpha = 0.3, color = "blue") +
     geom_smooth(mapping = aes(x = Calories, y = TotalSteps)) +
     labs(title = "Trend between Total Steps & Calories Burned")
`geom_smooth()` using method = 'loess' and formula 'y ~ x'
Scatter Plots

![image](https://user-images.githubusercontent.com/71042140/200431642-d5d33f68-0820-4543-9b77-88f71d7b6638.png)


ggplot(data = v4final_daily_activity) +
     geom_point(mapping = aes(x = Calories, y = TotalDistance), alpha = 0.3, color = "red") +
     geom_smooth(mapping = aes(x = Calories, y = TotalDistance)) +
     labs(title = "Trend between Total Distance & Calories Burned")
`geom_smooth()` using method = 'loess' and formula 'y ~ x'

![image](https://user-images.githubusercontent.com/71042140/200431678-01518980-f2bf-4534-83d2-e8279cdc7c04.png)


I noticed that as the total steps taken increases, the amount of calories burned also increase. We see a positive relationship between these variables, which signals to us that the more active you are, the more calories you burn.

Now, I want to observe how much of the Total BMI consists of individuals who are overweight and underweight

Overweight: (defined as above 23.5 BMI)

 Overweight <- sum(final_weight$BMI > 23.5)
> TotalBMI <- sum(final_weight$BMI)
> 
> #adding percentages
> slices <- c(Overweight, TotalBMI)
> titles <- c("Overweight", "TotalBMI")
> pct <- round(slices/sum(slices)*100)
> titles <- paste(titles, pct) # add percents to labels
> titles <- paste(titles,"%",sep="") # ad % to labels
> 
> pie(slices,labels = titles, col=rainbow(length(titles)),
+     main="Percentage of People Over the Healthy BMI Range")

![image](https://user-images.githubusercontent.com/71042140/200431744-1d57cded-fbbb-4aa6-b0c5-5e2969c3d5de.png)

Only 4% of all individuals in the study are overweight.

Underweight: (defined as below 18.5 BMI):

Underweight <- sum(final_weight$BMI < 18.5)
> TotalBMI <- sum(final_weight$BMI)
> 
> #adding percentages
> slices <- c(Underweight, TotalBMI)
> titles <- c("Under Weight", "TotalBMI")
> pct <- round(slices/sum(slices)*100)
> titles <- paste(titles, pct) # add percents to labels
> titles <- paste(titles,"%",sep="") # ad % to labels
> 
> pie(slices,labels = titles, col=rainbow(length(titles)),
+     main="Percentage of People with Under Weight BMI")

![image](https://user-images.githubusercontent.com/71042140/200431776-9c6d1e1d-7bcd-462a-8307-9956ea26fe78.png)



0% of people in this study are underweight.

I also wanted to compare the weights of the individuals to the sedentary minutes. In order to do this, I have to add the WeightKg column from our weight data set to the daily_activity data set. However, the weight data set only has 67 available rows, whereas the daily activity set has 863 rows, therefore, we cannot attempt to draw any conclusions between weight and sedentary minutes. That is another flaw in the credibility of the data.

ACT
The business task defined earlier in this study was to observe current trends in Fitbit tracking data and apply them to Bellabeat’s Time device to improve product development and marketability. The purpose of this analysis is to find data to give us insights into how to improve this business model. By creating a targeted approach to developing and selling the Time watch by BellaBeat, we can improve the company’s profits and cut losses where needed. This would make Bellabeat’s marketing and business strategy much more efficient.

Trends & Recommendations for the Future:

Based on the analysis of our trends and visualizations shown earlier, none of the users from the study using the FitBit are underweight. If anything, the only users that fall outside the healthy range of BMI are overweight (4% of all users). Though the average BMI of all individuals in this study is overweight (25.19), it would be wise to target BellaBeat Time watch advertisements to healthy individuals and those who are overweight. It can be inferred that overweight individuals will purchase to become more healthy whereas already healthy individuals will buy Time to maintain their progress and health.

This conclusion is further supported by the conclusion that the average individual in this study burns only 2313 calories per day whereas the recommended amount of calories to burn to lose 1 pound of fat is 3500. The average total steps an individual has taken is only 8319, whereas Medical News Today recommends an average of 10,000 per day.

Therefore, it is safe to assume that the individuals in this study are not getting an adequate amount of exercise. Perhaps they might not know they are not getting enough, which is why purchasing the Time device would encourage them to keep track of their movement. Furthermore, it could benefit the company to add a feature to the Time device that alerts the user when they have not received the adequate goal of Total steps, calories burned, BMI, and weight; this can ensure users know where they stand and be more proactive about whats best for them.

A big flaw in our data is the inability for us to reference what exactly is healthy for each user. As weight and height vary from each person, they are the primary factors in determining what healthy weights, BMI’s, Total Steps and Minutes of varying activity ranges are suitable to them. I suggest Bellabeat add a feature to the Time watch that allows users to input their body statistics such as sex, height, weight, calorie goals, and so forth so that the data logged is representative of each unique individual. This will also help users feel like Time is personalized and truly catered to help them reach their personal goals.

Another suggestion is that Time should automatically detect heart rate so that it can use an algorithim to determine when an individual is performing varying levels of activity, being sedentary, and also sleeping. Relying on the user to self-report these statistics can majorly skew the results. Therefore, automating this data collection process will refine the watch’s performance and give BellaBeat more accurate insights.
