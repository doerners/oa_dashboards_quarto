---
title: "Styling the Dashboard"
teaching: 25
exercises: 8
---

:::::::::::::::::::::::::::::::::::::: questions
- How do you set a dashboard's overall look with a built-in theme and add a logo?
- How do you choose value box colours that match your theme?
- How do you style charts and format numbers consistently?
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives
- Set a dashboard's overall look using a built-in theme and add a logo in the YAML header
- Choose value box colours that match the chosen theme
- Apply a consistent `ggplot2` theme and your own OA colours to charts
- Format large numbers consistently using `format()`
::::::::::::::::::::::::::::::::::::::::::::::::

## Time to make it look good

In the skeleton episode, we deliberately left styling for later. Now is the time.

## Choosing a theme and adding a logo

Quarto dashboards come with built-in themes (based on a project called Bootswatch). Each one
changes fonts, colours and spacing across the whole dashboard with a single YAML setting. A logo
is shown next to the title in the navigation bar. Both are settings of the dashboard format, so
they go *inside* `dashboard:`, indented one level further, just like `orientation` before:

```yaml
---
title: "Open Access Monitoring Dashboard"
format:
  dashboard:
    theme: morph
    logo: images/lc_logo.svg
---
```

First, create an `images` folder in your project (the same way you created `data` earlier), download the [Library Carpentry Logo](https://github.com/LibraryCarpentry/lc-styleguide/blob/main/logo/lc_logo.svg) and
move the file into it. The path in `logo:` is relative to your project folder. Render the dashboard to see the new look.

You can browse the full list of themes, with previews, in the
[Quarto theming documentation](https://quarto.org/docs/output-formats/html-themes.html). Try a few
and see which fits your institution.

## Matching value box colours to the theme

Recall the `color` option of the value boxes (`primary`, `success`, `warning`, `info`). These names refer
to a fixed set of colour roles defined by Bootstrap, the framework dashboards are built on:
`primary`, `secondary`, `success`, `info`, `warning`, `danger`, `light` and `dark`. Whichever theme
you choose defines what each role looks like, so your value boxes automatically match the theme,
without looking up colour codes. If you need a colour outside this set, you can use, for example, hex code colors.

::: callout
### Colours carry meaning
Be consistent: if green means "open" on one card, it should not mean "closed" on another. Pick
colours once and reuse them.
:::

## Styling charts to match

Your `ggplot2` charts do not automatically pick up the dashboard's theme. They need their own
styling. Two small additions make a big difference.

**A built-in `ggplot2` theme**, added with `+` like other layers. `theme_minimal()` removes the grey
background and gridlines, which fits dashboard cards well:

```r
oa_df |>
  group_by(publication_year) |>
  summarise(oa_share = mean(is_oa) * 100) |> 
  ggplot(aes(x = factor(publication_year), y = oa_share)) +
  geom_col() +
  labs(x = "Publication year", y = "Open access share (%)") +
  theme_minimal()
```

**Your own OA colours.** Remember the named vector `oa_colors` from the R episode? Named vectors
are exactly what the `scale_fill_manual()` function from `ggplot2` needs: each OA status gets its colour. Define the vector in
your setup chunk, with one entry for each OA status that occurs in your data (check `count(oa_status)` from the data episode):

```r
oa_colors <- c(
  gold = "gold",
  hybrid = "orange",
  green = "forestgreen",
  bronze = "chocolate",
  diamond = "skyblue",
  closed = "grey"
)
```

The named vector `oa_colors` is used inside a chart that colours the bars by status. For this
chart, we need the share of each OA status per publication year. We count the articles per year
and status, calculate each status's share within its year, and map `oa_status` to the bar colour
with `fill = oa_status`:

```r
oa_df |>
  count(publication_year, oa_status) |>
  group_by(publication_year) |>
  mutate(share = n / sum(n) * 100) |>
  ggplot(aes(x = factor(publication_year), y = share, fill = oa_status)) +
  geom_col() +
  scale_fill_manual(values = oa_colors) +
  labs(x = "Publication year", y = "Share of articles (%)", fill = "OA status") +
  theme_minimal() 
```

## Formatting numbers consistently

For value boxes (and anywhere else you display a large number), a comma as thousands separator
makes numbers easier to read at a glance: `1810861` versus `1,810,861`. The `format()` function
with its `big.mark` argument does this:

```r
format(nrow(oa_df), big.mark = ",")
```

Wrap it around the value in the "Total Publications" box:

```r
#| content: valuebox
#| title: "Total Publications"
#| icon: book
#| color: primary

format(nrow(oa_df), big.mark = ",")
```

::::::::::::::::::::::::::::::::::::: challenge
## Challenge: Apply the styling everywhere

Go through your dashboard and check:

1. The count value boxes ("Total Publications", "Open Access Publications") are wrapped in
   `format(..., big.mark = ",")`.
2. The charts have `theme_minimal()` added.
3. You chose a theme and added the logo to the YAML header.

Render once more and check that everything looks consistent.

::::::::::::::::::::::::::::::::::::::::::::::::


::::::::::::::::::::::::::::::::::::: keypoints
- Set `theme` and `logo` inside `dashboard:` in the YAML header to style the whole dashboard at
  once
- Value box `color` options (`primary`, `success`, `warning`, ...) automatically match your theme
- `ggplot2` charts need their own styling, such as `theme_minimal()`
- A named vector of colours, passed to `scale_fill_manual()`, gives each OA status a consistent
  colour
- `format(x, big.mark = ",")` adds thousands separators to numbers you display
::::::::::::::::::::::::::::::::::::::::::::::::
