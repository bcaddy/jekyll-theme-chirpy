---
title:      Classes and Bugfixing 5    # Title
author:     Robert Caddy               # Author Name
date:       2021-03-12 16:00:00 -0400  # Date
categories: [MHD, Coursework]          # Catagories, no more than 2
tags:       [Constrained Transport, Astronomical Techniques, Bugs]  # Tags, any number
pin:        false                      # Should this post be pinned?
toc:        true                       # Table of Contents?
math:       true                       # Does this post contain math?
# image:      /assets/img/...            # Header image path
---

# Astronomical Techniques
We had an in depth study of bootstrap methods and implemented a simple one.
Also, started discussing error types and propagation.


# Numerical MHD
Still working on debugging my MHD code. I, hopefully, finished debugging the
calculations for the CT fields. I found that my upwinded slope was not clear in
how it was implemented. It referened the two sides of the interface as "right"
and "left" which was confusing and led me to get the inputs switched around. Now
the sides are labeled as "negative" and "positive" to indicate which way along
the grid coordinates the sides are. This makes the code way clearer and removed
a bug. I also cleaned up the conservative update code. Now it's much clearer
what's going on

At this point I did what I probably should have started with and decided to test
a pure hydro problem with no magnetic field. In this limit the HLLD solver
exactly reduces to the HLLC solver and all the VL+CT integrator reduces to a
pure hydro Van Leer integrator. So I set up a Sod shock tube and started
comparing it's results to the pure hydro code I wrote earlier. After a lot of
searching I found that the momentum flux calculations were wrong and discovered
that I had forgotten to multiply by the density. e.g. I was trying to update the
momentum using the velocities in the state rather than the momenta in the state.
Fixing this made the code run perfectly for pure hydro problems; however it's
still not performing full MHD simulations properly.

The Sod shock tube running in the MHD simulation

<video muted autoplay controls>
    <source type="video/mp4" src="/assets/img/2021-post-assets/03-March/2021-03-12-SodShockTube.mp4">
</video>


The current results of the Brio & Wu shocktube: the first plot is just the first nine time steps and the second is the first ~550 time steps

<video muted autoplay controls>
    <source type="video/mp4" src="/assets/img/2021-post-assets/03-March/2021-03-12-BWShockTube-short.mp4">
</video>


<video muted autoplay controls>
    <source type="video/mp4" src="/assets/img/2021-post-assets/03-March/2021-03-12-BWShockTube-long.mp4">
</video>


As you can see there's still something wrong with how the fluxes are being
computed. I haven't found the bug yet but it seems like it should be in the HLLD
solver.