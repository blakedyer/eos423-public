In this lab you will develop a transport model that is considerably more
flexible than your previous approach. This new model will be used to
explore how sediment flux and relative sea level impact transport and
stratigraphy.

Bulk sediment transport: diffusion
==================================

The change of elevation over time is proportional to the second partial
derivative of the topography with respect to space (the curvature). This
equation is sometimes referred to as the *hillslope application* of the
**diffusion equation**:

.. math:: \dfrac{\partial h}{\partial t} = K \dfrac{\partial^2 h}{\partial x^2} \label{eq:diff}\tag{diffusion equation}

The diffusion equation is derived from combining the **continuity
equation**, which expresses the conservation of mass:

.. math:: \dfrac{\partial h}{\partial t} = - \dfrac{\partial S}{\partial x} \label{eq:cont}\tag{continuity equation}

with an expression of sediment transport rate, :math:`S`, as a
**diffusive flux**. In this formulation, :math:`S` is linearly
proportional to slope:

.. math:: S = - K \dfrac{\partial h}{\partial x}\tag{diffusive flux}

In part **1.1** of this assignment we looked at a specific application
of bulk (diffusive) transport with an *analytical* solution. This you
will be designing a numerical model of bulk transport using an
*implicit* finite difference scheme (specifically the Crank-Nicolson
algorithm). The questions below are designed to showcase that your model
is correctly solving the diffusion equation.

Crank-Nicholson implicit scheme
===============================

The Crank-Nicolson method is numerically stable, implicit, and
second-order in time – :math:`O(\Delta t^2)`. The method solves for the
next time-step iteration of the system by taking the average of the
central-difference estimate of the second partial derivative of the
topography with respect to space at both the current time step and the
future timestep. Red terms are unknown at the current timestep (this is
the *implicit* part of the numerical method).

.. math::

   \dfrac{{\color{red}h_{i}^{t+\Delta t}}
       - h_i^t}{\Delta t}
       =
       \dfrac{K}{2}
       \left(
       \dfrac{
           h_{i-\Delta x}^{t}
           -
           2~h_{i}^{t}
           +
           h_{i+\Delta x}^{t}
       }
       {
           \Delta x^2
       }
       +
       \dfrac{
           {\color{red}
               h_{i-\Delta x}^{t+\Delta t}
           }
           -
           2~{\color{red}
               h_{i}^{t+\Delta t}
           }
           +
           {\color{red}
               h_{i+\Delta x}^{t+\Delta t}
           }
       }
       {
           \Delta x^2
       }
       \right)
       \\

It is useful to define :math:`r` as (this number is sometimes called the
Fourier number):

.. math::

   r=K\dfrac{\Delta t}{2~\Delta x^2}
       \tag{Fourier number}

to simplify the equation to the following form:

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

This equation forms a system of linear equations that can be rearranged
and solved using linear algebra. The general form of your problem (once
you have collected unknowns on one side and knowns on the other) will
be:

.. math::

   A
       {\color{red}h^{t+\Delta t} }
       =
       Bh^{t}
       +
       b^{t}

where :math:`A` and :math:`B` are square matrices with whose side-length
is equal to the length of :math:`h` and :math:`b` is a vector of
boundary conditions (additional flux in or out of each finite element in
:math:`h_i`). The matrix equation above is solved by multiplying both
sides by :math:`A^{-1}`:

.. math::

   {\color{red}h^{t+\Delta t} }
       =
       A^{-1}(Bh^{t}
       +
       b^{t})
