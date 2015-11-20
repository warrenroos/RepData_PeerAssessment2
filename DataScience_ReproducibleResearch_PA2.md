


# Title:  Assessment of NOAA Storm Data and its Impact on Population Health and Economic Consequences 

## Synopsis:  
 
In this analysis, I look at NOAA Storm Data from 1950 to 2011.  I explore the impact of storms and severe weather and assess their impact on both the health of individuals in the population at large in addition to their impact on the economies of the areas those storms occurred.  Primarily I looked at 2 measures {property damage, crop damage} to assess economic impact and 2 measures {fatalities, injuries} to assess health impact.  An interesting outcome is that the answer varies based on which measure you choose.  For example, the top Event is Flood for Property Damage, Drought for Crop Damage, Tornado for Fatalities, and Tornado for Injuries.  Even though 2 of the events line up as a top measure, the 2nd most impactful event for each measure do not line up for Fatalities and Injuries.  Also to note, I interpreted "figure" as being a "plot", else I would have commented out more of my Exploratory Data Analysis.  The plots are at the end of the document in the results section.  

## Data Processing:  

Storm Data is obtained, per the Assignment, from the course website <a href='https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2FStormData.csv.bz2'>here</a>, however per the instructors, the data originated from the NOAA.  

Additional documenation related to this data is also provided, both related to its <a href='https://d396qusza40orc.cloudfront.net/repdata%2Fpeer2_doc%2Fpd01016005curr.pdf'>dataset preparation</a> and a <a href='https://d396qusza40orc.cloudfront.net/repdata%2Fpeer2_doc%2FNCDC%20Storm%20Events-FAQ%20Page.pdf'>FAQ</a>.  

Next, I read in the dataset, and do some exploratory data analysis on the data set. Note, as the Assignment required no more than 3 Plots, the Exploratory Plots are either commented out or removed (with some left here as representative of some of the plots / Exploratory Data Analysis done).    

After reading in the data, the PROPDMGEXP was used to translate the PROPDMG into Actual $ with the result stored in PROPDMGACT.  Similarly, the CROPDMGEXP was used to translate the CROPDMG into Actual $ with the result stored in CROPDMGACT.  These two computed variables, PROPDMGACT and CROPDMGACT, in addition to the two additional original variables, FATALITIES and INJURIES, are used here for analysis.  

Since the Assignment wants to know the the impact of Storms, both in terms of economic impact and personal health, I use the two computed variables, PROPDMGACT and CROPDMGACT, as a proxy to the Economic Damage Impact.  Likewise, I use the two additional original variables, FATALITIES and INJURIES, as a proxy to the Personal Health Impact.  

Next, the Assignment requested the impact to be determined by EVTYPE, and as such, I summarize the weather data into an aggregate dataset where summary statistics{sum, mean, sd, median, min, max} were computed for each variable {PROPDMGACT, CROPDMGACT, FATALITIES, INJURIES}.  

Please note, some of the preprocessing, e.g. the "head(weatherData.orig)" and the "unique(weatherData.orig$EVTYPE)", were commented out to to the sheer # of pages they added to the printout.  Since both of these were done on the raw dataset to which everyone had access, they were determined the best options to minimize the # of pages in the writeup (when originally completed, I had over 40 pp, with 10pp due just to the unique call).  

The Data Preprocessing code is as follows:  


```r
# read in data from file 
weatherData.orig <- read.table(
    file = "..\\..\\Data\\repdata_data_StormData.csv.bz2", 
    header = TRUE, sep = ",", na.strings = "NA", nrows = 2500000 # 250000
  )
    #colClasses = c("numeric", "factor", "factor")

# basc summary statistics 
#head(weatherData.orig)
summary(weatherData.orig)
```

```
##     STATE__                  BGN_DATE             BGN_TIME     
##  Min.   : 1.0   5/25/2011 0:00:00:  1202   12:00:00 AM: 10163  
##  1st Qu.:19.0   4/27/2011 0:00:00:  1193   06:00:00 PM:  7350  
##  Median :30.0   6/9/2011 0:00:00 :  1030   04:00:00 PM:  7261  
##  Mean   :31.2   5/30/2004 0:00:00:  1016   05:00:00 PM:  6891  
##  3rd Qu.:45.0   4/4/2011 0:00:00 :  1009   12:00:00 PM:  6703  
##  Max.   :95.0   4/2/2006 0:00:00 :   981   03:00:00 PM:  6700  
##                 (Other)          :895866   (Other)    :857229  
##    TIME_ZONE          COUNTY           COUNTYNAME         STATE       
##  CST    :547493   Min.   :  0.0   JEFFERSON :  7840   TX     : 83728  
##  EST    :245558   1st Qu.: 31.0   WASHINGTON:  7603   KS     : 53440  
##  MST    : 68390   Median : 75.0   JACKSON   :  6660   OK     : 46802  
##  PST    : 28302   Mean   :100.6   FRANKLIN  :  6256   MO     : 35648  
##  AST    :  6360   3rd Qu.:131.0   LINCOLN   :  5937   IA     : 31069  
##  HST    :  2563   Max.   :873.0   MADISON   :  5632   NE     : 30271  
##  (Other):  3631                   (Other)   :862369   (Other):621339  
##                EVTYPE         BGN_RANGE           BGN_AZI      
##  HAIL             :288661   Min.   :   0.000          :547332  
##  TSTM WIND        :219940   1st Qu.:   0.000   N      : 86752  
##  THUNDERSTORM WIND: 82563   Median :   0.000   W      : 38446  
##  TORNADO          : 60652   Mean   :   1.484   S      : 37558  
##  FLASH FLOOD      : 54277   3rd Qu.:   1.000   E      : 33178  
##  FLOOD            : 25326   Max.   :3749.000   NW     : 24041  
##  (Other)          :170878                      (Other):134990  
##          BGN_LOCATI                  END_DATE             END_TIME     
##               :287743                    :243411              :238978  
##  COUNTYWIDE   : 19680   4/27/2011 0:00:00:  1214   06:00:00 PM:  9802  
##  Countywide   :   993   5/25/2011 0:00:00:  1196   05:00:00 PM:  8314  
##  SPRINGFIELD  :   843   6/9/2011 0:00:00 :  1021   04:00:00 PM:  8104  
##  SOUTH PORTION:   810   4/4/2011 0:00:00 :  1007   12:00:00 PM:  7483  
##  NORTH PORTION:   784   5/30/2004 0:00:00:   998   11:59:00 PM:  7184  
##  (Other)      :591444   (Other)          :653450   (Other)    :622432  
##    COUNTY_END COUNTYENDN       END_RANGE           END_AZI      
##  Min.   :0    Mode:logical   Min.   :  0.0000          :724837  
##  1st Qu.:0    NA's:902297    1st Qu.:  0.0000   N      : 28082  
##  Median :0                   Median :  0.0000   S      : 22510  
##  Mean   :0                   Mean   :  0.9862   W      : 20119  
##  3rd Qu.:0                   3rd Qu.:  0.0000   E      : 20047  
##  Max.   :0                   Max.   :925.0000   NE     : 14606  
##                                                 (Other): 72096  
##            END_LOCATI         LENGTH              WIDTH         
##                 :499225   Min.   :   0.0000   Min.   :   0.000  
##  COUNTYWIDE     : 19731   1st Qu.:   0.0000   1st Qu.:   0.000  
##  SOUTH PORTION  :   833   Median :   0.0000   Median :   0.000  
##  NORTH PORTION  :   780   Mean   :   0.2301   Mean   :   7.503  
##  CENTRAL PORTION:   617   3rd Qu.:   0.0000   3rd Qu.:   0.000  
##  SPRINGFIELD    :   575   Max.   :2315.0000   Max.   :4400.000  
##  (Other)        :380536                                         
##        F               MAG            FATALITIES          INJURIES        
##  Min.   :0.0      Min.   :    0.0   Min.   :  0.0000   Min.   :   0.0000  
##  1st Qu.:0.0      1st Qu.:    0.0   1st Qu.:  0.0000   1st Qu.:   0.0000  
##  Median :1.0      Median :   50.0   Median :  0.0000   Median :   0.0000  
##  Mean   :0.9      Mean   :   46.9   Mean   :  0.0168   Mean   :   0.1557  
##  3rd Qu.:1.0      3rd Qu.:   75.0   3rd Qu.:  0.0000   3rd Qu.:   0.0000  
##  Max.   :5.0      Max.   :22000.0   Max.   :583.0000   Max.   :1700.0000  
##  NA's   :843563                                                           
##     PROPDMG          PROPDMGEXP        CROPDMG          CROPDMGEXP    
##  Min.   :   0.00          :465934   Min.   :  0.000          :618413  
##  1st Qu.:   0.00   K      :424665   1st Qu.:  0.000   K      :281832  
##  Median :   0.00   M      : 11330   Median :  0.000   M      :  1994  
##  Mean   :  12.06   0      :   216   Mean   :  1.527   k      :    21  
##  3rd Qu.:   0.50   B      :    40   3rd Qu.:  0.000   0      :    19  
##  Max.   :5000.00   5      :    28   Max.   :990.000   B      :     9  
##                    (Other):    84                     (Other):     9  
##       WFO                                       STATEOFFIC    
##         :142069                                      :248769  
##  OUN    : 17393   TEXAS, North                       : 12193  
##  JAN    : 13889   ARKANSAS, Central and North Central: 11738  
##  LWX    : 13174   IOWA, Central                      : 11345  
##  PHI    : 12551   KANSAS, Southwest                  : 11212  
##  TSA    : 12483   GEORGIA, North and Central         : 11120  
##  (Other):690738   (Other)                            :595920  
##                                                                                                                                                                                                     ZONENAMES     
##                                                                                                                                                                                                          :594029  
##                                                                                                                                                                                                          :205988  
##  GREATER RENO / CARSON CITY / M - GREATER RENO / CARSON CITY / M                                                                                                                                         :   639  
##  GREATER LAKE TAHOE AREA - GREATER LAKE TAHOE AREA                                                                                                                                                       :   592  
##  JEFFERSON - JEFFERSON                                                                                                                                                                                   :   303  
##  MADISON - MADISON                                                                                                                                                                                       :   302  
##  (Other)                                                                                                                                                                                                 :100444  
##     LATITUDE      LONGITUDE        LATITUDE_E     LONGITUDE_    
##  Min.   :   0   Min.   :-14451   Min.   :   0   Min.   :-14455  
##  1st Qu.:2802   1st Qu.:  7247   1st Qu.:   0   1st Qu.:     0  
##  Median :3540   Median :  8707   Median :   0   Median :     0  
##  Mean   :2875   Mean   :  6940   Mean   :1452   Mean   :  3509  
##  3rd Qu.:4019   3rd Qu.:  9605   3rd Qu.:3549   3rd Qu.:  8735  
##  Max.   :9706   Max.   : 17124   Max.   :9706   Max.   :106220  
##  NA's   :47                      NA's   :40                     
##                                            REMARKS           REFNUM      
##                                                :287433   Min.   :     1  
##                                                : 24013   1st Qu.:225575  
##  Trees down.\n                                 :  1110   Median :451149  
##  Several trees were blown down.\n              :   568   Mean   :451149  
##  Trees were downed.\n                          :   446   3rd Qu.:676723  
##  Large trees and power lines were blown down.\n:   432   Max.   :902297  
##  (Other)                                       :588295
```

```r
dim(weatherData.orig)
```

```
## [1] 902297     37
```

```r
# focus on impact to population and economy - identify the variables and provide basic statistcs 
quantile(weatherData.orig$FATALITIES)
```

```
##   0%  25%  50%  75% 100% 
##    0    0    0    0  583
```

```r
quantile(weatherData.orig$INJURIES)
```

```
##   0%  25%  50%  75% 100% 
##    0    0    0    0 1700
```

```r
quantile(weatherData.orig$PROPDMG)
```

```
##    0%   25%   50%   75%  100% 
## 0e+00 0e+00 0e+00 5e-01 5e+03
```

```r
quantile(weatherData.orig$CROPDMG)
```

```
##   0%  25%  50%  75% 100% 
##    0    0    0    0  990
```

```r
# unique(weatherData.orig$EVTYPE)
unique(weatherData.orig$PROPDMGEXP)
```

```
##  [1] K M   B m + 0 5 6 ? 4 2 3 h 7 H - 1 8
## Levels:  - ? + 0 1 2 3 4 5 6 7 8 B h H K m M
```

```r
unique(weatherData.orig$CROPDMGEXP)
```

```
## [1]   M K m B ? 0 k 2
## Levels:  ? 0 2 B k K m M
```

```r
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
## 
## The following objects are masked from 'package:stats':
## 
##     filter, lag
## 
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
# filter for "B" and get subset 
weatherData.PROP.B <- weatherData.orig %>% 
  select(everything()) %>% 
  filter(PROPDMGEXP %in% c("B")) 
  
# filter for "M" and get subset 
weatherData.PROP.M <- weatherData.orig %>% 
  select(everything()) %>% 
  filter(PROPDMGEXP %in% c("M")) 
  
# filter for "M" and get subset 
weatherData.PROP.K <- weatherData.orig %>% 
  select(everything()) %>% 
  filter(PROPDMGEXP %in% c("K")) 
  
# filter for "M" and get subset 
weatherData.PROP.N <- weatherData.orig %>% 
  select(everything()) %>% 
  filter(!(PROPDMGEXP %in% c("B", "M", "K"))) 
  
# add Property Damage in Actual $ to the respective datasets 
weatherData.PROP.B <- cbind(weatherData.PROP.B, PROPDMGACT = weatherData.PROP.B$PROPDMG * (1000 * 1000 * 1000))
weatherData.PROP.M <- cbind(weatherData.PROP.M, PROPDMGACT = weatherData.PROP.M$PROPDMG * (1000 * 1000))
weatherData.PROP.K <- cbind(weatherData.PROP.K, PROPDMGACT = weatherData.PROP.K$PROPDMG * (1000))
weatherData.PROP.N <- cbind(weatherData.PROP.N, PROPDMGACT = weatherData.PROP.N$PROPDMG * (1))

# resssemble dataset 
weatherData.INTERIM <- rbind(weatherData.PROP.B, weatherData.PROP.M, weatherData.PROP.K, weatherData.PROP.N) 

# filter for "B" and get subset 
weatherData.CROP.B <- weatherData.INTERIM %>% 
  select(everything()) %>% 
  filter(CROPDMGEXP %in% c("B")) 
  
# filter for "M" and get subset 
weatherData.CROP.M <- weatherData.INTERIM %>% 
  select(everything()) %>% 
  filter(CROPDMGEXP %in% c("M")) 
  
# filter for "M" and get subset 
weatherData.CROP.K <- weatherData.INTERIM %>% 
  select(everything()) %>% 
  filter(CROPDMGEXP %in% c("K")) 
  
# filter for "M" and get subset 
weatherData.CROP.N <- weatherData.INTERIM %>% 
  select(everything()) %>% 
  filter(!(CROPDMGEXP %in% c("B", "M", "K"))) 

# add CROP Damage in Actual $ to the respective datasets 
weatherData.CROP.B <- cbind(weatherData.CROP.B, CROPDMGACT = weatherData.CROP.B$CROPDMG * (1000 * 1000 * 1000))
weatherData.CROP.M <- cbind(weatherData.CROP.M, CROPDMGACT = weatherData.CROP.M$CROPDMG * (1000 * 1000))
weatherData.CROP.K <- cbind(weatherData.CROP.K, CROPDMGACT = weatherData.CROP.K$CROPDMG * (1000))
weatherData.CROP.N <- cbind(weatherData.CROP.N, CROPDMGACT = weatherData.CROP.N$CROPDMG * (1))

# resssemble dataset 
weatherData.ALL <- rbind(weatherData.CROP.B, weatherData.CROP.M, weatherData.CROP.K, weatherData.CROP.N) 

# group by EVTYPE 
weatherData.EVTYPE <- weatherData.ALL %>% 
  group_by(EVTYPE) %>% 
  summarize(
            sumPROPDMGACT = sum(PROPDMGACT, na.rm = TRUE), 
            meanPROPDMGACT = mean(PROPDMGACT, na.rm = TRUE), 
            sdPROPDMGACT = sd(PROPDMGACT, na.rm = TRUE), 
            medianPROPDMGACT = median(PROPDMGACT, na.rm = TRUE), 
            minPROPDMGACT = min(PROPDMGACT, na.rm = TRUE), 
            maxPROPDMGACT = max(PROPDMGACT, na.rm = TRUE),
            
            sumCROPDMGACT = sum(CROPDMGACT, na.rm = TRUE), 
            meanCROPDMGACT = mean(CROPDMGACT, na.rm = TRUE), 
            sdCROPDMGACT = sd(CROPDMGACT, na.rm = TRUE), 
            medianCROPDMGACT = median(CROPDMGACT, na.rm = TRUE), 
            minCROPDMGACT = min(CROPDMGACT, na.rm = TRUE), 
            maxCROPDMGACT = max(CROPDMGACT, na.rm = TRUE), 

            sumFATALITIES = sum(FATALITIES, na.rm = TRUE), 
            meanFATALITIES = mean(FATALITIES, na.rm = TRUE), 
            sdFATALITIES = sd(FATALITIES, na.rm = TRUE), 
            medianFATALITIES = median(FATALITIES, na.rm = TRUE), 
            minFATALITIES = min(FATALITIES, na.rm = TRUE), 
            maxFATALITIES = max(FATALITIES, na.rm = TRUE), 

            sumINJURIES = sum(INJURIES, na.rm = TRUE), 
            meanINJURIES = mean(INJURIES, na.rm = TRUE), 
            sdINJURIES = sd(INJURIES, na.rm = TRUE), 
            medianINJURIES = median(INJURIES, na.rm = TRUE), 
            minINJURIES = min(INJURIES, na.rm = TRUE), 
            maxINJURIES = max(INJURIES, na.rm = TRUE) 
            
            )

# explore results  
head(weatherData.EVTYPE)
```

```
## Source: local data frame [6 x 25]
## 
##                  EVTYPE sumPROPDMGACT meanPROPDMGACT sdPROPDMGACT
##                  (fctr)         (dbl)          (dbl)        (dbl)
## 1    HIGH SURF ADVISORY        200000         200000          NaN
## 2         COASTAL FLOOD             0              0          NaN
## 3           FLASH FLOOD         50000          50000          NaN
## 4             LIGHTNING             0              0          NaN
## 5             TSTM WIND       8100000        2025000      3983612
## 6       TSTM WIND (G45)          8000           8000          NaN
## Variables not shown: medianPROPDMGACT (dbl), minPROPDMGACT (dbl),
##   maxPROPDMGACT (dbl), sumCROPDMGACT (dbl), meanCROPDMGACT (dbl),
##   sdCROPDMGACT (dbl), medianCROPDMGACT (dbl), minCROPDMGACT (dbl),
##   maxCROPDMGACT (dbl), sumFATALITIES (dbl), meanFATALITIES (dbl),
##   sdFATALITIES (dbl), medianFATALITIES (dbl), minFATALITIES (dbl),
##   maxFATALITIES (dbl), sumINJURIES (dbl), meanINJURIES (dbl), sdINJURIES
##   (dbl), medianINJURIES (dbl), minINJURIES (dbl), maxINJURIES (dbl)
```

```r
summary(weatherData.EVTYPE)
```

```
##                    EVTYPE    sumPROPDMGACT       meanPROPDMGACT     
##     HIGH SURF ADVISORY:  1   Min.   :0.000e+00   Min.   :0.000e+00  
##   COASTAL FLOOD       :  1   1st Qu.:0.000e+00   1st Qu.:0.000e+00  
##   FLASH FLOOD         :  1   Median :0.000e+00   Median :0.000e+00  
##   LIGHTNING           :  1   Mean   :4.338e+08   Mean   :5.282e+06  
##   TSTM WIND           :  1   3rd Qu.:5.105e+04   3rd Qu.:1.200e+04  
##   TSTM WIND (G45)     :  1   Max.   :1.447e+11   Max.   :1.600e+09  
##  (Other)              :979                                          
##   sdPROPDMGACT       medianPROPDMGACT   minPROPDMGACT      
##  Min.   :0.000e+00   Min.   :0.00e+00   Min.   :0.000e+00  
##  1st Qu.:0.000e+00   1st Qu.:0.00e+00   1st Qu.:0.000e+00  
##  Median :4.950e+02   Median :0.00e+00   Median :0.000e+00  
##  Mean   :2.098e+07   Mean   :3.33e+06   Mean   :1.947e+06  
##  3rd Qu.:8.359e+04   3rd Qu.:5.00e+02   3rd Qu.:0.000e+00  
##  Max.   :2.446e+09   Max.   :1.60e+09   Max.   :1.600e+09  
##  NA's   :489                                               
##  maxPROPDMGACT      sumCROPDMGACT       meanCROPDMGACT     
##  Min.   :0.00e+00   Min.   :0.000e+00   Min.   :        0  
##  1st Qu.:0.00e+00   1st Qu.:0.000e+00   1st Qu.:        0  
##  Median :0.00e+00   Median :0.000e+00   Median :        0  
##  Mean   :2.11e+08   Mean   :4.984e+07   Mean   :   536350  
##  3rd Qu.:5.00e+04   3rd Qu.:0.000e+00   3rd Qu.:        0  
##  Max.   :1.15e+11   Max.   :1.397e+10   Max.   :142000000  
##                                                            
##   sdCROPDMGACT       medianCROPDMGACT    minCROPDMGACT      
##  Min.   :        0   Min.   :        0   Min.   :        0  
##  1st Qu.:        0   1st Qu.:        0   1st Qu.:        0  
##  Median :        0   Median :        0   Median :        0  
##  Mean   :  2395726   Mean   :   338704   Mean   :   294539  
##  3rd Qu.:        0   3rd Qu.:        0   3rd Qu.:        0  
##  Max.   :380131456   Max.   :142000000   Max.   :142000000  
##  NA's   :489                                                
##  maxCROPDMGACT       sumFATALITIES     meanFATALITIES     sdFATALITIES    
##  Min.   :0.000e+00   Min.   :   0.00   Min.   : 0.0000   Min.   : 0.0000  
##  1st Qu.:0.000e+00   1st Qu.:   0.00   1st Qu.: 0.0000   1st Qu.: 0.0000  
##  Median :0.000e+00   Median :   0.00   Median : 0.0000   Median : 0.0000  
##  Mean   :1.837e+07   Mean   :  15.38   Mean   : 0.1525   Mean   : 0.2960  
##  3rd Qu.:0.000e+00   3rd Qu.:   0.00   3rd Qu.: 0.0000   3rd Qu.: 0.0842  
##  Max.   :5.000e+09   Max.   :5633.00   Max.   :25.0000   Max.   :21.1026  
##                                                          NA's   :489      
##  medianFATALITIES  minFATALITIES      maxFATALITIES      sumINJURIES     
##  Min.   : 0.0000   Min.   : 0.00000   Min.   :  0.000   Min.   :    0.0  
##  1st Qu.: 0.0000   1st Qu.: 0.00000   1st Qu.:  0.000   1st Qu.:    0.0  
##  Median : 0.0000   Median : 0.00000   Median :  0.000   Median :    0.0  
##  Mean   : 0.1117   Mean   : 0.09645   Mean   :  1.631   Mean   :  142.7  
##  3rd Qu.: 0.0000   3rd Qu.: 0.00000   3rd Qu.:  0.000   3rd Qu.:    0.0  
##  Max.   :25.0000   Max.   :25.00000   Max.   :583.000   Max.   :91346.0  
##                                                                          
##   meanINJURIES       sdINJURIES      medianINJURIES     minINJURIES     
##  Min.   : 0.0000   Min.   : 0.0000   Min.   : 0.0000   Min.   : 0.0000  
##  1st Qu.: 0.0000   1st Qu.: 0.0000   1st Qu.: 0.0000   1st Qu.: 0.0000  
##  Median : 0.0000   Median : 0.0000   Median : 0.0000   Median : 0.0000  
##  Mean   : 0.4297   Mean   : 1.2667   Mean   : 0.2761   Mean   : 0.2447  
##  3rd Qu.: 0.0000   3rd Qu.: 0.1295   3rd Qu.: 0.0000   3rd Qu.: 0.0000  
##  Max.   :70.0000   Max.   :89.8041   Max.   :70.0000   Max.   :70.0000  
##                    NA's   :489                                          
##   maxINJURIES      
##  Min.   :   0.000  
##  1st Qu.:   0.000  
##  Median :   0.000  
##  Mean   :   9.835  
##  3rd Qu.:   0.000  
##  Max.   :1700.000  
## 
```

## Results:  

Above, I did a tremendous amount of preprocessing as already documented.  Here, the results are presented.  Also to note, I interpreted "figure" as being a "plot" (in the question also, it implies figure is synonymous with plot(s), not data tables), else I would have commented out more of my Exploratory Data Analysis.  

First, I look at the aggregate weather data sorted by PROPDMGACT, CROPDMGACT, FATALITIES, and INJURIES, each of which grouped by EVTYPE, in order to see which EVTYPE (Storm Type) has the biggest impact in each of these 4 measures.  


```r
## 3 figures - can use multi-panel plots 

# sort by sumPROPDMGACT 
weatherData.EVTYPE.BySumPROPDMGACT <- weatherData.EVTYPE %>%
    select(EVTYPE, sumPROPDMGACT) %>% 
    arrange(desc(sumPROPDMGACT)) 
head(weatherData.EVTYPE.BySumPROPDMGACT)
```

```
## Source: local data frame [6 x 2]
## 
##              EVTYPE sumPROPDMGACT
##              (fctr)         (dbl)
## 1             FLOOD  144657709807
## 2 HURRICANE/TYPHOON   69305840000
## 3           TORNADO   56925660790
## 4       STORM SURGE   43323536000
## 5       FLASH FLOOD   16140812067
## 6              HAIL   15727367053
```

```r
# sort by sumCROPDMGACT 
weatherData.EVTYPE.BySumCROPDMGACT <- weatherData.EVTYPE %>%
    select(EVTYPE, sumCROPDMGACT) %>% 
    arrange(desc(sumCROPDMGACT)) 
head(weatherData.EVTYPE.BySumCROPDMGACT)
```

```
## Source: local data frame [6 x 2]
## 
##        EVTYPE sumCROPDMGACT
##        (fctr)         (dbl)
## 1     DROUGHT   13972566000
## 2       FLOOD    5661968450
## 3 RIVER FLOOD    5029459000
## 4   ICE STORM    5022113500
## 5        HAIL    3025537890
## 6   HURRICANE    2741910000
```

```r
# sort by sumFATALITIES 
weatherData.EVTYPE.BySumFATALITIES <- weatherData.EVTYPE %>%
    select(EVTYPE, sumFATALITIES) %>% 
    arrange(desc(sumFATALITIES)) 
head(weatherData.EVTYPE.BySumFATALITIES)
```

```
## Source: local data frame [6 x 2]
## 
##           EVTYPE sumFATALITIES
##           (fctr)         (dbl)
## 1        TORNADO          5633
## 2 EXCESSIVE HEAT          1903
## 3    FLASH FLOOD           978
## 4           HEAT           937
## 5      LIGHTNING           816
## 6      TSTM WIND           504
```

```r
# sort by sumINJURIES 
weatherData.EVTYPE.BySumINJURIES <- weatherData.EVTYPE %>%
    select(EVTYPE, sumINJURIES) %>% 
    arrange(desc(sumINJURIES)) 
head(weatherData.EVTYPE.BySumINJURIES)
```

```
## Source: local data frame [6 x 2]
## 
##           EVTYPE sumINJURIES
##           (fctr)       (dbl)
## 1        TORNADO       91346
## 2      TSTM WIND        6957
## 3          FLOOD        6789
## 4 EXCESSIVE HEAT        6525
## 5      LIGHTNING        5230
## 6           HEAT        2100
```

From the above, in terms of Property Damage, the biggest impact storm types (in descending order) are:  Flood, Hurricane / Typhoon, and Tornado.  Similarly, in terms of Crop Damage, the biggest impact storm types (in descending order) are:  Drought, Flood, and River Flood.  Further, in terms of Fatalities, the biggest impact storm types (in descending order) are:  Tornado, Excessive Heat, and Flash Flood.  Lastly, in terms of Injuries, the biggest impact storm types (in descending order) are:  Tornado, Thunderstorm Wind, and Flood.  As mentioned earlier, the answer to what is the most harmful to Economic Health or Public Health depends on which measure you choose.   

Next, looking at the data graphically the aggregate weather data sorted by PROPDMGACT, CROPDMGACT, FATALITIES, and INJURIES, each of which grouped by EVTYPE, in order to see which EVTYPE (Storm Type) has the biggest impact in each of these 4 measures.  


```r
## 3 figures - can use multi-panel plots 

# Top5 Unique values by Impact Measure 
Top5.BySumPROPDMGACT = head(weatherData.EVTYPE.BySumPROPDMGACT, 5)
Top5.BySumCROPDMGACT = head(weatherData.EVTYPE.BySumCROPDMGACT, 5)
Top5.BySumFATALITIES = head(weatherData.EVTYPE.BySumFATALITIES, 5)
Top5.BySumINJURIES = head(weatherData.EVTYPE.BySumINJURIES, 5)

# Plot Only Top 10 
par(mfcol = c(2,2), mar = c(4,4,2,1))

barplot(height = Top5.BySumPROPDMGACT$sumPROPDMGACT, 
  names.arg = Top5.BySumPROPDMGACT$EVTYPE, 
  main="Economic - Property Damage By Event Type", 
  xlab="Event Type",
  ylab="Economic - Property Damage ($)"
  ) 
  
barplot(height = Top5.BySumCROPDMGACT$sumCROPDMGACT, 
  names.arg = Top5.BySumCROPDMGACT$EVTYPE, 
  main="Economic - Crop Damage By Event Type", 
  xlab="Event Type",
  ylab="Economic - Crop Damage ($)"
  ) 

barplot(height = Top5.BySumFATALITIES$sumFATALITIES, 
  names.arg = Top5.BySumFATALITIES$EVTYPE, 
  main="Personal - Fatalities By Event Type", 
  xlab="Event Type",
  ylab="Personal - Fatalities (#)"
  ) 

barplot(height = Top5.BySumINJURIES$sumINJURIES, 
  names.arg = Top5.BySumINJURIES$EVTYPE, 
  main="Personal - Injuries By Event Type", 
  xlab="Event Type",
  ylab="Personal - Injuries (#)"
  ) 
```

![](DataScience_ReproducibleResearch_PA2_files/figure-html/unnamed-chunk-4-1.png) 

Above, we look graphically at the aggregate weather data sorted by PROPDMGACT, CROPDMGACT, FATALITIES, and INJURIES, each of which grouped by EVTYPE, in order to see which EVTYPE (Storm Type) has the biggest impact in each of these 4 measures.  Again, in terms of Property Damage, the biggest impact storm types (in descending order) are:  Flood, Hurricane / Typhoon, and Tornado.  Similarly, in terms of Crop Damage, the biggest impact storm types (in descending order) are:  Drought, Flood, and River Flood.  Further, in terms of Fatalities, the biggest impact storm types (in descending order) are:  Tornado, Excessive Heat, and Flash Flood.  Lastly, in terms of Injuries, the biggest impact storm types (in descending order) are:  Tornado, Thunderstorm Wind, and Flood.  As mentioned earlier, the answer to what is the most harmful to Economic Health or Public Health depends on which measure you choose.   

In summary, I looked at the NOAA Storm Data from 1950 to 2011.  I explored the impact of storms and severe weather and assessed their impact on both the health of individuals in the population at large in addition to their impact on the economies of the areas those storms occurred.  Primarily I looked at 2 measures {property damage, crop damage} to assess economic impact and 2 measures {fatalities, injuries} to assess health impact.  An interesting outcome is that the answer varies based on which measure you choose, with Flood, Drought, or Tornado being some of the top events, depending on the measure of damage / impact.  
