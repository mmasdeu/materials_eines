---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.14.0
  kernelspec:
    display_name: SageMath 9.6
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

```

$M\cdot v_{1}$, $M\cdot v_{2}$, $P\cdot v_{1}$ i $P\cdot v_{2}$

```sage

```

```sage

```

```sage

```

```sage

```

$v_{1}\cdot M$ i $v_{2}\cdot M$

```sage

```

```sage

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

```

```sage

```

```sage

```

```sage

```

```sage

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

```

```sage

```

```sage

```

```sage

```

```sage

```

```sage

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

```

```sage

```

```sage

```

```sage

```

```sage

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

```

```sage

```

```sage

```

<font color=red> Exercici 6: </font> Feu una funció de sage PAreduccio(A) de manera que, donada una matriu A qualsevol, retornin 2 matrius J i P, on J és la forma esglaonada reduïda i P invertible tal que $PA=J$. Proveu-ho amb les matrius dels problemes (1), (4) i (5).


```sage

```

```sage

```

<font color=red> Exercici 7: </font> Construïu una funció que, donada una matriu quadrada $A$, doni la dimensió i una base de l'espai donat per la intersecció de l'espai generat per les files i l'espai generat per les columnes de $A$. Proveu-ho amb les matrius dels problemes (1), (4) i (5).

```sage

```

```sage

```

```sage

```

```sage

```

<font color=red> Exercici 8: </font> Determineu bases pels subespais de $\mathbb{Q}^5$ següents, les seves sumes i les seves interseccions.
\begin{gather*}
V_1=\langle (1,1,1,1,1),(-1,0,1,0,-1),(1,2,1,2,1),(0,2,0,2,0)\rangle \\
V_2=\langle (1,2,3,4,5),(5,4,3,2,1)\rangle\\
V_3=\{ (x,y,z,t,u)\in \mathbb{Q}^5\mid x+y=z+t=3\,u\}
\end{gather*}


```sage

```

```sage

```

```sage

```

```sage

```

```sage

```
