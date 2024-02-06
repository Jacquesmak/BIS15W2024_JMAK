---
title: "Lab 7 Homework"
author: "Your Name Here"
date: "2024-02-06"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(skimr)
```

For this assignment we are going to work with a large data set from the [United Nations Food and Agriculture Organization](http://www.fao.org/about/en/) on world fisheries. These data are pretty wild, so we need to do some cleaning. First, load the data.  

Load the data `FAO_1950to2012_111914.csv` as a new object titled `fisheries`.

```r
fisheries <- readr::read_csv(file = "data/FAO_1950to2012_111914.csv")
```

```
## Rows: 17692 Columns: 71
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (69): Country, Common name, ISSCAAP taxonomic group, ASFIS species#, ASF...
## dbl  (2): ISSCAAP group#, FAO major fishing area
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

1. Do an exploratory analysis of the data (your choice). What are the names of the variables, what are the dimensions, are there any NA's, what are the classes of the variables?  


```r
anyNA(fisheries)
```

```
## [1] TRUE
```


```r
names(fisheries)
```

```
##  [1] "Country"                 "Common name"            
##  [3] "ISSCAAP group#"          "ISSCAAP taxonomic group"
##  [5] "ASFIS species#"          "ASFIS species name"     
##  [7] "FAO major fishing area"  "Measure"                
##  [9] "1950"                    "1951"                   
## [11] "1952"                    "1953"                   
## [13] "1954"                    "1955"                   
## [15] "1956"                    "1957"                   
## [17] "1958"                    "1959"                   
## [19] "1960"                    "1961"                   
## [21] "1962"                    "1963"                   
## [23] "1964"                    "1965"                   
## [25] "1966"                    "1967"                   
## [27] "1968"                    "1969"                   
## [29] "1970"                    "1971"                   
## [31] "1972"                    "1973"                   
## [33] "1974"                    "1975"                   
## [35] "1976"                    "1977"                   
## [37] "1978"                    "1979"                   
## [39] "1980"                    "1981"                   
## [41] "1982"                    "1983"                   
## [43] "1984"                    "1985"                   
## [45] "1986"                    "1987"                   
## [47] "1988"                    "1989"                   
## [49] "1990"                    "1991"                   
## [51] "1992"                    "1993"                   
## [53] "1994"                    "1995"                   
## [55] "1996"                    "1997"                   
## [57] "1998"                    "1999"                   
## [59] "2000"                    "2001"                   
## [61] "2002"                    "2003"                   
## [63] "2004"                    "2005"                   
## [65] "2006"                    "2007"                   
## [67] "2008"                    "2009"                   
## [69] "2010"                    "2011"                   
## [71] "2012"
```

```r
dim(fisheries)
```

```
## [1] 17692    71
```


```r
summary(fisheries)
```

```
##    Country          Common name        ISSCAAP group#  ISSCAAP taxonomic group
##  Length:17692       Length:17692       Min.   :11.00   Length:17692           
##  Class :character   Class :character   1st Qu.:33.00   Class :character       
##  Mode  :character   Mode  :character   Median :36.00   Mode  :character       
##                                        Mean   :37.38                          
##                                        3rd Qu.:38.00                          
##                                        Max.   :77.00                          
##  ASFIS species#     ASFIS species name FAO major fishing area
##  Length:17692       Length:17692       Min.   :18.00         
##  Class :character   Class :character   1st Qu.:31.00         
##  Mode  :character   Mode  :character   Median :37.00         
##                                        Mean   :45.34         
##                                        3rd Qu.:57.00         
##                                        Max.   :88.00         
##    Measure              1950               1951               1952          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1953               1954               1955               1956          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1957               1958               1959               1960          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1961               1962               1963               1964          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1965               1966               1967               1968          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1969               1970               1971               1972          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1973               1974               1975               1976          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1977               1978               1979               1980          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1981               1982               1983               1984          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1985               1986               1987               1988          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1989               1990               1991               1992          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1993               1994               1995               1996          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1997               1998               1999               2000          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      2001               2002               2003               2004          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      2005               2006               2007               2008          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      2009               2010               2011               2012          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
## 
```


2. Use `janitor` to rename the columns and make them easier to use. As part of this cleaning step, change `country`, `isscaap_group_number`, `asfis_species_number`, and `fao_major_fishing_area` to data class factor. 

```r
fisheries <- clean_names(fisheries)
fisheries$country <- as.factor(fisheries$country)
fisheries$isscaap_group_number <- as.factor(fisheries$isscaap_group_number)
fisheries$asfis_species_number <- as.factor(fisheries$asfis_species_number)
fisheries$fao_major_fishing_area <- as.factor(fisheries$fao_major_fishing_area)
```

We need to deal with the years because they are being treated as characters and start with an X. We also have the problem that the column names that are years actually represent data. We haven't discussed tidy data yet, so here is some help. You should run this ugly chunk to transform the data for the rest of the homework. It will only work if you have used janitor to rename the variables in question 2!  

```r
fisheries_tidy <- fisheries %>% 
  pivot_longer(-c(country,common_name,isscaap_group_number,isscaap_taxonomic_group,asfis_species_number,asfis_species_name,fao_major_fishing_area,measure),
               names_to = "year",
               values_to = "catch",
               values_drop_na = TRUE) %>% 
  mutate(year= as.numeric(str_replace(year, 'x', ''))) %>% 
  mutate(catch= str_replace(catch, c(' F'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('...'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('-'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('0 0'), replacement = ''))

fisheries_tidy$catch <- as.numeric(fisheries_tidy$catch)
fisheries_tidy
```

```
## # A tibble: 376,771 × 10
##    country common_name               isscaap_group_number isscaap_taxonomic_gr…¹
##    <fct>   <chr>                     <fct>                <chr>                 
##  1 Albania Angelsharks, sand devils… 38                   Sharks, rays, chimaer…
##  2 Albania Angelsharks, sand devils… 38                   Sharks, rays, chimaer…
##  3 Albania Angelsharks, sand devils… 38                   Sharks, rays, chimaer…
##  4 Albania Angelsharks, sand devils… 38                   Sharks, rays, chimaer…
##  5 Albania Angelsharks, sand devils… 38                   Sharks, rays, chimaer…
##  6 Albania Angelsharks, sand devils… 38                   Sharks, rays, chimaer…
##  7 Albania Angelsharks, sand devils… 38                   Sharks, rays, chimaer…
##  8 Albania Angelsharks, sand devils… 38                   Sharks, rays, chimaer…
##  9 Albania Angelsharks, sand devils… 38                   Sharks, rays, chimaer…
## 10 Albania Angelsharks, sand devils… 38                   Sharks, rays, chimaer…
## # ℹ 376,761 more rows
## # ℹ abbreviated name: ¹​isscaap_taxonomic_group
## # ℹ 6 more variables: asfis_species_number <fct>, asfis_species_name <chr>,
## #   fao_major_fishing_area <fct>, measure <chr>, year <dbl>, catch <dbl>
```

3. How many countries are represented in the data? Provide a count and list their names.

```r
fisheries %>%
  summarize(distinct_countries = n_distinct(country))
```

```
## # A tibble: 1 × 1
##   distinct_countries
##                <int>
## 1                204
```
204 countries
4. Refocus the data only to include country, isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch.

```r
focus_data <- fisheries_tidy %>% 
  select(country,isscaap_taxonomic_group,asfis_species_name,asfis_species_number,year,catch)
focus_data
```

```
## # A tibble: 376,771 × 6
##    country isscaap_taxonomic_group asfis_species_name asfis_species_number  year
##    <fct>   <chr>                   <chr>              <fct>                <dbl>
##  1 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX            1995
##  2 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX            1996
##  3 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX            1997
##  4 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX            1998
##  5 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX            1999
##  6 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX            2000
##  7 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX            2001
##  8 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX            2002
##  9 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX            2003
## 10 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX            2004
## # ℹ 376,761 more rows
## # ℹ 1 more variable: catch <dbl>
```

5. Based on the asfis_species_number, how many distinct fish species were caught as part of these data?

```r
fisheries %>%
  summarize(distinct_fish = n_distinct(asfis_species_number))
```

```
## # A tibble: 1 × 1
##   distinct_fish
##           <int>
## 1          1553
```

6. Which country had the largest overall catch in the year 2000?

```r
fisheries_tidy%>%
  filter(year==2000)%>%
  group_by(country)%>%
  summarize(overall_catch_2000=sum(catch, na.rm=T))%>%
  arrange(desc(overall_catch_2000))
```

```
## # A tibble: 193 × 2
##    country                  overall_catch_2000
##    <fct>                                 <dbl>
##  1 China                                 25899
##  2 Russian Federation                    12181
##  3 United States of America              11762
##  4 Japan                                  8510
##  5 Indonesia                              8341
##  6 Peru                                   7443
##  7 Chile                                  6906
##  8 India                                  6351
##  9 Thailand                               6243
## 10 Korea, Republic of                     6124
## # ℹ 183 more rows
```

7. Which country caught the most sardines (_Sardina pilchardus_) between the years 1990-2000?

```r
fisheries_tidy %>%
  filter( asfis_species_name == "Sardina pilchardus") %>%
  filter( year %in% c(1990,2000) ) %>%
  group_by(country) %>%
  summarize(sardines_catch_90_00=sum(catch, na.rm = T)) %>%
  arrange(desc(sardines_catch_90_00))
```

```
## # A tibble: 32 × 2
##    country            sardines_catch_90_00
##    <fct>                             <dbl>
##  1 Morocco                            1733
##  2 Spain                               737
##  3 Ukraine                             351
##  4 Portugal                            257
##  5 Russian Federation                  203
##  6 France                              165
##  7 Greece                               79
##  8 Algeria                              77
##  9 Yugoslavia SFR                       69
## 10 Lithuania                            64
## # ℹ 22 more rows
```

8. Which five countries caught the most cephalopods between 2008-2012?

```r
fisheries_tidy %>%
  filter( isscaap_taxonomic_group == "Squids, cuttlefishes, octopuses") %>%
  filter( year %in% c(2008,2012) ) %>%
  group_by(country) %>%
  summarize(cephalopods_catch_2008_2012=sum(catch, na.rm = T)) %>%
  arrange(desc(cephalopods_catch_2008_2012))
```

```
## # A tibble: 121 × 2
##    country                  cephalopods_catch_2008_2012
##    <fct>                                          <dbl>
##  1 China                                           3616
##  2 Korea, Republic of                              1832
##  3 Chile                                           1635
##  4 Indonesia                                       1180
##  5 Peru                                             971
##  6 Taiwan Province of China                         963
##  7 Japan                                            864
##  8 Argentina                                        618
##  9 France                                           593
## 10 Spain                                            576
## # ℹ 111 more rows
```
 Country:32, 97, 31, 84 and 140
9. Which species had the highest catch total between 2008-2012? (hint: Osteichthyes is not a species)

```r
fisheries_tidy %>%
  filter( year %in% c(2008,2012) ) %>%
  filter( asfis_species_name != "Osteichthyes" ) %>%
  group_by(asfis_species_name)%>%
summarize(total_catch_08_12=sum(catch, na.rm = T)) %>%
  arrange(desc(total_catch_08_12))
```

```
## # A tibble: 1,438 × 2
##    asfis_species_name    total_catch_08_12
##    <chr>                             <dbl>
##  1 Engraulis ringens                 22550
##  2 Theragra chalcogramma             20461
##  3 Katsuwonus pelamis                13846
##  4 Clupea harengus                   13647
##  5 Trichiurus lepturus               10745
##  6 Thunnus albacares                  7786
##  7 Scomber japonicus                  5677
##  8 Natantia                           5024
##  9 Gadus morhua                       4869
## 10 Thunnus alalunga                   4798
## # ℹ 1,428 more rows
```

10. Use the data to do at least one analysis of your choice.
which 5 fish has the most catch in country four.


```r
fisheries_tidy %>%
  filter(country == "4") %>%
  filter( asfis_species_name != "Osteichthyes" ) %>%
  group_by(asfis_species_name) %>%
  summarize(country_catch=sum(catch, na.rm = T)) %>%
  arrange(desc(country_catch))
```

```
## # A tibble: 0 × 2
## # ℹ 2 variables: asfis_species_name <chr>, country_catch <dbl>
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
