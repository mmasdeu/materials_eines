---
jupyter:
  title : 'Pràctica 8: Aritmètica bàsica'
  authors: [ "name" : "Marc Masdeu", "name" : "Xavier Xarles" ]
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

# Aritmètica Bàsica

## Conjunts

El **SageMath** (i el Python en general) pot
treballar amb conjunts. La diferencia entre un conjunt i una llista o
una tupla és que en una llista o tupla els elements tenen un ordre i
poden estar repetits, i en canvi en un conjunt no. De fet el
**SageMath** té molta més llibertat en fer
llistes que conjunts, ja que accepta llistes amb elements molt dispars,
però en canvi els elements dels conjunts han de ser immutables (de fet el que han de ser és *hashables*, que és un concepte tècnic que no discutirem. Per exemple una tupla és *hashable* si els  seus membres ho són.).

La instrucció bàsica per a construir un conjunt és `set`, que pot tenir com
a argument des d'una llista, un tupla o un iterador, sempre que estiguin
formats per nombres, cadenes (*strings*) o tuples d'aquests elements (o
altres objectes immutables). També es pot construir un conjunt posant
entre claus (`{` i `}`) tots els elements del conjunt. Podeu usar
`a in A` per a demanar si un element `a` és dins d'un conjunt `A`. Però
(en principi) no podeu demanar el "primer" element d'un conjunt.


Per posar un exemple (estrany), anem a construir el conjunt que conté el
número 2, la variable x, el nombre $\pi$, el nombre real $3.2$ i el
símbol per l'anell dels enters.

```sage
A={2, x ,3.2, ZZ, pi}
show(A)
ZZ in A
```
```sage
A[0] # Us hauria de donar un error.
```

Fixeu-vos que `ZZ` és membre del conjunt (però no és un subconjunt seu).
Si al crear un conjunt posem elements repetits només sortiran una
vegada.

```sage
X = {1,2,5,5,6,1}
show(X)
```

Els conjunts es poden fer les operacions habituals, moltes d'elles com a
mètode (o sigui, amb el format `A.metode(B)`). Per exemple, unió és
`union`, intersecció és `intersection`, la diferencia és `difference`,
etc. També podeu demanar amb una funció el nombre d'elements (amb
`len`), i afeguir o treure un element donat (amb `remove` dona error si no
hi és, amb `discard` no fa res si no hi és). Finalment, per agafar un
element d'un conjunt ("a l'atzar") podeu usar `pop()`, però compte perquè això el treu del conjunt.

```sage
X.intersection(A)
```
```sage
X.union(A)
```
```sage
X.difference(A)
```
```sage
len(X)
```
```sage
X.add(3)
show(X)
```
```sage
X.remove(3)
show(X)
```
```sage
X.discard(3)
show(X)
```
```sage
X.pop()
```
```sage
show(X)
```


A l'hora de crear conjunts, podeu usar també la comprehensió, tal com
varem fer en les llistes. Per exemple, podem trobar el conjunt dels
residus mòdul 17 dels quadrats

```sage
{ a^2 % 17 for a in srange(17) }
```


El **SageMath** també té una altra construcció de conjunt,
`Set` (amb majúscula!) que permet treballar amb conjunts infinits, i amb
conjunts que contenen altres conjunts o llistes. Podem fer el conjunt de
tots els enters `ZZ`, els racionals `QQ`, els nombres primers
`Primes()`, etc.

```sage
Z = Set(ZZ)
show(Z)
```
```sage
2 in Z
```
```sage
pi in Z
```

Compte però que treballant amb conjunts infinits podeu provocar
fàcilment que us quedeu sense memòria.

## Diccionaris

Una construcció molt més general que la de conjunt i la de llista és la
de diccionari: un diccionari és com un conjunt de claus (*keys*, que
han de ser *hashables*) i a cada clau el seu valor. Podríem pensar que una
llista de llargada $n$ és com un diccionari on les claus són els nombres
de 0 a $n-1$, i que un conjunt és un diccionari on no mirem els valors
de les claus.


Una manera d'inicialitzar un diccionari és fent servir claus (`{` i `}`), i posant els parells *key* : *value* separats per comes. Per exemple, el següent diccionari

```sage
prova = { 1 : 'a' , 'x' : [1, 2], (4,5) : { 1, 2 } }
```


assigna al número $1$ la lletra $a$, a la lletra $x$ la llista $[1,2]$ i
a la tupla $(1,5)$ el conjunt $\{1,2\}$. Per accedir als valors només cal posar
`prova[1]` i respon `'a'`, i posar `prova['x']` i respon `[1,2]`, etc.

Les *keys* poden ser números, cadenes (strings), tuples de números o
de cadenes, però no poden ser ni llistes ni altres conjunts. En canvi als
valors s'hi pot posar qualsevol cosa.


Podem modificar el valor d'un diccionari com en el cas de les llistes.
Per exemple, si posem

```sage
prova[(4,5)] = {1,2,3}
print(prova)
```

ens imprimirà
$$\{ 1 \ : \  ' a' \  , \   'x'\ : \ [1,2] \ , \ (4,5) \ : \ \{1,2,3\} \}.$$


També podem afegir més elements a un diccionari:


```sage
prova[2] = 'z'
```

Si volem afegir tots els elements d'un altre diccionari, podem fer servir `update`. Per exemple


```sage
prova.update({2 : 'b', 7 : 'c'})
print(prova)
```

ens imprimirà
$$\{ 1 \ : \  ' a' \  , \   'x'\ : \ [1,2] \ , \ (4,5) \ : \ \{1,2,3\}, 2:'b', 7:'c' \}.$$
(i observem que ha sobreescrit el valor de $2$ anterior)


Si fem un bucle indexat en un diccionari, la variable es mou en la
llista de claus. D'aquesta manera, podem fer:

```sage
for k in prova:
	print(k, prova[k])
```

que ens imprimirà cada clau i el seu valor.

## Divisió entera

La majoria de programes de càlcul simbòlic, i en particular
**SageMath** quan realitzen divisions
entre nombres enters donen com a resultat el número racional
corresponent. Aquest comportament és el que resulta més pràctic en la
majoria de situacions però pels problemes específicament d'enters no
donen, en molts casos, els resultats que interessen. En primera
instància, quan es divideixen dos enters positius $a$ i $b$, el que cal
determinar és el seu quocient $q$ i el residu $r$ de la divisió. És a
dir, quan dividim $a$ entre $b$ busquem escriure $$a= q\times b+r$$ amb
$0\le r<|b|$. En alguna de les sessions anteriors ja ha aparegut, de
passada, l'operador que dóna la resta de la divisió entre dos enters
(que és `%`) i, en particular, es pot comprovar si un nombre enter `a`
és parell o senar comprovant si el valor de `a % 2` és igual a $0$ o a
$1$. Però aquesta mena de resultat es pot aplicar en qualsevol situació.
Per exemple:


```sage
124846 % 4
```

mostra que $124846$ és un nombre parell que no és múltiple de $4$.


Contrariament a la definició usual, el càlcul del residu mòdul un enter
qualsevol segueix el conveni de donar un resultat del mateix signe i
menor en valor absolut que el denominador com es pot comprovar en els
exemples següents.

```sage
(-7) % 4
```
```sage
7 % (-4)
```
```sage
(-7) % (-4)
```

A part de les aplicacions purament aritmètiques, la reducció mòdul un
enter pot ser molt útil en un exemple com el següent:

```
var('k')
colors = ['red', 'green', 'blue']
dibuix = plot(1,0,1, color=colors[0], aspect_ratio=1)
for k in [1..10]:
    dibuix += plot(x^k,x,0,1, color=colors[k%3],aspect_ratio=1)
dibuix
```

on s'obté el gràfic entre $0$ i $1$ de les potències successives de $x$
dibuixades en tres colors diferents que van canviant de forma
successiva.


Per un altre costat, l'altra part fonamental en una divisió és el
quocient corresponent. Per tal d'obtenir aquest valor
**SageMath** disposa d'un operador especial
dedicat a la *divisió entera* `//`. Podeu veure el seu funcionament en
els exemples següents


```sage
24 // 6
```
```sage
(-25) // 3
```
```sage
148 // (-5)
```
```sage
(-24) // (-6)
```


Noteu que, quan el numerador o el denominador de la divisió és negatiu,
els valors i els signes dels quocients i dels residus sempre es
corresponen per tal que el numerador sigui igual al producte del
quocient pel divisor més el residu.


En molts problemes de divisibilitat l'únic que interessa és saber si
l'enter $a$ divideix $b$ (o si $b$ és múltiple de $a$). En aquests casos
es disposa de l'opció `divides` que dóna com a resultat cert o fals
segons es verifiqui la condició o no:

```sage
4.divides(24)
```
```sage
4.divides(25)
```

```sage
23445567889.divides(12334455667789900023)
```

Un altre càlcul que apareix sovint és el de determinar tots els divisors
d'un nombre donat. Per tal d'obtenir aquest resultat la funció que fa
tota la feina és `divisors` com es pot veure en aquest exemple:

```sage
28.divisors()
```
```sage
divisors(28)
```

Utilitzant aquesta llista és molt fàcil definir una funció molt simple
que ens digui si un número qualsevol és *perfecte* (igual a la suma dels
seus divisors diferents d'ell mateix, és clar)

```sage
def esperfecte(k):
    sumdiv = sum(d for d in k.divisors())
	return sumdiv == 2 * k
```

Sabríeu fer, ara, una llista amb els números perfectes menors que 1000?

-- begin hide
```sage
[esperfecte(k) for k in [1..1000]]
```
-- end hide

Per acabar aquest apartat, si en un problema interessen igualment el
quocient i el residu de la divisió entre dos enters es poden obtenir les
dues dades amb `quo_rem()` que dona com a resultat un parell `(q,r)`
agrupats en un sol objecte:

```sage
567.quo_rem(6)
```
```sage
q, r = 567.quo_rem(6)
print(f'{q = }, {r = }')
```

## Màxim comú divisor i mínim comú múltiple

Amb les instruccions de l'apartat anterior (i alguna altra que veurem
tot seguit) ja n'hi ha prou per definir una funció que calculi, el màxim
comú divisor d'un parell d'enters qualsevol. Per exemple, per a trobar
el màxim comú divisor de $a$ i $b$ es poden considerar les llistes de
divisors com *conjunts* i, finalment, localitzar dins aquest conjunt el
valor màxim com es pot veure en l'exemple següent

```sage
a = 39854; b = 765756

D1 = set(a.divisors())
D2 = set(b.divisors())

D = D1.intersection(D2) # Els divisors comuns de a i b

max(D) # El màxim comú divisor de a i b
```

Com ja sabia Euclides fa molts anys, aquesta no és la forma més eficient
d'obtenir aquest resultat i una funció com la següent, que aplica l'algoritme d'Euclides, seria molt millor:

```sage
def euclides(a,b):
	m = max(abs(a),abs(b))
	n = min(abs(a),abs(b))
	r = m % n
	while r > 0:
		m = n
		n = r
		r = m % n
	return n
```


Aquesta funció es pot fer encara més curta utilitzant com hem vist que
es pot calcular el residu amb nombres negatius i que no cal prendre el
divisor més petit que el denominador:

```sage
def euclides(a,b):
    while b:
        a, b = b, a % b
    return a.abs()
```

En qualsevol cas, **SageMath** ja porta aquests
càlculs incorporats a `gcd()` que calcula el màxim comú divisor de dos
enters o d'una llista tan llarga com es vulgui d'enters. Podeu
comprovar, per exemple, com la funció `euclides` dona el mateix resultat
que `gcd`

```sage
gcd(748*2345,748*4686)
```
```sage
euclides(748*2345,748*4686)
```


Tingueu en compte que si es vol fer el màxim comú divisors de tres o més
enters caldrà incloure les dades en una *llista* ja que `gcd(a,b,c)` no
funcionarà.

```sage
gcd([748*3456,748*4520,748*4208])
```

Per als càlculs del mínim comú múltiple, s'aplica la funció `lcm()` a un
parell d'enters o a una llista de tres o més. Com en el cas del màxim
comú divisor, no funcionarà si s'introdueixen tres o més arguments sense
agrupar en una llista.

```sage
lcm(24,38)
```
```sage
lcm([24,38,18])
```



Hi ha problemes en els que no n'hi ha prou amb la informació del màxim
comú divisor i es volen conèixer els coeficients d'una *identitat de
Bézout* per a un parell d'enters (recordeu que una identitat de Bézout
per al parell $a$, $b$ consisteix a tenir una igualtat del tipus
$$\text{mcd}(a,b) = \alpha\times a +\beta \times b\,,$$ on $\alpha$ i
$\beta$ són les coeficients que s'han de determinar i sempre
existeixen). Per tal d'obtenir aquest resultat es pot utilitzar la
instrucció `xgcd()` que dona un resultat format per tres elements que
són, respectivament, el màxim comú divisor, $\alpha$ i $\beta$. Per
exemple:

```sage
a=39854; b=765756; show(gcd(a,b))
```
```sage
g, alpha, beta = xgcd(a,b)
show(g)
show(f'{alpha * a + beta * b = }')
```

Ara bé, si el que volem és obtenir identitats de Bézout per a més de dos
elements, caldrà fer-ho manualment. Per exemple, si volem calcular una
identitat de Bézout pels enters $a=72776$, $b=9944$ i $c=86823$:

Primer notem que $\text{mcd}(a,b,c)=\text{mcd}(\text{mcd}(a,b),c)$:

```sage
a=72776; b=9944; c=86823
show(gcd([a,b,c]))
show(gcd(gcd(a,b),c))
```


Fem una identitat de Bézout per $a$ i $b$, i emmagatzemem els
coeficients que obtenim:

```sage
d0, alpha0, beta0 = xgcd(a,b)
```

De forma que $d_{0}= \alpha_{0} \times a + \beta_{0} \times b$. Ara fem
una identitat per $d_0$ i $c$:

```sage
d1, alpha1, beta1 = xgcd(d0, c)
```

I com que $d_{1} = \alpha_{1} \times d_{0} + \beta_{1} \times c$,
obtenim el resultat que volem substituint $d_0$:

```sage
alpha = alpha1*alpha0
beta = alpha1*beta0
gamma = beta1

print(f'{gcd([a,b,c])} = {alpha} * {a} + {beta} * {b} + {gamma} * {c}')
```

## Sistemes de congruències

Un problema que apareix sovint quan es tracta de resoldre situacions en
les que s'han de fer reparticions és el de resoldre sistemes de
congruències del tipus $$\begin{gathered}
x \equiv a\ (\text{mod }\alpha)\\
x \equiv b\ (\text{mod }\beta)\\
\vdots\end{gathered}$$ Segurament ja sabeu que totes les solucions d'un
d'aquests problemes (si és que n'hi ha alguna) difereixen entre si per
múltiples del mínim comú múltiple $m$ del mòduls $\alpha$, $\beta$,
$\dots$ i, per tant que es pot dir que són de la forma
$s_{0}\ (\text{mod }m)$, on $s_{0}$ és una solució particular qualsevol.

La instrucció de **SageMath** que dona les
solucions d'aquests problemes és `crt()` (**c**hinese **r**emainder
**t**heorem) i, per exemple, per a solucionar el problema
$$\begin{gathered}
x \equiv 2\ (\text{mod }6)\\
x \equiv 4\ (\text{mod }8)\\\end{gathered}$$ n'hi haurà prou amb

```sage
s = crt([2,4],[6,8])

print(f'Solucio = {s}')
print(f'{s % 6 = }')
print(f'{s % 8 = }')
```

per tal de conèixer una solució particular, i calcular el mínim comú
múltiple per tal de saber com construir totes les altres

```sage
m = lcm(6,8)
print(f'Solucions = {s} mod {m}')
```

La llista de congruències pot ser tan llarga com es vulgui. Per exemple,
amb quatre condicions,

```sage
crt([2,4,8,18],[6,8,20,50])
```

Quan el problema no té solució la funció `crt()` reacciona amb un
missatge d'error on podreu detectar quina condició necessària per a la
solució està fallant.

```sage
crt([3,6],[6,8])
```


## Equacions diofàntiques

En general, la instrucció `solve()` calcularà només les solucions
enteres d'una equació sempre que fem la restricció corresponent a les
variables i l'equació que es plantegi no sigui *massa difícil*. Això
vol dir que es poden plantejar equacions lineals o polinòmiques
quadràtiques en les que intervinguin totes les variables que es
vulgui (les equacions diofàntiques de grau 3 donen feina a molts matemàtics. Sabeu la història de l'últim teorema de Fermat i de la solució d'Andrew Wiles?).


Per posar alguns exemples, noteu com es poden obtenir totes les *ternes
pitagòriques* (quadrats que sumats donen quadrats) resolent amb
`solve()` l'equació $x^{2}+y^{2}=z^{2}$

```sage
reset() # Per si de cas
var('y z')
assume(x,'integer')
assume(y,'integer')
assume(z,'integer')

tp = solve(x^2 + y^2 == z^2,(x,y,z))
show(tp)
```

I si volem una llista de valors concrets
```sage
ftp(p,q) = tp
for r in [1..10]:
    for s in [1..r-1]:
	    print(ftp(r,s))
```

Noteu que la forma com apareixen les solucions és diferent a la que
apareix en altres situacions, això és per què, en realitat, la
instrucció `solve()` *demana* a una instrucció específica
(`solve_diophantine()`) les solucions. De fet, es pot fer servir
directament aquesta instrucció, sense posar restriccions a les
variables, per tal d'obtenir el mateix resultat.

No cal que proveu de resoldre coses com $y^{2}=x^3+1$ (que, com a mínim,
és clar que es pot solucionar amb $x=-1, y=0$ i $x=2,y=3$, i es pot demostrar que aquestes són totes les solucions).

```sage
solve(x^3+1 == y^2,(x,y)) # Dona un error
```

## Els anells $\mathbb{Z}/n\mathbb{Z}$

Recordeu que hi ha càlculs en els que cal especificar amb quin tipus
d'elements s'està treballant, sent `ZZ` els enters, `QQ` els racionals,
`RR` els reals, etc., de forma que els resultats de les operacions
s'adapten al context corresponent. Si es vol treballar directament en
l'aritmètica *mòdul $n$* es pot fer una cosa equivalent a través de la
instrucció `Zmod()`. Per exemple, podem assignar com valor a la variable
`Z26` (o qualsevol altre nom que ens agradi) el de l'anell
$\mathbb{Z}/26\mathbb{Z}$:

```sage
Z26 = Zmod(26) # És equivalent a Integer(26)
Z26
```

Ara ja hi ha funcions que permeten veure algunes propietats de
$\mathbb{Z}/26\mathbb{Z}$ com per exemple

```sage
Z26.cardinality()
```

```sage
len(Z26)
```

```sage
Z26.addition_table() # La taula de l'operació suma
```

```sage
Z26.multiplication_table() # Endevineu
```


Si escriviu `Z26.` i premeu el *tabulador*, obtindreu totes les
propietats de l'objecte que es poden demanar.



Per tal de treballar directament amb elements de
$\mathbb{Z}/26\mathbb{Z}$, es poden anar creant de la forma següent:

```sage
a=Z26(765)
b=Z26(8934)
show(a)
show(b)
```
```sage
a^177
```
```sage
b^12
```

```sage
a.order() # L'ordre additiu
```
```sage
b.order()
```

Fixeu-vos que, encara que en el moment de mostrar el seu valor, sembli
que les variables `a` i `b` continguin nombres enters,
**SageMath** *sap* que en realitat s'ha
considerat el valors continguts com elements mòdul $26$:

```sage
a.parent()
```

Es pot treballar amb expressions més complexes sempre i quan les
operacions que es realitzin siguin permeses (per exemple, si es vol
dividir per un element, aquest ha de ser invertible)

```sage
c = Z26(13)
(3*a^5+(b+1)^(-2)+sqrt(c))^12 - 5
```

Observeu que a l'expressió anterior hi han alguns enters que
**SageMath** ha *forçat* a considerar-los a
$\mathbb{Z}/26\mathbb{Z}$. Per exemple, per fer la suma `b+1` que hi
apareix, **SageMath** converteix l'1 en el que
seria `Z26(1)`, i el mateix amb el `-5` del final de l'expressió. També
podeu veure que en l'expressió hi apareix una arrel quadrada. Si bé en
aquest cas $13^2\equiv 13 \mod(26)$, i per tant té sentit calcular
$\sqrt{\overline{13}\,}$. De fet això va una mica més enllà. Per
exemple, no hi ha cap element a $\mathbb{Z}/3\mathbb{Z}$ tal que
$x^2=\bar 2$. Tot i això, **SageMath** no té cap
problema en realitzar les comandes següents:

```
Z17 = Zmod(17)
z = sqrt(Z17(5))
show(z)
show(z^2)
```

Tot i que no correspon a aquesta assignatura, noteu que el fet de que
**SageMath** no es queixi per fer aquestes
operacions és anàleg al fet de que a $\mathbb{R}$ tampoc hi ha cap
element tal que $x^2=-1$, però això no impedeix que en definim un de
nou, $i =\sqrt{-1}$, i a partir d'aquí arribem a la
construcció del complexos.


```sage
sqrt(Z17(5)) in Z17
```

```sage
sqrt(Z17(2)) in Z17
```

```sage
parent(sqrt(Z17(5)))
```


En aquests últims exemples es veu com es poden utilitzar expressions del
tipus `in Z26`. Això pot servir a l'hora de fer llistes com la que
apareix en l'exemple següent, on es fabrica la llista dels quadrats a
$\mathbb{Z}/26\mathbb{Z}$.

```sage
[z^2 for z in Z26]
```

A la llista hi apareixen repeticions, però recordeu que ja sabeu com
eliminar-les:

```sage
list(set([z^2 for z in Z26]))
```

Un altre exemple del mateix tipus: suposem que es vol obtenir una llista
de les potències de l'element `a` en $\mathbb{Z}/26\mathbb{Z}$. Com que
sabem que en algun moment s'hauran d'anar repetint, podem fer-ho de la
forma següent:

```sage
S = [a]
k = 2
while a^k not in S:
    S.append(a^k)
    k+=1
S
```

En aquest cas s'ha arribat a $1$, cosa que ens indica que `a` és
invertible a `Z26`. Podem calcular el seu ordre multiplicatiu mirant els
elements de `S`, però, com passa sovint, ja hi ha altres formes de
fer-ho:

```sage
len(S)
```
```sage
a.multiplicative_order()
```

La llista dels elements que són invertibles s'obté, com ja sabeu,
considerant les classes d'enters coprimers amb $26$. Aquests elements es
poden calcular a través de les instruccions següents:

```sage
Z26.list_of_elements_of_multiplicative_group()
```
```sage
Z26.unit_group()
```

Aquest últim objecte ens permet treballar amb el grup dels elements
invertibles.


Com ja sabeu, si $n=p$ és un nombre primer, aleshores
$\mathbb{Z}/p\mathbb{Z}$ és un cos, el que vol dir que tots els elements
diferents de zero tenen invers (pel producte). Però si voleu treballar
amb $\mathbb{Z}/p\mathbb{Z}$ com a cos, hi ha una instrucció millor que
posar `Zmod(p)` i es posar `GF(p)` (que ve de Galois Field, que és com a
molts llocs s'anomenen els cossos finits). Compte, però, que no és el
mateix posar `Zmod(n)` que posar `GF(n)` si $n$ no és un nombre primer:
aquesta última ens donarà un error si $n$ no és una potència d'un primer
(ja que no hi ha cap cos amb aquest nombre d'elements), i ens donarà un
anell diferent si $n$ és una potència d'un primer (ja que
$\mathbb{Z}/n\mathbb{Z}$ no és mai un cos si $n$ no és un primer). Per
exemple `GF(4)` ens donarà un cos amb 4 elements que no és
$\mathbb{Z}/4\mathbb{Z}$ (això us ho explicaran a segon).


## Equacions *mòdul $n$*


Encara que per a determinar les solucions d'una equació a
$\mathbb{Z}/n\mathbb{Z}$ n'hi hauria prou utilitzant la *força bruta* i
anar provant per a tots els casos possibles (només n'hi ha un nombre
finit, en algun moment s'acabaran les proves) aquest no és,
probablement, el mètode més eficient. La funció `solve_mod()` intenta
ser una mica més eficient i permet resoldre a $\mathbb{Z}/25\mathbb{Z}$
l'equació $x^{2}-3y^{2}=14$ fent

```sage
reset() # Abans hem fet alguns assume()
var('x y')
solve_mod(x^2-3*y^2 == 14, 25)
```

O equacions de grau superior com

```sage
solve_mod(x^3-2*x^2-1 == 0, 11)
```

```sage
solve_mod(x^4-2*x^3-6 == 0, 91)
```

```sage
solve_mod(x^3+3*x+1 == 0, 10)
```

## Exercicis


### Exercici 1

Definiu una funció de **SageMath** que si li
dones una llista finita de nombres et retorna una llista ordenada i
sense repeticions (Indicació: transformeu-la a un conjunt, i
retorneu la llista ordenada amb sorted).

-- begin hide


```sage
def OrdNR(L):
    return sorted(list(set(L)))
```

```sage
OrdNR([5,7,3,2.1,1,3,6,,1,1,6,1,3,1,7])
```

Ho provo amb una llista de 30 nombres a l'atzar entre 1 i 30.

```sage
OrdNR([randint(1,30) for _ in range(30)])
```

(He usat `_` com a nom de la variable que s mou en el range, doncs no ens cal per res, només per repetir una cosa 30 cops).


-- end hide

### Exercici 2

Com que les llistes de Python comencen pel zero, el que és a vegades
un inconvenient, ens aniria molt bé poder usar una 'nova' versió de
les llistes que comenci per l'1. Expliqueu com podem fer això amb
diccionaris, i creeu una funció tal que, donada una llista ens
retorni un diccionari amb la mateixa llista però ara començant per
1.

-- begin hide
```sage
def nova_llista(L):
    return { i+1:L[i] for i in range(len(L))}

nL = nova_llista([1,2,3,4])
show(nL[3])
```
-- end hide


### Exercici 3

Es diu que un parell d'enters positius $m$ i $n$ són *amics* si la
suma dels divisors (propis) de cada un és igual a l'altre. Definiu
una funció que permeti decidir si una parella $(m,n)$ és una parella
d'amics o no. Utilitzeu la funció per a localitzar totes les
parelles d'amics menors que $1000$.


-- begin hide

```sage
def Amics(a,b):
    da=sum(divisors(a))-a
    db=sum(divisors(b))-b
    return da==b and db==a
```

```sage
Amics(12,15)
```

```sage
Amics(6,6)
```

```sage
Amics(220, 284)
```

```sage
for a in srange(2,1000):
    for b in srange(a,1000):
        if Amics(a,b):
            print(a,b)
```

Si voleu que no surtin els nombres perfectes (i per tant que les parelles siguin a < b), canvieu el segon bucle

```sage
for a in srange(2,1000):
    for b in srange(a+1,1000):
        if Amics(a,b):
            print(a,b)
```

-- end hide


### Exercici 4

Definiu una funció de **SageMath**, anomenada
`xxgcd`, que accepti tres enters $a$, $b$, $c$ com a argument, i
retorni el màxim comú divisor $d=\text{mcd}(a,b,c)$ i els
coeficients $\lambda$, $\mu$ i $\nu$ d'una identitat de Bézout per a
aquests números de forma que
$$d=\lambda\times a+\mu\times b+\nu\times c.$$
Sabeu modificar
`xxgcd` per tal que accepti com argument una llista d'enters, de
longitud arbitrària i doni com a resultat els coeficients d'una
identitat de Bézout?


-- begin hide


```sage
def xxgcd(a,b,c):
    d,l,m=xgcd(a,b)
    d,r,n=xgcd(d,c)
    return d,r*l,r*m,n
```

```sage
xxgcd(4*7,2*7,3*7)
```

Modificació per a llistes arbitraries (amb una funció recursiva, que es crida a si mateixa amb un element menys a cada llista).

```sage
def xxgcd(L):
    if len(L)==1:            # Si la llista només té un element, retornem l'element i una llista que conté el 1
        return L[0],[1]
    else:
        a=L[-1]              # Ultim element de la llista
        L2=L[:-1]            # La llista menys el ultim element
        d,S=xxgcd(L2)        # Apliquem la funció a la llista
        d,l,m=xgcd(d,a)      # Apliquem el xgcd al gcd de la llista L2, anomentat l, i a.
        SN=[l*s for s in S]  # Multipliquem tots els elements de la llista S pel coefficient que li toca a l
        SN.append(m)         # Afeguim a SN el coefficient que li toca a a
        return d,SN
```

```sage
L=[2*3*5*7,2*3*5*11,2*3*11*7,2*11*5*7]
print(L)
d,S=xxgcd(L)
print(S)
sum([L[i]*S[i] for i in range(len(L))])==d
```

-- end hide


### Exercici 5


Tenint en compte que `crt()` només dóna una solució particular del
problema de congruències que es plantegi, definiu una extensió
d'aquesta funció (`xcrt`) que, amb les mateixes dades, doni com a
resultat una llista de dos elements: el valor de la solució
particular i el mínim comú múltiple dels mòduls.


```sage
-- begin hide
def xcrt(C,M):
    return [crt(C,M), lcm(M)]

xcrt([2,4,8,18],[6,8,20,50]) # exemple
-- end hide
```


### Exercici 6


Definiu una funció de **SageMath** que
accepti com a arguments dos enters $a$ i $m$, i doni com resultat la
llista de potències
$\{{\overline a}^k \mid k\geq 0 \}\subseteq \mathbb{Z}/m\mathbb{Z}$.

-- begin hide
```sage
def potenciesmodm(a,m):
    Zm=Zmod(m)
    ab=Zm(a)
    if ab.is_unit():
        S=[ ab^k for k in srange(ab.multiplicative_order()) ]
        return(S)
    else:
        S=[1]
        k=1
        while ab^k not in S:
            S.append(ab^k)
            k+=1
        return S

show(potenciesmodm(3,100))
show(potenciesmodm(1,100))
show(potenciesmodm(10,100))
show(potenciesmodm(5,100))
```

He distingit entre el cas que és unitat, doncs aleshores la màxima $k$ que surt és l'ordre multiplicatiu, de quan no ho és (però de fet no calia). A l'enunciat diu "llista de les potències", però escriu un conjunt, així que he decidit que doni una llista ordenada. He assumit que a partir que surt una potencia repetida, les següent potències també son repetides, però aixó caldria demostrar-ho!
-- end hide
### Exercici 7


Feu una llista dels elements invertibles a $\mathbb{Z}/24\mathbb{Z}$
amb l'ordre multiplicatiu de cadascun d'ells.

-- begin hide
```sage
R = Zmod(24)
L1 = [(o, R(o).multiplicative_order()) for o in R.list_of_elements_of_multiplicative_group()] # El més ràpid

L2 = [(o, R(o).multiplicative_order()) for o in Zmod(24).unit_group()]

L3 = [(o, o.multiplicative_order()) for o in Zmod(24) if o.is_unit()] # Més clar, potser
```
-- end hide


### Exercici 8


Considereu l'anell $\mathbb{Z}/257\mathbb{Z}$.

- Comproveu que 257 és primer i que, per tant, tots els elements
  no nuls de $\mathbb{Z}/257\mathbb{Z}$ són invertibles.

-- begin hide
```sage
print(f'{is_prime(257) = }')
ZN = Zmod(257)
print(f'{ZN.is_field() = }')
```
-- end hide

- Trobeu un element
  $\bar a\in \mathbb{Z}/257\mathbb{Z}\setminus\{\bar 0\}$ amb
  ordre multiplicatiu màxim.

-- begin hide

Calculem el màxim dels ordres del grup d'unitats (=invertibles) de l'anell $\mathbb{Z}/257\mathbb{Z}$, que com que és un cos, són tots execepte el $0$. Hi ha un teorema que ens diu que, per $\mathbb{Z}/p\mathbb{Z}$ amb $p$ un primer, l'ordre sempre  és un divisor de $p-1=257-1=256$, i un altre teorema (més difícil) que diu que hi ha sempre algun element d'ordre $p-1$. Per tant l'ordre màxim ha de sortir $256$. 

```sage
max([a.order() for a in ZN.unit_group()])
```

```sage
for a in ZN:
    if a != 0 and a.multiplicative_order() == 256:
        print(f'{a = }')
        break
```

Una altra manera

```sage
next((a for a in ZN if a != 0 and a.multiplicative_order() == 256))
```

Un buscant-lo a l'atzar:

```sage
a = ZN(randint(1,256))
while a.multiplicative_order() != 256:
    a = ZN(randint(1,256))
print(a)
```

O un altre amb el mètode random_element()

```sage
a = 0
while a == 0 or a.multiplicative_order() != 256:
    a =  ZN.random_element()
print(a)
```


-- end hide

### Exercici 9


Hi ha molts jocs de màgia que estan basats en propietats de
divisibilitat. Un d'aquests és la possibilitat de recuperar la data
de naixement d'algú a partir del resultat de l'operació següent: si
aquesta data és $d$-$m$ on $d$ representa el dia i $m$ el mes
calcula $$v= 12\times d + 31\times m$$ Per exemple, al dia 3 de
febrer li correspondrà el valor $v= 12\times 3+31\times 2=98$.

Definiu una funció que recuperi les dades $(d,m)$ a partir del valor
$v$ i proveu amb els resultats dels vostres companys.


-- begin hide

Una funció buscant-lo sistemàticament (no és molt eficient, però no importa).

```sage
def diames(r):
    for d in srange(1,32):
        for m in srange(1,13):
            if (12*d+31*m)==r:
                return d,m
```

```sage
diames(12*13+31*11)
```

Fixeu-vos que hi ha nombres que no poden sortir, i la funció no respondrà res. 

```sage
diames(12)
```

Una altra manera molt millor es calcular els inversos de 12 mòdul 31 i de 31 mòdul 12 i observar que el dia es el residu de multiplicar v per el invers de 12 mòdul 31, i el més el residu de multiplicar v per el invers de 31 mòdul 12

```sage
Zmod(31)(12)^(-1)
```

```sage
Zmod(12)(31)^(-1)
```

```sage
def diames(r):
    return (13*r%31),(7*r%12)
```

```sage
diames(12*13+31*11)
```

Si ho provem ara amb un nombre que no pot sortir, veure-ho que surt un més o un dia impossible (com 0)

```sage
diames(31)
```

-- end hide
