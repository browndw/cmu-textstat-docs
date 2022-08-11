# `vnc_scree`

This is a simple function to return a scree plot based on the VNC algorithm.


## Description

This is a simple function to return a scree plot based on the VNC algorithm.


## Usage

```r
vnc_scree(time, values, distance.measure = c("sd", "cv"))
```


## Arguments

Argument      |Description
------------- |----------------
`time`     |     A vector of sequential time intervals like years or decades
`values`     |     A vector containing normaized frequency counts
`distance.measure`     |     Indicating whether the standard deviation or coefficient of variation should be used in dinstance calculations


## Value

A scree plot


