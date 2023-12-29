---
title: 'SigPy.RF: Comprehensive Python-Based MRI RF Pulse Design Tools'
tags:
  - Python
  - magnetic resonance imaging (MRI)
  - signal processing
  - RF pulse
  - radiofrequency pulse design
  - adiabatic pulse
  - parallel transmission (pTx)
  - multi-slice imaging
  - SLR pulse design
authors:
  - name: Jonathan B. Martin
    orcid: 0000-0002-9384-8056
    equal-contrib: false
    affiliation: 1 # (Multiple affiliations must be quoted)
  - name: Jonathan I. Tamir
    equal-contrib: false
    affiliation: 2
  - name: Heng Sun
    equal-contrib: false
    affiliation: 3
  - name: Madison Albert
    equal-contrib: false
    affiliation: 4
  - name: Michael Lustig
    equal-contrib: false
    affiliation: 5
  - name: Frank Ong
    equal-contrib: false
    affiliation: 6
  - name: William A. Grissom
    corresponding: true # (This is how to denote the corresponding author)
    affiliation: 7
affiliations:
 - name: Vanderbilt University Medical Center
   index: 1
 - name: University of Texas
   index: 2
 - name: Yale University
   index: 3
 - name: Vanderbilt University
   index: 4
 - name: University of California, Berkeley
   index: 5
 - name: Roblox Corporation
   index: 6
 - name: Case Western Reserve University
   index: 7
date: 26 Mar 2023
bibliography: paper.bib

---

# Summary
We present SigPy.RF, an extensive set of open-source, Python-ba­­sed tools for MRI RF pulse design. This toolbox extends the SigPy Python software package and leverages SigPy’s existing capabilities for GPU computation, iterative optimization, and powerful abstractions for linear operators, proximal operators, and applications. Tools are available for all steps of the excitation design process including trajectory/gradient design, pulse design, and simulation. Our implemented functions for pulse design include advanced SLR, multiband, adiabatic, optimal control, B$_1^+$-selective and small-tip pTx designers. Preliminary development of this toolbox was presented in reference [@Martin2020a]; the current version has been streamlined and expanded to include a larger collection of RF pulse design methods from the literature, as well as additional utility tools for I/O, gradient waveform design, and experimental B$_1^+$-selective pulse design algorithms which were prototyped using the initial SigPy.RF codebase, enabling the publication of Reference [@Martin2022].

# Statement of need
The field of magnetic resonance imaging is currently experiencing rapid growth in the num-
ber of available open source imaging tools. Tools have been made freely available for MRI hardware development [@Amrein2022; @Anand2018], system simulation [@Villena2014; @Stocker2010], pulse sequence pro-
gramming [@Layton2017], image reconstruction [@Ong2019; @Uecker2015], and post-processing and analysis [@Avants2014; @Duval2018].
However, one critical step of the imaging pipeline which has seen limited open-source tool de-
velopment is RF pulse design.  While RF pulse developers increasingly share code online
in independent repositories, no unified set of common pulse design tools has been main-
tained in a rigorous and consistent manner with easy-to-read code and tutorials. This is despite the reality that in many cases, carefully designed or application-specific RF pulses are crucial to the success of MRI techniques. An open
source pulse design code library would facilitate the development and dissemination of
novel techniques and the comparison of approaches, similar to how BART [@Uecker2015] and SigPy
[@Ong2019] have made advanced parallel imaging and recon-
struction methods widely accessible. To meet this need, we have developed a library of
pulse design tools as part of the SigPy Python package for signal processing and image re-
construction [@Ong2019]. We call this new package SigPy.RF. SigPy.RF is constructed as a nested
package within the broader SigPy package. Figure 1 illustrates where the RF pulse design tools fit into the broader SigPy software, as a nested package within the package for general MRI tools.

%![hierarchy](https://github.com/jonbmartin/sigpyrf-joss/blob/main/sigpy-hierarch.png)

|New RF pulse design tools (red) within the SigPy package hierarchy. The pulse
 design tools are grouped in a series of modules dedicated to a specific pulse class or type
of RF pulse design utility|

# About SigPy.RF
SigPy has a number of features that make it an ideal candidate to support a nested RF
pulse design package. Image reconstruction and RF pulse design require many of the same
operators and computational tools; for example, SENSE reconstruction [@Pruessmann] and small-tip
parallel transmit pulse design [@Grissom2006] are in many cases parallel processes moving in the oppo-
site directions between the spatial and frequency domains, with the same or similar linear
operators and the same requirements for iterative optimization tools. Thus critical tools
such as Fourier operators, conjugate gradient iterative optimizers, and matrix manipulation
methods are provided in SigPy [@Ong2019].
SigPy was further built with a number of computational features that make it an ideal fit
for RF pulse design. As a Python library, SigPy in general should not be expected to match
the performance of comparable C libraries such as BART [@Uecker2015]. However, SigPy uses
Numba [@Lam2015] to translate many of its’ most commonly used tools, such as gridding functions
to optimized machine code at runtime. Optimizations that are dominated by FFTs, such
as small-tip spatial domain pTx pulse designs, can closely match the performance of C
toolboxes since the same low-level C FFT libraries are used [@Ong2019]. Additionally, SigPy has
a general unified CPU and GPU interface for most functions, allowing for easy movement
of data between devices for computational flexibility and acceleration of computation with
GPUs. Figure 1 shows the mean computation times for a small-tip-angle spatial domain parallel transmit pulse design [@Grissom2006] on CPU and GPU. Design time was approximately an order of magnitude faster across all problem dimensions when the GPU was used versus CPU. 

%![Mean execution time for small-tip spatial domain pulse design using SigPy
%linear operators and CuPy matrices across all considered problem dimensions, on CPU and
%GPU](https://github.com/jonbmartin/sigpyrf-joss/blob/main/execution_time_white.png?raw=true)

| Mean execution time for small-tip spatial domain pulse design using SigPy
linear operators and CuPy matrices across all considered problem dimensions, on CPU and
GPU |

# Target Audience

The SigPy.RF toolbox has been developed for the use of MRI researchers focusing on pulse sequence design, MRI physics, signal processing and optimization. We believe that it will serve as an essential building block of more general image acquisition tools which require specialized RF pulses. The toolbox has already been incorporated into open-source sequence development software such as Pulseq [@Layton2017] and PyPulseq [@SravanRavi2019] to provide RF pulses critical to the performance of various pulse sequences. An example is shown in Figure 3. 

%![pypulseq](https://github.com/jonbmartin/sigpyrf-joss/blob/main/pulseq-sigpy_cropped.png)

| First TR of a GRE pulse sequence created in Pulseq [@Layton2017] with SigPy.RF-designed SLR excitation
pulse. Magnitude of the TB = 4 , 90 degree SLR pulse is plotted in the ‘RF mag’ plot, middle left. |

We also envision SigPy.RF may be utilized for replicability and
reproducibility studies, allowing for standardized conventions for RF pulses to be used across studies. The package could also serve as a hands-on teaching aid for MRI faculty and students. We have developed [several tutorials](https://github.com/jonbmartin/sigpy-rf-tutorials), which are accessible to a wide audience with minimal prior MRI knowledge. 

# Availability

The latest version of SigPy includes the most stable pulse
design tools to have been developed and is available from 
[the main repository](https://github.com/mikgroup/sigpy). It can be installed through conda or pip- see the [documentation](https://sigpy.readthedocs.io/en/latest/) for more details.
The [SigPy.RF fork](https://github.com/jonbmartin/sigpy-rf) includes pulse design tools that are still in progress, prior to being merged into the main codebase. Jupyter notebook [pulse design
tutorials](https://github.com/jonbmartin/sigpy-rf-tutorials) for SigPy.RF are available.

# Acknowledgements

We gratefully acknowledge the support this study received from NIH Grant R01 EB 016695.

# References
