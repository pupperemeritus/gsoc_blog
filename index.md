# pupperemeritus's GSoC Blog

## Interested Project

### Astronomy With Unevenly Sampled Data

Analyzing unevenly sampled data cannot be done with methods of the fourier domain. We use the lomb scargle domain to account for the unevenness of the data. I will be mainly implementing 3 classes, Lomb Scargle cross spectrum, power spectrum and dynamical power spectrum and in that order. I will be translating the implementation provided by Dr JD Scargle in MATLAB or FORTRAN into python. If the PSD arises as a special case of the CSD, then I would be reusing my CSD implementation otherwise I would be using the scipy.signal.LombScargle implementation to reduce development time. Later in the project after I complete the implementation of the 3 classes, I shall optimize the Lomb Scargle cross spectrum implementation by using JAX. Reason for choosing the scipy implementation is that it allows us to get an unnormalized PSD and astropy's implementation does not have that option, and their normalizations are different from the ones used in stingray. There would also be a need of helper functions to create objects of these classes automatically from various input types similar to the existing PSDs.
