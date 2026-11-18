---
title: "Introduction to Markdown"
teaching: 12
exercises: 8
---

:::::::::::::::::::::::::::::::::::::: questions
- What is Markdown, and why do we need it for Quarto dashboards?
- How do you create headings, bold text, lists, links and images in Markdown?
- How do you tell Quarto to *show* code and to *run* code?
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives
- Explain what Markdown is and why Quarto documents use it
- Write headings of different levels and explain why heading levels matter for dashboards
- Format text in bold and italics and create bulleted and numbered lists
- Create links and add images with alt text
- Distinguish between inline code, a highlighted code block and an executable code chunk
::::::::::::::::::::::::::::::::::::::::::::::::

::: callout
### You might skip this episode if...

You already write Markdown (headings, lists, links, images, code blocks) and know the difference
between a code block and an executable code chunk.
:::

## What is Markdown?

Markdown is a lightweight way of formatting text using plain-text symbols. Instead of clicking a
"bold" button in a word processor, you type symbols like `**` around your text. A program then
turns these symbols into formatted output, for example HTML, PDF or, in our case, a Quarto
dashboard.

Quarto uses a version of Markdown called
[Pandoc-flavored Markdown](https://pandoc.org/MANUAL.txt). Markdown matters for this lesson in
particular, because Quarto dashboards use headings to define the layout:

- A level 1 heading (`#`) starts a new dashboard **page**
- A level 2 heading (`##`) starts a new dashboard **row or column**

We will use this in a later episode. For now, we practise the syntax.

## Headings

Headings are created with one or more hash symbols (`#`) at the start of a line, followed by a
space and the heading text. The more hash symbols, the lower the heading level and the smaller
the font size.

```markdown
# Heading level 1
## Heading level 2
### Heading level 3
#### Heading level 4
```

::: callout
### Two things to watch out for

- **Missing space:** `#Heading` does not work, `# Heading` does.
- **Missing empty line:** put an empty line before every heading (and before every list).
  Otherwise it may be treated as regular text, and in a dashboard your layout might not work as
  expected.
:::

::::::::::::::::::::::::::::::::::::: challenge
## Challenge 1: Headings

You are writing a short guide on Open Access for your institution. The indentation below shows
which level each heading belongs to:

```
Open Access at Our University
    Publishing routes
        Gold Open Access
        Green Open Access
    OA Funding
        Article processing charges
```

Write this structure as Markdown headings, using the correct heading levels.

:::::::::::::::::::::::: hint
Start with the line that has no indentation. It is the main title, so it gets level 1.
:::::::::::::::::::::::::::::

:::::::::::::::::::::::: solution
## Solution

```markdown
# Open Access at Our University

## Publishing routes

### Gold Open Access

### Green Open Access

## OA Funding

### Article processing charges
```

The further a line is indented, the more hash symbols it gets. Later in this lesson you will use
exactly this idea to decide what becomes a page, a row or a column in your dashboard.
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Emphasis and lists

Surround text with one asterisk to make it *italic* and with two to make it **bold**:

```markdown
*This text is italic*

**This text is bold**
```

Bulleted lists start with a dash and a space (`- `) before each item, numbered lists with a number, a
period and a space (`1. `). Remember the empty line before a list:

```markdown
- open access
- open data
- open source

1. Collect your data
2. Clean your data
3. Build your dashboard
```

This renders as:

- open access
- open data
- open source

1. Collect your data
2. Clean your data
3. Build your dashboard

## Links and images

To create a link, put the link text in square brackets, directly followed by the URL in round
brackets:

```markdown
[Unpaywall](https://unpaywall.org)
```

This renders as: [Unpaywall](https://unpaywall.org). If you want to show the URL itself, wrap it
in angle brackets: `<https://openalex.org>`.

Adding an image works almost like a link, but with an exclamation mark `!` in front. The text in
the square brackets becomes the **alt text**: a short description that screen readers read out
and that is shown if the image cannot be loaded. Alt text makes images accessible to more people.

```markdown
![The Open Access logo, an open padlock](https://commons.wikimedia.org/wiki/Special:FilePath/Open_Access_logo_PLoS_transparent.svg)
```

This renders as an image of the Open Access logo:

![The Open Access logo, an open padlock](https://commons.wikimedia.org/wiki/Special:FilePath/Open_Access_logo_PLoS_transparent.svg){alt="The Open Access logo, an open padlock"}

You will use exactly this image syntax again when you build the dashboard.

::::::::::::::::::::::::::::::::::::: challenge
## Challenge 2: Links and images

You are preparing the "Useful resources" section of an OA guide.

1. Turn this URL into a Markdown link with the link text "Directory of Open Access Journals":
   `https://doaj.org`
2. Add a Markdown image using the URL below, and write a short, meaningful alt text for it:
   `https://commons.wikimedia.org/wiki/Special:FilePath/DOAJ_logo.png`

:::::::::::::::::::::::: solution
## Solution

```markdown
[Directory of Open Access Journals](https://doaj.org)

![Logo of the Directory of Open Access Journals (DOAJ)](https://commons.wikimedia.org/wiki/Special:FilePath/DOAJ_logo.png)
```

Any short description of what the image shows works as alt text. "Image" or "logo" alone would not
tell a screen reader user much.
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Code: inline, blocks and chunks

Markdown can mark text as code, which is displayed in a fixed-width (monospace) font. This matters
for Quarto, because the content of a dashboard is produced by code.

**Inline code** uses single backticks:

```markdown
Use `filter(oa_status == "gold")` to keep only gold Open Access articles.
```

A **fenced code block** uses three backticks on their own line before and after the code. If you
add the language name after the opening backticks, the code is coloured and easier to read. It is
still only displayed, not run. If you put the language name in **curly braces**, Quarto treats the
block as an **executable code chunk**: it runs the code and shows the result.

````markdown
```
summary(articles)
```

```r
summary(articles)
```

```{r}
summary(articles)
```
````

Quarto therefore knows three ways to handle code:

- ` ``` `: a plain code block (monospace text)
- ` ```r `: a highlighted code block (coloured, but **not** run)
- ` ```{r} `: an **executable** code chunk (Quarto runs it and shows the output)

In a Quarto dashboard, headings define the layout and executable code chunks produce its content.
These are the two Markdown building blocks you will use most.


::::::::::::::::::::::::::::::::::::: challenge
## Challenge 3: Spot the syntax

Look at the following snippet from an OA guide. Identify (a) the heading level, (b) the bold
text, (c) the inline code and (d) the link.

```markdown
## Gold Open Access

Use the `oa_status` column to find gold articles. You can check journals in
the [DOAJ](https://doaj.org). **Note:** gold Open Access often requires an
article processing charge.
```

:::::::::::::::::::::::: solution
## Solution

(a) `## Gold Open Access`: a level 2 heading

(b) `**Note:**`: bold text

(c) `` `oa_status` ``: inline code

(d) `[DOAJ](https://doaj.org)`: a link
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

::: callout
### Further reading

The Quarto documentation has a page on
[Markdown basics](https://quarto.org/docs/authoring/markdown-basics.html) with more syntax than
we covered here.
:::

::::::::::::::::::::::::::::::::::::: keypoints
- Markdown is a lightweight plain-text syntax for formatting text; Quarto is built on it
- Headings and lists need a space after the symbol and an empty line before them; heading levels
  (`#`, `##`) will define pages, rows and columns in a dashboard
- Links use `[text](url)` and images use `![alt text](url)`
- ` ```r ` displays highlighted code, ` ```{r} ` runs it
::::::::::::::::::::::::::::::::::::::::::::::::
