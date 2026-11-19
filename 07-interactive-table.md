---
title: "An Interactive Table"
teaching: 15
exercises: 6
---

:::::::::::::::::::::::::::::::::::::: questions
- How do you build a summary table of OA indicators per publisher?
- How do you display it as an interactive, searchable table with the DT package?
- How do you adapt the table display?
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives
- Create a summary table of publications and OA share per publisher
- Display a data frame as an interactive table with `datatable()`
- Adjust the table with arguments such as `rownames`, `colnames` and `filter`
- Remove rows with missing values with `filter()` and `is.na()`
::::::::::::::::::::::::::::::::::::::::::::::::



## Why an interactive table?

A static table answers the questions you thought of in advance. An interactive table lets the
people who use the dashboard explore the data on their own: a colleague handling the APC payments can search for one
publisher or sort by the OA share. In a Quarto dashboard this needs no extra server, instead
interactive elements can run directly in the user's browser.

In this episode we fill the second page of the dashboard, "Publications", with an interactive
table.

## A summary table per publisher

The table is only as useful as the data it displays. A useful overview for OA monitoring could be one
row per publisher with the number of publications, the number of OA publications and the OA share.

To create such a table we will use some of the functions from the previous episodes: `group_by()` + `summarise()`, `sum()` and
`mean()` applied to the `is_oa` column, and `arrange()`.


``` r
oa_by_publisher <- oa_df |>
  group_by(publisher) |>
  summarise(
    publications = n(),
    oa_publications = sum(is_oa),
    oa_share = round(mean(is_oa) * 100, 1)
  ) |>
  arrange(desc(publications))

oa_by_publisher
```

``` output
# A tibble: 44 × 4
   publisher                       publications oa_publications oa_share
   <chr>                                  <int>           <int>    <dbl>
 1 Wiley                                   2566            2196     85.6
 2 Springer Science+Business Media         1294            1166     90.1
 3 Elsevier BV                              470             354     75.3
 4 Nature Portfolio                         394             378     95.9
 5 BioMed Central                           256             256    100  
 6 Springer Nature                          214             204     95.3
 7 American Chemical Society                124             108     87.1
 8 Cambridge University Press                97              79     81.4
 9 Royal Society of Chemistry                86              85     98.8
10 Springer VS                               52              50     96.2
# ℹ 34 more rows
```

`summarise()` can calculate several values at once: separate them with commas. The `n()` function counts the
rows in each group, and `desc()` inside `arrange()` sorts the largest number of publications first.

## An interactive table with DT

The `DT` package turns a data frame into an interactive table, with sorting, searching and paging
built in. We already loaded the package with `library(DT)` in our setup chunk at the top of our dashboard:

```r
#| include: false
library(tidyverse)
library(DT)

my_ror <- "https://ror.org/01y9bpm73"

oa_all <- read_csv("data/jct_oal_TA_dataset.csv")

oa_df <- oa_all |>
  filter(ror == my_ror) |>
  mutate(is_oa = oa_status != "closed")
```

Now we will replace the placeholder chunk on the "Publications" page. We want the chunk to calculate the summary
table and pass it to the `datatable()` function from the `DT` package:

```r
#| title: "Publications by Publisher"

oa_by_publisher <- oa_df |>
  group_by(publisher) |>
  summarise(
    publications = n(),
    oa_publications = sum(is_oa),
    oa_share = round(mean(is_oa) * 100, 1)
  ) |>
  arrange(desc(publications))

datatable(oa_by_publisher)
```

Render the dashboard and switch to the "Publications" page. Click a column header to sort or type
into the search box to test out the functionalities. 

The default table is already useful, but `datatable()` has arguments to adapt its display:

```r
datatable(
  oa_by_publisher,
  rownames = FALSE,
  colnames = c("Publisher", "Publications", "OA publications", "OA share (%)"),
  filter = "top"
)
```

- `rownames = FALSE` removes the row numbers
- `colnames` sets readable column headers, in the same order as the columns
- `filter = "top"` adds a filter field above each column

Notice the structure: arguments inside `datatable()` are separated by commas. 
Replace your `datatable(oa_by_publisher)` line with this version and render again.

::::::::::::::::::::::::::::::::::::: challenge
## Challenge: Missing publishers

The budget office wants to use the publisher overview for the annual OA report. But some articles
have no publisher in the data: in R, a missing value is written `NA`, and in our table these
articles are summed up in one row with an empty publisher field. The function `is.na()` tests 
whether a value is missing: it returns `TRUE` for every `NA`.

1. How many articles in `oa_df` have no publisher?
2. Change the code of your table, so that these articles are left out.

:::::::::::::::::::::::: hint
The exclamation mark `!` turns `TRUE` into `FALSE` and the other way round. Use both inside `filter()`
to keep only the rows where the publisher is *not* missing. Do this before `group_by()`.
:::::::::::::::::::::::::::::

:::::::::::::::::::::::: solution
## Solution

1. `sum()` of the `TRUE`/`FALSE` values counts how many articles have no publisher:

```r
sum(is.na(oa_df$publisher))
```

2. Add a `filter()` step before `group_by()`:

```r
oa_by_publisher <- oa_df |>
  filter(!is.na(publisher)) |>
  group_by(publisher) |>
  summarise(
    publications = n(),
    oa_publications = sum(is_oa),
    oa_share = round(mean(is_oa) * 100, 1)
  ) |>
  arrange(desc(publications))
```

In the dashboard, add the same `filter(!is.na(publisher))` line to the chunk on the "Publications"
page.
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::


::::::::::::::::::::::::::::::::::::: keypoints
- Prepare the data frame first (`group_by()` + `summarise()` + `arrange()`), then display it
- `DT::datatable()` turns a data frame into a sortable, searchable table
- Arguments like `rownames`, `colnames` and `filter` adapt the table appearance
- `is.na()` finds missing values; `filter(!is.na(column))` removes them
::::::::::::::::::::::::::::::::::::::::::::::::
