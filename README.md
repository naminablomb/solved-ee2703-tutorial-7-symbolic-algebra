Download Link: https://assignmentchef.com/product/solved-ee2703-tutorial-7-symbolic-algebra
<br>
In this assignment, the focus will be on two powerful capabilities of Python:

<ul>

 <li>Symbolic Algebra</li>

 <li>Analysis of Circuits using Laplace Transforms</li>

</ul>

<h1>1          Analysis of Circuits using Laplace Transforms</h1>

Consider the following figure (from page 274 of Horowitz and Hill):

where <em>G </em>= 1<em>.</em>586 and <em>R</em><sub>1 </sub>= <em>R</em><sub>2 </sub>= 10<em>k</em>Ω and <em>C</em><sub>1 </sub>=<em>C</em><sub>2 </sub>= 10pF. This gives a 3dB Butterworth filter with cutoff frequency of 1<em>/</em>2<em>π</em>MHz. The circuit equations are

<table width="345">

 <tbody>

  <tr>

   <td width="330">                 <em>V</em><em>p        V</em>11+ <em>jωR</em><sub>2</sub><em>C</em><sub>2</sub></td>

   <td width="15">(2)</td>

  </tr>

  <tr>

   <td width="330"><em>V<sub>o </sub></em>= <em>G</em>(<em>V<sub>p </sub></em>−<em>V<sub>m</sub></em>)</td>

   <td width="15">(3)</td>

  </tr>

  <tr>

   <td width="330"><em>V</em><em>i </em>−<em>V</em>1             <em>V</em><em>p </em>−<em>V</em>1+                   + <em>jωC</em><sub>1</sub>(<em>V<sub>o </sub></em>−<em>V</em><sub>1</sub>)= 0</td>

   <td width="15">(4)</td>

  </tr>

 </tbody>

</table>

<em>o</em>

<em>V<sub>m </sub></em>=                                                                          (1)

=

<em>R</em>1                      <em>R</em>2

Solving for <em>V<sub>o </sub></em>in 3, we get

<em>GV</em><sub>1                 </sub>1

<em>V<sub>o </sub></em>=

2     1+ <em>jωR</em><sub>2</sub><em>C</em><sub>2</sub>

Thus, Eq. 4 becomes an equation for <em>V</em><sub>1 </sub>as follows:

<em>V<sub>i                             </sub></em>1        1            1                1                 <em>G           </em>1

+<em>V</em>

<em>R</em><sub>1                           </sub><em>R</em><sub>1             </sub><em>R</em><sub>2 </sub>1+ <em>jωR</em><sub>2</sub><em>C</em><sub>2             </sub><em>R</em><sub>2                            </sub>2 1+ <em>jωR</em><sub>2</sub><em>C</em><sub>2</sub>

The term involving <em>G </em>will dominate, and so we obtain

<em>V</em>1 ≈ 2<em>V</em><em>i </em>1+ <em>jωR</em>2<em>C</em>2

<em>G       jωR</em><sub>1</sub><em>C</em><sub>1</sub>

Substituting back into the expression for <em>V<sub>o </sub></em>we finally get

<em>V<sub>i</sub></em>

<em>V<sub>o </sub></em>≈       (5) <em>jωR</em><sub>1</sub><em>C</em><sub>1</sub>

We would like to solve this in Python and also get (and plot) the exact result. For this we need the sympy module.

<h1>2          Introduction to Sympy</h1>

Start <em>ipython </em>and import the sympy module

$ ipython

&gt;&gt;&gt; from sympy import *

&gt;&gt;&gt; init_session

&gt;&gt;&gt; x,y,z = symbols(’x y speed’)

Normally we invoke ipython and import * from pylab. Instead we invoke sympy. We are now ready to do symoblic work. The last line allows us to define variables that can be used for symbolic manipulations. The string inside the symbols command tells sympy how to print out expressions involving these symbols. For instance

&gt;&gt;&gt; x=y+1/z &gt;&gt;&gt; x

<em>y</em>+ <em>speed</em><u>1</u>

This is confusing, so just use the variable name for the symbol. One place where this is useful is for greek symbols:

&gt;&gt;&gt; w=symbols(’omega’) &gt;&gt;&gt; x=w*y+z omega*y+z

Note that symbols are not python variables:

&gt;&gt;&gt; expr=x+1 &gt;&gt;&gt; print expr x + 1 # a symbolic expression

&gt;&gt;&gt; x=2 # reassignment of x &gt;&gt;&gt; print expr x + 1 # changing x did not change expr

&gt;&gt;&gt; print x

<ul>

 <li># did change x though</li>

</ul>

&gt;&gt;&gt; expr=x+1

&gt;&gt;&gt; print expr

<ul>

 <li># now expr used current value of x – no longer a symbol</li>

</ul>

This can lead to confusion, so be careful.

Rational functions of <em>s </em>are straightforward in sympy

&gt;&gt;&gt; s=symbols(’s R_2 C_2’)

&gt;&gt;&gt; h=-1/(1+s*R2*C2)

&gt;&gt;&gt; print h

-1/(C_2*R_2*s+1)

We will require to create some matrices. For example

&gt;&gt;&gt; M=Matrix([[1 2],[3 4]])

&gt;&gt;&gt; print M

creates a 2×2 matrix containing

If you define a matrix and a column vector you can multiply them as in Matlab using

“*”

&gt;&gt;&gt; N=Matrix([5,6])

&gt;&gt;&gt; M*N

Matrix([

[17],

[39]])

Suppose you want to solve <em>Mx </em>= <em>N</em>. Then we need to use the inverse of <em>M</em>

&gt;&gt;&gt; M.inv()*N

Matrix([

[-4],

[9/2]])

Finally, to link to the Python we already know, we need to be able to generate numbers out of symbols. For that we use <em>evalf</em>.

&gt;&gt;&gt; expr=sqrt(8)

&gt;&gt;&gt; print expr

2*sqrt(2)

&gt;&gt;&gt; expr.evalf() 2.82842712474619

This works for single numbers. But what if we want to plot the magnitude response of a laplace transform expression? We use something called lambdify

&gt;&gt;&gt; h=1/(s**3+2*s**2+2*s+1)

&gt;&gt;&gt; import pylab as p

&gt;&gt;&gt; w=p.logspace(-1,1,21)

&gt;&gt;&gt; ss=1j*w

&gt;&gt;&gt; f=lambdify(x,h,”numpy”)

&gt;&gt;&gt; p.loglog(w,abs(f(ss)))

&gt;&gt;&gt; p.grid(True)

&gt;&gt;&gt; p.show()

What lambdify does is it takes the sympy function “h” and converts it into a python function which is then applied to the array ’ss’. This is much faster than iterating over the elements of ss and using evalf on the elements.

<h1>3          Using Sympy to solve our circuit problem</h1>

We can solve directly for the exact result from Python. Let us define <em>s </em>= <em>jω</em>, and rewrite the equations in a matrix equation

<ul>

 <li>0 1                          0              </li>

 <li>0 0</li>

</ul>

 <em>V</em><em>p </em>=              0         

−

<em>G     G                                   </em>             0         

0     <em>sC                                      V<sub>i</sub></em>(<em>s</em>)<em>/</em><em>R</em><sub>1</sub>

This is the equation that we create and solve for now. The following function both defines the matrices and solves for the solution.

from sympy import * import pylab as p def lowpass(R1,R2,C1,C2,G,Vi):

s=symbols(’s’)

A=Matrix([[0,0,1,-1/G],[-1/(1+s*R2*C2),1,0,0], 

[0,-G,G,1],[-1/R1-1/R2-s*C1,1/R2,0,s*C1]]) b=Matrix([0,0,0,Vi/R1])

That defines the coefficient matrices. Note that I did not include <em>V<sub>i</sub></em>(<em>s</em>) in the vector b. I multiply it in later. However, I could have included it here as well. Now we solve for the solution.

V=A.inv()*b

This solution is in <em>s </em>space.

Now extract the output voltage. Note that it is the fourth element of the vector. The logic of this line is explained below the code.

return (A,b,V)

We now use this function to generate the plot for the values mentioned above. Note that the input voltage is a unit step function.

A,b,V=lowpass(10000,10000,1e-9,1e-9,1.586,1) print ’G=1000’ Vo=V[3] print Vo w=p.logspace(0,8,801) ss=1j*w hf=lambdify(s,Vo,’numpy’) v=hf(ss)

p.loglog(w,abs(v),lw=2)

p.grid(True)

p.show()

<h1>4          The Assignment</h1>

<ol>

 <li>Obtain the step response of the circuit above. Use the signal toolbox of last week’sassignment.</li>

 <li>The input is</li>

</ol>

<em>v<sub>i</sub></em> Volts

Determine the output voltage <em>v</em><sub>0</sub>(<em>t</em>)

Consider the following circuit (from page 274 of Horowitz and Hill)

The values you can use are <em>R</em><sub>1 </sub>= <em>R</em><sub>3 </sub>= 10<em>k</em>Ω, <em>C</em><sub>1 </sub>=<em>C</em><sub>2 </sub>= 1nF, and <em>G </em>= 1<em>.</em>586.

<ol start="3">

 <li>Analyse the circuit using Sympy as explained above, i.e., create a function similar tothe one defined above. For the same choice of components and gain, this circuit is a highpass filter.</li>

 <li>Obtain the response of the circuit to a damped sinusoid (i.e., use suitable <em>V<sub>i</sub></em>). Use signal.lsim to simulate the output.</li>

 <li>Obtain the response of the circuit to a unit step function. Do you understand theresponse? (Just define Vi to be 1/s)</li>

</ol>