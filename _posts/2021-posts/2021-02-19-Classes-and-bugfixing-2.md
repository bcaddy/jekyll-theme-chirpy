---
title:      Classes and Bugfixing 2    # Title
author:     Robert Caddy               # Author Name
date:       2021-02-19 16:30:00 -0400  # Date
categories: [MHD, Coursework]          # Catagories, no more than 2
tags:       [Constrained Transport, Astronomical Techniques, Bugs]  # Tags, any number
pin:        false                      # Should this post be pinned?
toc:        true                       # Table of Contents?
math:       false                      # Does this post contain math?
# image:      /assets/img/...            # Header image path
---

# Astronomical Techniques
Started discussing statistics (the things not the subject) and I fixed some
issues with the homework from last week. Overall uneventful


# Numerical MHD
More bug hunting this week. I've hammered out most of the HLLD solver, all
that's left is 50-100 lines in the double star state computations. The
wavespeeds, L/R, and L/R star states are all debugged along with most of the
utility functions. Throughout this I've only found 1 bug, a simple typo in which
variable to use. Out of a couple hundred lines of code I think that's pretty
good.

I've also been experimenting with some new debugging methods. Mostly that of
using a jupyter notebook to help with comparing calculations. It actually works
really well and is something I'll have to continue doing. I've also been using
the debugger to edit values in memory to help with debugging. Specifically
editing the position over timer variable so that I can force the HLLD solver
into any state I want and don't have to fiddle with the initial conditions to
make it happen. This has significantly sped up debugging and it something I will
have to remember when I'm debugging
[Cholla's](https://github.com/cholla-hydro/cholla) future HLLD solver.


# Other
- Watched [Peng Oh's
  keynote](https://online.kitp.ucsb.edu/online/halo21/oh/rm/jwvideo.html) from
  KITP on the role of non-thermal physics in CGM evolution
- Attended the San Diego Super Computer Center's webinar on optimizing your
  code. It was really good, if short, and you should be able to find it
  [here](https://www.youtube.com/c/SanDiegoSupercomputerCenter/videos) when they
  post it.
- Started reading *Essential magnetohydrodynamics for astrophysics* by H.
  Spruit. It's a review article on astrophysical MHD that is regularly updated.
  I'm reading version 3.5.1 from 2017 as that is the most recent version at the
  time of writing