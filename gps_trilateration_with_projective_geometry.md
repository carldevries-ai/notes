### Solving the GPS Trilateration with Projective Geometry

The general form of quadric to which a sphere belongs is

$$\alpha x^2  + \eta y^2 + \theta z^2 + \beta xy + \gamma xz + \zeta yz + \delta x 
+ \eta y +\iota z + \kappa = 0$$

I want to represent an sphere in 3-D homogeneous coordinates and a homogeneous point is given by

$$\bold{x}^T\propto[x,y,z,1]$$

which we use to define a quadric $\bold{Q}$ which contains the parameters for the ellipse

$$\bold{x}^T\bold{Q}\bold{x}=0$$

The matrix containing the quadric coefficients takes the form
$$\bold{Q}\propto\begin{bmatrix} \alpha   & \beta/2   & \gamma/2 & \delta/2 \\
                                 \beta/2  & \epsilon  & \zeta/2  & \eta/2   \\
                                 \gamma/2 & \zeta/2   & \theta   & \iota/2  \\
                                 \delta/2 & \eta/2    & \iota/2  & \kappa   \\
\end{bmatrix}$$

A sphere centered at the point $\bold{x}_i^T\propto[x_i, y_i, z_i, 1]$ ($r_i =||\bold{x_i}||$) and a radius $R_i$ has the form

$$(x-x_i)^2 + (y-y_i)^2 + (z-z_i)^2 - R_i^2 = 0$$
$$x^2 + y^2 + z^2 - 2x_ix - 2y_iy - 2z_iz + x_i^2 + y_i^2 + z_i^2 - R_i^2 = 0$$

| $\bold{Q}$ Coeff. | Equation |
|-------------------|----------|
| $\alpha_i$          | $1$      |
| $\epsilon_i$        | $1$      |
| $\theta_i$          | $1$      |
| $\delta_i$          | $-2x_i$  |
| $\eta_i$            | $-2y_i$  |
| $\iota_i$           | $-2z_i$  |
| $\kappa_i$          | $r_i^2-R_i^2$|

The quadric matrix, $\bold{Q}$ takes the form
$$\bold{Q}\propto 
\begin{bmatrix} 1    &  0 & 0      & -x_i \\
                0    &  1 & 0      & -y_i \\
                0    &  0 & 1      & -z_i \\
                -x_i & -y_i & -z_i & r_i^2-R_i^2 \\
\end{bmatrix}
=
\begin{bmatrix}
    I             & -\bold{x}_i \\
    -\bold{x}_i^T & \kappa_i
\end{bmatrix}
$$

Let's say we have two spheres $\bold{Q}_1$ and $\bold{Q}_1$.

A generic quadric has 9 degrees of freedom (10 parameters less one because it's
a homogeneous structure and only unique up to a scale). For a sphere, add the 
two constraints 
$$\alpha = \beta = \gamma $$
plus three more
$$\beta = \gamma = \zeta = 0$$
for a total of 5 constraints resulting in 4 degrees of freedom. One degree each 
for the components of the center of the sphere and one for the radius.

Now we will look at forming a pencil of quadrics using two well-defined quadrics. 
The pencil of quadrics looks like 

**Note: This may be easier to reason about with conics first.**

$$\mu \bold{Q}_1 + \rho \bold{Q}_2 = 0$$
which is a mixing of the two quadrics, but because the equation is homogeneous
the two parameters collapse to a single parameter $\lambda$

$$\bold{Q}_1 + \lambda \bold{Q}_2 = 0$$

**Obviously there are issues as $\mu \rightarrow 0$ that are not discussed here.**

**Note: maybe a discussion on the Rayleigh quotient/variational form of the generalized eigenvalue problem. The extra pre-multiply by $x^T$ (in our case) is just a projection of
the general eigenvalue problem onto $v^T$.**

This can be solved as a generic eigenvalue problem, but can also be cast as a 
standard eigenvalue problem. Let's look at both approaches.

### Standard

Spheres are full rank quadrics and symmetric so $\bold{Q}_2$ is invertible. The
problem can then be case as the standard eigenvalue problem

$$\bold{Q}_2^{-1}\bold{Q}_1 + \lambda I = 0$$

Recall that the inverse of a matrix is $\bold{Q}^{-1} = \frac{adj(Q)}{det(Q)}$. Let $Q^*=adj(Q)$. Since the the matrices are homogeneous, they are only unique up to a scale so we can write

$$\bold{Q}_2^{*}\bold{Q}_1 + \lambda I = 0$$

Without proof, I know my I used the block matrix inversion formula because I know
the matrix for my sphere is invertible and further $I_{3x3} is invertible. It may
seem silly to compute the inverse to get the adjugate, but it makes for a better
simplification, so

$$\bold{Q}_i^* \propto det(\bold{Q}_i)Q_i^{-1}$$

and I simplified Schur's compliment (again unproven) ahead of time

$$S_{c_i} = \kappa_i-x_i^TI^{-1}x_i=-R_i^2$$

so because $det(I)$ is trivial and $R_i^2$ is a scalar

$$det(\bold{Q}) \propto det(I)det(S_{c_i}) = -3R_i^2$$

$$
\bold{Q}_i^{-1} \propto
\begin{bmatrix}
    I^{-1}+I^{-1}\bold{x}_iS_{c_i}^{-1}\bold{x}_i^TI^{-1} & -I^{-1}\bold{x}_iS_{c_i}^{-1} \\
    -S_{c_i}^{-1}\bold{x}_i^TI^{-1}                       & S_{c_i}^{-1} \\
\end{bmatrix}
=
\begin{bmatrix}
    I+\bold{x}_iS_{c_i}^{-1}\bold{x}_i^T & -\bold{x}_iS_{c_i}^{-1} \\
    -S_{c_i}^{-1}\bold{x}_i^T                     & S_{c_i}^{-1} \\
\end{bmatrix}
$$

We can construct the adjugate and simplify knowing that $det(S_{c_i}) = S_{c_i}$ 
because it's a scalar and that it's only unique up to a scale factor

$$
\bold{Q}_i^{*} \propto det(I)det(S_{c_i})
\begin{bmatrix}
    I+\bold{x}_iS_{c_i}^{-1}\bold{x}_i^T & -\bold{x}_iS_{c_i}^{-1} \\
    -S_{c_i}^{-1}\bold{x}_i^T                     & S_{c_i}^{-1} \\
\end{bmatrix}
=
\begin{bmatrix}
    -R_i^2I+\bold{x}_i\bold{x}_i^T & -\bold{x}_i \\
    -\bold{x}_i^T                   & 1 \\
\end{bmatrix}
$$

Sweet, what happens when we multiply by 

$$
\begin{bmatrix}
    -R_2^2I+\bold{x}_2\bold{x}_2^T & -\bold{x}_2 \\
    -\bold{x}_2^T                   & 1 \\
\end{bmatrix}
\begin{bmatrix}
    I             & -\bold{x}_1 \\
    -\bold{x}_1^T & \kappa_1
\end{bmatrix}
=
\begin{bmatrix}
    -R_2^2I+\bold{x}_2\bold{x}_2^T + \bold{x}_2\bold{x}_1^T & (R_2^2I-\bold{x}_2\bold{x}_2^T)\bold{x}_1 -\bold{x}_2 \kappa_1 \\
    -\bold{x}_2^T -\bold{x}_1^T   & \bold{x}_2^T\bold{x}_1+\kappa_1 \\
\end{bmatrix}
$$

$$
0=\bold{Q}_2^{*}\bold{Q}_1 + \lambda I =
\begin{bmatrix}
    -R_2^2I+\bold{x}_2\bold{x}_2^T + \bold{x}_2\bold{x}_1^T & (R_2^2I-\bold{x}_2\bold{x}_2^T)\bold{x}_1 -\bold{x}_2 \kappa_1 \\
    -\bold{x}_2^T -\bold{x}_1^T   & \bold{x}_2^T\bold{x}_1+\kappa_1 \\
\end{bmatrix}
+
\begin{bmatrix}
    \lambda I_{3x3} & 0_{3x1} \\
    0_{1x3} & \lambda\\
\end{bmatrix}
$$

$$
0=
\begin{bmatrix}
    -R_2^2I+\bold{x}_2\bold{x}_2^T + \bold{x}_2\bold{x}_1^T + \lambda I & (R_2^2I-\bold{x}_2\bold{x}_2^T)\bold{x}_1 -\bold{x}_2 \kappa_1 \\
    -\bold{x}_2^T -\bold{x}_1^T   & \bold{x}_2^T\bold{x}_1+\kappa_1 + \lambda\\
\end{bmatrix}
$$

$$
0=
det(
\begin{bmatrix}
    -R_2^2I+\bold{x}_2\bold{x}_2^T + \bold{x}_2\bold{x}_1^T + \lambda I & (R_2^2I-\bold{x}_2\bold{x}_2^T)\bold{x}_1 -\bold{x}_2 \kappa_1 \\
    -\bold{x}_2^T -\bold{x}_1^T   & \bold{x}_2^T\bold{x}_1+\kappa_1 + \lambda \\
\end{bmatrix}
)
$$

Using the determinant identity for block matrices

$$
0=det(-R_2^2I+\bold{x}_2\bold{x}_2^T + \bold{x}_2\bold{x}_1^T + \lambda I)det(\bold{x}_2^T\bold{x}_1+\kappa_1 + \lambda - (-\bold{x}_2^T -\bold{x}_1^T)(-R_2^2I+\bold{x}_2\bold{x}_2^T + \bold{x}_2\bold{x}_1^T + \lambda I)^{-1}((R_2^2I-\bold{x}_2\bold{x}_2^T)\bold{x}_1 -\bold{x}_2 \kappa_1))
$$

So the matrix drops rank when either determinant goes to zero

$$
0=det(-R_2^2I+\bold{x}_2\bold{x}_2^T + \bold{x}_2\bold{x}_1^T + \lambda I)
$$