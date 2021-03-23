---
title:      CT and Classes             # Title
author:     Robert Caddy               # Author Name
date:       2021-01-29 17:00:00 -0400  # Date
categories: [MHD, Coursework]     # Catagories, no more than 2
tags:       [Constrained Transport, Astronomical Techniques, Dotfiles]  # Tags, any number
pin:        false                      # Should this post be pinned?
toc:        true                       # Table of Contents?
math:       false                      # Does this post contain math?
# image:      /assets/img/...            # Header image path
---

# Astronomical Techniques
A little more work in python and we started on the difference between the
frequentist and Byesian interpretation of statistics.

# Dotfiles
I fixed a race condition I accidentally added to my
[dotfiles](https://github.com/bcaddy/dotfiles) repo a few weeks ago. I had set
them to start checking if the dotfiles repo was up to date first and running
everything else in parallel with that but since my script for checking requires
a newer version of Git that is not standard on some systems it would raise
errors if run before the required version of git was added to the path. I
reverted this change and will try to think of a better way of improving launch
times.

# Constrained Transport (CT)
I've finished writing the bulk of the new code for adding MHD to my simulation
code. This week I modified the following sections of code for MHD:

- Computing the edge averaged electric fields
- Slopes used in computing the CT electric fields
- Piecewise Constant Method
- Piecewise Linear Method
- The function that calls the Riemann solver
- Conservative update

There are still some bits to clean up, mostly just changing arguments to older
functions to account for the addition of MHD then I will be moving on to testing
and debugging.

# Other
- I attended the Pitt Center for Research Computing "Intro to the Cluster Workshop"
- Fixed some issues I was having with VS Code's default highlighting color being too difficult to see

## Useful links
Blog post on the VL+CT Algorithm: [VL+CT Algorithm]({% post_url 2021-posts/2021-01-06-VL+CT-Algorithm%})

Original VL+CT paper: [Stone & Gardiner 2009](https://www.sciencedirect.com/science/article/abs/pii/S1384107608000754?via%3Dihub)
