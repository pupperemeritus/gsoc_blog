# Application

## Contributor Information

- Name : Sri Guru Datta Pisupati
- Email : pupperemeritus9@protonmail.com
- Time-zone : +05:30 (IST)
- Slack : Sri Guru Datta Pisupati
- Github : pupperemeritus
- Blog : <https://pupperemeritus.github.io/gsoc_blog/>
- PR Links :

  - [notebooks pull request 64](https://github.com/StingraySoftware/notebooks/pull/64)
  - [stingray pull request 708](https://github.com/StingraySoftware/stingray/pull/708)
  - [stingray pull request 710](https://github.com/StingraySoftware/stingray/pull/710)

## Background

I am an undergraduate student of Chaitanya Bharathi Institute of Technology studying Artificial Intelligence and Data Science. I have a lot hobbies out of which some of them include astrophotography, Guitar(Rock and Pop Trinity Grade 8) and chess. I originally wanted to study a course in Computational Astronomy/Astrophysics as it is the confluence of all my interests but could not do it due to lack of availability of such a course in my country. I am passionate about using, propogating and contributing to open source software. I also like to do astrophotography in my free time, though I cannot really produce great images as I am limited by a Bortle 9 sky and lack of a tracker.

## Interest in Open Astronomy

I've always been interested in astronomy. I also delved into astrophotography recently. I originally wanted to study astrophysics/astronomy along with computer science as my major and minor but the institutes in my country neither offer such flexibility nor the courses in one institute. I have done a few courses on coursera on astronomy such as Data Driven Astronomy and Analyzing The Universe. I am passionate about using and propogating open source software, and I feel this is a multi-faceted way of giving back to the community that I like so much. I feel my skill set is best suited for OpenAstronomy compared to any other project as I have a decent amount of domain as well as programming and software design knowledge. I am always looking to learn more. I view research work and work that aids in other's research as noble pursuits. I hope OpenAstronomy will not only be my journey into the world of FOSS but also to research and help me gain experience for higher studies.

## Project Proposal Application

### Proposal title : Astronomy Using Unevenly Sampled Data

### Organization : StingraySoftware

## Abstract

Data is often imperfect and sampled at irregular intervals. Methods of Fourier domain require the data to be observed at evenly spaced intervals, reducing the amount of time we can observe a certain X-Ray or Gamma ray source. For some targets it is impossible to observe the data at regular intervals for a long period of time normally due to blocking of the source by Earth. Lomb Scargle Periodograms are used to detect periodic signals in unevenly spaced data. There are various algorithms for implementing Lomb-Scargle periodogram mainly based on Matrix Algebra, Fast Orthogonal Search and Chi-Squared Method.

## Detailed Description

The project involves time series analysis of unevenly sampled data. The goal is to create the class that generates Lomb-Scargle spectrum of the data and match these implementations with the structure of existing functionalities of Powerspectrum and Crosspectrum classes as well as the normalisations performed by them.

There are multiple possible implementations as there are multiple implementations with different time complexities. It would be best to choose the existing implementation of Lomb-Scargle in astropy as it it has many implementation neatly integrated. Out of these it would be best to give the user the option to choose the algorithm. Or if chosen to be monolithic, the scipy version would be the best choice in combination of speed and accuracyZ.

The following are the implmentations present in astropy.timeseries.LombScargle

- original Lomb Scargle
- Cython version of original
- Scipy C version of original
- FFT, Fast Orthogonal Search based fast version using extrapolation approach
- matrix algebra based chi^2
- FFT based chi^2

The following normalizations are available with the implementation

- Standard Normalization
- Model Normalization
- Logarithmic Normalization
- PSD Normalization

The false_alarm_probability method is used to determine the probability of a peak greater than or equal to the peak in the data assuming data consists of gaussian noise with no periodic component.

The major chunk of the work would be to get the wrap the Lomb-Scargle class of astropy in a way that works with input types stingray.Lightcurve object, array of stingray.Lightcurve objects,Time array and EventList.

## Deliverables

1. LombScargle class
2. Consistent behaviour with PowerSpectrum and Crosspectrum classes on input of different types of data
3. AveragedLombScargle class

## Description and Timeline

Period                      | Description
--------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
May 4 - 28th                | Getting to know mentors, reading documentation, setup development environment, contributing fixes to smaller issues to get familiar with the codebase and getting up to speed with working on the project.
May 29 - June 5th           | Creating sample implementations of various Lomb-Scargle Classes and evaluating physical correctness
June 6th - June 12th        | Choosing an implementation and finalizing the API of the class
June 13th - June 25th       | Making the class feature complete w.r.t PowerSpectrum class
June 26th - July 1st        | Making the class feature complete w.r.t PowerSpectrum class and possibly making an AveragedLombScargle class depending if it is useful
July 2nd - July 8th         | Writing Tests
July 9th - July 13th        | Adding Type Hints
July 10th - July 14th       | Midterm evaluation deadline
July 14th - August 21st     | Buffer period and improving code quality and more testing
August 21st - August 28th   | Final evaluation week
August 28th - September 4th | Mentor final evaluation week

## GSoC

I have not participated previously in GSoC. I am not applying to any other projects as this project has piqued my interest the most and aligns the most with my knowledge and skillset.

## Schedule availability

I don't really have any plans for holidays during the time of the program. There are barely any holidays between my 4th and 5th sems which would be around 10-15 days in late August early September. I would be willing to work during these days as well if need be. I would have slightly reduced output from 13th July to Mid/Late August due to semester end exams. My window of maximum productivity would be from May 29th ~ June 25th. I would not like to put behind any work for the buffer period as it clashes with my semester end exams. I would most likely be to work at full productivity from roughly 11th August onward assuming exam schedule would be similar to current semester. I would try and cover the work of later weeks if any unforeseen circumstances arise.
