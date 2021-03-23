---
title:      MHD Simulation Bugfixing                 # Title
author:     Robert Caddy               # Author Name
date:       2021-02-05 10:50:58 -0400  # Date
categories: [MHD, Coursework]     # Catagories, no more than 2
tags:       [Constrained Transport, Astronomical Techniques, Bugs]                     # Tags, any number
pin:        false                      # Should this post be pinned?
toc:        true                       # Table of Contents?
math:       true                      # Does this post contain math?
# image:      /assets/img/...            # Header image path
---

# Astronomical Techniques
We've been digging into the Bayesian vs Frequentist interpretation of statistics
and how to apply Bayes' Theorem. We had our first homework and it went well, it
was basic Monte Carlo simulation of coin flips.


# Numerical MHD
Last week I finished writing new code for the MHD solve so this week I've been working on bugfixes. First the linter showed some errors that I resolved, mostly changes to function calls in member functions I hadn't changed for MHD. After that I fixed a few compile time warnings, mostly out of order initialization warnings. At that point the code compiled but would segfault immediately. That issue turned out to be an off-by-one error in the time step computations. After fixing that the code ran!

Now that the code runs it's time to hunt down runtime bugs:
- Indexing in conservative update was incorrect
- Boundary values weren't being set properly, I had to add new code to fix this.
- Convert an int to a double
- Figure out the optimum number of cells to run while debugging (50). Once the code is stable I'll bump this up to 100-1000 for final testing

Currently known unfixed bugs:
- It looks like there's some kind of issue in the $$R^{**}$$ state of the HLLD solver. In this state there are wild oscillations
- The magnetic fields are not updating



# Other
- Read *"On the Survival of cool clouds in the circumgalactic medium"* by Li et al. 2020
- Tried to get VS Code's debugger intergration to connect to Pitt CRC compute nodes. TL;DR is that it's doable but more effort than it's worth at the moment. Checkout [this GitHub issue](https://github.com/microsoft/vscode-remote-release/issues/1722#issuecomment-667733248) for more info.

## Useful links
Blog post on the VL+CT Algorithm: [VL+CT Algorithm]({% post_url 2021-posts/2021-01-06-VL+CT-Algorithm%})

Original VL+CT paper: [Stone & Gardiner 2009](https://www.sciencedirect.com/science/article/abs/pii/S1384107608000754?via%3Dihub)
