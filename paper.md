---
title: 'SigPy.RF: Comprehensive Python MRI RF Pulse Design Tools'
tags:
  - Python
  - MRI
  - signal processing
  - RF pulse
  - radiofrequency pulse
authors:
  - name: Jonathan B. Martin
    orcid: 0000-0002-9384-8056
    equal-contrib: false
    affiliation: 1 # (Multiple affiliations must be quoted)
  - name: Jonathan I. Tamir
    affiliation: 2
  - name: Michael Lustig
    affiliation: 3
  - name: Frank Ong
    affiliation: 4
  - name: William A. Grissom
    corresponding: true # (This is how to denote the corresponding author)
    affiliation: 5
affiliations:
 - name: Vanderbilt University Medical Center
   index: 1
 - name: University of Texas
   index: 2
 - name: Roblox Corporation
   index: 3
 - name: University of California, Berkeley
   index: 4
 - name: Case Western Reserve University
   index: 5
date: 26 Mar 2023
bibliography: paper.bib

---

# Summary
We present SigPy.RF, an extensive set of open-source, Python-ba­­sed tools for MRI RF pulse design. This toolbox extends the SigPy Python software package and leverages SigPy’s existing capabilities for GPU computation, iterative optimization, and powerful abstractions for linear operators, proximal operators, and applications. Tools are available for all steps of the excitation design process including trajectory/gradient design, pulse design, and simulation. Our implemented functions for pulse design include advanced SLR, multiband, adiabatic, optimal control, B1+-selective and small-tip pTx designers.

# Statement of need
The field of magnetic resonance imaging is currently experiencing rapid growth in the num-
ber of available open source imaging tools. Tools have been made freely available for MRI hardware development (1; 2), system simulation (3; 4), pulse sequence pro-
gramming (5), image reconstruction (6; 7), and post-processing and analysis (8; 9)
However, one critical step of the imaging pipeline which has seen limited open-source tool de-
velopment is RF pulse design.  While RF pulse developers increasingly share code online
in independent repositories, no unified set of common pulse design tools has been main-
tained in a rigorous and consistent manner with easy-to-read code and tutorials. This is despite the reality that in many cases, carefully designed or application-specific RF pulses are crucial to the success of MRI techniques. An open
source pulse design code library would facilitate the development and dissemination of
novel techniques and the comparison of approaches, similar to how BART (7) and SigPy
(6) have made advanced parallel imaging and recon-
struction methods widely accessible. To meet this need, we have developed a library of
pulse design tools as part of the SigPy Python package for signal processing and image re-
construction (6). We call this new package SigPy.RF. SigPy.RF is constructed as a nested
package within the broader SigPy package. Figure 1 illustrates where the RF pulse design tools fit into the broader SigPy software, as a nested package within the package for general MRI tools.

![hierarchy](https://github.com/jonbmartin/sigpyrf-joss/blob/main/sigpy-hierarch.PNG)

|New RF pulse design tools (red) within the SigPy package hierarchy. The pulse
 design tools are grouped in a series of modules dedicated to a specific pulse class or type
of RF pulse design utility|

# About SigPy.RF
SigPy has a number of features that make it an ideal candidate to support a nested RF
pulse design package. Image reconstruction and RF pulse design require many of the same
operators and computational tools; for example, SENSE reconstruction (10) and small-tip
parallel transmit pulse design (11) are in many cases parallel processes moving in the oppo-
site directions between the spatial and frequency domains, with the same or similar linear
operators and the same requirements for iterative optimization tools. Thus critical tools
such as Fourier operators, conjugate gradient iterative optimizers, and matrix manipulation
methods are provided in SigPy (6).
SigPy was further built with a number of computational features that make it an ideal fit
for RF pulse design. As a Python library, SigPy in general should not be expected to match
the performance of comparable C libraries such as BART (7). However, SigPy uses
Numba (12) to translate many of its’ most commonly used tools, such as gridding functions
to optimized machine code at runtime. Optimizations that are dominated by FFTs, such
as small-tip spatial domain pTx pulse designs, can closely match the performance of C
toolboxes since the same low-level C FFT libraries are used (6). Additionally, SigPy has
a general unified CPU and GPU interface for most functions, allowing for easy movement
of data between devices for computational flexibility and acceleration of computation with
GPUs. Figure 1 shows the mean computation times for a small-tip-angle spatial domain parallel transmit pulse design (11) on CPU and GPU. Design time was approximately an order of magnitude faster across all problem dimensions when the GPU was used versus CPU. 

![Mean execution time for small-tip spatial domain pulse design using SigPy
linear operators and CuPy matrices across all considered problem dimensions, on CPU and
GPU](https://github.com/jonbmartin/sigpyrf-joss/blob/main/execution_time_white.png?raw=true)

| Mean execution time for small-tip spatial domain pulse design using SigPy
linear operators and CuPy matrices across all considered problem dimensions, on CPU and
GPU |

# Target Audience

The SigPy.RF toolbox has been developed for the use of MRI researchers focusing on pulse sequence design, MRI physics, signal processing and optimization. We believe that it will serve as an essential building block of more general image acquisition tools which require specialized RF pulses. The toolbox has already been incorporated into open-source sequence development software such as Pulseq (5) and PyPulseq (13) to provide RF pulses critical to the performance of various pulse sequences. An example is shown in Figure 3. 

![pypulseq](https://github.com/jonbmartin/sigpyrf-joss/blob/main/pulseq-sigpy_cropped.png)

| First TR of a GRE pulse sequence created in Pulseq (cite) with SigPy-designed SLR excitation
pulse. Magnitude of the TB = 4 , 90 degree SLR pulse is plotted in the ‘RF mag’ plot, middle left. |

We also envision SigPy.RF may be utilized for replicability and
reproducibility studies, allowing for standardized conventions for RF pulses to be used across studies. The package could also serve as a hands-on teaching aid for MRI faculty and students. We have developed [several tutorials](https://github.com/jonbmartin/sigpy-rf-tutorials), which are accessible to a wide audience with minimal prior MRI knowledge. 

# Availability

The latest version of SigPy includes the most stable pulse
design tools to have been developed and is available from
https://github.com/mikgroup/sigpy. It can be installed through conda or pip- see documentation at https://sigpy.readthedocs.io/en/latest/ for more details.
The SigPy.RF fork including pulse design tools still in progress can be found
at https://github.com/jonbmartin/sigpy-rf. Jupyter notebook pulse design
tutorials for SigPy.RF are available at https://github.com/jonbmartin/sigpy-rf-tutorials.

# Acknowledgements

We gratefully acknowledge the support this study received from NIH Grant R01 EB 016695.

# References

1) P. AMREIN, F. JIA, M. ZAITSEV, AND S. LITTIN, CoilGen: Open-source MR coil
layout generator, Magnetic Resonance in Medicine, 88 (2022), pp. 1465–1479

2) S. M. ANAND, OCRA : a low-cost, open-source FPGA-based MRI console capable
of real-time control, (2018)

3) . FERNANDEZ VILLENA, A. G. POLIMERIDIS, L. L. WALD, E. ADALSTEINS-
SON, J. WHITE, AND L. DANIEL, MARIE a MATLAB-based open source software
for the fast electromagnetic analysis of MRI systems, in Proc. Intl. Soc. Mag. Reson.
Med., 2015, p. 0709

4) T. ST  ̈OCKER, K. VAHEDIPOUR, D. PFLUGFELDER, AND N. J. SHAH, High-
performance computing MRI simulations, Magnetic Resonance in Medicine, 64
(2010), pp. 186–193.

5) K. J. LAYTON, S. KROBOTH, F. JIA, S. LITTIN, H. YU, J. LEUPOLD,
J. F. NIELSEN, T. ST  ̈OCKER, AND M. ZAITSEV, Pulseq: A rapid and
hardware-independent pulse sequence prototyping framework, Magnetic resonance
in medicine, 77 (2017), pp. 1544–1552

6) L. M. ONG, F., SigPy: A Python Package for High Performance Iterative Recon-
struction, in Proceedings of the ISMRM Annual Meeting and Exhibition, 2019

7) M. UECKER, F. ONG, J. I. TAMIR, D. BAHRI, P. VIRTUE, J. Y. CHENG,
T. ZHANG, AND M. LUSTIG, Berkeley Advanced Reconstruction Toolbox, in Proc.
Intl. Soc. Mag. Reson. Med. 23, 2015, p. 2486

8) B. B. AVANTS, N. TUSTISON, AND H. JOHNSON, Advanced Normalization Tools
(ANTS) Release 2.x, (2014).

9) T. DUVAL, I. R. LEPPERT, J.-F. CABANA, M. BOUDREAU, I. GAGNON,
G. BERESTOVOY, J. COHEN-ADAD, AND N. STIKOV, Quantitative MRI made easy
with qMRLab, in Proc. Intl. Soc. Mag. Reson. Med., 2018, p. 2288.

10) K. P. PRUESSMANN, M. WEIGER, M. B. SCHEIDEGGER, AND P. BOESIGER,
SENSE: Sensitivity Encoding for Fast MRI.

11) W. GRISSOM, C.-Y. YIP, Z. ZHANG, V. A. STENGER, J. A. FESSLER, AND D. C.
NOLL, Spatial domain method for the design of RF pulses in multicoil parallel exci-
tation, Magnetic Resonance in Medicine, 56 (2006), pp. 620–629

12) S. K. LAM, A. PITROU, AND S. SEIBERT, Numba: A LLVM-based Python JIT
Compiler, Proceedings of the Second Workshop on the LLVM Compiler Infrastruc-
ture in HPC - LLVM ’15

13) K. S. Ravi, S. Geethanath, AND J.T. Vaughan Jr., PyPulseq: A Python Package for MRI Pulse Sequence Design, JOSS (2019) 
