<div id="table-of-contents">
<h2>Table of Contents</h2>
<div id="text-table-of-contents">
<ul>
<li><a href="#sec-1">1. Grid/mesh independence&#xa0;&#xa0;&#xa0;<span class="tag"><span class="GCI">GCI</span></span></a>
<ul>
<li><a href="#sec-1-1">1.1. Richardson extrapolation</a></li>
<li><a href="#sec-1-2">1.2. grid refinement ratio</a></li>
<li><a href="#sec-1-3">1.3. Example of Grid convergence study</a></li>
<li><a href="#sec-1-4">1.4. calculation steps</a></li>
<li><a href="#sec-1-5">1.5. Example of Grid convergence for wing:</a></li>
<li><a href="#sec-1-6">1.6. References</a></li>
</ul>
</li>
</ul>
</div>
</div>


# Grid/mesh independence     :GCI:<a id="sec-1" name="sec-1"></a>

keywords: Richardson's extrapolation, Grid convergence index
a summary of Richardson's extrapolation is [here](https://www.grc.nasa.gov/www/wind/valid/tutorial/bibliog.html#Roach94)

requirement: GCI < 5%

[a summary of GCI](https://www.grc.nasa.gov/www/wind/valid/tutorial/spatconv.html) from nasa web , local downloaded file is [here](C:\akmkemin\Backup\ANSYS\Geo_mesh_solver_post_processing\Convergence-Verification-validation\grid convergence) ( print version is in **BEM** file folder)

## Richardson extrapolation<a id="sec-1-1" name="sec-1-1"></a>

## grid refinement ratio<a id="sec-1-2" name="sec-1-2"></a>

-   Hexa mesh &#x2013;>>   grid refinement ratio
    -   double nodes along each coordinates (x, y,z)

-   Tetra mesh &#x2013;>> effective grid refinement ratio

**Definitions:**

\[
r_{ij} = h_i/h_j
\]

-   r: grid refinement ratio,
-   h: grid spacing

1.  effective grid refinement ratio

    For tetra mesh, the effective grid refinement ratio is defined as:
    
    \begin{equation}
    r_e =( \frac{N_1}{N_2})
    ^{1/D}
    \end{equation}
    
    Where \( N \) is the total number of grid point and \( D \) is the dimension of the flow domain. 

## Example of Grid convergence study<a id="sec-1-3" name="sec-1-3"></a>

The example is from [here](https://www.grc.nasa.gov/www/wind/valid/tutorial/spatconv.html), 
The Fortran 90 program  [verify.f90](c:/Users/exw692/Dropbox/Emacs/verify.f90) was written to carry out the calculations associated with a grid convergence study involving 3 or more grids
The program is compiled on a unix system through the commands:

`f90 verify.f90 -o verify`

It reads in an ASCII file (`prD.do`) through the standard input unit (5) that contains a list of pairs of grid size and value of the observed quantity f.

input data format:
`1.0 0.97050 2.0 0.96854 4.0 0.96178`
 `verify < prD.do > prD.out`

It assumes the values from the finest grid are listed first. The output is then written to the standard output unit (6) `prD.out`.
 The output from the of {\tt verify} for the results of Appendix A are:

\#+BEGIN<sub>EXAMPLE</sub>

&#x2014; VERIFY: Performs verification calculations &#x2014;

Number of data sets read =  3

Grid Size     Quantity

1.000000      0.970500
2.000000      0.968540
4.000000      0.961780

Order of convergence using first three finest grid 
and assuming constant grid refinement (Eqn. 5.10.6.1) 
Order of Convergence, p =  1.78618479

Richardson Extrapolation: Use above order of convergence
and first and second finest grids (Eqn. 5.4.1) 
Estimate to zero grid value, f<sub>exact</sub> =  0.971300304

Grid Convergence Index on fine grids. Uses p from above.
Factor of Safety =  1.25

Grid     Refinement            
Step      Ratio, r       GCI(%)
1  2      2.000000      0.103080
2  3      2.000000      0.356244

Checking for asymptotic range using Eqn. 5.10.5.2.
A ratio of 1.0 indicates asymptotic range.

Grid Range    Ratio
12 23      0.997980

&#x2014; End of VERIFY &#x2014;

\#+END \_EXAMPLE

## calculation steps<a id="sec-1-4" name="sec-1-4"></a>

1.  Complete at least 3 simulations (Coarse, medium, fine) with a constant refinement ratio, r, between them (in our example we use r=2)
2.  Choose a parameter indicative of grid convergence. In most cases, this should be the parameter you are studying. ie if you are studying drag, you would use drag.
3.  Calculate the order of convergence, p, using:

\begin{equation}
p=ln(\frac{f_3 - f_2}{f_2- f_1}) / \ln (r)
\end{equation}

where \( f_i \) is the solution at different meshes, f<sub>1</sub> is fine grid, \( r \) is *grid refinement ratio*.

1.  Perform a Richardson extrapolation to predict the value at h=0

\begin{equation}
f_e = f_1 + \frac{f_1 -f_2 }{r^p - 1}
\end{equation}

f<sub>e</sub>, exact numerical value ( continuum value at zero grid spacing)

1.  Calculate grid convergence index (GCI) for the medium and fine refinement levels

\begin{equation}
GCI_{fine} = \frac{F_s \vert \epsilon \vert }{r^p - 1}
\end{equation}

where \( F_s \) is a safety factor. the recommended value is 3 for two grids comparisons and 1.25 for three or more grids comparisons.

1.  Ensure that grids are in the asymptotic range of convergence by checking:
    \frac{GCI<sub>2,3</sub>}{r<sup>p</sup> &times; GCI<sub>1,2</sub>} \approxeq 1

## Example of Grid convergence for wing:<a id="sec-1-5" name="sec-1-5"></a>

<C:\akmkemin\Backup\academic\Notes\ANSYS\mesh\Modules\wings\wing-mesh-comparing-pointwise.doc>

## References<a id="sec-1-6" name="sec-1-6"></a>

Roache, P. J. Perspective: A Method for Uniform Reporting of Grid Refinement Studies, Journal of Fluids Engineering, Vol. 116, 1994; 405-413.

Roache, P. J. Quantification of Uncertainty in Computational Fluid Dynamics, in Annual Review of Fluid Mechanics

Roache, Patrick J. Verification and validation in computational science and engineering. Vol. 895. Albuquerque, NM: Hermosa, 1998.
