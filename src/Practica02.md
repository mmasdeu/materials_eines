---
jupyter:
  title : 'Pràctica 2: Llistes, tuples, conjunts, diccionaris i cadenes'
  authors: [ "name" : "Marc Masdeu", "name" : "Xavier Xarles" ]
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.14.0
  kernelspec:
    display_name: SageMath 10.6
    language: sage
    name: sagemath
---

## Llistes

Una col·lecció d'expressions separades per comes i delimitades per
claudàtors (parèntesis quadrats, *square brackets*) és el que en
**SageMath** s'anomena una *llista*. Les llistes
serveixen per guardar en una variable única una sèrie d'expressions o
valors que conceptualment considerem relacionats d'alguna manera o tenen
una identitat comuna. Per exemple, la llista `A = [cos(t), sin(t), t]`
podria representar les tres coordenades d'una partícula que es mou en
l'espai, en funció del temps $t$. (Marginalment, quina figura descriu
aquesta partícula?)

Es pot accedir individualment a cada element d'una llista a través de la
seva posició a la llista: `A[k]` selecciona l'element $k$-èssim. Però
fixem-nos que la primera posició correspon a $k=0$. Això és així tant en
**SageMath** com en llenguatge **C**. Els elements
individuals de la llista anterior s'obtenen per tant fent `A[0]`,
`A[1]`, `A[2]`. Provem-ho:

```sage
var('t')
A = [cos(t), sin(t), t]
print(A[0])
print(A[1])
print(A[2])
```

El **SageMath** ens indicarà
amablement `list index out of range` (el C no serà tan amable) si
intentem accedir a la posició 3:

```sage
A[3] # No funcionarà
```

Per concatenar dues llistes, és a dir, posar els elements d'una a
continuació dels de l'altra n'hi haurà prou amb *sumar-les*, ja que
l'operador `+` està programat per reconèixer aquesta situació.
Considerem per exemple les velocitats de la partícula anterior i
concatenem-les amb el vector de posicions:

```sage
B=[ -sin(t), cos(t), 1]
C = A + B
print(C)
```

Naturalment, no és el mateix `A+B` que `B+A`. La "suma" de llistes no és
commutativa.

Podem obtenir una llista amb part d'una altra llista, expressant un rang
d'índexs dins els claudàtors, i si posem una posició negativa accedim
als elements de la llista començant pel final:

```sage
print(C[1:4])
print(C[1:5:2])
print(C[-2])
```

En general si posem `C[a:b:c]`
obtenim la subllista `[C[a],C[a+c],..,C[a+d*c]]` on $a+d\cdot c\lt b\le a+(d+1)\cdot c$. 
Tots els valors poden ser negatius. Si volem començar pel principi de la llista, o arribar fins el 
final, podeu deixar buit l'espai. Vegeu per exemple que fa

```sage
print(C[:3])
print(C[::2])
print(C[::-1])
```

Experimenteu amb més opcions per acabar d'entendre com funciona, doncs és un procediment molt útil. 

Com és d'esperar, cadascun dels elements d'una llista es pot modificar
individualment:

```sage
C[2] = t^2
C[5] = 2*t
print(C)
```

Es poden afegir elements al final d'una llista amb la funció `append()`,
inserir un element nou davant d'una posició donada amb `insert()`, i
eliminar un element amb `remove()`:

```sage
D = [1,t,t^2,t^3]
D.insert(1,t^(1/2))
print(D)
```
```sage
D.remove(t^2)
print(D)
```
```sage
D.remove(t^9) # Dona error, l'element no hi és
```

Freqüentment necessitem fer llistes els elements de la qual segueixen
una pauta o fórmula, depenent d'un valor que va variant. Per exemple,
per fer una llista dels quadrats dels enters entre $1$ i $10$, farem

```sage
quadrats = [ k^2 for k in [1..10] ]
```

Els claudàtors exteriors indiquen que estem fent una llista. La llista
estarà formada pels valors $k^2$, amb $k$ variant dins de la llista de
naturals de 1 a 10, que podem construir d'aquesta manera abreviada amb
els "dos punts suspensius" entre el principi i el final.

Les fórmules per a la construcció de llistes poden ser molt complicades.
Es pot fer que l'índex `k` avanci des de $a$ fins a $b$ (o sigui, es mogui 
dins de $\text a\le k\lt b$), incrementant-se cada pas en $c$ unitats, utilitzant
l'expressió `range(a,b,c)`, i també indicant una progressió aritmètica
amb $a$, $a+t$, $a+2t$,...$b$ amb `[a, a+t,..,b]` (només hi podeu posar 
**dos** punts, ni un ni tres). Així els quadrats dels múltiples de $3$
(fins al quadrat de $18$) s'obtenen amb:

```sage
[ k^2 for k in range(3,19,3) ]
```
o bé amb
```sage
[ k^2 for k in [3, 6,..,19] ]
```

També es poden afegir restriccions al rang dels índexs. Per exemple, la
instrucció següent generarà la llista dels quadrats dels nombres primers
entre $2$ i $29$

```sage
quadrats_primers = [ k^2 for k in [2..29] if k.is_prime() ]
```


La funció `len()` compta quants elements té una llista.

```sage
len(quadrats_primers)
```

I el resultat ens diu que hi ha 10 nombres primers entre 2 i 29.


## Tuples

Una tupla $(a_1,a_2,\dots,a_n)$ és similar a una llista, però té algunes diferències
importants que cal remarcar. La més important, i de fet la
que marca les altres, és que mentre que una llista és mutable, la tupla no: podem
modificar "l'interior" d'una llista però no d'una tupla.

Per exemple, si posem
```sage
T = (1,2,3,4,5)
print(T[-1])
T[-1] = 10
```
a part d'imprimir 5 ens surt un error que diu "'tuple' object does not
support item assignment". En canvi, observeu que amb llistes
no obtenim cap error:

```sage
L = [1,2,3,4,5]
print(L[-1])
L[-1] = 10
print(L)
```

Com que les tuples són immutables, podríeu pensar que no podem posar una
cosa mutable dins una tupla. Però si que és pot sense problemes: una
tupla, per tant, pot contenir una llista, i aquesta és pot modificar
sense problemes. El problema és que una tupla com aquesta no seria
"hashable", que és un concepte que sortirà a la propera pràctica.

Es pot passar de llista a tupla i de tupla a llista amb les comandes
`tuple` i `list`, respectivament. Les tuples, com les llistes, es poden
construir utilitzant comprehensió.

Compte que el fet que es pugui mutar té conseqüències delicades: si
poses, per exemple `M = L`, on `L` és una llista, i canvies el valor de
`M[1] = 1000`, llavors `L` també canvia. Per poder tenir una copia de `L` per
poder-la manipular sense canviar la llista particular s'ha de posar
`M = copy(L)` (però què passa llavors si tens una llista de llistes?...).

```sage
L = [1, 2, 3]
M = L
print(f'{L = }, {M = }')
M[1] = 1000
print(f'{L = }, {M = }')
```

```sage
L = [1, 2, 3]
M = copy(L) # També es pot fer M = L[:]
print(f'{L = }, {M = }')
M[1] = 1000
print(f'{L = }, {M = }')
```

Les tuples i les llistes es poden sumar, i l'efecte és que es construeix
una nova llista o tupla que conté els elements de la primera
llista / tupla seguit dels de la segona. Si posem `n * L` per un natural `n`,
obtindrem la llista/tupla repetida n cops.

Un cas especial és el de les tuples de només un element: si posem
`T = [2]`, això és una llista amb un sol element. Però si posem `T = (2)`
obtenim només el número, no pas una llista. Per poder remeiar això cal posar `T = (2,)`.


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
A = {2, x ,3.2, ZZ, pi}
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
print(X)
```

Els conjunts es poden fer les operacions habituals, moltes d'elles com a
mètode (o sigui, amb el format `A.metode(B)`). Per exemple, unió és
`union`, intersecció és `intersection`, la diferencia és `difference`,
etc.
```sage
X.intersection(A)
```
```sage
X.union(A)
```
```sage
X.difference(A)
```
També podeu demanar amb una funció el nombre d'elements (amb
`len`), i afegir o treure un element donat (amb `remove` dona error si no
hi és, amb `discard` no fa res si no hi és).
```sage
len(X)
```
```sage
X.add(3)
print(X)
```
```sage
X.remove(3)
print(X)
```
```sage
X.discard(3)
print(X)
```

Finalment, per agafar un
element d'un conjunt (arbitrari, és a dir, que no teniu control sobre quin us retornarà) podeu usar `pop()`, però compte perquè això el treu del conjunt.
```sage
print(X.pop())
print(X)
```
Si voleu un element *arbitrari* del conjunt però no el voleu treure, podeu fer servir la següent construcció:
```sage
X = {2, 1, 3}
next(iter(X))
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
print(Z)
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
a la tupla $(4,5)$ el conjunt $\{1,2\}$. Per accedir als valors només cal posar
`prova[1]` i respon `'a'`, i posar `prova['x']` i respon `[1,2]`, etc.

Les *keys* poden ser números, cadenes (*strings*), tuples de números o
de cadenes, però no poden ser ni llistes ni altres conjunts. En canvi als
valors s'hi pot posar qualsevol cosa.


Podem modificar el valor d'un diccionari com en el cas de les llistes.
Per exemple, observem el resultat del següent bloc:

```sage
prova[(4,5)] = {1,2,3}
print(prova)
```


També podem afegir més elements a un diccionari:


```sage
prova[2] = 'z'
```

Si volem afegir tots els elements d'un altre diccionari, podem fer servir `update`. Per exemple,
observem en el següent bloc que el valor en el $2$ es sobreescriu:


```sage
prova.update({2 : 'b', 7 : 'c'})
print(prova)
```


Si fem un bucle indexat en un diccionari, la variable es mou en la
llista de claus. D'aquesta manera, podem fer:

```sage
for k in prova:
	print(k, prova[k])
```

que ens imprimirà cada clau i el seu valor.

## Cadenes

Una cadena (*string*) és una successió de caràcters, que podem especificar com `'hola'` o `"hola"`, per exemple.

```sage
x = 'hola'
print(x)
y = "hola"
print(x == y)
```

Si la cadena conté més d'una línia, la podem especificar obrint amb 3 cometes:
```sage
x = '''Primera línia,
Segona línia,
i tercera.'''
print(x)
```

Fixeu-vos que si volem fer servir `'` o `"` dins la cadena, aleshores hem de delimitar-la amb l'altre tipus:
```sage
s = 'Un exemple "senzill".'
```

Podem accedir a posicions de les cadenes, com si fossin llistes:
```sage
print(len(s))
print(s[4])
print(s[:10])
print(s[::-1])
```

També podem preguntar si contenen una determinada subcadena:
```sage
print('ex' in s)
```
o si no la contenen:
```sage
print('ex' not in s)
```

El **SageMath** (de fet, **Python**) és molt potent a l'hora de manipular cadenes,
i incorpora moltes funcions. Per exemple, els mètodes `.lower()`, `.upper()`, `.strip()`,
`.replace()`, `.split()` ens poden ser útils:

```sage
s = '   Eines Informàtiques per les Matemàtiques   '
print(s.lower())
```

```sage
print(s.upper())
```

```sage
print(s.strip())
```

```sage
print(s.replace('Informàtiques', 'Computacionals'))
```

Com amb les llistes, podem sumar-les i obtenim la concatenació:

```sage
print('hola ' + 'adeu.')
```

## Cadenes amb format

Molt sovint volem construir cadenes a partir de variables que tenim declarades.
Igual que fa la funció `print()`, **SageMath** sap convertir qualsevol objecte en
una cadena, i podem forçar-lo a fer-ho amb `str()`:

```sage
temperatura = 38
str(temperatura)
```

Això ens permet construïr cadenes

```sage
print('Avui hem arribat a ' + str(temperatura) + ' graus!')
```

La manera recomanada de construir cadenes amb paràmetres és una altra. Es tracta
d'afegir una `f` davant les cometes, i aleshores podem especificar dins la cadena
les diferents variables que vulguem que s'avaluin, i fins i tot podem fer càlculs. Per exemple:

```sage
nom = 'Arale'
localitat = 'Vila del Pingüí'
setmanes = 3
s = f'Hola {nom}, benvinguda a {localitat}. Feia {7 * setmanes} dies que no et veia!'
print(s)
```

A vegades volem presentar el valor d'algunes variables. Les `f`-cadenes ens faciliten molt la feina,
ja que dins de `{}` hi podem posar un símbol `=` i aleshores també ens retorna el nom de la variable.
Fixeu-vos en aquest exemple.

```sage
u = 3
v = 4
w = 5

print(f'La variable {u = } i a més  {v * w = }')
```

Pot ser que vulguem fer servir una plantilla que després anirem omplint. En aquest cas, hi ha el
mètode `.format()`, que s'aplica a cadenes normals (sense `f` al començament).

```sage
s = 'Hola {nom}, benvinguda a {localitat}.'
print(s.format(localitat = 'Cerdanyola', nom = 'Jana'))
```

Hi ha alguns caràcters especials, com ara el tabulador `\t` o el salt de línia, `\n`, que ens poden ser
útils per representar informació:

```sage
s = '{nom}\t{edat}'
print('NOM\tEDAT\n')
print(s.format(nom='Júlia', edat=21))
print(s.format(nom='Gerard', edat=19))
```
Si els hem de fer servir en una cadena, els hem d'"escapar", és a dir, afegir una `\` extra perquè no els interpreti:

```sage
print('Per fer un salt de línia cal escriure \\n al mig de la cadena')
```


## Exercicis

### Exercici 1

La funció `randint()` genera un nombre enter (`int` de **Python**) a l'atzar en el rang
marcat pels arguments. Per exemple, cada cop que s'executa la
instrucció `randint(1,6)` s'obté un nombre aleatori entre 1 i 6, amb
la mateixa probabilitat per a tots (o sigui, estem llançant un *dau
equilibrat*). Així, la instrucció

```sage
L = [randint(1,6) for _ in [1..100]]
```
simula el llançament de 100 tirades de dau (fixem-nos que no cal
especificar la variable que es mou entre 1 i 100, ja que no la fem servir).


Si fem

```sage
L = [randint(1,6) for k in range(randint(10,200))]
```

obtindrem una llista de tirades aleatòries de dau, de longitud
aleatòria entre 10 i 200.

Amb aquesta última llista:

- Determineu quants elements té.

-- begin hide
```sage
print(len(L))
```
-- end hide

- Feu dues llistes, a partir dels elements de `L`, una amb els
  resultats parells i l'altra amb els senars.

-- begin hide
```sage
Lp = [o for o in L if o % 2 == 0]
Ls = [o for o in L if o % 2 == 1]
```
-- end hide

### Exercici 2

Construïu els següents objectes:

- Una llista dels primers entre 100 i 300 acabats en 1.

-- begin hide
```sage
L = [p for p in srange(100, 300) if p.is_prime() and p % 10 == 1]
```
-- end hide

- Una llista dels divisors senars de 400.

-- begin hide
```sage
L = [d for d in divisors(400) if d % 2 == 1]
```
-- end hide


- El conjunt de nombres entre 1 i 100 que són suma d'un cub i un quadrat.

-- begin hide
```sage
S = {n^2 + m^3 for n in range(10) for m in range(5)}.intersection(set(range(1,101)))
S1 = {n^2 + m^3 for n in range(10) for m in range(5) if 0 < n^2+m^3 < 101}
S2 = {n for n in srange(1,101) if any({(n-a^3).is_square() for a in range(n)})}
```
-- end hide

- Un diccionari que assigni a cada enter $n$ entre $40$ i $50$ la llista de tuples de naturals $(a,b)$ tals que $n=a^2+b^2$.

-- begin hide
```sage
D = {n : list(set([(a,b) for a in range(9) for b in range(9) if a^2+b^2 == n])) for n in range(40,51)}
D1 = {n : [(a,b) for a in range(9) for b in range(9) if a^2+b^2 == n] for n in range(40,51)}
D2 = {n : [(a,sqrt(n-a^2)) for a in srange(9) if (n-a^2).is_square()] for n in srange(40,51)}
```
-- end hide



### Exercici 3

Feu una llista amb les coordenades dels sis vèrtexs
	de l'hexàgon regular inscrit en la circumferència de radi $1$, que són
	els punts de la forma
	$$\big(\cos(\tfrac{2k\pi}{6}),\sin(\tfrac{2k\pi}{6})\big), \quad\text{per a $k=0,\dots,5$}\ .$$
	Utilitzant la llista anterior i una instrucció `line`, dibuixeu aquest
	hexàgon (Tingueu en compte que també és molt possible que existeixi una
    instrucció del tipus `polygon`).

### Exercici 4

Definiu un diccionari on les claus siguin els nombres enters
   des de 2 fins a 10, i els valors els seus quadrats.

### Exercici 5

Expliqueu com ho faríeu per crear una nova llista d'una llista `L` que tingui els mateixos elements que `L` però sense repeticions.

### Exercici 6

Donada una cadena `C`, expliqueu com crear un conjunt que contingui les lletres de `C`. Podeu fer un diccionari tal que les claus siguin les lletres de `C` i els valors el nombre de vegades que hi surt cada lletra? Proveu-ho per `C='Mississipi'`.
