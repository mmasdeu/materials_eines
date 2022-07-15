::: center
**Complements a la programació en Python**
:::

# Més sobre les funcions de Python

Recordeu que una funció ben definida en Python hauria de tenir un
docstring, o sigui un text explicatiu, que descrigui què fa la funció.
És estàndard escriure una frase a la línia després de def amb tres \"  a
cada banda. Si té més d'una línia es posen tres \"  en una línia a part.

Per a definir una funció de Python amb `def` podeu, si voleu, posar
algunes entrades (o paràmetres) opcionals, amb uns valors predeterminats
en el cas que no hi siguin. Això es fa escrivint $(variable=valor)$ en
la definició. Tots aquests paràmetres han d'anar al final de la llista
de paràmetres.

Per exemple, imagineu que a la funció `ICos` de la pràctica 5, en que es
donava l'origen, la precisió i el màxim d'iteracions, voleu que, si no
dieu res, per defecte es facin $10$ iteracions. Podem redefinir-la de la
següent manera: ``

::: list

def ICos(origen,precisio,maxit=10):

\"\"\"  Itera la funció cos(s) fins que dos valors consecutius

estan a distància menor que prec i com a màxim maxit iteracions

(per defecte maxit=10)

\"\"\"

s=origen.n()

sa=s+1

k=0

print
\"\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\"

while abs(s-sa)\>precisio and k\<maxit:

k+=1; sa=s; s=cos(s)

show('Iteracio', k,', s=', s)

if k==maxit:

show(\"S'ha arribat al màxim d'iteracions previstes!!!\")

show(\"No és segur que el valor sigui prou ajustat\")

show(\"El resultat correspon a l'últim valor obtingut.\")

else:

show(\"Després de \", k, \"iteracions,\")

show(\"les dues últimes aproximacions difereixen en \")

show(abs(s-sa).n(digits=3), \"i són:\")

show(sa,\",\",s)

print
\"\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\"

return s
:::

Ara, si posem `valor=ICos(pi/3,0.00001)` ens respondrà que
`"S’ha arribat al màxim d’iteracions previstes!!!"`\... després de fer
10 iteracions. Però també podem executar la instrucció
`valor=ICos(pi/3,0.00001,30)` i després de 29 iteracions troba el valor
amb un error de $0.00001$. Observeu també com hem escrit el "docstring"
al inici.

Una comanda més que es pot utilitzar amb les funcions enlloc del
`return` és el `yield`: així com amb el `return`, un cop la funció
retorna un valor oblida tot el que ha passat dins de la funció, amb el
`yield` la funció recorda els valors que tenia, de manera que quan la
tornes a cridar recorda exactament on estava executant i amb quins
valors. De fet la funció amb un yield el que retorna és un iterador.

Anem a veure un exemple: farem una funció que retorni els nombres
primers acabat en 1 entre $n$ i $m$, però enlloc de ser una llista serà
un iterador (he utilitzat `xsrange()` per assegurar que el
[**SageMath**]{style="color: blue"} em retorni un iterador): ``

::: list

def primers_acabats_en_1(n,m):

for p in xsrange(n+1,m+1):

if (p.is_prime() and (p%10)==1):

yield p
:::

Ara, si poseu ``

::: list

it = primers_acabats_en_1(10,30)

next(it)
:::

us retorna 11, però al posar `next(it)` de nou us diu `StopIteration` ja
que no hi ha més primers i ha arribat al final del iterador. Podeu usar
aquest iterador en un for, fins i tot posant nombres molt grans, amb la
seguretat que no crearà una llista massa gran de nombres, i només
recorda el procediment (la funció) per a trobar el següent valor.

En general, es recomanable que una funció (acabada) de Python

1.  Tingui una breu descripció de què fa.

2.  Se li passi com a paràmetres tots els valors que necessitarà.

3.  Retorni el o els valors que vulguem (amb un return o similar): es
    diu que la funció és productiva.

4.  No demani al usuari amb un input cap dada.

5.  No imprimeixi cap missatge entremig (a menys que siguin missatges
    d'error).

Es poden posar prints en mig d'una funció mentre s'està programant per a
veure si fa el que un vol, però no és recomanable fer-ho quan la funció
està acabada, o com a mínim no per defecte. En qualsevol cas, les
funcions haurien de retornar el que es demana per tal que després es
pugui usar en un programa.

# Control d'errors en Python

Una altra eina que hauríeu de conèixer de les funcions és com fer que
doni un error en el cas que no se li passin entrades adequades. Això
s'hauria de fer amb la comanda raise. Quan es troba un raise, es mira la
condició, i si es compleix, la funció para i envia un missatge d'error.
Aquest missatge es pot posar escrivint TypeError (si el tipus no és
l'adequat) o ValueError (si el valor no és el adequat), o altres tipus
d'errors (el tipus d'error és una cosa que decidiu vosaltres, però es bo
que sigui el que correspon).

Per exemple, en l'exercici 5 dels exercicis de consolidació es demanava
fer una funció Goldbach(n) de manera que donat un nombre enter de Sage
que sigui parell i més gran que 4 ens retorni una llista amb les
parelles de primers $(p,q)$ amb $p+q=n$ (i entendrem que $p\le q$). Per
a tal de assegurar que ens passen una dada adequada podem fer ``

::: list

def Goldbach(n):

if type(n)!=Integer:

raise TypeError('Ha de ser un enter de sage')

elif is_odd(n):

raise ValueError('El nombre '+ str(n)+ ' no es parell')

elif n\<6:

raise ValueError('El nombre '+ str(n)+' es mes petit que 6')

return \[(p,n-p) for p in prime_range(3,n//2+1) if (n-p).is_prime()\]
:::

Si posem `Goldbach(12/2)` ens respon
`TypeError: Ha de ser un enter de sage`, si posem `Goldbach(11)` ens
respon `ValueError: El nombre 11 no es parell`, i si posem `Goldbach(4)`
ens respon `ValueError: El nombre 4 es mes petit que 6`.

Fixeu-vos que per imprimir el nombre `n` en el error el que he fet ha
estat transformar-lo en una cadena (string) amb `str(n)`, i després he
sumat les cadenes (que equival a posar-les una rere l'altra).

Una altra possibilitat que ens ofereix el Python és utilitzar el
`assert`: es tracta de demanar que una certa condició ha de ser
satisfeta per seguir amb la funció, sinó retorna un error (de assert).
Per exemple, el codi anterior es pot fer amb asserts com ``

::: list

def Goldbach(n):

assert(type(n)==Integer),'Ha de ser un enter de sage'

assert(is_even(n)),'El nombre '+ str(n)+ ' no es parell'

assert(n\>=6),'El nombre '+ str(n)+' es mes petit que 6'

return \[(p,n-p) for p in prime_range(3,n//2+1) if (n-p).is_prime()\]
:::

Fixeu-vos que hem de posar la condició que s'ha de satisfer, (i no com
abans, que posàvem la que no es satisfà). Ara, si posem `Goldbach(12/2)`
ens respon `AssertError: Ha de ser un enter de sage`, i la resta igual
amb un `AssertError`. L'inconvenient és, per tant, que no et distingeix
el tipus d'error. En general, el `assert` només s'hauria de usar per
casos que un no espera que passin mai però que es vol assegurar per
evitar que el programa funcioni malament. En canvi el raise es pot
utilitzar per casos que es poden donar i així avisar de què ha passat a
l'usuari. Això es relaciona directament amb el try/except que ara
comentarem.

Finalment explicarem l'us de try i except, que justament són molt útils
per tal d'aplicar una funció fins que, o llevat que, hi hagi un error
indesitjat. Aquí hem d'anar en compte, doncs amb aquestes comandes podem
fer que una funció segueixi aplicant-se malgrat hi hagi un error, el que
podria provocar mals resultats o fins i tot mal funcionament de
l'ordinador.

La sintaxi és la següent: tot el que hi ha entre el try i un except
s'executa, excepte si es compleix el except, que ha de ser un tipus
d'error (o res, el que no és gens recomanable, ja que no faria cas de
cap tipus d'error, i no hi hauria manera fàcil de parar l'execució de la
funció externament).

Per exemple, imaginem que volem avaluar la funció $\sin(x)/x$, que està
ben definida per a tot $x$, ja que per a $x=0$ el seu limit val $1$.
Però si posem $sin(x)/x$ al [**SageMath**]{style="color: blue"} ens dona
un error de divisió per zero. Una possibilitat podria ser definir la
següent funció: ``

::: list

def sinxpartitperx(a):

try:

f=(sin(a)/a).n()

return f

except ZeroDivisionError:

return 1
:::

El que fa la funció és provar d'avaluar $sin(x)/x$, i, si pot, retorna
el resultat, però si té un error `ZeroDivisionError` (de dividir per
zero), llavors dóna $1$.

# Mòduls

Un mòdul és un fitxer que conté definicions i sentències de Python que
es poden utilitzar en d'altres programes Python. Hi ha molts mòduls de
Python que formen part de la biblioteca estàndard de Python.

Els mòduls s'han de carregar si es volen utilitzar en un programa de
Python. La instrucció per a fer-ho és `import`. El mòdul contindrà
moltes funcions, i si es vol es pot importar sols una funció, utilitzant
`from` [mòdul]{.underline} `import` [funció]{.underline}. Les funcions
de dins del mòdul s'utilitzen posant
[mòdul]{.underline}.[funció]{.underline}. Una pràctica força usual és
abreujar el nom del mòdul (o canviar-lo), que es pot fer posant `import`
[mòdul]{.underline} ` as ` [nou nom]{.underline}.

Un mòdul important de conèixer és **time**, que permet comptar el temps
d'execució d'un procés. Per exemple, si volem veure quants segons triga
a executar-se una funció podem fer

``

::: list

from time import perf_counter

inici = perf_counter()

lamevafuncio()

final = perf_counter()

temps_execucio = (final - inici)

temps_execucio
:::

També es pot fer amb perf_counter_ns per comptar el temps en nanosegons.
Això ho podem utilitzar per a veure quina de dues opcions que tinguem
per a fer un càlcul triga menys en executar-se.

Un mòdul que també es pot utilitzar per a mirar el temps d'execució
d'una funció és `timeit`. El que fa es executar moltes (les que el
usuari li indiqui) vegades el que li demanes i en calcula el temps
mitjà.

Es recomanable conèixer alguns dels mòduls més utilitzats, tot i que
alguns no ens caldran normalment si utilitzem
[**SageMath**]{style="color: blue"} , com la majoria de mòduls de
matemàtiques (math, numpy, etc).

# Classes

Una classe (a Python) és una manera de poder agrupar dades i funcions
alhora, creant un nou tipus d'objecte i les formes de manipular-lo (i
per tant forma part del que s'anomena programació orientada objectes).
El [**SageMath**]{style="color: blue"} i el Python estan escrits, de
fet, en el llenguatge de la programació orientada a objectes. Ja hem
vist molts exemples en els capítols anteriors: per exemple, els enters
de Sage són una classe, i tenen molts atributs i mètodes associats com
.factors(), .is_prime(), etc. Un altre exemple que hem vist és la classe
de les matrius, la classe dels vectors, i tants d'altres.

En aquesta secció veurem com podem crear nosaltres mateixos una classe
(senzilla), i com podem assignar-los atributs i mètodes amb ella.

El que farem serà crear la classe dels triangles en el pla. Primer de
tot hem de pensar com donarem un triangle. Hem optat per a definir-lo
com un conjunt de tres punts del pla $\mathbb{R}^2$ (això inclourà els
triangles "degenerats" formats per tres punts alineats, però en principi
no ens causarà problemes).

Així un triangle es crearà donant tres punts, com

`T1=Triangle([0,0],[0,12],[16,12])`

Per a definir una classe posem `class` i el nom de la classe. Per
exemple, per a definir la classe `Triangle` utilitzarem `class`
`Triangle`. Després cal inicialitzar la classe amb `__init__` cal dir el
nom que utilitzarem internament a la classe (tradicionalment s'utilitza
self, però no caldria!), i les dades que les defineixen (els tres
punts).

Dins de la mateixa classe podem definir funcions aplicades al objecte.
Per exemple, en aquest cas la funció `area` calcula l'àrea del triangle
donat pels tres vèrtexs. ``

::: list

class Triangle:

def \_\_init\_\_(self, punt1,punt2,punt3):

self.\_\_p1 = punt1

self.\_\_p2 = punt2

self.\_\_p3 = punt3

self.vertexs=\[punt1,punt2,punt3\]

def area(self):

p1=self.\_\_p1

p2=self.\_\_p2

p3=self.\_\_p3

p123=(p2\[0\]-p1\[0\])\*(p3\[1\]-p1\[1\])

p132=(p2\[1\]-p1\[1\])\*(p3\[0\]-p1\[0\])

area=abs((p123-p132)/2)

return(area)
:::

Varies observacions: les variables `__p1`, `__p2` i `__p3` són variables
privades (la manera com li diem a Python que una variable és privada és
amb el doble guió baix al davant). Per tant no es poden cridar des de
fora. La variable `vertexs` és pública, i es pot cridar amb el
`nom.vertexs`.

Vegeu un exemple de creació d'un triangle.

``

::: list

T1=Triangle(\[0,0\],\[0,12\],\[16,12\])

print(T1.area())

print(T1.vertexs)
:::

El que hem fet ha estat crear un triangle format pels vèrtexs a $(0,0)$,
$(0,12)$ i $(16,12)$, i hem calculat l'àrea. També li hem demanat qui
són els punts.

A part de calcular l'àrea podem incorporar moltes altres funcions. Per a
fer-ho primer crearé una funció per a calcular la distància entre dos
punts de $\mathbb{R}^n$ (escrits com una llista formada per dues llistes
de nombres reals).

``

::: list

def distancia(ps):

return((sum((x-y)\*\*2 for x,y in zip(\*ps))\*\*0.5))
:::

Observeu que enlloc d'utilitzar $^\wedge$ per elevar a $2$ he utilitzat
$**$; tot i que en [**SageMath**]{style="color: blue"} té el mateix
efecte, la segona opció és com ho fa el Python i dóna menys problemes de
compatibilitat.

També utilitzat la funció zip del Python que agafa dues llistes i les
converteix en una llista de parelles, formades per l'element enèsim de
cada llista. Concretament, la funció zip s'aplica a $n$ llistes o tuples
o cadenes, i en fa un iterable de $n$-tuples, agafant el ièssim de cada
llista, des de i=0 fins que s'acaba alguna tupla.

Per exemple, ``

::: list

print(\[x for x in zip(\[1,2\],\[3,4\])\])
:::

respon ``

::: list

$[(1, 3), (2, 4)]$
:::

Si apliques zip(\*llista) on llista és una llista, el que fa és
"esborrar" els parèntesis de la llista per poder fer-li el zip.
(S'utilitza per desfer un zip, per exemple.)

``

::: list

print(\[a for a in zip(\*\[(1, 1, 2), (2, 3, 4)\])\])
:::

respon

``

::: list

$[(1, 2), (1, 3), (2, 4)]$
:::

``

::: list

class Triangle:

def \_\_init\_\_(self, punt1,punt2,punt3):

self.\_\_p1 = punt1

self.\_\_p2 = punt2

self.\_\_p3 = punt3

self.vertexs=\[punt1,punt2,punt3\]

def area(self):

p1 = self.\_\_p1

p2 = self.\_\_p2

p3 = self.\_\_p3

p123 = (p2\[0\]-p1\[0\])\*(p3\[1\]-p1\[1\])

p132 = (p2\[1\]-p1\[1\])\*(p3\[0\]-p1\[0\])

area = abs((p123-p132)/2)

return(area)

def costats(self):

p = self.vertexs

pp = \[\[a for a in p if a!=b\] for b in p\]

return sorted(\[distancia(ps) for ps in pp\])

def perimetre(self):

return sum(self.costats())

def baricentre(self):

p=self.vertexs

baricentre=\[sum(a)/3 for a in zip(\*p)\]

return(baricentre)

def inradi(self):

return(2\*self.area()/self.perimetre())

def circumradi(self):

return(prod(self.costats())/(4\*self.area()))
:::

Ara podem crear com abans un triangle i fer que calculi els costats (la
llista ordenada de les longituds de cada costat), el baricentre, o el
circumradi, de manera obvia.

``

::: list

T1=Triangle(\[0,0\],\[0,12\],\[16,12\])

print(T1.area())

print(T1.costats())

print(T1.baricentre())

print(T1.circumradi())
:::

Una de les propietats interessants de les classes en Python és
l'herència. Així, un pot definir una classe a partir d'una classe
prèviament definida. Per exemple, podem definir una nova classe dels
triangles rectangles donant només els dos catets (que situarem amb un
vèrtex a l'origen).

``

::: list

class TriangleRectangle(Triangle):

def \_\_init\_\_(self, catet1,catet2):

Triangle.\_\_init\_\_(self,\[0,0\], \[0,catet1\], \[catet2,catet1\])

self.catet1=catet1

self.catet2=catet2

def hipotenusa(self):

return((self.catet1\*\*2+self.catet2\*\*2)\*\*0.5)
:::

Observeu que la funció `hipotenusa` només estarà definida si el triangle
està definit com a triangle rectangle.

``

::: list

T1=Triangle(\[0,0\],\[0,12\],\[16,12\])

T2=TriangleRectangle(12,16)

print(T2.vertexs)

\# Resposta \[\[0,0\],\[0,12\],\[16,12\]\]

print(T1.vertexs==T2.vertexs)

\# Resposta True

print(T2.hipotenusa())

\# Resposta 20.0

print(T1.hipotenusa())

\# Dona un error dient que no l'objecte Triangle no te l'atribut
hipotenusa
:::

Podeu veure que tot i que $T1$ i $T2$ els dos són triangles definits
pels mateixos punts, un té un atribut que no té l'altre, i per tant no
són el mateix.

Per definició dos objectes són iguals si estan formats per les mateixes
dades. Si volem donar una altra definició d'igualtat dins d'una classe
(el que de fet vindria a ser matemàticament com definir una relació
d'equivalència en el conjunt de les classes donades), es pot fer
expressament definint \_\_eq\_\_ (i \_\_ne\_\_ per a diferent).

Per exemple, podem voler considerar que dos triangles són el mateix
triangle si coincideixen al moure un a sobre de l'altre (i girar-lo o
fer el simètric si cal). Això és equivalent a definir la igualtat entre
triangles si tenen els mateixos costats (com a llista ordenada de menor
a major). Es por fer afegint això a la definició de la classe triangle:

``

::: list

def \_\_eq\_\_(self, other):

return self.costats() == other.costats()

def \_\_ne\_\_(self, other):

return not self.\_\_eq\_\_(other)
:::

Llavors si posem

``

::: list

T1=Triangle(\[0,0\],\[0,12\],\[16,12\])

T11=Triangle(\[1,0\],\[1,12\],\[17,12\])

print(T1 == T11)

#Resposta True
:::

# Referències

Si voleu tenir més informació sobre programació amb Python podeu
consultar

[How to Think Like a Computer Scientist: Learning with Python
3](http://openbookproject.net/thinkcs/python/english3e/) de Peter
Wentworth, Jefrey Elkner, Allen B. Downey i Chris Meyers.

Hi ha una versió (molt versionada) en català

[Introducció a la
programació](https://ocwitic.epsem.upc.edu/assignatures/inf/Apunts/introduccio-a-la-programacio/at_download/file)

Jefrey Elkner, Allen B. Downey, Chris Meyers\
Antoni Soto-Riera, Marc Vigo-Anglada, Sebastià Vila-Marta, Jordi Vives

# Exercicis {#exercicis .unnumbered}

1.  Feu una funció de Sage de manera que, si li passem una funció $f(x)$
    d'una variable, valors $a$ i $b$ reals, amb $a<b$, i un nombre de
    passos $n\ge 1$, divideixi el interval $[a,b]$ en $n$ intervals
    iguals, i retorni una llista amb elements $(t,f(t))$ on $t$ es mou
    de $a$ a $b$ (inclosos) passant pels extrems dels intervals
    ordenadament, i, aquí hi ha la dificultat, si no pot avaluar la
    funció, es salti el valor. Indicació: useu try except amb el error
    ValueError.

2.  Comproveu, amb l'ús del mòdul time, quina de les dues maneres
    d'ordenar una llista de nombres a l'atzar és més ràpida. Una,
    convertint la llista en conjunt i tornant-la a convertit en llista.
    L'altra, fent una funció que fa una copia de la llista, i creant una
    nova llista escollint el mínim i traient tots els elements de la
    copia de la llista repetits, fins que la llista sigui buida.

3.  Donats dos vectors $u=(u_1,\dots,u_n)$ i $v=(v_1,\dots,v_n)$ de
    $\mathbb{R}^n$, diem que $u\le v$ en l'ordre lexicogràfic, si, o bé
    són iguals, o bé $u_1<v_1$, o bé existeix un $i\le n$ tal que
    $u_j=v_j$ per a tot $j<i$ i $u_i<v_i$. Feu una funció `ordlex(u,v)`
    que comprovi que $u$ i $v$ són vectors de la mateixa llargada, i si
    no ho són doni un error 'TypeError', i si ho són retorni True si
    $u\le v$ en l'ordre lexicogràfic, i si no retorni False.

4.  Definiu una funció tal que, donades dues parelles de punts diferents
    del pla $\mathbb{R}^2$, $\{p_1,p_2\}$ i $\{q_1,q_2\}$, determini si
    el segment obert $r_1$ entre la primera parella talla o no el
    segment obert $r_2$ entre la segona parella. La funció ha d'acceptar
    com a dades dos conjunts formats per dos elements cadascun, i els
    elements han de ser punts de $\mathbb{R}^2$ (com a llistes, o com a
    tuples, etc). La resposta he de ser True si tallen, False si no.

    Indicació: Per a fer-ho podeu utilitzar que els punts del segment
    obert que uneix dos punts del pla $p_1$ i $p_2$ són els de la forma
    $tp_1+(1-t)p_2$, on $0< t <1$. Per tant, si tenim ara una altre
    parella de punts $p_3$ i $p_4$, volem comprovar si hi ha o no
    $0<s,t<1$ de manera que $$tp_1+(1-t)p_2=sp_3+(1-s)p_4.$$ Utilitzant
    la regla de Cramer això es tradueix a una desigualtat entre
    determinants: el determinant $A$ de la matriu que té com a columnes
    (o files) $p_1-p_2$ i $p_4-p_3$ ha de ser diferent de $0$ (per tal
    que no siguin parał.els o coincidents), i, si denotem per $B$ el
    determinant de la matriu que té com a columnes (o files) $p_4-p_2$ i
    $p_4-p_3$ i per $C$ el mateix però amb columnes (o files) $p_1-p_2$
    i $p_4-p_2$, llavors $$0<\frac{B}{A}<1 \text{ i } 0<\frac{C}{A}<1$$
    (doncs aquests quocients corresponen a la $t$ i la $s$ de la
    equació).

5.  Definiu una classe Isosceles, formada per triangles isòsceles donats
    per la base i l'altura, definida a partir de la classe Triangle
    definida a dalt.

6.  Definiu una classe dels Quadrilàters (convexos), determinada donant
    $4$ punts del pla.

    Observeu primer que si els quadrilàters no són convexos aleshores
    els vèrtexs no determinen el quadrilàter.

    ![Tres quadrilàters no convexos diferents amb els mateixos
    vèrtexs](quadnconvex1.png){#fig:Noconvex width="\\textwidth"}

    ![Tres quadrilàters no convexos diferents amb els mateixos
    vèrtexs](quadnconvex2.png){#fig:Noconvex width="\\textwidth"}

    ![Tres quadrilàters no convexos diferents amb els mateixos
    vèrtexs](quadnconvex3.png){#fig:Noconvex width="\\textwidth"}

    Per tant el primer que hem de fer és comprovar si els 4 punts formen
    o no un quadrilàter convex, i a més ordenar bé els vèrtexs. Tot això
    ho podem fer usant que en un quadrilàter convex els segments
    determinats pels vèrtexs oposats es tallen en un punt
    (necessàriament a l'interior del quadrilàter).

    ![Quadrilàter convex amb les diagonals
    dibuixades](quadconvex.png){width="\\textwidth"}

    ![El mateix però amb els vèrtexs mal
    posats](quadconvexn.png){width="\\textwidth"}

    []{#fig:Quadrilater label="fig:Quadrilater"}

    La classe pot tenir una funció àrea, i funcions que comprovin si el
    quadrilàter és un trapezi, si és un parał.lelogram, si és un rombe,
    si és un rectangle, si és un quadrat, etc. També podeu provar de
    definir igualtat de quadrilàters de manera anàloga a com ho hem fet
    pels triangles però no és fàcil.
