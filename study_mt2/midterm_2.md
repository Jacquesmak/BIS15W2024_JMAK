---
title: "BIS 15L Midterm 2"
output:
  html_document: 
    theme: spacelab
    keep_md: true
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean! Use the tidyverse and pipes unless otherwise indicated. To receive full credit, all plots must have clearly labeled axes, a title, and consistent aesthetics. This exam is worth a total of 35 points. 

Please load the following libraries.

```r
library("tidyverse")
library("janitor")
library("naniar")
```

## Data
These data are from a study on surgical residents. The study was originally published by Sessier et al. “Operation Timing and 30-Day Mortality After Elective General Surgery”. Anesth Analg 2011; 113: 1423-8. The data were cleaned for instructional use by Amy S. Nowacki, “Surgery Timing Dataset”, TSHS Resources Portal (2016). Available at https://www.causeweb.org/tshs/surgery-timing/.

Descriptions of the variables and the study are included as pdf's in the data folder.  

Please run the following chunk to import the data.

```r
surgery <- read_csv("data/surgery.csv")
```

1. Use the summary function(s) of your choice to explore the data and get an idea of its structure. Please also check for NA's.


```r
glimpse(surgery)
```

```
## Rows: 32,001
## Columns: 25
## $ ahrq_ccs            <chr> "<Other>", "<Other>", "<Other>", "<Other>", "<Othe…
## $ age                 <dbl> 67.8, 39.5, 56.5, 71.0, 56.3, 57.7, 56.6, 64.2, 66…
## $ gender              <chr> "M", "F", "F", "M", "M", "F", "M", "F", "M", "F", …
## $ race                <chr> "Caucasian", "Caucasian", "Caucasian", "Caucasian"…
## $ asa_status          <chr> "I-II", "I-II", "I-II", "III", "I-II", "I-II", "IV…
## $ bmi                 <dbl> 28.04, 37.85, 19.56, 32.22, 24.32, 40.30, 64.57, 4…
## $ baseline_cancer     <chr> "No", "No", "No", "No", "Yes", "No", "No", "No", "…
## $ baseline_cvd        <chr> "Yes", "Yes", "No", "Yes", "No", "Yes", "Yes", "Ye…
## $ baseline_dementia   <chr> "No", "No", "No", "No", "No", "No", "No", "No", "N…
## $ baseline_diabetes   <chr> "No", "No", "No", "No", "No", "No", "Yes", "No", "…
## $ baseline_digestive  <chr> "Yes", "No", "No", "No", "No", "No", "No", "No", "…
## $ baseline_osteoart   <chr> "No", "No", "No", "No", "No", "No", "No", "No", "N…
## $ baseline_psych      <chr> "No", "No", "No", "No", "No", "Yes", "No", "No", "…
## $ baseline_pulmonary  <chr> "No", "No", "No", "No", "No", "No", "No", "No", "N…
## $ baseline_charlson   <dbl> 0, 0, 0, 0, 0, 0, 2, 0, 1, 2, 0, 1, 0, 0, 0, 0, 0,…
## $ mortality_rsi       <dbl> -0.63, -0.63, -0.49, -1.38, 0.00, -0.77, -0.36, -0…
## $ complication_rsi    <dbl> -0.26, -0.26, 0.00, -1.15, 0.00, -0.84, -1.34, 0.0…
## $ ccsmort30rate       <dbl> 0.0042508, 0.0042508, 0.0042508, 0.0042508, 0.0042…
## $ ccscomplicationrate <dbl> 0.07226355, 0.07226355, 0.07226355, 0.07226355, 0.…
## $ hour                <dbl> 9.03, 18.48, 7.88, 8.80, 12.20, 7.67, 9.53, 7.52, …
## $ dow                 <chr> "Mon", "Wed", "Fri", "Wed", "Thu", "Thu", "Tue", "…
## $ month               <chr> "Nov", "Sep", "Aug", "Jun", "Aug", "Dec", "Apr", "…
## $ moonphase           <chr> "Full Moon", "New Moon", "Full Moon", "Last Quarte…
## $ mort30              <chr> "No", "No", "No", "No", "No", "No", "No", "No", "N…
## $ complication        <chr> "No", "No", "No", "No", "No", "No", "No", "Yes", "…
```


```r
head(surgery)
```

```
## # A tibble: 6 × 25
##   ahrq_ccs   age gender race       asa_status   bmi baseline_cancer baseline_cvd
##   <chr>    <dbl> <chr>  <chr>      <chr>      <dbl> <chr>           <chr>       
## 1 <Other>   67.8 M      Caucasian  I-II        28.0 No              Yes         
## 2 <Other>   39.5 F      Caucasian  I-II        37.8 No              Yes         
## 3 <Other>   56.5 F      Caucasian  I-II        19.6 No              No          
## 4 <Other>   71   M      Caucasian  III         32.2 No              Yes         
## 5 <Other>   56.3 M      African A… I-II        24.3 Yes             No          
## 6 <Other>   57.7 F      Caucasian  I-II        40.3 No              Yes         
## # ℹ 17 more variables: baseline_dementia <chr>, baseline_diabetes <chr>,
## #   baseline_digestive <chr>, baseline_osteoart <chr>, baseline_psych <chr>,
## #   baseline_pulmonary <chr>, baseline_charlson <dbl>, mortality_rsi <dbl>,
## #   complication_rsi <dbl>, ccsmort30rate <dbl>, ccscomplicationrate <dbl>,
## #   hour <dbl>, dow <chr>, month <chr>, moonphase <chr>, mort30 <chr>,
## #   complication <chr>
```


```r
miss_var_summary(surgery)
```

```
## # A tibble: 25 × 3
##    variable          n_miss pct_miss
##    <chr>              <int>    <dbl>
##  1 bmi                 3290 10.3    
##  2 race                 480  1.50   
##  3 asa_status             8  0.0250 
##  4 gender                 3  0.00937
##  5 age                    2  0.00625
##  6 ahrq_ccs               0  0      
##  7 baseline_cancer        0  0      
##  8 baseline_cvd           0  0      
##  9 baseline_dementia      0  0      
## 10 baseline_diabetes      0  0      
## # ℹ 15 more rows
```

2. Let's explore the participants in the study. Show a count of participants by race AND make a plot that visually represents your output.


```r
surgery %>% 
  count(race)
```

```
## # A tibble: 4 × 2
##   race                 n
##   <chr>            <int>
## 1 African American  3790
## 2 Caucasian        26488
## 3 Other             1243
## 4 <NA>               480
```


```r
surgery %>% 
  count(race) %>% 
  ggplot(aes(x=race, y=n, fill=race)) +
  geom_col(color="black", alpha=0.5) +
  labs(title="Race of Participants",
       x=NULL,
       fill="Race") +
  theme(plot.title = element_text(size=12, face="bold"),
        legend.position = "bottom")
```

![](midterm_2_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

3. What is the mean age of participants by gender? (hint: please provide a number for each) Since only three participants do not have gender indicated, remove these participants from the data.


```r
surgery %>% 
  filter(gender!="NA") %>% 
  group_by(gender) %>% 
  summarize(mean_age=mean(age, na.rm=T))
```

```
## # A tibble: 2 × 2
##   gender mean_age
##   <chr>     <dbl>
## 1 F          56.7
## 2 M          58.8
```

4. Make a plot that shows the range of age associated with gender.


```r
surgery %>% 
  filter(gender!="NA") %>% 
  ggplot(aes(x=gender, y=age, fill=gender)) +
  geom_boxplot(na.rm=T, alpha=0.5) +
  labs(title="Range of Age by Gender",
       x=NULL,
       y="Age",
       fill="Gender") +
  theme(plot.title = element_text(size=12, face="bold"),
        axis.title.x = element_text(size=10),
        axis.title.y = element_text(size=10))
```

![](midterm_2_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

5. How healthy are the participants? The variable `asa_status` is an evaluation of patient physical status prior to surgery. Lower numbers indicate fewer comorbidities (presence of two or more diseases or medical conditions in a patient). Make a plot that compares the number of `asa_status` I-II, III, and IV-V.


```r
surgery %>% 
  count(asa_status)
```

```
## # A tibble: 4 × 2
##   asa_status     n
##   <chr>      <int>
## 1 I-II       17261
## 2 III        13677
## 3 IV-VI       1055
## 4 <NA>           8
```


```r
surgery %>%
  filter(asa_status!="NA") %>% 
  ggplot(aes(x=asa_status, fill=asa_status)) +
  geom_bar(alpha=0.5, color="black") +
  labs(title="Number of ASA Status",
       x=NULL,
       y="# of Patients",
       fill="ASA Status") +
  theme(plot.title = element_text(size=12, face="bold"),
        axis.title.x = element_text(size=10),
        axis.title.y = element_text(size=10))
```

![](midterm_2_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

6. Create a plot that displays the distribution of body mass index for each `asa_status` as a probability distribution- not a histogram. (hint: use faceting!)


```r
surgery %>% 
  filter(asa_status!="NA" & bmi!="NA") %>% 
  ggplot(aes(x=bmi)) +
  geom_density(fill="cornflowerblue", alpha=0.5) +
  facet_wrap(~asa_status) +
  labs(title="Distribution of BMI for ASA Status",
       x="BMI",
       y="Density") +
  theme(plot.title = element_text(size=12, face="bold"),
        axis.title.x = element_text(size=10),
        axis.title.y = element_text(size=10))
```

![](midterm_2_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

The variable `ccsmort30rate` is a measure of the overall 30-day mortality rate associated with each type of operation. The variable `ccscomplicationrate` is a measure of the 30-day in-hospital complication rate. The variable `ahrq_ccs` lists each type of operation.  

7. What are the 5 procedures associated with highest risk of 30-day mortality AND how do they compare with the 5 procedures with highest risk of complication? (hint: no need for a plot here)


```r
surgery %>% 
  select(ahrq_ccs, ccsmort30rate) %>% 
  group_by(ahrq_ccs) %>% 
  summarize(avg_30mort=mean(ccsmort30rate)) %>% 
  top_n(5)
```

```
## Selecting by avg_30mort
```

```
## # A tibble: 5 × 2
##   ahrq_ccs                                             avg_30mort
##   <chr>                                                     <dbl>
## 1 Colorectal resection                                    0.0167 
## 2 Endoscopy and endoscopic biopsy of the urinary tract    0.00811
## 3 Gastrectomy; partial and total                          0.0127 
## 4 Small bowel resection                                   0.0129 
## 5 Spinal fusion                                           0.00742
```


```r
surgery %>% 
  select(ahrq_ccs, ccscomplicationrate) %>% 
  group_by(ahrq_ccs) %>% 
  summarize(avg_comp=mean(ccscomplicationrate)) %>% 
  top_n(5)
```

```
## Selecting by avg_comp
```

```
## # A tibble: 5 × 2
##   ahrq_ccs                         avg_comp
##   <chr>                               <dbl>
## 1 Colorectal resection                0.312
## 2 Gastrectomy; partial and total      0.190
## 3 Nephrectomy; partial or complete    0.197
## 4 Small bowel resection               0.466
## 5 Spinal fusion                       0.183
```

8. Make a plot that compares the `ccsmort30rate` for all listed `ahrq_ccs` procedures.


```r
surgery %>% 
  select(ahrq_ccs, ccsmort30rate) %>% 
  group_by(ahrq_ccs) %>% 
  summarize(avg_30mort=mean(ccsmort30rate)) %>% 
  ggplot(aes(x=ahrq_ccs, y=avg_30mort)) +
  geom_col(color="black", fill="cornflowerblue", alpha=0.5) +
  coord_flip() +
  labs(title="30 Mortality Rate by Operation",
       x="Operation",
       y="30 Mort. Rate") +
  theme(plot.title = element_text(size=12, face="bold"),
        axis.title.x = element_text(size=10),
        axis.title.y = element_text(size=10))
```

![](midterm_2_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

9. When is the best month to have surgery? Make a chart that shows the 30-day mortality and complications for the patients by month. `mort30` is the variable that shows whether or not a patient survived 30 days post-operation.


```r
surgery %>% 
  mutate(mort30_n=ifelse(mort30 == "Yes", 1, 0), 
         comp30_n=ifelse(complication == "Yes", 1, 0)) %>% 
  group_by(month) %>% 
  summarize(n_mort=sum(mort30_n),
            n_comp=sum(comp30_n))
```

```
## # A tibble: 12 × 3
##    month n_mort n_comp
##    <chr>  <dbl>  <dbl>
##  1 Apr       12    321
##  2 Aug        9    462
##  3 Dec        4    237
##  4 Feb       17    343
##  5 Jan       19    407
##  6 Jul       12    301
##  7 Jun       14    410
##  8 Mar       12    324
##  9 May       10    333
## 10 Nov        5    325
## 11 Oct        8    377
## 12 Sep       16    424
```

10. Make a plot that visualizes the chart from question #9. Make sure that the months are on the x-axis. Do a search online and figure out how to order the months Jan-Dec.

ggplot(aes(x = factor(month, month.abb)))

```r
surgery %>% 
  mutate(mort30_n=ifelse(mort30 == "Yes", 1, 0), 
         comp30_n=ifelse(complication == "Yes", 1, 0)) %>% 
  group_by(month) %>% 
  summarize(n_mort=sum(mort30_n),
            n_comp=sum(comp30_n)) %>% 
  ggplot(aes(x=factor(month, month.abb), y=n_mort)) +
  geom_col(color="black", fill="cornflowerblue", alpha=0.5) +
   labs(title="Mortality by Month",
       x="Month",
       y="30-Day Mortality") +
  theme(plot.title = element_text(size=12, face="bold"),
        axis.title.x = element_text(size=10),
        axis.title.y = element_text(size=10))
```

![](midterm_2_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

Please be 100% sure your exam is saved, knitted, and pushed to your github repository. No need to submit a link on canvas, we will find your exam in your repository.
