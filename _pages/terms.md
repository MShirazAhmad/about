\documentclass[9pt,twocolumn,twoside]{osajnl}
\usepackage{amsmath}
\usepackage{soul}
\usepackage{float}
\usepackage{physics}
\usepackage{caption}
\usepackage{siunitx}
\include{commands.tex}
\newenvironment{absolutelynopagebreak}
  {\par\nobreak\vfil\penalty0\vfilneg
   \vtop\bgroup}
  {\par\xdef\tpd{\the\prevdepth}\egroup
   \prevdepth=\tpd}
\usepackage[numbered,framed]{matlab-prettifier}
\let\ph\mlplaceholder % shorter macro
\lstMakeShortInline"

\lstset{
  style              = Matlab-editor,
  basicstyle         = \mlttfamily,
  escapechar         = ",
  mlshowsectionrules = true,
}

%---------------------------------------------------------
% Begin Matlab Code (Macro is available in Custom_cmds.tex)
%\usepackage[T1]{fontenc}       % change font encoding to T1
\usepackage[framed,numbered]{matlab-prettifier}
% End Matlab Code
%---------------------------------------------------------
\usepackage{tikz}
\newcommand{\veryshortarrow}[1][3pt]{\mathrel{%
		\hbox{\rule[\dimexpr\fontdimen22\textfont2-.2pt\relax]{#1}{.4pt}}%
		\mkern-4mu\hbox{\usefont{U}{lasy}{m}{n}\symbol{41}}}}
\usetikzlibrary{shapes,arrows}
\journal{ol} % Choose journal (ao, aop, josaa, josab, ol)

% See template introduction for guidance on setting shortarticle option
\setboolean{shortarticle}{false}
% true = letter / tutorial
% false = research / review article
% (depending on journal).

\ifthenelse{\boolean{shortarticle}}{\colorlet{color2}{color2b}}{\colorlet{color2}{color2}} % Automatically switches colors for short articles

\title{Reflection and Transmission of Light from Multilayer Films: An easy approach, using MATLAB}

\author[1]{M.Shiraz Ahmad\footnote{17120006@lums.edu.pk}}
\affil[1]{Department of Physics, Syed Babar Ali School of Science and Engineering (SBASSE), Lahore University of Management Sciences (LUMS), Opposite Sector U, D.H.A., Lahore 54792, Pakistan}
\dates{Compiled \today}

\ociscodes{.}

\doi{\url{http://dx.doi.org/10.1364/ao.XX.XXXXXX}}

\begin{abstract}

	When optical beam hits a multilayered system of different refractive indices, it gets reflected, refracted, and absorbed in a way that can be derived from the Fresnel equations. But, with increasing number of layers, math becomes complicated.  We have designed a MATLAB algorithm underlying the transfer--matrix method for the calculation of the optical properties of multilayered system and have verified it with experimental observations.
\end{abstract}

\setboolean{displaycopyright}{true}

\begin{document}

	\maketitle
	\thispagestyle{fancy}

	\ifthenelse{\boolean{shortarticle}}{\ifthenelse{\boolean{singlecolumn}}{\abscontentformatted}{\abscontent}}{}
	\section{Introduction}
	The optical beam passing through the interface of different refractive indices, changes its direction towards/away normal to the interface, and the changed direction can be calculated mathematically using refractive indices making that interface. To understand this, we can consider an example illustrated in Figure~\ref{fig:1}. Assuming $n_2$ to be sand. If a car enters into a region with higher refractive index at oblique angle, its right front wheel enters into an area of $n_2$ earlier than the left front wheel, hence starts facing lossy force earlier, causing a change in direction towards the normal. Same intuition can help to predict diverted optical path of optical beam.
	\\
	But if a linearly polarized light faces an interface of higher refractive index it gets refracted and reflected. The direction of beam propagation ($\vec {k}$) is shown in figure~\ref{fig:2} and sinusoidal waves shows that the oscillation of electric field is perpendicular to the direction of wave propagation. Optical beam is characterized as $p$--polarized, if electric field oscillations are perpendicular to the plane formed by incident, reflected and transmitted beam, and $p$ if oscillations are in the same plane.
	The direction of reflected and transmitted beams can be calculated by using Snell's Law and intensities can be computed via Fresnel coefficients as:
	\begin{align}
	r_p &= \frac{n_2\cos\theta_1-n_1\cos\theta_2}{n_2\cos\theta_1+n_1\cos\theta_2}, \\
	r_s &= \frac{n_1\cos\theta_1-n_2\cos\theta_2}{n_1\cos\theta_1+n_2\cos\theta_2}, \\
	t_p &= \frac{2 n_1\cos\theta_1}{n_2\cos\theta_1+n_1\cos\theta_2}, \\
	r_s &= \frac{2 n_1\cos\theta_1}{n_1\cos\theta_1+n_2\cos\theta_2}.
	\end{align}
		    \begin{figure}[H]
    	\centering
    	\includegraphics[width=6.5cm]{SneilsLaw}
    	\caption{Car entering into sand (intuition)}
    	\label{fig:1}

    	\centering
    	\includegraphics[width=6.5cm]{ppol}
    	\caption{Refraction of optical beam at the interface between two media of different refractive indices}
    	\label{fig:2}
    \end{figure}
    	These coefficients can easily be used on single interface, but for multilayered system, matrix transformation method is more useful.
\begin{absolutelynopagebreak}
\subsection[short title]{Matrix Method}


	Suppose we have a multilayered system\cite{zangwill_zangwill_2018} having $N$ refractive indices stacked together making $N-1$ interfaces with refractive index $n_j$, impedance $Z_j$, thickness $d_j$ for layer $j$. Also the layer $0$ is semi--infinite with $Z = - \infty$ and layer $N$ is being treated semi--infinite with $Z =  \infty$ and phase change is $\phi_j$.
\nobreak
	\begin{align}
	\mqty(E_{j-1} \\ H_{j-1}) &= \mqty(\cos \phi_j & i\sin \phi_j / Z_j \\ Z_j i\sin \phi_j & \sin \phi_j)\mqty(E_{j} \\ H_{j}) \label{eq:6}
	\end{align}
\nobreak
	Eq.~(\ref{eq:6}) relates amplitudes in one layer to the next adjacent layer and therefore repeated application of transfer matrix allows us to propagate waves from one side of the multilayer system to the other using
\nobreak
	\begin{align}
	\mqty(E_{1} \\ H_{1}) &= \prod_{j=2}^{N-1} \mqty(\cos \phi_j & i\sin \phi_j / Z_j \\ Z_j i\sin \phi_j & \sin \phi_j)\mqty(E_{N} \\ H_{N}).
	\end{align}
\nobreak
	And the characteristic matrix\cite{Multilayer} for the entire system will be
\nobreak
	\begin{align}
	M= \mqty(m_{11} & m_{12} \\ m_{21} & m_{22}) &= \prod_{j=1}^{N-1} \mqty(\cos \phi_j & i\sin \phi_j / Z_j \\ Z_j i\sin \phi_j & \sin \phi_j).
	\end{align}
\nobreak
	Here, $Z_j = \sqrt{\frac{\epsilon_0}{\mu_0}}n_j\cos\theta_j$ and by applying the boundary conditions for figure~(\ref{fig:3}), we have
\nobreak
	\begin{align}
	Z_0 = \sqrt{\frac{\epsilon_0}{\mu_0}}n_0\cos\theta_i  \quad\text{and}\quad
	Z_N = \sqrt{\frac{\epsilon_0}{\mu_0}}n_N\cos\theta_t.
	\end{align}
\nobreak
	Consequently,
\nobreak
	\begin{align}
	r &= \frac{Z_{1} m_{11} + Z_{1} Z_{N} m_{12} - m_{21} - Z_{N}m_{22}}{Z_{1} m_{11} + Z_{1} Z_{N} m_{12} + m_{21} + Z_{N}m_{22}}\label{eq:9}\\
	t &= \frac{2Z_{1}}{Z_{1} m_{11} + Z_{1} Z_{N} m_{12} + m_{21} + Z_{N}m_{22}}\label{eq:10}
	\end{align}
\nobreak
	To find $r$ or $t$ for any configuration of multilayered system, we only need to compute the characteristic matrices for each film, multiply them and substitute resulting matrix elements into the Eqs.~(\ref{eq:9}) and~(\ref{eq:10}).
\nobreak    	
    	\begin{figure}[H]
    		\centering
    		\includegraphics[width=6.5cm]{MultiLayers}
    		\caption{Propagation of optical beam through a multilayer structure consisting of the materials with different indices of refraction.  }
    		\label{fig:3}
    	\end{figure}
    To find reflection and transmission coefficients, we have
\nobreak
	\begin{align}
	R = rr' \quad\text{and}\quad T = tt',
	\end{align}
	where $r'$ and $t'$ are the complex conjugates of $r$ and $t$.
\end{absolutelynopagebreak}~
\section{Algorithm}

	We implement the matrix transformation method via MATLAB. Syntax of such function is
    \begin{align*}
        [\theta_\text{incident},R_s,R_p,T_s,T_p]=\textit{MultiLayerFilm}(n_{1 \rightarrow N},d_{2 \rightarrow K},\theta_\text{incident},\lambda.]
    \end{align*}

	Here \textit{MultiLayerFilm} is the MATLAB function whose algorithm is shown in algorithm~(1), $n_{1 \rightarrow N},d_{2 \rightarrow K},\theta_\text{incident},\lambda$ are input arguments and function gives output values.

		\begin{algorithm}
		\caption{$\textit{MultiLayerFilm}(n_{1 \rightarrow N},d_{2 \rightarrow K},\theta_{Incident},Lambda)$\label{alg:euclid}\\
		}
		\begin{algorithmic}[1]
			\For{$\theta_i := \theta_{Incident}^*$}
			\State $\theta_{i_{1 \rightarrow N}}=\textit{SneilsLaw}(n_{1 \rightarrow N},\theta_i)$
			\State $\phi_{2\rightarrow K} = n_{2 \rightarrow K}d_{2 \rightarrow K}\frac{2\pi}{\lambda}$ \Comment{here $K=N-1$}
			\State $Z_s = \sqrt{\frac{\epsilon_0}{\mu_0}}n_{1 \rightarrow N}\cos\theta_{1 \rightarrow N}$
			\State $m_s = \textit{Matrix}(\phi_{2 \rightarrow K},Z_s)$
			\State $[R_s,T_s] = \textit{RT}(m_s,Z_{s1},Z_{sN})$
			\State $Z_p = \sqrt{\frac{\epsilon_0}{\mu_0}}n_{1 \rightarrow N}/\cos\theta_{1 \rightarrow N}$
			\State $m_p = \textit{Matrix}(\phi_{2 \rightarrow K},Z_)$
			\State $[R_p,T_p] = \textit{RT}(m_p,Z_{p1},Z_{pN})$
			\EndFor
			\State \textit{End.}
			\State $\textit{rtplot}(\theta_{Incident}^*,R_s,R_p,T_s,T_p)$
		\end{algorithmic}
		$^* \theta_{Incident}$ is an array.\\
		User defined functions are in boldface.
	\end{algorithm}

	Let we have two layers of thickness \SI{1}{\milli\meter} and \SI{0.2}{\milli\meter} separated with distance of \SI{0.3}{\milli\meter} having refractive indices  $1.4, 1.5$, wavelength \SI{1547}{\nano\meter}. Now we want to find $R_s, R_p, T_s, T_p$ for incident angles from \ang{0} to \ang{90} with the following function.
	\lstinputlisting[style=Matlab-editor,basicstyle=\mlttfamily]{./codes/command.m}

	This generate arrays \texttt{Incident, RS, RP, TS, TP} corresponding to $\theta_\text{incident}, R_s, R_p, T_s, T_p$, as shown in figure~\ref{fig:4}

	\begin{figure}[H]
		\centering
		\includegraphics[scale=1.0]{MatlabWorkSpace.png}
		\caption{Workspace of MATLAB showing output values of function \textit{MultiLayerFilm}.}
		\label{fig:4}
	\end{figure}
~
\begin{absolutelynopagebreak}
\section{Experimental Setup}
	Experimental setup is shown in Figure~\ref{fig:Schematic diagram}. We used two transparent materials of thickness \SI {1}{\milli\meter}, \SI{2.288}{\milli\meter} having refractive indices $1.47, 1.5007$ (at $\lambda = \SI{1547}{\nano\meter}$ ) with separation of \SI{0.302}{\milli\meter} and placed on a rotating table. Optical power sensor that can be rotated along the table to measure transmitted/reflected beam. Actual setup is shown in Figure~\ref{fig:setup} and measured reflection and transmission coefficients for $\theta_i \text{ from } \ang{1} \text{ to } \ang{90}$ and measured corresponding transmission and reflection power amplitudes, for both $s$ and $p$ polarized optical beams.
\section{Results and Discussion}
	Figure~\ref{fig:6} show results of our experimentally measured transmission/reflection intensities, measured for different incident angles $\theta_i(\ang{0} \rightarrow \ang{90})$. Here lines represent theoretical plots generated by algorithm~(\ref{alg:euclid}) and stars represent experimental measurements which exactly matches to the observations based on MATLAB algorithm.
	\begin{figure}[H]
		\centering
		\includegraphics[width=\linewidth]{./codes/Results}
		\caption{Power coefficients, lines show theoretical and stars show experimental results.}
		\label{fig:6}
	\end{figure}
	\begin{figure}[H]
		\centering
		\includegraphics[width=7cm]{experiment.png}
		\caption{Screen shot of experimental setup}
		\label{fig:setup}
	\end{figure}
\end{absolutelynopagebreak}
\begin{figure}[H]
		\centering
		\includegraphics[width=6.5cm]{Setup}
		\caption{Schematic diagram of experimental setup.}
		\label{fig:Schematic diagram}
\end{figure}
\section{Appendix}
%\begin{appendices}
\subsection{MATLAB Functions}

\subsubsection{MultiLayerFilm}
\lstinputlisting[style=Matlab-editor,basicstyle=\mlttfamily]{./codes/MultiLayerFilm.m}

\subsubsection{Matrix}
\lstinputlisting[style=Matlab-editor,basicstyle=\mlttfamily]{./codes/Matrix.m}

\subsubsection{$R\_T$}
\lstinputlisting[style=Matlab-editor,basicstyle=\mlttfamily]{./codes/R_T.m}

\subsubsection{norm2unity}
\lstinputlisting[style=Matlab-editor,basicstyle=\mlttfamily]{./codes/norm2unity.m}

\subsubsection{rtplot}
\lstinputlisting[style=Matlab-editor,basicstyle=\mlttfamily]{./codes/rtplot.m}

\subsubsection{SnellsLaw}
\lstinputlisting[style=Matlab-editor,basicstyle=\mlttfamily]{./codes/SnellsLaw.m}

%\end{appendices}

\bibliography{sample.bib}{}
\bibliographystyle{plain}
\end{document}
