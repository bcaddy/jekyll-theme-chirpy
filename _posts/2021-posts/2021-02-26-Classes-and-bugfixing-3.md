---
title:      Classes and Bugfixing 3                 # Title
author:     Robert Caddy               # Author Name
date:       2021-02-26 16:00:00 -0400  # Date
categories: [MHD, Coursework]     # Catagories, no more than 2
tags:       [Constrained Transport, Astronomical Techniques, Bugs]                     # Tags, any number
pin:        false                      # Should this post be pinned?
toc:        true                       # Table of Contents?
math:       true                      # Does this post contain math?
# image:      /assets/img/...            # Header image path
---

# Astronomical Techniques
This week has mostly been a discussion of various statistics for finding
distribution centers and spreads.


# Numerical MHD
Finished debugging the HLLD solver. I didn't find any bugs to explain the
current issues so I moved on to the
[VL+CT](https://www.sciencedirect.com/science/article/abs/pii/S1384107608000754?via%3Dihub)
integrator and found a sign error in the conservative update for the magnetic
field in the \\( y \\)  direction. That changed the output but there are still
significant errors that are likely in the velocity/momentum calculations and
maybe in the boundary conditions. The current results of the Brio & Wu shocktube
are below,

<video muted autoplay controls>
    <source type="video/mp4" src="/assets/img/2021-post-assets/02-February/2021-02-26-BWShockTube.mp4">
</video>

# Other
- Presented on [*Revealing the physical properties of gas accreting to haloes in
  the EAGLE simulations*](https://arxiv.org/abs/2102.10913) by Wight et al. in
  astrocoffee.
