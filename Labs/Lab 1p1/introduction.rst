In this lab, you will develop a model of the profile of a prograding
delta. You will then use your model to understand the conditions that
explain variations in delta shape.

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

*Kenyon and Turcotte 1985* solves the diffusion equation analytically by
introducing a coordinate system that moves along with the landward edge
of the delta front, which is steadily prograding at a rate, :math:`u_0`:

.. math:: \xi = x - u_0t

.. math::

   t'  = t \label{eq:mrf}
       \tag{moving reference frame}

and the associated partial derivatives:

.. math::

   \begin{aligned}
       \dfrac{\partial h}{\partial x} & =\dfrac{\partial h}{\partial \xi}                                      \\
       \dfrac{\partial h}{\partial t} & =\dfrac{\partial h}{\partial t'} - u_0\dfrac{\partial h}{\partial \xi} \\
   \end{aligned}

Now, we can write the **diffusion equation within a moving reference
frame** as:

.. math::

   \begin{aligned}
       \dfrac{\partial h}{\partial t'} - u_0 \dfrac{\partial h}{\partial \xi} = K \dfrac{\partial^2 h}{\partial \xi^2} \label{eq:diffmrf}
   \end{aligned}

In this reference frame, the height of the delta does not change with
respect to time, so :math:`\dfrac{\partial h}{\partial t'}=0`, and this
form of the diffusion equation reduces to the ordinary differential
equation:

.. math::

   \begin{aligned}
       0 = \dfrac{d^2 h}{d \xi^2} + \dfrac{u_0}{K} \dfrac{d h}{d \xi}
   \end{aligned}

with the following solution:

.. math::

   \begin{aligned}
       h(\xi) = h_0 e^{\left(-\dfrac{u_0 \xi}{K}\right)}
   \end{aligned}
