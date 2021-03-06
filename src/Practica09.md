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

# Complements a la programació en Python


## Més sobre les funcions de Python

Recordeu que una funció ben definida en Python hauria de tenir un
docstring, o sigui un text explicatiu, que descrigui què fa la funció.
És estàndard escriure una frase a la línia després de def amb tres `"` o `'`
a cada banda. Si té més d'una línia es posen tres `"` o `'` en una línia a part.

Per a definir una funció de Python amb `def` podeu, si voleu, posar
algunes entrades (o paràmetres) opcionals, amb uns valors predeterminats
en el cas que no hi siguin. Això es fa escrivint $(variable=valor)$ en
la definició. Tots aquests paràmetres han d'anar al final de la llista
de paràmetres.

Per exemple, imagineu que a la funció `ICos` de la Pràctica 5, en que es
donava l'origen, la precisió i el màxim d'iteracions, voleu que, si no
dieu res, per defecte es facin $10$ iteracions. Podem redefinir-la de la
següent manera:

```sage
reset()
def ICos(origen,precisio,maxit=10):
    s=origen.n()
    sa=s+1
    k=0
    print(30 * '*')
    while abs(s-sa) > precisio and k < maxit:
        k += 1; sa = s; s = cos(s)
        show(f'Iteracio {k}, {s = }')
    if k==maxit:
        show("S'ha arribat al maxim d'iteracions previstes!!!")
        show("No es segur que el valor sigui prou ajustat")
        show("El resultat correspon a l'ultim valor obtingut.")
    else:
        show(f"Despres de {k} iteracions,")
        show("les dues ultimes aproximacions difereixen en ")
        show(f"{abs(s-sa).n(digits=3)}, i són:")
        show(f'{sa}, {s}')
    print(30 * '*')
    return s
```

Vegem com funciona:
```sage
valor = ICos(pi / 3, 0.00001)
```

També podem executar:
```sage
valor = ICos(pi/3,0.00001,30)
```
Observeu també com hem escrit el "docstring" al inici.

Una comanda més que es pot utilitzar amb les funcions enlloc del
`return` és el `yield`: així com amb el `return`, un cop la funció
retorna un valor oblida tot el que ha passat dins de la funció, amb el
`yield` la funció recorda els valors que tenia, de manera que quan la
tornes a cridar recorda exactament on estava executant i amb quins
valors. De fet la funció amb un yield el que retorna és un iterador.

Anem a veure un exemple: farem una funció que retorni els nombres
primers acabat en 1 entre $n$ i $m$, però enlloc de ser una llista serà
un iterador (he utilitzat `xsrange()` per assegurar que el
**SageMath** em retorni un iterador):

```sage
def primers_acabats_en_1(n,m):
    for p in xsrange(n+1,m):
        if p.is_prime() and (p%10)==1:
            yield p 
```

```sage
it = primers_acabats_en_1(10,30)
next(it)
```

```sage
next(it)
```

us retorna 11, però al posar `next(it)` de nou us diu `StopIteration` ja
que no hi ha més primers i ha arribat al final del iterador. Podeu usar
aquest iterador en un `for`, fins i tot posant nombres molt grans, amb la
seguretat que no crearà una llista massa gran de nombres, i només
recorda el procediment (la funció) per a trobar el següent valor.


En general, es recomanable que una funció (acabada) de Python

1.  Tingui una breu descripció de què fa.

2.  Se li passi com a paràmetres tots els valors que necessitarà (no ha
	de fer servir variables *globals*)

3.  Retorni el o els valors que vulguem (amb un return o similar): es
    diu que la funció és productiva.

4.  No demani cap dada l'usuari amb un input.

5.  No imprimeixi cap missatge entremig (a menys que siguin missatges
    d'error).

Es poden posar `print`s en mig d'una funció mentre s'està programant per a
veure si fa el que un vol, però no és recomanable fer-ho quan la funció
està acabada, o com a mínim no per defecte. En qualsevol cas, les
funcions haurien de retornar el que es demana per tal que després es
pugui usar en un programa.

## Control d'errors en Python

Una altra eina que hauríeu de conèixer de les funcions és com fer que
doni un error en el cas que no se li passin entrades adequades. Això
s'hauria de fer amb la comanda `raise`. Quan es troba un `raise` la funció para i envia un missatge d'error.
Aquest missatge es pot posar escrivint `TypeError` (si el tipus no és
l'adequat) o `ValueError` (si el valor no és el adequat), o altres tipus
d'errors (el tipus d'error és una cosa que decidiu vosaltres, però es bo
que sigui el que correspon).

Per exemple, en l'exercici 5 dels exercicis de consolidació es demanava
fer una funció `Goldbach(n)` de manera que donat un nombre enter de Sage
que sigui parell i més gran que 4 ens retorni una llista amb les
parelles de primers $(p,q)$ amb $p+q=n$ (i entendrem que $p\le q$). Per
a tal de assegurar que ens passen una dada adequada podem fer


```sage
def Goldbach(n):
    """ Retorna la llista de parelles de primers senars que sumen n """
    if type(n)! = Integer:
        raise TypeError('No és un enter de Sage')
    elif is_odd(n):
        raise ValueError(f'El nombre {n} és senar')
    elif n < 6:
        raise ValueError(f'El nombre {n} és menor que 6')
    return [(p,n-p) for p in prime_range(3, n//2 + 1) if (n-p).is_prime()]
```

```sage
Goldbach(12/2)
```

```sage
Goldbach(11)
```

```sage
Goldbach(4)
```

Fixeu-vos que per imprimir el nombre `n` en el error el que he fet ha
estat transformar-lo en una cadena (string) amb `str(n)`, i després hem
sumat les cadenes (que equival a posar-les una rere l'altra).

Una altra possibilitat que ens ofereix el Python és utilitzar el
`assert`: es tracta de demanar que una certa condició ha de ser
satisfeta per seguir amb la funció, sinó retorna un error (de `assert`).
Per exemple, el codi anterior es pot fer amb asserts com:

```sage
def Goldbacha(n):
    assert type(n) == Integer,'Ha de ser un enter de sage'
    assert is_even(n), f'El nombre {n} ha de ser parell'
    assert n >= 6, f'El nombre {n} ha de ser com a mínim 6'
    return [(p, n - p) for p in prime_range(3,n // 2 + 1) if (n-p).is_prime()]
```

```sage
Goldbacha(12/2)
```

```sage
Goldbacha(11)
```

```sage
Goldbacha(4)
```

Fixeu-vos que hem de posar la condició que s'ha de satisfer, (i no com
abans, que posàvem la que no es satisfà). Ara, si posem `Goldbach(12/2)`
ens respon `AssertionError: Ha de ser un enter de sage`, i la resta igual
amb un `AssertionError`. L'inconvenient és, per tant, que no et distingeix
el tipus d'error. En general, el `assert` només s'hauria de usar per
casos que un no espera que passin mai però que es vol assegurar per
evitar que el programa funcioni malament. En canvi el `raise` es pot
utilitzar per casos que es poden donar i així avisar de què ha passat a
l'usuari. Això es relaciona directament amb el `try / except` que ara
comentarem.

## Try i except

Finalment explicarem l'us de try i except, que justament són molt útils
a l'hora de programar una funció. La idea és que ens concentrem en el
funcioament normal, i tractem a part els casos excepcionals (que poden
ser errors o no).

La sintaxi és la següent: tot el que hi ha entre el `try` i un `except`
s'executa. Si hi ha alguna *excepció* (el que abans en dèiem *errors*)
aleshores **Python** va executa bloc `except` corresponent si existeix
(si no existeix, aleshores retorna l'error com seria habitual). Si no
escrivim cap tipus d'error després d'`except` aleshores s'intercepten
**totes** les exepcions, fins i tot la que es produeix quan intentem
aturar l'execució mitjançant *Ctrl + C*.


Per exemple, imaginem que volem avaluar la funció $\sin(x)/x$, que la
podem pensar com a
ben definida per a tot $x$, ja que per a $x=0$ el seu limit val $1$.
Però si executem $sin(0) / 0$ el **SageMath** ens dona
un error de divisió per zero. Una possibilitat podria ser definir la
següent funció:


```sage
def sinxpartitperx(a):
    try:
        f = (sin(a) / a).n()
        return f
    except ZeroDivisionError:
        return 1
```

```sage
sinxpartitperx(1)
```

```sage
sinxpartitperx(0)
```

El que fa la funció és provar d'avaluar $sin(x) / x$, i, si pot, retorna
el resultat, però si té un error `ZeroDivisionError` (de dividir per
zero), llavors retorna $1$.

## Mòduls

Un mòdul és un fitxer que conté definicions i sentències de **Python** que
es poden utilitzar en d'altres programes **Python**. Hi ha molts mòduls de
**Python** que formen part de la biblioteca estàndard de **Python**, i
que tenim disponibles des de **SageMath** (també en podem instal·lar d'altres, si ens cal).

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

```sage
from time import perf_counter
```

```sage
inici = perf_counter()
G = Goldbach(100000)
final = perf_counter()
temps_execucio = final-inici
print(temps_execucio)
```

Tot i que també es pot fer servir:
```sage
%time G = Goldbach(100000)
```

Ara farem amb el mateix però amb una implimentació diferent

```sage
def Goldbach2(n):
    """ Retorna la llista de parelles de primers senars que sumen n """
    if type(n)! = Integer:
        raise TypeError('No és un enter de Sage')
    elif is_odd(n):
        raise ValueError(f'El nombre {n} és senar')
    elif n < 6:
        raise ValueError(f'El nombre {n} és menor que 6')
    return [(p,n-p) for p in xsrange(3,n//2+1) if p.is_prime() and (n-p).is_prime()]
```

```sage
inici = perf_counter()
G = Goldbach2(100000)
final = perf_counter()
temps_execucio2 = final - inici
print(temps_execucio2)
```

Observeu que aquesta triga més del doble.

```sage
print(temps_execucio2 / temps_execucio)
```

També es pot fer amb `perf_counter_ns` per comptar el temps en nanosegons.
Això ho podem utilitzar per a veure quina de les opcions que tinguem
per a fer un càlcul triga menys en executar-se.

Un mòdul que també es pot utilitzar per a mirar el temps d'execució
d'una funció és `timeit`. El que fa es executar moltes (les que el
usuari li indiqui) vegades el que li demanes i en calcula el temps
mitjà.

Es recomanable conèixer alguns dels mòduls més utilitzats, tot i que
alguns no ens caldran normalment si utilitzem
**SageMath**, com la majoria de mòduls de
matemàtiques (`math`, `numpy`, etc).

## Classes


### Preliminars: zip


La funció zip s'aplica a n llistes o tuples o cadenes, i en fa un iterable de n-tuples, agafant el ièssim de cada llista, des de i=0 fins que s'acaba alguna tupla.

```sage
print([x for x in zip([1,2],[3,4])])
```

```sage
print([x for x in zip([1,2,5],[3,4])])
```

```sage
print([x for x in zip([1,2],[3,4],[5,6])])
```

```sage
print([x for x in zip('abc',[1,2,3])])
```

Si apliques zip(*llista) on llista és una llista, el que fa és "esborrar" els parentesis de la llista per poder fer-li el zip. (S'utilita per desfer un zip, per exemple.) Fixeu-vos la diferència entre fer un zip amb * o sense *

```sage
print([a for a in zip(*[(1, 1, 2), (2, 3, 4)])])
```

```sage
print([a for a in zip((1, 1, 2), (2, 3, 4))])
```


Una classe (a Python) és una manera de poder agrupar dades i funcions
alhora, creant un nou tipus d'objecte i les formes de manipular-lo (i
per tant forma part del que s'anomena programació orientada objectes).
El **SageMath** i el **Python** són, de
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

Així un triangle es crearà donant tres punts, com `T1 = Triangle([0,0],[0,12],[16,12])`.

Per a definir una classe posem `class` i el nom de la classe. Per
exemple, per a definir la classe `Triangle` utilitzarem `class`
`Triangle`. Després cal inicialitzar la classe amb `__init__` cal dir el
nom que utilitzarem internament a la classe (tradicionalment s'utilitza
self, però no caldria!), i les dades que les defineixen (els tres
punts).

Dins de la mateixa classe podem definir funcions aplicades al objecte.
Per exemple, en aquest cas la funció `area` calcula l'àrea del triangle
donat pels tres vèrtexs. ``

## Una classe per treballar amb triangles

Considerem el següent codi:
```sage
class Triangle:
    def __init__(self, punt1,punt2,punt3):
        self._p1 = punt1
        self._p2 = punt2
        self._p3 = punt3
        self.vertexs = (punt1, punt2, punt3)

    def baricentre(self):
        return (sum(a)/3 for a in zip(self.vertexs))
```

```sage
T = Triangle((0,0), (0,12), (16,12))
show(f'{T.vertexs = }')
show(f'{T.baricentre() = }')
```

El que hem fet ha estat crear un triangle format pels vèrtexs a $(0,0)$,
$(0,12)$ i $(16,12)$, i hem calculat el seu baricentre.


A part de calcular l'àrea podem incorporar moltes altres funcions. Per a
fer-ho primer crearem una funció per a calcular la distància entre dos
punts de $\mathbb{R}^n$:


```sage
def distancia(P, Q):
    '''Calcula la distància entre dos punts de R^n'''
    return((sum((xi-yi)**2 for xi, yi in zip(P, Q))**0.5))
```

```sage
distancia((1,2),(2,3))
```

Ara definirem de nou la classe dels `Triangle`:


```sage
class Triangle:
    def __init__(self, punt1,punt2,punt3):
        self._p1 = punt1
        self._p2 = punt2
        self._p3 = punt3
        self.vertexs = (punt1, punt2, punt3)

    def area(self):
        p1 = self._p1
        p2 = self._p2
        p3 = self._p3
        p123 = (p2[0] - p1[0]) * (p3[1] - p1[1])
        p132 = (p2[1] - p1[1]) * (p3[0] - p1[0])
        return abs((p123-p132) / 2)

    def costats(self):
        p = self.vertexs
        pp = [[a for a in p if a != b] for b in p]
        return sorted([distancia(*a) for a in pp])

    def perimetre(self):
        return sum(self.costats())

    def baricentre(self):
        p = self.vertexs
        return (sum(a)/3 for a in zip(*p))

    def inradi(self):
        return 2 * self.area() / self.perimetre()

    def circumradi(self):
        return prod(self.costats()) / (4 * self.area())

    def __eq__(self, other):
        return self.costats() == other.costats()

    def __ne__(self, other):
        return not self.__eq__(other)
```

```sage
T1 = Triangle([0,0],[0,12],[16,12])
show(f'{T1.vertexs = }')
show(f'{T1.costats() = }')
show(f'{T1.perimetre() = }')
show(f'{T1.area() = }')
show(f'{T1.baricentre() = }')
show(f'{T1.intradi() = }')
show(f'{T1.circumradi() = }')
```

Hem definit també igualtat de triangles; comprovem-ho amb un triangle traslladat

```sage
T11 = Triangle([1,0],[1,12],[17,12])
T11 == T1
```


Una de les propietats interessants de les classes en Python és
l'herència. Així, un pot definir una classe a partir d'una classe
prèviament definida. Per exemple, podem definir una nova classe dels
triangles rectangles donant només els dos catets (que situarem amb un
vèrtex a l'origen).


Definim triangle rectangle com una classe a partir de la classe dels triangles

```sage
class TriangleRectangle(Triangle):
    def __init__(self, catet1, catet2):
        Triangle.__init__(self,(0,0), (0,catet1), (catet2,catet1))
        self._catet1 = catet1
        self._catet2 = catet2
    def catets(self):
	    return self._catet1, self._catet2
    def hipotenusa(self):
	    c1, c2 = self.catets()
        return (c1**2 + c2**2)**.5
```

```sage
T2 = TriangleRectangle(12,16)
print(T2.vertexs)
```

```sage
print(T1.vertexs == T2.vertexs)
```

```sage
T1 == T2
```

```sage
print('Hipotenusa =', T2.hipotenusa())
print('Hipotenusa =', T1.hipotenusa())

```

Tot i que tenen els mateixos vèrtexs, i de fet són iguals (com a triangles), el triangle T1 no té definida la hipotenusa, ja que no està definit com a triangle rectangle.



### Observacions

- Les variables `_p1`, `_p2` i `_p3` són variables
privades (no s'haurien de fer servir fora de la classe), tot i que a **Python** es considera que tothom és adult i sap què fa, així que no estan amagades del tot. Les podem fer "més privades" amb un doble guió baix (`__p1`, etc) i aleshores el seu nom real canvia a `__Triangle_p1`, fet que les fa més difícils d'utilitzar accidentalment.

- Per definició dos objectes són iguals si estan formats per les mateixes
dades. Si volem donar una altra definició d'igualtat dins d'una classe
(el que de fet vindria a ser matemàticament com definir una relació
d'equivalència en el conjunt de les classes donades), es pot fer
expressament definint `__eq__` (i `__ne__` per a diferent).

- Per exemple, podem voler considerar que dos triangles són el mateix
triangle si coincideixen al moure un a sobre de l'altre (i girar-lo o
fer el simètric si cal). Això és equivalent a definir la igualtat entre
triangles si tenen els mateixos costats (com a llista ordenada de menor
a major), que és el què hem implementat.



## Referències

Si voleu tenir més informació sobre programació amb Python podeu
consultar

[How to Think Like a Computer Scientist: Learning with Python
3](http://openbookproject.net/thinkcs/python/english3e/) de Peter
Wentworth, Jefrey Elkner, Allen B. Downey i Chris Meyers.

## Exercicis


### Exercici 1
Feu una funció de Sage de manera que, si li passem una funció $f(x)$
d'una variable, valors $a$ i $b$ reals, amb $a<b$, i un nombre de
passos $n\ge 1$, divideixi el interval $[a,b]$ en $n$ intervals
iguals, i retorni una llista amb elements $(t,f(t))$ on $t$ es mou
de $a$ a $b$ (inclosos) passant pels extrems dels intervals
ordenadament, i, aquí hi ha la dificultat, si no pot avaluar la
funció, es salti el valor. Indicació: useu try except amb el error
ValueError.

-- begin hide

Una possible solució: si no ho pot avaluar li dic que 'passi', amb un pass. 

```sage
def valorsf(f,a,b,n):
    valorsx=[a+i*(b-a)/n for i in range(n+1)]
    valorsf=[]
    for x in valorsx:
        try:
            valorsf.append((x,f.subs(x=x)))
        except ValueError:
            pass
    return valorsf
```

Exemple per la funció 1/x

```sage
valorsf(1/x,-2,2,4)
```

Podeu observar que per x=0 no l'ha avaluat

-- end hide

### Exercici 2


Comproveu, amb l'ús del mòdul time, quina de les dues maneres
d'ordenar una llista de nombres a l'atzar és més ràpida. Una,
convertint la llista en conjunt i tornant-la a convertit en llista.
L'altra, fent una funció que fa una copia de la llista, i creant una
nova llista escollint el mínim i traient tots els elements de la
copia de la llista repetits, fins que la llista sigui buida.


-- begin hide


```sage
reset()
```

Funció per ordenar primera

```sage
def ord2(llist):
    cllist = copy(llist)
    nllist = []
    while (len(cllist) > 0):
        a = min(cllist)
        nllist.append(a)
        while (a in cllist):
            cllist.remove(a)
    return nllist
```


Creem una llista a l'atzar de 1000 nombres del 1 al 30


```sage
llist = [randint(1,30) for i in range(1000)]
```

Importem el mòdul time 

```sage
from time import perf_counter
```

Ordenem la llista amb la primera funció comptant el temps

```sage
inici = perf_counter()
print (ord1(llist))
final = perf_counter() 
print(final-inici)
```

El mateix amb el segon mètode

```sage
inici = perf_counter()
print (ord2(llist))
final = perf_counter() 
print(final-inici)
```

-- end hide


### Exercici 3


Donats dos vectors $u=(u_1,\dots,u_n)$ i $v=(v_1,\dots,v_n)$ de
$\mathbb{R}^n$, diem que $u\le v$ en l'ordre lexicogràfic, si, o bé
són iguals, o bé $u_1<v_1$, o bé existeix un $i\le n$ tal que
$u_j=v_j$ per a tot $j<i$ i $u_i<v_i$. Feu una funció `ordlex(u,v)`
que comprovi que $u$ i $v$ són vectors de la mateixa llargada, i si
no ho són doni un error `TypeError` i si ho són retorni `True` si
$u\le v$ en l'ordre lexicogràfic, i si no retorni `False`.


-- begin hide
Una possible manera. Ho he fet amb "llistes de nombres", no vectors

```sage
def ordlex(u,v):
    '''Retorna cert si u <= v en ordre lexicogràfic'''
    if type(u)!=list or type(v)!=list:
        raise TypeError('No són llistes de nombres')
    if len(u) != len(v):
        raise TypeError('No tenen la mateixa llargada')
    for i in range(len(v)):
        if u[i] < v[i]:
            return True
        if u[i] > v[i]: 
            return False
    return True
```

Observeu que es cumpleix el que es demana, ja que si $u[1]<v[1]$, llavors retorna True a la primera iteració, si $u[j]=v[j]$ per tot $j<i$ i $u[i]<v[i]$, llavors retorna True a la iteració número $j$, si fa totes les iteracions i surt del for és que $u=v$, i retorna True, i si no passa res d'això retorna False

```sage
u = [1,1,1,1]
v = [1,1,1,2]
```

```sage
ordlex(u,v)
```

```sage
u = [1,1,1,1]
v = [1,1,1,1]
ordlex(u,v)
```

```sage
u = [1,1,1]
v = [1,1,1,1]
ordlex(u,v)
```

```sage
u = (1,1,1,1)
v = [1,1,1,1]
ordlex(u,v)
```
-- end hide

### Exercici 4


Definiu una funció tal que, donades dues parelles de punts diferents
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

-- begin hide

He fet una funció que comprova si les dades són conjunts, si tenen dos elements, si cada elements té llargada 2 i després he convertit els "punts" a vectors de $\mathbb{R^2}$. He calculat els determinants i comprovo si $A=0$, després si tenen el mateix signe tots (amb la funció `sign`), i si els quocients són $\ge 1$, i si es compleix alguna d'elles la resposta és `False`, i sino la resposta és `True`.


```sage
def EsTallen(S,T):
    '''Donats dos conjunts de dos punts del pla,
    determina si les rectes que formen es tallen o no
    '''
    if type(S) != set or type(T) != set:
        raise TypeError('No són conjunts')
    if len(S) != 2 or len(T) != 2: 
        raise TypeError('Els conjunts no tenen dos elements')
    if any(len(v)!= 2 for v in S.union(T)):
        raise TypeError('Han de tenir dues coordenades')
    E = RR^2
    V = [E(v) for v in S] + [E(v) for v in T]
    A = matrix([V[0]-V[1],V[3]-V[2]]).det()
    B = matrix([V[3]-V[1],V[3]-V[2]]).det()
    C = matrix([V[0]-V[1],V[3]-V[1]]).det()
    if A == 0: 
        return False
    elif A.sign()!=B.sign() or A.sign()!=C.sign(): 
        return False
    elif B/A >= 1 or C/A >= 1:
        return False
    t = B/A
    return True
```

A més he fet una funció Tallen que retorni el punt de tall a més (o bé 0 si no)

```sage
def Tallen(S,T):
    '''Donats dos conjunts de dos punts del pla,
    determina si les rectes que formen es tallen o no
    '''
    if type(S) != set or type(T) != set:
        raise TypeError('No són conjunts')
    if len(S) != 2 or len(T) != 2: 
        raise TypeError('Els conjunts no tenen dos elements')
    if any(len(v)!= 2 for v in S.union(T)):
        raise TypeError('Han de tenir dues coordenades')
    E = RR^2
    V = [E(v) for v in S] + [E(v) for v in T]
    A = matrix([V[0]-V[1],V[3]-V[2]]).det()
    B = matrix([V[3]-V[1],V[3]-V[2]]).det()
    C = matrix([V[0]-V[1],V[3]-V[1]]).det()
    if A == 0: 
        return False, 0
    elif A.sign()!=B.sign() or A.sign()!=C.sign(): 
        return False, 0
    elif B/A >= 1 or C/A >= 1:
        return False, 0 
    t = B/A
    return True, t*V[0]+(1-t)*V[1]
```

Un exemple

```sage
S = {(2.1,2),(2.3,1)}
T = {(2.1,1),(2.3,2)}
b,pt = Tallen(S,T)
```

```sage
line([v for v in S])+line([v for v in T])+point(pt,color='red',size=30)
```

```sage
S = {(2.1,2),(2.3,2)}
T = {(2.1,1),(2.3,1)}
b,pt = Tallen(S,T)
b
```

```sage
line([v for v in S])+line([v for v in T])
```
-- end hide

### Exercici 5

Definiu una classe `Isosceles`, formada per triangles isòsceles donats
per la base i l'altura, definida a partir de la classe `Triangle`
definida a dalt.

-- begin hide
```sage
class TriangleIsosceles(Triangle):
    def __init__(self, base, altura):
        Triangle.__init__(self, (0, 0), (0, base), (altura, base/2))
        self.base = base
        self.altura = altura
    def area(self):
        return(self.base * self.altura)

T = TriangleIsosceles(10,10)
show(T.area())
```
-- end hide


### Exercici 6


Definiu una classe dels Quadrilàters (convexos), determinada donant
$4$ punts del pla.

Observeu primer que si els quadrilàters no són convexos aleshores
els vèrtexs no determinen el quadrilàter.

![Tres quadrilàters no convexos diferents amb els mateixos
vèrtexs](quadnconvex1.png)

![Tres quadrilàters no convexos diferents amb els mateixos
vèrtexs](quadnconvex2.png)

![Tres quadrilàters no convexos diferents amb els mateixos
vèrtexs](quadnconvex3.png)

Per tant el primer que hem de fer és comprovar si els 4 punts formen
o no un quadrilàter convex, i a més ordenar bé els vèrtexs. Tot això
ho podem fer usant que en un quadrilàter convex els segments
determinats pels vèrtexs oposats es tallen en un punt
(necessàriament a l'interior del quadrilàter).

![Quadrilàter convex amb les diagonals
dibuixades](quadconvex.png)

![El mateix però amb els vèrtexs mal
posats](quadconvexn.png)

La classe pot tenir una funció àrea, i funcions que comprovin si el
quadrilàter és un trapezi, si és un parał.lelogram, si és un rombe,
si és un rectangle, si és un quadrat, etc. També podeu provar de
definir igualtat de quadrilàters de manera anàloga a com ho hem fet
pels triangles però no és fàcil.

-- begin hide
Hi ha varis problemes tècnics a resoldre. Per exemple, he decidit que els punts siguin tuples, i per tant els transformo a tuples només començar. A més accepto com a quadrilater qualsevol llista de 4 punts, però a l'hora de obtenir els vèrtexs ordenats adequadament, si no determinen un quadrilater convex faig que retorni la llista buida. 


Podríem haver decidit que un quadrilàter és una llista de punts i si surt "degenerat" per haver agafat la llista mal ordenada llavors no tingués les altres instàncies. Però llavors hauríem de decidir que fer amb els quadrilàters no convexos. 


Primer he definit una funció similar a la de EsTallen per saber si els costats determinades per dos conjunts de dos punts són paral·lels o no. 

```sage
def SonParalels(S,T):
    '''
	Donats dos conjunts de dos punts del pla,
    determina si els costats que formen son paral·lels o no
    '''
    if type(S) != set or type(T) != set:
        raise TypeError('No són conjunts')
    if len(S) != 2 or len(T) != 2: 
        raise TypeError('Els conjunts no tenen dos elements')
    if any(len(v)!= 2 for v in S.union(T)):
        raise TypeError('Han de tenir dues coordenades')
    E = RR^2
    V = [E(v) for v in S] + [E(v) for v in T]
    A = matrix([V[0]-V[1],V[3]-V[2]]).det()
    return A == 0
```

```sage
class Quadrilater: 
    def __init__(self, punt1,punt2,punt3,punt4):
        self._p1 = tuple(punt1)
        self._p2 = tuple(punt2)
        self._p3 = tuple(punt3)
        self._p4 = tuple(punt4)
    def vertexs(self):
        V = [self._p1,self._p2,self._p3,self._p4]
        if EsTallen({V[0],V[2]},{V[1],V[3]}):
            return(V)
        elif EsTallen({V[0],V[1]},{V[2],V[3]}):
            return([V[0],V[2],V[1],V[3]])
        elif EsTallen({V[0],V[3]},{V[1],V[2]}):
            return([V[0],V[1],V[3],V[2]])
        else:
            return([])
    def EsConvex():
        V = self.vertexs()
        return len(V) != 0
    def costats(self):
        V = self.vertexs()
        if len(V)==0:
            return []
        C = [distancia([V[i],V[i+1]]) for i in range(3)] 
        C.append(distancia([V[3],V[0]]))
        return C
    def perimetre(self):
        return sum(self.costats())
    def baricentre(self):
        V = self.vertexs()
        if len(V)==0:
            return 
        b,pt = Tallen({V[0],V[2]},{V[1],V[3]})
        return pt
    def area(self):
        V = self.vertexs()
        if len(V)==0:
            return 0
        A1 = Triangle(V[0],V[1],V[2]).area()
        A2 = Triangle(V[0],V[3],V[2]).area()
        return A1+A2
    def trapezi(self): #Mirem si tenen dos costats oposats paral·lels
        V = self.vertexs()
        if len(V)==0:
            return False
        return SonParalels({V[0],V[1]},{V[2],V[3]}) or SonParalels({V[0],V[3]},{V[1],V[2]})
    # He fet dues instancies de paral·lelogram. Les dues van bé.
    def paralelogramc(self): #Mirem si tenen costats oposats iguals
        C = self.costats()
        if len(C)==0:
            return False
        return C[0]==C[2] and C[1] == C[3]
    def paralelogram(self):  #Mirem si tenen costats oposats paral·lels
        V = self.vertexs()
        if len(V)==0:
            return False
        return SonParalels({V[0],V[1]},{V[2],V[3]}) and SonParalels({V[0],V[3]},{V[1],V[2]})
    def rombe(self): #Mirem si tenen tots els costats iguals
        C = self.costats()
        if len(C)==0:
            return False
        return self.paralelogramc() and C[0] == C[1]        
    def rectangle(self): # Paral·lelogram que té dos costats perpendiculars
        V = self.vertexs()
        if len(V)==0:
            return False
        # Calculem el producte escalar dels dos costats amb vertex V[0]. Ha de ser 0.
        esc = (V[1][0]-V[0][0])*(V[3][0]-V[0][0])+(V[1][1]-V[0][1])*(V[3][1]-V[0][1])
        return self.paralelogram() and esc == 0
    def quadrat(self): #Un quadrat és un rectangle que és un rombe
        return self.rectangle() and self.rombe()
    # He fet una instància que fa un plot del quadrilàter per poder visualitzar el que fem 
    def plot(self):
        V = self.vertexs()
        if len(V)==0:
            return 
        pV = points(V,color='red',axes=False,aspect_ratio=1)
        lV = line(V[:2])
        lV += line(V[1:3])
        lV += line(V[2:])
        lV += line([V[3],V[0]])
        return pV+lV
    # Defineixo igualtat entre Quadrilaters si es poden descomposar en dos triangles iguals.
    # Trenquem el primer d'una manera, i el segon de les 2 possibles maneres
    # Cal comparar el primer triangle del primer quadrilater amb casacun dels 4 del segon
    # i el segon triangle del primer quadrilater amb el complementari del segon.
    def __eq__(self, other):
            V1 = self.vertexs()
            V2 = other.vertexs()
            T111 = Triangle(V1[0],V1[1],V1[2])
            T112 = Triangle(V1[1],V1[2],V1[3])
            T211 = Triangle(V2[0],V2[1],V2[2])
            T212 = Triangle(V2[1],V2[2],V2[3])
            T221 = Triangle(V2[0],V2[1],V2[3])
            T222 = Triangle(V2[1],V2[2],V2[3])
            bo1 = (T111 == T211) and (T112 == T212)
            bo2 = (T111 == T212) and (T112 == T211)
            bo3 = (T111 == T221) and (T112 == T212)
            bo4 = (T111 == T221) and (T112 == T222)
            return bo1 or bo2 or bo3 or bo4 
    def __ne__(self, other):
        return not self.__eq__(other)
```

Un quadrat

```sage
punt1 = (0,0)
punt2 = (0,1)
punt3 = (1,0)
punt4 = (1,1)
Q1 = Quadrilater(punt1,punt2,punt3,punt4)
```

```sage
Q1.vertexs()
```

```sage
Q1.area()
```

```sage
Q1.costats()
```

```sage
Q1.baricentre()
```

```sage
Q1.trapezi()
```

```sage
Q1.rombe()
```

```sage
Q1.paralelogram()
```

```sage
Q1.paralelogramc()
```

```sage
Q1.rectangle()
```

```sage
Q1.quadrat()
```

```sage
Q1.plot()
```

Un rectangle

```sage
punt1 = (0,0)
punt2 = (0,2)
punt3 = (1,0)
punt4 = (1,2)
Q1 = Quadrilater(punt1,punt2,punt3,punt4)
```

```sage
Q1.vertexs()
```

```sage
Q1.area()
```

```sage
Q1.costats()
```

```sage
Q1.baricentre()
```

```sage
Q1.trapezi()
```

```sage
Q1.rombe()
```

```sage
Q1.paralelogram()
```

```sage
Q1.paralelogramc()
```

```sage
Q1.rectangle()
```

```sage
Q1.quadrat()
```

```sage
Q1.plot()
```

Un paral·lelogram no rectangle

```sage
punt1 = (0,0)
punt2 = (1,1)
punt3 = (3,0)
punt4 = (4,1)
Q1 = Quadrilater(punt1,punt2,punt3,punt4)
```

```sage
Q1.vertexs()
```

```sage
Q1.area()
```

```sage
Q1.costats()
```

```sage
Q1.baricentre()
```

```sage
Q1.trapezi()
```

```sage
Q1.rombe()
```

```sage
Q1.paralelogram()
```

```sage
Q1.paralelogramc()
```

```sage
Q1.rectangle()
```

```sage
Q1.quadrat()
```

```sage
Q1.plot()
```

Un trapeci no paral·lelogram

```sage
punt1 = (0,0)
punt2 = (1,1)
punt3 = (3,0)
punt4 = (6,1)
Q1 = Quadrilater(punt1,punt2,punt3,punt4)
```

```sage
Q1.vertexs()
```

```sage
Q1.area()
```

```sage
Q1.costats()
```

```sage
Q1.baricentre()
```

```sage
Q1.trapezi()
```

```sage
Q1.rombe()
```

```sage
Q1.paralelogram()
```

```sage
Q1.paralelogramc()
```

```sage
Q1.rectangle()
```

```sage
Q1.quadrat()
```

```sage
Q1.plot()
```

Un de no convex

```sage
punt1 = (0,0)
punt2 = (1,2)
punt3 = (3,0)
punt4 = (1,1)
Q1 = Quadrilater(punt1,punt2,punt3,punt4)
points([punt1,punt2,punt3,punt4])
```

```sage
Q1.vertexs()
```

```sage
Q1.area()
```

```sage
Q1.costats()
```

```sage
Q1.baricentre()
```

```sage
Q1.trapezi()
```

```sage
Q1.rombe()
```

```sage
Q1.paralelogram()
```

```sage
Q1.paralelogramc()
```

```sage
Q1.rectangle()
```

```sage
Q1.quadrat()
```

```sage

```

Un rombe no quadrat

```sage
punt1 = (0,0)
punt2 = (3,4)
punt3 = (5,0)
punt4 = (8,4)
Q1 = Quadrilater(punt1,punt2,punt3,punt4)
```

```sage
Q1.vertexs()
```

```sage
Q1.area()
```

```sage
Q1.costats()
```

```sage
Q1.baricentre()
```

```sage
Q1.trapezi()
```

```sage
Q1.rombe()
```

```sage
Q1.paralelogram()
```

```sage
Q1.paralelogramc()
```

```sage
Q1.rectangle()
```

```sage
Q1.quadrat()
```

```sage
Q1.plot()
```

Un que no és "res" (de fet és un estel o deltoïde)

```sage
punt1 = (0,0)
punt2 = (3,4)
punt3 = (5,0)
punt4 = (6,3)
Q1 = Quadrilater(punt1,punt2,punt3,punt4)
```

```sage
Q1.plot()
```

```sage
Q1.vertexs()
```

```sage
Q1.area()
```

```sage
Q1.costats()
```

```sage
Q1.baricentre()
```

```sage
Q1.trapezi()
```

```sage
Q1.rombe()
```

```sage
Q1.paralelogram()
```

```sage
Q1.paralelogramc()
```

```sage
Q1.rectangle()
```

```sage
Q1.quadrat()
```

Algun exemple de comparació

```sage
punt1 = (0,0)
punt2 = (3,4)
punt3 = (5,0)
punt4 = (6,3)
Q1 = Quadrilater(punt1,punt2,punt3,punt4)
Q2 = Quadrilater(punt2,punt3,punt4,punt1)
```

```sage
Q1 == Q2
```

```sage
punt1 = (1,1)
punt2 = (5,4)
punt3 = (1,6)
punt4 = (4,7)
Q2 = Quadrilater(punt2,punt3,punt4,punt1)
```

```sage
Q1 == Q2
```

```sage
Q1.plot()+Q2.plot()
```
-- end hide
