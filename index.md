---
site: sandpaper::sandpaper_site
---

How open is our institution's research output? Which publishers and transformative agreements
account for most of our articles? Is our Open Access (OA) share growing? Library and scholarly
communication teams answer questions like these for diverse audiences: for management, for funders and
in negotiations with publishers. A spreadsheet or a one-off report answers them once. A
**dashboard that is generated from code** answers them every time the data is updated.

This workshop explores how to build such a dashboard for OA monitoring with
[Quarto](https://quarto.org/) and the [R programming language](https://www.r-project.org/) in
[RStudio](https://posit.co/download/rstudio-desktop/). Quarto is an open-source scientific and
technical publishing system: you write text, layout instructions and code in one plain-text
file, and Quarto turns it into a finished document.

The lesson is designed for **library and information professionals who work with open access and scholarly communication** 
and who have little or no programming experience. It combines programming and professional skills: 
learners build a dashboard step by step with code and all exercises are based on tasks and topics from OA practice.

## Learning Objectives

By the end of this lesson, learners will be able to:

1. Describe what Quarto is, how a Quarto document is structured, and what a Quarto dashboard is
   used for in OA monitoring.
2. Apply Markdown syntax to structure text and to define the pages, rows, columns and cards of a
   dashboard.
3. Use R to load packages and data, filter publication records for one institution, and
   calculate OA indicators.
4. Create value boxes and plots from calculated OA indicators as dashboard content.
5. Create a searchable table.
6. Use YAML syntax to style and customise the appearance of the dashboard.
7. Render the dashboard, update it with new data and share it.

:::::: prereq
This training **does not** assume or require any prerequisite programming skills.

You need a laptop on which you can install software. Please complete the
[setup instructions](learners/setup.md) below **before** the workshop.

The training builds on several software components (Markdown, R/RStudio, YAML, Quarto), but
introduces them only at the minimum level needed to successfully build an OA dashboard in Quarto.
If you would like a more thorough introduction to these topics, The Carpentries offer dedicated
lessons, like:

- [Introduction to R](https://librarycarpentry.github.io/lc-r/)
- [Introduction to Reproducible Publications with Quarto](https://carpentries-incubator.github.io/reproducible-publications-quarto/)
:::::::::

