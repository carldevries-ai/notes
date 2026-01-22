### Gyroscope Model: What Do Additive Errors Represent?

## References
Several equations in this document can be referenced to equations or sections in Fundamentals of Spacecraft Attitude Determination and Control by Markley and Crassidis, but a few are provided for quick reference:

- Eq. (2.123) on page 45 $A(\vec{e}, \nu)=e^{-[\vec{\nu}\times]}$ where $\vec{\nu}=\nu\vec{e}$
- Section 2.10 Attitude Error Representations on page 59
- Section 3.1.1 Attitude Matrix on page 68

## Start
A triad of gyroscopes fixed to a body measures its angular velocity with respect to an inertial frame along each gyroscopes sensitive axis. The gyroscopes are mounted with their sensitive axes mutally orthogonal. This frame is called the body frame, $B$. The true angular velocity of the body frame respect to the inertial frame is measured in the body frame and is given by $\vec{\omega}_B^{B/I}$. The measured angular acceleration model just contains a Gaussian random noise compoent

$$ \tilde{\omega}_B^{B/I}=\vec{\omega}_B^{B/I}+\eta $$

Integrating the angular velocity produces an angle relative to the initial condition. Since there is an error term in the modeled angular velocity, the estimated body frame will rotate to a different location than where it truely rotated. *I want to confirm my intuition that the random error, $\eta$, represents the angular velocity of the estimated body frame with respect to the true body frame expressed in the body frame.*

Think of the true body to estimated body frame as a rotation from the true body frame, to a fixed inertial frame and from there to the estimated body frame attitude. We can write the DCM from the body frame to the estimated body frame as

$$ A^B_{\hat{B}}=\hat{A}^I_B A^B_I$$

where the hat notation (e.g, $\hat{A}$) represents the estimated attitude. As the attitude estimate gets close to the truth, the body to estimated body attitude matrix goes to identity. The derivative of this equation is

$$\dot{A}^B_{\hat{B}} = \dot{\hat{A}}^I_B A^B_I + \hat{A}^I_B \dot{A}^B_I$$

The DCM kinematics for the true angular velocity, with $A^I_B$ being the DCM from I to B, is (Markley and Crassidis Eq. 3.3)

$$\dot{A}^I_B = -[\vec{\omega}^{B/I}_B \times]A^I_B$$

The DCM kinematics for the estimated system is then

$$\dot{\hat{A}}^I_B = -[(\vec{\omega}^{B/I}_B + \eta) \times]\hat{A}^I_B$$

Substitute these into the derivative

$$\dot{A}^B_{\hat{B}} = (-[(\vec{\omega}^{B/I}_B + \eta) \times]\hat{A}^I_B) A^B_I + \hat{A}^I_B (-[\vec{\omega}^{B/I}_B \times]A^I_B)^T$$

Evaluate the transpose in the second term

### There is still something funky going on here or I worked it out completely incorrectly the first time. I expect the \omega terms to cancel, but I can't get the signs right with the skew-symmetric matrix identities I used.

$$\dot{A}^B_{\hat{B}} = -[(\vec{\omega}^{B/I}_B + \eta) \times]\hat{A}^I_B A^B_I + \hat{A}^I_B A^B_I[\vec{\omega}^{B/I}_B \times]$$

Reverse the cross product

$$\dot{A}^B_{\hat{B}} = -[(\vec{\omega}^{B/I}_B + \eta) \times]\hat{A}^I_B A^B_I + -[\vec{\omega}^{B/I}_B \times]\hat{A}^I_B A^B_I$$

Cancel common terms

$$\dot{A}^B_{\hat{B}} = -[\eta \times]\hat{A}^I_B A^B_I$$

And recognize from above that $ A^B_{\hat{B}}=\hat{A}^I_B A^B_I$ to produce

$$\dot{A}^B_{\hat{B}} = -[\eta \times]A^B_{\hat{B}}$$

and when matched with the form above

$$\dot{A}^B_{\hat{B}} = -[\vec{\omega}^{\hat{B}/B}_{\hat{B}} \times] A^B_{\hat{B}} = -[\eta \times] A^B_{\hat{B}}$$

so 

$$ \vec{\omega}^{\hat{B}/B}_{\hat{B}} = \eta $$

### which almost proves my intuition, but it's expressed in $\hat{B}$

though if I did everything right, Markley and Crassidis say I can do this

$$\dot{A}^B_{\hat{B}} = -[\vec{\omega}^{\hat{B}/B}_{\hat{B}} \times] A^B_{\hat{B}} = -A^B_{\hat{B}}[\vec{\omega}^{\hat{B}/B}_{B} \times] $$