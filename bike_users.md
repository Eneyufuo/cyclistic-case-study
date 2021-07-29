How casual members and annual members use bikes differently
================
Eneye
7/29/2021

## Set Up Environment

Note: Install and load Packages: tidyverse, ggplot2 and lubridate

## Collect Data

Note: Upload the ride sharing data sets

## Explore data and merge into one sheet

Note: Compare column names each of the files. While the names don’t have
to be in the same order, they do need to match perfectly before we can
use a command to join them into one file

Note: make ride\_length data type consistent

Note: make start\_station \_id and end\_station\_id data type consistent

Note: Merge all months to one year

``` r
all_trips <- bind_rows(july_2020, august_2020, september_2020, october_2020, november_2020, december_2020, january_2021, february_2021, march_2021, april_2021, may_2021, june_2021)
```

## Clean Consolidated Data

Note: Delete columns that are not needed

``` r
all_trips$X17 <- NULL
all_trips$X18 <- NULL
all_trips$X19 <- NULL
all_trips$X20 <- NULL
all_trips$X21 <- NULL
all_trips$X22 <- NULL
all_trips$X23 <- NULL
all_trips$X24 <- NULL
all_trips$days_of_week <- NULL
```

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
all_trips <- mutate(all_trips, started_at = mdy_hm(started_at))
all_trips <- mutate(all_trips, ended_at = mdy_hm(ended_at))
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

## COnduct Descriptive Analysis

Note: Descriptive Analysis on ride\_duration

``` r
summary(all_trips_v2$ride_duration)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##       0     480     840    1720    1500 3356640

Note: Compare members and casual users

``` r
aggregate(all_trips_v2$ride_duration ~ all_trips_v2$member_casual, FUN = mean)
```

    ##   all_trips_v2$member_casual all_trips_v2$ride_duration
    ## 1                     casual                   2520.662
    ## 2                     member                   1109.304

``` r
aggregate(all_trips_v2$ride_duration ~ all_trips_v2$member_casual, FUN = median)
```

    ##   all_trips_v2$member_casual all_trips_v2$ride_duration
    ## 1                     casual                       1140
    ## 2                     member                        660

``` r
aggregate(all_trips_v2$ride_duration ~ all_trips_v2$member_casual, FUN = max)
```

    ##   all_trips_v2$member_casual all_trips_v2$ride_duration
    ## 1                     casual                    3356640
    ## 2                     member                    2005260

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
    ## 1                   2346.7375
    ## 2                    895.6428
    ## 3                   2372.2447
    ## 4                    873.9371
    ## 5                   2608.2320
    ## 6                    993.7208
    ## 7                   2816.9718
    ## 8                   1024.1281
    ## 9                   2226.5666
    ## 10                   862.2998
    ## 11                  2169.0831
    ## 12                   855.3662
    ## 13                  2832.0248
    ## 14                  2169.8400

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
    ## 1                   2816.9718
    ## 2                   1024.1281
    ## 3                   2372.2447
    ## 4                    873.9371
    ## 5                   2169.0831
    ## 6                    855.3662
    ## 7                   2832.0248
    ## 8                   2169.8400
    ## 9                   2226.5666
    ## 10                   862.2998
    ## 11                  2346.7375
    ## 12                   895.6428
    ## 13                  2608.2320
    ## 14                   993.7208

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
    ##  1 casual        Sun              366593            2817.
    ##  2 casual        Mon              209391            2372.
    ##  3 casual        Tue              204222            2169.
    ##  4 casual        Wed              215685            2832.
    ##  5 casual        Thu              212530            2227.
    ##  6 casual        Fri              281188            2347.
    ##  7 casual        Sat              439710            2608.
    ##  8 member        Sun              323420            1024.
    ##  9 member        Mon              337503             874.
    ## 10 member        Tue              364505             855.
    ## 11 member        Wed              389547            2170.
    ## 12 member        Thu              363894             862.
    ## 13 member        Fri              375058             896.
    ## 14 member        Sat              376904             994.

## Visualize the analysed results

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

## Export Summary File fo Further Analysis
