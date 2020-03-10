---
title: Is Roulette the Best Way to Make Money?
author: 'Vincent Oktavianus'
date: '2020-03-08'
slug: play-a-roulette
categories:
  - R
tags:
  - R
subtitle: 'Simulation of a French Roulette'
summary: 'We will try to make a roulette simulation and investigate whether we will make a profit or loss by playing randomly'
authors: ["vincent"]
lastmod: '2020-03-08T19:27:02-05:00'
featured: no
image: 
  caption: 'Image credit: [**Macau Photo Agency**](https://unsplash.com/photos/as5EWdBWKqk)'
  focal_point: ""
  placement: 2
  preview_only: false
---

***

# Load library


```r
library(tidyverse)
```

```
## ── Attaching packages ───────────────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──
```

```
## ✔ ggplot2 3.2.1     ✔ purrr   0.3.3
## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
## ✔ tidyr   1.0.0     ✔ stringr 1.4.0
## ✔ readr   1.3.1     ✔ forcats 0.4.0
```

```
## ── Conflicts ──────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
library(kableExtra)
```

```
## 
## Attaching package: 'kableExtra'
```

```
## The following object is masked from 'package:dplyr':
## 
##     group_rows
```

- A French roulette has:
  - **37 colored and numbered pockets** on the wheel.
  - 0 is <span style="color:green">green</span>.
  - In number ranges from 1 to 10 and 19 to 28: 
    - Odd numbers are <span style="color:red">red</span>.
    - Even numbers are <span style="color:black">black</span>.
  - In ranges from 11 to 18 and 29 to 36:
    - Odd numbers are <span style="color:black">black</span>.
    - Even numbers are <span style="color:red">red</span>.


```r
wheel = read.csv("https://nkha149.github.io/stat385-sp2020/files/data/roulette.csv")
wheel = as_tibble(wheel)
kable(wheel) %>%
  kable_styling(bootstrap_options = "striped", full_width = FALSE)
```

<table class="table table-striped" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:right;"> number </th>
   <th style="text-align:left;"> color </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:left;"> green </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> red </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:left;"> black </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> red </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> black </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:left;"> red </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:left;"> black </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:left;"> red </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:left;"> black </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:left;"> red </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:left;"> black </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:left;"> black </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:left;"> red </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:left;"> black </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:left;"> red </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:left;"> black </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:left;"> red </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:left;"> black </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 18 </td>
   <td style="text-align:left;"> red </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:left;"> red </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:left;"> black </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 21 </td>
   <td style="text-align:left;"> red </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 22 </td>
   <td style="text-align:left;"> black </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 23 </td>
   <td style="text-align:left;"> red </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 24 </td>
   <td style="text-align:left;"> black </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 25 </td>
   <td style="text-align:left;"> red </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 26 </td>
   <td style="text-align:left;"> black </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:left;"> red </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 28 </td>
   <td style="text-align:left;"> black </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:left;"> black </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:left;"> red </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 31 </td>
   <td style="text-align:left;"> black </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 32 </td>
   <td style="text-align:left;"> red </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 33 </td>
   <td style="text-align:left;"> black </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 34 </td>
   <td style="text-align:left;"> red </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 35 </td>
   <td style="text-align:left;"> black </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 36 </td>
   <td style="text-align:left;"> red </td>
  </tr>
</tbody>
</table>

- We will write an R function named `roulette()` that simulate a roulette, that is it has:
  - **Input**: 2 arguments `bet` and `amount`
    - `bet`: argument that takes one of the following options:
      - `low`(1-18) or `high` (19-36): A bet that the number will be in the chosen range.
      - `red` or `black`: A bet that the number will be the chosen color.
      - `even` or `odd`: A bet that the number will be of the chosen type.
      - `first` or `second` or `third`: A bet that the number will be in the chosen dozen: first (1-12), second (13-24), or third (25-36).
      - any number from 0 to 36.
    - `amount`: amount in dollars that you want to bet on. The default value for `amount` is `1`.
  - **Output**:
    - The amount of money you gain/lose after the bet.
      - `-amount` if you lose the bet.
      - The amount of money win is calculated following the table below.
      - Make sure to include the `$` dollar sign.

| Bet Name                      | Winning spaces                                                     | Payout  |
| :---------------------------- | :----------------------------------------------------------------- | :------ |
| Straight up (a single number) | Any single number                                                  | 36 to 1 |
| Low (1 to 18)                 | 1, 2, 3, ..., 18                                                   | 1 to 1  |
| High (19 to 36)               | 19, 20, 21, ..., 36                                                | 1 to 1  |
| Red                           | 1, 3, 5, 7, 9, 12, 14, 16, 18, 19, 21, 23, 25, 27, 30, 32, 34, 36  | 1 to 1  |
| Black                         | 2, 4, 6, 8, 10, 11, 13, 15, 17, 20, 22, 24, 26, 28, 29, 31, 33, 35 | 1 to 1  |
| Odd                           | 1, 3, 5, ..., 35                                                   | 1 to 1  |
| Even                          | 2, 4, 6, ..., 36                                                   | 1 to 1  |
| 1st dozen                     | 1 through 12                                                       | 2 to 1  |
| 2nd dozen                     | 13 through 24                                                      | 2 to 1  |
| 3rd dozen                     | 25 through 36                                                      | 2 to 1  |

---

## Simulations and Graphing


```r
roulette = function(bet, amount = 1) {
  pick = sample(x = 0:(nrow(wheel) - 1), size = 1, replace = TRUE)
  low = 1:18
  high = 19:36
  red = c(1, 3, 5, 7, 9, 12, 14, 16, 18, 19, 21, 23, 25, 27, 30, 32, 34, 36)
  odd = seq(from = 1, to = 36, by = 2)
  black = c(2, 4, 6, 8, 10, 11, 13, 15, 17, 20, 22, 24, 26, 28, 29, 31, 33, 35)
  even = seq(from = 2, to = 36, by = 2)
  first = 1:12
  second = 13:24
  third = 25:36
  if ((pick %in% low) & (bet == "low")) {
    amount
  } else if ((pick %in% high) & (bet == "high")) {
    amount
  } else if ((bet == "red") & (pick %in% red)) {
    amount
  } else if ((bet == "black") & (pick %in% black)) {
    amount
  } else if ((bet == "even") & (pick %in% even)) {
    amount
  } else if ((bet == "odd") & (pick %in% odd)) {
    amount
  } else if ((bet == "first") & (pick %in% first)) {
    amount * 2
  } else if ((bet == "second") & (pick %in% second)) {
    amount * 2
  } else if ((bet == "third") & (pick %in% third)) {
    amount * 2
  } else if (pick == bet) {
    amount * 36
  } else {
    amount * -1
  }
}
```

- We want to estimate the **probability of winning if we keep betting on `red`**. To do that, we use simulation studies, that is running the `roulette()` function many many times and record the number of times we win (not have a negative total amount at the end of the game). The number of simulations `n` is 5000.
  

```r
set.seed(385)
results = replicate(roulette(bet = "red", amount = 1), n = 5000)
length(results[results == 1]) / 5000 # probability of winning if we keep betting on red
```

```
## [1] 0.4862
```

- Similarly, we want estimate the **probability of winning if we keep betting on the `first` dozen**. The number of simulations `n` is 5000.


```r
set.seed(385)
results2= replicate(roulette(bet = "first"), n = 5000)
length(results2[results2 == 2]) / 5000 # probability of winning if we keep betting on the first dozen
```

```
## [1] 0.3114
```

- Now, we want to estimate the **expected value of amount of money we will have by the end of the game if we bet on `red` with $1**. We will do the simulations for 10000 times where `n` = 10,000.


```r
set.seed(385)
results3 = replicate(roulette(bet = "red", amount = 1), n = 10000)
1 * length(results3[results3 == 1]) / 10000 - 1 * (1 - length(results3[results3 == 1]) / 10000) # expected value of amount of money we will have by the end of the game if we bet on odd with $5
```

```
## [1] -0.0248
```

## Interpretation of Results

It helps to remember the meaning of expected value to interpret the results of this calculation. The expected value is basically, a measurement of the average. It indicates what will happen in the long run every time that we bet $1 on red.

While we might win several times in a row in the short term, in the long run we will lose over 2 cents on average each time that we play. The presence of the 0 and 00 spaces are just enough to give the house a slight advantage. This advantage is so small that it can be difficult to detect, but in the end, the player always loses
