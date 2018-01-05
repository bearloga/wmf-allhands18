# Plotting the Course Through Charted Waters

> Heat maps, stacked area plots, mosaic plots, choropleths – oh my! There are so many different ways to visually convey relationships and patterns in data! While there are many tutorials on making them, there are very few for learning to read and understand them. In this workshop on data visualization literacy, you'll learn to recognize many popular types of charts and how to glean insights from them.

To be given at [Wikimedia Foundation's All Hands 2018](https://office.wikimedia.org/wiki/All_Hands/2018)'s [third workshop session](https://office.wikimedia.org/wiki/All_Hands/2018/Workshops) (3:15P-4:00P) in the Cypress room.

## Setup

This workshop uses the [learnr](https://rstudio.github.io/learnr/) and [rmarkdown](http://rmarkdown.rstudio.com/) packages from [RStudio](https://www.rstudio.com/) to create [an interactive web application](https://bearloga.shinyapps.io/wmf-allhands18/) and [a static web page](https://bearloga.github.io/wmf-allhands18/).

```R
install.packages(c("tidyverse", "devtools", "learnr"))
# Visualizations
devtools::install_github("tidyverse/ggplot2")
install.packages(c("cowplot", "waffle", "RColorBrewer"))
# Data
install.packages("pageviews")
```
