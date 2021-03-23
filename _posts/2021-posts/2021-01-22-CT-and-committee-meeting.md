---
title:      CT and First Committee Meeting  # Title
author:     Robert Caddy                    # Author Name
date:       2021-01-22 17:00:00 -0400  # Date
categories: [MHD, Course Work]         # Catagories, no more than 2
tags:       [Constrained Transport, Astronomical Techniques]  # Tags, any number
pin:        false                      # Should this post be pinned?
toc:        true                       # Table of Contents?
math:       false                      # Does this post contain math?
# image:      /assets/img/...          # Header image path
---


# Astronomical Techniques
I'm starting a new class this spring; Astronimcal Techniques taught by Prof.
Newman. This week was an introduction to python.

# Committee Meeting
I had my first committee meeting this Wednesday so I spent the bulk of last week
and first three days of this week prepping for it. It went well and was very
helpful in providing future direction.

# Constrained Transport (CT)
After a hiatus from CT to prepare for my committee meeting I got back into the thick of it Thursday. The biggest challenges are:

1. I'm implementing a 3D algorithm in a 1D simulation. I've gotten around this by basically making ghost cells in 3D so my implementation should translate more directly into [Cholla](https://github.com/cholla-hydro/cholla)
2. Indexing is a nightmare. Lots of non-integer indexing is requiring 5D arrays
   and figuring out the indexes is challenging. A big part of the issue is that
   to compute the electric field in one direction I need info from the other two
   directions. I've been getting around this with the modulo operator and some
   clever indexing but it's still a headache.

I'm about a third of the way through the computations of the electric field and
I have a firm plan for the last two thirds. Hopefully by my next update I will
have it done.

## Useful links
Blog post on the VL+CT Algorithm: [VL+CT Algorithm]({% post_url 2021-posts/2021-01-06-VL+CT-Algorithm%})

Original VL+CT paper: [Stone & Gardiner 2009](https://www.sciencedirect.com/science/article/abs/pii/S1384107608000754?via%3Dihub)
