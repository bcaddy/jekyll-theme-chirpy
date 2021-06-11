---
title:      Learning Cuda and GPU Resident MPI                # Title
author:     Robert Caddy               # Author Name
date:       2021-06-11 16:00:00 -0400  # Date
categories: [CUDA]     # Catagories, no more than 2
tags:       [GPU MPI]                     # Tags, any number
pin:        false                      # Should this post be pinned?
toc:        true                       # Table of Contents?
math:       false                      # Does this post contain math?
#image:      /assets/img/#            # Header image path
---

# GPU MPI
I started on getting GPU resident MPI working on the Pitt CRC cluster. So far it
compiles but doesn't run. I'm working with the CRC and other group members to
get that fixed. I got access to Summit and other OLCF systems this week so after
getting setup there and familarizing myself with the systems I worked on getting
MPI GPU to work there. After several hurdles and with help I got it working with
both the IBM XL and GCC compilers then sent that test info to the Pitt CRC
personell.

# Learning CUDA
I started on the
[OLCF CUDA Training Series](https://www.olcf.ornl.gov/cuda-training-series/)
and so far I've completed the first three parts.

## Near future plans
- Get Cholla to run with MPI GPU on H2P
- Figure out how I want to do future tests in Cholla
  - Check if Cholla's regression test is working and port it to whatever the new
    testing system is
- Learn how Cholla works
- Learn CUDA
