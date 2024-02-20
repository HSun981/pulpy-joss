---
title: 'PulPy: A Python Toolkit for MRI RF and Gradient Pulse Design'
tags:
  - Python
  - magnetic resonance imaging (MRI)
  - signal processing
  - RF pulse
  - gradient
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
  - name: Heng Sun
    equal-contrib: false
    affiliation: 2
  - name: Madison Albert
    equal-contrib: false
    affiliation: 3
  - name: Jonathan I. Tamir
    equal-contrib: false
    affiliation: 4
  - name: Frank Ong
    equal-contrib: false
    affiliation: 5
  - name: William A. Grissom
    corresponding: true # (This is how to denote the corresponding author)
    affiliation: 6
affiliations:
 - name: Vanderbilt University Institute of Imaging Science, Vanderbilt University Medical Center, Nashville, TN, USA
   index: 1
 - name: Department of Biomedical Engineering, Yale University, New Haven, CT, USA
   index: 2
 - name: Department of Biomedical Engineering, Vanderbilt University, Nashville, TN, USA
   index: 3
 - name: Department of Electical and Computer Engineering, University of Texas at Austin, Austin, TX, USA
   index: 4
 - name: Roblox Corporation, San Mateo, CA, USA
   index: 5
 - name: Department of Biomedical Engineering, Case Western Reserve University, Cleveland, OH, USA
   index: 6
date: 26 Mar 2023
bibliography: paper.bib

---

# Summary
We present PulPy (Pulses in Python), an extensive set of open-source, Python-based tools for magnetic resonance imaging (MRI) radiofrequency (RF) and gradient pulse design. This toolbox builds on the RF pools introduced previously in the SigPy.RF Python software package and leverages SigPy’s existing capabilities for GPU computation, iterative optimization, and powerful abstractions for linear operators and applications. Tools are available for all steps of the pulse design process including trajectory/gradient design, pulse design, and simulation. Our implemented functions for pulse design include advanced Shinnar-LeRoux (SLR), multiband, adiabatic, optimal control, B$_1^+$-selective and small-tip parallel transmission (pTx) designers. 

| Module               | Description                                                      |
|:--------------------:|:----------------------------------------------------------------:|
| .rf.adiabatic.py     | Adiabatic/frequency-swept RF pulses (e.g. [@Garwood2001])        |
| .rf.b1sel.py         | B1-selective pulses (e.g. [@Martin2022])                         |
| .rf.multiband.py     | Pulses for simultaneous multi-slice (e.g. [@Norris2011])         |
| .rf.optcont.py       | Large tip angle optimal control design (e.g. [@Connolly1986])    |
| .rf.ptx.py           | parallel transmit pulse designers (e.g. [@Grissom2006])          |
| .rf.shim.py          | parallel transmit RF shimming (e.g. [@                           |
| .rf.slr.py           | Conventional SLR and variations (e.g. [@Pauly1991])              |
| .rf.util.py          | RF pulse design utilities                                        |
| .grad.waveform.py    | Gradient and trajectory designers (e.g. [@Kim2003])              |
| .grad.optim.py       | Gradient and trajectory optimization (e.g. [@Lustig2008])        |
| io.py                | Vendor-specific scanner input/output                             |
| linop.py             | Linear operators for pulse design                                |
| sim.py               | 1-D, N-D, and N-coil Bloch simulation                            |
| verse.py             | RF pulse/gradient reshaping tools                                |



# Statement of need
The field of magnetic resonance imaging is currently experiencing rapid growth in open source imaging tools. Tools have been made freely available for MRI hardware development [@Amrein2022; @Anand2018], system simulation [@Villena2014; @Stocker2010], pulse sequence programming [@Layton2017], image reconstruction [@Ong2019; @Uecker2015], and post-processing and analysis [@Avants2014; @Duval2018]. The great increase in open-source tools 
However, one critical step of the imaging pipeline which has seen limited open-source tool development is RF pulse design.  While RF pulse developers increasingly share code online
in independent repositories, no unified set of common pulse design tools has been maintained in a rigorous and consistent manner with easy-to-read code and tutorials. This is despite the reality that in many cases, carefully designed or application-specific RF pulses are crucial to the success of MRI techniques. An open
source pulse design code library would facilitate the development and dissemination of
novel techniques and the comparison of approaches, similar to how BART [@Uecker2015] and SigPy
[@Ong2019] have made advanced parallel imaging and reconstruction methods widely accessible. To meet this need, we have developed a library of
pulse design tools as part of the SigPy Python package for signal processing and image reconstruction [@Ong2019]. We call this new package PulPy, short for Pulses in Python. SigPy.RF is constructed as a nested
package within the broader SigPy package.\autoref{fig:hierarchy} illustrates where the RF pulse design tools fit into the broader SigPy software, as a nested package within the package for general MRI tools.

![New RF pulse design tools (red) within the SigPy package hierarchy. The pulse
 design tools are grouped in a series of modules dedicated to a specific pulse class or type
of RF pulse design utility. \label{fig:hierarchy}](sigpy-hierarch.png){ width=60% }


# About PulPy
SigPy has a number of features that make it an ideal candidate to support a nested RF
pulse design package. Image reconstruction and RF pulse design require many of the same
operators and computational tools; for example, SENSE reconstruction [@Pruessmann] and small-tip
parallel transmit pulse design [@Grissom2006] are in many cases parallel processes moving in opposite directions between the spatial and frequency domains, with the same or similar linear
operators and the solved with the same iterative optimization tools. Thus critical RF pulse design operations
such as Fourier transforms, gradient-based iterative optimizers, and matrix manipulation
methods are provided in SigPy [@Ong2019].
SigPy was further built with a number of computational features that make it an ideal fit
for RF pulse design. As a Python library, SigPy in general should not be expected to match
the performance of comparable C libraries such as BART [@Uecker2015]. However, SigPy uses
Numba [@Lam2015] to translate many of its most commonly used tools, such as gridding functions,
to optimized machine code at runtime. Optimizations that are dominated by FFTs, such
as small-tip spatial domain pTx pulse designs, can closely match the performance of C
toolboxes as the same low-level C FFT libraries are used [@Ong2019]. Additionally, SigPy has
a general unified CPU and GPU interface for most functions, allowing for easy movement
of data between devices for computational flexibility and acceleration of computation with
GPUs. \autoref{fig:execution} shows the mean computation times for a small-tip-angle spatial domain parallel transmit pulse design [@Grissom2006] on CPU and GPU. Design time was approximately an order of magnitude faster across all problem dimensions when the GPU was used versus CPU.

![Mean execution time for small-tip spatial domain pulse design using SigPy
linear operators and CuPy matrices across all considered problem dimensions, on CPU and
GPU \label{fig:execution}](execution_time.png){ width=60% }

# Target Audience

The PulPy toolbox has been developed for use by MRI researchers focusing on pulse sequence design, MRI physics, signal processing, and optimization. We believe that it will serve as an essential building block for more general image acquisition tools which require specialized RF pulses. The toolbox has already been incorporated into open-source sequence development software such as Pulseq [@Layton2017] and PyPulseq [@SravanRavi2019] to provide RF pulses critical to the performance of various pulse sequences. An example is shown in \autoref{fig:pulseq}. Finally, end-to-end optimization of MRI pulse sequences and reconstructions is 
being increasingly explored [@Radhakrishna2023; @Wang2022]; with the RF pulse and gradient waveform design functions
provided, the PulPy package could facilitate this research. 

![ First TR of a GRE pulse sequence created in Pulseq [@Layton2017] with SigPy.RF-designed SLR excitation
pulse. Magnitude of the TB = 4 , 90 degree SLR pulse is plotted in the ‘RF mag’ plot, middle left. \label{fig:pulseq}](pulseq-sigpy_cropped.png){ width=80% }


We also envision PulPy may be utilized for replicability and
reproducibility studies, allowing for standardized conventions for RF pulses to be used across studies. The package could also serve as a hands-on teaching aid for researchers and students. We have developed [several tutorials](https://github.com/jonbmartin/pulpy-tutorials), which are accessible to a wide audience with minimal prior MRI knowledge. 

# Availability and Use

The latest version of PulPy includes the latest stable release of the pulse
design tools and is available from 
[the main repository](https://github.com/jonbmartin/pulpy). It can be installed through pip- see the [documentation](https://pulpy.readthedocs.io/en/latest/) for more details. Jupyter notebook [pulse design
tutorials](https://github.com/jonbmartin/pulpy-tutorials) for PulPy are also available, which demonstrate several classes of pulse design.

Preliminary development of this toolbox was presented in references [@Martin2020a]; the current version has been streamlined and expanded to include a larger collection of RF pulse design methods from the literature, as well as additional utility tools for I/O, gradient waveform design, and experimental B$_1^+$-selective pulse design algorithms which were prototyped using the initial SigPy.RF codebase, enabling the publication of Reference [@Martin2022].

# Acknowledgements

We gratefully acknowledge the support this study received from NIH grants R01 EB 016695 and T32 EB 001628.

# References
