---
title: "Rendering, Updating and Sharing"
teaching: 6
exercises: 5
---

:::::::::::::::::::::::::::::::::::::: questions
- How do you turn your dashboard into a file that colleagues can open?
- How do you update the dashboard when new data arrives?
- Where can you go from here?
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives
- Render the dashboard to a single, shareable HTML file
- Update the dashboard with new data or for a different institution by changing one line
- Name next steps for extending your OA dashboard
::::::::::::::::::::::::::::::::::::::::::::::::

## Rendering the dashboard

Every time you click **Render** in RStudio, Quarto runs all the code in your document from top to
bottom and writes a file named after your document with the `.html` extension, into your project
folder. Open it with a double click: it works in any web browser, and the interactive table work without R or RStudio.

By default, Quarto also creates a folder (`filename_files`) with supporting files that the
HTML file needs. To share the dashboard as **one self-contained file**, for example as an e-mail
attachment or on an intranet page, add one more setting to the YAML header:

```yaml
---
title: "Open Access Monitoring Dashboard"
format:
  dashboard:
    theme: morph
    logo: images/lc_logo.svg
    embed-resources: true
---
```

Render again. The resulting dashboard file now contains everything it needs.

## Updating the dashboard

This is where a code-based dashboard pays off. Imagine a new data export arrives next quarter:

1. Save the new CSV in the `data` folder (with the same file name, or adapt the file name in the
   setup chunk)
2. Click **Render**

That is all. Every value box, chart and table is recalculated. To produce the dashboard for
another institution, change `my_ror` in the setup chunk and render.

::::::::::::::::::::::::::::::::::::: challenge
## Challenge: One dashboard, two institutions

Your library supports a partner institution and wants to compare. Change `my_ror` in the setup
chunk of your dashboard to a different institution's ROR ID (from the `count()` output in the data
episode) and render again.

1. What changed on the dashboard? What stayed the same?
2. Which indicators would you want to put next to each other to compare the two institutions?

::::::::::::::::::::::::::::::::::::::::::::::::

## Sharing your dashboard

There are several ways to share a dashboard, from simple to more involved:

- send the single HTML file by e-mail or put it on a shared drive or intranet page
- publish it on [Quarto Pub](https://quarto.org/docs/publishing/quarto-pub.html) or
  [GitHub Pages](https://quarto.org/docs/publishing/github-pages.html)
- use your institution's web server or an institutional publishing platform

See the Quarto [publishing documentation](https://quarto.org/docs/publishing/) for details.

## Where to go from here

You now have all the building blocks of a Quarto dashboard. Natural next steps:

- add a **sidebar** and **tabsets** to organise more content
  (see the Quarto [layout documentation](https://quarto.org/docs/dashboards/layout.html))
- add more OA indicators, such as the share of articles covered by a transformative agreement
  (`!is.na(esac_id)`), or APC spending if you have cost data
- fetch your own data directly from open sources such as OpenAlex instead of working
  with a CSV file
- look at the [Quarto dashboard gallery](https://quarto.org/docs/dashboards/examples/) for ideas
- continue with other [Library Carpentry](https://librarycarpentry.github.io/) lessons


::::::::::::::::::::::::::::::::::::: keypoints
- **Render** runs all code and writes an HTML file; `embed-resources: true` makes it a single
  shareable file
- To update the dashboard, replace the data and render again; to change the institution, change
  the ROR ID in `my_ror`
- A dashboard can be shared as a single HTML file, or published on Quarto Pub or GitHub Pages
::::::::::::::::::::::::::::::::::::::::::::::::
