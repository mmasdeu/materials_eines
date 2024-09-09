---
jupyter:
  title : 'Pràctica 8: Vectors i matrius'
  authors: [ "name" : "Marc Masdeu", "name" : "Xavier Xarles" ]
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.14.0
  kernelspec:
    display_name: SageMath 10.4
    language: sage
    name: sagemath
---


# Vectors i matrius

Per tal de fer els càlculs relacionats amb problemes d'àlgebra lineal
cal utilitzar vectors i matrius. Tot i que, en principi, podríeu pensar
que n'hi hauria prou considerant que un vector és una llista de
coeficients i una matriu una llista de vectors (columnes o files segons
convingui) **SageMath**  defineix unes classes
especials per a aquests tipus d'objectes per tal d'optimitzar les
funcions que els tractaran. Començarem, doncs, veient els mecanismes per
a definir matrius i vectors i algunes de les instruccions que permeten
manipular aquests objectes.

## Vectors

### Construcció de vectors

La instrucció per fabricar un objecte que tingui les característiques
d'un vector és `vector()`. L'argument d'aquesta funció és una llista o
una tupla amb les components del vector. Els vectors es tracten, en
principi, com files, però si cal interpretar les seves components com
una columna no caldrà fer cap transformació ja que les funcions que
tracten els vectors ja estan preparades per fer-ho, com veurem més
endavant.

```sage
v=vector([1,-3,0,-4])
u=vector((3/4,-1,1,0))
show(v)
show(u)
```

Per tal d'accedir a cada una de les components d'un vector, es pot fer
com si fos una llista (recordeu que la numeració de les posicions
comença per $0$). Per tant, podem fer

```sage
print(v[0])
print(v[2])
print(v[-1])
```

Però no funcionarà

```sage
print(v[4])
```

De la mateixa manera que es pot conèixer el valor de cada una de les
components d'un vector també és possible modificar-les d'una en una.

```sage
print(f'Abans, {v = }')
v[1] = 7
print(f'Després, {v = }')
```


Per tal de conèixer la mida d'un vector es pot utilitzar `degree` que
actua com una *propietat* del vector.

```sage
print(u.degree())
print(len(v))
```

Noteu que `degree` no es pot usar fent `degree(u)` però sí que podeu fer
`len(u)`. Noteu també que la sintaxi `u.len()` tampoc és correcta.

Els vectors viuen en espais vectorials. En el nostre cas viuran en el
espai $\mathbb{Q}^d$, on $d$ és la mida del vector, i enlloc de
$\mathbb{Q}$ pot ser el cos on estiguin definits. Recordem que el cos
$\mathbb{Q}$ es posa `QQ` en **SageMath** . Posant

```sage
u.parent()
```

veiem que ens
indica l'espai vectorial on viu el vector. Ho podem comprovar definint
l'espai corresponent

```sage
Q4 = VectorSpace(QQ, 4)
u in Q4
```

L'espai `VectorSpace(QQ, 4)` també és pot definir de manera més curta com
`QQ^4`. Compte, però, que normalment el
**SageMath**  assigna el vector a l'espai més petit
on pot fer-ho. Així el vector `v` que té coefficients enters de fet es
pensa que viu en "l'espai vectorial $\mathbb{Z}^4$ sobre $\mathbb{Z}$
de dimensió $4$" (que de fet no és un espai vectorial, sinò un mòdul).
Això ho podem comprovar posant

```sage
v.parent()
```

Tot i així si li preguntem si està a `Q4` diu que si, doncs
$\mathbb{Z}^4\subset \mathbb{Q}^4$. D'altra banda, també podem forçar
que estigui a `Q4` posant `v = Q4(v)`, que de fet ens dona una manera
alternativa de definir vectors: primer definir l'espai `V` on viuen i
després posar `V(llista)`. ``

```sage
w = Q4([1,-5,6,2])
print(w)
w.parent()
```

**Molt important:**  Abans de passar a les operacions en les que intervenen vectors, cal fer notar que els  vectors i matrius  es comporten com les llistes; són mutables, i per tant modificables.  De forma que, si generem una variable nova *igualant-la* amb una que ja conté un vector, l'únic que obtindrem seran dos noms diferents per accedir al mateix contingut i qualsevol canvi que es faci a aquest contingut a través d'un d'aquests noms es reflectirà simultàniament a través de l'altre.

Per posar un exemple, suposeu que comencem amb un vector de components $(1,2,3,4)$

```sage
v = vector([1,2,3,4])
print(f'{v = }')
vc = v
print(f'{vc = }')
```

Si ara modifiqueu el vector `vc`, com que "apunta" al mateix lloc de memòria, el vector `v` també quedarà modificat.

```sage
vc[2] = 0
print(f'{vc = }')
print(f'{v = }')
```


Si realment es vol obtenir un vector amb el mateix contingut però que es
pugui manipular de forma independent caldrà *fabricar* una còpia i
assignar una variable nova a aquesta còpia. La instrucció que es
necessita és, òbviament, `copy`:

```sage
vc = copy(v)
print(f'{vc = }, {v = }')
vc[2] = -1
print(f'{vc = }, {v = }')
v[0] = -2
print(f'{vc = }, {v = }')
```

Comproveu amb exemples que aquest comportament no succeeix amb variables que contenen constants o expressions simbòliques però que també passa el mateix quan es tracta de llistes.

```sage
```

### Suma de vectors, producte de vectors per escalars i producte escalar de vectors

Les operacions bàsiques amb vectors (suma de vectors i multiplicació
d'un vector per un escalar) es realitzen directament utilitzant els
operadors `+` o `*`. Cal tenir en compte que l'operació de multiplicació
entre vectors de la mateixa mida també està definida i calcula el seu
producte escalar (tot i que la funció `dot_product` també calcula el
producte escalar de vectors).

```sage
3 * u
```

```sage
v / 2
```

```sage
0.5 * v
```

```sage
u + v
```

```sage
u * v
```

```sage
u.dot_product(v)
```

Naturalment, es produirà un error si s'intenta realitzar una operació no
permesa.

```sage
u + 5
```

```sage
 u + vector([1,2])
```

```sage
 u * vector([1,2])
```

Ara bé, tingueu en compte que `0` s'interpreta com el vector zero i sí que es
permet operar amb ell

```sage
print(u + 0)
print(u - u == 0)
```

Si multipliquem un vector sobre el racionals per un nombre que no sigui
(explícitament) racional, el vector canvia de lloc on viu. Al posar

```sage
(0.5 * v).parent()
```

ens diu que viu a l'espai $\mathbb{R}^4$ (de fet a l'espai $\texttt{RR}^4$, on
$\texttt{RR}$ és el "cos" dels reals amb precisió simple de 53 bits).

Noteu que, en particular, el comportament de l'operador `+` és diferent
si la suma es realitza entre vectors o llistes: quan la suma és de
vectors el resultat té com a components les sumes de les components
corresponents de cada un dels factors, mentre que la suma de dos
objectes de tipus llista consisteix en la concatenació dels elements
respectius (aquí es veu un dels motius que justifiquen crear una classe
d'objectes especial per als vectors).

```sage
print(vector([1,2,3])+vector([-2,-1,0]))
print([1,2,3]+[-2,-1,0])
```

## Matrius

### Creació de matrius

La funció `matrix` és la que permet crear objectes de tipus matricial.
En la versió més curta utilitza com argument una llista amb els
continguts de les files (els elements de la llista són llistes de la
mateixa longitud amb els elements de cada fila) i genera l'objecte de
tipus matricial corresponent.

```sage
A = matrix([[1,2],[3,4],[5,6],[7,8]]) 
show(A)
```

També es pot generar un matriu a partir d'una llista de vectors o de
tuples de la mateixa longitud (que seran les files de la matriu
resultant).

```sage
u = vector([1,2,3])
v = vector([3,4,5])
M = matrix([u,v])
show(M)
```

Alternativament, es pot especificar el nombre de files i columnes i
donar les dades com una llista individual (ordenant, és clar, per files)

```sage
M0 = matrix(2,4,[1,2,3,4,5,6,7,8])
show(M0)
```
```sage
M10 = matrix(10,10,range(100))
show(M10)
```

Si mireu la informació que proporciona la instrucció `matrix?` encara
descobrireu més variants per generar matrius amb aquesta instrucció.

```sage
matrix?
```

D'altra banda, hi ha funcions que construeixen matrius especials, com
per exemple, la matriu identitat de la mida que es desitgi

```sage
show(identity_matrix(4))
```

Una matriu diagonal amb uns coeficients donats

```sage
show(diagonal_matrix([1,-1,2,3]))
```

Una matriu zero de mida $4\times 5$:

```sage
show(zero_matrix(4,5))
```


També podem crear matrius a partir de les files i columnes d'una altra
(submatriu):

```sage
Br = M10.matrix_from_rows([1,3,4])
Bc = M10.matrix_from_columns([0,2])
show(Br)
show(Bc)
```
```sage
Brc = M10.matrix_from_rows_and_columns([0,2],[1,2,4])
show(Brc)
```

```sage
Bsub = M10.submatrix(1,1,2,3) # submatriu 2x3 que comença a la posició (1,1)
show(Bsub)
```
### Accedint als continguts d'una matriu

Si es vol accedir al valor d'una de les posicions de la matriu n'hi
haurà prou expressant l'índex corresponent a la fila i la columna (en
aquest ordre), i tenint en compte que les matrius, com les llistes,
comencen amb la fila 0 i columna 0.

```sage
print(A[0,0])
print(A[3,1])
```

Tot i que es pot extreure una fila qualsevol d'una matriu indicant un
sol índex, hi ha funcions específiques per extreure files i columnes
(noteu però que el resultat és un objecte de tipus vector)

```sage
show(A[2])
show(A.row(2))
show(A.column(1))
```

Si es necessita saber les mides d'una matriu (si es vol comprovar si és
quadrada, per exemple) es pot utilitzar `nrows` i `ncols`


```sage
A.nrows()
```

```sage
A.ncols()
```

Encara que, com que hi ha una bateria molt extensa de tests associats a
una matriu, entre els quals hi ha la comprovació del fet que sigui
quadrada, aquesta comprovació es podria fer directament amb

```sage
print(A.is_square())
print(M10.is_square())
```

També podem canviar les files per les columnes (transposar la matriu)
amb la instrucció `transpose`

```sage
show(A.transpose())
```

### Suma de matrius i producte per escalars

Com és d'esperar, les matrius es poden multiplicar per escalars i sumar
entre elles sempre que tinguin la mateixa mida. Les operacions es fan
component a component.

```sage
B1 = matrix(3,2,range(6))
B2 = matrix(3,2,[3,3,3,1,1,1])
B3 = B1 + 5 * B2
show(B3)
```


### Producte de matrius per vectors

Quan multipliquem una matriu per un vector,
**SageMath**  interpreta convenientment el vector
com a fila o columna, segons si es vol fer la multiplicació per la dreta
o per l'esquerra (sempre que el producte que plantegem tingui sentit).

```sage
u = vector([1,2,3])
print(f'{u = }')
w = vector([-1,1])
print(f'{w = }')
A = matrix([[1,1],[-1,2],[2,1]])
show(A)
```

```sage
A * u
```

```sage
u * A
```

```sage
A * w
```

### Producte de matrius

Naturalment, l'operador ` * ` realitza el producte de matrius sempre que
estigui definit.

```sage
A = matrix([[1,2,3],[4,5,6]])
B = matrix([[1,2],[3,4],[5,6]])
show(A, B)
show(A*B)
show(B*A)
```

```sage
C = matrix([[1,2,3],[4,5,6],[7,8,9]])
show(C)
show(A*C)
```


### Matriu inversa i altres potències

Les multiplicacions successives d'una matriu quadrada per si mateixa
(potències) es poden obtenir amb l'operador de potències ordinari
`^` i, en particular, la inversa (si existeix) es pot obtenir amb
l'exponent $-1$.

```sage
A = matrix([[1,1,0],[0,1,0],[0,0,-1]]);show(A)
show(A^15)
show(A^(-1))
```

```sage
B = matrix([[1,1,1],[-1,2,1],[0,3,2]]);show(B)
show(B^3)
show(B^(-1))
```

Encara que, si es vol, es pot calcular la inversa d'una matriu
utilitzant la funció específica `inverse` o l'operador `~`:

```sage
show(A.inverse())
```

```sage
show(~A)
```

Fins i tot podem calcular potències simbòlicament!

```sage
var('n')
show(A^n)
```

```sage
show(B^n)
```

### Traça i determinant

En les matrius quadrades, la traça i el determinant juguen un paper molt
important. Per tal de calcular el determinant d'una matriu quadrada es
pot aplicar `det` o, si no ens fa mandra escriure més lletres,
`determinant`. Per calcular la traça, cal usar el mètode de les matrius
`trace()`.

```sage
A.det()
```

```sage
A.determinant()
```

```sage
A.trace()
```


Si la matriu no és quadrada ens dona un error

```sage
Mc = matrix(2,3,[1,2,3,4,5,6])
Mc.det()
```

```sage
Mc.trace()
```

### Rang

La instrucció que calcula el rang de les matrius és `rank`

```sage
B = matrix([[1,2,3],[2,1,-1],[1,-1,-4],[3,3,2]]);show(B)
B.rank()
```

Naturalment, quan el problema del càlcul del rang està posat en una
família de matrius depenent d'un, o més, paràmetres, la funció `rank` no
és capaç de distingir dins la família quins són els casos especials. Per
exemple, si es considera la família de matrius depenent del paràmetre
$k$ donada per
$$
A_k=\begin{pmatrix}
 1 & k + 1 & -1 & 0 \\
-1 & 2 & k & 1 \\
k + 2 & -1 & -1 & 2 \, k - 1
\end{pmatrix}
$$
el valor de la funció `rank` aplicada a l'expressió genèrica de les matrius de la família donarà $3$.

```sage
var('k')
Ak = matrix([[1,k+1,-1,0],[-1,2,k,1],[k+2,-1,-1,2*k-1]]);show(Ak)
Ak.rank()
```

De fet, **SageMath**  treballa en un context
(utilitza un *cos de coeficients* especial) on la matriu en efecte té
rang $3$:

```sage
Ak.parent()
```

Tot i això, com a mínim en el cas
$k=0$, la matriu només té rang $2$ (es pot veure clarament que l'última
fila és la diferència entre les altres dues).

```sage
A0 = Ak.subs(k=0)
show(A0)
```
```sage
A0.rank()
```

Fet que es pot preveure si es calcula, per exemple, el determinant de la
submatriu formada per les tres primeres columnes, se'n determinen les
arrels als racionals i

```sage
Asub = Ak.matrix_from_columns([0,1,2])
show(Asub)
d = Asub.det().expand()
show(d)
sols = d.roots(ring = QQ)
show(sols)
```

i queda clar que l'únic valor racional $k$ que anul·la el determinant és
$k=0$
mentre que en qualsevol altre cas el rang és el màxim possible ($3$). També podriem haver fet esglaonament (com farem a la propera secció.)


## Sistemes d'equacions lineals. Reducció de matrius

Encara que la instrucció `solve` permet plantejar i resoldre sistemes
d'equacions lineals, hi ha situacions en les que és convenient poder
conèixer, de forma explícita, el procés de reducció del sistema que
porta a l'expressió de les solucions.

Com ja deveu saber, la tècnica més pràctica per tal de solucionar un
sistema d'equacions lineals de la forma $A\cdot X= B$ consisteix a
transformar la matriu ampliada dels sistema $(A\mid B)$ en la seva forma
reduïda (Gauss-Jordan), ja que en el procés es manté el conjunt de
solucions i en aquesta forma reduïda es poden llegir directament les
solucions (les incògnites corresponents als *pivots* són les *incògnites
lligades* i les solucions s'obtenen expressant aquestes incògnites en
funció de les restants, les *incògnites lliures*, que poden prendre
qualsevol valor).

A continuació podeu veure, amb un parell d'exemples, quines funcions de
**SageMath**  es poden utilitzar per tal de
resoldre un sistema seguint aquest procés.

### Exemple 1

Considerem
$$A= \begin{pmatrix}
    2 & -4 & 0 & -1 & -5 \\
0 & 0 & 2 & 1 & 5 \\
-1 & 2 & 3 & 2 & 10
    \end{pmatrix},\qquad B= \begin{pmatrix}
    2 \\
0 \\
-1
\end{pmatrix}$$

Calcularem les solucions del sistema d'equacions
$A\cdot X=B$ (és dir, determinarem quina forma tenen les columnes
$X=\begin{pmatrix} x_0\\x_1\\x_2\\x_3\\x_4 \end{pmatrix}$ que compleixen
l'equació anterior) reduint la matriu ampliada a la seva forma de
Gauss-Jordan.


A l'hora d'introduir les dades noteu que en el procés de reducció de
Gauss-Jordan pot ser necessari, en algun moment, realitzar divisions, i
per tant, per fer-ho cal *treballar amb coeficients a un cos*. Tot i
això, **SageMath**  sap trobar formes reduïdes
similars a la de Gauss-Jordan treballant per exemple als enters, o
altres situacions més complexes (polinomis amb coeficients a un
cos,...). Aquest pocés es fa amb `echelon_form` que no usa divisions a
no ser que la matriu ja tingui els coeficients adequats (que accepten
dividirse entre ells). Com que, en el nostre cas, la matriu de
coeficients només té valors enters, cal especificar que volem pensar-los
racionals (`QQ` en **SageMath** ) per tal que
s'apliqui l'esglaonament usant divisions (o, alternativament, utilitzar
`rref()` (consulteu la documentació per tal de veure les diferències entre
`echelon_form` i `rref`).

```sage
A = matrix(QQ,3,5,[2,-4,0,-1,-5,0,0,2,1,5,-1,2,3,2,10]);show(A)
B = vector([2,0,-1]);show(B)
Am = A.augment(B, subdivide=True);show(Am)
```

Encara que $B$ sigui un vector, i per tant és una *fila*, la funció
`augment`, que fabrica la matriu augmentada del sistema, *sap decidir*
que l'ha de posar com una columna.

Calculem ara la forma esglaonada del sistema.

```sage
Ar = Am.echelon_form()
show(Ar)
```

A partir d'aquesta expressió tindrem els pivots (incògnites lligades) i
les incògnites lliures de forma immediata (o a partir del resultat de
les funcions `pivots` i `nonpivots`) i les equacions (files) en les que
una de les incògnites lligades s'obté com a funció de les lliures i del
terme independent corresponent (funció `pivot_rows`).

```sage
pvts = Ar.pivots();show(pvts)
npvts = Ar.nonpivots();show(npvts)
flspv = Ar.pivot_rows();show(flspv)
```

I amb aquesta informació us hauria de quedar clar que les solucions són
de la forma
$$\begin{aligned}
x_0 &= 2\, x_1+\dfrac12\, x_3+\dfrac52\, x_4+1
\\
x_2 &= -\dfrac12\, x_3-\dfrac52\, x_4
\end{aligned}
$$
amb els valors de $x_1$, $x_3$ i $x_4$ arbitraris.

Segurament sabeu que, en aquest context, cada cop que es realitza un
procés de reducció sobre una matriu `Am` i s'arriba a un cert resultat
`Ar`, hi ha una mariu invertible `P` que codifica *totes* les operacions
que s'han fet en els sentit que es té la igualtat
$$\mathtt{P\cdot Am=Ar}$$ Si es vol saber quina és aquesta matriu `P`
només cal utilitzar `extended_echelon_form()` i extreure del resultat
que dona les parts corresponents.

```sage
GJP = Am.extended_echelon_form(subdivide=True); show(GJP)
GJ = GJP.matrix_from_columns([0,1,2,3,4,5])
P = GJP.matrix_from_columns([6,7,8])
show(GJ)
show(P)
```

Podeu comprovar, fent la multiplicació, com el producte de `P` per la
matriu original `Am` dona la forma reduïda `Ar`.


### Esgalonament de matrius amb paràmetres

Que passa si fem la forma esglaonada per una matriu amb paràmetres?
Veiem-ho. Si reintroduïm la matriu $Ak$ anterior

```sage
var('k')
Ak=matrix([[1,k+1,-1,0],[-1,2,k,1],[k+2,-1,-1,2*k-1]]);show(Ak)
AkE=Ak.echelon_form()
show(AkE)
```

obtenim un matriu horrorosa, que no és fàcil de simplificar (no funciona
automàticament). Podem fer

```sage
show(AkE.simplify_full())
```

```sage
show([a.simplify_full() for a in AkE.column(3)])
```

Però fent així semblaria que només hi ha problemes quan el denominador
(comú) de les tres entades de la columna 3 és zero. Però de fet sabem
que quan $k=0$ no té rang 3, i que de fet és el únic cas. El que ha
passat és que en algun moment **SageMath**  ha
dividit una fila o una columna per $k$.

El que podem fer és treballar directament en l'anell de polinomis en una
variable. Aleshores **SageMath**  no dividirà per
cap polinomi de grau $>0$, ja que aquests no tenen inversos als
polinomis. Si posem

```sage
R.<k> = QQ['k']  # També es pot escriure com PolynomialRing(QQ)
Ak = matrix([[1,k+1,-1,0],[-1,2,k,1],[k+2,-1,-1,2*k-1]]);show(Ak)
AkE = Ak.echelon_form()
show(AkE)
```

Ara és evident que només quan els dos polinomis de la darrera fila són
zero el rang és 2, i això només passa si $k=0$. Per exemple podeu
calcular el màxim comú divisor dels dos polinomis (ja que un zero comú
serà un zero del mcm), i surt $k$.

### Reducció pas a pas a mà d'una matriu (Opcional)

### Exemple 2 (Reducció pas per pas).
Quan interessa anar controlant
quines operacions de reducció es fan, el mètode anterior no serveix, ja
que només dona el resultat final. En aquests casos caldrà anar
realitzant les operacions de forma manual, cosa que es pot aconseguir
utilitzant: `.swap_rows()`, `.rescale_row()` i `.add_multiple_of_row()`
(el que fa cada una d'aquestes funcions resulta obvi a partir del seu
nom).

Comencem generant una còpia de la matriu ampliada del sistema per tal de
fer les operacions en aquesta matriu mentre mantenim el valor de la
matriu original.

```sage
AA=copy(Am); show(AA);
```

Intercanviar files $0$ i $2$

```sage
AA.swap_rows(0,2); show(AA)
```

Canvi de signe de la primera fila (índex $0$)

```sage
AA.rescale_row(0,-1); show(AA)
```

Restar $2$ vegades la tercera fila (índex $2$) a la primera (índex $0$)

```sage
AA.add_multiple_of_row(2,0,-2);show(AA)
```

Restar 3 cops la segona fila a la tercera.

```
AA.add_multiple_of_row(2,1,-3);show(AA)
```

Dividir per 2 la segona fila.

```sage
AA.rescale_row(1,1/2);show(AA)
```

Sumar 3 cops la segona fila a primera.

```sage
AA.add_multiple_of_row(0,1,3);show(AA)
```

I, com podeu comprovar, el resultat coincideix exactament (segurament)
amb el de la funció `echelon_form()`

## Subespais vectorials, suma i intersecció

### Construcció d'espais i subespais vectorials

Recordeu que per a construir un espai vectorial del tipus $V=K^n$ per un
cos $K$ i un enter $n\ge 1$ tenim la instrucció `VectorSpace(K,n)` o bé
`K^n`.

```sage
E = QQ^4
```

Recordeu que els cossos més usuals estan definits a
**SageMath**  amb els noms següents:

-   `QQ` els racionals $\mathbb{Q}$.

-   `RR` els nombre reals $\mathbb{R}$.

-   `CC` els nombres complexos $\mathbb{C}$.

**Molt important:** Per **SageMath** els nombres reals són
expressions decimals amb 53 bits de precissió. El fet de treballar
de forma aproximada fa que la majoria de propietats d'un cos no
siguin certes, sinó aproximadament certes. Per això, és probable que
ens trobem amb gran quantitat de resultats inesperats i per tant, a
no ser que sigui del tot necessari, evitarem treballar amb `RR` i `CC`.

Per treballar de forma genèrica amb una gran varietat d'expressions
simbòliques, **SageMath**  usa el que anomea
l'anell simbòlic (*symbolic ring*), que es denota per `SR`, i que de fet
és un cos. Tot i que no entrarem en més detalls, també podem treballar
amb cossos més complicats, com per exemple el cos de funcions racionals
amb coeficients a $\mathbb{Q}$,
$\mathbb{Q}(t)=\{\frac{p(t)}{q(t)}\mid q(t)\neq 0\}$:

```sage
E1 = VectorSpace(FractionField(PolynomialRing(QQ,'x')),3)
E1
```

Donats uns vectors d'un tal espai vectorial podem construir el subespai
vectorial generat per aquests vectors es pot utilitzar `subspace`, o bé
span.


```sage
u = E([5,-2,1,3])
v = E([1,1,2,-1])
```

```sage
w = 3*u-6*v
```

```sage
F = E.subspace([u,v,w])
show(F)
```

```sage
F
```

```sage
span([u,v,w])
```

Observem que al construir subespais també se'ls assigna automàticament
una base. Aquesta s'obté de forma canònica esglaonant (Gauss-Jordan) el
conjunt de vectors generadors.

Si no es vol treballar amb aquesta base, es pot especificar la base per
al subespai amb

```sage
F1 = E.subspace_with_basis([u,v])
```

```sage
F1
```

```sage
show(F1.gen(0))
show(F1.gen(1))
```

Notem però que els dos espais, tot i tenir assignades bases diferents,
són el mateix:

```sage
F == F1
```

Encara que un espai vectorial tingui una base assignada diferent, sempre
podem accedir a la que és la base canònica per a
**SageMath**  amb

```sage
F1.echelonized_basis()
```

Vegem ara com construir un espai vectorial a partir d'equacions que
satisfan les seves coordenades. Com sabeu, aquestes equacions han de
correspondre a un sistema d'equacions lineals homogeni i, per tant,
s'han de poder representar matricialment com $A\cdot X = 0$. Per tant,
el problema inicial serà crear la matriu del sistema. En aquest cas,
serà important el cos dels coeficients que assignem.

Per exemple, si volem construir el subespai vectorial $W$ dels vectors
$(x,y,z,t)\in\mathbb{Q}^4$ tals que $x+y=0$ i $3\,x-y-z-t=0$ es pot fer
el següent:

```sage
A = matrix(QQ, [[1,1,0,0],[3,-1,-1,-1]])
W = A.right_kernel()
W
```

Ja que tenim una matriu entrada, aprofitem per veure dues instruccions
que ens permeten obtenir els subespais vectorials generats per les files
o per les columnes de la matriu:

```sage
A.row_space()
```

```sage
A.column_space()
```

Com és d'esperar, els valors següents coincideixen:

```sage
A.row_space().dimension()
```

```sage
A.column_space().dimension()
```

```sage
A.rank()
```

### Suma i intersecció d'espais

La suma d'espais vectorials es pot crear de forma natural sumant-los amb
l'operador `+`, sempre i quan l'expressió tingui sentit, és a dir, que
siguin subespais vectorials d'un espai vectorial en comú.

```sage
F + W
```

```sage
V=(QQ^3).subspace([(1,1,-1)])
```

```sage
F + V    #Ha de donar error
```

Podem fer sumes de més d'un espai:

```sage
 span([u])+span([v])+span([w])
```

La intersecció d'espais vectorials es crea com a una propietat d'un dels
espais, però òbviament l'ordre que s'utilitzi serà irrellevant per al
resultat.

```sage
F.intersection(W)
```

```sage
W.intersection(F)
```

Per posar un exemple on apareixen aquestes construccions, calculem la
dimensió i bases pel subespais de $\mathbb{Q}^4$, $U$, $V$, $U+V$ i
$U\cap W$, on $$\begin{gathered}
U=\langle (1,-1,1,-1),(1,2,1,1),(2,-1,2,0),(4,0,4,2)\rangle,\text{ i}\\
V=\{(x,y,z,t)\in \mathbb{Q}^4 \mid 3\,x-2\,y-z-t=x-y-t=2\,x-y-z=0\}.\end{gathered}$$

Per tal de *crear* els espais $U$ i $V$ n'hi ha prou amb:

```sage
U = span([(1,-1,1,-1),(1,2,1,1),(2,-1,2,0),(4,0,4,2)],QQ)
```

```sage
B = matrix(QQ,[[3,-2,-1,-1],[1,-1,0,-1],[2,-1,-1,0]])
```

```sage
V = B.right_kernel()
```

I ara es poden obtenir les dimensions i bases dels espais $U$, $V$,
$U+V$ i $U\cap V$ de forma senzilla:


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

## Exercicis

### Exercici 1


Considereu les matrius $$P= \begin{pmatrix}
\dfrac {4}{7} & \dfrac {1}{7} &  - \dfrac {1}{7} \\[10pt]
 - \dfrac {5}{21} & \dfrac {4}{21} & \dfrac {1}{7} \\[10pt]
 - \dfrac {3}{7} & \dfrac {1}{7} &  - \dfrac {1}{7}
\end{pmatrix} \text {i } {M} =  \begin{pmatrix}
1 & 0 & -1 \\[5pt]
2 & 3 & 1 \\[5pt]
-1 & 3 &  - 3
\end{pmatrix}$$ i el vector
${v_{1}} =  (1,\ - \dfrac {11}{3},\ - 2)$ i el vector ${v_{2}} = 
(1,\ -1,\ 2 - 2\,\sqrt{2})$. Calculeu:


-- begin hide
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
-- end hide

- $M\cdot P$

```sage
-- begin hide
M*P
-- end hide
```

- $M\cdot v_{1}$, $M\cdot v_{2}$, $P\cdot v_{1}$ i $P\cdot v_{2}$

-- begin hide
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
-- end hide

- $v_{1}\cdot M$ i $v_{2}\cdot M$

-- begin hide
```sage
v1*M
```

```sage
v2*M
```
-- end hide

- Hi ha algun patró en els valors dels productes de les matrius pels
  vectors?

### Exercici 2


Considereu el sistema d'equacions lineals $$\left.
    \begin{aligned}
    2\, x_{1}-4\, x_{2}+3\, x_{3}-5\, x_{4}+x_{5} &= 3
    \\
    x_{1}-4\, x_{2}-2\, x_{3}-7\, x_{5} &= 2
    \\
    5\, x_{1} -3\, x_{3} -2\, x_{4}+2\, x_{5} &= 1
    \\
    x_{2}-4\, x_{3} +3\, x_{4}-x_{5} &= 3/2
    \end{aligned}
    \right\}$$

Genereu la seva matriu ampliada i realitzeu en aquesta matriu les
operacions de reducció necessàries per tal de determinar si és
compatible o no i conèixer les solucions si en té.

-- begin hide
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
-- end hide

### Exercici 3


Estudieu, per als diferents valors dels paràmetres $b$ i $t$, el
sistema d'equacions lineals (amb incògnites $x,\ y,\ z$)
$$
\left.
\begin{aligned}
    3\, x+2\, y +z &= t
    \\
    x-y+2\, z &= 1+t^{2}
    \\
    3\, x +7\, y -4\, z &= -1-t-t^{2}-t^{3}
    \\
    2\, x +y +b\, z &= t^{3}
\end{aligned}
\right\}
$$

-- begin hide

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

-- end hide

### Exercici 4


Determineu el rang de la matriu següent
$$\left( \begin{array}{ccccc}
4-x & 2 & -2+({2}/{3})x & 8-x & -4\\
13/2 & -1 & x-3 & 10 & -{9}/{2} \\
4x-4 & 6 & 2-({4}/{3})x & 3x-8 & 4 \\
6x-4 & 3 & 2-2x & 3x-8 & 4+x
\end{array}\right)$$
en funció del paràmetre $x$.

-- begin hide

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

-- end hide

### Exercici 5


Doneu una forma reduïda (per files) $F$ de la matriu $A =  \left(
{\begin{array}{rcc}
1 & 3 & 0 \\
2 & -1 &  - 5 \\
0 & 1 & 3
\end{array}}
 \right)$ i una matriu invertible $P$ tal que $P\cdot A= F$.

-- begin hide

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

-- end hide

### Exercici 6


Feu una funció de sage PAreduccio(A) de manera que, donada una
matriu A qualsevol, retornin 2 matrius J i P, on J és la forma
esglaonada reduïda i P invertible tal que $PA=J$. Proveu-ho amb les
matrius dels problemes (1), (4) i (5).

-- begin hide

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
-- end hide

### Exercici 7


Construïu una funció que, donada una matriu quadrada $A$, doni la
dimensió i una base de l'espai donat per la intersecció de l'espai
generat per les files i l'espai generat per les columnes de $A$.
Proveu-ho amb les matrius quadrades dels problemes (1), (4) i (5).

-- begin hide

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

-- end hide

### Exercici 8


Determineu bases pels subespais de $\mathbb{Q}^5$ següents, les
seves sumes i les seves interseccions. $$\begin{gathered}
V_1=\langle (1,1,1,1,1),(-1,0,1,0,-1),(1,2,1,2,1),(0,2,0,2,0)\rangle \\
V_2=\langle (1,2,3,4,5),(5,4,3,2,1)\rangle\\
V_3=\{ (x,y,z,t,u)\in \mathbb{Q}^5\mid x+y=z+t=3\,u\}\end{gathered}$$

-- begin hide

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
-- end hide
