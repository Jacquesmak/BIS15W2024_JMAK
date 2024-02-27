BIS 015L W24 MT2 Study Guide
============================

This document serves as a reference sheet for the topics covered on Midterm 2.

For resources on data transformation, see `mt1_studyguide.md` in the `study_mt1`
folder.

+ [Lab 8](#lab-8)
+ [Lab 9](#lab-9)
+ [Lab 10](#lab-10)
+ [Lab 11](#lab-11)
+ [Lab 12](#lab-12)

## Lab 8

### 8-1

+ `summarize()`, `group_by()`
    + Use the `.groups="keep"` argument in `summarize()` if grouping by 2 variables at 
    once!
+ `count()` as a combination of `summarize()` and `group_by()` (number of observations
under specified variable)
+ `across()` to use `filter()` and `select()` across multiple variables
	+ also works with `group_by()`
+ `summarize_all()`

### 8-2

+ Nuclear approach to dealing with NAs: manage NAs when you load data
    + `readr::read_csv(file = "#file", na = c("NA"", "-999"))`
+ `map_df(~sum(is.na(.)))` for number of NAs under each variable
+ `naniar` is a package for dealing with NAs
    + `miss_var_summary()`
        + can use with `group_by()`
    + `hist()` # to visualize observations
    + `replace_with_na()`
+ `mutate()` and `na_if()` to change values to NAs
+ `visdat` is a package we can use to visualize NAs
    + `vis_dat()`
    + `vis_miss()`

## Lab 9

### 9-1

+ Messy vs. tidy data
+ `pivot_longer()` shifts data from wide to long format
    + `pivot_longer(# cols to pivot, names_to = , #names_prefix =, values_to =, 
    #values_drop_na=T)`
    	+ `names_prefix` (to drop prefix in column name) and `values_drop_na` (to drop 
    	NAs) are optional
    + The columns you specify will become values under the new column named in 
    `names_to`, while the values originally stored under the columns will go to the
    new column in `values_to`
    + If there is more than one variable in a column name separated by a `_`, for
    example:
        + `names_to = c("var1", "var2"), names_sep="_"`

### 9-2

+ `separate()` to split one column into multiple columns
+ `unite()` is the opposite
+ `pivot_wider()` shifts data from long to wide format
    + `pivot_wider(names_from = , values_from =)`
    + The observations under `names_from` will become columns, under which values from
    `values_from` will be stored

## Lab 10

### 10-1

+ ggplot2 cheatsheet
+ plot = data + geom_ + aesthetics
+ `dataframe >%> ggplot(aes(x =, y=)) + geom_()`
+ `geom_boxplot()`
+ `geom_point()` vs. `geom_jitter()` (prevent overplotting)
    + `geom_smooth()` to add regression line (can layer)
+ `geom_bar()`: don't need to specify y-axis, since it is automatically the counts for
each observation under the categorical variable x
    + If you really want to specify a y-axis with `geom_bar()`, use `geom_bar(stat=
    identity)`
+ FYI: `top_n(10, variable)` gives you top 10 while `top_n(-10, variable)` gives you
bottom 10
+ `geom_col()`: allows us to specify y-axis besides counts

### 10-2

+ To visualize range of values, use boxplots (x = categorical, y = continuous)
+ Transform the data prior to creating plot

## Lab 11

### 11-1

+ `coord_flip()`
+ `options(scipen=999)` cancels scientific notation for the session
+ `scale_y_log10()` and `scale_x_log10`
+ `ggpplot(aes(x=reorder(x, y), y=y))`
    + reorder the order of the values on the x axis from highest values of y to lowest
    (bar graphs)
+ `geom_point()` or `geom_jitter()` for plotting two continuous variables
+ to create labels: `labs(title="", x=""/NULL, y=""/NULL)`
+ Use `theme()` to further modify the labels/their style (text size, justification, face,
angle)
+ Grouping:
    + `fill` is a common grouping option for bar plots
    + `size` adjusts the size of points relative to a continuous variable; or you can
    just specify the size yourself

### 11-2

+ More grouping:
    + `color` or `shape` groups by categorical variable on scatterplots
+ `position = "dodge"` to create side by side bar plots that are grouped by a
categorical variable
    + Default with `fill` is a stacked bar plot
+ Change angle of variables on axis (`theme`)
+ Scale all bars to a percentage: `scale_y_continuous(labels = scales::percent)` (e.g,
in each of my bars what proportion of the counts are carnivores or herbivores)
+ `group` is like `fill` , but it doesn't add color

## Lab 12

### 12-1

+ Line plots to show changes over time
	+ You might want to `mutate()` your time variable to be a factor 
	+ `geom_line()` + `geom_line()`
+ Histograms show the distribution of continuous variables
    + `geom_histogram(bins=)` if you need to adjust number of bins
+ Use `color` and `fill` in your `geom_()` to specify
+ Density plots are similar to histograms, but use a smoothing function 
+ To layer `geom_histogram()` and `geom_density()`:
	+ `geom_histogram(aes(y = after_stat(density)), # color, fill) +  geom_density()`
+ `case_when()` to calculate a new variable from other variables
    + We use `case_when()` within `mutate()`
    + For example, if we want to put values of mass into categories (which will go under
    a new variable): `mutate(mass_category=case_when(mass<=2 ~ "small", mass>2 & 
    mass<=5 ~ "medium", mass>5 ~ "large"))`
        + "small," "medium," and "large" are the distinct values under your new variable,
        "mass_category"
    + The `gtools` package has a useful `quantcut()` function which can be used with 
    `table()` to see how many observations fall under each quartile; these are useful
    guidelines for creating categories

### 12-2

+ Layer one of the ggplot themes to your plot to make it fancy
    + E.g., `+ theme_linedraw()`
    + + The `ggthemes` library lets you layer more fun themes
+ In `theme()`, you can also adjust the position of your legend, not just the main title
and axes 
    + Or the title of the legend: `theme(legend.title = element_text(#color, size))`
    + Or the text under the legend: `theme(legend.text = element_text(#color, size))`
+ The `RColorBrewer` package is fun for customizing colors
    + `?RColorBrewer`
    + `scale_colour_brewer(palette = )` is for points
    + `scale_fill_brewer(palette = )` is for fills
+ You can manually set colors with the `paletteer` package
    + `colors <- paletter::palettes_d_names` to view collection
    + `mypalette <- paletteer_d("  ::  ")` to store your favorite palette
    + `barplot(rep(1, #), axes=FALSE, col=mypalette)` to view your selected colors
+ Faceting allows us to create multi-panel plots for easy comparison
    + `facet_wrap(~variable)` makes a panels for categorical variable (one panel for
    each value)
    + `facet_grid(variable~.)` gives panels in rows
    + `facet_grid(.~variable)` gives panels in columns (default with `facet_wrap()`)