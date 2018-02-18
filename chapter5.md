Chapter 5 - R for Data Science
================

5.1 Data Tranformation
======================

5.1.1 Prerequisites
-------------------

``` r
# Let's load the NYC Flights package and tidyverse
library(nycflights13)
```

    ## Warning: package 'nycflights13' was built under R version 3.4.3

``` r
library(tidyverse)
```

    ## Loading tidyverse: ggplot2
    ## Loading tidyverse: tibble
    ## Loading tidyverse: tidyr
    ## Loading tidyverse: readr
    ## Loading tidyverse: purrr
    ## Loading tidyverse: dplyr

    ## Conflicts with tidy packages ----------------------------------------------

    ## filter(): dplyr, stats
    ## lag():    dplyr, stats

5.1.2 NYCFlights 13 - Data Frame containing departed flights from NYC for 2013
------------------------------------------------------------------------------

``` r
# let's view the data frame
flights
```

    ## # A tibble: 336,776 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
    ##  1  2013     1     1      517            515         2      830
    ##  2  2013     1     1      533            529         4      850
    ##  3  2013     1     1      542            540         2      923
    ##  4  2013     1     1      544            545        -1     1004
    ##  5  2013     1     1      554            600        -6      812
    ##  6  2013     1     1      554            558        -4      740
    ##  7  2013     1     1      555            600        -5      913
    ##  8  2013     1     1      557            600        -3      709
    ##  9  2013     1     1      557            600        -3      838
    ## 10  2013     1     1      558            600        -2      753
    ## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
    ## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
    ## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
    ## #   minute <dbl>, time_hour <dttm>

5.2 Filtering rows with filter()
--------------------------------

``` r
# select flights for the 1st of Jan
filter(flights, month == 1, day == 1)
```

    ## # A tibble: 842 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
    ##  1  2013     1     1      517            515         2      830
    ##  2  2013     1     1      533            529         4      850
    ##  3  2013     1     1      542            540         2      923
    ##  4  2013     1     1      544            545        -1     1004
    ##  5  2013     1     1      554            600        -6      812
    ##  6  2013     1     1      554            558        -4      740
    ##  7  2013     1     1      555            600        -5      913
    ##  8  2013     1     1      557            600        -3      709
    ##  9  2013     1     1      557            600        -3      838
    ## 10  2013     1     1      558            600        -2      753
    ## # ... with 832 more rows, and 12 more variables: sched_arr_time <int>,
    ## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
    ## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
    ## #   minute <dbl>, time_hour <dttm>

``` r
jan1 <- filter(flights, month == 1, day == 1)

(dec25 <- filter(flights, month == 12, day == 25))
```

    ## # A tibble: 719 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
    ##  1  2013    12    25      456            500        -4      649
    ##  2  2013    12    25      524            515         9      805
    ##  3  2013    12    25      542            540         2      832
    ##  4  2013    12    25      546            550        -4     1022
    ##  5  2013    12    25      556            600        -4      730
    ##  6  2013    12    25      557            600        -3      743
    ##  7  2013    12    25      557            600        -3      818
    ##  8  2013    12    25      559            600        -1      855
    ##  9  2013    12    25      559            600        -1      849
    ## 10  2013    12    25      600            600         0      850
    ## # ... with 709 more rows, and 12 more variables: sched_arr_time <int>,
    ## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
    ## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
    ## #   minute <dbl>, time_hour <dttm>

5.2.2 Logical Operators
-----------------------

``` r
nov_dec <- filter(flights, month %in% c(11, 12))
```

5.2.3 Missing values
--------------------

``` r
# filter only includes rows where the condition is TRUE, meaning that
# it won't include NA value unless specifically mentioned

df <- tibble(x = c(1, NA, 3))
filter(df, x > 1)
```

    ## # A tibble: 1 x 1
    ##       x
    ##   <dbl>
    ## 1     3

``` r
filter(df, is.na(x) | x > 1)
```

    ## # A tibble: 2 x 1
    ##       x
    ##   <dbl>
    ## 1    NA
    ## 2     3

5.2.4 Exercises
---------------

``` r
# Flights with arrival delay of 2 or more hours
delayed_flights <- filter(flights, arr_delay >= 120)
paste('Number of delayed flights: ', count(delayed_flights))
```

    ## [1] "Number of delayed flights:  10200"

``` r
# Flights to Houston
hou_flights <- filter(flights, dest %in% c('IAH', 'HOU'))
paste('Number of flights to Houston: ', count(hou_flights))
```

    ## [1] "Number of flights to Houston:  9313"

``` r
# Flights operated by United, American or Delta
uad_flights <- filter(flights, carrier %in% c('UA', 'AA', 'DL'))
paste('Number of flights operated by United, American or Delta: ', count(uad_flights))
```

    ## [1] "Number of flights operated by United, American or Delta:  139504"

``` r
# Flights departed in summer
summer_flights <- filter(flights, month %in% c(7,8,9))
paste('Number of flights in July, Aug, Sep is:', count(summer_flights))
```

    ## [1] "Number of flights in July, Aug, Sep is: 86326"

``` r
# Flights that arrived more than 2 hours late but didn't leave late
late_flights <- filter(flights, arr_delay >= 120 & dep_delay <= 0)
paste('Number of flights arrived 2 hrs late but no late dep:', count(late_flights))
```

    ## [1] "Number of flights arrived 2 hrs late but no late dep: 29"

``` r
# Flights delayed at least 1 hr, but over 30 min in flight
dl_flights <- filter(flights, (dep_delay >= 60 | arr_delay >= 60) & air_time >= 30)
paste('Number of flights delayed least 1 hour, over 30 mins:', count(dl_flights))
```

    ## [1] "Number of flights delayed least 1 hour, over 30 mins: 31932"

``` r
# Flights departed between midnight and 6am
mid_flights <- filter(flights, !(dep_time < 2400 & dep_time >= 600))
mid_flights <- filter(flights, !between(dep_time, 600, 2400))

# Flights with missing dep_time
miss_flights <- filter(flights, is.na(dep_time))

# Flights missing departure times along with other variables such as dep_delay, arr_time, sched_arr_time might mean canceled flights
```

5.3 Arrange rows with arrange()
-------------------------------

``` r
# arrange function
arrange(flights, year, month, day)
```

    ## # A tibble: 336,776 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
    ##  1  2013     1     1      517            515         2      830
    ##  2  2013     1     1      533            529         4      850
    ##  3  2013     1     1      542            540         2      923
    ##  4  2013     1     1      544            545        -1     1004
    ##  5  2013     1     1      554            600        -6      812
    ##  6  2013     1     1      554            558        -4      740
    ##  7  2013     1     1      555            600        -5      913
    ##  8  2013     1     1      557            600        -3      709
    ##  9  2013     1     1      557            600        -3      838
    ## 10  2013     1     1      558            600        -2      753
    ## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
    ## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
    ## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
    ## #   minute <dbl>, time_hour <dttm>

``` r
# Use desc() to re-order by a column
arrange(flights, desc(arr_delay))
```

    ## # A tibble: 336,776 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
    ##  1  2013     1     9      641            900      1301     1242
    ##  2  2013     6    15     1432           1935      1137     1607
    ##  3  2013     1    10     1121           1635      1126     1239
    ##  4  2013     9    20     1139           1845      1014     1457
    ##  5  2013     7    22      845           1600      1005     1044
    ##  6  2013     4    10     1100           1900       960     1342
    ##  7  2013     3    17     2321            810       911      135
    ##  8  2013     7    22     2257            759       898      121
    ##  9  2013    12     5      756           1700       896     1058
    ## 10  2013     5     3     1133           2055       878     1250
    ## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
    ## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
    ## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
    ## #   minute <dbl>, time_hour <dttm>

5.3.1 Exercises
---------------

``` r
# use arrange to sort missing values to start
arrange(flights, desc(is.na(dep_time)))
```

    ## # A tibble: 336,776 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
    ##  1  2013     1     1       NA           1630        NA       NA
    ##  2  2013     1     1       NA           1935        NA       NA
    ##  3  2013     1     1       NA           1500        NA       NA
    ##  4  2013     1     1       NA            600        NA       NA
    ##  5  2013     1     2       NA           1540        NA       NA
    ##  6  2013     1     2       NA           1620        NA       NA
    ##  7  2013     1     2       NA           1355        NA       NA
    ##  8  2013     1     2       NA           1420        NA       NA
    ##  9  2013     1     2       NA           1321        NA       NA
    ## 10  2013     1     2       NA           1545        NA       NA
    ## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
    ## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
    ## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
    ## #   minute <dbl>, time_hour <dttm>

``` r
# sort flights to find most delayed ones
arrange(flights, desc(arr_delay), desc(dep_delay), dep_time)
```

    ## # A tibble: 336,776 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
    ##  1  2013     1     9      641            900      1301     1242
    ##  2  2013     6    15     1432           1935      1137     1607
    ##  3  2013     1    10     1121           1635      1126     1239
    ##  4  2013     9    20     1139           1845      1014     1457
    ##  5  2013     7    22      845           1600      1005     1044
    ##  6  2013     4    10     1100           1900       960     1342
    ##  7  2013     3    17     2321            810       911      135
    ##  8  2013     7    22     2257            759       898      121
    ##  9  2013    12     5      756           1700       896     1058
    ## 10  2013     5     3     1133           2055       878     1250
    ## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
    ## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
    ## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
    ## #   minute <dbl>, time_hour <dttm>

``` r
# sort flights to find the fastest flights
arrange(flights, dep_delay)
```

    ## # A tibble: 336,776 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
    ##  1  2013    12     7     2040           2123       -43       40
    ##  2  2013     2     3     2022           2055       -33     2240
    ##  3  2013    11    10     1408           1440       -32     1549
    ##  4  2013     1    11     1900           1930       -30     2233
    ##  5  2013     1    29     1703           1730       -27     1947
    ##  6  2013     8     9      729            755       -26     1002
    ##  7  2013    10    23     1907           1932       -25     2143
    ##  8  2013     3    30     2030           2055       -25     2213
    ##  9  2013     3     2     1431           1455       -24     1601
    ## 10  2013     5     5      934            958       -24     1225
    ## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
    ## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
    ## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
    ## #   minute <dbl>, time_hour <dttm>

``` r
# flights travelling the longest & shortest
arrange(flights, desc(air_time))
```

    ## # A tibble: 336,776 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
    ##  1  2013     3    17     1337           1335         2     1937
    ##  2  2013     2     6      853            900        -7     1542
    ##  3  2013     3    15     1001           1000         1     1551
    ##  4  2013     3    17     1006           1000         6     1607
    ##  5  2013     3    16     1001           1000         1     1544
    ##  6  2013     2     5      900            900         0     1555
    ##  7  2013    11    12      936            930         6     1630
    ##  8  2013     3    14      958           1000        -2     1542
    ##  9  2013    11    20     1006           1000         6     1639
    ## 10  2013     3    15     1342           1335         7     1924
    ## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
    ## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
    ## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
    ## #   minute <dbl>, time_hour <dttm>

``` r
arrange(flights, air_time)
```

    ## # A tibble: 336,776 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
    ##  1  2013     1    16     1355           1315        40     1442
    ##  2  2013     4    13      537            527        10      622
    ##  3  2013    12     6      922            851        31     1021
    ##  4  2013     2     3     2153           2129        24     2247
    ##  5  2013     2     5     1303           1315       -12     1342
    ##  6  2013     2    12     2123           2130        -7     2211
    ##  7  2013     3     2     1450           1500       -10     1547
    ##  8  2013     3     8     2026           1935        51     2131
    ##  9  2013     3    18     1456           1329        87     1533
    ## 10  2013     3    19     2226           2145        41     2305
    ## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
    ## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
    ## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
    ## #   minute <dbl>, time_hour <dttm>

5.4 Select columns with select()
--------------------------------

``` r
select(flights, year, month, day)
```

    ## # A tibble: 336,776 x 3
    ##     year month   day
    ##    <int> <int> <int>
    ##  1  2013     1     1
    ##  2  2013     1     1
    ##  3  2013     1     1
    ##  4  2013     1     1
    ##  5  2013     1     1
    ##  6  2013     1     1
    ##  7  2013     1     1
    ##  8  2013     1     1
    ##  9  2013     1     1
    ## 10  2013     1     1
    ## # ... with 336,766 more rows

``` r
# Select all columns between year and day (inclusive)
select(flights, year:day)
```

    ## # A tibble: 336,776 x 3
    ##     year month   day
    ##    <int> <int> <int>
    ##  1  2013     1     1
    ##  2  2013     1     1
    ##  3  2013     1     1
    ##  4  2013     1     1
    ##  5  2013     1     1
    ##  6  2013     1     1
    ##  7  2013     1     1
    ##  8  2013     1     1
    ##  9  2013     1     1
    ## 10  2013     1     1
    ## # ... with 336,766 more rows

``` r
# Select all columns except those from year to day (inclusive)
select(flights, -(year:arr_time))
```

    ## # A tibble: 336,776 x 12
    ##    sched_arr_time arr_delay carrier flight tailnum origin  dest air_time
    ##             <int>     <dbl>   <chr>  <int>   <chr>  <chr> <chr>    <dbl>
    ##  1            819        11      UA   1545  N14228    EWR   IAH      227
    ##  2            830        20      UA   1714  N24211    LGA   IAH      227
    ##  3            850        33      AA   1141  N619AA    JFK   MIA      160
    ##  4           1022       -18      B6    725  N804JB    JFK   BQN      183
    ##  5            837       -25      DL    461  N668DN    LGA   ATL      116
    ##  6            728        12      UA   1696  N39463    EWR   ORD      150
    ##  7            854        19      B6    507  N516JB    EWR   FLL      158
    ##  8            723       -14      EV   5708  N829AS    LGA   IAD       53
    ##  9            846        -8      B6     79  N593JB    JFK   MCO      140
    ## 10            745         8      AA    301  N3ALAA    LGA   ORD      138
    ## # ... with 336,766 more rows, and 4 more variables: distance <dbl>,
    ## #   hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
# to rename columns
rename(flights, tail_num = tailnum)
```

    ## # A tibble: 336,776 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
    ##  1  2013     1     1      517            515         2      830
    ##  2  2013     1     1      533            529         4      850
    ##  3  2013     1     1      542            540         2      923
    ##  4  2013     1     1      544            545        -1     1004
    ##  5  2013     1     1      554            600        -6      812
    ##  6  2013     1     1      554            558        -4      740
    ##  7  2013     1     1      555            600        -5      913
    ##  8  2013     1     1      557            600        -3      709
    ##  9  2013     1     1      557            600        -3      838
    ## 10  2013     1     1      558            600        -2      753
    ## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
    ## #   arr_delay <dbl>, carrier <chr>, flight <int>, tail_num <chr>,
    ## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
    ## #   minute <dbl>, time_hour <dttm>

``` r
# useful to move handful of variables to the start of the data frame
select(flights, time_hour, air_time, dep_time, everything())
```

    ## # A tibble: 336,776 x 19
    ##              time_hour air_time dep_time  year month   day sched_dep_time
    ##                 <dttm>    <dbl>    <int> <int> <int> <int>          <int>
    ##  1 2013-01-01 05:00:00      227      517  2013     1     1            515
    ##  2 2013-01-01 05:00:00      227      533  2013     1     1            529
    ##  3 2013-01-01 05:00:00      160      542  2013     1     1            540
    ##  4 2013-01-01 05:00:00      183      544  2013     1     1            545
    ##  5 2013-01-01 06:00:00      116      554  2013     1     1            600
    ##  6 2013-01-01 05:00:00      150      554  2013     1     1            558
    ##  7 2013-01-01 06:00:00      158      555  2013     1     1            600
    ##  8 2013-01-01 06:00:00       53      557  2013     1     1            600
    ##  9 2013-01-01 06:00:00      140      557  2013     1     1            600
    ## 10 2013-01-01 06:00:00      138      558  2013     1     1            600
    ## # ... with 336,766 more rows, and 12 more variables: dep_delay <dbl>,
    ## #   arr_time <int>, sched_arr_time <int>, arr_delay <dbl>, carrier <chr>,
    ## #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>, distance <dbl>,
    ## #   hour <dbl>, minute <dbl>

5.4.1 Exercises
---------------

``` r
# 2 ways to select
select(flights, dep_time, dep_delay, arr_time, arr_delay)
```

    ## # A tibble: 336,776 x 4
    ##    dep_time dep_delay arr_time arr_delay
    ##       <int>     <dbl>    <int>     <dbl>
    ##  1      517         2      830        11
    ##  2      533         4      850        20
    ##  3      542         2      923        33
    ##  4      544        -1     1004       -18
    ##  5      554        -6      812       -25
    ##  6      554        -4      740        12
    ##  7      555        -5      913        19
    ##  8      557        -3      709       -14
    ##  9      557        -3      838        -8
    ## 10      558        -2      753         8
    ## # ... with 336,766 more rows

``` r
flights[c('dep_time', 'dep_delay', 'arr_time', 'arr_delay')]
```

    ## # A tibble: 336,776 x 4
    ##    dep_time dep_delay arr_time arr_delay
    ##       <int>     <dbl>    <int>     <dbl>
    ##  1      517         2      830        11
    ##  2      533         4      850        20
    ##  3      542         2      923        33
    ##  4      544        -1     1004       -18
    ##  5      554        -6      812       -25
    ##  6      554        -4      740        12
    ##  7      555        -5      913        19
    ##  8      557        -3      709       -14
    ##  9      557        -3      838        -8
    ## 10      558        -2      753         8
    ## # ... with 336,766 more rows

``` r
# what happens if we include name of var in select() multiple times
select(flights, dep_time, dep_time)
```

    ## # A tibble: 336,776 x 1
    ##    dep_time
    ##       <int>
    ##  1      517
    ##  2      533
    ##  3      542
    ##  4      544
    ##  5      554
    ##  6      554
    ##  7      555
    ##  8      557
    ##  9      557
    ## 10      558
    ## # ... with 336,766 more rows

``` r
# what does one_of() function do?
varsi <- c("year", "month", "day", "dep_delay", "arr_delay")

# one_of needs to be used in conjunction with select()
select(flights, one_of(varsi))
```

    ## # A tibble: 336,776 x 5
    ##     year month   day dep_delay arr_delay
    ##    <int> <int> <int>     <dbl>     <dbl>
    ##  1  2013     1     1         2        11
    ##  2  2013     1     1         4        20
    ##  3  2013     1     1         2        33
    ##  4  2013     1     1        -1       -18
    ##  5  2013     1     1        -6       -25
    ##  6  2013     1     1        -4        12
    ##  7  2013     1     1        -5        19
    ##  8  2013     1     1        -3       -14
    ##  9  2013     1     1        -3        -8
    ## 10  2013     1     1        -2         8
    ## # ... with 336,766 more rows

``` r
# is the result surprising?
select(flights, contains("TIME"))
```

    ## # A tibble: 336,776 x 6
    ##    dep_time sched_dep_time arr_time sched_arr_time air_time
    ##       <int>          <int>    <int>          <int>    <dbl>
    ##  1      517            515      830            819      227
    ##  2      533            529      850            830      227
    ##  3      542            540      923            850      160
    ##  4      544            545     1004           1022      183
    ##  5      554            600      812            837      116
    ##  6      554            558      740            728      150
    ##  7      555            600      913            854      158
    ##  8      557            600      709            723       53
    ##  9      557            600      838            846      140
    ## 10      558            600      753            745      138
    ## # ... with 336,766 more rows, and 1 more variables: time_hour <dttm>

``` r
# basically selects all vars which have time in their name
```

5.5 Adding new vars with mutate()
---------------------------------

``` r
# mutate adds new columns
flights_sml <- select(flights, 
  year:day, 
  ends_with("delay"), 
  distance, 
  air_time
)

mutate(flights_sml,
  gain = arr_delay - dep_delay,
  speed = distance / air_time * 60
)
```

    ## # A tibble: 336,776 x 9
    ##     year month   day dep_delay arr_delay distance air_time  gain    speed
    ##    <int> <int> <int>     <dbl>     <dbl>    <dbl>    <dbl> <dbl>    <dbl>
    ##  1  2013     1     1         2        11     1400      227     9 370.0441
    ##  2  2013     1     1         4        20     1416      227    16 374.2731
    ##  3  2013     1     1         2        33     1089      160    31 408.3750
    ##  4  2013     1     1        -1       -18     1576      183   -17 516.7213
    ##  5  2013     1     1        -6       -25      762      116   -19 394.1379
    ##  6  2013     1     1        -4        12      719      150    16 287.6000
    ##  7  2013     1     1        -5        19     1065      158    24 404.4304
    ##  8  2013     1     1        -3       -14      229       53   -11 259.2453
    ##  9  2013     1     1        -3        -8      944      140    -5 404.5714
    ## 10  2013     1     1        -2         8      733      138    10 318.6957
    ## # ... with 336,766 more rows

``` r
# its possible to refer to newly created columns
mutate(flights_sml,
  gain = arr_delay - dep_delay,
  hours = air_time / 60,
  gain_per_hour = gain / hours
)
```

    ## # A tibble: 336,776 x 10
    ##     year month   day dep_delay arr_delay distance air_time  gain     hours
    ##    <int> <int> <int>     <dbl>     <dbl>    <dbl>    <dbl> <dbl>     <dbl>
    ##  1  2013     1     1         2        11     1400      227     9 3.7833333
    ##  2  2013     1     1         4        20     1416      227    16 3.7833333
    ##  3  2013     1     1         2        33     1089      160    31 2.6666667
    ##  4  2013     1     1        -1       -18     1576      183   -17 3.0500000
    ##  5  2013     1     1        -6       -25      762      116   -19 1.9333333
    ##  6  2013     1     1        -4        12      719      150    16 2.5000000
    ##  7  2013     1     1        -5        19     1065      158    24 2.6333333
    ##  8  2013     1     1        -3       -14      229       53   -11 0.8833333
    ##  9  2013     1     1        -3        -8      944      140    -5 2.3333333
    ## 10  2013     1     1        -2         8      733      138    10 2.3000000
    ## # ... with 336,766 more rows, and 1 more variables: gain_per_hour <dbl>

``` r
# if we want to keep only the new variables use transmute()
transmute(flights,
  gain = arr_delay - dep_delay,
  hours = air_time / 60,
  gain_per_hour = gain / hours
)
```

    ## # A tibble: 336,776 x 3
    ##     gain     hours gain_per_hour
    ##    <dbl>     <dbl>         <dbl>
    ##  1     9 3.7833333      2.378855
    ##  2    16 3.7833333      4.229075
    ##  3    31 2.6666667     11.625000
    ##  4   -17 3.0500000     -5.573770
    ##  5   -19 1.9333333     -9.827586
    ##  6    16 2.5000000      6.400000
    ##  7    24 2.6333333      9.113924
    ##  8   -11 0.8833333    -12.452830
    ##  9    -5 2.3333333     -2.142857
    ## 10    10 2.3000000      4.347826
    ## # ... with 336,766 more rows

5.5.1 Useful creation functions
-------------------------------

``` r
# let's create a simple vector (x <- 1:10)
(x <- 1:10)
```

    ##  [1]  1  2  3  4  5  6  7  8  9 10

``` r
x
```

    ##  [1]  1  2  3  4  5  6  7  8  9 10

``` r
# lag and lead
lag(x)
```

    ##  [1] NA  1  2  3  4  5  6  7  8  9

``` r
lead(x)
```

    ##  [1]  2  3  4  5  6  7  8  9 10 NA

``` r
# cumulative and rolling aggregates
cumsum(x)
```

    ##  [1]  1  3  6 10 15 21 28 36 45 55

``` r
cummean(x)
```

    ##  [1] 1.0 1.5 2.0 2.5 3.0 3.5 4.0 4.5 5.0 5.5

``` r
# ranking
y <- c(1, 2, 2, NA, 3, 4, 0)
min_rank(y)
```

    ## [1]  2  3  3 NA  5  6  1

``` r
min_rank(desc(y))
```

    ## [1]  5  3  3 NA  2  1  6

5.5.2 Exercises
---------------

``` r
# function to conver hh:mm to minutes
t2m <- function(time) {
  hours <- trunc(time/100)
  mins <- 100 * (time/100 - hours)
  newtime <- hours*60 + mins
  return(newtime)
}

# convert dep_time sched_dep_time to minutes since midnight
newflights <- mutate(flights,
  dep_time,
  sched_dep_time,
  new_dep_time = t2m(dep_time),
  new_sched_dep_time = t2m(sched_dep_time),
  diff_dep_arr = arr_time - dep_time
)

select(newflights, dep_time, new_dep_time, 
       sched_dep_time, new_sched_dep_time, diff_dep_arr)
```

    ## # A tibble: 336,776 x 5
    ##    dep_time new_dep_time sched_dep_time new_sched_dep_time diff_dep_arr
    ##       <int>        <dbl>          <int>              <dbl>        <int>
    ##  1      517          317            515                315          313
    ##  2      533          333            529                329          317
    ##  3      542          342            540                340          381
    ##  4      544          344            545                345          460
    ##  5      554          354            600                360          258
    ##  6      554          354            558                358          186
    ##  7      555          355            600                360          358
    ##  8      557          357            600                360          152
    ##  9      557          357            600                360          281
    ## 10      558          358            600                360          195
    ## # ... with 336,766 more rows

``` r
# comparing air_time with arr_time - dep_time
select(newflights, air_time, diff_dep_arr)
```

    ## # A tibble: 336,776 x 2
    ##    air_time diff_dep_arr
    ##       <dbl>        <int>
    ##  1      227          313
    ##  2      227          317
    ##  3      160          381
    ##  4      183          460
    ##  5      116          258
    ##  6      150          186
    ##  7      158          358
    ##  8       53          152
    ##  9      140          281
    ## 10      138          195
    ## # ... with 336,766 more rows

``` r
"
After comparing air time to the different between arrival time and departure time
we see quite a difference
"
```

    ## [1] "\nAfter comparing air time to the different between arrival time and departure time\nwe see quite a difference\n"

``` r
# compare dep_time, sched_dep_time, dep_delay
select(flights, dep_time, sched_dep_time, dep_delay)
```

    ## # A tibble: 336,776 x 3
    ##    dep_time sched_dep_time dep_delay
    ##       <int>          <int>     <dbl>
    ##  1      517            515         2
    ##  2      533            529         4
    ##  3      542            540         2
    ##  4      544            545        -1
    ##  5      554            600        -6
    ##  6      554            558        -4
    ##  7      555            600        -5
    ##  8      557            600        -3
    ##  9      557            600        -3
    ## 10      558            600        -2
    ## # ... with 336,766 more rows

``` r
"The above 3 features are related since delay is sched_dep_time - dep_time"
```

    ## [1] "The above 3 features are related since delay is sched_dep_time - dep_time"

``` r
# 10 most delayed flights
head(arrange(select(flights, year, month, day, dep_time, 
                    sched_dep_time, dep_delay, carrier, origin, dest), 
             min_rank(desc(dep_delay))), n=10)
```

    ## # A tibble: 10 x 9
    ##     year month   day dep_time sched_dep_time dep_delay carrier origin
    ##    <int> <int> <int>    <int>          <int>     <dbl>   <chr>  <chr>
    ##  1  2013     1     9      641            900      1301      HA    JFK
    ##  2  2013     6    15     1432           1935      1137      MQ    JFK
    ##  3  2013     1    10     1121           1635      1126      MQ    EWR
    ##  4  2013     9    20     1139           1845      1014      AA    JFK
    ##  5  2013     7    22      845           1600      1005      MQ    JFK
    ##  6  2013     4    10     1100           1900       960      DL    JFK
    ##  7  2013     3    17     2321            810       911      DL    LGA
    ##  8  2013     6    27      959           1900       899      DL    JFK
    ##  9  2013     7    22     2257            759       898      DL    LGA
    ## 10  2013    12     5      756           1700       896      AA    EWR
    ## # ... with 1 more variables: dest <chr>

``` r
# task num 5 - ans: not the same length
1:3 + 1:10
```

    ## Warning in 1:3 + 1:10: la taille d'un objet plus long n'est pas multiple de
    ## la taille d'un objet plus court

    ##  [1]  2  4  6  5  7  9  8 10 12 11

``` r
# R provides trig functions
x <- 5
cos(x)
```

    ## [1] 0.2836622

``` r
sin(x)
```

    ## [1] -0.9589243

``` r
tan(x)
```

    ## [1] -3.380515

``` r
acos(x)
```

    ## Warning in acos(x): production de NaN

    ## [1] NaN

``` r
asin(x)
```

    ## Warning in asin(x): production de NaN

    ## [1] NaN

``` r
atan(x)
```

    ## [1] 1.373401

``` r
atan2(y, x)
```

    ## [1] 0.1973956 0.3805064 0.3805064        NA 0.5404195 0.6747409 0.0000000

``` r
cospi(x)
```

    ## [1] -1

``` r
sinpi(x)
```

    ## [1] 0

``` r
tanpi(x)
```

    ## [1] 0

5.6 Grouped summaries with summarise()
--------------------------------------

``` r
# intro
summarise(flights, delay=mean(dep_delay, na.rm=T))
```

    ## # A tibble: 1 x 1
    ##      delay
    ##      <dbl>
    ## 1 12.63907

``` r
# group_by
by_day <- group_by(flights, year, month, day)
summarise(by_day, delay=mean(dep_delay, na.rm=T))
```

    ## # A tibble: 365 x 4
    ## # Groups:   year, month [?]
    ##     year month   day     delay
    ##    <int> <int> <int>     <dbl>
    ##  1  2013     1     1 11.548926
    ##  2  2013     1     2 13.858824
    ##  3  2013     1     3 10.987832
    ##  4  2013     1     4  8.951595
    ##  5  2013     1     5  5.732218
    ##  6  2013     1     6  7.148014
    ##  7  2013     1     7  5.417204
    ##  8  2013     1     8  2.553073
    ##  9  2013     1     9  2.276477
    ## 10  2013     1    10  2.844995
    ## # ... with 355 more rows

``` r
# imagine we want relationship between distance and avg delay for each location
by_dest <- group_by(flights, dest)
delay <- summarise(by_dest,
     count=n(),
     dist=mean(distance, na.rm = T),
     delay=mean(arr_delay, na.rm = T)
)
delay <- filter(delay, count>20, dest != "HNL")

# let's visualize
ggplot(data=delay, mapping=aes(x=dist,y=delay))+
  geom_point(aes(size=count), alpha=1/3)+
  geom_smooth(se=F)
```

    ## `geom_smooth()` using method = 'loess'

![](chapter5_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-14-1.png)

``` r
# there's a faster way to do the above using piping
delays <- flights %>% 
  group_by(dest) %>% 
  summarise(
    count = n(),
    dist = mean(distance, na.rm = TRUE),
    delay = mean(arr_delay, na.rm = TRUE)
  ) %>% 
  filter(count > 20, dest != "HNL")

# to reuse the dataset we will have a separate one
not_cancelled <- flights %>%
  filter(!is.na(dep_delay), !is.na(arr_delay))

not_cancelled %>%
  group_by(year, month, day) %>%
  summarise(mean=mean(dep_delay))
```

    ## # A tibble: 365 x 4
    ## # Groups:   year, month [?]
    ##     year month   day      mean
    ##    <int> <int> <int>     <dbl>
    ##  1  2013     1     1 11.435620
    ##  2  2013     1     2 13.677802
    ##  3  2013     1     3 10.907778
    ##  4  2013     1     4  8.965859
    ##  5  2013     1     5  5.732218
    ##  6  2013     1     6  7.145959
    ##  7  2013     1     7  5.417204
    ##  8  2013     1     8  2.558296
    ##  9  2013     1     9  2.301232
    ## 10  2013     1    10  2.844995
    ## # ... with 355 more rows

5.6.3 Count
-----------

``` r
# let's look at the planes (identified by their tail number) that have 
# the highest avg delays
delays <- not_cancelled %>%
  group_by(tailnum) %>%
  summarise(
    delay=mean(arr_delay)
  )

# visualize
ggplot(delays, aes(x=delay))+
  geom_freqpoly(binwidth=10)
```

![](chapter5_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-15-1.png)

``` r
# we can get a better picture with scatterplot
delays <- not_cancelled %>%
  group_by(tailnum) %>%
  summarise(
    delay=mean(arr_delay, na.rm=T),
    n=n()
  )


ggplot(delays, aes(n,delay))+
  geom_point(alpha=1/10)
```

![](chapter5_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-15-2.png)

``` r
# the code shows how to integrate ggplot into piping
delays %>%
  filter(n>50) %>%
  ggplot(aes(n, delay))+
    geom_point(alpha=1/10)
```

![](chapter5_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-15-3.png)

``` r
# Another variation using batters in baseball from Lahman package
batting <- as_tibble(Lahman::Batting)

batters <- batting %>%
  group_by(playerID) %>%
  summarise(
    ba=sum(H, na.rm = T)/sum(AB, na.rm = T),
    ab=sum(AB, na.rm = T)
  )

batters %>%
  filter(ab>100) %>%
  ggplot(aes(ab,ba))+
    geom_point()+
    geom_smooth(se=F)
```

    ## `geom_smooth()` using method = 'gam'

![](chapter5_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-15-4.png)

5.6.4 Some useful summary functions
-----------------------------------

``` r
# please refer below for more info
# http://r4ds.had.co.nz/transform.html#grouped-summaries-with-summarise

# first and last
not_cancelled %>% 
  group_by(year, month, day) %>%
  summarise(
    first_dep = first(dep_time),
    last_dep = last(dep_time)
  )
```

    ## # A tibble: 365 x 5
    ## # Groups:   year, month [?]
    ##     year month   day first_dep last_dep
    ##    <int> <int> <int>     <int>    <int>
    ##  1  2013     1     1       517     2356
    ##  2  2013     1     2        42     2354
    ##  3  2013     1     3        32     2349
    ##  4  2013     1     4        25     2358
    ##  5  2013     1     5        14     2357
    ##  6  2013     1     6        16     2355
    ##  7  2013     1     7        49     2359
    ##  8  2013     1     8       454     2351
    ##  9  2013     1     9         2     2252
    ## 10  2013     1    10         3     2320
    ## # ... with 355 more rows

``` r
# filtering with ranks
not_cancelled %>%
  group_by(year, month, day) %>%
  mutate(r=min_rank(desc(dep_time))) %>%
  filter(r %in% range(r)) %>%
  select(year, month, day, r)
```

    ## # A tibble: 770 x 4
    ## # Groups:   year, month, day [365]
    ##     year month   day     r
    ##    <int> <int> <int> <int>
    ##  1  2013     1     1   831
    ##  2  2013     1     1     1
    ##  3  2013     1     2   928
    ##  4  2013     1     2     1
    ##  5  2013     1     3   900
    ##  6  2013     1     3     1
    ##  7  2013     1     4   908
    ##  8  2013     1     4     1
    ##  9  2013     1     4     1
    ## 10  2013     1     5   717
    ## # ... with 760 more rows

``` r
# to count the number of distinct (unique) values
not_cancelled %>%
  group_by(dest) %>%
  summarise(carriers=n_distinct(carrier)) %>%
  arrange(desc(carriers))
```

    ## # A tibble: 104 x 2
    ##     dest carriers
    ##    <chr>    <int>
    ##  1   ATL        7
    ##  2   BOS        7
    ##  3   CLT        7
    ##  4   ORD        7
    ##  5   TPA        7
    ##  6   AUS        6
    ##  7   DCA        6
    ##  8   DTW        6
    ##  9   IAD        6
    ## 10   MSP        6
    ## # ... with 94 more rows

``` r
# count which dplyr provides
not_cancelled %>%
  count(dest)
```

    ## # A tibble: 104 x 2
    ##     dest     n
    ##    <chr> <int>
    ##  1   ABQ   254
    ##  2   ACK   264
    ##  3   ALB   418
    ##  4   ANC     8
    ##  5   ATL 16837
    ##  6   AUS  2411
    ##  7   AVL   261
    ##  8   BDL   412
    ##  9   BGR   358
    ## 10   BHM   269
    ## # ... with 94 more rows

``` r
# its also possible to provide a weight variable to count 'sum' e.g.
not_cancelled %>%
  count(tailnum, wt=air_time) %>%
  arrange(desc(n))
```

    ## # A tibble: 4,037 x 2
    ##    tailnum      n
    ##      <chr>  <dbl>
    ##  1  N328AA 123768
    ##  2  N338AA 122141
    ##  3  N335AA 120200
    ##  4  N327AA 120068
    ##  5  N319AA 111925
    ##  6  N323AA 111398
    ##  7  N336AA 111147
    ##  8  N329AA 109426
    ##  9  N324AA 104229
    ## 10  N339AA 104042
    ## # ... with 4,027 more rows

``` r
# using sum() and mean() to count e.g
# How many flights left before 5am? (these usually indicate delayed
# flights from the previous day)
not_cancelled %>%
  group_by(year, month, day) %>%
  summarise(n_early=sum(dep_time<500))
```

    ## # A tibble: 365 x 4
    ## # Groups:   year, month [?]
    ##     year month   day n_early
    ##    <int> <int> <int>   <int>
    ##  1  2013     1     1       0
    ##  2  2013     1     2       3
    ##  3  2013     1     3       4
    ##  4  2013     1     4       3
    ##  5  2013     1     5       3
    ##  6  2013     1     6       2
    ##  7  2013     1     7       2
    ##  8  2013     1     8       1
    ##  9  2013     1     9       3
    ## 10  2013     1    10       3
    ## # ... with 355 more rows

``` r
# What proportion of flights are delayed by more than an hour?
not_cancelled %>%
  group_by(year, month, day) %>%
  summarise(hour_perc=round(mean(arr_delay>60), 3)*100)
```

    ## # A tibble: 365 x 4
    ## # Groups:   year, month [?]
    ##     year month   day hour_perc
    ##    <int> <int> <int>     <dbl>
    ##  1  2013     1     1       7.2
    ##  2  2013     1     2       8.5
    ##  3  2013     1     3       5.7
    ##  4  2013     1     4       4.0
    ##  5  2013     1     5       3.5
    ##  6  2013     1     6       4.7
    ##  7  2013     1     7       3.3
    ##  8  2013     1     8       2.1
    ##  9  2013     1     9       2.0
    ## 10  2013     1    10       1.8
    ## # ... with 355 more rows

5.6.5 Grouping by multiple variables
------------------------------------

``` r
# possibility to group by multiple variables
daily <- group_by(flights, year, month, day)
(per_day <- summarise(daily, flights = n()))
```

    ## # A tibble: 365 x 4
    ## # Groups:   year, month [?]
    ##     year month   day flights
    ##    <int> <int> <int>   <int>
    ##  1  2013     1     1     842
    ##  2  2013     1     2     943
    ##  3  2013     1     3     914
    ##  4  2013     1     4     915
    ##  5  2013     1     5     720
    ##  6  2013     1     6     832
    ##  7  2013     1     7     933
    ##  8  2013     1     8     899
    ##  9  2013     1     9     902
    ## 10  2013     1    10     932
    ## # ... with 355 more rows

``` r
(per_month <- summarise(per_day, flights=sum(flights)))
```

    ## # A tibble: 12 x 3
    ## # Groups:   year [?]
    ##     year month flights
    ##    <int> <int>   <int>
    ##  1  2013     1   27004
    ##  2  2013     2   24951
    ##  3  2013     3   28834
    ##  4  2013     4   28330
    ##  5  2013     5   28796
    ##  6  2013     6   28243
    ##  7  2013     7   29425
    ##  8  2013     8   29327
    ##  9  2013     9   27574
    ## 10  2013    10   28889
    ## 11  2013    11   27268
    ## 12  2013    12   28135

``` r
(per_year <- summarise(per_month, flights=sum(flights)))
```

    ## # A tibble: 1 x 2
    ##    year flights
    ##   <int>   <int>
    ## 1  2013  336776

``` r
# unggrouping
daily %>%
  ungroup() %>%
  summarise(flights = n())
```

    ## # A tibble: 1 x 1
    ##   flights
    ##     <int>
    ## 1  336776

5.6.7 Exercises
---------------

### 1. Brainstorm 5 different ways to assess the typical delay of group of flights

``` r
str(flights)
```

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    336776 obs. of  19 variables:
    ##  $ year          : int  2013 2013 2013 2013 2013 2013 2013 2013 2013 2013 ...
    ##  $ month         : int  1 1 1 1 1 1 1 1 1 1 ...
    ##  $ day           : int  1 1 1 1 1 1 1 1 1 1 ...
    ##  $ dep_time      : int  517 533 542 544 554 554 555 557 557 558 ...
    ##  $ sched_dep_time: int  515 529 540 545 600 558 600 600 600 600 ...
    ##  $ dep_delay     : num  2 4 2 -1 -6 -4 -5 -3 -3 -2 ...
    ##  $ arr_time      : int  830 850 923 1004 812 740 913 709 838 753 ...
    ##  $ sched_arr_time: int  819 830 850 1022 837 728 854 723 846 745 ...
    ##  $ arr_delay     : num  11 20 33 -18 -25 12 19 -14 -8 8 ...
    ##  $ carrier       : chr  "UA" "UA" "AA" "B6" ...
    ##  $ flight        : int  1545 1714 1141 725 461 1696 507 5708 79 301 ...
    ##  $ tailnum       : chr  "N14228" "N24211" "N619AA" "N804JB" ...
    ##  $ origin        : chr  "EWR" "LGA" "JFK" "JFK" ...
    ##  $ dest          : chr  "IAH" "IAH" "MIA" "BQN" ...
    ##  $ air_time      : num  227 227 160 183 116 150 158 53 140 138 ...
    ##  $ distance      : num  1400 1416 1089 1576 762 ...
    ##  $ hour          : num  5 5 5 5 6 5 6 6 6 6 ...
    ##  $ minute        : num  15 29 40 45 0 58 0 0 0 0 ...
    ##  $ time_hour     : POSIXct, format: "2013-01-01 05:00:00" "2013-01-01 05:00:00" ...

``` r
head(flights, n=10)
```

    ## # A tibble: 10 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
    ##  1  2013     1     1      517            515         2      830
    ##  2  2013     1     1      533            529         4      850
    ##  3  2013     1     1      542            540         2      923
    ##  4  2013     1     1      544            545        -1     1004
    ##  5  2013     1     1      554            600        -6      812
    ##  6  2013     1     1      554            558        -4      740
    ##  7  2013     1     1      555            600        -5      913
    ##  8  2013     1     1      557            600        -3      709
    ##  9  2013     1     1      557            600        -3      838
    ## 10  2013     1     1      558            600        -2      753
    ## # ... with 12 more variables: sched_arr_time <int>, arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>,
    ## #   time_hour <dttm>

``` r
# first its useful to get summarise all and get percentages for each characteristic
flights_delay_summary <- flights %>%
  group_by(flight) %>%
  summarise(num_fl = n(),
            perc_on_time = sum(arr_time == sched_arr_time, na.rm=T)/num_fl,
            perc_early = sum(arr_time < sched_arr_time, na.rm=T)/num_fl,
            perc_late = sum(arr_time > sched_arr_time, na.rm=T)/num_fl,
            perc_10min_late = sum(sched_arr_time -arr_time == 10)/num_fl,
            perc_15min_early = sum(sched_arr_time - arr_time == 15, na.rm=T)/num_fl,
            perc_15min_late = sum(arr_time - sched_arr_time == 15, na.rm=T)/num_fl,
            perc_30min_early = sum(sched_arr_time - arr_time == 30, na.rm=T)/num_fl,
            perc_30min_late = sum(arr_time - sched_arr_time == 30, na.rm=T)/num_fl,
            perc_2hr_early = sum(sched_arr_time - arr_time == 120, na.rm=T)/num_fl,
            perc_2hr_late = sum(arr_time - sched_arr_time == 120, na.rm=T)/num_fl
)

# this allows us to answer the questions above using simply filtering
```

A flight is 15 minutes early 50% of the time, and 15 minutes late 50% of the time.

``` r
filter(flights_delay_summary, perc_15min_early == 0.5 & perc_15min_late == 0.5)
```

    ## # A tibble: 0 x 12
    ## # ... with 12 variables: flight <int>, num_fl <int>, perc_on_time <dbl>,
    ## #   perc_early <dbl>, perc_late <dbl>, perc_10min_late <dbl>,
    ## #   perc_15min_early <dbl>, perc_15min_late <dbl>, perc_30min_early <dbl>,
    ## #   perc_30min_late <dbl>, perc_2hr_early <dbl>, perc_2hr_late <dbl>

A flight is always 10 minutes late.

``` r
filter(flights_delay_summary, perc_10min_late == 1)
```

    ## # A tibble: 4 x 12
    ##   flight num_fl perc_on_time perc_early perc_late perc_10min_late
    ##    <int>  <int>        <dbl>      <dbl>     <dbl>           <dbl>
    ## 1   2663      1            0          1         0               1
    ## 2   4098      1            0          1         0               1
    ## 3   5831      1            0          1         0               1
    ## 4   6103      1            0          1         0               1
    ## # ... with 6 more variables: perc_15min_early <dbl>,
    ## #   perc_15min_late <dbl>, perc_30min_early <dbl>, perc_30min_late <dbl>,
    ## #   perc_2hr_early <dbl>, perc_2hr_late <dbl>

A flight is 30 minutes early 50% of the time, and 30 minutes late 50% of the time.

``` r
filter(flights_delay_summary, perc_30min_early == 0.5 & perc_30min_late == 0.5)
```

    ## # A tibble: 0 x 12
    ## # ... with 12 variables: flight <int>, num_fl <int>, perc_on_time <dbl>,
    ## #   perc_early <dbl>, perc_late <dbl>, perc_10min_late <dbl>,
    ## #   perc_15min_early <dbl>, perc_15min_late <dbl>, perc_30min_early <dbl>,
    ## #   perc_30min_late <dbl>, perc_2hr_early <dbl>, perc_2hr_late <dbl>

99% of the time a flight is on time. 1% of the time it’s 2 hours late.

``` r
filter(flights_delay_summary, perc_on_time == 0.99 & perc_2hr_late == 0.01)
```

    ## # A tibble: 0 x 12
    ## # ... with 12 variables: flight <int>, num_fl <int>, perc_on_time <dbl>,
    ## #   perc_early <dbl>, perc_late <dbl>, perc_10min_late <dbl>,
    ## #   perc_15min_early <dbl>, perc_15min_late <dbl>, perc_30min_early <dbl>,
    ## #   perc_30min_late <dbl>, perc_2hr_early <dbl>, perc_2hr_late <dbl>

### 2. Come up with another approach that will give you the same output as not\_cancelled %&gt;% count(dest) and not\_cancelled %&gt;% count(tailnum, wt = distance) (without using count()).

``` r
not_cancelled %>% count(dest)
```

    ## # A tibble: 104 x 2
    ##     dest     n
    ##    <chr> <int>
    ##  1   ABQ   254
    ##  2   ACK   264
    ##  3   ALB   418
    ##  4   ANC     8
    ##  5   ATL 16837
    ##  6   AUS  2411
    ##  7   AVL   261
    ##  8   BDL   412
    ##  9   BGR   358
    ## 10   BHM   269
    ## # ... with 94 more rows

``` r
not_cancelled %>% count(tailnum, wt=distance)
```

    ## # A tibble: 4,037 x 2
    ##    tailnum      n
    ##      <chr>  <dbl>
    ##  1  D942DN   3418
    ##  2  N0EGMQ 239143
    ##  3  N10156 109664
    ##  4  N102UW  25722
    ##  5  N103US  24619
    ##  6  N104UW  24616
    ##  7  N10575 139903
    ##  8  N105UW  23618
    ##  9  N107US  21677
    ## 10  N108UW  32070
    ## # ... with 4,027 more rows

``` r
# another approach to the first line in the chunk
not_cancelled %>%
  group_by(dest) %>%
  summarise(n=n())
```

    ## # A tibble: 104 x 2
    ##     dest     n
    ##    <chr> <int>
    ##  1   ABQ   254
    ##  2   ACK   264
    ##  3   ALB   418
    ##  4   ANC     8
    ##  5   ATL 16837
    ##  6   AUS  2411
    ##  7   AVL   261
    ##  8   BDL   412
    ##  9   BGR   358
    ## 10   BHM   269
    ## # ... with 94 more rows

``` r
# another approach to the second line in the chunk
not_cancelled %>%
  group_by(tailnum) %>%
  summarise(n=sum(distance))
```

    ## # A tibble: 4,037 x 2
    ##    tailnum      n
    ##      <chr>  <dbl>
    ##  1  D942DN   3418
    ##  2  N0EGMQ 239143
    ##  3  N10156 109664
    ##  4  N102UW  25722
    ##  5  N103US  24619
    ##  6  N104UW  24616
    ##  7  N10575 139903
    ##  8  N105UW  23618
    ##  9  N107US  21677
    ## 10  N108UW  32070
    ## # ... with 4,027 more rows

### 3. Our definition of cancelled flights (is.na(dep\_delay) | is.na(arr\_delay) ) is slightly suboptimal. Why? Which is the most important column?

The most important one is dep\_delay since if the flight did not leave it was cancelled.

### 4. Look at the number of cancelled flights per day. Is there a pattern? Is the proportion of cancelled flights related to the average delay?

``` r
flights %>%
  group_by(day) %>%
  summarise(cancelled = mean(is.na(dep_delay)),
            mean_dep = mean(dep_delay, na.rm = T),
            mean_arr = mean(arr_delay, na.rm = T)) %>%
  ggplot(aes(y = cancelled)) +
  geom_point(aes(x = mean_dep), colour = "red") +
  geom_point(aes(x = mean_arr), colour = "blue") +
  labs(x = "Average delay per day", y = "Cancelled flights per day")
```

![](chapter5_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-24-1.png)

``` r
# There is a positive correlation
```

### 5. Which carrier has the worst delays? Challenge: can you disentangle the effects of bad airports vs. bad carriers? Why/why not? (Hint: think about flights %&gt;% group\_by(carrier, dest) %&gt;% summarise(n()))

``` r
flights %>%
  group_by(carrier) %>%
  summarise(dl_max=max(dep_delay, na.rm=T),
            arr_max=max(arr_delay, na.rm=T)) %>%
  arrange(desc(dl_max, arr_max))
```

    ## # A tibble: 16 x 3
    ##    carrier dl_max arr_max
    ##      <chr>  <dbl>   <dbl>
    ##  1      HA   1301    1272
    ##  2      MQ   1137    1127
    ##  3      AA   1014    1007
    ##  4      DL    960     931
    ##  5      F9    853     834
    ##  6      9E    747     744
    ##  7      VX    653     676
    ##  8      FL    602     572
    ##  9      EV    548     577
    ## 10      B6    502     497
    ## 11      US    500     492
    ## 12      UA    483     455
    ## 13      WN    471     453
    ## 14      YV    387     381
    ## 15      AS    225     198
    ## 16      OO    154     157

``` r
flights %>% 
  group_by(carrier, dest) %>%
  summarise(n())
```

    ## # A tibble: 314 x 3
    ## # Groups:   carrier [?]
    ##    carrier  dest `n()`
    ##      <chr> <chr> <int>
    ##  1      9E   ATL    59
    ##  2      9E   AUS     2
    ##  3      9E   AVL    10
    ##  4      9E   BGR     1
    ##  5      9E   BNA   474
    ##  6      9E   BOS   914
    ##  7      9E   BTV     2
    ##  8      9E   BUF   833
    ##  9      9E   BWI   856
    ## 10      9E   CAE     3
    ## # ... with 304 more rows

### 6. What does the sort argument to count(). When you might use it?

``` r
not_cancelled %>% count(dest, sort=T)
```

    ## # A tibble: 104 x 2
    ##     dest     n
    ##    <chr> <int>
    ##  1   ATL 16837
    ##  2   ORD 16566
    ##  3   LAX 16026
    ##  4   BOS 15022
    ##  5   MCO 13967
    ##  6   CLT 13674
    ##  7   SFO 13173
    ##  8   FLL 11897
    ##  9   MIA 11593
    ## 10   DCA  9111
    ## # ... with 94 more rows

The sort argument simply sorts in a descending order

5.7 Grouped mutates (and filters)
---------------------------------

### Finding the worst members of each group

``` r
flights_sml %>%
  group_by(year, month, day) %>%
  filter(rank(desc(arr_delay)) < 10)
```

    ## # A tibble: 3,306 x 7
    ## # Groups:   year, month, day [365]
    ##     year month   day dep_delay arr_delay distance air_time
    ##    <int> <int> <int>     <dbl>     <dbl>    <dbl>    <dbl>
    ##  1  2013     1     1       853       851      184       41
    ##  2  2013     1     1       290       338     1134      213
    ##  3  2013     1     1       260       263      266       46
    ##  4  2013     1     1       157       174      213       60
    ##  5  2013     1     1       216       222      708      121
    ##  6  2013     1     1       255       250      589      115
    ##  7  2013     1     1       285       246     1085      146
    ##  8  2013     1     1       192       191      199       44
    ##  9  2013     1     1       379       456     1092      222
    ## 10  2013     1     2       224       207      550       94
    ## # ... with 3,296 more rows

### Finding all groups bigger than a threshold

``` r
popular_dests <- flights %>%
  group_by(dest) %>%
  filter(n() > 365)
```

### Standartise to computer per group metrics

``` r
popular_dests %>%
  filter(arr_delay > 0) %>%
  mutate(prop_delay = arr_delay / sum(arr_delay)) %>%
  select(year:day, dest, arr_delay, prop_delay)
```

    ## # A tibble: 131,106 x 6
    ## # Groups:   dest [77]
    ##     year month   day  dest arr_delay   prop_delay
    ##    <int> <int> <int> <chr>     <dbl>        <dbl>
    ##  1  2013     1     1   IAH        11 1.106740e-04
    ##  2  2013     1     1   IAH        20 2.012255e-04
    ##  3  2013     1     1   MIA        33 2.350026e-04
    ##  4  2013     1     1   ORD        12 4.239594e-05
    ##  5  2013     1     1   FLL        19 9.377853e-05
    ##  6  2013     1     1   ORD         8 2.826396e-05
    ##  7  2013     1     1   LAX         7 3.444441e-05
    ##  8  2013     1     1   DFW        31 2.817951e-04
    ##  9  2013     1     1   ATL        12 3.996017e-05
    ## 10  2013     1     1   DTW        16 1.157257e-04
    ## # ... with 131,096 more rows

### 5.7.1 Exercises

#### 1. Refer back to the lists of useful mutate and filtering functions. Describe how each operation changes when you combine it with grouping.

#### 2. Which plane (tailnum) has the worst on-time record?

``` r
flights %>%
  group_by(tailnum) %>%
  summarise(on_time_rec = max(arr_delay)) %>%
  arrange(desc(on_time_rec))
```

    ## # A tibble: 4,044 x 2
    ##    tailnum on_time_rec
    ##      <chr>       <dbl>
    ##  1  N384HA        1272
    ##  2  N665MQ         989
    ##  3  N959DL         931
    ##  4  N927DA         915
    ##  5  N6716C         895
    ##  6  N5DMAA         878
    ##  7  N375NC         847
    ##  8  N203FR         834
    ##  9  N3GJAA         783
    ## 10  N324US         767
    ## # ... with 4,034 more rows

#### 3. What time of day should you fly if you want to avoid delays as much as possible?

``` r
flights %>%
  group_by(dep_time) %>%
  summarise(ar_del = min(arr_delay, na.rm=T),
            dep_del = min(dep_delay, nar.rm=T)) %>%
  arrange(desc(ar_del), desc(dep_del))
```

    ## Warning in min(arr_delay, na.rm = T): aucun argument trouvé pour min ; Inf
    ## est renvoyé

    ## Warning in min(arr_delay, na.rm = T): aucun argument trouvé pour min ; Inf
    ## est renvoyé

    ## # A tibble: 1,319 x 3
    ##    dep_time ar_del dep_del
    ##       <int>  <dbl>   <dbl>
    ##  1      251    Inf       1
    ##  2       NA    Inf      NA
    ##  3      353    493       1
    ##  4      226    420       1
    ##  5      241    385       1
    ##  6      229    373       1
    ##  7      133    356       1
    ##  8      245    355       1
    ##  9      325    354       1
    ## 10      210    331       1
    ## # ... with 1,309 more rows

#### 4. For each destination, compute the total minutes of delay. For each, flight, compute the proportion of the total delay for its destination.

###### Here we will use arr\_delay, dep\_delay and sum of arr\_delay and dep\_delay separately

``` r
flights %>%
  filter(arr_delay > 0, dep_delay > 0) %>%
  select(dest, flight, arr_delay, dep_delay) %>%
  group_by(dest) %>%
  mutate(total_delay_dest = sum(arr_delay) + sum(dep_delay)) %>%
  group_by(flight) %>%
  mutate(proportion = round(((arr_delay + dep_delay) / total_delay_dest)*100, 5)) %>%
  arrange(desc(proportion))
```

    ## # A tibble: 92,303 x 6
    ## # Groups:   flight [3,321]
    ##     dest flight arr_delay dep_delay total_delay_dest proportion
    ##    <chr>  <int>     <dbl>     <dbl>            <dbl>      <dbl>
    ##  1   PSP     55         1        10               11  100.00000
    ##  2   ANC    887        39        75              147   77.55102
    ##  3   MTJ    385       101       122              341   65.39589
    ##  4   SBN   5383        53        68              302   40.06623
    ##  5   SBN   5383        50        67              302   38.74172
    ##  6   BZN    568       154       165              825   38.66667
    ##  7   EYW   1873        31        40              223   31.83857
    ##  8   JAC   1506       175       198             1177   31.69074
    ##  9   HDN    355        32        46              265   29.43396
    ## 10   EYW   1873        45        13              223   26.00897
    ## # ... with 92,293 more rows

#### 5. Delays are typically temporally correlated: even once the problem that caused the initial delay has been resolved, later flights are delayed to allow earlier flights to leave. Using lag() explore how the delay of a flight is related to the delay of the immediately preceding flight.

``` r
flights %>%
  mutate(new_sched_dep_time = lubridate::make_datetime(year, month, day, hour, minute)) %>%
  arrange(new_sched_dep_time) %>%
  mutate(prev_time = lag(dep_delay)) %>%
  # filter(between(dep_delay, 0, 300), between(prev_time, 0, 300)) %>% # play with this one
  select(origin, new_sched_dep_time, dep_delay, prev_time) %>%
  ggplot(aes(dep_delay, prev_time)) + geom_point(alpha = 1/10) +
  geom_smooth()
```

    ## `geom_smooth()` using method = 'gam'

    ## Warning: Removed 14167 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 14167 rows containing missing values (geom_point).

![](chapter5_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-33-1.png)

#### 6. Look at each destination. Can you find flights that are suspiciously fast? (i.e. flights that represent a potential data entry error). Compute the air time a flight relative to the shortest flight to that destination. Which flights were most delayed in the air?

``` r
flights %>%
  filter(!is.na(air_time)) %>%
  select(dest, flight, air_time) %>%
  group_by(dest) %>%
  mutate(shortest = min(air_time, na.rm = T)) %>%
  group_by(flight) %>%
  mutate(ratio = air_time / shortest) %>%
  arrange(desc(ratio))
```

    ## # A tibble: 327,346 x 5
    ## # Groups:   flight [3,835]
    ##     dest flight air_time shortest    ratio
    ##    <chr>  <int>    <dbl>    <dbl>    <dbl>
    ##  1   BOS   1703      112       21 5.333333
    ##  2   BOS   2136      107       21 5.095238
    ##  3   BOS   2480       99       21 4.714286
    ##  4   BOS   1750       96       21 4.571429
    ##  5   BOS   1248       92       21 4.380952
    ##  6   BOS   1006       91       21 4.333333
    ##  7   BOS   3422       86       21 4.095238
    ##  8   BOS   1850       86       21 4.095238
    ##  9   DCA   2175      131       32 4.093750
    ## 10   ACK   1491      141       35 4.028571
    ## # ... with 327,336 more rows

``` r
# (1)
flights %>%
  group_by(dest) %>%
  arrange(air_time) %>%
  slice(1:5) %>%
  select(tailnum, sched_dep_time, sched_arr_time, air_time) %>%
  arrange(air_time)
```

    ## Adding missing grouping variables: `dest`

    ## # A tibble: 517 x 5
    ## # Groups:   dest [105]
    ##     dest tailnum sched_dep_time sched_arr_time air_time
    ##    <chr>   <chr>          <int>          <int>    <dbl>
    ##  1   BDL  N16911           1315           1411       20
    ##  2   BDL  N12167            527            628       20
    ##  3   BDL  N27200            851            954       21
    ##  4   BDL  N13955           1315           1411       21
    ##  5   BDL  N12160           1329           1426       21
    ##  6   BOS  N947UW           1500           1608       21
    ##  7   PHL  N13913           2129           2224       21
    ##  8   PHL  N12921           2130           2225       21
    ##  9   PHL  N8501F           1935           2056       21
    ## 10   PHL  N22909           2129           2224       22
    ## # ... with 507 more rows

``` r
flights %>%
  filter(!is.na(air_time)) %>%
  group_by(dest) %>%
  mutate(shortest = air_time - min(air_time, na.rm = T)) %>%
  top_n(1, air_time) %>%
  arrange(-air_time) %>%
  select(tailnum, sched_dep_time, sched_arr_time, shortest)
```

    ## Adding missing grouping variables: `dest`

    ## # A tibble: 112 x 5
    ## # Groups:   dest [104]
    ##     dest tailnum sched_dep_time sched_arr_time shortest
    ##    <chr>   <chr>          <int>          <int>    <dbl>
    ##  1   HNL  N77066           1335           1836      133
    ##  2   SFO  N703TW           1730           2110      195
    ##  3   LAX  N178DN           1815           2146      165
    ##  4   ANC  N572UA           1615           1953       46
    ##  5   SAN  N794JB           1620           1934      134
    ##  6   SNA  N16709           1819           2137      131
    ##  7   BUR  N624JB           1730           2046      110
    ##  8   LAS  N852UA           1729           2013      143
    ##  9   SJC  N632JB           1830           2205       91
    ## 10   SEA  N17245           1727           2040      119
    ## # ... with 102 more rows

#### 7. Find all destinations that are flown by at least two carriers. Use that information to rank the carriers.

``` r
flights %>%
  select(dest, carrier) %>%
  group_by(dest) %>%
  filter(n_distinct(carrier) > 2) %>%
  group_by(carrier) %>%
  summarise(n = n_distinct(dest)) %>%
  arrange(-n)
```

    ## # A tibble: 15 x 2
    ##    carrier     n
    ##      <chr> <int>
    ##  1      DL    37
    ##  2      EV    36
    ##  3      UA    36
    ##  4      9E    35
    ##  5      B6    30
    ##  6      AA    17
    ##  7      MQ    17
    ##  8      WN     9
    ##  9      OO     5
    ## 10      US     5
    ## 11      VX     3
    ## 12      YV     3
    ## 13      FL     2
    ## 14      AS     1
    ## 15      F9     1

#### 8. For each plane, count the number of flights before the first delay of greater than 1 hour.

``` r
flights %>%
  filter(!is.na(arr_delay), arr_delay > 0, arr_delay < 60) %>%
  select(flight, tailnum, arr_delay) %>%
  group_by(tailnum) %>%
  summarise(count = n()) %>%
  arrange(desc(count))
```

    ## # A tibble: 3,815 x 2
    ##    tailnum count
    ##      <chr> <int>
    ##  1  N725MQ   177
    ##  2  N228JB   146
    ##  3  N534MQ   146
    ##  4  N711MQ   146
    ##  5  N258JB   145
    ##  6  N542MQ   143
    ##  7  N6EAMQ   143
    ##  8  N713MQ   143
    ##  9  N723MQ   138
    ## 10  N523MQ   137
    ## # ... with 3,805 more rows
