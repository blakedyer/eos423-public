Building stratigraphic records with matrix math
===============================================

In part **1.1** of this module, we learned about how in many instances,
delta morphology could be shown to be controlled by bulk sediment
transport. Mathematically, this claim means that *diffusion* is a
dominant process in building deltas, where the change of elevation over
time is proportional to the second partial derivative of the topography
with respect to space (the curvature). This equation is sometimes
referred to as the *hillslope application* of the **diffusion
equation**:

.. math::

   \dfrac{\partial h}{\partial t} = K \dfrac{\partial^2 h}{\partial x^2} \label{eq:diff}
       \tag{diffusion equation}

We first looked at steady state solutions of the diffusion equation
(i.e., where :math:`\dfrac{\partial h}{\partial t} = 0`, through use of
a moving reference frame). This allowed the derivation of an
*analytical* solution to the diffusion equation, and could be used to
predict the bathymetry of deltas whose morphology was not changing with
time.

| Of course, having tools to predict how delta morphology and coastline
  bathymetry can *change* as conditions change, such as from variations
  in sediment type, relative sea level or sediment supply, would be very
  useful. In studies of the sedimentary rock record and the history it
  tells, these types of boundary conditions frequently change. This need
  was the motivation for part **1.2**, where you built a numerical model
  of bulk transport using an *implicit* finite difference scheme called
  the Crank-Nicolson algorithm.

This approach estimates how topography changes with time by taking the
average of the central-difference estimate of the second partial
derivative of the topography with respect to space at both the current
time step and the future timestep:

.. math::

   {\color{red}h_{i}^{t+\Delta t}}
       -
       h_i^t
       =
       r~(h_{i-\Delta x}^{t}
       -
       2~h_{i}^{t}
       +
       h_{i+\Delta x}^{t}
       +
       {\color{red}
       h_{i-\Delta x}^{t+\Delta t}}
       -
       2~{\color{red}h_{i}^{t+\Delta t}}
       +
       {\color{red}h_{i+\Delta x}^{t+\Delta t}})
       \\

with :math:`r` known as the Fourier number:

.. math::

   r=K\dfrac{\Delta t}{2~\Delta x^2}
       \tag{Fourier number}

This equation forms a system of linear equations that can be rearranged
and solved using linear algebra:

.. math::

   \begin{bmatrix}
           2r+1 & -r   & 0    & \\
           -r   & 2r+1 & -r   & \\
           0    & -r   & 2r+1 & \\
       \end{bmatrix}
       \begin{bmatrix}
           \color{red}{h_0^{t+\Delta t} } \\
           \color{red}{h_i^{t+\Delta t}}  \\
           \color{red}{h_N^{t+\Delta t}}  \\
       \end{bmatrix}
       =
       \begin{bmatrix}
           1-2r & r    & 0    & \\
           r    & 1-2r & r    & \\
           0    & r    & 1-2r & \\
       \end{bmatrix}
       \begin{bmatrix}
           h_0 \\
           h_i \\
           h_N \\
       \end{bmatrix}
       +
       2r~
       \begin{bmatrix}
           b_0 \\
           b_i \\
           b_N \\
       \end{bmatrix}

If we call the matrices on the right-hand side and left-hand side
:math:`A` and :math:`B`, respectively, we can rewrite the expression as:

.. math::

   A
       {\color{red}h^{t+\Delta t} }
       =
       Bh^{t}
       +
       b^{t}

where :math:`A` and :math:`B` are square matrices with whose side-length
is equal to the length of :math:`h` and :math:`b` is a vector of
boundary conditions. The first and last entries of :math:`b` define the
boundary conditions of your model space - setting these will fix the
fluxes into the left and right edges of the system, and thus set the
topography at those edges. The matrix equation above is solved by
multiplying both sides by :math:`A^{-1}`:

.. math::

   {\color{red}h^{t+\Delta t} }
       =
       A^{-1}(Bh^{t}
       +
       b^{t})

Now you have the tools to solve the diffusion equation numerically. In
each time step, the current topography (:math:`h^t`) and boundary
conditions (:math:`b^t`) can be used to predict the topography in the
next time step (:math:`h^{t+\Delta t}`). This new topography, in turn,
will be used to predict the next time step, and so on, until your model
run ends. One interesting way to to use this model is to introduce a
prescribed sedimentation term, represented here by the vector :math:`s`:

.. math::

   {\color{red}h^{t+\Delta t} }
       =
       A^{-1}(Bh^{t}
       +
       b^{t}
       +
       s^{t})

| The vector :math:`s` can be used to add topography to some part of
  your model space, and then allow that added topography to be modified
  through diffusion. For example, if you wish to simulate delta growth,
  a sediment flux term (which you define) could be added where
  topography intersects local sea level (which you also define). If this
  intersection occurs at :math:`h_i`, then :math:`s_i` would become
  non-zero to reflect a growth in topography. As topography changes, so
  potentially can the location of sediment delivery.

As the old adage goes, *“All models are wrong, but some models are
useful.”* Models will *never* capture every single biological, chemical
or physical process at play in the natural world that may be having an
influence on the surface process you are studying. [1]_ Rather, models
should be designed to capture some aspect of how the natural world
works, and model experiments should be run to see how it responds to
different parameter values or forcings. Although it is very simple, the
numerical model you have developed for bulk sediment transport is rooted
in some well-founded assumptions about how sediment is transported.
Namely, that sediment fluxes should behave as *diffusive fluxes,* acting
to smooth topography over time.

| Thus, the model can be used to explore how topography changes under
  various boundary and initial conditions, and compared to real world
  data (for example, lithology logs from stratigraphic sections, or 2-D
  seismic data, measured across a shallow-to-deep transect of an ancient
  continental margin). If the model captures important aspects of the
  data, you can potentially make inferences about the processes recorded
  by your stratigraphic records (i.e., relative sea level rose, sediment
  flux increased, etc.). If the model fails to explain the data in some
  important way, this result suggests important processes are not
  currently captured by your model, and may spur more model development.
  Either outcome would be useful to science!

So, let’s get some practice using this model, examining the results, and
thinking about what those results mean...

.. [1]
   Another good saying goes, *“The best model of a cat is a cat.”*
