---
title:      Astro Project and Debugging                 # Title
author:     Robert Caddy               # Author Name
date:       2021-04-16 17:00:00 -0400  # Date
categories: [Coursework, MHD]     # Catagories, no more than 2
tags:       [Astronomical Techniques, TSNE, Bugs] # Tags, any number
pin:        false                      # Should this post be pinned?
toc:        true                       # Table of Contents?
math:       false                      # Does this post contain math?
#image:      /assets/img/#            # Header image path
---

# Astronomical Techniques
This week in class we discussed non-parametric tests like the U test.

I was working a lot on the project this week. I spent about a day reading up on
MCMC techniques and time implementing t-SNE on the data to see if there are
clusters that I might be interested in fitting.

# MHD Debugging
I found a tricky bug in the integrator. The first step in the integrator was
computing the time step, this requires the centered magnetic field and since I
use that later in the algorithm I only wanted to compute it once and so saved it
to a member variable. However since I was doing that before I computed the
boundary conditions I was getting non-physical results on the first time step
since the ghost cells hadn't been set yet. Simply calling the boundary
conditions function before the time step function resolved that issue. Now the
single waves run stably, for a little bit at least. The issue looks like an
error in the derivatives somewhere and I'm leaning towards the CT fields since I
get similar results when forcing the Riemann solver into the L or R state.

<video muted autoplay controls style="max-width:100%; height:auto">
    <source type="video/mp4" src="/assets/img/2021-post-assets/04-April/2021-04-16-mhd-contact.mp4">
</video>
*The MHD contact wave*
