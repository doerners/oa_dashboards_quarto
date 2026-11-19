---
title: "Starting with OA Data"
teaching: 25
exercises: 8
---

:::::::::::::::::::::::::::::::::::::: questions
- How do you organise data files within a project?
- How do you load the packages and data needed for the dashboard?
- How do you get a first overview of a dataset?
- How do you filter a dataset down to a single institution?
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives
- Organise a data file within a project's folder structure
- Load the packages and data needed to work with the dataset
- Get a first overview of a dataset's size, columns and values
- Filter a dataset to a single institution and add a column that records whether an article is
  open access
::::::::::::::::::::::::::::::::::::::::::::::::

## The dataset

Throughout the rest of this lesson, we work with a single CSV file, `jct_oal_TA_dataset.csv`.
It contains data on articles that were published under transformative agreements, enriched with
bibliographic metadata from OpenAlex. Each row is one article. Here is what each column means:

| column name | description |
|-------------|-------------|
| openalex_id | article identifier given within the OpenAlex database |
| doi | digital object identifier of the article |
| issn_l | linking ISSN of the journal the article was published in |
| ror | Research Organization Registry (ROR) identifier of the corresponding author's affiliated institution |
| esac_id | ESAC Registry identifier for the transformative agreement under which the article was published |
| start_date | transformative agreement term start date |
| end_date | transformative agreement term end date |
| publication_date | article publication date |
| oa_status | article open access status |
| field | primary subject field the article was assigned to, based on the classification of OpenAlex |
| domain | primary subject domain the article was assigned to, based on the classification of OpenAlex |
| institution | display name of the corresponding author's affiliated institution |
| publisher | display name of the publisher the article was published by |
| publication_year | article publication year |

## Organising your data

Before loading data into our dashboard, create a `data` folder inside your project. You can do this by clicking
the **New Folder** button in the Files pane (bottom right in RStudio), or by running this command once in the **Console**:

```r
dir.create("data")
```

Then move `jct_oal_TA_dataset.csv` into that new folder, for example by dragging it there in your file manager.

## Loading packages and data

We now work inside the dashboard document, in a chunk at the very top, **above the first heading** but below the YAML header. This chunk is for setup only, so it should not show anything on the dashboard. The chunk option to achieve this is `#| include: false`. 
Insert a new chunk (**Ctrl+Alt+I** / **Cmd+Option+I**) with the `#| include: false` option:


```r
#| include: false
library(tidyverse)
library(DT)

oa_all <- read_csv("data/jct_oal_TA_dataset.csv")
```




The code has two parts. First, the two `library()` lines load the packages we installed in the
R episode: `tidyverse` gives us `read_csv()` for reading CSV files and the dplyr verbs from the
previous episode, and `DT` is needed for creating an interactive table later. 
Second, `read_csv()` reads the dataset into the object `oa_all`.

The path `"data/jct_oal_TA_dataset.csv"` is *relative*, this means RStudio looks for the `data` folder
next to your `.qmd` file in the project directory. Run the chunk with the green play arrow so that the 
objects exist in your RStudio session. 

## A first look at the data

A full data exploration and cleaning step is its own topic and beyond the scope of this lesson.
We will only get a general overview of the contents of the dataset and do minimal data preparation. 
We do not want the data preparation output displayed in our dashboard, so run the following lines 
in the RStudio **Console**, not in a code chunk of your dashboard.

The `dim()` function tells you the dimensions of your data frame, that is how many rows and columns you
are working with.


``` r
dim(oa_all)
```

``` output
[1] 231695     14
```

The `glimpse()` function shows the number of rows, every column name, each columns data type and a preview of its first values,
all in one compact view.


``` r
glimpse(oa_all)
```

``` output
Rows: 231,695
Columns: 14
$ openalex_id      <chr> "https://openalex.org/W3139948516", "https://openalex…
$ doi              <chr> "10.1145/3664595", "10.1145/3663666", "10.1021/acs.bi…
$ issn_l           <chr> "0360-0300", "1549-6325", "1525-7797", "1044-0305", "…
$ ror              <chr> "https://ror.org/01111rn36", "https://ror.org/01a77tt…
$ esac_id          <chr> "acm2022crui", "acm2023jisc", "acs2020crui", "acs2020…
$ start_date       <date> 2022-05-01, 2023-01-01, 2020-01-01, 2020-01-01, 2020…
$ end_date         <date> 2025-12-31, 2025-12-31, 2023-12-31, 2023-12-31, 2023…
$ publication_date <date> 2024-05-11, 2024-05-07, 2020-02-03, 2020-02-18, 2020…
$ oa_status        <chr> "bronze", "bronze", "hybrid", "hybrid", "hybrid", "cl…
$ field            <chr> "Psychology", "Engineering", "Medicine", "Medicine", …
$ domain           <chr> "Social Sciences", "Physical Sciences", "Health Scien…
$ institution      <chr> "University of Bologna", "University of Warwick", "Un…
$ publisher        <chr> "Association for Computing Machinery", "Association f…
$ publication_year <dbl> 2024, 2024, 2020, 2020, 2020, 2021, 2022, 2023, 2023,…
```

The `count()` function counts how often each value, or each combination of values, appears in
the columns you provide. With the `sort = TRUE` argument the output is ordered by the most common values. 
We will provide the institution and the ror column names to the `count()` function. 
This gives us an overview of how many articles per institution are in our dataset.


``` r
oa_all |>
  count(institution, ror, sort = TRUE)
```

``` output
# A tibble: 46 × 3
   institution                                 ror                           n
   <chr>                                       <chr>                     <int>
 1 University of Göttingen                     https://ror.org/01y9bpm73  5992
 2 University of Liverpool                     https://ror.org/04xs57h96  5968
 3 Queen Mary University of London             https://ror.org/026zzn846  5895
 4 Newcastle University                        https://ror.org/01kj2bm70  5872
 5 The University of Queensland                https://ror.org/00rqy9422  5869
 6 Tampere University                          https://ror.org/033003e23  5822
 7 University Medical Center Hamburg-Eppendorf https://ror.org/01zgy1s35  5822
 8 University of Glasgow                       https://ror.org/00vtgdb53  5812
 9 Wageningen University & Research            https://ror.org/04qw24q55  5812
10 University of Turku                         https://ror.org/05vghhr25  5772
# ℹ 36 more rows
```

## Filtering for a single institution

The lesson dataset contains data for 46 institutions, but we want our dashboard to monitor open access for just *one* institution. We identify an institution by its ROR ID (Research Organization Registry identifier), which is unambiguous, unlike institution names that can be spelled in many ways. As an example we use the University of Göttingen.

Back in the **setup chunk of our dashboard**, we store the ROR ID of the University of Göttingen in an object and filter the data based on it:

```r
#| include: false
library(tidyverse)
library(DT)

oa_all <- read_csv("data/jct_oal_TA_dataset.csv")

my_ror <- "https://ror.org/01y9bpm73"

oa_df <- oa_all |>
  filter(ror == my_ror)
```



Keeping the ROR in its own object `my_ror` at the top has the advantage that to build the dashboard
for another institution, you change **one line** and render again. 

`oa_df` is the data frame the rest of lesson works with. 

## Identifying open access articles

For our OA monitoring dashboard, we want to know which articles are open access and which ones are closed. The column
`oa_status` tells us how an article is available. We can use the `count()` function again to check which OA status values occur in the data:


``` r
oa_df |>
  count(oa_status, sort = TRUE)
```

``` output
# A tibble: 6 × 2
  oa_status     n
  <chr>     <int>
1 hybrid     3500
2 gold       1121
3 closed      728
4 green       374
5 bronze      231
6 diamond      38
```

The column names the open access route (for example gold, hybrid or green) or says
that an article is closed. But there is no column that directly answers the question: Is
this article open access, yes or no? We will create such a column now.

For this lesson an article is considered open access if its OA status is anything except
"closed". We record this in a new column `is_oa`, to which we assign the value `TRUE` for OA articles and the value `FALSE` 
for closed ones. To create the new column we add the `mutate()` function to our setup chunk:

```r
#| include: false
library(tidyverse)
library(DT)

oa_all <- read_csv("data/jct_oal_TA_dataset.csv")

my_ror <- "https://ror.org/01y9bpm73"

oa_df <- oa_all |>
  filter(ror == my_ror) |>
  mutate(is_oa = oa_status != "closed")
```




## Saving a dataset

We can save `oa_df`, now including the new column, as a CSV file, for example to share it with
a colleague, or in case we do not want to load the bigger file every time our dashboard renders.
This can be especially relevant if we work with a lot of data. We use the `write_csv()`
function, which takes the data frame and a file path:


``` r
write_csv(oa_df, file = "data/oa_df_goettingen.csv")
```

After running this line in a code chunk or in the **Console**, a new file `oa_df_goettingen.csv`
appears in the `data` folder of your project.

::::::::::::::::::::::::::::::::::::: challenge
## Challenge: A list of gold OA articles

A colleague from the library wants a list of all gold open access articles of your institution.
She only needs three columns: `doi`, `publisher` and `publication_year`. Create this list from
`oa_df` and save it as a CSV file in the `data` folder, so that you can send it to her.

:::::::::::::::::::::::: hint
Use three steps one after another: `filter()` for the rows with `oa_status` equal to `"gold"`,
`select()` for the columns, and `write_csv()` to save the result.
:::::::::::::::::::::::::::::

:::::::::::::::::::::::: solution
## Solution

```r
oa_gold <- oa_df |>
  filter(oa_status == "gold") |>
  select(doi, publisher, publication_year)

write_csv(oa_gold, file = "data/gold_articles_goettingen.csv")
```

Remember to put `"gold"` in quotation marks and to use two equal signs (`==`) for comparing.
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::


::::::::::::::::::::::::::::::::::::: keypoints
- `dim()`, `glimpse()` and `count()` give a general overview of a new dataset
- `filter()` narrows the data to the rows you need
- `mutate()` adds or modifies a column
- `write_csv()` saves a data frame as a CSV file
::::::::::::::::::::::::::::::::::::::::::::::::
