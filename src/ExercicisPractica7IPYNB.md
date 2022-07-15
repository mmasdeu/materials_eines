---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.14.0
  kernelspec:
    display_name: SageMath 9.2
    language: sage
    name: sagemath
---

# Exercicis Pràctica 7


<font color=red> Exercici 1 </font> Considereu les matrius $$P= \begin{pmatrix}
\dfrac {4}{7} & \dfrac {1}{7} &  - \dfrac {1}{7} \\[10pt]
 - \dfrac {5}{21} & \dfrac {4}{21} & \dfrac {1}{7} \\[10pt]
 - \dfrac {3}{7} & \dfrac {1}{7} &  - \dfrac {1}{7}
\end{pmatrix} \text {i } {M} =  \begin{pmatrix}
1 & 0 & -1 \\[5pt]
2 & 3 & 1 \\[5pt]
-1 & 3 &  - 3
\end{pmatrix}$$ i el vector ${v_{1}} =  (1,\ - \dfrac {11}{3},\ - 2)$ i el vector ${v_{2}} = 
(1,\ -1,\ 2 - 2\,\sqrt{2})$. Calculeu:


```sage
P=matrix(QQ,[[4/7,1/7,-1/7],[-5/21,4/21,1/7],[-3/7,1/7,-1/7]])
show(P)
```

```sage
M=matrix(QQ,[[1,0,-1],[2,3,1],[-1,3,-3]])
show(M)
```

```sage
v1=vector([1,-11/3,-2])
v1
```

```sage
v2=vector([1,-1,2-2*sqrt(2)])
v2
```

$M\cdot P$

```sage
M*P
```

$M\cdot v_{1}$, $M\cdot v_{2}$, $P\cdot v_{1}$ i $P\cdot v_{2}$

```sage
M*v1
```

```sage
M*v2
```

```sage
P*v1
```

```sage
P*v2
```

$v_{1}\cdot M$ i $v_{2}\cdot M$

```sage
v1*M
```

```sage
v2*M
```

<font color=red> Exercici 2: </font>  Considereu el sistema d'equacions lineals
$$
\begin{aligned}
2\, x_{1}-4\, x_{2}+3\, x_{3}-5\, x_{4}+x_{5} &= 3
\\
x_{1}-4\, x_{2}-2\, x_{3}-7\, x_{5} &= 2
\\
5\, x_{1} -3\, x_{3} -2\, x_{4}+2\, x_{5} &= 1
\\
x_{2}-4\, x_{3} +3\, x_{4}-x_{5} &= 3/2
\end{aligned}
$$
Genereu la seva matriu ampliada i realitzeu en aquesta matriu les operacions de reducció necessàries per tal de determinar si és compatible o no i conèixer les solucions si en té.

```sage
A=matrix(QQ,[[1,-4,3,-5,1],[1,-4,-2,0,-7],[5,0,-3,-2,2],[0,2,-4,3,-1]])
A
```

```sage
B=vector([3,2,1,3/2])
B
```

```sage
Am=A.augment(B,subdivide=True)
show(Am)
```

```sage
AmE=Am.echelon_form()
AmE
```

```sage
show('Es compatible? ', rank(A) == rank(Am))
```

```sage
AmE.pivots()
```

```sage
show(' Les solucions son ')
for i in AmE.pivots():
    show('x',i,' = ', -AmE[i,4],'x4 + (', AmE[i,5],')')
```

<font color=red> Exercici 3. </font> Estudieu, per als diferents valors dels paràmetres $b$ i $t$, el sistema d'equacions lineals (amb incògnites $x,\ y,\ z$)
$$
\begin{aligned}
3\, x+2\, y +z &= t
\\
x-y+2\, z &= 1+t^{2}
\\
3\, x +7\, y -4\, z &= -1-t-t^{2}-t^{3}
\\
2\, x +y +b\, z &= t^{3}
\end{aligned}
$$


```sage
K.<b,t>=PolynomialRing(QQ)
```

```sage
A=matrix(K,[[3,2,1],[1,-1,2],[3,7,-4],[2,1,b]])
A.echelon_form()
```

Podem veure que si $b\ne 1$ el rank de $A$ serà 3. Per tant el sistema serà compatible determinat o incompatible.

```sage
B=vector(K,[t,1+t^2,-1-t-t^2-t^3,t^3])
```

```sage
Am=A.augment(B)
```

```sage
AmE=Am.echelon_form()
show(AmE)
```

Només es compatible si el terme $AmE[2,3]$ és zero, i, si $b=1$, si a més $AmE[3,3]=0$. Per trobar els zeros substituïm $t$ per una variable $x$.

```sage
rootsAmE23=AmE[2,3].subs(t=x).roots(ring=RR)
rootsAmE23
```

Per tant cal que el valor de la t sigui 1 (si volguéssím solucions complexes també n'hi hauria dues més). 

```sage
AmE[3,3].subs(t=x).roots(ring=RR)
```

Veiem que si $b=1$ només té solució si $t=1$ també. 


També podriem haver trobat el mcd dels dos polinomis per a trobar les arrels en comú. 

```sage
AmE[2,3].gcd(AmE[3,3])
```

Veiem quines solucions hi ha quan t=1

```sage
AmE.subs(t=1).echelon_form()
```

Les solucions si $t=1$ i $b\ne 0$ són $x=1$,$y=-1$ i $z=0$.


Si $t=1$ i $b=1$, les solucions són $$x=1-z \text{ i } y=-1+z $$

```sage
AmE11 = AmE.subs(t=1).subs(b=1).echelon_form()
```

```sage
var('x,y,z')
vxs = [x,y,z]
show(' Les solucions son ')
for i in AmE11.pivots():
    show(vxs[i],' = ', -AmE11[i,2]*vxs[2] + AmE11[i,3])
```

<font color=red> Exercici 4: </font> Determineu el rang de la matriu següent
$$
\left( \begin{array}{ccccc}
4-x & 2 & -2+({2}/{3})x & 8-x & -4\\
13/2 & -1 & x-3 & 10 & -{9}/{2} \\
4x-4 & 6 & 2-({4}/{3})x & 3x-8 & 4 \\
6x-4 & 3 & 2-2x & 3x-8 & 4+x
\end{array}\right) 
$$
en funció del paràmetre $x$. 

```sage
reset()
K.<x>=PolynomialRing(QQ)
A=matrix(K,[[4-x , 2 , -2+(2/3)*x , 8-x , -4],[
13/2 , -1 , x-3 , 10 , -9/2 ],[
4*x-4 , 6 , 2-(4/3)*x , 3*x-8 , 4 ],[
6*x-4 , 3 , 2-2*x , 3*x-8 , 4+x]])
show(A)
```

```sage
AE=A.echelon_form()
show(AE)
```

Si la última fila és zero, el rang és 3. Si un dels dos coefficients és no zero, el rang és 4. Per tal que els dos coeficients siguin 0, ho ha de ser el seu mcm

```sage
gcd(AE.row(3))
```

Per tant el rank és 3 només quan x=0. 

```sage
show(AE.subs(x=0).echelon_form())
```

<font color=red> Exercici 5: </font> Doneu una forma reduïda (per files) $F$ de la matriu $A =  \left(
{\begin{array}{rcc}
1 & 3 & 0 \\
2 & -1 &  - 5 \\
0 & 1 & 3
\end{array}}
 \right) $ i una matriu invertible $P$ tal que $P\cdot A= F$.


```sage
reset()
A=matrix(QQ,[[1,3,0],[2,-1,-5],[0,1,3]])
```

```sage
AE=A.extended_echelon_form()
show(AE)
```

```sage
P=AE.matrix_from_columns([3,4,5])
show(P)
```

```sage
P*A
```

<font color=red> Exercici 6: </font> Feu una funció de sage PAreduccio(A) de manera que, donada una matriu A qualsevol, retornin 2 matrius J i P, on J és la forma esglaonada reduïda i P invertible tal que $PA=J$. Proveu-ho amb les matrius dels problemes (1), (4) i (5).


```sage
def PAreduccio(A):
    c=A.ncols()
    r=A.nrows()
    AE=A.extended_echelon_form()
    J=AE.matrix_from_columns([0..c-1])
    P=AE.matrix_from_columns([c..c+r-1])
    return J,P
```

Matrius del problema 1

```sage
A = matrix(QQ,[[4/7,1/7,-1/7],[-5/21,4/21,1/7],[-3/7,1/7,-1/7]])
```

```sage
PAreduccio(A)
```

```sage
A = matrix(QQ,[[1,0,-1],[2,3,1],[-1,3,-3]])
```

```sage
show(PAreduccio(A))
```

Matriu del problema 4

```sage
K.<x>=PolynomialRing(QQ)
A=matrix(K,[[4-x , 2 , -2+(2/3)*x , 8-x , -4],[
13/2 , -1 , x-3 , 10 , -9/2 ],[
4*x-4 , 6 , 2-(4/3)*x , 3*x-8 , 4 ],[
6*x-4 , 3 , 2-2*x , 3*x-8 , 4+x]])
show(A)
```

```sage
I,P=PAreduccio(A)
show(I,P)
```

```sage
A0 = A.subs(x=0)
show(A0)
```

```sage
I,P=PAreduccio(A0)
show(I, " \t ", P)
```

Matriu del problema 5

```sage
A=matrix(QQ,[[1,3,0],[2,-1,-5],[0,1,3]])
```

```sage
I,P=PAreduccio(A)
show(I, " \t ", P)
```

<font color=red> Exercici 7: </font> Construïu una funció que, donada una matriu quadrada $A$, doni la dimensió i una base de l'espai donat per la intersecció de l'espai generat per les files i l'espai generat per les columnes de $A$. Proveu-ho amb les matrius quadrades dels problemes (1), (4) i (5).


```sage
def InterseccioColumnesFiles(A):
    if A.is_square():
        return (A.row_space()).intersection(A.column_space())
```

```sage
A = matrix(QQ,[[4/7,1/7,-1/7],[-5/21,4/21,1/7],[-3/7,1/7,-1/7]])
InterseccioColumnesFiles(A)
```

```sage
A = matrix(QQ,[[1,0,-1],[2,3,1],[-1,3,-3]])
InterseccioColumnesFiles(A)
```

```sage
A=matrix(QQ,[[1,3,0],[2,-1,-5],[0,1,3]])
InterseccioColumnesFiles(A)
```

He fet més exemples per a veure altres casos. 

```sage
A=matrix(QQ,[[1,3,0],[2,-1,-5]])
InterseccioColumnesFiles(A)
```

```sage
A=matrix(QQ,[[1,0,0],[0,0,0],[0,0,0]])
InterseccioColumnesFiles(A)
```

```sage
A=matrix(QQ,[[1,0,0],[0,0,1],[0,0,0]])
InterseccioColumnesFiles(A)
```

<font color=red> Exercici 8: </font> Determineu bases pels subespais de $\mathbb{Q}^5$ següents, les seves sumes i les seves interseccions.
\begin{gather*}
V_1=\langle (1,1,1,1,1),(-1,0,1,0,-1),(1,2,1,2,1),(0,2,0,2,0)\rangle \\
V_2=\langle (1,2,3,4,5),(5,4,3,2,1)\rangle\\
V_3=\{ (x,y,z,t,u)\in \mathbb{Q}^5\mid x+y=z+t=3\,u\}
\end{gather*}


```sage
V1=span([(1,1,1,1,1),(-1,0,1,0,-1),(1,2,1,2,1),(0,2,0,2,0)],QQ)
show(V1.basis())
```

```sage
V2=span([(1,2,3,4,5),(5,4,3,2,1)],QQ)
show(V2.basis())
```

```sage
B=matrix(QQ,[[1,1,0,0,-3],[0,0,1,1,-3]])
V3=B.right_kernel()
show(V3.basis())
```

```sage
show((V1+V2).basis())
show((V1+V3).basis())
show((V2+V3).basis())
```

```sage
show(V1.intersection(V2).basis())
show(V1.intersection(V3).basis())
show(V3.intersection(V2).basis())

```
