---
title:      Cholla Prep                # Title
author:     Robert Caddy               # Author Name
date:       2021-05-28 16:00:00 -0400  # Date
categories: [MHD, Cholla]              # Catagories, no more than 2
tags:       [Constrained Transport, Testing]  # Tags, any number
pin:        false                      # Should this post be pinned?
toc:        true                       # Table of Contents?
math:       false                      # Does this post contain math?
#image:      /assets/img/#             # Header image path
---

# MHD Debugging
I spent some more time bug hunting and found that the instability only shows up
when a number is not exactly expressible in binary. After discussin this with
Evan they pointed out that it could just be an artifact of turning a 3D
algorithm into a 1D algorithm. So instead of continueing bug hunting I'm going
to switch to implementing MHD into Cholla.

# Miscellaneous Cholla Prep
Switching to working on Cholla requires quite a bit of prep so here's what I did
this week:
- Cleaned up a few notes on the
  [Hydro-Sandbox](https://github.com/bcaddy/hydro-sandbox) code so that it can
  be put away for awhile
- Submit account request for the ORNL supercomputer system
- Update my fork of [Cholla](https://github.com/cholla-hydro/cholla)
- Figure out how to access POWER9/V100 nodes at Pitt's H2P cluster
- Update the modules I'm using on H2P and the POWER9 nodes
- Get [Cholla](https://github.com/cholla-hydro/cholla) to compile on H2P
- Get Cholla running on H2P in serial and parallel
- Meet with Damon to discuss testing schemes


## Near future plans
- Check if I can get cholla to run with GPU-MPI on H2P
- Check if Cholla's regression test is working and fix it if it isn't
- Figure out how I want to do future tests in Cholla
- Learn CUDA
- Learn how Cholla works

# Miscellaneous
- Research pair programming
- Present on 3.3-3.5 of [Naab & Ostriker 2017](https://www.annualreviews.org/doi/abs/10.1146/annurev-astro-081913-040019?casa_token=0oOLHbp8TxgAAAAA%3ApFgE9ZUGDoAIMhoZGkVFuvY1W-scLKqkD3Cf_PodERTA79D-jr8kPizLlyn9fUo1ch-GB7ZOaqx6)
