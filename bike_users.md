How Does a Bike-Share Navigate Speedy Success?
================
Eneye
7/29/2021

### Introduction

Cyclistic is a bike-share company that features more than 5,800 bicycles
and 600 docking stations. Cyclistic users are more likely to ride for
leisure, but about 30% use them to commute to work each day. Until now,
Cyclistic’s marketing strategy relied on building general awareness and
appealing to broad consumer segments. One approach that helped make
these things possible was the flexibility of its pricing plans:
single-ride passes, full-day passes, and annual memberships. Customers
who purchase single-ride or full-day passes are referred to as casual
riders. Customers who purchase annual memberships are Cyclistic members.

### Business Task

Cyclistic’s finance analysts have concluded that annual members are much
more profitable than casual riders. Although the pricing flexibility
helps Cyclistic attract more customers. They believe there is a very
good chance to convert casual riders into members.

In order to do that, however, the marketing analyst team needs to better
understand how annual members and casual riders differ, why casual
riders would buy a membership, and how digital media could affect their
marketing tactics.

Three questions will guide the future marketing program:

1.  How do annual members and casual riders use Cyclistic bikes
    differently?

2.  Why would casual riders buy Cyclistic annual memberships?

3.  How can Cyclistic use digital media to influence casual riders to
    become members?

This analysis will answer the first question.

### Deliverables

A report with the following will be produced:

-   A clear statement of the business task

-   A description of all data sources used

-   Documentation of any cleaning or manipulation of data

-   A summary of your analysis

-   Supporting visualizations and key findings

-   Your top three recommendations based on your analysis

### Data Source

Cyclistic’s historical trip data will be used to analyze and identify
trends.

The data can be found here: [Download the previous 12 months of
Cyclistic trip data
here](https://divvy-tripdata.s3.amazonaws.com/index.html).

The data has been made available by Motivate International Inc. under
this [license](https://www.divvybikes.com/data-license-agreement).

############################################################################################ 

#### Set Up Environment

Note: Install and load Packages: tidyverse, ggplot2 and lubridate

``` r
library(tidyverse)
```

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v ggplot2 3.3.5     v purrr   0.3.4
    ## v tibble  3.1.3     v dplyr   1.0.7
    ## v tidyr   1.1.3     v stringr 1.4.0
    ## v readr   2.0.0     v forcats 0.5.1

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(lubridate)
```

    ## 
    ## Attaching package: 'lubridate'

    ## The following objects are masked from 'package:base':
    ## 
    ##     date, intersect, setdiff, union

``` r
library(ggplot2)
```

#### Collect Data

Note: Upload the ride sharing data sets

``` r
july_2020 <- read_csv("202007-divvy-tripdata.csv")
```

    ## Rows: 551480 Columns: 13

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (5): ride_id, rideable_type, start_station_name, end_station_name, memb...
    ## dbl  (6): start_station_id, end_station_id, start_lat, start_lng, end_lat, e...
    ## dttm (2): started_at, ended_at

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
august_2020 <- read_csv("202008-divvy-tripdata.csv")
```

    ## Rows: 622361 Columns: 13

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (5): ride_id, rideable_type, start_station_name, end_station_name, memb...
    ## dbl  (6): start_station_id, end_station_id, start_lat, start_lng, end_lat, e...
    ## dttm (2): started_at, ended_at

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
september_2020 <- read_csv("202009-divvy-tripdata.csv")
```

    ## Rows: 532958 Columns: 13

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (5): ride_id, rideable_type, start_station_name, end_station_name, memb...
    ## dbl  (6): start_station_id, end_station_id, start_lat, start_lng, end_lat, e...
    ## dttm (2): started_at, ended_at

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
october_2020 <- read_csv("202010-divvy-tripdata.csv")
```

    ## Rows: 388653 Columns: 13

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (5): ride_id, rideable_type, start_station_name, end_station_name, memb...
    ## dbl  (6): start_station_id, end_station_id, start_lat, start_lng, end_lat, e...
    ## dttm (2): started_at, ended_at

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
november_2020 <- read_csv("202011-divvy-tripdata.csv")
```

    ## Rows: 259716 Columns: 13

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (5): ride_id, rideable_type, start_station_name, end_station_name, memb...
    ## dbl  (6): start_station_id, end_station_id, start_lat, start_lng, end_lat, e...
    ## dttm (2): started_at, ended_at

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
december_2020 <- read_csv("202012-divvy-tripdata.csv")
```

    ## Rows: 131573 Columns: 13

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
january_2021 <- read_csv("202101-divvy-tripdata.csv")
```

    ## Rows: 96834 Columns: 13

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
february_2021 <- read_csv("202102-divvy-tripdata.csv")
```

    ## Rows: 49622 Columns: 13

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
march_2021 <- read_csv("202103-divvy-tripdata.csv")
```

    ## Rows: 228496 Columns: 13

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
april_2021 <- read_csv("202104-divvy-tripdata.csv")
```

    ## Rows: 337230 Columns: 13

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
may_2021 <- read_csv("202105-divvy-tripdata.csv")
```

    ## Rows: 531633 Columns: 13

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
june_2021 <- read_csv("202106-divvy-tripdata.csv")
```

    ## Rows: 729595 Columns: 13

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

#### Explore data and merge into one sheet

Note: Compare column names each of the files. While the names don’t have
to be in the same order, they do need to match perfectly before we can
use a command to join them into one file

``` r
colnames(july_2020)
```

    ##  [1] "ride_id"            "rideable_type"      "started_at"        
    ##  [4] "ended_at"           "start_station_name" "start_station_id"  
    ##  [7] "end_station_name"   "end_station_id"     "start_lat"         
    ## [10] "start_lng"          "end_lat"            "end_lng"           
    ## [13] "member_casual"

``` r
colnames(august_2020)
```

    ##  [1] "ride_id"            "rideable_type"      "started_at"        
    ##  [4] "ended_at"           "start_station_name" "start_station_id"  
    ##  [7] "end_station_name"   "end_station_id"     "start_lat"         
    ## [10] "start_lng"          "end_lat"            "end_lng"           
    ## [13] "member_casual"

``` r
colnames(september_2020)
```

    ##  [1] "ride_id"            "rideable_type"      "started_at"        
    ##  [4] "ended_at"           "start_station_name" "start_station_id"  
    ##  [7] "end_station_name"   "end_station_id"     "start_lat"         
    ## [10] "start_lng"          "end_lat"            "end_lng"           
    ## [13] "member_casual"

``` r
colnames(october_2020)
```

    ##  [1] "ride_id"            "rideable_type"      "started_at"        
    ##  [4] "ended_at"           "start_station_name" "start_station_id"  
    ##  [7] "end_station_name"   "end_station_id"     "start_lat"         
    ## [10] "start_lng"          "end_lat"            "end_lng"           
    ## [13] "member_casual"

``` r
colnames(november_2020)
```

    ##  [1] "ride_id"            "rideable_type"      "started_at"        
    ##  [4] "ended_at"           "start_station_name" "start_station_id"  
    ##  [7] "end_station_name"   "end_station_id"     "start_lat"         
    ## [10] "start_lng"          "end_lat"            "end_lng"           
    ## [13] "member_casual"

``` r
colnames(december_2020)
```

    ##  [1] "ride_id"            "rideable_type"      "started_at"        
    ##  [4] "ended_at"           "start_station_name" "start_station_id"  
    ##  [7] "end_station_name"   "end_station_id"     "start_lat"         
    ## [10] "start_lng"          "end_lat"            "end_lng"           
    ## [13] "member_casual"

``` r
colnames(january_2021)
```

    ##  [1] "ride_id"            "rideable_type"      "started_at"        
    ##  [4] "ended_at"           "start_station_name" "start_station_id"  
    ##  [7] "end_station_name"   "end_station_id"     "start_lat"         
    ## [10] "start_lng"          "end_lat"            "end_lng"           
    ## [13] "member_casual"

``` r
colnames(february_2021)
```

    ##  [1] "ride_id"            "rideable_type"      "started_at"        
    ##  [4] "ended_at"           "start_station_name" "start_station_id"  
    ##  [7] "end_station_name"   "end_station_id"     "start_lat"         
    ## [10] "start_lng"          "end_lat"            "end_lng"           
    ## [13] "member_casual"

``` r
colnames(march_2021)
```

    ##  [1] "ride_id"            "rideable_type"      "started_at"        
    ##  [4] "ended_at"           "start_station_name" "start_station_id"  
    ##  [7] "end_station_name"   "end_station_id"     "start_lat"         
    ## [10] "start_lng"          "end_lat"            "end_lng"           
    ## [13] "member_casual"

``` r
colnames(april_2021)
```

    ##  [1] "ride_id"            "rideable_type"      "started_at"        
    ##  [4] "ended_at"           "start_station_name" "start_station_id"  
    ##  [7] "end_station_name"   "end_station_id"     "start_lat"         
    ## [10] "start_lng"          "end_lat"            "end_lng"           
    ## [13] "member_casual"

``` r
colnames(may_2021)
```

    ##  [1] "ride_id"            "rideable_type"      "started_at"        
    ##  [4] "ended_at"           "start_station_name" "start_station_id"  
    ##  [7] "end_station_name"   "end_station_id"     "start_lat"         
    ## [10] "start_lng"          "end_lat"            "end_lng"           
    ## [13] "member_casual"

Note: make start\_station \_id and end\_station\_id data type consistent

``` r
july_2020 <- mutate(july_2020, start_station_id = as.character(start_station_id), end_station_id = as.character(end_station_id))
august_2020 <- mutate(august_2020, start_station_id = as.character(start_station_id), end_station_id = as.character(end_station_id))
september_2020 <- mutate(september_2020, start_station_id = as.character(start_station_id), end_station_id = as.character(end_station_id))
october_2020 <- mutate(october_2020, start_station_id = as.character(start_station_id), end_station_id = as.character(end_station_id))
november_2020 <- mutate(november_2020, start_station_id = as.character(start_station_id), end_station_id = as.character(end_station_id))
december_2020 <- mutate(december_2020, start_station_id = as.character(start_station_id), end_station_id = as.character(end_station_id))
january_2021 <- mutate(january_2021, start_station_id = as.character(start_station_id), end_station_id = as.character(end_station_id))
february_2021 <- mutate(february_2021, start_station_id = as.character(start_station_id), end_station_id = as.character(end_station_id))
march_2021 <- mutate(march_2021, start_station_id = as.character(start_station_id), end_station_id = as.character(end_station_id))
april_2021 <- mutate(april_2021, start_station_id = as.character(start_station_id), end_station_id = as.character(end_station_id))
may_2021 <- mutate(may_2021, start_station_id = as.character(start_station_id), end_station_id = as.character(end_station_id))
june_2021 <- mutate(june_2021, start_station_id = as.character(start_station_id), end_station_id = as.character(end_station_id))
```

Note: Merge all months to one year

``` r
all_trips <- bind_rows(july_2020, august_2020, september_2020, october_2020, november_2020, december_2020, january_2021, february_2021, march_2021, april_2021, may_2021, june_2021)
```

#### Clean Consolidated Data

Note: rideable\_type: “classic\_bike” and “docked\_bike” should both be
represented by one value: “docked\_bike”

``` r
all_trips <-  all_trips %>% 
  mutate(rideable_type = recode(rideable_type,"classic_bike" = "docked_bike"))
```

Note: started\_at and ended\_at were converted from character to date
time formats, and a new column "ride\_duration was created in seconds to
calculate the time difference

``` r
all_trips$ride_duration <- difftime(all_trips$ended_at,all_trips$started_at)
```

Note: Add columns that list the date, month, day, and year of each ride

``` r
all_trips$date <- as.Date(all_trips$started_at) #The default format is yyyy-mm-dd
all_trips$month <- format(as.Date(all_trips$date), "%m")
all_trips$day <- format(as.Date(all_trips$date), "%d")
all_trips$year <- format(as.Date(all_trips$date), "%Y")
all_trips$day_of_week <- format(as.Date(all_trips$date), "%A")
```

Note: Convert “ride\_duration” from factor to numeric so we can run
calculations on the data

``` r
is.factor(all_trips$ride_duration)
```

    ## [1] FALSE

``` r
all_trips$ride_duration <- as.numeric(as.character(all_trips$ride_duration))
is.numeric(all_trips$ride_duration)
```

    ## [1] TRUE

Note: Remove entries where bikes were taken for repairs and where
ride\_duration is &lt; 0 and where member\_casual is null

``` r
all_trips_v2 <- all_trips[!(all_trips$ride_duration<0 | is.na(all_trips$member_casual) ),]
```

#### Conduct Descriptive Analysis

Note: Descriptive Analysis on ride\_duration

``` r
summary(all_trips_v2$ride_duration)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##       0     453     820    1578    1511 3356649

Note: Compare members and casual users

``` r
aggregate(all_trips_v2$ride_duration ~ all_trips_v2$member_casual, FUN = mean)
```

    ##   all_trips_v2$member_casual all_trips_v2$ride_duration
    ## 1                     casual                  2454.2374
    ## 2                     member                   908.8719

``` r
aggregate(all_trips_v2$ride_duration ~ all_trips_v2$member_casual, FUN = median)
```

    ##   all_trips_v2$member_casual all_trips_v2$ride_duration
    ## 1                     casual                       1155
    ## 2                     member                        648

``` r
aggregate(all_trips_v2$ride_duration ~ all_trips_v2$member_casual, FUN = max)
```

    ##   all_trips_v2$member_casual all_trips_v2$ride_duration
    ## 1                     casual                    3356649
    ## 2                     member                    2005282

``` r
aggregate(all_trips_v2$ride_duration ~ all_trips_v2$member_casual, FUN = min)
```

    ##   all_trips_v2$member_casual all_trips_v2$ride_duration
    ## 1                     casual                          0
    ## 2                     member                          0

Note: See the average ride time by each day for members vs casual users

``` r
aggregate(all_trips_v2$ride_duration ~ all_trips_v2$member_casual + all_trips_v2$day_of_week, FUN = mean)
```

    ##    all_trips_v2$member_casual all_trips_v2$day_of_week
    ## 1                      casual                   Friday
    ## 2                      member                   Friday
    ## 3                      casual                   Monday
    ## 4                      member                   Monday
    ## 5                      casual                 Saturday
    ## 6                      member                 Saturday
    ## 7                      casual                   Sunday
    ## 8                      member                   Sunday
    ## 9                      casual                 Thursday
    ## 10                     member                 Thursday
    ## 11                     casual                  Tuesday
    ## 12                     member                  Tuesday
    ## 13                     casual                Wednesday
    ## 14                     member                Wednesday
    ##    all_trips_v2$ride_duration
    ## 1                   2351.0295
    ## 2                    897.7132
    ## 3                   2375.1075
    ## 4                    875.7180
    ## 5                   2612.9410
    ## 6                    996.5180
    ## 7                   2820.9525
    ## 8                   1026.8438
    ## 9                   2221.2650
    ## 10                   855.0401
    ## 11                  2171.9910
    ## 12                   857.2856
    ## 13                  2215.7100
    ## 14                   864.2018

Note: Set weekdays to be in order

``` r
all_trips_v2$day_of_week <- ordered(all_trips_v2$day_of_week, levels=c("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"))
```

Note: Now, let’s run the average ride time by each day for members vs
casual users

``` r
aggregate(all_trips_v2$ride_duration ~ all_trips_v2$member_casual + all_trips_v2$day_of_week, FUN = mean)
```

    ##    all_trips_v2$member_casual all_trips_v2$day_of_week
    ## 1                      casual                   Sunday
    ## 2                      member                   Sunday
    ## 3                      casual                   Monday
    ## 4                      member                   Monday
    ## 5                      casual                  Tuesday
    ## 6                      member                  Tuesday
    ## 7                      casual                Wednesday
    ## 8                      member                Wednesday
    ## 9                      casual                 Thursday
    ## 10                     member                 Thursday
    ## 11                     casual                   Friday
    ## 12                     member                   Friday
    ## 13                     casual                 Saturday
    ## 14                     member                 Saturday
    ##    all_trips_v2$ride_duration
    ## 1                   2820.9525
    ## 2                   1026.8438
    ## 3                   2375.1075
    ## 4                    875.7180
    ## 5                   2171.9910
    ## 6                    857.2856
    ## 7                   2215.7100
    ## 8                    864.2018
    ## 9                   2221.2650
    ## 10                   855.0401
    ## 11                  2351.0295
    ## 12                   897.7132
    ## 13                  2612.9410
    ## 14                   996.5180

Note: analyze ridership data by type and weekday

``` r
all_trips_v2 %>% 
  mutate(weekday = wday(started_at, label = TRUE)) %>%  #creates weekday field using wday()
  group_by(member_casual, weekday) %>%  #groups by usertype and weekday
  summarise(number_of_rides = n(), average_duration = mean(ride_duration)) %>%  #calculates the number of rides and average duration 
  arrange(member_casual, weekday)                       # sorts
```

    ## `summarise()` has grouped output by 'member_casual'. You can override using the `.groups` argument.

    ## # A tibble: 14 x 4
    ## # Groups:   member_casual [2]
    ##    member_casual weekday number_of_rides average_duration
    ##    <chr>         <ord>             <int>            <dbl>
    ##  1 casual        Sun              366054            2821.
    ##  2 casual        Mon              209137            2375.
    ##  3 casual        Tue              203934            2172.
    ##  4 casual        Wed              215333            2216.
    ##  5 casual        Thu              212287            2221.
    ##  6 casual        Fri              280655            2351.
    ##  7 casual        Sat              438911            2613.
    ##  8 member        Sun              322523            1027.
    ##  9 member        Mon              336810             876.
    ## 10 member        Tue              363607             857.
    ## 11 member        Wed              388301             864.
    ## 12 member        Thu              363082             855.
    ## 13 member        Fri              373845             898.
    ## 14 member        Sat              375800             997.

#### Visualize the analysed results

Note: Let’s visualize the number of rides by rider type

``` r
all_trips_v2 %>% 
  mutate(weekday = wday(started_at, label = TRUE)) %>% 
  group_by(member_casual, weekday) %>% 
  summarise(number_of_rides = n(), average_duration = mean(ride_duration)) %>% 
  arrange(member_casual, weekday)  %>% 
  ggplot(aes(x = weekday, y = number_of_rides, fill = member_casual)) +
  geom_col(position = "dodge")
```

    ## `summarise()` has grouped output by 'member_casual'. You can override using the `.groups` argument.

![](bike_users_files/figure-gfm/Visualize%20number%20of%20rides%20on%20weekdays%20by%20members%20and%20casual%20users-1.png)<!-- -->

Note: Let’s create a visualization for average duration

``` r
all_trips_v2 %>% 
  mutate(weekday = wday(started_at, label = TRUE)) %>% 
  group_by(member_casual, weekday) %>% 
  summarise(number_of_rides = n(), average_duration = mean(ride_duration)) %>% 
  arrange(member_casual, weekday)  %>% 
  ggplot(aes(x = weekday, y = average_duration, fill = member_casual)) +
  geom_col(position = "dodge")
```

    ## `summarise()` has grouped output by 'member_casual'. You can override using the `.groups` argument.

![](bike_users_files/figure-gfm/Visualization%20for%20average%20duration%20on%20weekdays%20by%20members%20and%20casual%20users-1.png)<!-- -->

#### Export Summary File fo Further Analysis
