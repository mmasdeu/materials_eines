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

# Àlgebra Lineal


## 1. Llistes i tuples

```sage
L=[1,2,3,4,5]
print(L[-1])
L[-1]=10
print(L)
```

```sage
T=(1,2,3,4,5)
print(T[-1])
T[-1]=10
```

```sage
T=([1,2,3],2,'a')
```

```sage
T[0][0]=7
```

```sage
print(T)
```

```sage
LL = L
```

```sage
LL[1]=1000
```

```sage
L
```

```sage
LL=copy(L)
LL[1]=0
print(L,LL)
```

```sage
(1,2,3)+(2,3,4)
```

```sage
3*(1,2,3)
```

```sage
T=(123)
print(T)
print(T[0])
```

```sage
T=(123,)
print(T)
print(len(T))
print(T[0])
```

## 2. Vectors


La instrucció per fabricar un objecte que tingui les característiques d'un vector és  vector(). L'argument d'aquesta funció és una llista o una tupla amb les components del vector. Els vectors es tracten, en principi, com files, però si cal interpretar les seves components com una columna no caldrà fer cap transformació ja que les funcions que tracten els vectors ja estan preparades per fer-ho, com veurem més endavant.

```sage
v=vector([1,-3,0,-4])
u=vector((3/4,-1,1,0))
show(v)
show(u)
```

<font color=black> Per tal d'accedir a cada una de les components d'un vector, es pot fer com si fos una llista (recordeu que la numeració de les posicions comença per $0$). Per tant, podem fer

```sage
print(v[0])
print(v[2])
print(v[-1])
```

però no funcionarà

```sage
print(v[4])
```

<font color=black> De la mateixa manera que es pot conèixer el valor de cada una de les components d'un vector també és possible modificar-les d'una en una.

```sage
v[1]=7
show(v)
```

<font color=black> Per tal de conèixer la mida d'un vector es pot utilitzar degree() que actua com una propietat del vector, o bé len() que és una funció aplicada als vectors.

```sage
print(u.degree())
print(len(v))
```

<font color=black> Noteu que no es pot posar degree(u) ni u.len().


<font color=black> Els vectors viuen en espais vectorials. En el nostre cas viuran en el espai $\mathbb{Q}^d$, on $d$ és la mida del vector, i enlloc de $\mathbb{Q}$ pot ser el cos on estiguin definits.

```sage
u.parent()
```

```sage
Q4=VectorSpace(QQ,4)
u in Q4 
```

<font color=black> Compte, però, que normalment el `SageMath` assigna el vector al espai més petit on pot fer-ho. Així el vector `v` que té coefficients enters de fet es pensa que viu en "el espai vectorial sobre $\mathbb{Z}$ de dimensió $d$ (que de fet no és un espai vectorial, sinò un mòdul).  

```sage
v.parent()
```

<font color=black> Tot i així si li preguntem si està a Q4 diu que si, doncs $\mathbb{Z}^4\subset \mathbb{Q}^4$.

```sage
v in Q4
```

<font color=black> El que podem fer és forçar que estigui al espai que volem definint-lo allà.

```sage
v=Q4(v)
v.parent()
```

<font color=black> De fet aquesta és també una bona manera de definir vectors

```sage
w=Q4([1,-5,6,2])
show(w)
w.parent()
```

<font color=red> Molt important! </font> <font color=blue> Abans de passar a les operacions en les que intervenen vectors, cal fer notar que els  vectors i matrius  es comporten com les llistes; són mutables, i per tant modificables.  De forma que, si generem una variable nova *igualant-la* amb una que ja conté un vector, l'únic que obtindrem seran dos noms diferents per accedir al mateix contingut i qualsevol canvi que es faci a aquest contingut a través d'un d'aquests noms es reflectirà simultàniament a través de l'altre. 

</font> <font color=black>  Per posar un exemple, suposeu que comencem amb un vector de components $(1,2,3,4)$

```sage
v=vector([1,2,3,4]); show(v)
vc=v; show(vc)
```

<font color=black> Si ara modifiqueu el vector `vc`, com que "apunta" al mateix lloc de memòria, el vector `v` també quedarà modificat. 

```sage
vc[2]=0; show(vc)
show(v)
```

<font color=black> Si el que voleu es tenir una copia del vector `v` per tal de modificar-lo preservant el valor de `v` poeu utilitzar la instrucció `copy`

```sage
vc=copy(v)
show(vc);show(v)
vc[2]=-1
show(vc);show(v)
v[0]=-2
show(vc);show(v)
```

<font color=black> Comproveu amb exemples que aquest comportament no succeeix amb variables que contenen constants o expressions simbòliques però que també passa el mateix quan es tracta de llistes.


### Operacions amb vectors


<font color=black> Els vectors es poden operar de les següents maneres: es poden multiplicar per nombres amb $*$, es poden sumar si tenen la mateixa mida amb $+$ i es poden multiplicar també si tenen la mateixa mida amb $*$, obtenint el producte escalar (que tambe és pot fer amb `dot_product`)

```sage
3*u
```

```sage
v/2
```

```sage
0.5*v
```

```sage
u+v
```

```sage
u*v
```

```sage
u.dot_product(v)
```

Naturalment, es produirà un error si s'intenta realitzar una operació no permesa.

```sage
u+5
```

```sage
 u+vector([1,2])
```

```sage
 u*vector([1,2])
```

Ara bé, tingueu en compte que 0 s'interpreta com el vector zero en l'espai on toqui i sí permet operar amb ell

```sage
print(u+0)
print(u-u==0)
```

<font color=black> Noteu que si multipliquem un vector sobre el racionals per un nombre que no sigui (explícitament) racional. el vector canvia de lloc on viu. 

```sage
(0.5*v).parent()
```

<font color=black> El comportament de l'operador `+` és diferent si la suma es realitza entre vectors o llistes: quan la suma és de vectors el resultat té com a components les sumes de les components corresponents de cada un dels factors, mentre que la suma de dos objectes de tipus llista consisteix en la concatenació dels elements respectius.

```sage
print(vector([1,2,3])+vector([-2,-1,0]))
print([1,2,3]+[-2,-1,0])
```

## 3. Matrius 


<font color=black> La funció `matrix` és la que permet crear objectes de tipus matricial. En la versió més curta utilitza com argument una llista amb els continguts de les files (els elements de la llista són llistes de la mateixa longitud amb els elements de cada fila) i genera l'objecte de tipus matricial corresponent.

```sage
A=matrix([[1,2],[3,4],[5,6],[7,8]]) 
show(A)
```

<font color=black> També es pot generar un matriu a partir d'una llista de vectors o de tuples de la mateixa longitud (que seran les files de la matriu resultant).

```sage
u=vector([1,2,3]);v=vector([3,4,5])
M=matrix([u,v])
show(M)
```

<font color=black> Alternativament, es pot especificar el nombre de files i columnes i donar les dades com una llista individual (ordenant, és clar, per files).

```sage
M0=matrix(2,4,[1,2,3,4,5,6,7,8]); show(M0)
M10=matrix(10,10,range(100)); show(M10)
```

<font color=black> Si mireu la informació que proporciona la instrucció `matrix?` encara descobrireu més variants per generar matrius amb aquesta instrucció.


```sage
matrix?
```

<font color=black> D'altre banda, hi ha funcions que construeixen matrius especials, com per exemple, la matriu identitat de la mida que es desitgi, la matriu zero, les matrius diagonals, etc...

```sage
show(identity_matrix(4))
dd=diagonal_matrix([1,-1,2,3]);show(dd)
```

<font color=black>  Proveu de fer la matriu zero de mida 4x5.


<font color=black>  També podem crear matrius a partir de les files i columnes d'una altra (submatriu).

```sage
Br=M10.matrix_from_rows([1,3,4])
Bc=M10.matrix_from_columns([0,2])
show(Br)
show(Bc)
Brc=M10.matrix_from_rows_and_columns([0,2],[1,2,4])
show(Brc)
```

### Accedint als continguts d'una matriu


<font color=black>  Si es vol accedir al valor d'una de les posicions de la matriu n'hi haurà prou expressant l'índex corresponent a la fila i la columna (amb aquest ordre).

```sage
print(A[0,0])
print(A[3,1])
```

<font color=black>  Tot i que es pot extreure una fila qualsevol d'una matriu indicant un sol índex, hi ha funcions específiques per extreure files i columnes (noteu però que el resultat és un objecte de tipus vector).

```sage
show(A[2])
show(A.row(2))
show(A.column(1))
```

<font color=black>  Si es necessita saber les mides d'una matriu (si es vol comprovar si és quadrada, per exemple) es pot utilitzar nrows i ncols.

```sage
A.nrows()
```

```sage
A.ncols()
```

<font color=black>  Encara que, com que hi ha una bateria molt extensa de tests associats a una matriu, entre els quals hi ha la comprovació del fet que sigui quadrada, aquesta comprovació es podria fer directament amb

```sage
show(A.is_square())
show(M10.is_square())
```

<font color=black>  També podem canviar les files per les columnes (transposar la matriu) amb la instrucció transpose

```sage
show(A.transpose())
show(transpose(A))
```

<font color=black>  Com és d'esperar, les matrius es poden multiplicar per escalars i sumar entre elles sempre que tinguin la mateixa mida. Les operacions es fan component a component.

```sage
B1=matrix(3,2,range(6))
B2=matrix(3,2,[3,3,3,1,1,1])
B3=B1+5*B2
show(B3)
```

<font color=black>  Quan multipliquem una matriu per un vector, SageMath interpreta convenientment el vector com a fila o columna, segons si es vol fer la multiplicació per la dreta o per l'esquerra (sempre que el producte que plantegem tingui sentit).

```sage
u=vector([1,2,3]);show(u)
w=vector([-1,1]);show(w)
A=matrix([[1,1],[-1,2],[2,1]]);show(A)
```

```sage
A*u
```

```sage
u*A
```

```sage
A*w
```

<font color=black> Naturalment, l'operador * realitza el producte de matrius sempre que estigui definit.

```sage
A=matrix([[1,2,3],[4,5,6]])
B=matrix([[1,2],[3,4],[5,6]])
show(A,B)
show(A*B)
show(B*A)
```

```sage
C=matrix([[1,2,3],[4,5,6],[7,8,9]]);show(C)
show(A*C)
show(C*A)
```

<font color=black> Les multiplicacions successives d'una matriu quadrada per si mateixa (potències) es poden obtenir amb l'operador de potències ordinari $\ ^\wedge$ i, en particular, la inversa (si existeix) es pot obtenir amb l'exponent $-1$.

```sage
A=matrix([[1,1,0],[0,1,0],[0,0,-1]]);show(A)
show(A^15)
show(A^(-1))
```

```sage
B=matrix([[1,1,1],[-1,2,1],[0,3,2]]);show(B)
show(B^3)
show(B^(-1))
```

<font color=black>  Fins i tot podem calcular potències simbòlicament!

```sage
var('n')
show(A^n)
```

```sage
show(B^n)
```

<font color=black>  Per calcular la traça i el determinant, apliquem les funcions (o mètodes) corresponents.

```sage
A.det()
```

```sage
det(A)
```

```sage
A.trace()
```

<font color=black> La matriu ha de ser quadrada. 

```sage
Mc=matrix(2,3,[1,2,3,4,5,6])
Mc.det()
```

```sage
Mc.trace()
```

<font color=black>  La instrucció que calcula el rang de les matrius és rank.

```sage
B=matrix([[1,2,3],[2,1,-1],[1,-1,-4],[3,3,2]]);show(B)
B.rank()
```

```sage
rank(B)
```

<font color=black>  Naturalment, quan el problema del càlcul del rang està posat en una família de matrius depenent d'un, o més, paràmetres, la funció rank no és capaç de distingir dins la família quins són els casos especials.   Per exemple, si es considera la família de matrius depenent del paràmetre $k$ donada per
$$
A_k=\begin{pmatrix} 
 1 & k + 1 & -1 & 0 \\
-1 & 2 & k & 1 \\
k + 2 & -1 & -1 & 2 \, k - 1
\end{pmatrix}
$$
el valor de la funció rank aplicada a l'expressió genèrica de les matrius de la família donarà $3$. 

```sage
var('k')
Ak=matrix([[1,k+1,-1,0],[-1,2,k,1],[k+2,-1,-1,2*k-1]]);show(Ak)
Ak.rank()
```

<font color=black>  De fet, SageMath treballa en un context (utilitza el cos de coeficients $\mathbb{Q}(k)$) on la matriu en efecte té rang $3$ (executeu Ak.parent()). 
Tot i això, com a mínim en el cas $k=0$, la matriu només té rang $2$ (es pot veure clarament que l'última fila és la diferència entre les altres dues).

```sage
Ak.parent()
```

```sage
A0=Ak.subs(k=0);show(A0)
rank(A0)
```

<font color=black>  Fet que es pot preveure si es calcula, per exemple, el determinant de la submatriu formada per les tres primeres columnes i mirant quant val zero (surt un polinomi de grau 3 en $k$). 

```sage
AA=Ak.matrix_from_columns([0,1,2])
show(AA)
d=AA.det().expand()
show(d)
sols=d.roots(ring=QQ)
show(sols)
```

També podriem haver fet esglaonament (com farem a la propera secció.)


## Sistemes d'equacions lineals i esglaonament.


<font color=black>  Encara que la instrucció solve permet plantejar i resoldre sistemes d'equacions lineals, hi ha situacions en les que és convenient poder conèixer, de forma explícita, el procés de reducció del sistema que porta a l'expressió de les solucions. 

Com ja deveu saber, la tècnica més pràctica per tal de solucionar un sistema d'equacions lineals de la forma $A\cdot X= B$ consisteix a transformar la matriu ampliada dels sistema $(A\mid B)$ en la seva forma reduïda (Gauss-Jordan), ja que en el procés es manté el conjunt de solucions i en aquesta forma reduïda es poden llegir directament les solucions (les incògnites corresponents als pivots són les incògnites lligades i les solucions s'obtenen expressant aquestes incògnites en funció de les restants, les incògnites lliures, que poden prendre qualsevol valor). 

A continuació podeu veure, amb un parell d'exemples, quines funcions de SageMath es poden utilitzar per tal de resoldre un sistema seguint aquest procés.


<font color=black>  Considerem
    $$
    A= \begin{pmatrix}
    2 & -4 & 0 & -1 & -5 \\
0 & 0 & 2 & 1 & 5 \\
-1 & 2 & 3 & 2 & 10
    \end{pmatrix},\qquad B= \begin{pmatrix}
    2 \\
0 \\
-1
    \end{pmatrix}
    $$
    Calcularem les solucions del sitema d'equacions $A\cdot X=B$ (és dir, determinarem quina forma tenen les columnes $X=\begin{pmatrix} x_0\\x_1\\x_2\\x_3\\x_4 \end{pmatrix}$ que compleixen l'equació anterior) reduint la matriu ampliada a la seva forma de Gauss-Jordan.

```sage
A=matrix(QQ,3,5,[2,-4,0,-1,-5,0,0,2,1,5,-1,2,3,2,10]);show(A)
B=vector([2,0,-1]);show(B)
Am=A.augment(B,subdivide=True);show(Am)

```

<font color=black> Encara que $B$ sigui un vector, i per tant és una fila, la funció augment, que fabrica la matriu augmentada del sistema, sap decidir que l'ha de posar com una columna.


<font color=black> Calculem ara la forma esglaonada del sistema.

```sage
Ar=Am.echelon_form();show(Ar)
```

<font color=black> A partir d'aquesta expressió tindrem els pivots (incògnites lligades) i les incògnites lliures de forma immediata (o a partir del resultat de les funcions pivots i nonpivots) i les equacions (files) en les que una de les incògnites lligades s'obté com a funció de les lliures i del terme independent corresponent (funció pivot\_rows).

```sage
pvts=Ar.pivots();show(pvts)
npvts=Ar.nonpivots();show(npvts)
flspv=Ar.pivot_rows();show(flspv)
```

<font color=black> I amb aquesta informació us hauria de quedar clar que les solucions són de la forma
$$
\begin{aligned}
x_0 &= 2\, x_1+\dfrac12\, x_3+\dfrac52\, x_4+1
\\
x_2 &= -\dfrac12\, x_3-\dfrac52\, x_4 
\end{aligned}
$$
amb els valors de $x_1$, $x_3$ i $x_4$ arbitraris.



<font color=black>  Quan interessa anar controlant quines operacions de reducció es fan, el mètode anterior no serveix, ja que només dóna el resultat final. En aquests casos caldrà anar realitzant les operacions de forma manual, cosa que es pot aconseguir utilitzant:
.swap\_rows()
.rescale\_row()
.add\_multiple\_of\_row()
(el que fa cada una d'aquestes funcions resulta obvi a partir del seu nom).

```sage

```

<font color=red> Exercici: </font> Feu la reducció pas a pas de la matriu Ar

```sage

```

<font color=black> Segurament sabeu que, en aquest context, cada cop que es realitza un procés de reducció sobre una matriu Am i s'arriba a un cert resultat Ar, hi ha una mariu invertible P que codifica totes les operacions que s'han fet en els sentit que es té la igualtat
$$
P\cdot Am=Ar
$$
Si es vol saber quina és aquesta matriu P només cal utilitzar 
extended\_echelon\_form() i extreure del resultat que dóna les parts corresponents.

```sage
GJP=Am.extended_echelon_form(subdivide=True); show(GJP)
GJ=GJP.matrix_from_columns([0,1,2,3,4,5])
P=GJP.matrix_from_columns([6,7,8])
show(GJ)
show(P)
```

<font color=black> Que passa si fem la forma esglaonada per una matriu amb paràmetres? Veiem-ho

```sage
var('k')
Ak=matrix([[1,k+1,-1,0],[-1,2,k,1],[k+2,-1,-1,2*k-1]]);show(Ak)
AkE=Ak.echelon_form()
show(AkE)
```

<font color=black> No podem simplificar matrius fàcilment. 

```sage
show(AkE.full_simplify())
```

<font color=black> Podem simplificar els elements un a un de la columna 3.

```sage
show([a.full_simplify() for a in AkE.column(3)])
```

<font color=red> Compte!. Al fer-ho així no veiem que per a $k=0$ el rang és 2. Això és perquè en algun moment del càlcul el Sage ha dividit per $k$ tota una fila o columna, ja que al anell on treballa els polinomis són invertibles!!


El que podem fer és treballar directament en l'anell de polinomis en una variable. Aleshores 'SageMath' no dividirà per cap polinomi de grau $>0$, ja que aquests no tenen inversos als polinomis. Si posem

```sage
R.<k> = PolynomialRing(QQ)
Ak=matrix([[1,k+1,-1,0],[-1,2,k,1],[k+2,-1,-1,2*k-1]]);show(Ak)
AkE=Ak.echelon_form()
show(AkE)
```

<font color=black> Ara és evident que només quan els dos polinomis de la darrera fila són zero el rank és 2, i això només passa si $k=0$. Per exemple podeu calcular el màxim comú divisor dels dos polinomis (ja que un zero comú serà un zero del mcm), i surt

```sage
gcd([a for a in AkE.row(2)])
```

## 5. Subespais vectorials, suma i intersecció


<font color=black> Recordeu que per a construir un espai vectorial del tipus $V=K^n$ per un cos $K$ i un enter $n\ge 1$ tenim la instrucció VectorSpace(K,n) o bé $K^n$.


```sage
E=QQ^4
```

<font color=black> Per treballar de forma genèrica amb una gran varietat d'expressions simbòliques, SageMath usa el que anomea l'anell simbòlic (symbolic ring), que es denota per SR, i que de fet és un cos. Tot i que no entrarem en més detalls, també podem treballar amb cossos més complicats, com per exemple el cos de funcions racionals amb coeficients a $\mathbb{Q}$, $\mathbb{Q}(t)=\{\frac{p(t)}{q(t)}\mid q(t)\neq 0\}$:

```sage
E1=VectorSpace(FractionField(PolynomialRing(QQ,'x')),3)
E1
```

```sage
u=E([5,-2,1,3])
v=E([1,1,2,-1])
```

```sage
w=3*u-6*v
```

```sage
F=E.subspace([u,v,w])
show(F)
```

```sage
F
```

```sage
span([u,v,w])
```

<font color=black> Observem que al construir subespais també se'ls assigna automàticament una base. Aquesta s'obté de forma canònica esglaonant (Gauss-Jordan) el conjunt de vectors generadors. 

Si no es vol treballar amb aquesta base, es pot especificar la base per al subespai amb

```sage
F1=E.subspace_with_basis([u,v])
```

```sage
F1
```

```sage
F1.0
```

<font color=black> Notem però que els dos espais, tot i tenir assignades bases diferents, són el mateix:


```sage
F==F1
```

<font color=black> Encara que un espai vectorial tingui una base assignada diferent, sempre podem accedir a la que és la base canònica per a SageMath amb

```sage
F1.echelonized_basis()
```

<font color=black> Vegem ara com construir un espai vectorial a partir d'equacions que satisfan les seves coordenades. Com sabeu, aquestes equacions han de correspondre a un sistema d'equacions lineals homogeni i, per tant, s'han de poder representar matricialment com $A\cdot X = 0$. Per tant, el problema inicial serà crear la matriu del sistema. En aquest cas, serà important el cos dels coeficients que assignem. 

Per exemple, si volem construir el subespai vectorial  $W$ dels vectors 
$(x,y,z,t)\in\mathbb{Q}^4$ tals que $x+y=0$ i $3\,x-y-z-t=0$ es pot fer el següent:

```sage
A=matrix(QQ, [[1,1,0,0],[3,-1,-1,-1]])
W=A.right_kernel()
W
```

<font color=black> Ja que tenim una matriu entrada, aprofitem per veure dues instruccions que ens permeten obtenir els subespais vectorials generats per les files o per les columnes de la matriu:

```sage
A.row_space()
```

```sage
A.column_space()
```

<font color=black> Com és d'esperar, els valors següents coincideixen:

```sage
A.row_space().dimension()
```

```sage
A.column_space().dimension()
```

```sage
A.rank()
```

## Suma i intersecció de subespais


<font color=black> La suma d'espais vectorials es pot crear de forma natural sumant-los amb l'operador +, sempre i quan l'expressió tingui sentit, és a dir, que siguin subespais vectorials d'un espai vectorial en comú.


```sage
F+W
```

```sage
V=(QQ^3).subspace([(1,1,-1)])
```

```sage
F+V    #Ha de donar error
```

<font color=black> Podem fer sumes de més d'un espai

```sage
 span([u])+span([v])+span([w])
```

<font color=black> La intersecció d'espais vectorials es crea com a una propietat d'un dels espais, però òbviament l'ordre que s'utilitzi serà irrellevant per al resultat.

```sage
F.intersection(W)
```

```sage
W.intersection(F)
```

<font color=black> Per posar un exemple on apareixen aquestes construccions, calculeu la dimensió i bases pel subespais de $\mathbb{Q}^4$, $U$, $V$, $U+V$ i $U\cap W$, on 
\begin{gather*}
U=\langle (1,-1,1,-1),(1,2,1,1),(2,-1,2,0),(4,0,4,2)\rangle,\text{ i}\\
V=\{(x,y,z,t)\in \mathbb{Q}^4 \mid 3\,x-2\,y-z-t=x-y-t=2\,x-y-z=0\}.
\end{gather*}



```sage
U=span([(1,-1,1,-1),(1,2,1,1),(2,-1,2,0),(4,0,4,2)],QQ)
```

```sage
B=matrix(QQ,[[3,-2,-1,-1],[1,-1,0,-1],[2,-1,-1,0]])
```

```sage
V=B.right_kernel()
```

```sage
show(U.basis())
U.dimension()
```

```sage
show(V.basis())
V.dimension()

```

```sage
show((U+V).basis())
(U+V).dimension()
```

```sage
show(U.intersection(V).basis())
V.intersection(V).dimension()
```

```sage

```
