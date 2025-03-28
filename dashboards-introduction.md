---
title: "Quarto Dashboards"
teaching: 0
exercises: 0
---

:::::::::::::::::::::::::::::::::::::: questions 

- What are Quarto Dashboards?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explain the components of a Quarto Dashboards

::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::: challenge
### Exercise Title
Add a new row to your dashboard containing a value box that dispalys the total number of publications for the year 2024.

::::::::::::::::: solution
### Solution

``` markdown

## Row

```

``` r
|# content: valuebox

n_distinct(unigoe$id[unigoe$publication_year==2024])

```

::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints 

- tba

::::::::::::::::::::::::::::::::::::::::::::::::
