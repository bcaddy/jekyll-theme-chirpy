---
title:      Numerical Noise and Confusion # Title
author:     Robert Caddy               # Author Name
date:       2021-05-07 16:58:18 -0400  # Date
categories: [MHD]     # Catagories, no more than 2
tags:       [Bugs]                     # Tags, any number
pin:        false                      # Should this post be pinned?
toc:        true                       # Table of Contents?
math:       true                      # Does this post contain math?
#image:      /assets/img/#            # Header image path
---

# MHD Debugging

I spent this week comparing my code to Athena++. Throughout that process I fixed
a sign error in the CT equation, turns out they should all be plusses with no
minuses. I also fixed an error in the single wave initial conditions, made my
floating point arithamatic match Athena's for associativity symmetry, and a few
other minor tweaks. Except the sign error nothing has really changed the
fundamental issue, there appears to be an error occuring at the level of
\\(10^{-16} \\), i.e. the last floating point digit, that is slowly growing until
it destabilizes the entire simulation.  I've been hunting down the issue and it
seems to be related to an instability in the z magnetic field but I can't find a
cause. It happens faster in the left moving contact wave than the right but I
haven't isolated the reason yet.

<video muted autoplay controls style="max-width:100%; height:auto">
    <source type="video/mp4" src="/assets/img/2021-post-assets/05-May/2021-05-07-lowRes.mp4">
</video>
*A low res simulation. You can see the "noise" develope immediately*

<video muted autoplay controls style="max-width:100%; height:auto">
    <source type="video/mp4" src="/assets/img/2021-post-assets/05-May/2021-05-07-highRes-long.mp4">
</video>
*A higher res simulation. The "noise" takes awhile to build up but once it does
the entire simulation destabilizes*