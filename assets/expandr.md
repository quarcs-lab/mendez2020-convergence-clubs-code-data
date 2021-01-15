---
title: "Introduction to expandr"
output:
  html_document:
    keep_md: yes
  github_document: default
always_allow_html: true
---
 
 
### Load R packages
 

```r
suppressWarnings(suppressMessages({
  library(knitr)
  library(kableExtra)
  library(htmltools)
  library(tidyverse)
  library(scales)
  library(ExPanDaR)
}))
knitr::opts_chunk$set(fig.align = 'center')
```
 
 
### Import data



```r
dat <- read_csv("https://raw.githubusercontent.com/quarcs-lab/mendez2020-convergence-clubs-code-data/master/assets/dat.csv")
```

```
## Parsed with column specification:
## cols(
##   .default = col_double(),
##   country = col_character(),
##   region = col_character(),
##   hi1990 = col_character(),
##   isocode = col_character()
## )
```

```
## See spec(...) for full column specifications.
```


```r
dat %>%
 glimpse()
```

```
## Rows: 2,700
## Columns: 29
## $ id            <dbl> 62, 62, 62, 62, 62, 62, 62, 13, 13, 13, 13, 62, 13, 13,…
## $ country       <chr> "Mozambique", "Mozambique", "Mozambique", "Mozambique",…
## $ year          <dbl> 1990, 1991, 1992, 1993, 1994, 1995, 1996, 2004, 2003, 2…
## $ Y             <dbl> 7034.000, 7742.999, 6792.000, 7223.000, 8194.000, 7671.…
## $ K             <dbl> 6262, 6462, 6592, 6859, 7246, 7734, 8121, 10047, 9311, …
## $ pop           <dbl> 13.371971, 13.719853, 14.203987, 14.775877, 15.363065, …
## $ L             <dbl> 5.413710, 5.593190, 5.844729, 6.187860, 6.513672, 6.804…
## $ s             <dbl> 0.9964694, 0.9829382, 0.9694070, 0.9558758, 0.9423445, …
## $ alpha_it      <dbl> 0.5737705, 0.5737705, 0.5737705, 0.5737705, 0.5737705, …
## $ GDPpc         <dbl> 526.0256, 564.3646, 478.1756, 488.8373, 533.3571, 482.0…
## $ lp            <dbl> 1299.294, 1384.362, 1162.073, 1167.286, 1257.969, 1127.…
## $ h             <dbl> 1.347076, 1.343633, 1.340181, 1.336720, 1.333250, 1.329…
## $ kl            <dbl> 1156.693, 1155.333, 1127.854, 1108.461, 1112.429, 1136.…
## $ kp            <dbl> 1.1232833, 1.1982358, 1.0303398, 1.0530690, 1.1308308, …
## $ ky            <dbl> 0.8902473, 0.8345603, 0.9705536, 0.9496054, 0.8843056, …
## $ TFP           <dbl> 203.9550, 220.0026, 189.1149, 194.9619, 213.7372, 193.1…
## $ log_GDPpc_raw <dbl> 6.265350, 6.335700, 6.169978, 6.192030, 6.279191, 6.178…
## $ log_lp_raw    <dbl> 7.169576, 7.232995, 7.057961, 7.062437, 7.137254, 7.027…
## $ log_ky_raw    <dbl> -0.116255940, -0.180850310, -0.029888673, -0.051708743,…
## $ log_h_raw     <dbl> 0.2979364, 0.2953772, 0.2928047, 0.2902188, 0.2876196, …
## $ log_tfp_raw   <dbl> 5.317899, 5.393640, 5.242355, 5.272804, 5.364747, 5.263…
## $ log_GDPpc     <dbl> 6.163751, 6.195724, 6.227951, 6.261036, 6.295438, 6.331…
## $ log_lp        <dbl> 7.050233, 7.075745, 7.101554, 7.128354, 7.156726, 7.187…
## $ log_ky        <dbl> -0.1290631, -0.1301618, -0.1312285, -0.1323578, -0.1333…
## $ log_h         <dbl> 0.2770405, 0.2796887, 0.2823892, 0.2852334, 0.2883388, …
## $ log_tfp       <dbl> 5.257494, 5.286922, 5.316501, 5.346648, 5.377597, 5.409…
## $ region        <chr> "Africa", "Africa", "Africa", "Africa", "Africa", "Afri…
## $ hi1990        <chr> "no", "no", "no", "no", "no", "no", "no", "no", "no", "…
## $ isocode       <chr> "MOZ", "MOZ", "MOZ", "MOZ", "MOZ", "MOZ", "MOZ", "BDI",…
```



```r
# Import data definitions
dat_def <- read_csv("https://raw.githubusercontent.com/quarcs-lab/mendez2020-convergence-clubs-code-data/master/assets/dat-definitions.csv")
```

```
## Parsed with column specification:
## cols(
##   var_name = col_character(),
##   var_def = col_character(),
##   type = col_character()
## )
```


```r
dat_def %>%
  print(n = Inf)
```

```
## # A tibble: 28 x 3
##    var_name      var_def                                                  type  
##    <chr>         <chr>                                                    <chr> 
##  1 country       Standardized country name (from PWT)                     cs_id 
##  2 year          Year                                                     ts_id 
##  3 Y             GDP                                                      numer…
##  4 K             Physical Capital                                         numer…
##  5 pop           Population                                               numer…
##  6 L             Labor Force                                              numer…
##  7 s             Years of Schooling                                       numer…
##  8 alpha_it      Variable Capital Share                                   numer…
##  9 GDPpc         GDP per capita                                           numer…
## 10 lp            Labor Productivity                                       numer…
## 11 h             Human Capital Index                                      numer…
## 12 kl            Capital per Worker                                       numer…
## 13 kp            Capital Productivity                                     numer…
## 14 ky            Capital-Output Ratio                                     numer…
## 15 TFP           Aggregate Efficiency                                     numer…
## 16 log_GDPpc_raw Log of GDP per capita                                    numer…
## 17 log_lp_raw    Log of Labor Productivity                                numer…
## 18 log_ky_raw    Log of Capital-Output Ratio                              numer…
## 19 log_h_raw     Log of Human Capital                                     numer…
## 20 log_tfp_raw   Log of Total Factor Productivity                         numer…
## 21 log_GDPpc     Trend (HP400) of log of Labor Productivity               numer…
## 22 log_lp        Trend (HP400) of log of GDP per capita                   numer…
## 23 log_ky        Trend (HP400) of log of Capital-Output Ratio             numer…
## 24 log_h         Trend (HP400) of log of Human Capital                    numer…
## 25 log_tfp       Trend (HP400) of log of Aggregate Efficiency             numer…
## 26 region        Regional group (Classification of the UN)                factor
## 27 hi1990        High income country (as of 1990, World Bank classificat… factor
## 28 isocode       ISO code from the PWT9.0                                 factor
```



### Bar Chart
 

```r
df <- dat
df$year <- as.factor(df$year)
df$hi1990 <- as.factor(df$hi1990)
p <- ggplot(df, aes(x = year)) +
  geom_bar(aes(fill = hi1990), position = "fill") +
  labs(x = "year", fill = "hi1990", y = "Percent") +
  scale_y_continuous(labels = percent_format()) 
p <- p + scale_x_discrete(breaks = pretty(as.numeric(as.character(df$year)), n = 10))
p
```

<img src="expandr_files/figure-html/bar_chart-1.png" style="display: block; margin: auto;" />
 
 
### Missing Values
 

```r
df <- dat
prepare_missing_values_graph(df, "year")
```

<img src="expandr_files/figure-html/missing_values-1.png" style="display: block; margin: auto;" />
 
 
### Descriptive Statistics
 

```r
df <- dat[df$year == "1990", ]
t <- prepare_descriptive_table(df)
t$kable_ret  %>%
  kable_styling("condensed", full_width = F, position = "center")
```

<table class="table table-condensed" style="width: auto !important; margin-left: auto; margin-right: auto;">
<caption>Descriptive Statistics</caption>
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:right;"> N </th>
   <th style="text-align:right;"> Mean </th>
   <th style="text-align:right;"> Std. dev. </th>
   <th style="text-align:right;"> Min. </th>
   <th style="text-align:right;"> 25 % </th>
   <th style="text-align:right;"> Median </th>
   <th style="text-align:right;"> 75 % </th>
   <th style="text-align:right;"> Max. </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> id </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 54.500 </td>
   <td style="text-align:right;"> 31.321 </td>
   <td style="text-align:right;"> 1.000 </td>
   <td style="text-align:right;"> 27.750 </td>
   <td style="text-align:right;"> 54.500 </td>
   <td style="text-align:right;"> 81.250 </td>
   <td style="text-align:right;"> 108.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> year </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 1,990.000 </td>
   <td style="text-align:right;"> 0.000 </td>
   <td style="text-align:right;"> 1,990.000 </td>
   <td style="text-align:right;"> 1,990.000 </td>
   <td style="text-align:right;"> 1,990.000 </td>
   <td style="text-align:right;"> 1,990.000 </td>
   <td style="text-align:right;"> 1,990.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Y </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 364,598.139 </td>
   <td style="text-align:right;"> 1,030,271.047 </td>
   <td style="text-align:right;"> 3,067.000 </td>
   <td style="text-align:right;"> 19,377.750 </td>
   <td style="text-align:right;"> 76,730.500 </td>
   <td style="text-align:right;"> 234,608.000 </td>
   <td style="text-align:right;"> 9,259,567.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> K </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 962,050.796 </td>
   <td style="text-align:right;"> 2,873,937.935 </td>
   <td style="text-align:right;"> 2,004.000 </td>
   <td style="text-align:right;"> 31,131.000 </td>
   <td style="text-align:right;"> 162,679.000 </td>
   <td style="text-align:right;"> 713,896.250 </td>
   <td style="text-align:right;"> 26,453,210.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> pop </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 45.410 </td>
   <td style="text-align:right;"> 140.672 </td>
   <td style="text-align:right;"> 1.565 </td>
   <td style="text-align:right;"> 5.106 </td>
   <td style="text-align:right;"> 10.354 </td>
   <td style="text-align:right;"> 34.444 </td>
   <td style="text-align:right;"> 1,154.606 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> L </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 19.507 </td>
   <td style="text-align:right;"> 69.002 </td>
   <td style="text-align:right;"> 0.703 </td>
   <td style="text-align:right;"> 2.056 </td>
   <td style="text-align:right;"> 4.352 </td>
   <td style="text-align:right;"> 12.218 </td>
   <td style="text-align:right;"> 637.075 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> s </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 6.499 </td>
   <td style="text-align:right;"> 2.905 </td>
   <td style="text-align:right;"> 0.893 </td>
   <td style="text-align:right;"> 4.164 </td>
   <td style="text-align:right;"> 6.982 </td>
   <td style="text-align:right;"> 8.792 </td>
   <td style="text-align:right;"> 12.199 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> alpha_it </td>
   <td style="text-align:right;"> 90 </td>
   <td style="text-align:right;"> 0.433 </td>
   <td style="text-align:right;"> 0.113 </td>
   <td style="text-align:right;"> 0.148 </td>
   <td style="text-align:right;"> 0.355 </td>
   <td style="text-align:right;"> 0.432 </td>
   <td style="text-align:right;"> 0.493 </td>
   <td style="text-align:right;"> 0.768 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> GDPpc </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 9,784.000 </td>
   <td style="text-align:right;"> 9,475.931 </td>
   <td style="text-align:right;"> 526.026 </td>
   <td style="text-align:right;"> 2,103.651 </td>
   <td style="text-align:right;"> 6,126.897 </td>
   <td style="text-align:right;"> 16,453.113 </td>
   <td style="text-align:right;"> 37,503.441 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> lp </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 23,223.794 </td>
   <td style="text-align:right;"> 20,088.835 </td>
   <td style="text-align:right;"> 1,299.294 </td>
   <td style="text-align:right;"> 6,294.126 </td>
   <td style="text-align:right;"> 16,984.627 </td>
   <td style="text-align:right;"> 38,834.154 </td>
   <td style="text-align:right;"> 75,036.344 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> h </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 2.682 </td>
   <td style="text-align:right;"> 0.734 </td>
   <td style="text-align:right;"> 1.320 </td>
   <td style="text-align:right;"> 2.085 </td>
   <td style="text-align:right;"> 2.767 </td>
   <td style="text-align:right;"> 3.246 </td>
   <td style="text-align:right;"> 4.252 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> kl </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 63,403.227 </td>
   <td style="text-align:right;"> 68,027.748 </td>
   <td style="text-align:right;"> 725.042 </td>
   <td style="text-align:right;"> 9,651.041 </td>
   <td style="text-align:right;"> 35,519.549 </td>
   <td style="text-align:right;"> 102,058.916 </td>
   <td style="text-align:right;"> 255,639.410 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> kp </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 0.614 </td>
   <td style="text-align:right;"> 0.610 </td>
   <td style="text-align:right;"> 0.209 </td>
   <td style="text-align:right;"> 0.329 </td>
   <td style="text-align:right;"> 0.458 </td>
   <td style="text-align:right;"> 0.625 </td>
   <td style="text-align:right;"> 5.070 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ky </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 2.279 </td>
   <td style="text-align:right;"> 0.987 </td>
   <td style="text-align:right;"> 0.197 </td>
   <td style="text-align:right;"> 1.599 </td>
   <td style="text-align:right;"> 2.184 </td>
   <td style="text-align:right;"> 3.039 </td>
   <td style="text-align:right;"> 4.775 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> TFP </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 824.208 </td>
   <td style="text-align:right;"> 652.602 </td>
   <td style="text-align:right;"> 139.966 </td>
   <td style="text-align:right;"> 360.481 </td>
   <td style="text-align:right;"> 702.477 </td>
   <td style="text-align:right;"> 1,053.205 </td>
   <td style="text-align:right;"> 4,164.725 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> log_GDPpc_raw </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 8.606 </td>
   <td style="text-align:right;"> 1.182 </td>
   <td style="text-align:right;"> 6.265 </td>
   <td style="text-align:right;"> 7.651 </td>
   <td style="text-align:right;"> 8.720 </td>
   <td style="text-align:right;"> 9.708 </td>
   <td style="text-align:right;"> 10.532 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> log_lp_raw </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 9.564 </td>
   <td style="text-align:right;"> 1.101 </td>
   <td style="text-align:right;"> 7.170 </td>
   <td style="text-align:right;"> 8.747 </td>
   <td style="text-align:right;"> 9.740 </td>
   <td style="text-align:right;"> 10.567 </td>
   <td style="text-align:right;"> 11.226 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> log_ky_raw </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 0.699 </td>
   <td style="text-align:right;"> 0.563 </td>
   <td style="text-align:right;"> -1.623 </td>
   <td style="text-align:right;"> 0.470 </td>
   <td style="text-align:right;"> 0.781 </td>
   <td style="text-align:right;"> 1.112 </td>
   <td style="text-align:right;"> 1.563 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> log_h_raw </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 0.946 </td>
   <td style="text-align:right;"> 0.291 </td>
   <td style="text-align:right;"> 0.278 </td>
   <td style="text-align:right;"> 0.735 </td>
   <td style="text-align:right;"> 1.018 </td>
   <td style="text-align:right;"> 1.177 </td>
   <td style="text-align:right;"> 1.447 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> log_tfp_raw </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 6.441 </td>
   <td style="text-align:right;"> 0.756 </td>
   <td style="text-align:right;"> 4.941 </td>
   <td style="text-align:right;"> 5.887 </td>
   <td style="text-align:right;"> 6.555 </td>
   <td style="text-align:right;"> 6.960 </td>
   <td style="text-align:right;"> 8.334 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> log_GDPpc </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 8.542 </td>
   <td style="text-align:right;"> 1.189 </td>
   <td style="text-align:right;"> 6.164 </td>
   <td style="text-align:right;"> 7.568 </td>
   <td style="text-align:right;"> 8.676 </td>
   <td style="text-align:right;"> 9.604 </td>
   <td style="text-align:right;"> 10.492 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> log_lp </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 9.519 </td>
   <td style="text-align:right;"> 1.123 </td>
   <td style="text-align:right;"> 7.050 </td>
   <td style="text-align:right;"> 8.633 </td>
   <td style="text-align:right;"> 9.698 </td>
   <td style="text-align:right;"> 10.386 </td>
   <td style="text-align:right;"> 11.225 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> log_ky </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 0.740 </td>
   <td style="text-align:right;"> 0.585 </td>
   <td style="text-align:right;"> -1.807 </td>
   <td style="text-align:right;"> 0.478 </td>
   <td style="text-align:right;"> 0.846 </td>
   <td style="text-align:right;"> 1.130 </td>
   <td style="text-align:right;"> 1.574 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> log_h </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 0.948 </td>
   <td style="text-align:right;"> 0.293 </td>
   <td style="text-align:right;"> 0.266 </td>
   <td style="text-align:right;"> 0.728 </td>
   <td style="text-align:right;"> 1.031 </td>
   <td style="text-align:right;"> 1.176 </td>
   <td style="text-align:right;"> 1.453 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> log_tfp </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 6.391 </td>
   <td style="text-align:right;"> 0.787 </td>
   <td style="text-align:right;"> 4.913 </td>
   <td style="text-align:right;"> 5.713 </td>
   <td style="text-align:right;"> 6.543 </td>
   <td style="text-align:right;"> 6.948 </td>
   <td style="text-align:right;"> 8.336 </td>
  </tr>
</tbody>
</table>
 
 

```r
t <- prepare_descriptive_table(df)

# Create a function to round the decimals of a df
round_df <- function(x, digits) {
    # round all numeric variables
    # x: data frame 
    # digits: number of digits to round
    numeric_columns <- sapply(x, mode) == 'numeric'
    x[numeric_columns] <-  round(x[numeric_columns], digits)
    x
}

round_df(t$df, 2)
```

```
##                 N      Mean  Std. dev.    Min.     25 %    Median      75 %
## id            108     54.50      31.32    1.00    27.75     54.50     81.25
## year          108   1990.00       0.00 1990.00  1990.00   1990.00   1990.00
## Y             108 364598.14 1030271.05 3067.00 19377.75  76730.50 234608.00
## K             108 962050.80 2873937.94 2004.00 31131.00 162679.00 713896.25
## pop           108     45.41     140.67    1.57     5.11     10.35     34.44
## L             108     19.51      69.00    0.70     2.06      4.35     12.22
## s             108      6.50       2.90    0.89     4.16      6.98      8.79
## alpha_it       90      0.43       0.11    0.15     0.35      0.43      0.49
## GDPpc         108   9784.00    9475.93  526.03  2103.65   6126.90  16453.11
## lp            108  23223.79   20088.83 1299.29  6294.13  16984.63  38834.15
## h             108      2.68       0.73    1.32     2.08      2.77      3.25
## kl            108  63403.23   68027.75  725.04  9651.04  35519.55 102058.92
## kp            108      0.61       0.61    0.21     0.33      0.46      0.63
## ky            108      2.28       0.99    0.20     1.60      2.18      3.04
## TFP           108    824.21     652.60  139.97   360.48    702.48   1053.21
## log_GDPpc_raw 108      8.61       1.18    6.27     7.65      8.72      9.71
## log_lp_raw    108      9.56       1.10    7.17     8.75      9.74     10.57
## log_ky_raw    108      0.70       0.56   -1.62     0.47      0.78      1.11
## log_h_raw     108      0.95       0.29    0.28     0.73      1.02      1.18
## log_tfp_raw   108      6.44       0.76    4.94     5.89      6.55      6.96
## log_GDPpc     108      8.54       1.19    6.16     7.57      8.68      9.60
## log_lp        108      9.52       1.12    7.05     8.63      9.70     10.39
## log_ky        108      0.74       0.59   -1.81     0.48      0.85      1.13
## log_h         108      0.95       0.29    0.27     0.73      1.03      1.18
## log_tfp       108      6.39       0.79    4.91     5.71      6.54      6.95
##                      Max.
## id                 108.00
## year              1990.00
## Y              9259567.00
## K             26453210.00
## pop               1154.61
## L                  637.07
## s                   12.20
## alpha_it             0.77
## GDPpc            37503.44
## lp               75036.34
## h                    4.25
## kl              255639.41
## kp                   5.07
## ky                   4.78
## TFP               4164.72
## log_GDPpc_raw       10.53
## log_lp_raw          11.23
## log_ky_raw           1.56
## log_h_raw            1.45
## log_tfp_raw          8.33
## log_GDPpc           10.49
## log_lp              11.23
## log_ky               1.57
## log_h                1.45
## log_tfp              8.34
```
 
 
 
### Histogram
 

```r
var <- as.numeric(dat$log_lp[dat$year == "1990"])
hist(var, main="", xlab = "log_lp", col="red", right = FALSE, breaks= 10)
```

<img src="expandr_files/figure-html/histogram-1.png" style="display: block; margin: auto;" />
 
 
### Extreme Observations
 

```r
t <- prepare_ext_obs_table(dat, n = 10,
                           cs_id = "country",
                           ts_id = "year",
                           var = "log_lp")
t$df
```

```
##           country year    log_lp
## 2700       Norway 2014 11.984427
## 2699       Norway 2013 11.958503
## 2698       Norway 2012 11.932280
## 2697       Norway 2011 11.905351
## 2696       Norway 2010 11.877324
## 2695 Saudi Arabia 2014 11.871549
## 2694       Norway 2009 11.847876
## 2693 Saudi Arabia 2013 11.820003
## 2692       Norway 2008 11.816551
## 2691      Ireland 2014 11.797161
## 10        Burundi 2005  7.249489
## 9         Burundi 2003  7.248836
## 8         Burundi 2004  7.247464
## 7      Mozambique 1996  7.219810
## 6      Mozambique 1995  7.187088
## 5      Mozambique 1994  7.156726
## 4      Mozambique 1993  7.128354
## 3      Mozambique 1992  7.101554
## 2      Mozambique 1991  7.075745
## 1      Mozambique 1990  7.050233
```
 
 
### By Group: Bar Graph
 

```r
df <- dat
df <- df[df$year == "1990", ]
prepare_by_group_bar_graph(df, "hi1990", "lp", mean, TRUE)$plot +
  ylab("mean lp")
```

<img src="expandr_files/figure-html/by_group_bar_graph-1.png" style="display: block; margin: auto;" />
 
 
### By group: Violin plot
 

```r
df <- dat
prepare_by_group_violin_graph(df, "region", "log_lp", TRUE)
```

<img src="expandr_files/figure-html/by_group_violin_graph-1.png" style="display: block; margin: auto;" />
 
 
### Trend Graph
 

```r
df <- dat
prepare_trend_graph(df, "year", c("lp"))$plot
```

<img src="expandr_files/figure-html/trend_graph-1.png" style="display: block; margin: auto;" />
 
 
### Quantile Trend Graph
 

```r
df <- dat
prepare_quantile_trend_graph(df, "year", c(0.05, 0.25, 0.5, 0.75, 0.95), "lp", points = FALSE)$plot
```

<img src="expandr_files/figure-html/quantile_trend_graph-1.png" style="display: block; margin: auto;" />
 
 
#### Custimized quantile trend graph  

```r
log_lp_raw <- prepare_quantile_trend_graph(dat, "year", c(0.05, 0.25, 0.5, 0.75, 0.95), "log_lp_raw", points = FALSE)$plot
```



```r
log_lp_raw <- log_lp_raw +
theme_minimal() +
  guides(color = guide_legend(reverse = TRUE)) +
  scale_color_discrete(name = "Quantile") +
  labs(x = "",
       y = "Log of Labor Productivity")
```

```
## Scale for 'colour' is already present. Adding another scale for 'colour',
## which will replace the existing scale.
```

```r
#ggsave("figs/quintiles_all_log_lp_raw.pdf", width = 6, height = 4)
log_lp_raw
```

<img src="expandr_files/figure-html/unnamed-chunk-7-1.png" style="display: block; margin: auto;" />

 
### Correlation Graph
 

```r
df <- dat
ret <- prepare_correlation_graph(df)
```

<img src="expandr_files/figure-html/corrplot-1.png" style="display: block; margin: auto;" />

```r
ret2 <- prepare_correlation_graph(df[, c(10, 11, 12, 13, 14, 15, 16)])
```

<img src="expandr_files/figure-html/corrplot-2.png" style="display: block; margin: auto;" />
 
 
### Scatter Plot
 

```r
df <- dat
df <- df[, c("country", "year", "log_lp", "log_GDPpc", "region", "pop")]
df <- df[complete.cases(df), ]
df$region <- as.factor(df$region)
set.seed(42)
df <- sample_n(df, 1000)
prepare_scatter_plot(df, "log_lp", "log_GDPpc", color = "region", size = "pop", loess = 1)
```

```
## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'
```

<img src="expandr_files/figure-html/scatter_plot-1.png" style="display: block; margin: auto;" />
 
 
### Regresssion Table
 

```r
df <- dat
df <- df[, c("log_lp", "log_ky", "log_h", "log_tfp", "country", "year", "hi1990")]
df <- df[complete.cases(df), ]
df$hi1990 <- as.factor(df$hi1990)
df <- droplevels(df)
t <- prepare_regression_table(df, dvs = "log_lp", idvs = c("log_ky", "log_h", "log_tfp"), feffects = c("country", "year"), clusters = c("country", "year"), byvar = "hi1990", models = "ols")
HTML(t$table)
```

<!--html_preserve--> <table style="text-align:center"><tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td colspan="3"><em>Dependent variable:</em></td></tr> <tr><td></td><td colspan="3" style="border-bottom: 1px solid black"></td></tr> <tr><td style="text-align:left"></td><td colspan="3">log_lp</td></tr> <tr><td style="text-align:left"></td><td>Full Sample</td><td>no</td><td>yes</td></tr> <tr><td style="text-align:left"></td><td>(1)</td><td>(2)</td><td>(3)</td></tr> <tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">log_ky</td><td>0.472<sup>***</sup></td><td>0.483<sup>***</sup></td><td>0.469<sup>***</sup></td></tr> <tr><td style="text-align:left"></td><td>(0.031)</td><td>(0.032)</td><td>(0.060)</td></tr> <tr><td style="text-align:left"></td><td></td><td></td><td></td></tr> <tr><td style="text-align:left">log_h</td><td>0.226<sup>*</sup></td><td>0.183</td><td>0.435<sup>***</sup></td></tr> <tr><td style="text-align:left"></td><td>(0.132)</td><td>(0.168)</td><td>(0.135)</td></tr> <tr><td style="text-align:left"></td><td></td><td></td><td></td></tr> <tr><td style="text-align:left">log_tfp</td><td>1.447<sup>***</sup></td><td>1.503<sup>***</sup></td><td>1.180<sup>***</sup></td></tr> <tr><td style="text-align:left"></td><td>(0.042)</td><td>(0.048)</td><td>(0.064)</td></tr> <tr><td style="text-align:left"></td><td></td><td></td><td></td></tr> <tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Estimator</td><td>ols</td><td>ols</td><td>ols</td></tr> <tr><td style="text-align:left">Fixed effects</td><td>country, year</td><td>country, year</td><td>country, year</td></tr> <tr><td style="text-align:left">Std. errors clustered</td><td>country, year</td><td>country, year</td><td>country, year</td></tr> <tr><td style="text-align:left">Observations</td><td>2,700</td><td>2,050</td><td>650</td></tr> <tr><td style="text-align:left">R<sup>2</sup></td><td>0.888</td><td>0.893</td><td>0.895</td></tr> <tr><td style="text-align:left">Adjusted R<sup>2</sup></td><td>0.883</td><td>0.887</td><td>0.886</td></tr> <tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td colspan="3" style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr> </table><!--/html_preserve-->
 
 

```r
df <- dat
df <- df[, c("log_lp", "log_ky", "log_h", "log_tfp", "country", "year", "hi1990")]
df <- df[complete.cases(df), ]
df$hi1990 <- as.factor(df$hi1990)
df <- droplevels(df)
t <- prepare_regression_table(df, dvs = "log_lp", idvs = c("log_ky", "log_h", "log_tfp"), feffects = c("country", "year"), clusters = c("country", "year"), byvar = "hi1990", models = "ols", format = "text")
t
```

```
## $models
## $models[[1]]
## $models[[1]]$model
## 
## Model Formula: log_lp ~ log_ky + log_h + log_tfp
## <environment: 0x7f7ed4df3178>
## 
## Coefficients:
##  log_ky   log_h log_tfp 
## 0.47182 0.22569 1.44690 
## 
## 
## $models[[1]]$type_str
## [1] "ols"
## 
## $models[[1]]$fe_str
## [1] "country, year"
## 
## $models[[1]]$cl_str
## [1] "country, year"
## 
## $models[[1]]$p
##        log_ky         log_h       log_tfp 
##  2.524332e-51  8.698143e-02 6.546745e-212 
## 
## $models[[1]]$se
##     log_ky      log_h    log_tfp 
## 0.03061389 0.13181026 0.04226128 
## 
## $models[[1]]$omit_vars
## NULL
## 
## $models[[1]]$byvalue
## [1] "Full Sample"
## 
## 
## $models[[2]]
## $models[[2]]$model
## 
## Model Formula: log_lp ~ log_ky + log_h + log_tfp
## <environment: 0x7f7edad8c7b8>
## 
## Coefficients:
##  log_ky   log_h log_tfp 
## 0.48272 0.18284 1.50339 
## 
## 
## $models[[2]]$type_str
## [1] "ols"
## 
## $models[[2]]$fe_str
## [1] "country, year"
## 
## $models[[2]]$cl_str
## [1] "country, year"
## 
## $models[[2]]$p
##        log_ky         log_h       log_tfp 
##  7.936025e-48  2.756112e-01 3.362078e-177 
## 
## $models[[2]]$se
##     log_ky      log_h    log_tfp 
## 0.03232132 0.16765703 0.04757119 
## 
## $models[[2]]$omit_vars
## NULL
## 
## $models[[2]]$byvalue
## [1] "no"
## 
## 
## $models[[3]]
## $models[[3]]$model
## 
## Model Formula: log_lp ~ log_ky + log_h + log_tfp
## <environment: 0x7f7ed3207ce8>
## 
## Coefficients:
##  log_ky   log_h log_tfp 
## 0.46857 0.43509 1.18037 
## 
## 
## $models[[3]]$type_str
## [1] "ols"
## 
## $models[[3]]$fe_str
## [1] "country, year"
## 
## $models[[3]]$cl_str
## [1] "country, year"
## 
## $models[[3]]$p
##       log_ky        log_h      log_tfp 
## 2.890378e-14 1.350437e-03 6.453452e-60 
## 
## $models[[3]]$se
##     log_ky      log_h    log_tfp 
## 0.06011667 0.13511032 0.06436493 
## 
## $models[[3]]$omit_vars
## NULL
## 
## $models[[3]]$byvalue
## [1] "yes"
## 
## 
## 
## $table
##  [1] ""                                                               
##  [2] "==============================================================="
##  [3] "                                 Dependent variable:           "
##  [4] "                      -----------------------------------------"
##  [5] "                                       log_lp                  "
##  [6] "                       Full Sample       no            yes     "
##  [7] "                           (1)           (2)           (3)     "
##  [8] "---------------------------------------------------------------"
##  [9] "log_ky                  0.472***      0.483***      0.469***   "
## [10] "                         (0.031)       (0.032)       (0.060)   "
## [11] "                                                               "
## [12] "log_h                    0.226*         0.183       0.435***   "
## [13] "                         (0.132)       (0.168)       (0.135)   "
## [14] "                                                               "
## [15] "log_tfp                 1.447***      1.503***      1.180***   "
## [16] "                         (0.042)       (0.048)       (0.064)   "
## [17] "                                                               "
## [18] "---------------------------------------------------------------"
## [19] "Estimator                  ols           ols           ols     "
## [20] "Fixed effects         country, year country, year country, year"
## [21] "Std. errors clustered country, year country, year country, year"
## [22] "Observations              2,700         2,050          650     "
## [23] "R2                        0.888         0.893         0.895    "
## [24] "Adjusted R2               0.883         0.887         0.886    "
## [25] "==============================================================="
## [26] "Note:                               *p<0.1; **p<0.05; ***p<0.01"
```
 
 
## References
 
- <https://joachim-gassen.github.io/ExPanDaR>
