---
title:      VL+CT Algorithm            # Title
author:     Robert Caddy               # Author Name
date:       2021-01-06 15:44:00 -0400   # Date
categories: [MHD, Recontruction]       # Catagories, no more than 2
tags:       [MHD, Hydro, Computational Methods, Numerical Methods, Riemann Solver, Van Leer, VL+CT]
pin:        false                      # Should this post be pinned?
toc:        true                       # Table of Contents?
math:       true                       # Does this post contain math?
# image:      /assets/img/...           # Header image path
---

# Overview

This is a step through of the VL+CT (Van Leer plus Constrained Transport)
integrator I'm going to add to my
[hydro-sandbox](https://github.com/bcaddy/hydro-sandbox) code. The specific
algorithm is from [Stone & Gardiner
2009](https://ui.adsabs.harvard.edu/abs/2009NewA...14..139S/abstract) which is
the original paper where the VL+CT integrator was introduced. This will serve as
practice and figuring out any kinks before I add the VL+CT integrator to
[Cholla](https://github.com/cholla-hydro/cholla).

Overall the algorithm is very similar to the Van Leer algorithm, though with
significant new additions for Constrained Transport(CT). CT treats the magnetic
field as surface, rather than volume, averaged quantities then updates using
edge averaged electric fields. By updating with electric fields you
automatically fulfill the divergence free condition for magnetic fields so no
magnetic monopoles will appear (assuming that the initial conditions don't
contain any).

## Glossary of Symbols
- \\( \vec{U} \\), Vector of conserved variables. Note that the magnetic field
  is the face averaged value at the \\( i-1/2 \\) interface whereas all the
  other values are volume averages
- \\( \vec{W} \\), Vector of primitive variables.
- \\( \vec{F} \\), The flux vector
- \\( x, y, \text{or} z \\), Subscript to indicate which vector element is in use
- \\( i, j, k \\), Subscript indices to indicate which cell the value is in
- \\( n \\), Superscript index to indicate the time step
- \\( \delta t \\), the time step
- \\( \delta x, \delta y, \delta z \\), the size of each cell in the \\(x)\\,
  \\(y)\\, or \\(z)\\ direction
- \\(\rho  \\), Density
- \\(p  \\), Pressure
- \\(E \\), Energy
- \\(v  \\), Velocity
- \\(B \\), Magnetic Field
- \\(p_T \\), Total pressure
- \\( \mathcal{E} \\), the electric field
- \\( C_{CFL} \\), the CFL number, must be less than 1/2




## Glossary of New Equations
MHD comes along with many modifications to the various basic equations that
we're used to; new terms in the pressure and energy equations, new wave speeds,
new eigenvalues etc. The glossary can be found in my post on the [HLLD
Algorithm]({% post_url 2020-posts/2020-10-23-HLLD-Algorithm%})



# Algorithm
Our goal is to choose the correct HLLD flux depending on the interface states.

## 1. Compute the Time Step
The first thing we need to do is compute the time step which is done with this
equation

$$
    \delta t = C_{CFL} \min \left(
        \frac{\delta x}{\mid v^n_{x,i,j,k} \mid + C^n_{f,i,j,k}},
        \frac{\delta y}{\mid v^n_{y,i,j,k} \mid + C^n_{f,i,j,k}},
        \frac{\delta z}{\mid v^n_{z,i,j,k} \mid + C^n_{f,i,j,k}}
    \right).
$$

Where \\( C^n_f \\) is the fast magnetosonic wave-speed computed using *cell
centered* values. Computing the cell centered magnetic field is done with a
direct average of the face centered values. This cell centered magnetic field
will be needed in the next two steps so make sure to keep it.

$$
    \begin{aligned}
        B^n_{x,i,j,k,} = \frac{1}{2} \left( B^n_{x,i+1/2,j,k} + B^n_{x,i-1/2,j,k} \right) \\
        B^n_{y,i,j,k,} = \frac{1}{2} \left( B^n_{y,i,j+1/2,k} + B^n_{y,i,j-1/2,k} \right) \\
        B^n_{z,i,j,k,} = \frac{1}{2} \left( B^n_{z,i,j,k+1/2} + B^n_{z,i,j,k-1/2} \right) \\
    \end{aligned}
$$




## 2. First Riemann Solve
The next step is the solve the Riemann problem with the first order variables.
This is identical to the standard Van Leer integrator for the hydrodynamic
variables; the magnetic field is a bit more complicated since the magnetic field
is stored as surface averages. The longitudinal field (i.e. the field that is
stored at the interface itself) does not require reconstruction and can be used
directly. The transverse fields (i.e. the fields parallel to the interface) are
reconstructed using a straight average, which has already been done in step 1.

The magnetic fluxes that are returned by the Riemann solver are the face
centered electric fields (section 5.3 of Stone et. al 2008)

## 3. Compute the Constrained Transport Electric Fields
Now we need to calculate the CT electric fields. These fields are *line
averaged* along each cell edge. These line averaged fields are constructed by a
complicated average of the face centered electric fields and their slopes which
are given by the flux of the magnetic field from the Riemann solve in the
previous step; i.e. the magnetic flux at a given interface is the electric field
at that interface.

On any face there are two non-zero electric fields; both transverse to the face.
The most important thing to note is that the component that is used to calculate
the field along a given edge is the component that is parallel to that edge.
i.e. if the edge points along the \\( z \\)-direction then you use the field
pointing along the \\( z \\)-direction, not the field in the \\( x \\) or \\( y
\\) direction.

We also need cell centered electric fields (reference fields) for computing the
slopes. This reference field can just be computed with the following cross
product.

$$
    \mathcal{E}_{i,j,k}^{ref,n} = - \left( \vec{v}^{n}_{i.j.k} \times \vec{B}^{n}_{i.j.k} \right)
$$

Example for computing the CT electric field in the \\( z \\)-direction. The
other direction can be computed by just substituting out the \\( z \\) index
with \\( x \\) or \\( y \\) and changing the derivatives appropriately, see the
image in this section for more details on derivatives.

$$
    \begin{aligned}
        \mathcal{E}_{z, i-1/2, j-1/2, k} = \frac{1}{4} \left( \mathcal{E}_{z, i-1/2, j, k} + \mathcal{E}_{z, i-1/2, j-1, k} + \mathcal{E}_{z, i, j-1/2, k} + \mathcal{E}_{z, i-1, j-1/2, k}\right) \\
        + \frac{\delta y}{8} \left( \left( \frac{\partial \mathcal{E}_z }{\partial y} \right)_{i-1/2, j-1/4, k} + \left(  \frac{\partial \mathcal{E}_z }{\partial y} \right)_{i-1/2, j-3/4, k} \right) \\
        + \frac{\delta x}{8} \left( \left( \frac{\partial \mathcal{E}_z }{\partial x} \right)_{i-1/4, j-1/2, k} + \left(  \frac{\partial \mathcal{E}_z }{\partial x} \right)_{i-3/4, j-1/2, k} \right)
    \end{aligned}
$$

Where the derivatives are computed using the "upwind" direction as follows.

$$
    \left( \frac{\partial \mathcal{E}_z }{\partial y} \right)_{i-1/2, j-1/4, k} =
        \begin{cases}
            \left( \frac{\partial \mathcal{E}_z }{\partial y} \right)_{i-1, j-1/4, k} & \text{for} \; v_{x, i-1/2} > 0
            \\
            \\
            \left( \frac{\partial \mathcal{E}_z }{\partial y} \right)_{i, j-1/4, k} & \text{for} \; v_{x, i-1/2} < 0
            \\
            \\
            \frac{1}{2} \left( \left( \frac{\partial \mathcal{E}_z }{\partial y} \right)_{i-1, j-1/4, k} + \left( \frac{\partial \mathcal{E}_z }{\partial y} \right)_{i, j-1/4, k} \right) & \text{otherwise}
            \\
            \\
        \end{cases}
$$

Where, for example, the derivatives are given by

$$
    \left( \frac{\partial \mathcal{E}_z }{\partial y} \right)_{i, j-1/4, k} =
    2 \left( \frac{\mathcal{E}_{z,i,j,k}^{ref} - \mathcal{E}_{z,i,j-1/2,k}}{\delta y} \right).
$$

The easiest way to see how to compute these derivatives is with the following
diagram from [Stone et al.
2008](https://iopscience.iop.org/article/10.1086/588755/pdf). Each edge requires
4 derivatives (in 3D anyway) and they are computed as differences between a
reference state and an edge state.

![Stone et al. 2008 Fig. 5](/assets/img/2021-post-assets/01-January/Stone-et-al.-2008-fig-5.png)


## 4. Perform the Half Time-step Update
Update all the hydro variables using this equation

$$
    \begin{aligned}
        \vec{U}^{n+1/2}_{i,j,k} = \vec{U}^{n}_{i,j,k}
        - \frac{\delta t}{\delta x} \left( \vec{F}^n_{x,i+1/2,j,k} - \vec{F}^n_{x,i-1/2,j,k} \right) \\
        - \frac{\delta t}{\delta y} \left( \vec{F}^n_{y,i+1/2,j,k} - \vec{F}^n_{y,i-1/2,j,k} \right) \\
        - \frac{\delta t}{\delta z} \left( \vec{F}^n_{z,i+1/2,j,k} - \vec{F}^n_{z,i-1/2,j,k} \right).
    \end{aligned}
$$

Update the magnetic field using these equations

$$
    \begin{aligned}
        B^{n+1/2}_{x,i-1/2,j,k} = B^{n}_{x,i-1/2,j,k}
        - \frac{\delta t}{\delta y} \left( \mathcal{E}^n_{z,i-1/2,j+1/2,k} - \mathcal{E}^n_{z,i-1/2,j-1/2,k} \right) \\
        + \frac{\delta t}{\delta z} \left( \mathcal{E}^n_{y,i-1/2,j,k+1/2} - \mathcal{E}^n_{y,i-1/2,j,k-1/2} \right)
    \end{aligned}
$$

$$
    \begin{aligned}
        B^{n+1/2}_{y,i,j-1/2,k} = B^{n}_{y,i,j-1/2,k}
        + \frac{\delta t}{\delta x} \left( \mathcal{E}^n_{z,i+1/2,j-1/2,k} - \mathcal{E}^n_{z,i-1/2,j-1/2,k} \right) \\
        - \frac{\delta t}{\delta z} \left( \mathcal{E}^n_{x,i,j-1/2,k+1/2} - \mathcal{E}^n_{x,i,j-1/2,k-1/2} \right)
    \end{aligned}
$$

$$
    \begin{aligned}
        B^{n+1/2}_{z,i,j,k-1/2} = B^{n}_{z,i-1/2,j,k}
        - \frac{\delta t}{\delta x} \left( \mathcal{E}^n_{y,i+1/2,j,k-1/2} - \mathcal{E}^n_{y,i-1/2,j,k-1/2} \right) \\
        + \frac{\delta t}{\delta y} \left( \mathcal{E}^n_{x,i,j+1/2,k-1/2} - \mathcal{E}^n_{x,i,j-1/2,k-1/2} \right).
    \end{aligned}
$$


## 5. Compute Cell Centered Magnetic Fields
Average the values on the faces to compute the cell centered magnetic fields.
We'll need this for the second order reconstruction.

$$
    \begin{aligned}
        B^{n+1/2}_{x,i,j,k,} = \frac{1}{2} \left( B^{n+1/2}_{x,i+1/2,j,k} + B^{n+1/2}_{x,i-1/2,j,k} \right) \\
        B^{n+1/2}_{y,i,j,k,} = \frac{1}{2} \left( B^{n+1/2}_{y,i,j+1/2,k} + B^{n+1/2}_{y,i,j-1/2,k} \right) \\
        B^{n+1/2}_{z,i,j,k,} = \frac{1}{2} \left( B^{n+1/2}_{z,i,j,k+1/2} + B^{n+1/2}_{z,i,j,k-1/2} \right) \\
    \end{aligned}
$$

## 6. Half Time-step Second Order Reconstruction
Now we need to perform the high order reconstruction. I will show the algorithm
for a second order (Piecewise Linear Method, PLM) reconstruction though any
method should work. Note that at a given face only the transverse components of
the electric field need to be reconstructed. The longitudinal component is
already given at the face.

1. Convert to primitive variables
2. Compute the limited slopes of all the primitive variables (except the
   transverse magnetic field). Stone & Gardiner 2009 suggests minmod but any TVD
   limiter should work fine
3. Compute the interface states using

    $$
        \begin{aligned}
            \vec{W}_{L, i+1/2} = \vec{W}_{i} + \frac{\delta \vec{W}_{i}^m}{2} \\
            \vec{W}_{R, i-1/2} = \vec{W}_{i} - \frac{\delta \vec{W}_{i}^m}{2} \\
        \end{aligned}
    $$

    where \\( \vec{W}\_{L/R, i+1/2} \\) is the state on the left or right side
    of the cell and \\( \delta \vec{W}\_{i}^m \\) is the monotonically limited
    slope.

## 7. Second Riemann Solve
Solve the Riemann problem again with the half time step MHD interface states
from step 6.


## 8. Compute the Constrained Transport Electric Fields
Repeat step 3 but using the fluxes from the second Riemann solve and the half
time step MHD variables.


## 9. Perform the Full Time-step Update
Update all the hydro variables using the below equations. Note that this is
almost identical to step 3 except we're updating the \\( t = n \\) state with
the \\( t=n+1/2 \\) fluxes/CT fields instead of the \\( t=n \\) fluxes/CT
fields.

$$
    \begin{aligned}
        \vec{U}^{n+1}_{i,j,k} = \vec{U}^{n}_{i,j,k}
        - \frac{\delta t}{\delta x} \left( \vec{F}^{n+1/2}_{x,i+1/2,j,k} - \vec{F}^{n+1/2}_{x,i-1/2,j,k} \right) \\
        - \frac{\delta t}{\delta y} \left( \vec{F}^{n+1/2}_{y,i+1/2,j,k} - \vec{F}^{n+1/2}_{y,i-1/2,j,k} \right) \\
        - \frac{\delta t}{\delta z} \left( \vec{F}^{n+1/2}_{z,i+1/2,j,k} - \vec{F}^{n+1/2}_{z,i-1/2,j,k} \right).
    \end{aligned}
$$

Update the magnetic field using these equations

$$
    \begin{aligned}
        B^{n+1}_{x,i-1/2,j,k} = B^{n}_{x,i-1/2,j,k}
        - \frac{\delta t}{\delta y} \left( \mathcal{E}^{n+1/2}_{z,i-1/2,j+1/2,k} - \mathcal{E}^{n+1/2}_{z,i-1/2,j-1/2,k} \right) \\
        + \frac{\delta t}{\delta z} \left( \mathcal{E}^{n+1/2}_{y,i-1/2,j,k+1/2} - \mathcal{E}^{n+1/2}_{y,i-1/2,j,k-1/2} \right)
    \end{aligned}
$$

$$
    \begin{aligned}
        B^{n+1}_{y,i,j-1/2,k} = B^{n}_{y,i,j-1/2,k}
        + \frac{\delta t}{\delta x} \left( \mathcal{E}^{n+1/2}_{z,i+1/2,j-1/2,k} - \mathcal{E}^{n+1/2}_{z,i-1/2,j-1/2,k} \right) \\
        - \frac{\delta t}{\delta z} \left( \mathcal{E}^{n+1/2}_{x,i,j-1/2,k+1/2} - \mathcal{E}^{n+1/2}_{x,i,j-1/2,k-1/2} \right)
    \end{aligned}
$$

$$
    \begin{aligned}
        B^{n+1}_{z,i-1/2,j,k} = B^{n}_{z,i-1/2,j,k}
        - \frac{\delta t}{\delta x} \left( \mathcal{E}^{n+1/2}_{y,i+1/2,j,k-1/2} - \mathcal{E}^{n+1/2}_{y,i-1/2,j,k-1/2} \right) \\
        + \frac{\delta t}{\delta y} \left( \mathcal{E}^{n+1/2}_{x,i,j+1/2,k-1/2} - \mathcal{E}^{n+1/2}_{x,i,j-1/2,k-1/2} \right).
    \end{aligned}
$$

## 10. Increment the Time by \\( \delta t \\)


And that's it! Just loop this until you've reached or exceed max time and you're
done!