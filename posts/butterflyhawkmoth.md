
# Butterfly Effect and Hawkmoth Effect

<div style="text-align: right"> BX Pan </div>

## Motivation

The interrogation of a deterministic physical view can be conducted in different ways. Despite the underlying **Principle of Uncertainty** rooted in quantum mechanics, even in a Newtonian playground, people have long realized the limitation of predictability for dynamic systems. 

The limitation is termed under various names, such as *Laplace Demon*(Laplace), *Three Body Dilemma*(Henry Poincare), *Butterfly Effect*(Lorenz), etc.. The last one is in fact over exaggerating although it successfully catched the public's eyes(See Orrell, Smith, etc. 2001 and Stephen Wolfram 2002).  

This post try to illustrate the idea of the limitation with numerical simulations, followed by a discussion of its effects on the unstable foundation of "***Climate Science***". 


## Toy Model -- the Lorenz System

The *Lorenz System* is selected here for its simplicity. It is derived from the **Oberbeck-Boussinesq Approximation** to the equations describing fluid circulation in a shallow layer of fluid, heated uniformly from below and cooled uniformly from above. This fluid circulation is known as **Rayleigh-Bénard Convection**. The fluid is assumed to circulate in two dimensions (vertical and horizontal) with periodic rectangular boundary conditions(wikipedia). The system is described as follows:

![lorenzsystem.jpg](https://raw.githubusercontent.com/lambdamore/lambdamore.github.io/master/figures/lorenz/lorenzsystem.jpg)

## Numerical Simulation

The equations can not be analytically solved. A numerical simulator using *Mathematica* is coded as follows:

	LorenzSystemSolution[x0_,y0_,z0_,a_;3,b_:26.5,c_:1]:=
	Block[{x,y,z,t,tend,eq,init,xs,ys,zs},
		tend = 500;
		eq = { x'[t] == a (y[t] - x[t]), 
          	y'[t] == x[t] (b - z[t]) - y[t], 
          	z'[t] == x[t] y[t] - c z[t]};
		init = {x[0] == x0, y[0] == y0, z[0] == z0};
		{xs, ys, zs} = NDSolveValue[{eq , init}, {x, y, z}, {t, 0, tend},
			MaxSteps -> \[Infinity]];
		Table[{xs[t], ys[t], zs[t]}, {t, 0, tend}]]
		
I set the default parameters to a=3, b=26.5, c=1. There is rich literature considering the parameter settings and properties of the system. For those who are interested, please refer to Hirsch, Smale & Devaney, 2003. 

Given the initial condition, I can draw the trajectory of the dynamical system. For instance:
 
![lorenzsimulation.jpg](https://raw.githubusercontent.com/lambdamore/lambdamore.github.io/master/figures/lorenz/lorenzsimulation.jpg)
 	
The initial *y* differs slightly by 0.000001. It seems that the trajectories are generally indistinguishable. But, if I compare the Euclidean Distance between the trajectories, the result is a little bit surprising:

![lorenzdistance.jpg](https://raw.githubusercontent.com/lambdamore/lambdamore.github.io/master/figures/)

Different magnitude of purtubation were added to initial *y*. For pertubations larger than 10^-16, we don't need to wait too long before the divergence of the two trajectories.  Similar things happen by tuning initial *x* or *z*. They diverge, then they never come back. The initial closeness does not guarantee likeliness forever. Just as marriage.

The divergent time here did not exceed 40 steps. It is reported that if we can initialize the Lorenz System with 4180 digit multiple precision, we can reach reliable simulation for 100000 steps, according to a 1200 CPU parallel simulation experiment(Shijun Liao, etc.. 2013). 


## *What could Not be Achieved, Here Comes to Pass*

### On the Value of Temporal Mean
The general pattern of the  trajectories are quite alike although they are not close enough in the Euclidean distance sense. A mathematical translation for the likeliness is that their temporal average location should be close. Below I do a uniform convolution over the trajectory to compare the temporal mean of two trajectories starting from pertubated initials:

![space-1.jpg](https://raw.githubusercontent.com/lambdamore/lambdamore.github.io/master/figures/)



### On the Value of Ensemble

## *What no-one could Describe, Is Here Accomplished*

## Conclusion
That information should help water managers
to predict what atmospheric rivers may
bring. Reservoir engineers in northern California
typically release precious water from
their reservoirs in the winter, so as to have
enough space behind the dam to cope with
the threat of flooding. With better knowledge
of when atmospheric rivers might
arrive and how much water they might carry,
says Ralph, engineers should be able to manage
that winter release more effectively

*It is not complicated, it's just a lot of it.* <div style="text-align: right"> -- Richard P. Feynman </div>

![space-1.jpg](https://raw.githubusercontent.com/lambdamore/lambdamore.github.io/master/figures/)

