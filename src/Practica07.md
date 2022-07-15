::: center
**Vectors i matrius**
:::

Per tal de fer els càlculs relacionats amb problemes d'àlgebra lineal
cal utilitzar vectors i matrius. Tot i que, en principi, podríeu pensar
que n'hi hauria prou considerant que un vector és una llista de
coeficients i una matriu una llista de vectors (columnes o files segons
convingui) [**SageMath**]{style="color: blue"} defineix unes classes
especials per a aquests tipus d'objectes per tal d'optimitzar les
funcions que els tractaran. Començarem, doncs, veient els mecanismes per
a definir matrius i vectors i algunes de les instruccions que permeten
manipular aquests objectes.

Abans, però, farem un breu recordatori de llistes i tuples.

# Llistes i tuples

Una llista $[a_1,a_2,\dots,a_n]$ i una tupla $(a_1,a_2,\dots,a_n)$
d'elements són objectes similars del Python, però tenen diferencies
importants que cal remarcar. La diferència més important, i de fet la
que marca les altres, és que una llista és mutable i una tupla no: podem
modificar "l'interior" d'una llista però no d'una tupla.

Per exemple, si posem ``

::: list

L = \[1,2,3,4,5\]

print(L\[-1\])

L\[-1\] = 10

print(L)
:::

la resposta serà $5$ i $[1,2,3,4,10]$. Però si posem ``

::: list

T = (1,2,3,4,5)

print(T\[-1\])

T\[-1\] = 10
:::

a part d'imprimir 5 ens surt un error que diu " 'tuple' object does not
support item assignment".

Com que les tuples són immutables, podrieu pensar que no podem posar una
cosa mutable dins una tupla. Però si que és pot sense problemes: una
tupla, per tant, pot contenir una llista, i aquesta és pot modificar
sense problemes. El problema és que una tupla com aquesta no seria
"hashable", que és un concepte que sortirà a la propera pràctica.

Es pot passar de llista a tupla i de tupla a llista amb les comandes
`tuple` i `list`, respectivament. Les tuples, com les llistes, es poden
construir utilitzant comprehensió.

Compte que el fet que és pugui mutar té conseqüències delicades: si
poses, per exemple LL=L, on L és una llista, i canvies el valor de
LL\[1\]=1000, llavors L també canvia. Per poder tenir una copia de L per
poder-la manipular sense canviar la llista particular s'ha de posar
LL=copy(L).

Les tuples i les llistes és poden sumar, i l'efecte és que es construeix
una nova llista o tupla que conté els elements de la primera
llista/tupla seguit del de la segona. Si posem $n*L$ per un natural $n$,
obtindrem la llista/tupla repetida n cops.

Un cas especial és el de les tuples de només un element: si poses
$T=[2]$, això és una llista amb un sol element. Però si poses $T=(2)$
obtens només el número 2. Per poder remeiar això cal posar $T=(2,)$.

# Vectors

## Construcció de vectors

La instrucció per fabricar un objecte que tingui les característiques
d'un vector és `vector()`. L'argument d'aquesta funció és una llista o
una tupla amb les components del vector. Els vectors es tracten, en
principi, com files, però si cal interpretar les seves components com
una columna no caldrà fer cap transformació ja que les funcions que
tracten els vectors ja estan preparades per fer-ho, com veurem més
endavant.

1.  v=vector(\[1,-3,0,-4\])

2.  u=vector(\[3/4,-1,1,0\])

3.  show(v)

4.  show(u)

Per tal d'accedir a cada una de les components d'un vector, es pot fer
com si fos una llista (recordeu que la numeració de les posicions
comença per $0$). Per tant, podem fer

1.  v\[0\]

2.  v\[2\]

3.  v\[-1\]

Però no funcionarà

1.  v\[4\]

De la mateixa manera que es pot conèixer el valor de cada una de les
components d'un vector també és possible modificar-les d'una en una.

1.  show(v)

2.  v\[1\]=7

3.  show(v)

Per tal de conèixer la mida d'un vector es pot utilitzar `degree` que
actua com una *propietat* del vector.

1.  u.degree()

2.  v.degree()

Noteu que `degree` no es pot usar fent `degree(u)` però sí que podeu fer
`len(u)`. Noteu també que la sintaxi `u.len()` tampoc és correcta.

Els vectors viuen en espais vectorials. En el nostre cas viuran en el
espai $\mathbb{Q}^d$, on $d$ és la mida del vector, i enlloc de
$\mathbb{Q}$ pot ser el cos on estiguin definits. Recordem que el cos
$\mathbb{Q}$ es posa `QQ` en [**SageMath**]{style="color: blue"}. Posat
``

::: list

u.parent()
:::

respon `Vector space of dimension 4 over Rational Field`, el que ens
indica el espai vectorial on viu el vector. Ho podem comprovar definint
l'espai ``

::: list

Q4=VectorSpace(QQ,4)

u in Q4
:::

L'espai $\mathbb{Q}^4$ també és pot definir de manera més curta com
$QQ^4$. Compte, però, que normalment el
[**SageMath**]{style="color: blue"} assigna el vector al espai més petit
on pot fer-ho. Així el vector `v` que té coefficients enters de fet es
pensa que viu en \"l'espai vectorial $\mathbb{Z}^4$ sobre $\mathbb{Z}$
de dimensió $4$\" (que de fet no és un espai vectorial, sinò un mòdul).
Això ho podem comprovar posant ``

::: list

v.parent()
:::

Tot i així si li preguntem si està a `Q4` diu que si, doncs
$\mathbb{Z}^4\subset \mathbb{Q}^4$. D'altra banda, també podem forçar
que estigui a `Q4` posant `v=Q4(v)`, que de fet ens dona una manera
alternativa de definir vectors: primer definir l'espai `V` on viuen i
després posar `V(llista)`. ``

::: list

w=Q4(\[1,-5,6,2\])

show(w)

w.parent()
:::

Abans de passar a les operacions en les que intervenen vectors, cal fer
notar que els vectors i matrius es comporten com les llistes; són
mutables, i per tant modificables. De forma que, si generem una variable
nova igualant-la amb una que ja conté un vector, l'únic que obtindrem
seran dos noms diferents per accedir al mateix contingut i qualsevol
canvi que es faci a aquest contingut a través d'un d'aquests noms es
reflectirà simultàniament a través de l'altre. Per tant, si posem

1.  v=vector(\[1,2,3,4\]); show(v)

generem una còpia assignant a una altre variable el valor de `v`

1.  vc=v; show(vc)

i ara modifiquem alguna de les components de `vc` (suposadament, per tal
de fer alguns càlculs guardant els valors originals)

1.  vc\[2\]=0

2.  show(vc)

Si ara mireu de recuperar el vector `v` original us trobareu amb

1.  show(v)

on s'observa que el canvi també apareix reflectit a `v`.

Si realment es vol obtenir un vector amb el mateix contingut però que es
pugui manipular de forma independent caldrà *fabricar* una còpia i
assignar una variable nova a aquesta còpia. La instrucció que es
necessita és, òbviament, `copy`

1.  vc=copy(v)

2.  show(vc);show(v)

3.  vc\[2\]=-1

4.  show(vc);show(v)

5.  v\[0\]=-2

6.  show(vc);show(v)

## Suma de vectors, producte de vectors per escalars i producte escalar de vectors

Les operacions bàsiques amb vectors (suma de vectors i multiplicació
d'un vector per un escalar) es realitzen directament utilitzant els
operadors `+` o `*`. Cal tenir en compte que l'operació de multiplicació
entre vectors de la mateixa mida també està definida i calcula el seu
producte escalar (tot i que la funció `dot_product` també calcula el
producte escalar de vectors).

1.  3\*u

2.  v/2

3.  0.5\*v

4.  5.2\*v

5.  u+v

6.  u\*v

7.  u.dot_product(v)

Naturalment, es produirà un error si s'intenta realitzar una operació no
permesa.

1.  u+5

2.  u+vector(\[1,2\])

3.  u\*vector(\[1,2\])

Ara bé, tingueu en compte que `0` s'interpreta com el vector zero i sí
permet operar amb ell

1.  print(u+0)

2.  print(u-u==0)

Si multipliquem un vector sobre el racionals per un nombre que no sigui
(explícitament) racional, el vector canvia de lloc on viu. Al posar ``

::: list

(0.5\*v).parent()
:::

ens diu que viu al espai $\mathbb{RR}^4$ (de fet al espai $RR^4$, on
$RR$ és el cos dels reals amb precisió simple, de 53 bits de precisió).

Noteu que, en particular, el comportament de l'operador `+` és diferent
si la suma es realitza entre vectors o llistes: quan la suma és de
vectors el resultat té com a components les sumes de les components
corresponents de cada un dels factors, mentre que la suma de dos
objectes de tipus llista consisteix en la concatenació dels elements
respectius[^1].

1.  vector(\[1,2,3\])+vector(\[-2,-1,0\])

2.  \[1,2,3\]+\[-2,-1,0\]

# Matrius

## Creació de matrius

La funció `matrix` és la que permet crear objectes de tipus matricial.
En la versió més curta utilitza com argument una llista amb els
continguts de les files (els elements de la llista són llistes de la
mateixa longitud amb els elements de cada fila) i genera l'objecte de
tipus matricial corresponent.

1.  A=matrix(\[\[1,2\],\[3,4\],\[5,6\],\[7,8\]\])

2.  show(A)

També es pot generar un matriu a partir d'una llista de vectors o de
tuples de la mateixa longitud (que seran les files de la matriu
resultant).

1.  u=vector(\[1,2,3\]);v=vector(\[3,4,5\])

2.  M=matrix(\[u,v\])

3.  show(M)

Alternativament, es pot especificar el nombre de files i columnes i
donar les dades com una llista individual (ordenant, és clar, per files)

1.  M0=matrix(2,4,\[1,2,3,4,5,6,7,8\])

2.  M10=matrix(10,10,range(100))

Si mireu la informació que proporciona la instrucció `matrix?` encara
descobrireu més variants per generar matrius amb aquesta instrucció.

D'altre banda, hi ha funcions que construeixen matrius especials, com
per exemple, la matriu identitat de la mida que es desitgi

1.  show(identity_matrix(4))

Una matriu diagonal amb uns coeficients donats

1.  dd=diagonal_matrix(\[1,-1,2,3\]);show(dd)

També podem crear matrius a partir de les files i columnes d'una altra
(submatriu).

1.  Br=M10.matrix_from_rows(\[1,3,4\])

2.  Bc=M10.matrix_from_columns(\[0,2\])

3.  show(Br)

4.  show(Bc)

5.  Brc=M10.matrix_from_rows_and_columns(\[0,2\],\[1,2,4\])

6.  show(Brc)

## Accedint als continguts d'una matriu

Si es vol accedir al valor d'una de les posicions de la matriu n'hi
haurà prou expressant l'índex corresponent a la fila i la columna (amb
aquest ordre), i tenint en compte que les matrius, com les llistes,
comencen amb la fila 0 i columna 0.

1.  A\[0,0\]

2.  A\[3,1\]

Tot i que es pot extreure una fila qualsevol d'una matriu indicant un
sol índex, hi ha funcions específiques per extreure files i columnes
(noteu però que el resultat és un objecte de tipus vector)

1.  show(A\[2\])

2.  show(A.row(2))

3.  show(A.column(1))

Si es necessita saber les mides d'una matriu (si es vol comprovar si és
quadrada, per exemple) es pot utilitzar `nrows` i `ncols`

1.  A.nrows()

2.  A.ncols()

Encara que, com que hi ha una bateria molt extensa de tests associats a
una matriu, entre els quals hi ha la comprovació del fet que sigui
quadrada, aquesta comprovació es podria fer directament amb

1.  show(A.is_square())

2.  show(M10.is_square())

També podem canviar les files per les columnes (transposar la matriu)
amb la instrucció `transpose`

1.  show(A.transpose())

2.  show(transpose(A))

## Suma de matrius i producte per escalars

Com és d'esperar, les matrius es poden multiplicar per escalars i sumar
entre elles sempre que tinguin la mateixa mida. Les operacions es fan
component a component.

1.  B1=matrix(3,2,range(6))

2.  B2=matrix(3,2,\[3,3,3,1,1,1\])

3.  B3=B1+5\*B2

4.  show(B3)

## Producte de matrius per vectors

Quan multipliquem una matriu per un vector,
[**SageMath**]{style="color: blue"} interpreta convenientment el vector
com a fila o columna, segons si es vol fer la multiplicació per la dreta
o per l'esquerra (sempre que el producte que plantegem tingui sentit).

1.  u=vector(\[1,2,3\]);show(u)

2.  w=vector(\[-1,1\]);show(w)

3.  A=matrix(\[\[1,1\],\[-1,2\],\[2,1\]\]);show(A)

4.  u\*A

5.  A\*u

6.  w\*A

7.  A\*w

## Producte de matrius

Naturalment, l'operador ` * ` realitza el producte de matrius sempre que
estigui definit.

1.  A=matrix(\[\[1,2,3\],\[4,5,6\]\])

2.  B=matrix(\[\[1,2\],\[3,4\],\[5,6\]\])

3.  show(A,B)

4.  show(A\*B)

5.  show(B\*A)

6.  C=matrix(\[\[1,2,3\],\[4,5,6\],\[7,8,9\]\]);show(C)

7.  show(A\*C)

8.  show(C\*A)

## Matriu inversa i altres potències

Les multiplicacions successives d'una matriu quadrada per si mateixa
(potències) es poden obtenir amb l'operador de potències ordinari
`^\wedge` i, en particular, la inversa (si existeix) es pot obtenir amb
l'exponent $-1$.

1.  A=matrix(\[\[1,1,0\],\[0,1,0\],\[0,0,-1\]\]);show(A)

2.  show(A$\wedge$15)

3.  show(A$\wedge$(-1))

4.  B=matrix(\[\[1,1,1\],\[-1,2,1\],\[0,3,2\]\]);show(B)

5.  show(B$\wedge$`<!-- -->`{=html}3)

6.  show(B$\wedge$(-1))

Encara que, si es vol, es pot calcular la inversa d'una matriu
utilitzant la funció específica `inverse`

1.  show(A.inverse())

Fins i tot podem calcular potències simbòlicament!

1.  var('n')

2.  show(A$\wedge n$)

## Traça i determinant

En les matrius quadrades, la traça i el determinant juguen un paper molt
important. Per tal de calcular el determinant d'una matriu quadrada es
pot aplicar `det` o, si no tenim mandra d'escriure més lletres,
`determinant`. Per calcular la traça, cal usar el mètode de les matrius
`trace()`.

1.  A.det()

2.  A.determinant()

3.  B.trace()

Si la matriu no és quadrada ens dóna un error

1.  Mc=matrix(2,3,\[1,2,3,4,5,6\])

2.  Mc.det()

3.  Mc.trace()

## Rang

La instrucció que calcula el rang de les matrius és `rank`

1.  B=matrix(\[\[1,2,3\],\[2,1,-1\],\[1,-1,-4\],\[3,3,2\]\]);show(B)

2.  B.rank();rank(B)

Naturalment, quan el problema del càlcul del rang està posat en una
família de matrius depenent d'un, o més, paràmetres, la funció `rank` no
és capaç de distingir dins la família quins són els casos especials. Per
exemple, si es considera la família de matrius depenent del paràmetre
$k$ donada per $$A_k=\begin{pmatrix} 
 1 & k + 1 & -1 & 0 \\
-1 & 2 & k & 1 \\
k + 2 & -1 & -1 & 2 \, k - 1
\end{pmatrix}$$ el valor de la funció `rank` aplicada a l'expressió
genèrica de les matrius de la família donarà $3$.

1.  var('k')

2.  Ak=matrix(\[\[1,k+1,-1,0\],\[-1,2,k,1\],\[k+2,-1,-1,2\*k-1\]\]);show(Ak)

3.  Ak.rank()

4.  Ak.parent

De fet, [**SageMath**]{style="color: blue"} treballa en un context
(utilitza un *cos de coeficients* especial) on la matriu en efecte té
rang $3$ (executeu `Ak.parent()`). Tot i això, com a mínim en el cas
$k=0$, la matriu només té rang $2$ (es pot veure clarament que l'última
fila és la diferència entre les altres dues).

1.  A0=Ak.subs(k=0);show(A0)

2.  rank(A0)

Fet que es pot preveure si es calcula, per exemple, el determinant de la
submatriu formada per les tres primeres columnes, se'n determinen les
arrels als racionals i

1.  AA=Ak.matrix_from_columns(\[0,1,2\]) show(AA)

2.  d=AA.det().expand()

3.  show(d)

4.  sols=d.roots(ring=QQ)

5.  show(sols)

i queda clar que l'únic valor racional $k$ que anuł.la el determinant és
$k=0$ és l'únic valor en els que el rang podria ser inferior a $3$
mentre que en qualsevol altre cas el rang és el màxim possible ($3$).

# Sistemes d'equacions lineals. Reducció de matrius

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
[**SageMath**]{style="color: blue"} es poden utilitzar per tal de
resoldre un sistema seguint aquest procés.

::: exemple
**Exemple 1**. *Considerem $$A= \begin{pmatrix}
    2 & -4 & 0 & -1 & -5 \\
0 & 0 & 2 & 1 & 5 \\
-1 & 2 & 3 & 2 & 10
    \end{pmatrix},\qquad B= \begin{pmatrix}
    2 \\
0 \\
-1
    \end{pmatrix}$$ Calcularem les solucions del sistema d'equacions
$A\cdot X=B$ (és dir, determinarem quina forma tenen les columnes
$X=\begin{pmatrix} x_0\\x_1\\x_2\\x_3\\x_4 \end{pmatrix}$ que compleixen
l'equació anterior) reduint la matriu ampliada a la seva forma de
Gauss-Jordan.*
:::

A l'hora d'introduir les dades noteu que en el procés de reducció de
Gauss-Jordan pot ser necessari, en algun moment, realitzar divisions, i
per tant, per fer-ho cal *treballar amb coeficients a un cos*. Tot i
això, [**SageMath**]{style="color: blue"} sap trobar formes reduïdes
similars a la de Gauss-Jordan treballant per exemple als enters, o
altres situacions més complexes (polinomis amb coeficients a un
cos,...). Aquest pocés es fa amb `echelon_form` que no usa divisions a
no ser que la matriu ja tingui els coeficients adequats (que accepten
dividirse entre ells). Com que, en el nostre cas, la matriu de
coeficients només té valors enters, cal expecificar que volem pensar-los
racionals (`QQ` en [**SageMath**]{style="color: blue"}) per tal que
s'apliqui l'esglaonament usant divisions (o, alternativament, utilitzar
`rref()`[^2]).

1.  A=matrix(QQ,3,5,\[2,-4,0,-1,-5,0,0,2,1,5,-1,2,3,2,10\]);show(A)

2.  B=vector(\[2,0,-1\]);show(B)

3.  Am=A.augment(B,subdivide=True);show(Am)

Encara que $B$ sigui un vector, i per tant és una *fila*, la funció
`augment`, que fabrica la matriu augmentada del sistema, *sap decidir*
que l'ha de posar com una columna.

Calculem ara la forma esglaonada del sistema.

1.  Ar=Am.echelon_form();show(Ar)

A partir d'aquesta expressió tindrem els pivots (incògnites lligades) i
les incògnites lliures de forma immediata (o a partir del resultat de
les funcions `pivots` i `nonpivots`) i les equacions (files) en les que
una de les incògnites lligades s'obté com a funció de les lliures i del
terme independent corresponent (funció `pivot_rows`).

1.  pvts=Ar.pivots();show(pvts)

2.  npvts=Ar.nonpivots();show(npvts)

3.  flspv=Ar.pivot_rows();show(flspv)

I amb aquesta informació us hauria de quedar clar que les solucions són
de la forma $$\begin{aligned}
x_0 &= 2\, x_1+\dfrac12\, x_3+\dfrac52\, x_4+1
\\
x_2 &= -\dfrac12\, x_3-\dfrac52\, x_4 
\end{aligned}$$ amb els valors de $x_1$, $x_3$ i $x_4$ arbitraris.

Segurament sabeu que, en aquest context, cada cop que es realitza un
procés de reducció sobre una matriu `Am` i s'arriba a un cert resultat
`Ar`, hi ha una mariu invertible `P` que codifica *totes* les operacions
que s'han fet en els sentit que es té la igualtat
$$\mathtt{P\cdot Am=Ar}$$ Si es vol saber quina és aquesta matriu `P`
només cal utilitzar `extended_echelon_form()` i extreure del resultat
que dóna les parts corresponents.

1.  GJP=Am.extended_echelon_form(subdivide=True); show(GJP)

2.  GJ=GJP.matrix_from_columns(\[0,1,2,3,4,5\])

3.  P=GJP.matrix_from_columns(\[6,7,8\])

4.  show(GJ)

5.  show(P)

Podeu comprovar, fent la multiplicació, com el producte de `P` per la
matriu original `Am` dóna la forma reduïda `Ar`.

## Esgalonament de matrius amb paràmetres

Que passa si fem la forma esglaonada per una matriu amb paràmetres?
Veiem-ho. Si reintroduïm la matriu $Ak$ anterior

``

::: list

var('k')

Ak=matrix(\[\[1,k+1,-1,0\],\[-1,2,k,1\],\[k+2,-1,-1,2\*k-1\]\]);show(Ak)

AkE=Ak.echelon_form(); show(AkE)
:::

obtenim un matriu horrorosa, que no és fàcil de simplificar (no funciona
automàticament). Podem fer

``

::: list

show(\[a.full_simplify() for a in AkE.column(3)\])
:::

Però fent així semblaria que només hi ha problemes quan el denominador
(comú) de les tres entades de la columna 3 és zero. Però de fet sabem
que quan $k=0$ no té rang 3, i que de fet és el únic cas. El que ha
passat és que en algun moment [**SageMath**]{style="color: blue"} ha
dividit una fila o una columna per $k$.

El que podem fer és treballar directament en l'anell de polinomis en una
variable. Aleshores [**SageMath**]{style="color: blue"} no dividirà per
cap polinomi de grau $>0$, ja que aquests no tenen inversos als
polinomis. Si posem

``

::: list

R.\<k\> = PolynomialRing(QQ)

Ak=matrix(\[\[1,k+1,-1,0\],\[-1,2,k,1\],\[k+2,-1,-1,2\*k-1\]\]);show(Ak)

AkE=Ak.echelon_form(); show(AkE)
:::

Ara és evident que només quan els dos polinomis de la darrera fila són
zero el rank és 2, i això només passa si $k=0$. Per exemple podeu
calcular el màxim comú divisor dels dos polinomis (ja que un zero comú
serà un zero del mcm), i surt $k$.

## Reducció pas a pas a mà d'una matriu (Opcional)

::: exemple
**Exemple 2** (Reducció pas per pas). *Quan interessa anar controlant
quines operacions de reducció es fan, el mètode anterior no serveix, ja
que només dóna el resultat final. En aquests casos caldrà anar
realitzant les operacions de forma manual, cosa que es pot aconseguir
utilitzant:*

::: center
*` .swap_rows() .rescale_row() .add_multiple_of_row() `*
:::

*(el que fa cada una d'aquestes funcions resulta obvi a partir del seu
nom).*
:::

Comencem generant una còpia de la matriu ampliada del sistema per tal de
fer les operacions en aquesta matriu mentre mantenim el valor de la
matriu original.

1.  AA=copy(Am); show(AA);

Intercanviar files $0$ i $2$

1.  AA.swap_rows(0,2); show(AA)

Canvi de signe de la primera fila (índex $0$)

1.  AA.rescale_row(0,-1);show(AA)

Restar $2$ vegades la tercera fila (índex $2$) a la primera (índex $0$)

1.  AA.add_multiple_of_row(2,0,-2);show(AA)

Restar 3 cops la segona fila a la tercera.

1.  AA.add_multiple_of_row(2,1,-3);show(AA)

Dividir per 2 la segona fila.

1.  AA.rescale_row(1,1/2);show(AA)

Sumar 3 cops la segona fila a primera.

1.  AA.add_multiple_of_row(0,1,3);show(AA)

I, com podeu comprovar, el resultat coincideix exactament (segurament)
amb el de la funció `echelon_form()`

# Subespais vectorials, suma i intersecció

## Construcció d'espais i subespais vectorials

Recordeu que per a construir un espai vectorial del tipus $V=K^n$ per un
cos $K$ i un enter $n\ge 1$ tenim la instrucció `VectorSpace(K,n)` o bé
`K^\wedge n`.

``

::: list

E=QQ\^4

E
:::

Recordeu que els cossos més usuals estan definits a
[**SageMath**]{style="color: blue"} amb els noms següents:

-   `QQ` els racionals $\mathbb{Q}$.

-   `RR` els nombre reals[^3] $\mathbb{R}$.

-   `CC` els nombres complexos [^4] $\mathbb{C}$.

Per treballar de forma genèrica amb una gran varietat d'expressions
simbòliques, [**SageMath**]{style="color: blue"} usa el que anomea
l'anell simbòlic (*symbolic ring*), que es denota per `SR`, i que de fet
és un cos. Tot i que no entrarem en més detalls, també podem treballar
amb cossos més complicats, com per exemple el cos de funcions racionals
amb coeficients a $\mathbb{Q}$,
$\mathbb{Q}(t)=\{\frac{p(t)}{q(t)}\mid q(t)\neq 0\}$:

``

::: list

E1=VectorSpace(FractionField(PolynomialRing(QQ,'x')),3)

E1
:::

Donats uns vectors d'un tal espai vectorial podem construir el subespai
vectorial generat per aquests vectors es pot utilitzar `subspace`, o bé
span.

``

::: list

u=E(\[5,-2,1,3\])

v=E((1,1,2,-1))

w=3\*u-6\*v

F=E.subspace(\[u,v,w\])

show(F)

F

span(\[u,v,w\])
:::

Observem que al construir subespais també se'ls assigna automàticament
una base. Aquesta s'obté de forma canònica esglaonant (Gauss-Jordan) el
conjunt de vectors generadors.

Si no es vol treballar amb aquesta base, es pot especificar la base per
al subespai amb

``

::: list

F1=E.subspace_with_basis(\[u,v\])

F1

F1.0

F1.1
:::

Notem però que els dos espais, tot i tenir assignades bases diferents,
són el mateix:

``

::: list

F

F1

F==F1
:::

Encara que un espai vectorial tingui una base assignada diferent, sempre
podem accedir a la que és la base canònica per a
[**SageMath**]{style="color: blue"} amb

``

::: list

F1.echelonized_basis()
:::

Vegem ara com construir un espai vectorial a partir d'equacions que
satisfan les seves coordenades. Com sabeu, aquestes equacions han de
correspondre a un sistema d'equacions lineals homogeni i, per tant,
s'han de poder representar matricialment com $A\cdot X = 0$. Per tant,
el problema inicial serà crear la matriu del sistema. En aquest cas,
serà important el cos dels coeficients que assignem.

Per exemple, si volem construir el subespai vectorial $W$ dels vectors
$(x,y,z,t)\in\mathbb{Q}^4$ tals que $x+y=0$ i $3\,x-y-z-t=0$ es pot fer
el següent:

``

::: list

A=matrix(QQ, \[\[1,1,0,0\],\[3,-1,-1,-1\]\])

W=A.right_kernel()

W
:::

Ja que tenim una matriu entrada, aprofitem per veure dues instruccions
que ens permeten obtenir els subespais vectorials generats per les files
o per les columnes de la matriu:

``

::: list

A.row_space() \# subsepai generat per les files.

A.column_space() \# subespai generat per les columnes.
:::

Com és d'esperar, els valors següents coincideixen:

``

::: list

print(A.row_space().dimension())

print(A.column_space().dimension())

print(A.rank())
:::

## Suma i intersecció d'espais

La suma d'espais vectorials es pot crear de forma natural sumant-los amb
l'operador `+`, sempre i quan l'expressió tingui sentit, és a dir, que
siguin subespais vectorials d'un espai vectorial en comú.

``

::: list

F+W \# Els dos són subespais de QQ\^4

V=(QQ\^3).subspace(\[(1,1,-1)\])

F+V \# Ha de donar error.
:::

Podem fer sumes de més d'un espai

``

::: list

span(\[u\])+span(\[v\])+span(\[w\])
:::

La intersecció d'espais vectorials es crea com a una propietat d'un dels
espais, però òbviament l'ordre que s'utilitzi serà irrellevant per al
resultat. ``

::: list

F.intersection(W)

W.intersection(F)
:::

Per posar un exemple on apareixen aquestes construccions, calculem la
dimensió i bases pel subespais de $\mathbb{Q}^4$, $U$, $V$, $U+V$ i
$U\cap W$, on $$\begin{gathered}
U=\langle (1,-1,1,-1),(1,2,1,1),(2,-1,2,0),(4,0,4,2)\rangle,\text{ i}\\
V=\{(x,y,z,t)\in \mathbb{Q}^4 \mid 3\,x-2\,y-z-t=x-y-t=2\,x-y-z=0\}.\end{gathered}$$

Per tal de *crear* els espais $U$ i $V$ n'hi ha prou amb: ``

::: list

U=span(\[(1,-1,1,-1),(1,2,1,1),(2,-1,2,0),(4,0,4,2)\],QQ)

B=matrix(QQ,\[\[3,-2,-1,-1\],\[1,-1,0,-1\],\[2,-1,-1,0\]\])

V=B.right_kernel()
:::

I ara es poden obtenir les dimensions i bases dels espais $U$, $V$,
$U+V$ i $U\cap V$ de forma senzilla: ``

::: list

U.basis()

U.dimension()

V.basis()

V.dimension()

(U+V).basis()

(U+V).dimension()

U.intersection(V).basis()

V.intersection(V).dimension()
:::

# Exercicis {#exercicis .unnumbered}

1.  Considereu les matrius $$P= \begin{pmatrix}
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

    (a) $M\cdot P$

    (b) $M\cdot v_{1}$, $M\cdot v_{2}$, $P\cdot v_{1}$ i $P\cdot v_{2}$

    (c) $v_{1}\cdot M$ i $v_{2}\cdot M$

    Hi ha algun patró en els valors dels productes de les matrius pels
    vectors?

2.  Considereu el sistema d'equacions lineals $$\left.
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

3.  Estudieu, per als diferents valors dels paràmetres $b$ i $t$, el
    sistema d'equacions lineals (amb incògnites $x,\ y,\ z$) $$\left.
    \begin{aligned}
    3\, x+2\, y +z &= t
    \\
    x-y+2\, z &= 1+t^{2}
    \\
    3\, x +7\, y -4\, z &= -1-t-t^{2}-t^{3}
    \\
    2\, x +y +b\, z &= t^{3}
    \end{aligned}
    \right\}$$

4.  Determineu el rang de la matriu següent
    $$\left( \begin{array}{ccccc}
    4-x & 2 & -2+({2}/{3})x & 8-x & -4\\
    13/2 & -1 & x-3 & 10 & -{9}/{2} \\
    4x-4 & 6 & 2-({4}/{3})x & 3x-8 & 4 \\
    6x-4 & 3 & 2-2x & 3x-8 & 4+x
    \end{array}\right)$$ en funció del paràmetre $x$.

5.  Doneu una forma reduïda (per files) $F$ de la matriu $A =  \left(
    {\begin{array}{rcc}
    1 & 3 & 0 \\
    2 & -1 &  - 5 \\
    0 & 1 & 3
    \end{array}}
     \right)$ i una matriu invertible $P$ tal que $P\cdot A= F$.

6.  Feu una funció de sage PAreduccio(A) de manera que, donada una
    matriu A qualsevol, retornin 2 matrius J i P, on J és la forma
    esglaonada reduïda i P invertible tal que $PA=J$. Proveu-ho amb les
    matrius dels problemes (1), (4) i (5).

7.  Construïu una funció que, donada una matriu quadrada $A$, doni la
    dimensió i una base de l'espai donat per la intersecció de l'espai
    generat per les files i l'espai generat per les columnes de $A$.
    Proveu-ho amb les matrius quadrades dels problemes (1), (4) i (5).

8.  Determineu bases pels subespais de $\mathbb{Q}^5$ següents, les
    seves sumes i les seves interseccions. $$\begin{gathered}
    V_1=\langle (1,1,1,1,1),(-1,0,1,0,-1),(1,2,1,2,1),(0,2,0,2,0)\rangle \\
    V_2=\langle (1,2,3,4,5),(5,4,3,2,1)\rangle\\
    V_3=\{ (x,y,z,t,u)\in \mathbb{Q}^5\mid x+y=z+t=3\,u\}\end{gathered}$$

[^1]: Aquí es veu un dels motius que justifiquen crear una classe
    d'objectes especial per als vectors.

[^2]: Consulteu la documentació per tal de veure les diferències entre
    `echelon_form` i `rref`.

[^3]: []{#precissio label="precissio"}Per a
    [**SageMath**]{style="color: blue"} els nombres reals són
    expressions decimals amb 53 bits de precissió. El fet de treballar
    de forma aproximada fa que la majoria de propietats d'un cos no
    siguin certes, sino aproximadament certes. Per això, és probable que
    ens trobem amb gran quantitat de resultats inesperats i per tant, a
    no ser que sigui del tot necessari, evitarem treballar amb `RR`.

[^4]: Vegeu nota [3](#precissio){reference-type="ref"
    reference="precissio"}
