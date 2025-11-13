# Covariance of State Mapped through a Non-linear Function

The motivation for this was that I was simulating a system with a monocular visual 
odometry (VO) sensor, and I wanted the uncertainties of the direction of motion
vector. The absolute scale cannot be resolved with VO only, so it was difficult 
to evaluate the filter performance with inertial states only because they cannot be
expected to converge. This is a generic development of how to find a linearized
approximation to the covariance of values that have a non-linear mapping to the 
states.

Say we have the true state vector $x$, and the non-linear function of the states, 
$y=f(x)$. The generic first order Taylor series approximation is

$$y=f(x)\approx f(x_0) + \frac{\partial f(x)}{\partial x}\rvert_{x_0} (x-x_0) = f(x_0) + H(x-x_0)$$

The filter error is $e=x-\hat{x}$ where $\hat{x}$ is the filter state estimate.
The expected value of the error is $E[e]=0$, and the filter covariance is 
$P=E[ee^T]$. Compute the covariance of $y$,

$$P_y = E[(y-E[y])(y-E[y])^T]$$

and substitute in the first order Taylor series of $y$ which is the truth
state linearized about the estimated state. I think this makes sense since the 
estimate is known in the filter, but the truth is not. Let's look at a 
single parenthesis first which is

$$y-E[y]=(f(\hat{x}) + H(x-\hat{x})) - E[f(\hat{x}) + H(x-\hat{x})]$$
$$y-E[y]=(f(\hat{x}) + H(x-\hat{x})) - E[f(\hat{x})] - E[H(x-\hat{x})]$$

where the expected value of the 0th order term $E[f(\hat{x})]=f(\hat{x})$ which
cancels with the other 0th order term and by linearity $E[H(x-\hat{x})]=HE[(x-\hat{x})]=0$
resulting in

$$y-E[y]=H(x-\hat{x})$$

So, the covariance of $y$ is

$$P_y=E[(He)(He)^T]= E[Hee^TH^T]=HE[ee^T]H^T=HPH^T$$

The point here is that the partials in the Jacobian, H, is with respect to the 
states and not the errors in the covariance. I cannot figure out how to do it
if the state is linearized about the error because as mentioned before, the 
truth state is unknown.