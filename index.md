---
title       : Baltimore Murders by neighborhoods
subtitle    : Developing Data Products week 4
author      : Adrian Martin
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Reproducible Project Pitch

* My Shiny App shows the distribution of homicides in Baltimore neighborhoods, from 2011 to 2015.
* I pulled the data from the Baltimore City website, which you can access at data.baltimorecity.gov.



--- .class #id 

## Pulling the data

* Baltimore uses the company Socrata to manage their data portal.
* You simply find a data view on the city website that you like, and use the RSocrata package to pull the data from that view

```r
install.packages("RSocrata")
library(RSocrata)
murders <- read.socrata("https://data.baltimorecity.gov/Public-Safety/HOMICIDES/gzf8-pc84")
```

---

## Looking at the data

* The data has the date and time of the crime, the "CrimeCode" (the number refers to the type of crime), the address of the time (aggregated to the block), the description of the crime, whether it occured inside or outside, the weapon used in the crime, the Baltimore Police patrolling district and sub-district (post), the neighborhood in which the crime occured, the GPS location, and the total number of incidents referred to by this line of data (which was 1 for all lines in this dataset).

* Here's the first line of the data (later, I extract the GPS coordinates to put lat and lon in their own columns).


```r
murders[1,]
```

```
##    CrimeDate CrimeTime CrimeCode      Location Description Inside.Outside
## 1 2011-01-01       136        1K 1500 HAZEL ST    HOMICIDE        Outside
##   Weapon Post District Neighborhood          Location.1 Total.Incidents
## 1  KNIFE  911 SOUTHERN   Curtis Bay (39.226, -76.58984)               1
```



---

## Exploring the data

* Most Baltimore neighborhoods have experienced one or fewer homicides per year in the past five years.
* The distribution of homicides is very unequal - it is a long-tail distribution.
![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5-1.png)

---

## Showing the data with a Shiny App

* For my Shiny App, I ask the user to select one of the one hundred and ninety nine neighborhoods in Baltimore that has experienced a homicide in the past five years. Note that there are almost a hundred Baltimore neighborhoods that do not appear in the data as they did not experience any homicides in this time period.
* That value is passed to "input$nhood", and is used to subset the data used for the table calculation and displayed in the leaflet map.
* Have fun poking around!
