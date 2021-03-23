---
title:      Classes and Bugfixing 4    # Title
author:     Robert Caddy               # Author Name
date:       2021-03-05 16:00:00 -0400  # Date
categories: [MHD, Coursework]          # Catagories, no more than 2
tags:       [Constrained Transport, Astronomical Techniques, Bugs]  # Tags, any number
pin:        false                      # Should this post be pinned?
toc:        true                       # Table of Contents?
math:       true                       # Does this post contain math?
# image:      /assets/img/...            # Header image path
---

# Astronomical Techniques
Homework 3 was due this week, we were studying the spread and biases of various
location and dispersion estimators on Gaussian data and data with outliers. We
also started talking about jacknife and bootstrap methods.


# Numerical MHD
Still working on debugging the
[VL+CT](https://www.sciencedirect.com/science/article/abs/pii/S1384107608000754?via%3Dihub)
integrator. Here's what I accomplished this week

- I found a couple of style errors, now whenever I have a pointer or the half
  time or standard grid it's always named `workingGrid` instead of a mix of
  `workingGrid` and `activeGrid`
- I fixed the bug that was occuring at the right boundary. It turns out that
  since the magnetic fields are stored on cell faces you need 1 extra array
  element to store the face on the rightmost (or leftmost) side so that all the
  cells have magnetic fields on both faces.
- The PLM function was treating all the magnetic fields the same and
  reconstructing all of them when it should have been getting the \\( x
  \\)-direction field directly from the grid.
- Checked the entire integrator except the CT field calculations for bugs.
- Checked the 3D virtual grid I use for calculating the CT fields


The current results of the Brio & Wu shocktube are below,

<video muted autoplay controls>
    <source type="video/mp4" src="/assets/img/2021-post-assets/03-March/2021-03-05-BWShockTube.mp4">
</video>
