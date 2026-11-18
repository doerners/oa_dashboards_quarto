---
title: Setup
---

## Getting ready

You need to install R and RStudio and download a data file to follow this lesson.

### Installing R and RStudio

You need R and RStudio installed and also need to be able to install R packages, create directories, and download files.
To do this, please make sure you either have administrative rights for your computer or get help for the setup from an IT administrator at your institution.

Several Carpentries lessons already rely on R and RStudio, please see [this section of the workshop template](https://carpentries.github.io/workshop-template/install_instructions/#r-1) for instructions on installing R/RStudio for various operating systems.

- [R/RStudio installation on Windows](https://carpentries.github.io/workshop-template/install_instructions/#r)
- [R/RStudio installation on MacOS](https://carpentries.github.io/workshop-template/install_instructions/#r)
- [R/RStudio installation on Linux](https://carpentries.github.io/workshop-template/install_instructions/#r)

R is the programming language we'll use in this lesson to calculate metrics and create visualizations for the dashboard. 
RStudio is the integrated development environment (IDE) we'll use in this lesson to interact with R and Quarto.

### Installing Quarto

> A stable release of Quarto is bundled with RStudio v2022.07.1, and later. Upgrading to new versions of RStudio in the future will also upgrade the bundled Quarto version ([Posit user guide](https://docs.posit.co/ide/user/ide/guide/documents/quarto-project.html)). 

**You will not need to install Quarto seperately** unless you want to upgrade Quarto out of sync with the bundled version in RStudio. Quarto dashboards require Quarto 1.4 or newer. Recent RStudio versions bundle it. 
To check your version, open the Terminal tab in RStudio and type `quarto --version`. For this lesson we will use the bundled Quarto version.

### Updating R and RStudio

This lesson was developed and tested with R version 4.6.2 and RStudio version 2026.11.0+203. 

If you already have R and RStudio installed, please check if your R version is up to date:

- When you open RStudio your R version will be printed in the console on the bottom left. Alternatively, you can type `sessionInfo()` into the console. If your R version is 4.6.2 or later, you don't need to update R for this lesson. If your version of R is older than that, download and install the latest version of R from the R project website [for Windows](https://cran.r-project.org/bin/windows/base/), [for MacOS](https://cran.r-project.org/bin/macosx/), or [for Linux](https://cran.r-project.org/bin/linux/).
- It is not necessary to remove old versions of R from your system, but if you wish to do so you can check [How do I uninstall R?](https://cran.r-project.org/bin/windows/base/rw-FAQ.html#How-do-I-UNinstall-R_003f) 
- After installing a new version of R, you will have to reinstall all your packages with the new version.
- To update RStudio to the latest version, open RStudio and click on `Help > Check for Updates`. If a new version is available follow the instruction on screen. By default, RStudio will also automatically notify you of new versions every once in a while.

### Downloading the data

Download the [data zip file](https://github.com/doerners/oa_dashboards_quarto/raw/refs/heads/main/data/jct_oal_TA_dataset.zip) and unzip it. Make a note of the location (i.e. the folder, your desktop) to which you saved the file. 

This file is based on the Journal Checker Tool articles file from the *OPENBIB: Selected curated open metadata based on OpenAlex* data set published on [Zenodo](https://doi.org/10.5281/zenodo.15308680) under a CC0 license. 

The data file was enhanced with open access related data from [OpenAlex](https://docs.openalex.org/) which are also available under a CC0 license.

The compiled teaching dataset used in this lessons is a CSV formatted dataset containing the following variables:

| column name | description |
|-------------|-------------|
|openalex\_id | article identifier given within the OpenAlex database|
|doi          | digital object identifier of the article |
|issn\_l      | linking ISSN of the journal the article was published in |
|ror          | Research Organisation Registry identifier of the corresponding author's affiliated institution|
|esac\_id     | ESAC Registry identifier for the transformative agreement under which the article was published|
|start\_date  | transformative agreement term start date|
|end\_date    | transformative agreement term end date|
|publication\_date| article publication date|
|oa\_status| article open access status|
|field| primary subject field the article was assigned to based on the classification of OpenAlex|
|domain| primary subject domain the article was assigned to based on the classification of OpenAlex|
|institution| display name of the corresponding author's affiliated institution|
|publisher| display name of the publisher the article was published by|
|publication\_year| article publication year|


