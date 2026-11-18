---
title: "Introduction to R and RStudio"
teaching: 26
exercises: 18
---

:::::::::::::::::::::::::::::::::::::: questions
- What are R and RStudio, and why does this lesson use them to build dashboard content?
- How do you create and work with values in R, using objects, functions, vectors and data frames?
- How do you install and load an R package?
- How can you use the pipe operator to chain dplyr verbs together for readable data wrangling?
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives
- Navigate the RStudio interface, create a project and run code
- Create objects and vectors, call functions, and build and access columns of a data frame
- Install packages with `install.packages()` and load them with `library()`
- Use the pipe operator `|>` together with `filter()`, `select()`, `mutate()`, `arrange()` and
  `group_by()` + `summarise()` to wrangle a data frame
::::::::::::::::::::::::::::::::::::::::::::::::

::: callout
### You might skip this episode if...
You already know how to work with R/RStudio: creating objects, calling functions, using vectors
and data frames, installing and loading packages, chaining steps with the pipe operator (`|>`) and using basic
dplyr verbs (`filter()`, `select()`, `mutate()`, `arrange()`, `group_by()` + `summarise()`).
Everything here is general-purpose R, not specific to Quarto dashboards.

If you skip this episode, please make sure that the packages `tidyverse` and `DT` are
installed (see [Installing packages](#installing-packages)) and that you have created an RStudio project.
:::

::: callout
### A note before we start
This episode teaches only the minimum R needed to read and adapt the dashboard code in this
lesson. If you want to learn R more thoroughly, Library Carpentry has a dedicated lesson:
[Introduction to R](https://librarycarpentry.github.io/lc-r/).
:::

## Why R for dashboards?

In the Markdown episode, we learned that a fenced code block becomes **executable** in Quarto when
you wrap the language name in curly braces: `` ```{r} `` instead of `` ```r ``. Every chart,
table and value in your dashboard can be the *output* of an R chunk like that. This episode teaches
just enough R for you to read, understand and adapt those chunks.

R is free, widely used for data analysis in research and libraries, and it has excellent tools for
exactly what an OA dashboard needs: reading data, calculating shares and counts, drawing charts
and building tables.

## RStudio

We write and run all our R code in **RStudio**, a free application (an *integrated development environment*, IDE) 
built for working with R. RStudio also works well with Quarto. When you open
RStudio, you typically see four panes:

- **Source / Script** (top left): where you edit files
- **Console** (bottom left): where code actually runs, one command at a time
- **Environment / History** (top right): lists the objects you have created, so you can see what
  currently exists
- **Files / Plots / Packages / Help** (bottom right): browse files, view plots, manage packages
  and read documentation

![Screenshot of RStudio with labels of the four panes. Image source: [Library Carpentry, Introduction to R](https://librarycarpentry.github.io/lc-r/00-before-we-start.html#the-rstudio-interface), licensed CC BY 4.0](https://librarycarpentry.github.io/lc-r/fig/RStudioPanes.png){alt="Screenshot of RStudio with labels of the four panes"}

### Create a new R project

An RStudio project keeps all files of one project (code, data, images) together in one
folder. We create one for our dashboard:

1. Go to `File > New Project...`
2. Choose `New Directory`, then `New Project`
3. Name your project (for example `oa-dashboard`) and pick a location to save it
4. Click `Create Project`

### Create a new Quarto document

1. Go to `File > New File > Quarto Document...`
2. Give it a title, leave the format on the default (HTML) and click `Create`
3. Go to `File > Save As...` (or press `Ctrl+S` / `Cmd+S`)
4. Name the file (e.g. `oa-dashboard.qmd`) and make sure it is saved in your **project folder**
   (the folder of your RStudio project). Click `Save`

The saved file should now appear in the `Files` pane at the bottom right.

RStudio fills the new document with some example Markdown text and two `` ```{r} `` code
chunks. We use this document to try things out in this episode, and we turn it into a dashboard
later. You can work with it like this:

- **Run a chunk:** click the green "play" arrow in the top-right corner of the chunk
- **Insert an empty chunk:** press `Ctrl+Alt+I` (Windows/Linux) or `Cmd+Option+I` (Mac), or use
  the green `Insert` button in the toolbar
- **Run a single line:** place the cursor on the line and press `Ctrl+Enter` (Windows/Linux) or
  `Cmd+Enter` (Mac)
- **Render the whole document:** click the `Render` button

The code in the rest of this episode can be typed into a chunk of this document or directly
into the Console.

## Objects

In R, you store a value by giving it a name with the **assignment operator** `<-`. Once an
object exists, you can reuse it by typing its name.


``` r
number_of_articles <- 5
number_of_articles
```

``` output
[1] 5
```

You can overwrite an object at any time by assigning a new value to the same name:


``` r
number_of_articles <- 8
number_of_articles
```

``` output
[1] 8
```

## Functions

A function takes some input (its **arguments**), does something with it, and returns a result.
You call a function by writing its name followed by round brackets containing the arguments.


``` r
round(3.14159, 2)
```

``` output
[1] 3.14
```

Here, `round()` is the function, `3.14159` is the value to round, and `2` tells it how many
decimal places to keep. Functions can also take more than one value at once:


``` r
sum(4, 8, 15)
```

``` output
[1] 27
```

::::::::::::::::::::::::::::::::::::: challenge
## Challenge 1: Objects and functions

Your institution published 1,382 articles last year, 847 of them open access. Create two objects,
`all_articles` and `oa_articles`, and use them to calculate the OA share in percent, rounded to
one decimal place.

:::::::::::::::::::::::: hint
The share in percent is `oa_articles / all_articles * 100`. Wrap the calculation in `round()`.
:::::::::::::::::::::::::::::

:::::::::::::::::::::::: solution
## Solution


``` r
all_articles <- 1382
oa_articles <- 847
round(oa_articles / all_articles * 100, 1)
```

``` output
[1] 61.3
```
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Vectors

A vector holds several values of the same data type in a single object. You create one with the
`c()` function ("combine"):


``` r
oa_status <- c("gold", "green", "closed")
oa_status
```

``` output
[1] "gold"   "green"  "closed"
```

You can pick out a single element using square brackets and its position:


``` r
oa_status[2]
```

``` output
[1] "green"
```

### Named vectors

A vector can also have a **name** attached to each value:


``` r
apcs <- c(gold = 2000, hybrid = 2500, diamond = 0)
apcs
```

``` output
   gold  hybrid diamond 
   2000    2500       0 
```

You can now pick out a value by its name instead of its position:


``` r
apcs["hybrid"]
```

``` output
hybrid 
  2500 
```

::::::::::::::::::::::::::::::::::::: challenge
## Challenge 2: Named vectors

For the charts in your dashboard you want one colour per OA status. Create a named vector called
`oa_colors` that assigns a colour to three OA statuses: `gold` should map to `"gold"`, `green`
to `"forestgreen"` and `closed` to `"grey"`. Then retrieve the colour for `green`.

We will use this vector again when we style the dashboard.

:::::::::::::::::::::::: solution
## Solution


``` r
oa_colors <- c(gold = "gold", green = "forestgreen", closed = "grey")
oa_colors["green"]
```

``` output
        green 
"forestgreen" 
```
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Data frames

A data frame stores data in rows and columns. Think of it as a small spreadsheet inside R. You can
build one with `data.frame()`. Here is a tiny example of six articles with their OA status and
number of citations:


``` r
articles <- data.frame(
  title = c("Paper A", "Paper B", "Paper C", "Paper D", "Paper E", "Paper F"),
  oa_status = c("gold", "green", "closed", "gold", "closed", "green"),
  citations = c(12, 5, 20, 8, 3, 9)
)
articles
```

``` output
    title oa_status citations
1 Paper A      gold        12
2 Paper B     green         5
3 Paper C    closed        20
4 Paper D      gold         8
5 Paper E    closed         3
6 Paper F     green         9
```

To access a single column, use the dollar sign `$` between the data frame's name and the
column's name:


``` r
articles$citations
```

``` output
[1] 12  5 20  8  3  9
```

Because `articles$citations` is a vector, you can pass it straight into a function:


``` r
mean(articles$citations)
```

``` output
[1] 9.5
```

::::::::::::::::::::::::::::::::::::: challenge
## Challenge 3: Data frames

Using the `articles` data frame, find the highest and the lowest number of citations among the six
articles. (Hint: besides `sum()` and `mean()`, R has the functions `max()` and `min()`.)

:::::::::::::::::::::::: solution
## Solution


``` r
max(articles$citations)
```

``` output
[1] 20
```

``` r
min(articles$citations)
```

``` output
[1] 3
```
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Packages

Not all functions come built into R. **Packages** are collections of extra functions that other
people have written. Using a package takes two steps:

1. **Install** it once on your computer, with `install.packages()`
2. **Load** it with `library()` at the start of every R session in which you want to use it

Think of it like a book: you buy it once (install), but you take it off the shelf every time you
want to read it (load).

### Installing packages {#installing-packages}

Type the `install.packages()` function directly into the **Console**, not into a document, 
because R would otherwise reinstall the packages every time you run or render the document:

```r
install.packages("tidyverse")
```

For our OA dashboard we need two packages:

| Package | What we use it for |
|---------|--------------------|
| `tidyverse` | wrangling data (`dplyr`), reading CSV files (`readr`) and drawing charts (`ggplot2`) |
| `DT` | interactive, searchable tables |

You can install both (or more) in one go by combining the names with `c()`:

```r
install.packages(c("tidyverse", "DT"))
```

Run this line in the Console now. The installation can take a few minutes and prints a lot of
text while it works; that is normal. Wait until the `>` prompt appears again in the Console.

### Loading packages

Loading happens inside the document itself, because clicking **Render** always starts a brand new
R session. That is why this line belongs in a chunk:


``` r
library(tidyverse)
```

``` output
── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
✔ dplyr     1.2.1     ✔ readr     2.2.0
✔ forcats   1.0.1     ✔ stringr   1.6.0
✔ ggplot2   4.0.4     ✔ tibble    3.3.1
✔ lubridate 1.9.5     ✔ tidyr     1.3.2
✔ purrr     1.2.2     
── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
✖ dplyr::filter() masks stats::filter()
✖ dplyr::lag()    masks stats::lag()
ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```

``` r
library(DT)
```

``` error
Error in `library()`:
! there is no package called 'DT'
```

Nothing visible happens when a package loads successfully (aside from some startup messages), but
afterwards extra functions are available that were not there before, like the `dplyr` verbs we will use 
in the next sections. We load `DT` already now, even though we only use it in a later episode. If both lines run
without an error, you know that both packages are available when needed. If R reports `there is no package called ‘DT’` (or another name), 
that package is not installed yet. Install it with `install.packages("DT")` and try again.

::: callout
### What is the tidyverse?
`tidyverse` is not a single package. It is a collection of several packages designed to work well
together, including `dplyr` (for wrangling data), `ggplot2` (for plotting) and `readr` (for reading
data files like CSVs). Loading `tidyverse` loads all of them at once.
:::

## The pipe operator

The pipe operator `|>` takes whatever is on its left and feeds it into the function on its right
as the *first* argument. This lets you chain several steps together, left to right, instead of
nesting functions inside each other. It is built into R itself (since R 4.1), so you do not need
to load any package to use it.


``` r
articles$citations |> mean() |> round(1)
```

``` output
[1] 9.5
```

This does exactly the same thing as `round(mean(articles$citations), 1)`, but it reads left to
right, in the order the steps actually happen. When you chain several steps, start a new line
after each pipe to keep the code readable:


``` r
articles$citations |>
  mean() |>
  round(1)
```

``` output
[1] 9.5
```

::: callout
### A note on `%>%`
You may come across a different-looking pipe, `%>%`, in other R code or tutorials. It comes from
the magrittr package and was the original pipe, before R got its own built-in version. It works
almost the same way. This lesson uses `|>` throughout, because it needs no additional package.
If you see `%>%` in someone else's code, read it the same way: "take what is on the left, and pass
it into what is on the right."
:::

::::::::::::::::::::::::::::::::::::: challenge
## Challenge 4: Using the pipe

Rewrite `round(sum(articles$citations), 0)` using the pipe operator `|>` instead of nesting the
two functions.

:::::::::::::::::::::::: solution
## Solution


``` r
articles$citations |> 
  sum() |> 
  round(0)
```

``` output
[1] 57
```
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## A few essential dplyr verbs

`dplyr` (loaded as part of the `tidyverse`) provides a small set of functions, often called
"verbs", for working with data frames. Combined with the pipe, they read almost like plain
English. We keep using the `articles` data frame from before.

**`filter()`** keeps only the rows that match a condition:


``` r
articles |>
  filter(oa_status == "gold")
```

``` output
    title oa_status citations
1 Paper A      gold        12
2 Paper D      gold         8
```

**`select()`** keeps only the columns you name:


``` r
articles |>
  select(title, citations)
```

``` output
    title citations
1 Paper A        12
2 Paper B         5
3 Paper C        20
4 Paper D         8
5 Paper E         3
6 Paper F         9
```

**`mutate()`** adds a new column (or changes an existing one):


``` r
articles |>
  mutate(is_oa = oa_status != "closed")
```

``` output
    title oa_status citations is_oa
1 Paper A      gold        12  TRUE
2 Paper B     green         5  TRUE
3 Paper C    closed        20 FALSE
4 Paper D      gold         8  TRUE
5 Paper E    closed         3 FALSE
6 Paper F     green         9  TRUE
```

::: callout
### `=` vs. `==` vs. `!=`
These look similar but mean different things:

- A single `=` inside a function call names an **argument**, e.g. `mutate(is_oa = ...)` creates a
  column called `is_oa`.
- A double `==` checks whether two values are **equal**, e.g. `oa_status == "gold"` asks "is
  `oa_status` equal to `"gold"`?" and returns `TRUE` or `FALSE`.
- `!=` means **not equal**: `oa_status != "closed"` is `TRUE` for every article that is not
  closed, in other words every open access article.

Writing `=` where you meant `==` is one of the most common early mistakes in R.
:::

**`arrange()`** sorts the rows by the values of a column in ascending order (wrap the column in
`desc()` for descending order):


``` r
articles |>
  arrange(citations)
```

``` output
    title oa_status citations
1 Paper E    closed         3
2 Paper B     green         5
3 Paper D      gold         8
4 Paper F     green         9
5 Paper A      gold        12
6 Paper C    closed        20
```


``` r
articles |>
  arrange(desc(citations))
```

``` output
    title oa_status citations
1 Paper C    closed        20
2 Paper A      gold        12
3 Paper F     green         9
4 Paper D      gold         8
5 Paper B     green         5
6 Paper E    closed         3
```

**`group_by()` + `summarise()`** is a pair you will use a lot. `group_by()` splits the data into
groups, and `summarise()` then calculates one or more summary values per group:


``` r
articles |>
  group_by(oa_status) |>
  summarise(average_citations = mean(citations))
```

``` output
# A tibble: 3 × 2
  oa_status average_citations
  <chr>                 <dbl>
1 closed                 11.5
2 gold                   10  
3 green                   7  
```

::::::::::::::::::::::::::::::::::::: challenge
## Challenge 5: Filter and select

Your colleague from the repository team asks for the green OA articles. Using the `articles` data
frame, keep only the articles with `oa_status` equal to `"green"`, and show only their `title`
and `citations` columns.

:::::::::::::::::::::::: solution
## Solution


``` r
articles |>
  filter(oa_status == "green") |>
  select(title, citations)
```

``` output
    title citations
1 Paper B         5
2 Paper F         9
```
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge
## Challenge 6: Group and summarise

Your library director wants to know how many articles there are of each `oa_status`. Using the `articles`
data frame, calculate this. (Hint: `n()` counts the rows in each group. Try
`summarise(count = n())`.)

:::::::::::::::::::::::: solution
## Solution


``` r
articles |>
  group_by(oa_status) |>
  summarise(count = n())
```

``` output
# A tibble: 3 × 2
  oa_status count
  <chr>     <int>
1 closed        2
2 gold          2
3 green         2
```
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints
- Only `` ```{r} `` chunks are executed by Quarto, not `` ```r `` ones; chunks generate dashboard
  content
- Objects are created with `<-`, functions are called as `name(arguments)`, and vectors (`c()`)
  and data frames (`$` accesses a column) store several values
- Install packages once with `install.packages()` in the Console; load them in every session with
  `library()` inside the document
- The pipe `|>` chains steps left to right instead of nesting functions
- `filter()`, `select()`, `mutate()`, `arrange()` and `group_by()` + `summarise()` cover most
  day-to-day data wrangling
::::::::::::::::::::::::::::::::::::::::::::::::
