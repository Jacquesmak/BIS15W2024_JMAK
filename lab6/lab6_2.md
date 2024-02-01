---
title: "dplyr Superhero"
date: "2024-02-01"
output:
  html_document: 
    theme: spacelab
    toc: true
    toc_float: true
    keep_md: true
---

## Learning Goals  
*At the end of this exercise, you will be able to:*    
1. Develop your dplyr superpowers so you can easily and confidently manipulate dataframes.  
2. Learn helpful new functions that are part of the `janitor` package.  

## Instructions
For the second part of lab today, we are going to spend time practicing the dplyr functions we have learned and add a few new ones. This lab doubles as your homework. Please complete the lab and push your final code to GitHub.  

## Load the libraries

```r
library("tidyverse")
```

```
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.4     ✔ readr     2.1.5
## ✔ forcats   1.0.0     ✔ stringr   1.5.1
## ✔ ggplot2   3.4.4     ✔ tibble    3.2.1
## ✔ lubridate 1.9.3     ✔ tidyr     1.3.0
## ✔ purrr     1.0.2     
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```

```r
library("janitor")
```

```
## 
## Attaching package: 'janitor'
## 
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

```r
library(dplyr)
```

## Load the superhero data
These are data taken from comic books and assembled by fans. The include a good mix of categorical and continuous data.  Data taken from: https://www.kaggle.com/claudiodavi/superhero-set  

Check out the way I am loading these data. If I know there are NAs, I can take care of them at the beginning. But, we should do this very cautiously. At times it is better to keep the original columns and data intact.  

```r
superhero_info <- read_csv("data/heroes_information.csv", na = c("", "-99", "-"))
```

```
## Rows: 734 Columns: 10
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (8): name, Gender, Eye color, Race, Hair color, Publisher, Skin color, A...
## dbl (2): Height, Weight
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
superhero_powers <- read_csv("data/super_hero_powers.csv", na = c("", "-99", "-"))
```

```
## Rows: 667 Columns: 168
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr   (1): hero_names
## lgl (167): Agility, Accelerated Healing, Lantern Power Ring, Dimensional Awa...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

## Data tidy
1. Some of the names used in the `superhero_info` data are problematic so you should rename them here. Before you do anything, first have a look at the names of the variables. You can use `rename()` or `clean_names()`.    


```r
superhero_info <- read_csv("data/heroes_information.csv") %>% clean_names()
```

```
## Rows: 734 Columns: 10
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (8): name, Gender, Eye color, Race, Hair color, Publisher, Skin color, A...
## dbl (2): Height, Weight
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
superhero_info
```

```
## # A tibble: 734 × 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 A-Bo… Male   yellow    Human No Hair       203 Marvel C… -          good     
##  2 Abe … Male   blue      Icth… No Hair       191 Dark Hor… blue       good     
##  3 Abin… Male   blue      Unga… No Hair       185 DC Comics red        good     
##  4 Abom… Male   green     Huma… No Hair       203 Marvel C… -          bad      
##  5 Abra… Male   blue      Cosm… Black         -99 Marvel C… -          bad      
##  6 Abso… Male   blue      Human No Hair       193 Marvel C… -          bad      
##  7 Adam… Male   blue      -     Blond         -99 NBC - He… -          good     
##  8 Adam… Male   blue      Human Blond         185 DC Comics -          good     
##  9 Agen… Female blue      -     Blond         173 Marvel C… -          good     
## 10 Agen… Male   brown     Human Brown         178 Marvel C… -          good     
## # ℹ 724 more rows
## # ℹ 1 more variable: weight <dbl>
```


## `tabyl`
The `janitor` package has many awesome functions that we will explore. Here is its version of `table` which not only produces counts but also percentages. Very handy! Let's use it to explore the proportion of good guys and bad guys in the `superhero_info` data.  

```r
tabyl(superhero_info, alignment)
```

```
##  alignment   n     percent
##          -   7 0.009536785
##        bad 207 0.282016349
##       good 496 0.675749319
##    neutral  24 0.032697548
```

1. Who are the publishers of the superheros? Show the proportion of superheros from each publisher. Which publisher has the highest number of superheros?  


```r
table(superhero_info$publisher)
```

```
## 
##       ABC Studios Dark Horse Comics         DC Comics      George Lucas 
##                 4                18               215                14 
##     Hanna-Barbera     HarperCollins       Icon Comics    IDW Publishing 
##                 1                 6                 4                 4 
##      Image Comics     J. K. Rowling  J. R. R. Tolkien     Marvel Comics 
##                14                 1                 1               388 
##         Microsoft      NBC - Heroes         Rebellion          Shueisha 
##                 1                19                 1                 4 
##     Sony Pictures        South Park         Star Trek              SyFy 
##                 2                 1                 6                 5 
##      Team Epic TV       Titan Books Universal Studios         Wildstorm 
##                 5                 1                 1                 3
```



2. Notice that we have some neutral superheros! Who are they? List their names below.  


```r
superhero_info %>%
  filter(alignment == "neutral") %>% 
  select(name, alignment)
```

```
## # A tibble: 24 × 2
##    name         alignment
##    <chr>        <chr>    
##  1 Bizarro      neutral  
##  2 Black Flash  neutral  
##  3 Captain Cold neutral  
##  4 Copycat      neutral  
##  5 Deadpool     neutral  
##  6 Deathstroke  neutral  
##  7 Etrigan      neutral  
##  8 Galactus     neutral  
##  9 Gladiator    neutral  
## 10 Indigo       neutral  
## # ℹ 14 more rows
```


## `superhero_info`
3. Let's say we are only interested in the variables name, alignment, and "race". How would you isolate these variables from `superhero_info`?

```r
superhero_info %>% 
  select(name,alignment, race)
```

```
## # A tibble: 734 × 3
##    name          alignment race             
##    <chr>         <chr>     <chr>            
##  1 A-Bomb        good      Human            
##  2 Abe Sapien    good      Icthyo Sapien    
##  3 Abin Sur      good      Ungaran          
##  4 Abomination   bad       Human / Radiation
##  5 Abraxas       bad       Cosmic Entity    
##  6 Absorbing Man bad       Human            
##  7 Adam Monroe   good      -                
##  8 Adam Strange  good      Human            
##  9 Agent 13      good      -                
## 10 Agent Bob     good      Human            
## # ℹ 724 more rows
```

## Not Human
4. List all of the superheros that are not human.


```r
filter(superhero_info, race != "Human")
```

```
## # A tibble: 526 × 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Abe … Male   blue      Icth… No Hair       191 Dark Hor… blue       good     
##  2 Abin… Male   blue      Unga… No Hair       185 DC Comics red        good     
##  3 Abom… Male   green     Huma… No Hair       203 Marvel C… -          bad      
##  4 Abra… Male   blue      Cosm… Black         -99 Marvel C… -          bad      
##  5 Adam… Male   blue      -     Blond         -99 NBC - He… -          good     
##  6 Agen… Female blue      -     Blond         173 Marvel C… -          good     
##  7 Agen… Male   -         -     -             191 Marvel C… -          good     
##  8 Air-… Male   blue      -     White         188 Marvel C… -          bad      
##  9 Ajax  Male   brown     Cybo… Black         193 Marvel C… -          bad      
## 10 Alan… Male   blue      -     Blond         180 DC Comics -          good     
## # ℹ 516 more rows
## # ℹ 1 more variable: weight <dbl>
```

## Good and Evil
5. Let's make two different data frames, one focused on the "good guys" and another focused on the "bad guys".


```r
good_guys <- filter(superhero_info, alignment == "good")
bad_guys <- filter(superhero_info, alignment == "bad")
```


6. For the good guys, use the `tabyl` function to summarize their "race".


```r
tabyl(good_guys,race)
```

```
##               race   n     percent
##                  - 217 0.437500000
##              Alien   3 0.006048387
##              Alpha   5 0.010080645
##             Amazon   2 0.004032258
##            Android   4 0.008064516
##             Animal   2 0.004032258
##          Asgardian   3 0.006048387
##          Atlantean   4 0.008064516
##         Bolovaxian   1 0.002016129
##              Clone   1 0.002016129
##             Cyborg   3 0.006048387
##           Demi-God   2 0.004032258
##              Demon   3 0.006048387
##            Eternal   1 0.002016129
##     Flora Colossus   1 0.002016129
##        Frost Giant   1 0.002016129
##      God / Eternal   6 0.012096774
##             Gungan   1 0.002016129
##              Human 148 0.298387097
##    Human / Altered   2 0.004032258
##     Human / Cosmic   2 0.004032258
##  Human / Radiation   8 0.016129032
##         Human-Kree   2 0.004032258
##      Human-Spartoi   1 0.002016129
##       Human-Vulcan   1 0.002016129
##    Human-Vuldarian   1 0.002016129
##      Icthyo Sapien   1 0.002016129
##            Inhuman   4 0.008064516
##    Kakarantharaian   1 0.002016129
##         Kryptonian   4 0.008064516
##            Martian   1 0.002016129
##          Metahuman   1 0.002016129
##             Mutant  46 0.092741935
##     Mutant / Clone   1 0.002016129
##             Planet   1 0.002016129
##             Saiyan   1 0.002016129
##           Symbiote   3 0.006048387
##           Talokite   1 0.002016129
##         Tamaranean   1 0.002016129
##            Ungaran   1 0.002016129
##            Vampire   2 0.004032258
##     Yoda's species   1 0.002016129
##      Zen-Whoberian   1 0.002016129
```


7. Among the good guys, Who are the Vampires?


```r
good_guys %>% 
  filter(race == "Vampire")
```

```
## # A tibble: 2 × 10
##   name  gender eye_color race   hair_color height publisher skin_color alignment
##   <chr> <chr>  <chr>     <chr>  <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Angel Male   -         Vampi… -             -99 Dark Hor… -          good     
## 2 Blade Male   brown     Vampi… Black         188 Marvel C… -          good     
## # ℹ 1 more variable: weight <dbl>
```


8. Among the bad guys, who are the male humans over 200 inches in height?


```r
bad_guys %>% 
  filter(race=="human", height >= 200)
```

```
## # A tibble: 0 × 10
## # ℹ 10 variables: name <chr>, gender <chr>, eye_color <chr>, race <chr>,
## #   hair_color <chr>, height <dbl>, publisher <chr>, skin_color <chr>,
## #   alignment <chr>, weight <dbl>
```


9. Are there more good guys or bad guys with green hair?  


```r
filter(good_guys, hair_color == "Green")
```

```
## # A tibble: 7 × 10
##   name   gender eye_color race  hair_color height publisher skin_color alignment
##   <chr>  <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Beast… Male   green     Human Green         173 DC Comics green      good     
## 2 Capta… Male   red       God … Green         -99 Marvel C… -          good     
## 3 Doc S… Male   blue      Huma… Green         198 Marvel C… -          good     
## 4 Hulk   Male   green     Huma… Green         244 Marvel C… green      good     
## 5 Lyja   Female green     -     Green         -99 Marvel C… -          good     
## 6 Polar… Female green     Muta… Green         170 Marvel C… -          good     
## 7 She-H… Female green     Human Green         201 Marvel C… -          good     
## # ℹ 1 more variable: weight <dbl>
```


```r
filter(bad_guys, hair_color == "Green")
```

```
## # A tibble: 1 × 10
##   name  gender eye_color race  hair_color height publisher skin_color alignment
##   <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Joker Male   green     Human Green         196 DC Comics white      bad      
## # ℹ 1 more variable: weight <dbl>
```



10. Let's explore who the really small superheros are. In the `superhero_info` data, which have a weight less than 50? Be sure to sort your results by weight lowest to highest.  

```r
superhero_info %>%
  select(name,weight) %>%
  mutate(weight_new = ifelse(weight == -99, NA, weight)) %>%
   filter(weight < 50) %>%
  arrange(weight)
```

```
## # A tibble: 256 × 3
##    name             weight weight_new
##    <chr>             <dbl>      <dbl>
##  1 Abraxas             -99         NA
##  2 Adam Monroe         -99         NA
##  3 Alex Mercer         -99         NA
##  4 Alex Woolsly        -99         NA
##  5 Allan Quatermain    -99         NA
##  6 Ando Masahashi      -99         NA
##  7 Angel               -99         NA
##  8 Angela              -99         NA
##  9 Anti-Monitor        -99         NA
## 10 Anti-Spawn          -99         NA
## # ℹ 246 more rows
```

11. Let's make a new variable that is the ratio of height to weight. Call this variable `height_weight_ratio`.  


```r
height_weight_ration <- select(superhero_info, "name", "height", "weight")
height_weight_ration$ratio <- height_weight_ration$weight/height_weight_ration$height
height_weight_ration
```

```
## # A tibble: 734 × 4
##    name          height weight ratio
##    <chr>          <dbl>  <dbl> <dbl>
##  1 A-Bomb           203    441 2.17 
##  2 Abe Sapien       191     65 0.340
##  3 Abin Sur         185     90 0.486
##  4 Abomination      203    441 2.17 
##  5 Abraxas          -99    -99 1    
##  6 Absorbing Man    193    122 0.632
##  7 Adam Monroe      -99    -99 1    
##  8 Adam Strange     185     88 0.476
##  9 Agent 13         173     61 0.353
## 10 Agent Bob        178     81 0.455
## # ℹ 724 more rows
```


12. Who has the highest height to weight ratio?  


```r
height_weight_ration %>% 
  arrange(desc(ratio))
```

```
## # A tibble: 734 × 4
##    name        height weight ratio
##    <chr>        <dbl>  <dbl> <dbl>
##  1 Giganta       62.5    630 10.1 
##  2 Utgard-Loki   15.2     58  3.82
##  3 Darkseid     267      817  3.06
##  4 Juggernaut   287      855  2.98
##  5 Red Hulk     213      630  2.96
##  6 Sasquatch    305      900  2.95
##  7 Hulk         244      630  2.58
##  8 Bloodaxe     218      495  2.27
##  9 Thanos       201      443  2.20
## 10 A-Bomb       203      441  2.17
## # ℹ 724 more rows
```
Giganta is the highest

## `superhero_powers`
Have a quick look at the `superhero_powers` data frame.  

13. How many superheros have a combination of agility, stealth, super_strength, stamina?


```r
#superhero_powers %>%
#  select(hero_names, Agility, Stealth, Super Strength, stamina) %>% #stuck
  summary(superhero_powers)
```

```
##   hero_names         Agility        Accelerated Healing Lantern Power Ring
##  Length:667         Mode :logical   Mode :logical       Mode :logical     
##  Class :character   FALSE:425       FALSE:489           FALSE:656         
##  Mode  :character   TRUE :242       TRUE :178           TRUE :11          
##  Dimensional Awareness Cold Resistance Durability       Stealth       
##  Mode :logical         Mode :logical   Mode :logical   Mode :logical  
##  FALSE:642             FALSE:620       FALSE:410       FALSE:541      
##  TRUE :25              TRUE :47        TRUE :257       TRUE :126      
##  Energy Absorption   Flight        Danger Sense    Underwater breathing
##  Mode :logical     Mode :logical   Mode :logical   Mode :logical       
##  FALSE:590         FALSE:455       FALSE:637       FALSE:646           
##  TRUE :77          TRUE :212       TRUE :30        TRUE :21            
##  Marksmanship    Weapons Master  Power Augmentation Animal Attributes
##  Mode :logical   Mode :logical   Mode :logical      Mode :logical    
##  FALSE:548       FALSE:562       FALSE:659          FALSE:642        
##  TRUE :119       TRUE :105       TRUE :8            TRUE :25         
##  Longevity       Intelligence    Super Strength  Cryokinesis    
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:554       FALSE:509       FALSE:307       FALSE:648      
##  TRUE :113       TRUE :158       TRUE :360       TRUE :19       
##  Telepathy       Energy Armor    Energy Blasts   Duplication    
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:575       FALSE:659       FALSE:520       FALSE:651      
##  TRUE :92        TRUE :8         TRUE :147       TRUE :16       
##  Size Changing   Density Control  Stamina        Astral Travel  
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:612       FALSE:652       FALSE:378       FALSE:663      
##  TRUE :55        TRUE :15        TRUE :289       TRUE :4        
##  Audio Control   Dexterity        Omnitrix       Super Speed    
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:660       FALSE:661       FALSE:666       FALSE:418      
##  TRUE :7         TRUE :6         TRUE :1         TRUE :249      
##  Possession      Animal Oriented Powers Weapon-based Powers Electrokinesis 
##  Mode :logical   Mode :logical          Mode :logical       Mode :logical  
##  FALSE:659       FALSE:627              FALSE:609           FALSE:645      
##  TRUE :8         TRUE :40               TRUE :58            TRUE :22       
##  Darkforce Manipulation Death Touch     Teleportation   Enhanced Senses
##  Mode :logical          Mode :logical   Mode :logical   Mode :logical  
##  FALSE:657              FALSE:660       FALSE:595       FALSE:578      
##  TRUE :10               TRUE :7         TRUE :72        TRUE :89       
##  Telekinesis     Energy Beams      Magic         Hyperkinesis   
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:606       FALSE:625       FALSE:623       FALSE:666      
##  TRUE :61        TRUE :42        TRUE :44        TRUE :1        
##     Jump         Clairvoyance    Dimensional Travel Power Sense    
##  Mode :logical   Mode :logical   Mode :logical      Mode :logical  
##  FALSE:602       FALSE:663       FALSE:644          FALSE:664      
##  TRUE :65        TRUE :4         TRUE :23           TRUE :3        
##  Shapeshifting   Peak Human Condition Immortality     Camouflage     
##  Mode :logical   Mode :logical        Mode :logical   Mode :logical  
##  FALSE:601       FALSE:637            FALSE:598       FALSE:646      
##  TRUE :66        TRUE :30             TRUE :69        TRUE :21       
##  Element Control  Phasing        Astral Projection Electrical Transport
##  Mode :logical   Mode :logical   Mode :logical     Mode :logical       
##  FALSE:659       FALSE:636       FALSE:638         FALSE:666           
##  TRUE :8         TRUE :31        TRUE :29          TRUE :1             
##  Fire Control    Projection      Summoning       Enhanced Memory
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:635       FALSE:665       FALSE:663       FALSE:642      
##  TRUE :32        TRUE :2         TRUE :4         TRUE :25       
##   Reflexes       Invulnerability Energy Constructs Force Fields   
##  Mode :logical   Mode :logical   Mode :logical     Mode :logical  
##  FALSE:503       FALSE:550       FALSE:629         FALSE:581      
##  TRUE :164       TRUE :117       TRUE :38          TRUE :86       
##  Self-Sustenance Anti-Gravity     Empathy        Power Nullifier
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:630       FALSE:666       FALSE:648       FALSE:663      
##  TRUE :37        TRUE :1         TRUE :19        TRUE :4        
##  Radiation Control Psionic Powers  Elasticity      Substance Secretion
##  Mode :logical     Mode :logical   Mode :logical   Mode :logical      
##  FALSE:660         FALSE:618       FALSE:656       FALSE:650          
##  TRUE :7           TRUE :49        TRUE :11        TRUE :17           
##  Elemental Transmogrification Technopath/Cyberpath Photographic Reflexes
##  Mode :logical                Mode :logical        Mode :logical        
##  FALSE:661                    FALSE:644            FALSE:664            
##  TRUE :6                      TRUE :23             TRUE :3              
##  Seismic Power   Animation       Precognition    Mind Control   
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:664       FALSE:662       FALSE:645       FALSE:645      
##  TRUE :3         TRUE :5         TRUE :22        TRUE :22       
##  Fire Resistance Power Absorption Enhanced Hearing Nova Force     
##  Mode :logical   Mode :logical    Mode :logical    Mode :logical  
##  FALSE:649       FALSE:655        FALSE:595        FALSE:665      
##  TRUE :18        TRUE :12         TRUE :72         TRUE :2        
##   Insanity       Hypnokinesis    Animal Control  Natural Armor  
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:662       FALSE:644       FALSE:658       FALSE:631      
##  TRUE :5         TRUE :23        TRUE :9         TRUE :36       
##  Intangibility   Enhanced Sight  Molecular Manipulation Heat Generation
##  Mode :logical   Mode :logical   Mode :logical          Mode :logical  
##  FALSE:647       FALSE:642       FALSE:625              FALSE:643      
##  TRUE :20        TRUE :25        TRUE :42               TRUE :24       
##  Adaptation       Gliding        Power Suit      Mind Blast     
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:662       FALSE:657       FALSE:634       FALSE:655      
##  TRUE :5         TRUE :10        TRUE :33        TRUE :12       
##  Probability Manipulation Gravity Control Regeneration    Light Control  
##  Mode :logical            Mode :logical   Mode :logical   Mode :logical  
##  FALSE:658                FALSE:651       FALSE:639       FALSE:652      
##  TRUE :9                  TRUE :16        TRUE :28        TRUE :15       
##  Echolocation    Levitation      Toxin and Disease Control   Banish       
##  Mode :logical   Mode :logical   Mode :logical             Mode :logical  
##  FALSE:665       FALSE:641       FALSE:657                 FALSE:666      
##  TRUE :2         TRUE :26        TRUE :10                  TRUE :1        
##  Energy Manipulation Heat Resistance Natural Weapons Time Travel    
##  Mode :logical       Mode :logical   Mode :logical   Mode :logical  
##  FALSE:615           FALSE:624       FALSE:609       FALSE:634      
##  TRUE :52            TRUE :43        TRUE :58        TRUE :33       
##  Enhanced Smell  Illusions       Thirstokinesis  Hair Manipulation
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical    
##  FALSE:635       FALSE:629       FALSE:666       FALSE:666        
##  TRUE :32        TRUE :38        TRUE :1         TRUE :1          
##  Illumination    Omnipotent       Cloaking       Changing Armor 
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:665       FALSE:660       FALSE:660       FALSE:666      
##  TRUE :2         TRUE :7         TRUE :7         TRUE :1        
##  Power Cosmic    Biokinesis      Water Control   Radiation Immunity
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical     
##  FALSE:660       FALSE:666       FALSE:654       FALSE:657         
##  TRUE :7         TRUE :1         TRUE :13        TRUE :10          
##  Vision - Telescopic Toxin and Disease Resistance Spatial Awareness
##  Mode :logical       Mode :logical                Mode :logical    
##  FALSE:624           FALSE:619                    FALSE:666        
##  TRUE :43            TRUE :48                     TRUE :1          
##  Energy Resistance Telepathy Resistance Molecular Combustion Omnilingualism 
##  Mode :logical     Mode :logical        Mode :logical        Mode :logical  
##  FALSE:660         FALSE:634            FALSE:665            FALSE:646      
##  TRUE :7           TRUE :33             TRUE :2              TRUE :21       
##  Portal Creation Magnetism       Mind Control Resistance Plant Control  
##  Mode :logical   Mode :logical   Mode :logical           Mode :logical  
##  FALSE:663       FALSE:656       FALSE:655               FALSE:659      
##  TRUE :4         TRUE :11        TRUE :12                TRUE :8        
##    Sonar         Sonic Scream    Time Manipulation Enhanced Touch 
##  Mode :logical   Mode :logical   Mode :logical     Mode :logical  
##  FALSE:663       FALSE:661       FALSE:647         FALSE:660      
##  TRUE :4         TRUE :6         TRUE :20          TRUE :7        
##  Magic Resistance Invisibility    Sub-Mariner     Radiation Absorption
##  Mode :logical    Mode :logical   Mode :logical   Mode :logical       
##  FALSE:661        FALSE:645       FALSE:647       FALSE:660           
##  TRUE :6          TRUE :22        TRUE :20        TRUE :7             
##  Intuitive aptitude Vision - Microscopic  Melting        Wind Control   
##  Mode :logical      Mode :logical        Mode :logical   Mode :logical  
##  FALSE:666          FALSE:648            FALSE:665       FALSE:664      
##  TRUE :1            TRUE :19             TRUE :2         TRUE :3        
##  Super Breath    Wallcrawling    Vision - Night  Vision - Infrared
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical    
##  FALSE:644       FALSE:633       FALSE:633       FALSE:645        
##  TRUE :23        TRUE :34        TRUE :34        TRUE :22         
##  Grim Reaping    Matter Absorption The Force       Resurrection   
##  Mode :logical   Mode :logical     Mode :logical   Mode :logical  
##  FALSE:664       FALSE:656         FALSE:661       FALSE:652      
##  TRUE :3         TRUE :11          TRUE :6         TRUE :15       
##  Terrakinesis    Vision - Heat   Vitakinesis     Radar Sense    
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:665       FALSE:648       FALSE:665       FALSE:661      
##  TRUE :2         TRUE :19        TRUE :2         TRUE :6        
##  Qwardian Power Ring Weather Control Vision - X-Ray  Vision - Thermal
##  Mode :logical       Mode :logical   Mode :logical   Mode :logical   
##  FALSE:665           FALSE:659       FALSE:644       FALSE:644       
##  TRUE :2             TRUE :8         TRUE :23        TRUE :23        
##  Web Creation    Reality Warping Odin Force      Symbiote Costume
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical   
##  FALSE:653       FALSE:651       FALSE:665       FALSE:658       
##  TRUE :14        TRUE :16        TRUE :2         TRUE :9         
##  Speed Force     Phoenix Force   Molecular Dissipation Vision - Cryo  
##  Mode :logical   Mode :logical   Mode :logical         Mode :logical  
##  FALSE:666       FALSE:666       FALSE:666             FALSE:665      
##  TRUE :1         TRUE :1         TRUE :1               TRUE :2        
##  Omnipresent     Omniscient     
##  Mode :logical   Mode :logical  
##  FALSE:665       FALSE:665      
##  TRUE :2         TRUE :2
```



## Your Favorite
14. Pick your favorite superhero and let's see their powers!  


```r
filter(superhero_powers, hero_names=="Cat")
```

```
## # A tibble: 1 × 168
##   hero_names Agility `Accelerated Healing` `Lantern Power Ring`
##   <chr>      <lgl>   <lgl>                 <lgl>               
## 1 Cat        TRUE    FALSE                 FALSE               
## # ℹ 164 more variables: `Dimensional Awareness` <lgl>, `Cold Resistance` <lgl>,
## #   Durability <lgl>, Stealth <lgl>, `Energy Absorption` <lgl>, Flight <lgl>,
## #   `Danger Sense` <lgl>, `Underwater breathing` <lgl>, Marksmanship <lgl>,
## #   `Weapons Master` <lgl>, `Power Augmentation` <lgl>,
## #   `Animal Attributes` <lgl>, Longevity <lgl>, Intelligence <lgl>,
## #   `Super Strength` <lgl>, Cryokinesis <lgl>, Telepathy <lgl>,
## #   `Energy Armor` <lgl>, `Energy Blasts` <lgl>, Duplication <lgl>, …
```

15. Can you find your hero in the superhero_info data? Show their info!  


```r
filter(superhero_info, name=="Cat")
```

```
## # A tibble: 1 × 10
##   name  gender eye_color race  hair_color height publisher  skin_color alignment
##   <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>      <chr>      <chr>    
## 1 Cat   Female blue      -     Blond         173 Marvel Co… -          good     
## # ℹ 1 more variable: weight <dbl>
```

```r
getwd()
```

```
## [1] "/Users/jacquesmak/Desktop/BIS15W2024_JMAK/lab6"
```


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
