EulerImplicitSolver  
===================

This component belongs to the category of [integration schemes or ODE Solver](https://www.sofa-framework.org/community/doc/main-principles/system-resolution/integration-schemes/). This scheme builds the system following an implicit scheme: forces are considered based on the state information at the next time step <img class="latex" src="https://latex.codecogs.com/png.latex?x(t+dt)" title="Unknown state"/>, unknown at the current time step.

Looking at continuum mechanics, the linear system <img class="latex" src="https://latex.codecogs.com/png.latex?$$\mathbf{A}x=b$$" title="Linear system" /> arises from the dynamic equation. This dynamic is written as follows but other physics (like heat transfer) result in a similar equation:

<img class="latex" src="https://latex.codecogs.com/png.latex?$$\mathbf{M}\Delta%20v=dt\left(f(x,t)\right)$$" title="Dynamic system" />

where <img class="latex" src="https://latex.codecogs.com/png.latex?$$x$$" title="DOF at next time step system" /> is the degrees of freedom, <img class="latex" src="https://latex.codecogs.com/png.latex?$$\mathbf{M}$$" title="Mass matrix" /> the mass matrix and <img class="latex" src="https://latex.codecogs.com/png.latex?$$f(x,t)$$" title="Forces" /> a function of <img class="latex" src="https://latex.codecogs.com/png.latex?$$x$$" title="DOF" /> (and possibly its derivatives) acting on our system. In the case of the EulerImplicitSolver, this equation can be written: 

<img class="latex" src="https://latex.codecogs.com/png.latex?$$M%20\Delta%20v=dt%20\cdot%20f(x(t+dt))$$" title="Implicit dynamic system" />

by using a Taylor expansion, we get:

<img class="latex" src="https://latex.codecogs.com/png.latex?$$M%20\Delta%20v=dt%20\cdot%20\left(%20f(x(t)+\cdot%20\frac{\partial%20f}{\partial%20x}%20\Delta%20x%20\right)$$" title="Implicit dynamic system" />

since we have: <img class="latex" src="https://latex.codecogs.com/png.latex?$$\Delta%20x=dt(v(t)+\Delta%20v)$$" title="Implicit scheme" />, then:

<img class="latex" src="https://latex.codecogs.com/png.latex?$$M\Delta%20v=dt\cdot%20\left(%20f(x(t)+dt\cdot%20\frac{\partial%20f}{\partial%20x}v(t)+dt\cdot%20\frac{\partial%20f}{\partial%20x}%20\Delta%20v%20\right)$$" title="Implicit dynamic system" />

Finally, gathering the unknown (depending on <img class="latex" src="https://latex.codecogs.com/png.latex?$$\Delta%20v$$" title="Unknown delta of velocity" />) in the left hand side, we have:

<img class="latex" src="https://latex.codecogs.com/png.latex?$$\left(%20M-dt^2%20\cdot%20\frac{\partial%20f}{\partial%20x}%20\right)%20\Delta%20v=dt\cdot%20f(x(t)+dt^2\cdot%20\frac{\partial%20f}{\partial%20x}v(t)$$" title="Implicit dynamic system" />

In this equation, we can notice the same explicit contribution <img class="latex" src="https://latex.codecogs.com/png.latex?$$dt\left(f(x(t))\right)$$" title="Explicit contribution" />. Just like in the explicit case (see [EulerExplicitSolver](https://www.sofa-framework.org/community/doc/using-sofa/components/odesolver/eulerexplicitsolver/)), this part is implemented in the same function `addForce()`. We can also notice the appearance of the stiffness matrix : <img class="latex" src="https://latex.codecogs.com/png.latex?$$\mathbf{K}_{ij}=\textstyle\frac{\partial%20f_i}{\partial%20x_j}$$" title="Implicit contribution" />. The stiffness matrix <img class="latex" src="https://latex.codecogs.com/png.latex?$$\mathbf{K}$$" title="Stiffness matrix" /> is a symetric matrix, can either be linear or non-linear regarding <img class="latex" src="https://latex.codecogs.com/png.latex?$$x$$" title="DOF" />.



It is important to note that, depending on the choice of LinearSolver (direct or iterative), the API functions called to build the system will not be the same.


Sequence diagram
----------------

<a href="https://github.com/sofa-framework/doc/blob/master/Images/integrationscheme/EulerImplicitSolver.png?raw=true"><img src="https://github.com/sofa-framework/doc/blob/master/Images/integrationscheme/EulerImplicitSolver.png?raw=true" title="Flow diagram for the EulerImplicitSolver"/></a>


Data  
----

The data **trapezoidalScheme**

The option is given to the user to hadd numerical Rayleigh damping using the data **rayleighStiffness** and **rayleighMass**. The description of the meaning and effect of these Rayleigh damping coefficients is given in [ODESolver](https://www.sofa-framework.org/community/doc/main-principles/system-resolution/integration-schemes/#rayleigh-damping).

The data **firstOrder** enables to use the EulerImplicitSolver at the order 1, which means that only the first derivative of the DOFs (state) x appears in the equation. Higher derivatives are absent. This option is for instance well suited for heat diffusion equation using only the first derivative of the temperature field: <img class="latex" src="https://latex.codecogs.com/png.latex?$$M\frac{\partial%20T}{\partial%20t}=\Delta%20T$$" title="Heat diffusion" />.



Usage  
-----  

The EulerImplicitSolver **requires**:

- a [LinearSolver](https://www.sofa-framework.org/community/doc/main-principles/system-resolution/linear-solvers/) to solve the linear system
- and a MechanicalObject to store the state vectors.


 
Example  
-------  
 
This component is used as follows in XML format:  
 
``` xml  
<EulerImplicitSolver name="ODEsolver" rayleighStiffness="0.1" rayleighMass="0.1" />
```  
 
or using Python:  
 
``` python  
node.createObject('EulerImplicitSolver', name='ODEsolver', rayleighStiffness='0.1' rayleighMass='0.1')  
```  
 
An example scene involving a StaticSolver is available in [*examples/Components/solver/EulerImplicitSolver.scn*](https://github.com/sofa-framework/sofa/blob/master/examples/Components/solver/EulerImplicitSolver.scn)