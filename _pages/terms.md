---
permalink: /terms/
title: "Terms and Privacy Policy"
---

# Reflection and Transmission of Light from Multilayer Films: An easy approach, using MATLAB

When optical beam hits a multilayered system of different refractive indices, it gets reflected, refracted, and absorbed in a way that can be derived from the Fresnel equations. But, with increasing number of layers, math becomes complicated.  We have designed a MATLAB algorithm underlying the transfer--matrix method for the calculation of the optical properties of multilayered system and have verified it with experimental observations.

## Introduction

The optical beam passing through the interface of different refractive indices, changes its direction towards/away normal to the interface, and the changed direction can be calculated mathematically using refractive indices making that interface. To understand this, we can consider an example illustrated in Figure. Assuming $n_2$ to be sand. If a car enters into a region with higher refractive index at oblique angle, its right front wheel enters into an area of $n_2$ earlier than the left front wheel, hence starts facing lossy force earlier, causing a change in direction towards the normal. Same intuition can help to predict diverted optical path of optical beam.

But if a linearly polarized light faces an interface of higher refractive index it gets refracted and reflected. The direction of beam propagation ($\vec {k}$) is shown in figure and sinusoidal waves shows that the oscillation of electric field is perpendicular to the direction of wave propagation. Optical beam is characterized as $p$--polarized, if electric field oscillations are perpendicular to the plane formed by incident, reflected and transmitted beam, and $p$ if oscillations are in the same plane.
The direction of reflected and transmitted beams can be calculated by using Snell's Law and intensities can be computed via Fresnel coefficients as:
$$r_p &= \frac{n_2\cos\theta_1-n_1\cos\theta_2}{n_2\cos\theta_1+n_1\cos\theta_2}, \\
r_s &= \frac{n_1\cos\theta_1-n_2\cos\theta_2}{n_1\cos\theta_1+n_2\cos\theta_2}, \\
t_p &= \frac{2 n_1\cos\theta_1}{n_2\cos\theta_1+n_1\cos\theta_2}, \\
r_s &= \frac{2 n_1\cos\theta_1}{n_1\cos\theta_1+n_2\cos\theta_2}. $$


width=6.5cmSneilsLaw
Car entering into sand (intuition)


width=6.5cm ppol
Refraction of optical beam at the interface between two media of different refractive indices


These coefficients can easily be used on single interface, but for multilayered system, matrix transformation method is more useful.

Suppose we have a multilayered system having $N$ refractive indices stacked together making $N-1$ interfaces with refractive index $n_j$, impedance $Z_j$, thickness $d_j$ for layer $j$. Also the layer $0$ is semi--infinite with $Z = - \infty$ and layer $N$ is being treated semi--infinite with $Z =  \infty$ and phase change is $\phi_j$.

$$\begin{pmatrix} E_{j-1} \\ H_{j-1}\end{pmatrix}$$
$$= \begin{pmatrix} \cos \phi_j & i\sin \phi_j / Z_j \\ Z_j i\sin \phi_j & \sin \phi_j\end{pmatrix} \begin{pmatrix} E_{j} \\ H_{j}\end{pmatrix} $$

Eq. relates amplitudes in one layer to the next adjacent layer and therefore repeated application of transfer matrix allows us to propagate waves from one side of the multilayer system to the other using

$$\begin{pmatrix} E_{1} \\ H_{1}\end{pmatrix} $$
$$= \prod_{j=2}^{N-1} \begin{pmatrix} \cos \phi_j & i\sin \phi_j / Z_j \\ Z_j i\sin \phi_j & \sin \phi_j\end{pmatrix} \begin{pmatrix} E_{N} \\ H_{N}\end{pmatrix}. $$

And the characteristic matrix\cite{Multilayer} for the entire system will be

$$M= \begin{pmatrix} m_{11} & m_{12} \\ m_{21} & m_{22}\end{pmatrix} $$
$$= \prod_{j=1}^{N-1} \begin{pmatrix} \cos \phi_j & i\sin \phi_j / Z_j \\ Z_j i\sin \phi_j & \sin \phi_j \end{pmatrix}. $$

Here, $Z_j = \sqrt{\frac{\epsilon_0}{\mu_0}}n_j\cos\theta_j$ and by applying the boundary conditions for figure, we have

$$Z_0 = \sqrt{\frac{\epsilon_0}{\mu_0}}n_0\cos\theta_i  \quad\text{and}\quad
Z_N = \sqrt{\frac{\epsilon_0}{\mu_0}}n_N\cos\theta_t. $$

Consequently,

$$r &= \frac{Z_{1} m_{11} + Z_{1} Z_{N} m_{12} - m_{21} - Z_{N}m_{22}}{Z_{1} m_{11} + Z_{1} Z_{N} m_{12} + m_{21} + Z_{N}m_{22}}\\
t &= \frac{2Z_{1}}{Z_{1} m_{11} + Z_{1} Z_{N} m_{12} + m_{21} + Z_{N}m_{22}}$$

To find $r$ or $t$ for any configuration of multilayered system, we only need to compute the characteristic matrices for each film, multiply them and substitute resulting matrix elements into the Eqs.) and.


width=6.5cm MultiLayers
Propagation of optical beam through a multilayer structure consisting of the materials with different indices of refraction.
To find reflection and transmission coefficients, we have

$$R = rr' \quad\text{and}\quad T = tt', $$
where $r'$ and $t'$ are the complex conjugates of $r$ and $t$.


We implement the matrix transformation method via MATLAB. Syntax of such function is
$$\theta_\text{incident},R_s,R_p,T_s,T_p]={MultiLayerFilm}(n_{1 \rightarrow N},d_{2 \rightarrow K},\theta_\text{incident},\lambda. $$

Here MultiLayerFilm is the MATLAB function whose algorithm is shown in algorithm~(1), $n_{1 \rightarrow N},d_{2 \rightarrow K},\theta_\text{incident},\lambda$ are input arguments and function gives output values.


{${MultiLayerFilm}(n_{1 \rightarrow N},d_{2 \rightarrow K},\theta_{Incident},Lambda)$\\


$\theta_{i_{1 \rightarrow N}}={SneilsLaw}(n_{1 \rightarrow N},\theta_i)$
$\phi_{2\rightarrow K} = n_{2 \rightarrow K}d_{2 \rightarrow K}\frac{2\pi}{\lambda}$
$Z_s = \sqrt{\frac{\epsilon_0}{\mu_0}}n_{1 \rightarrow N}\cos\theta_{1 \rightarrow N}$
$m_s = {Matrix}(\phi_{2 \rightarrow K},Z_s)$
$[R_s,T_s] = {RT}(m_s,Z_{s1},Z_{sN})$
$Z_p = \sqrt{\frac{\epsilon_0}{\mu_0}}n_{1 \rightarrow N}/\cos\theta_{1 \rightarrow N}$
$m_p = {Matrix}(\phi_{2 \rightarrow K},Z_)$
$[R_p,T_p] = {RT}(m_p,Z_{p1},Z_{pN})$



Let we have two layers of thickness  1 milli meter and 0.2 milli meter} separated with distance of 0.3 milli meter having refractive indices  $1.4, 1.5$, wavelength  1547 nano meter}. Now we want to find $R_s, R_p, T_s, T_p$ for incident angles from $\ang{0}$ to $\ang{90}$ with the following function.


This generate arrays Incident, RS, RP, TS, TP corresponding to $\theta_\text{incident}, R_s, R_p, T_s, T_p$, as shown in figure

scale=1.0 MatlabWorkSpace.png
Workspace of MATLAB showing output values of function MultiLayerFilm

## Experimental Setup

Experimental setup is shown in Figure. We used two transparent materials of thickness  1 milli meter, 2.288 milli meter having refractive indices $1.47, 1.5007$ (at $\lambda = 1547}{\nano\meter}$ ) with separation of 0.302 milli meter and placed on a rotating table. Optical power sensor that can be rotated along the table to measure transmitted/reflected beam. Actual setup is shown in Figure and measured reflection and transmission coefficients for $\theta_i \text{ from } \ang{1} \text{ to } \ang{90}$ and measured corresponding transmission and reflection power amplitudes, for both $s$ and $p$ polarized optical beams.

## Results and Discussion

Figure show results of our experimentally measured transmission/reflection intensities, measured for different incident angles $\theta_i(\ang{0} \rightarrow \ang{90})$. Here lines represent theoretical plots generated by algorithm~(\ref{alg:euclid}) and stars represent experimental measurements which exactly matches to the observations based on MATLAB algorithm.
