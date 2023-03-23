# Application

## Proposal title

Astronomy Using Unevenly Sampled Data

## Organization

StingraySoftware

## Summary

Analyzing unevenly sampled data cannot be done with methods of the fourier domain. We use the lomb scargle domain to account for the unevenness of the data. I will be mainly implementing 3 classes, Lomb Scargle cross spectrum, power spectrum and dynamical power spectrum and in that order. I will be translating the implementation provided by Dr JD Scargle in MATLAB or FORTRAN into python. If the PSD arises as a special case of the CSD, then I would be reusing my CSD implementation otherwise I would be using the scipy.signal.LombScargle implementation to reduce development time. The Lomb Scargle cross spectrum implementation can be optimized by using JAX. Reason for choosing the scipy implementation is that it allows us to get an unnormalized PSD to which we can apply stingray's methods of normalization and astropy's implementation does not have that option, and their normalizations are different from the ones used in stingray. There would also be a need of helper functions to create objects of these classes automatically from various input types similar to the existing PSDs. The crosspectrum class must be compatible with statistic functions already existing in stingray such as the time lag,phase lag and coherence (which exist only for crossspectra).

## Contributor Information

- Blog : <https://pupperemeritus.github.io/gsoc_blog/>

- Name : Sri Guru Datta Pisupati

- Email : pupperemeritus9@protonmail.com

- Time-zone : +05:30 (IST)

- Slack : Sri Guru Datta Pisupati

- Github : pupperemeritus

- PR Links :

  - [notebooks pull request 64](https://github.com/StingraySoftware/notebooks/pull/64)
  - [stingray pull request 708](https://github.com/StingraySoftware/stingray/pull/708)
  - [stingray pull request 710](https://github.com/StingraySoftware/stingray/pull/710)

## Background

I am an undergraduate student of Chaitanya Bharathi Institute of Technology studying Artificial Intelligence and Data Science. I have a lot hobbies out of which some of them include astrophotography, Guitar(Rock and Pop Trinity Grade 8) and chess. I originally wanted to study a course in Computational Astronomy/Astrophysics as it is the confluence of all my interests but could not do it due to lack of availability of such a course in my country. I am passionate about using, propogating and contributing to open source software. I also like to do astrophotography in my free time, though I cannot really produce great images as I am limited by a Bortle 9 sky and lack of a tracker.

## Interest in Open Astronomy

I've always been interested in astronomy. I also delved into astrophotography recently. I originally wanted to study astrophysics/astronomy along with computer science as my major and minor but the institutes in my country neither offer such flexibility nor the courses in one institute. I have done a few courses on coursera on astronomy such as Data Driven Astronomy and Analyzing The Universe. I am passionate about using and propogating open source software, and I feel this is a multi-faceted way of giving back to the community that I like so much. I feel my skill set is best suited for OpenAstronomy compared to any other project as I have a decent amount of domain as well as programming and software design knowledge. I am always looking to learn more. I view research work and work that aids in other's research as a noble pursuit. I hope OpenAstronomy will not only be my journey into the world of FOSS but also to research and helps me gain experience for higher studies.

## My approach

### Proposed Structure

Filename                    | Description
--------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
LombScargle.py              | contains the translation of crossspectrum implementation into python
LombScargleCrossspectrum.py | Using our custom crossspectrum implementation in LombScargle.py creating a class that can take in Light curve and eventlist data and create the cross spectrum
LombScarglePowerSpectrum.py | using existing implementation in astropy creating the power spectrum or using our cross spectrum implementation if power spectrum arises as a special case of power spectrum

### Lomb Scargle module

Containing the core computational parts. Translating the MATLAB or FORTRAN version of crossspectrum given by Dr. Jeffrey D Scargle into Python. Using JAX for improved speed after implementing in regular python.

### Lomb Scargle Cross Spectrum

Wraps the Lomb Scargle cross spectrum in the above LombScargle implementation. Handles inputs of two light curves, eventlists, timearray and light curve iterables. Needs to perform data validity checks and normalization(leahy/absolute rms/fractional rms).

Class parameters

Parameter   | Description                                       | Default
----------- | ------------------------------------------------- | --------------
data1       | light curve or eventlist                          | None
data2       | light curve or eventlist                          | None
power_type  | real or complex power                             | real
norm        | Normalization methods(frac RMS, abs RMS, leahy)   | Fractional RMS
dt          | time resolution only needed for eventlist objects | None
gti         | time intervals which have good data               | None
skip_checks | flag to skip checks                               | False

Class attributes

Attribute   | Description
----------- | ----------------------------------------------------------------------
freq        | array of mid bin frequencies that the lsft samples
power       | array of cross spectra (complex numbers)
power error | uncertainties in power
df          | frequency resolution
m           | the number of averages cross spectra amplitudes in each bin
n           | the number of datapoints/time bins in one segment of the light curves
k           | the rebinning scheme if object has been rebinned otherwise is set to 1
nphots1     | the total number of photons in light curve 1
nphots2     | the total number of photons in light curve 2

NOTE : This is a rough outline and the attributes and parameters may change depending on further research at the time of implementation

### Lomb Scargle Power Spectrum

With the help of scipy.signal.LombScargle, this class creates the power spectrum. Handles input of one light curve. Helper functions that allow creation of PSD from timearray, eventlists and light curve iterables must be created. And must perform validity checks and normalization(leahy/absolute rms/fractional rms).

Class parameters

Parameter   | Description                                       | Default
----------- | ------------------------------------------------- | --------------
data        | light curve or eventlist                          | None
power_type  | real or complex power                             | real
norm        | Normalization methods(frac RMS, abs RMS, leahy)   | Fractional RMS
dt          | time resolution only needed for eventlist objects | None
gti         | time intervals which have good data               | None
skip_checks | flag to skip checks                               | False

Class attributes

Attribute   | Description
----------- | ----------------------------------------------------------------------
freq        | array of mid bin frequencies that the lsft samples
power       | array of cross spectra (complex numbers)
power error | uncertainties in power
df          | frequency resolution
m           | the number of averages cross spectra amplitudes in each bin
n           | the number of datapoints/time bins in one segment of the light curves
k           | the rebinning scheme if object has been rebinned otherwise is set to 1
nphots1     | the total number of photons in light curve 1
nphots2     | the total number of photons in light curve 2

NOTE : This is a rough outline and the attributes and parameters may change depending on further research at the time of implementation

## Detailed Description

The project involves time series analysis of unevenly sampled data. The goal is to create the classes that generates Lomb-Scargle Power spectrum and Crosspectrum of the data respectively and match these implementations with the structure of existing functionalities of Powerspectrum and Crosspectrum classes as well as the normalisations performed by them.

Time-lag, phase lag and coherence of light curves are also to be found out in the case of cross spectra.

The implementation of cross spectrum is to be done from scratch in Python as it only exists in MATLAB and FORTRAN courtesy of Dr. Jeffrey D. Scargle ([Studies in astronomical time series analysis. III-Fourier transforms, autocorrelation functions, and cross-correlation functions of unevenly spaced data](https://adsabs.harvard.edu/full/1989ApJ...343..874S)).

The implementation of power spectrum is not required since a compatible implementation exists in scipy.signal.LombScargle. We can refer to the original paper by Jeffrey D. Scargle [Studies in astronomical time series analysis. II-Statistical aspects of Spectral Analysis of Unevenly Spaced Data](https://adsabs.harvard.edu/full/1982ApJ...263..835S7)

## Deliverables

1. LombScargleCrossSpectrum class
2. LombScarglePowerSpectrum class

## Description and Timeline

Period                      | Description
--------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
May 4th - 28th              | Getting to know mentors, reading documentation, setup development environment, contributing fixes to smaller issues to get familiar with the codebase and getting up to speed with working on the project.
May 29th - June 4th         | Translating the Lomb Scargle Crossspectrum implementation into python
June 5th - June 19th        | Implementing the Lomb Scargle Crossspectrum class
June 19th - July 2nd        | Implementing Lomb Scargle Power Spectrum class
July 3rd - July 16th        | Implementing the Lomb Scargle Dynamical Power Spectrum class
July 14th                   | Midterm evaluation deadline
July 31st - August 13th     | JAX optimization
August 13th - August 21st   | Buffer period and improving code quality, testing, and documentation
August 21st - August 28th   | Final evaluation week
August 28th - September 4th | Mentor final evaluation week

## GSoC

I have not participated previously in GSoC. I am not applying to any other projects as this project has piqued my interest the most and aligns the most with my knowledge and skillset.

## Schedule availability

I don't really have any plans for holidays during the time of the program. There are barely any holidays between my 4th and 5th sems which would be around 10-15 days in late August early September. I would be willing to work during these days as well if need be. I would have slightly reduced output from 13th July to Mid/Late August due to semester end exams. My window of maximum productivity would be from May 29th ~ June 25th. I would not like to put behind any work for the buffer period as it clashes with my semester end exams. I would most likely be to work at full productivity from roughly 11th August onward assuming exam schedule would be similar to current semester. I would try and cover the work of later weeks early if any unforeseen circumstances arise.
