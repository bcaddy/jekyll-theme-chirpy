---
title:      Sign Errors and Debugging # Title
author:     Robert Caddy               # Author Name
date:       2021-05-14 14:45:00 -0400  # Date
categories: [MHD]     # Catagories, no more than 2
tags:       [Bugs, Constrained Transport]                     # Tags, any number
pin:        false                      # Should this post be pinned?
toc:        true                       # Table of Contents?
math:       true                      # Does this post contain math?
#image:      /assets/img/#            # Header image path
---

# MHD Debugging
My implementation of constrained transport was complicated and hard to maintain
so I refactored the main loop. Primarily I just unrolled the innermost loop over
the x,y,z directions. This eliminated a bunch of complicated math using modulos
to compute indices etc. Now it's much cleaner and easier to maintain. This
helped me find multiple errors in the VL+CT paper ([Stone & Gardiner
2009](https://www.sciencedirect.com/science/article/abs/pii/S1384107608000754?via%3Dihub))
that affected my results. I found them by comparing to the Athena codebase.

Now I'm trying to isolate and find the remaining issue. I found that in the case
where magnetic fields are constant forcing them to not update made the
simulation stable, though still with noisy \\( V_y, V_z \\) and pressure. This
seems to indicate that the issue is in the CT calculations.

Next I implemented a square wave initial condition for testing. You can see the
results of that below. It looks like there's still and issue that is presenting
in the y and z velocities but I haven't found it yet.

<video muted autoplay controls style="max-width:100%; height:auto">
    <source type="video/mp4" src="/assets/img/2021-post-assets/05-May/2021-05-14-square-wave.mp4">
</video>

# Other
- Applied to Rensselaer Summer Program -- Advanced Cyberinfrastructure Training
  for Modeling Physical Systems
- Applied to the LSST Data Science Fellowship
- Watched [AMD EPYC Advanced User Training on Expanse](https://education.sdsc.edu/training/interactive/202104_amd_epyc/index.html)
- Updated the codebase for this website