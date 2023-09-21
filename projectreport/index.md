---
title: GSoC 2023 Project Report
layout: report
---

# Astronomy Using Unevenly Sampled Data

# About the Project

In astronomy, its common for data to be imperfect and unevenly sampled. This is due to various observational factors such as obscuration of subject, instrumental limitations, transmission issues etc. In X-ray astronomy objects of interest often include high energy events such as Gamma Ray Bursts, Pulsar/Magnetar Flares, Stellar Flares, X-ray binaries etc. These are often highly periodic signals especially for gamma ray bursts and pulsar/magnetar flares. Analyzing unevenly sampled data cannot be done with the regular Fourier transform. Lomb Scargle Fourier transform is a modification of the fourier transform for unevenly sampled data. It works by accounting for the uneven sampling intervals. This project is quite complementary to Gaurav Joshi's project ["Quasi Periodic Oscillation detection using Gaussian Processes"](https://gaurav17joshi.github.io/contrast/project-report/). The Lomb Scargle method of detecting oscillations is several orders of magnitude faster than quasi periodic oscillation detection using Gaussian Processes method. Thus my project can be used for a rougher analysis of a lot of unevenly sampled data out of which the more interesting candidates can be sifted through and analyzed further using Gaurav's project. This project actually contains implementation of two versions of the same algorithm one is the orignal slow algorithm introduced in the [original paper](https://adsabs.harvard.edu/full/1982ApJ...263..835S7) and the fast algorithm introduced in [this paper](https://ui.adsabs.harvard.edu/abs/1989ApJ...338..277P/abstract)

For greater understanding please refer [Understanding the Lomb-Scargle Periodogram](https://doi.org/10.3847/1538-4365/aab766).

## Goals of the project

- Implementing the Lomb Scargle Fourier transform method
- Implementing the Lomb Scargle cross spectrum class
- Implementing the Lomb Scargle power spectrum class
- Testing the above code
- Documentation

# Code and Submission

My code is present in this pull request [StingrayPR#737](https://github.com/StingraySoftware/stingray/pull/737)

The usage of my code can be demonstrated in these [notebooks](https://gist.github.com/pupperemeritus/cd9ed30315c36818b75730905a6dc432)

# Future Work

1. Further simplifying and optimizing the code(Multithreading, Memory optimization, enabling usage of GPUs etc).
2. Adding more documentation and making the existing documentation more clearer. 3.

# Maths Behind the Project

The Lomb Scargle method uses the mean of the sampling intervals to account for the uneven sampling intervals. Lomb Scargle method is equivalently interpreted as a Fourier method as well as a least squares method. The generalized periodogram form ensures time shift invariance.

This project uses the Lomb Scargle implementation based on the Fourier interpretation. $$ P(f) = \frac{1}{2} [{ \frac{ (\sum_n{g_n\cos(2\pi f[t_n-\tau])})^2} {\sum_n\cos^2(2\pi f[t_n-\tau])} + \frac{(\sum_ng_n\sin(2\pi f[t_n-\tau]))^2} {\sum_n\sin^2(2\pi f[t_n-\tau]) }}] $$ where $$ \ \tau =\frac{1}{4\pi f} \tan^{-1} {\frac{\sum_n\sin(4\pi f_n)}{\sum_n{\cos(4\pi f_n)}}} $$

The above is further optimized by using the [optimizations introduced by William H. Press and George B. Rybicki](https://articles.adsabs.harvard.edu/pdf/1989ApJ...338..277P) which brings down the overall time complexity of the algorithm to $$ \mathcal{O}(N\log{}N) $$

The optimizations basically include the following:

- Simplifications of the following trigonometric sums if we define

  $$S_y = \sum_j y_j \sin(\omega t_j)$$

  $$C_y = \sum_j y_j \cos(\omega t_j)$$

  $$S_2 = \sum_j \sin(2 \omega t_j)$$

  $$C_2 = \sum_j \cos(2 \omega t_j)$$

- Then we can replace

  $$ \sum_j y_j \cos(\omega (t_j - \tau)) = C_y \cos(\omega \tau) + S_y \sin(\omega \tau) $$

  $$ \sum_j y_j \sin(\omega (t_j - \tau)) = S_y \sin(\omega \tau) - C_y \sin(\omega \tau) $$

  $$ \sum_j \cos^2(\omega (t_j - \tau)) = 0.5 (N + C_2 \cos(2 \omega \tau) + S_2 \sin(2 \omega \tau)) $$

  $$ \sum_j \sin^2(\omega (t_j - \tau)) = 0.5 (N - C_2 \cos(2 \omega \tau) - S_2 \sin(2 \omega \tau)) $$

- Where $$ \omega\ is\ 2 \pi f $$ and N is the number of samples.

- If the $$ t_j $$'s were evenly spaced then the above calculations could be performed by FFTs

- Therefore, we extirpolate the values at evenly spaced $$ t_j $$'s by using extirpolation weight which is the same as interpolation weight

  $$ g(t) = \sum_n w_n(t) g(\hat{t}_n) $$ where $$ w_n(t) $$ are interpolation weights and $$ \hat{t}_n $$ is the extirpolated evenly spaced points

- Our sum of interest

  $$ \sum_n y_n g(t_n) \approx \sum_j y_j[\sum_k(w_k(t_j) g(\hat{t}_k))] = \sum_k\sum_j h_j w_k(t_j) \equiv \sum_k \hat{h}_kg(\hat{t}_k)$$ where $$ \hat{h}_k = \sum_j h_j w_k(t_j) $$

# Challenges faced and lessons learnt

1. Implementing the Lomb Scargle fourier transform was quite challenging to get right. It was easy to get SOME output but getting sensible output was very difficult. Almost 1.5 months was spent on this.
2. Pivoting the class from the legacy interface to the modern interface followed by stingray. I had created the LombScargle classes using the legacy interface. It took a bit of relearning to think of the class in the modern interface. The legacy interface had a lot more monolithic structure whereeas the new interface from the AveragedCrossspectrum and the likes is more modular as well as clean.
3. Writing meaningful tests has been quite a new experience for me as I had not written any tests for my code before. I learnt a lot about them.
4. I learnt a lot about CI/CD processes for testing and deploying code. The different testing tools like pytest, codecov, black formatter etc were used a lot.

# Acknowledgements

I am very grateful to my mentors [Daniela Huppukothen](https://github.com/dhuppenkothen) and [Matteo Bachetti](https://github.com/matteobachetti) for their extremely valuable feedback and guidance. Especially I would like to thank them for their patience and hands-on help with some parts where I was having trouble getting things to work. Their guidance not only helped me complete my own projec but also make a few contributions to the library outside my project as well.
