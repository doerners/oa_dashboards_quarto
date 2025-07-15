---
site: sandpaper::sandpaper_site
---

This is lesson is designed for professionals involved in scholarly communication analytics, e.g. in the areas of research, data analysis, library and information science and policy, looking to enhance their technical skills in dashboard creation for open access monitoring activities using the open-source tool [Quarto](https://quarto.org/) with [R](https://www.r-project.org/) as an example programming language in the [RStudio](https://posit.co/download/rstudio-desktop/) integrated development environment.

Learners will become familiar with Quarto for dashboard creation. Quarto, as an open-source scientific and technical publishing system, allows you to generate multiple output formats from a single source file, seamlessly integrating text and dynamic content. While Quarto is a versatile tool with applications ranging from data visualization to reproducible documentation, this lesson will focus specifically on using it for creating (interactive) dashboards tailored to open access monitoring. 

## Learning Objectives

By the end of this lesson, learners will be able to:

1. Describe what Quarto is and explain how Quarto documents are structured.
2. Describe what Quarto dashboards are, their key features, and the components they consist of.
3. Apply Markdown syntax to define components and structure content within a Quarto dashboard.
4. Use R functions to load libraries and import data into the dashboard.
5. Apply R functions to generate calculations and plots as dashboard content.
6. Use YAML syntax to style and customize the appearance of the dashboard.
7. Use YAML syntax and apply R functions to generate interactive dashboard content.

::::::::::::::::::::::::::::: callout

If you are a novice learner with little to no experience in programming this lesson might be hard to follow. In this case it is **highly** recommended to visit a training for novices first, e.g. [Library Carpentry OpenRefine training](https://librarycarpentry.github.io/lc-open-refine/) to learn about data cleaning, [Library Carpentry Introduction to R](https://librarycarpentry.github.io/lc-r/index.html) to get acquainted with R, [Library Carpentry Introduction to Python](https://librarycarpentry.github.io/lc-python-intro/index.html) to get acquainted with Python.

:::::::::::::::::::::::::::::

:::::: prereq
Before joining this training, learners should meet the following criteria.

### R (or Python) 
- You have basic knowledge of coding in R (loading libraries and data, transforming data, computing values, creating tables and visualizations).
- Alternatively you have basic knowledge of coding in Python. Although we will use R as an example programming language, Quarto works with Python as well. 
- You have installed R and RStudio along with some packages before the lesson.

### Markdown (optional but recommended)
- You have basic knowledge of markdown syntax (creating headings, links and importing images)

### YAML (optional but recommended)
- You have basic knowledge of YAML syntax (strings, booleans, nesting, files)

:::::::::
