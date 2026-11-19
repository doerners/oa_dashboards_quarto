---
title: "Building the Dashboard Skeleton"
teaching: 20
exercises: 12
---

:::::::::::::::::::::::::::::::::::::: questions
- How do you turn a Quarto document into a dashboard?
- How do you combine pages, rows and columns to lay out a dashboard?
- How do you create value boxes and a card with an image?
- How do you check that the structure works before adding real content?
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives
- Turn an existing Quarto document into a dashboard
- Use Markdown headings to define pages, rows and columns
- Create value boxes with placeholder values
- Add a card with an image and text using Markdown
- Render the dashboard and confirm its structure before adding actual content
::::::::::::::::::::::::::::::::::::::::::::::::

## Why build a skeleton first?

Before we add real-world data, we build a dashboard **skeleton**: a dashboard that already has its complete
structure (two pages, rows, columns, value boxes and cards) but only placeholder content. This has
two advantages. You can focus entirely on getting the *layout* right, without debugging data or
code at the same time. And once the skeleton renders correctly, later episodes can focus on creating
the content.

The finished skeleton will have the following elements:

| Page | Row | Content |
|------|-----|---------|
| Overview | Row 1 | three value boxes: Total Publications, Open Access Publications, Open Access Share |
| Overview | Row 2 | left column: a chart card; right column: a card with an image and text |
| Publications | Row 1 | a card for a table |

## Turning your document into a dashboard

You already have a Quarto document from the previous episode. A dashboard is the same kind of
file with a different output format, so we keep the file and only change what it produces.

1. Open your Quarto document in RStudio (if it is not open yet, double-click it in the `Files` pane)
2. In the YAML header, change `format: html` to `format: dashboard`
3. Click `Render` and look at the result

Quarto now shows the same example text and the output of the code chunks as cards in a
dashboard layout. A dashboard is built from **cards**, and each piece of content ends up in
one of them. To build our OA monitoring dashboard, we start with an empty document. Delete all 
content below the YAML header.

Your file should now look like this:

```yaml
---
title: "Open Access Monitoring Dashboard"
format: dashboard
---
```

## Pages

A level 1 heading creates a page, shown as a tab in the navigation bar. 
We create our first page called "Overview" below the YAML header:

```markdown
# Overview
```

## Rows

Within a page, level 2 headings create rows. Add one, and control how much vertical space it
takes with the `height` attribute in curly brackets:

```markdown
# Overview

## Row {height="20%"}
```

The word "Row" is a label for humans reading the source; Quarto does not print it. What
creates a row is the heading *level* (`##`) together with the dashboard's orientation. You could
write `## Anything` and get the same result, but writing "Row" or "Column" makes the file easier
to read. `height` only matters relative to *other* rows on the same page, so it will start to
have a visible effect once we add a second row.

::: callout
### An empty row shows nothing
A row without content is not displayed. If you render now, you will not see any change. Rows
only appear once you put something inside them.
:::

## Value boxes

We want to fill our first row with three value boxes. A value box is a code chunk with the chunk option
`#| content: valuebox`. Chunk options are special comment lines starting with `#|` at the very top
of a chunk. Each sets one `key: value` pair. Here, `title`, `icon` and `color` style the box:

````markdown
# Overview

## Row {height="20%"}

```{r}
#| content: valuebox
#| title: "Total Publications"
#| icon: book
#| color: primary

"1,234"
```

```{r}
#| content: valuebox
#| title: "Open Access Publications"
#| icon: unlock
#| color: success

"765"
```

```{r}
#| content: valuebox
#| title: "Open Access Share"
#| icon: percent
#| color: warning

"62%"
```
````

The last line of each chunk is the value that the box displays. For now, these are placeholder
texts in quotation marks; in a later episode we replace them with calculated values.

Click **Render**. You should see three value boxes on the "Overview" page.

::: callout
### Icons and colours
The `icon` option accepts any name from the [Bootstrap Icons](https://icons.getbootstrap.com/)
library. Browse it, find an icon that fits and use its name, for example `icon: unlock`. The
`color` option accepts a set of colour names (`primary`, `success`, `warning`, ...) that we meet
again in the styling episode. For all value box options, see
[Dashboard Data Display](https://quarto.org/docs/dashboards/data-display.html#value-boxes) in the
Quarto documentation.
:::

## Columns and cards

A level 3 heading (`###`) nested below a level 2 heading splits that row into **columns**. Add a
second row with two columns. The left column gets a placeholder for our chart. Remember: every
code chunk's output automatically becomes its own card, and a chunk option `title` gives the
card a title:

````markdown
## Row {height="80%"}

### Column {width="65%"}

```{r}
#| title: "Open Access Share by Publication Year"

"The chart will go here."
```
````

The `width` attribute works for columns like `height` does for rows. When you click **Render** you should
see a wide card with your placeholder text below the value boxes.

## A card with an image and text

Cards are not limited to code output. A card that contains Markdown is created with a `.card`
div: a block that starts and ends with three colons. You have already learned the image syntax in
the Markdown episode. We now add a `.card` div to the right column with an image taken from the German Open Access Monitor, including alt
text:

````markdown
### Column {width="35%"}

::: {.card title="Creative Commons Licences"}

![CC Licences per OA Status, source: [German Open Access Monitor](https://open-access-monitor.de/open-access)](fig/oam_cc_licences.png)

:::
````

Render again. The page now has two rows, and the second row has two columns side by side.

## A second page

Now we want to add our second page "Publications" with one placeholder card for the table we build later:

````markdown
# Publications

## Row

```{r}
#| title: "Publications by Publisher"
"The table will go here."
```
````

After rendering, check that the navigation bar shows two page tabs, "Overview" and
"Publications". The skeleton now contains every component we want in our dashboard.

::: callout
### What about making it look good?
We have not touched colours, fonts or themes. Everything you see is default Quarto styling. That
is deliberate: a later episode is dedicated to styling. For now, the goal is a correct structure.
:::

::::::::::::::::::::::::::::::::::::: challenge
## Challenge 1: A text card about OA routes

Your dashboard should explain the most common routes to open access, so that colleagues who
are new to the topic can understand the numbers. Add a card below the ‘About Open Access’ card 
in the right column of your Overview page that has:

- the title "OA publishing routes"
- a list of three routes, each with its name in **bold**:
  - Gold: published open access in an OA journal
  - Green: a copy is deposited in a repository
  - Hybrid: open access article in a subscription journal
- a link to the [Directory of Open Access Journals](https://doaj.org) with a short sentence
  such as "Find open access journals and articles"

Render your dashboard and check that all three elements appear.

:::::::::::::::::::::::: hint
A text card is a `:::` block with the class `.card`. The title is set in curly braces:
`::: {.card title="..."}`. Inside the card, you write plain Markdown. Remember, a list needs
an empty line before it and a space after each `-`.
:::::::::::::::::::::::::::::

:::::::::::::::::::::::: solution
## Solution

````markdown
::: {.card title="OA publishing routes"}

- **Gold**: published open access in an OA journal
- **Green**: a copy is deposited in a repository
- **Hybrid**: open access article in a subscription journal

Find open access journals and articles: [Directory of Open Access Journals](https://doaj.org)
:::
````
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge
## Challenge 2: Orientation experiment

Your university manager would prefer the layout rotated: the three OA indicators stacked in a column on the
left. Without touching any headings, change the YAML header so that level 2 headings create
**columns** instead of rows. Render and describe what changed. Then change it back.

:::::::::::::::::::::::: hint
Remember the `orientation` setting from the "Introduction to Quarto Dashboards" episode, and how it is nested in the YAML
header.
:::::::::::::::::::::::::::::

:::::::::::::::::::::::: solution
## Solution

```yaml
---
title: "Open Access Monitoring Dashboard"
format:
  dashboard:
    orientation: columns
---
```

Level 2 headings now create columns instead of rows, and the `height` attribute of the
"rows" no longer makes sense (you would use `width` now). Level 3 headings inside a 
column now create rows. Revert the change to continue with the lesson.
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints
- Create a dashboard by setting `format: dashboard` in the YAML header
- `#` creates a page, `##` a row, `###` a column; `height` and `width` set their relative size
- Value boxes are chunks with `#| content: valuebox`
- Every chunk's output becomes a card; Markdown text and images go into `.card` divs
::::::::::::::::::::::::::::::::::::::::::::::::
