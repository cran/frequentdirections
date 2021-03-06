
<!-- README.md is generated from README.Rmd. Please edit that file -->

# frequentdirections [![Build Status](https://travis-ci.com/shinichi-takayanagi/frequentdirections.svg?branch=master)](https://travis-ci.com/shinichi-takayanagi/frequentdirections)

Implementation of Frequent-Directions algorithm for efficient matrix
sketching \[E. Liberty, SIGKDD2013\]

## Installation

``` r
# Not yet onCRAN
install.packages("frequentdirections")

# Or the development version from GitHub:
install.packages("devtools")
devtools::install_github("shinichi-takayanagi/frequentdirections")
```

## Example

### Download example data

Here, we use [Handwritten digits USPS
dataset](https://www.kaggle.com/bistaumanga/usps-dataset/version/1) as
sample data. In the following example, we assume that you save the above
sample data into `/tmp` directory.

### Load data

The dataset has 7291 train and 2007 test images in `h5` format. The
images are 16\*16 grayscale pixels.

``` r
library("h5")
file <- h5file("/tmp/usps.h5")
x <- file["train/data"][]
y <- file["train/target"][]
str(x)
#>  num [1:7291, 1:256] 0 0 0 0 0 0 0 0 0 0 ...
```

### Plot example image

Example the number `8`

``` r
image(matrix(x[338,], nrow=16, byrow = FALSE))
```

![](man/figures/README-plot-example-image-1.png)<!-- -->

### Plot SVD

Plot the original data on the first and second singular vector plane.

``` r
x <- scale(x)
frequentdirections::plot_svd(x, y)
```

![](man/figures/README-plot-svd-1.png)<!-- -->

### Matrix Sketching

#### l = 8 case

``` r
eps <- 10^(-8)
# 7291 x 256 -> 8 * 256 matrix
b <- frequentdirections::sketching(x, 8, eps)
frequentdirections::plot_svd(x, y, b)
```

![](man/figures/README-frequentdirections-8-1.png)<!-- -->

#### l = 32 case

``` r
# 7291 x 256 -> 32 * 256 matrix
b <- frequentdirections::sketching(x, 32, eps)
frequentdirections::plot_svd(x, y, b)
```

![](man/figures/README-frequentdirections-32-1.png)<!-- -->

#### l = 128 case

``` r
# 7291 x 256 -> 128 * 256 matrix
b <- frequentdirections::sketching(x, 128, eps)
frequentdirections::plot_svd(x, y, b)
```

![](man/figures/README-frequentdirections-128-1.png)<!-- -->

This result is almost the same with the original data SVD expression.

That’s why we can think that the original data is expressed with only
`128` rows.
