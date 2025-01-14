biosurvey: Tools for Biological Survey Planning
================
Claudia Nunez-Penichet, Marlon E. Cobos, Jorge Soberon, Tomer Gueta,
Narayani Barve, Vijay Barve, Adolfo G. Navarro-Siguenza, A. Townsend
Peterson

-   [Package description](#package-description)
-   [Installing the package](#installing-the-package)
-   [biosurvey functions and
    vignettes](#biosurvey-functions-and-vignettes)
-   [Workflow description](#workflow-description)
    -   [Initial data](#initial-data)
    -   [Data preparation](#data-preparation)
    -   [Selection of sites for biodiversity
        inventory](#selection-of-sites-for-biodiversity-inventory)
    -   [Evaluation of sampling site
        effectiveness](#evaluation-of-sampling-site-effectiveness)
-   [GSoC project description](#gsoc-project-description)
    -   [Status of the project](#status-of-the-project)

<!-- badges: start -->

[![R build
status](https://github.com/claununez/biosurvey/workflows/R-CMD-check/badge.svg)](https://github.com/claununez/biosurvey/actions)
<!-- badges: end -->

<br>
<hr>

<img src='man/figures/biosurveyfinal.png' align="right" height="200" /></a>

**This repository is for the project “Biological Survey Planning
Considering Hutchinson’s Duality” developed during the program GSoC 2020
(see details at the end).**

<br>

## Package description

The **biosurvey** R package implements multiple tools to select sampling
sites for biodiversity inventory, increasing effectiveness by
considering the relationship of environmental and geographic conditions
in a region. The functions are grouped in three main modules: 1) Data
preparation; 2) Selection of sampling sites; and, 3) Tools for testing
effectiveness (Fig. 1). Data are prepared in ways that avoid the need
for more data in posterior analyses and allow users to focus on critical
methodological decisions to select sampling sites. Various algorithms
for selecting sampling sites are available, and options for considering
pre-selected sites (known to be important for biodiversity monitoring)
are included. Visualization is a critical component in this set of tools
and most of the results obtained can be plotted to help users to
understand their implications. The options for selecting sampling sites
included here differ from other implementations in that they consider
the environmental and geographic structure of a region to suggest
sampling sites that could increase the effectiveness of efforts
dedicated to inventor biodiversity.

<div class="figure">

<img src="man/figures/simple_wf.png" alt="Figure 1. Schematic view of the workflow to use biosurvey. Details on the workflow below." width="2372" />
<p class="caption">
Figure 1. Schematic view of the workflow to use biosurvey. Details on
the workflow below.
</p>

</div>

<br>

## Installing the package

Note: Internet connection is required to install the package.

To install the latest release of **biosurvey** use the following line of
code:

``` r
# Installing from CRAN
install.packages("biosurvey")
```

<br>

The development version of **biosurvey** can be installed using the code
below.

``` r
# Installing and loading packages
if(!require(remotes)){
  install.packages("remotes")
}

# To install the package use
remotes::install_github("claununez/biosurvey")

# To install the package and its vignettes use (if needed use: force = TRUE)  
remotes::install_github("claununez/biosurvey", build_vignettes = TRUE)
```

<br>

If you have any problems during installation of the development version
from GitHub, restart R session, close other RStudio sessions you may
have open, and try again. If during the installation you are asked to
update packages, do so if you don’t need a specific version of one or
more of the packages to be installed. If any of the packages give an
error when updating, please install it alone using `install.packages()`,
then try installing **biosurvey** again.

<br>

To load the package use:

``` r
# Load biosurvey
library(biosurvey)
```

<br>

## biosurvey functions and vignettes

To check all functions in the package use:

``` r
help(biosurvey)
```

<br>

If the package was installed with its vignettes you can see all options
with:

``` r
vignette(package = "biosurvey")
```

<br>

To check vignettes you can use:

-   `vignette("biosurvey_preparing_data")`.- For a guide on how to
    prepare data for analysis.
-   `vignette("biosurvey_selecting_sites")`.- For a guide on how to
    select sampling sites.
-   `vignette("biosurvey_selection_with_preselected_sites")`.- For a
    guide on how to select sampling sites when some sites have been
    preselected.
-   `vignette("biosurvey_testing_module")`.- For a guide on how to use
    the testing module.

<br>

## Workflow description

### Initial data

As shown in Fig. 1, to use **biosurvey** and select sites for
biodiversity inventory you need:

-   *Environmental variables*.- These variables must be in raster format
    (e.g., GTiff, BIL, ASCII). To load these variables to your R
    environment, you can use the function `stack` from the package
    `raster`.
-   *Region of interest*.- As your analyses will be focused on a region,
    a spatial polygon of such an area is needed. Common formats in which
    your polygon can be are Shapefile, GeoPackage, GeoJSON, etc. To load
    this information to your R environment you can use the function
    `readOGR` from the `rgdal` package.

Additionally, other data can be used to make sampling site selection
more effective. The functions that help to prepare the data for analysis
also allow users to include:

-   *Sites selected a priori*.- A data.frame of sites that researchers
    consider important to be included in any set of localities to be
    sampled. These site(s) will be included (by force) in any of the
    results obtained in later analyses. Using these sites represents a
    good opportunity to consider areas that are well known and should be
    included to monitor biodiversity changes in a region.
-   *A mask to restrict analyses to smaller areas*.- A spatial polygon
    that reduces the region of interest to areas that are considered to
    be more relevant for analysis can be considered. Some examples of
    how to define these masks include areas with natural vegetation,
    areas that are accessible, regions with particular vegetation cover
    types, etc. This mask is usually in the same format that the *Region
    of interest*.

If enough, good-quality data on species distributions are available,
analyses of the effectiveness of sampling sites can be performed. The
data used to prepare information to perform such analyses can be of
different types:

-   Spatial polygons of species distributions
-   Raster layers defining suitable and unsuitable areas
-   Geographic points of species occurrences

<br>

### Data preparation

To use **biosurvey** efficiently the first thing to do is to prepare an
object containing all information to be used in following analyses. This
can be done using the function `prepare_master_matrix()`. After that,
the function `make_blocks()` can be used to partition the environmental
space of the region of interest. To explore how your data looks like,
the functions `explore_data_EG()` and `plot_blocks_EG()` can be used.

<br>

### Selection of sites for biodiversity inventory

After preparing data, distinct functions can be used to select sampling
sites:

-   `random_selection()`.- Random selection of sites to be sampled in a
    survey.
-   `uniformG_selection()`.- Selection of sites with the goal of
    maximizing uniformity of points in geographic space.
-   `uniformE_selection()`.- Selection of sites with the goal of
    maximizing uniformity of points in environmental space.
-   `EG_selection()`.- Selection of sites with the goal of maximizing
    uniformity of points in environment, but considering geographic
    patterns of data.

See also how your selected sites look like with the functions
`plot_sites_EG()`, `plot_sites_E()`, and `plot_sites_G()`.

<br>

### Evaluation of sampling site effectiveness

After the selection of sampling sites, and, if enough high-quality data
are available, functions from the testing module of this package can be
used to explore which sets of sites selected could be better. Explore
the following functions to prepare your data, and assess how well your
selected sites perform in representing the exiting biodiversity:

-   `prepare_base_PAM()`.- Prepares a presence-absence matrix (PAM) from
    species distributional data; all sites (rows) will have a value for
    presence or absence of species (columns).
-   `PAM_indices()`.- Calculates a set of biodiversity indices using
    values contained in the presence-absence matrix.
-   `plot_PAM_geo()`.- Plot of PAM indices in geography.
-   `subset_PAM()`.- Subsets a base\_PAM object according to sites
    selected previously that are contained in a master\_selection
    object.
-   `selected_sites_SAC()`.- Creates species accumulation curves for
    each set of selected sites contained in elements of PAM\_subset.
-   `plot_SAC()`.- Creates species accumulation curve plots for selected
    sites.
-   `compare_SAC()`.- Creates comparative plots of two species
    accumulation curves from information contained in lists obtained
    with the function `selected_sites_SAC()`.
-   `selected_sites_DI()`.- Computes dissimilarity indices among sites
    selected and among sets of selected sites, based on the communities
    of species represented in such units.
-   `plot_DI()`.- Creates matrix-like plots of dissimilarities found
    among communities of species in distinct sites selected or sets of
    sites selected.
-   `DI_dendrogram()`.- Plot dissimilarities withing and among sets of
    selected sites as a dendrogram.

<br>
<hr>

## GSoC project description

Student: *Claudia Nuñez-Penichet*

GSoC Mentors: *Narayani Barve, Vijay Barve, Tomer Gueta*

Complete list of authors: *Claudia Nunez-Penichet, Marlon E. Cobos,
Jorge Soberon, Tomer Gueta, Narayani Barve, Vijay Barve, Adolfo G.
Navarro-Siguenza, A. Townsend Peterson*

Motivation:

Given the increasing intensity of threats to biodiversity in the world,
one of the challenges in biodiversity conservation is to complete
inventories of existing species at distinct scales. Species
distributions depend on the relationships between accessible areas,
environmental conditions, and biotic interactions. As planning a survey
system only aims to register species in a region, biodiversity
interaction can be overlooked in this case. However, the relationship
between environmental conditions and the geographic configuration of an
area is of crucial importance when trying to identify key sites for
biodiversity surveys. Among the diverse packages in R for selecting
survey sites, such considerations are not implemented and are limited to
a random selection of sampling sites or analyses that allow detecting
potential sampling sites based on the environmental similarity between
sampled and unsampled areas. Given the need for more solutions, the
**biosurvey** package aimed for considering the relationship between
environmental and geographic conditions in a region when designing
survey systems that allow sampling of most of its biodiversity.

### Status of the project

At the moment we have completed the three main modules of the package.
We have made modifications to the original list of products, which have
helped us to improve the package functionality. The package is fully
functional and available on CRAN.

All commits made can be seen at the
<a href="https://github.com/claununez/biosurvey/commits/master" target="_blank">complete
list of commits</a>.
