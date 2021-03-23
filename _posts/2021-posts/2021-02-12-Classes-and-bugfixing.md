---
title:      Classes and Bugfixing                 # Title
author:     Robert Caddy               # Author Name
date:       2021-02-12 16:00:00 -0400  # Date
categories: [MHD, Coursework]     # Catagories, no more than 2
tags:       [Constrained Transport, Astronomical Techniques, Bugs]                     # Tags, any number
pin:        false                      # Should this post be pinned?
toc:        true                       # Table of Contents?
math:       true                      # Does this post contain math?
# image:      /assets/img/...            # Header image path
---

# Astronomical Techniques
We've been digging into the details of various common distributions and we had
our second homework. The homework was our first really hands on time with
Bayesian statistics so it took several days to complete but I think I understand
what's going on now.


# Numerical MHD
After last week's plethora of bugfixes I've got a much shorter list this week. I
resolved the issue with the magnetic fields not updating, turns out I forgot to
call the function that computed the CT electric fields and so the arrays were
always zero. Fixing that up got the magnetic fields updating as expected. I've
started digging into the potential bugs in the HLLD solver but haven't made a
ton of progress; the wavespeeds are correct and that's all I know so far.

Here's my current results from running the Brio and Wu shock tube. You can see
the immediate issue in what appears to be the $R^{**}$ state.
<video muted autoplay controls style="max-width:100%; height:auto">
    <source type="video/mp4" src="/assets/img/2021-post-assets/02-February/2021-02-12-BWShockTube.mp4">
</video>


# Other
- We have an annual panel session for first and second year graduate students to
  help them find and navigate the world of research. I was on the panel this
  year

## Useful links
Blog post on the VL+CT Algorithm: [VL+CT Algorithm]({% post_url 2021-posts/2021-01-06-VL+CT-Algorithm%})

Original VL+CT paper: [Stone & Gardiner 2009](https://www.sciencedirect.com/science/article/abs/pii/S1384107608000754?via%3Dihub)
