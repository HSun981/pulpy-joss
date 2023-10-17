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
  - name: William A. Grissom
    corresponding: true # (This is how to denote the corresponding author)
    affiliation: 2
affiliations:
 - name: Vanderbilt University
   index: 1
 - name: Case Western Reserve University
   index: 2
date: 26 Mar 2023
bibliography: paper.bib

# Optional fields if submitting to a AAS journal too, see this blog post:
# https://blog.joss.theoj.org/2018/12/a-new-collaboration-with-aas-publishing
aas-doi: 10.3847/xxxxx <- update this with the DOI from AAS once you know it.
aas-journal: Astrophysical Journal <- The name of the AAS journal.
---

# Summary

While RF pulse developers increasingly share code online in independent repositories, no unified set of common pulse design tools is
maintained in a rigorous and consistent manner with easy-to-read code and tutorials. An open source pulse design code library would facilitate the
development and dissemination of novel techniques and the comparison of approaches, similar to how BART 1 , SigPy 2 , and MIRT 3 have made advanced
parallel imaging and compressed sensing reconstruction methods widely accessible. To meet this need, we have developed a library of pulse design
tools as part of the SigPy Python package 2 , which we call SigPy.RF. Here we detail the software’s organization and goals, provide examples,

# Statement of need
The field of magnetic resonance imaging is currently experiencing rapid growth in the num-
ber of available open source imaging tools. Tools have been made freely available for MRI hardware development (3; 4), system simulation (32; 123), pulse sequence pro-
gramming (70), image reconstruction (92; 128), and post-processing and analysis (5; 31;
115; 116)
However, one critical step of the imaging pipeline which has seen limited open-source tool de-
velopment is RF pulse design.  While RF pulse developers increasingly share code online
in independent repositories, no unified set of common pulse design tools has been main-
tained in a rigorous and consistent manner with easy-to-read code and tutorials. This is despite the reality that in many cases, carefully designed or application-specific RF pulses are crucial to the success of MRI techniques. An open
source pulse design code library would facilitate the development and dissemination of
novel techniques and the comparison of approaches, similar to how BART (128), SigPy
(92), and MIRT (33) have made advanced parallel imaging and recon-
struction methods widely accessible. To meet this need, we have developed a library of
pulse design tools as part of the SigPy Python package for signal processing and image re-
construction (92). We call this new package SigPy.RF. SigPy.RF is constructed as a nested
package within the broader SigPy package. Here we detail the software’s organization and
goals, provide examples, and 

# About the SigPy Package

# Target Audience

The SigPy.RF toolbox has been developed for the use of MRI researchers focusing on pulse sequence design, MRI physics, signal processing and optimization. We believe that it will serve as an essential building block of more general image acquisition tools which require specialized RF pulses. The toolbox has already been incorporated into open-source sequence development software such as Pulseq [rMarkdown](http://rmarkdown.rstudio.com/authoring_bibliographies_and_citations.html) and PyPulseq [rMarkdown](http://rmarkdown.rstudio.com/authoring_bibliographies_and_citations.html) to provide RF pulses critical to the performance of various pulse sequences. We also envision SigPy.RF may be utilized for replicability and
reproducibility studies, allowing for standardized conventions for RF pulses to be used across studies. The package could also serve as a hands-on teaching aid for MRI faculty and students. We have developed [several tutorials](https://github.com/jonbmartin/sigpy-rf-tutorials), which are accessible to a wide audience with minimal prior MRI knowledge. 

# Citations

Citations to entries in paper.bib should be in
[rMarkdown](http://rmarkdown.rstudio.com/authoring_bibliographies_and_citations.html)
format.

If you want to cite a software repository URL (e.g. something on GitHub without a preferred
citation) then you can do it with the example BibTeX entry below for @fidgit.

For a quick reference, the following citation commands can be used:
- `@author:2001`  ->  "Author et al. (2001)"
- `[@author:2001]` -> "(Author et al., 2001)"
- `[@author1:2001; @author2:2001]` -> "(Author1 et al., 2001; Author2 et al., 2002)"

# Figures

Figures can be included like this:
![Caption for example figure.\label{fig:example}](figure.png)
and referenced from text using \autoref{fig:example}.

Figure sizes can be customized by adding an optional second parameter:
![Caption for example figure.](figure.png){ width=20% }

# Acknowledgements

We acknowledge contributions from Brigitta Sipocz, Syrtis Major, and Semyeong
Oh, and support from Kathryn Johnston during the genesis of this project.

# References
