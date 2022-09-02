# Storm-Data-Course-Project
This project is the final pre-graduate Assignment of "Data Science: Foundations using R Specialization" course

# introduction
Storms and other severe weather events can cause both public health and economic problems for communities and municipalities. Many severe events can result in fatalities, injuries, and property damage, and preventing such outcomes to the extent possible is a key concern.

This project involves exploring the U.S. National Oceanic and Atmospheric Administration's (NOAA) storm database. This database tracks characteristics of major storms and weather events in the United States, including when and where they occur, as well as estimates of any fatalities, injuries, and property damage.

# 1. ask phase
We need to find out by using the data:

1. Across the United States, which types of events (as indicated in the EVTYPE variable) are most harmful with respect to population health?

    
2. Across the United States, which types of events have the greatest economic consequences?

# 2.Prepare phase
The data for this assignment come in the form of a comma-separated-value file [Storm Data](https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2FStormData.csv.bz2)

The documentation on how some of the variables are constructed/defined for the database is available from below links:

  **-** National Weather Service Storm [Data Documentation](https://d396qusza40orc.cloudfront.net/repdata%2Fpeer2_doc%2Fpd01016005curr.pdf)

  **-** National Climatic Data Center Storm Events [FAQ](https://d396qusza40orc.cloudfront.net/repdata%2Fpeer2_doc%2FNCDC%20Storm%20Events-FAQ%20Page.pdf)
  
The events in the database start in the year 1950 and end in November 2011. In the earlier years of the database there are generally fewer events recorded, most likely due to a lack of good records. More recent years should be considered more complete.

# 3.Process phase

Firstly, load sone of the required packages using the library() function

```
library(tidyverse)
```
reading the data 

```
storm<-read.csv("repdata_data_StormData.csv")
```
### Understanding the Data

the names of variable

```
names(storm)
```

```
[1] "STATE__"    "BGN_DATE"   "BGN_TIME"   "TIME_ZONE"  "COUNTY"     "COUNTYNAME"
 [7] "STATE"      "EVTYPE"     "BGN_RANGE"  "BGN_AZI"    "BGN_LOCATI" "END_DATE"  
[13] "END_TIME"   "COUNTY_END" "COUNTYENDN" "END_RANGE"  "END_AZI"    "END_LOCATI"
[19] "LENGTH"     "WIDTH"      "F"          "MAG"        "FATALITIES" "INJURIES"  
[25] "PROPDMG"    "PROPDMGEXP" "CROPDMG"    "CROPDMGEXP" "WFO"        "STATEOFFIC"
[31] "ZONENAMES"  "LATITUDE"   "LONGITUDE"  "LATITUDE_E" "LONGITUDE_" "REMARKS"   
[37] "REFNUM" 
```

the structure of the data
```
str(storm)
```

```
'data.frame':	902297 obs. of  37 variables:
 $ STATE__   : num  1 1 1 1 1 1 1 1 1 1 ...
 $ BGN_DATE  : chr  "4/18/1950 0:00:00" "4/18/1950 0:00:00" "2/20/1951 0:00:00" "6/8/1951 0:00:00" ...
 $ BGN_TIME  : chr  "0130" "0145" "1600" "0900" ...
 $ TIME_ZONE : chr  "CST" "CST" "CST" "CST" ...
 $ COUNTY    : num  97 3 57 89 43 77 9 123 125 57 ...
 $ COUNTYNAME: chr  "MOBILE" "BALDWIN" "FAYETTE" "MADISON" ...
 $ STATE     : chr  "AL" "AL" "AL" "AL" ...
 $ EVTYPE    : chr  "TORNADO" "TORNADO" "TORNADO" "TORNADO" ...
 $ BGN_RANGE : num  0 0 0 0 0 0 0 0 0 0 ...
 $ BGN_AZI   : chr  "" "" "" "" ...
 $ BGN_LOCATI: chr  "" "" "" "" ...
 $ END_DATE  : chr  "" "" "" "" ...
 $ END_TIME  : chr  "" "" "" "" ...
 $ COUNTY_END: num  0 0 0 0 0 0 0 0 0 0 ...
 $ COUNTYENDN: logi  NA NA NA NA NA NA ...
 $ END_RANGE : num  0 0 0 0 0 0 0 0 0 0 ...
 $ END_AZI   : chr  "" "" "" "" ...
 $ END_LOCATI: chr  "" "" "" "" ...
 $ LENGTH    : num  14 2 0.1 0 0 1.5 1.5 0 3.3 2.3 ...
 $ WIDTH     : num  100 150 123 100 150 177 33 33 100 100 ...
 $ F         : int  3 2 2 2 2 2 2 1 3 3 ...
 $ MAG       : num  0 0 0 0 0 0 0 0 0 0 ...
 $ FATALITIES: num  0 0 0 0 0 0 0 0 1 0 ...
 $ INJURIES  : num  15 0 2 2 2 6 1 0 14 0 ...
 $ PROPDMG   : num  25 2.5 25 2.5 2.5 2.5 2.5 2.5 25 25 ...
 $ PROPDMGEXP: chr  "K" "K" "K" "K" ...
 $ CROPDMG   : num  0 0 0 0 0 0 0 0 0 0 ...
 $ CROPDMGEXP: chr  "" "" "" "" ...
 $ WFO       : chr  "" "" "" "" ...
 $ STATEOFFIC: chr  "" "" "" "" ...
 $ ZONENAMES : chr  "" "" "" "" ...
 $ LATITUDE  : num  3040 3042 3340 3458 3412 ...
 $ LONGITUDE : num  8812 8755 8742 8626 8642 ...
 $ LATITUDE_E: num  3051 0 0 0 0 ...
 $ LONGITUDE_: num  8806 0 0 0 0 ...
 $ REMARKS   : chr  "" "" "" "" ...
 $ REFNUM    : num  1 2 3 4 5 6 7 8 9 10 ...
```
summarize the data
```
summary(storm)
```

```
    STATE__       BGN_DATE           BGN_TIME          TIME_ZONE        
 Min.   : 1.0   Length:902297      Length:902297      Length:902297     
 1st Qu.:19.0   Class :character   Class :character   Class :character  
 Median :30.0   Mode  :character   Mode  :character   Mode  :character  
 Mean   :31.2                                                           
 3rd Qu.:45.0                                                           
 Max.   :95.0                                                           
                                                                        
     COUNTY       COUNTYNAME           STATE              EVTYPE         
 Min.   :  0.0   Length:902297      Length:902297      Length:902297     
 1st Qu.: 31.0   Class :character   Class :character   Class :character  
 Median : 75.0   Mode  :character   Mode  :character   Mode  :character  
 Mean   :100.6                                                           
 3rd Qu.:131.0                                                           
 Max.   :873.0                                                           
                                                                         
   BGN_RANGE          BGN_AZI           BGN_LOCATI          END_DATE        
 Min.   :   0.000   Length:902297      Length:902297      Length:902297     
 1st Qu.:   0.000   Class :character   Class :character   Class :character  
 Median :   0.000   Mode  :character   Mode  :character   Mode  :character  
 Mean   :   1.484                                                           
 3rd Qu.:   1.000                                                           
 Max.   :3749.000                                                           
                                                                            
   END_TIME           COUNTY_END COUNTYENDN       END_RANGE       
 Length:902297      Min.   :0    Mode:logical   Min.   :  0.0000  
 Class :character   1st Qu.:0    NA's:902297    1st Qu.:  0.0000  
 Mode  :character   Median :0                   Median :  0.0000  
                    Mean   :0                   Mean   :  0.9862  
                    3rd Qu.:0                   3rd Qu.:  0.0000  
                    Max.   :0                   Max.   :925.0000  
                                                                  
   END_AZI           END_LOCATI            LENGTH              WIDTH         
 Length:902297      Length:902297      Min.   :   0.0000   Min.   :   0.000  
 Class :character   Class :character   1st Qu.:   0.0000   1st Qu.:   0.000  
 Mode  :character   Mode  :character   Median :   0.0000   Median :   0.000  
                                       Mean   :   0.2301   Mean   :   7.503  
                                       3rd Qu.:   0.0000   3rd Qu.:   0.000  
                                       Max.   :2315.0000   Max.   :4400.000  
                                                                             
       F               MAG            FATALITIES          INJURIES        
 Min.   :0.0      Min.   :    0.0   Min.   :  0.0000   Min.   :   0.0000  
 1st Qu.:0.0      1st Qu.:    0.0   1st Qu.:  0.0000   1st Qu.:   0.0000  
 Median :1.0      Median :   50.0   Median :  0.0000   Median :   0.0000  
 Mean   :0.9      Mean   :   46.9   Mean   :  0.0168   Mean   :   0.1557  
 3rd Qu.:1.0      3rd Qu.:   75.0   3rd Qu.:  0.0000   3rd Qu.:   0.0000  
 Max.   :5.0      Max.   :22000.0   Max.   :583.0000   Max.   :1700.0000  
 NA's   :843563                                                           
    PROPDMG         PROPDMGEXP           CROPDMG         CROPDMGEXP       
 Min.   :   0.00   Length:902297      Min.   :  0.000   Length:902297     
 1st Qu.:   0.00   Class :character   1st Qu.:  0.000   Class :character  
 Median :   0.00   Mode  :character   Median :  0.000   Mode  :character  
 Mean   :  12.06                      Mean   :  1.527                     
 3rd Qu.:   0.50                      3rd Qu.:  0.000                     
 Max.   :5000.00                      Max.   :990.000                     
                                                                          
     WFO             STATEOFFIC         ZONENAMES            LATITUDE   
 Length:902297      Length:902297      Length:902297      Min.   :   0  
 Class :character   Class :character   Class :character   1st Qu.:2802  
 Mode  :character   Mode  :character   Mode  :character   Median :3540  
                                                          Mean   :2875  
                                                          3rd Qu.:4019  
                                                          Max.   :9706  
                                                          NA's   :47    
   LONGITUDE        LATITUDE_E     LONGITUDE_       REMARKS         
 Min.   :-14451   Min.   :   0   Min.   :-14455   Length:902297     
 1st Qu.:  7247   1st Qu.:   0   1st Qu.:     0   Class :character  
 Median :  8707   Median :   0   Median :     0   Mode  :character  
 Mean   :  6940   Mean   :1452   Mean   :  3509                     
 3rd Qu.:  9605   3rd Qu.:3549   3rd Qu.:  8735                     
 Max.   : 17124   Max.   :9706   Max.   :106220                     
                  NA's   :40                                        
     REFNUM      
 Min.   :     1  
 1st Qu.:225575  
 Median :451149  
 Mean   :451149  
 3rd Qu.:676723  
 Max.   :902297  
                 
```

### Data cleaning
Looking for NAs
```
colSums(is.na(storm))
```

```
   STATE__   BGN_DATE   BGN_TIME  TIME_ZONE     COUNTY COUNTYNAME      STATE 
         0          0          0          0          0          0          0 
    EVTYPE  BGN_RANGE    BGN_AZI BGN_LOCATI   END_DATE   END_TIME COUNTY_END 
         0          0          0          0          0          0          0 
COUNTYENDN  END_RANGE    END_AZI END_LOCATI     LENGTH      WIDTH          F 
    902297          0          0          0          0          0     843563 
       MAG FATALITIES   INJURIES    PROPDMG PROPDMGEXP    CROPDMG CROPDMGEXP 
         0          0          0          0          0          0          0 
       WFO STATEOFFIC  ZONENAMES   LATITUDE  LONGITUDE LATITUDE_E LONGITUDE_ 
         0          0          0         47          0         40          0 
   REMARKS     REFNUM 
         0          0 
```

the NAs doesn't gonna affect our work

Rename all the variable to lower case

```
storm<-storm %>% rename_all(tolower)
```
# 4.Analyze phase
Our questions required focusing on health and economic problems, so we don't need to process all the fields.

to answer the first question we need to focus on the variables "fatalities and injuries"

make a new table with the data that affect the health
```
storm_health<-storm %>% select(evtype,fatalities,injuries)
```
summation of all the fatalities and injuries per evtype

```
storm_health_type<-storm_health %>% group_by(evtype) %>% 
                   summarize(sum_of_fatalities=sum(fatalities),sum_of_injuries=sum(injuries))%>%
                   arrange(desc(sum_of_fatalities),desc(sum_of_injuries))
```
To answer the second question we need to focus on the variables "propdmg ,propdmgexp,cropdmg and cropdmgexp"

and the description of each variable is below:

**propdmg :** property damage amount.

**propdmgexp:** property damage in exponents.

**cropdmg :** crop damage amount.

**cropdmgexp:** crop damage in exponents.

make a new table with the data that affect the economy

```
storm_economy<-storm %>% select(evtype,propdmg ,propdmgexp,cropdmg,cropdmgexp)
```

this data has some exponents defined in propdmgexp and cropdmgexp, respectively. So first we have to decode these exponents. Let's look at unique variables in exps

```
unique(storm_economy$propdmgexp)
```

```
 [1] "K" "M" ""  "B" "m" "+" "0" "5" "6" "?" "4" "2" "3" "h" "7" "H" "-" "1" "8"
```

```
unique(storm_economy$cropdmgexp)
```

```
[1] ""  "M" "K" "m" "B" "?" "0" "k" "2"
```

converting the exp with their corresponding values

```
storm_economy$propdmgexp[storm_economy$propdmgexp=="K"]<-10^3
storm_economy$propdmgexp[storm_economy$propdmgexp=="M"]<-10^6
storm_economy$propdmgexp[storm_economy$propdmgexp==""]<-10^0
storm_economy$propdmgexp[storm_economy$propdmgexp=="B"]<-10^9
storm_economy$propdmgexp[storm_economy$propdmgexp=="m"]<-10^6
storm_economy$propdmgexp[storm_economy$propdmgexp=="+"]<-10^0
storm_economy$propdmgexp[storm_economy$propdmgexp=="0"]<-10^0
storm_economy$propdmgexp[storm_economy$propdmgexp=="5"]<-10^5
storm_economy$propdmgexp[storm_economy$propdmgexp=="6"]<-10^6
storm_economy$propdmgexp[storm_economy$propdmgexp=="?"]<-10^0
storm_economy$propdmgexp[storm_economy$propdmgexp=="4"]<-10^4
storm_economy$propdmgexp[storm_economy$propdmgexp=="2"]<-10^2
storm_economy$propdmgexp[storm_economy$propdmgexp=="3"]<-10^3
storm_economy$propdmgexp[storm_economy$propdmgexp=="h"]<-10^2
storm_economy$propdmgexp[storm_economy$propdmgexp=="7"]<-10^7
storm_economy$propdmgexp[storm_economy$propdmgexp=="H"]<-10^2
storm_economy$propdmgexp[storm_economy$propdmgexp=="-"]<-10^0
storm_economy$propdmgexp[storm_economy$propdmgexp=="1"]<-10^1
storm_economy$propdmgexp[storm_economy$propdmgexp=="8"]<-10^8


storm_economy$cropdmgexp[storm_economy$cropdmgexp==""]<-10^0
storm_economy$cropdmgexp[storm_economy$cropdmgexp=="M"]<-10^6
storm_economy$cropdmgexp[storm_economy$cropdmgexp=="K"]<-10^3
storm_economy$cropdmgexp[storm_economy$cropdmgexp=="m"]<-10^6
storm_economy$cropdmgexp[storm_economy$cropdmgexp=="B"]<-10^9
storm_economy$cropdmgexp[storm_economy$cropdmgexp=="?"]<-10^0
storm_economy$cropdmgexp[storm_economy$cropdmgexp=="0"]<-10^0
storm_economy$cropdmgexp[storm_economy$cropdmgexp=="k"]<-10^3
storm_economy$cropdmgexp[storm_economy$cropdmgexp=="2"]<-10^2
```

Make a new variable that calculates all damages 

```
storm_economy$damages<-((as.numeric(storm_economy$propdmg)*as.numeric(storm_economy$propdmgexp))+
(as.numeric(storm_economy$cropdmg)*as.numeric(storm_economy$cropdmgexp)))
```

summation of all the damages per evtype

```
storm_economy_type<-storm_economy %>%
                    group_by(evtype)%>%
                    summarize(all_damages=sum(damages))%>%
                    arrange(desc(all_damages))
```

# 5.Share phase

### The result

**1.1-Total Number of Fatalities of top 10 storm event types**

```
png(filename = "C:/Users/mahmo/Documents/plot1.png")
ggplot(data = storm_health_type[tail(order(storm_health_type$sum_of_fatalities),5),],aes(x=reorder(evtype,-sum_of_fatalities),y=sum_of_fatalities))+geom_col()+labs(x="storm event types",y="number of fatalities")+ggtitle("Total Number of Fatalities of top 5 storm event types")
dev.off()
```

![plot1](https://user-images.githubusercontent.com/41892582/188075797-8b70ff53-90b2-4339-b46e-6e6c63dd10d1.png)

**1.2-Total Number of injuries of top 10 storm event types**

```
png(filename = "C:/Users/mahmo/Documents/plot2.png")
ggplot(data = storm_health_type[tail(order(storm_health_type$sum_of_injuries),5),],aes(x=reorder(evtype,-sum_of_injuries),y=sum_of_injuries))+geom_col()+labs(x="storm event types",y="number of injuries")+ggtitle("Total Number of injuries of top 5 storm event types")
dev.off()
```

![plot2](https://user-images.githubusercontent.com/41892582/188076504-97e98301-6672-4c12-b1dd-1f0d62ac3acd.png)

**2-The most type of events which have the most economic damage**

```
png(filename = "C:/Users/mahmo/Documents/plot3.png")
ggplot(data = storm_economy_type[tail(order(storm_economy_type$all_damages),5),],aes(x=reorder(evtype,-all_damages),y=all_damages/10^9))+geom_col()+labs(x="storm event types",y="Damage by (Billion USD dollar)")+ggtitle("Total cost of property and crop Damages by top 5 storm event type")
dev.off()
```

![plot3](https://user-images.githubusercontent.com/41892582/188079074-8ebc99f6-aeb5-4bc2-b5ce-2e65e26e7a70.png)

# 6.Act phase

### Conclusion

Based on analysis, the most resources of dealing with the Storms and other severe weather events in the United State should be directed towards dealing with tornadoes (causes highest damage to human health including fatalities and injuries in US) and Floods (cause highest economic loss in US)
