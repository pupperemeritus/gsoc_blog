---
layout: page
---

# pupperemeritus's GSoC Blog

## Interested Project

### Astronomy With Unevenly Sampled Data

The project involves time series analysis of unevenly sampled data. The goal is to create the class that generates Lomb-Scargle spectrum of the data and match these implementations with the structure of existing functionalities of Powerspectrum and Crosspectrum classes as well as the normalisations performed by them.

There are multiple possible implementations as there are multiple implementations with different time complexities. It would be best to choose the existing implementation of Lomb-Scargle in astropy as it it has many implementation neatly integrated. Out of these it would be best to give the user the option to choose the algorithm. Or if chosen to be monolithic, the scipy version would be the best choice in combination of speed and accuracy.

The following are the implmentations present in astropy.timeseries.LombScargle

- original Lomb Scargle
- Cython version of original
- Scipy C version of original
- FFT based fast version
- matrix algebra based chi^2
- FFT based chi^2

The following normalizations are available with the implementation

- Standard Normalization
- Model Normalization
- Logarithmic Normalization
- PSD Normalization The false_alarm_probability method is used to determine the probability of a peak as high as the input or higher.

The major chunk of the work would be to get the wrap the Lomb-Scargle class of astropy in a way that works with input types stingray.Lightcurve object, array of stingray.Lightcurve objects,Time array and EventList.
