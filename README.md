<h1 align="center">
  <br>
  <a href="https://r-spatial.github.io/rgee/"><img src="https://user-images.githubusercontent.com/27021459/175114532-3bf7ef37-eb7c-4302-95c8-ef1359a2001a.png" alt="Markdownify" width="400"></a>
  <br>
  TVDI algorithm
  <br>
</h1>

<h4 align="center">Google Earth Engine function to compute the Temperature-Vegetation Dryness Index (TVDI).</h4>


<p align="center">
<a href="https://github.com/r-earthengine/rgeeExtra/actions"><img src="https://github.com/r-earthengine/rgeeExtra/workflows/R-CMD-check/badge.svg" alt="R build status"></a>
<a href="https://www.repostatus.org/#active"><img src="https://www.repostatus.org/badges/latest/active.svg" alt="Project Status: Active – The project has reached a stable, usable
state and is being actively
developed."></a>
<a href="https://codecov.io/gh/r-earthengine/rgeeExtra">
  <img src="https://codecov.io/gh/r-earthengine/rgeeExtra/branch/master/graph/badge.svg?token=Q1SNZZU62W"/>
</a>
<a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/license-MIT-green" alt="License"></a>
<a href="https://www.tidyverse.org/lifecycle/#maturing"><img src="https://img.shields.io/badge/lifecycle-maturing-blue.svg" alt="lifecycle"></a>
<br>
</p>



<p align="center">  
  • 
  <a href="#methodology">Methodology</a> &nbsp;•
  <a href="#how-to-run-on-gee">How to run on GEE?</a> &nbsp;•
  <a href="#example">Example</a> &nbsp;•
  <a href="#share-the-love">Citation</a> &nbsp;•
</p>

[Google Earth Engine](https://earthengine.google.com/) is a cloud-based platform that allows users to have an easy access to a petabyte-scale archive of remote sensing data and run geospatial analysis on Google's infrastructure. The present work **developed a function in Google Earth Engine to calculate the TVDI index based on the paper [L. W. Schirmbeck et al., 2018](https://doi.org/10.1590/1678-992X-2016-0315).


## Methodology
The methodology used in this work was the same used in the paper written by  Lucimara Wolfarth Schirmbeck, Denise Cybis Fontana and Juliano Schirmbeck called ´Two approaches to calculate TVDI in humid subtropical climate of southern Brazil´, published in 2018. This methodology briefly consists of the following steps: 

- Slice the NDVI values into 100 intervals of 0.01 and obtain the LST values for each of these intervals;
- Construct the cumulative frequency histogram for each NDVI interval and identify the LST values corresponding to 2% and 98% occurrence;
- Define the pixels of the wet edge (all pixels with LST < temperature corresponding to 2%) and the dry edge (all pixels with LST ≥ temperature corresponding to 98%). This procedure was performed for each of the 100 NDVI intervals;
- Group the pixels of each boundary and interval;
- Calculate the dry edge - Linear regression using all pixels that compose the dry edge to obtain the coefficients a and b of the fitted equation;
- Calculate the wet edge - Average value of all pixels that compose the wet edge;



## Example

An example for calculating TVDI in GEE is shown below. Once the user has his region of interest and the NDVI and LST images, the only two steps to invoke the calculation is to import the TVDI module **TVDImodule = require(...)** and invoke the function **TVDImodule.computeTVDI(...)**

``` r
// Define the Region of Interest
var ROI = ee.FeatureCollection(...)

// Obtain the NDVI image to be processed
var imageNDVI = ee.Image(...)

// Obtain the LST image to be processed
var imageLST = ee.Image(...)

// Define the spatial scale to resample NDVi and LST images
var SCALE_M_PX = 250

// Define the DEBUG flag - If the function will print the results during processing
var DEBUG_FLAG = false

// Import TVDI processing module
var TVDImodule = require('AAAAAAALLLLLLTERARAAAAAAAAAAAAAAAAAAAAAAAusers/leobeckerdaluz/TVDI_algorithm_dev:compute_TVDI');

var imageTVDI = TVDImodule.computeTVDIAAAAAALLLLLLLLLLTERRRR(imageNDVI, imageLST, ROI, SCALE_M_PX, DEBUG_FLAG)
```


## Example

Look at this simple example to estimate the NDVI from a Landsat-8 Surface Reflectance image.

```

With [**rgeeExtra**](https://github.com/r-earthengine/rgeeExtra):

``` r
library(rgee)

ee_Initialize()

img <- ee$Image("LANDSAT/LC08/C01/T1_SR/LC08_038029_20180810")
ndvi <- (img[["B5"]] - img[["B4"]])/(img[["B5"]] + img[["B4"]])**2
names(ndvi) <- "pow_ndvi"

Map$centerObject(img,zoom=12)
Map$addLayer(img)
```



## Share the love

Think **rgeeExtra** is useful? Let others discover it, by telling them in person via a blog post.

Using **rgeeExtra** for a paper you are writing? Consider citing it

``` r
citation("rgeeExtra")
To cite rgee in publications use:
  
  C Aybar, Q Wu, L Bautista, R Yali and A Barja (2020) rgee: An R
  package for interacting with Google Earth Engine Journal of Open
  Source Software URL https://github.com/r-spatial/rgee/.

A BibTeX entry for LaTeX users is

@Article{,
  title = {rgee: An R package for interacting with Google Earth Engine},
  author = {Cesar Aybar and Quisheng Wu and Lesly Bautista and Roy Yali and Antony Barja},
  journal = {Journal of Open Source Software},
  year = {2020},
}
```
