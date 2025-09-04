---
jupyter:
  title : 'Pràctica 2: Més manipulacions bàsiques'
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

# Més Manipulacions Bàsiques


## Més coses sobre Jupyter Lab

L'entorn **SageMath** té dues parts que cal diferenciar
bé: Una és el notebook de Jupyter, que és la interfície web on escrivim
instruccions de **SageMath**. L'altra és el
*kernel*, que és el motor computacional que executa les ordres. Cada
notebook té un kernel executant-se al darrera, independent dels altres
notebooks que puguin estar oberts. Podem aturar el kernel sense tancar
la pestanya del notebook; o tancar la pestanya sense aturar el kernel (i
per tant tornar a obrir el notebook amb el kernel en el mateix estat).


Per aturar el kernel d'un notebook, cal anar a la pestanya del panel
de la Jupyter, marcar el notebook, i prémer el botó `Shutdown`. Per
tornar a iniciar el kernel, cal fer-ho des de la pestanya del notebook,
amb `Kernel -> Restart`. Quan es reinicia el kernel, és com si no
s'hagués executat encara cap instrucció del notebook.


En el menú `File` del notebook hi ha diverses ordres similars:

-   `Make a copy` crea una còpia del notebook i l'obre en un altra
    pestanya. El kernel d'aquesta còpia està tot just inicialitzat, i
    per tant no s'ha executat res del que hi ha en el notebook.

-   Quan fem `Save as...`, es guarda el notebook en un fitxer `.ipynb`
    amb el nom que escollim, i continuem treballant amb aquest nou
    fitxer; el kernel continua funcionant. El fitxer antic queda guardat
    i el seu kernel aturat. Això és semblant a com funciona el
    `Save as...` en la majoria de programes amb menús.

-   `Rename...` només canvia el nom del fitxer que està obert. El kernel
    segueix funcionant. No queda guardat res amb el nom anterior. És el
    que podem utilitzar d'entrada per començar la sessió: Després de
    `New -> Notebook`, feu `Rename...` i poseu `Practica02` o el que
    vulgueu.



Recordeu que la manera neta de sortir del
**SageMath** és fent `File -> Close and Halt` i
després tancant la pestanya. Podem aturar un kernel amb el menú
`Kernel -> Shutdown`, mantenint obert el notebook. Si fem
`Restart -> Run all`, es tornen a executar totes les instruccions *en
l'ordre en què estan en aquest moment*. Les cel·les quedaran renumerades
dels del principi.


Com ja haureu vist, el Jupyter fa *Autosave* del notebook cada dos
minuts, de manera que és difícil perdre gaire feina en cas d'accident.
Si es vol guardar un notebook expressament, es pot fer
`File -> Save and Checkpoint`. Naturalment, fent `Close and Halt` també
es guarda el notebook (i a més es tanca la pestanya i s'atura el
kernel).


En el menú del notebook, `File -> Download As` permet convertir el
notebook a altres formats, com HTML o PDF. O al mateix format de
notebook (`.ipynb`) per guardar-lo en algun altre lloc.


En el menú desplegable que posa `Code` podem canviar a `Markdown`.
Aleshores la cel·la on som es transforma en una cel·la on es pot posar
text normal (proveu-ho), instruccions de LaTeX (proveu `$\alpha^2$`), i
*headers*, o sigui títols de seccions, subseccions, etc (proveu
d'escriure `# Capítol 1` o algun text precedit de `#`, o bé `##`, o bé
`###`, etc, que permet fer seccions, subseccions, etc). Igualment cal
executar la cel·la amb les tecles *Shift + Return*.


Si volem inserir cel·les en el notebook, tenim el menú `Insert`.
Però recordeu sempre que l'ordre d'execució és el de la numeració de les
cel·les.



Quan arrenca *Jupyter Notebook*, veiem una pestanya amb el
directori base personal del nostre sistema operatiu, que anomenem en
anglès *Home*. Típicament, en Windows és el directori
`C:\Users\nom_usuari`, i en Linux o MacOS és `/home/nom_usuari`. La
Jupyter no ens deixarà sortir d'aquest directori i els seus
subdirectoris.


## La instrucció `expand`

A la pràctica anterior vam fer la substitució $a=(u-1)^2$ en l'expressió
$E=2a-3b+c^2$. Repetiu-ho i observeu que el quadrat queda tal qual
$(u-1)^2$. Si volem desenvolupar aquest quadrat, necessitem la funció
`expand()`.

```sage
var('a b c u')
E = 2*a-3*b+c^2
E.subs(a=(u-1)^2).expand()
```

```sage
expand(E.subs(a=(u-1)^2))
```

Observeu altre cop que les dues sintaxis són equivalents. Hem
d'acostumar-nos a veure les dues formes.

Amb expressions més complicades pot ser convenient fer servir la comanda
`show()` en comptes de `print()`. Amb `show()` se'ns mostra una versió
compilada amb LaTeX:

```sage
show(expand(E.subs(a=(u-1)^2)))
```

Desenvolupeu les expressions $(a+b)^2$, $(a+b)^3$, $(a+b)^4$,
$(a+b+c)^2$, $(a+b)(c+d)$, $(a+b)^2(c+d)^2$.

-- begin hide
```sage
var('a b c d')
show(((a+b)^2).expand())
show(((a+b)^3).expand())
show(((a+b)^4).expand())
show(((a+b+c)^2).expand())
show(((a+b)(c+d).expand())
show(((a+b)^2(c+d)^2).expand())
```
-- end hide

## La instrucció `factor`

La operació inversa a l'anterior és descompondre una expressió en
factors. Per fer això s'utilitza la funció `factor()`. Tingueu en compte
però que descompondre en factors és un concepte profund matemàticament
parlant, com veureu durant els estudis. Factoritzar depèn del "context"
matemàtic en què ens movem.

De tota manera, podem veure el que passa sense entrar en detalls subtils
(un cop més, es pot usar aquesta sintaxi o bé posar l'expressió a
factoritzar com a argument de la funció `factor()`):

```sage
var('a x')
print((x^2-2*x+1).factor())
print((x^2-2*a*x+a^2).factor())
print((x^2-x-1).factor())
```

Observeu que l'última expressió té dues arrels reals: En efecte
$x^2-x-1=0$ té solucions $x=(1+\sqrt{5})/2,\ x=(1-\sqrt{5})/2$, i per
tant es pot factoritzar com
$$\big(x-\tfrac{1+\sqrt{5}}{2})\cdot\big(x-\tfrac{1-\sqrt{5}}{2}\big) \ .$$
Per obtenir aquestes arrels necessitaríem un "context" en què haguéssim
afegit explícitament el símbol $\sqrt{5}$. Això no ho veurem en aquest
curs. Però sí veurem com obtenir les arrels d'equacions com ara
$x^2-x-1=0$, i el resultat serà les dues solucions esperades.

Si apliquem `factor()` a un quocient de polinomis, obtindrem
factoritzacions de numerador i denominador, i eventualment una
simplificació de la fracció.

Quan s'aplica a nombres enters, la funció `factor()` fa la típica
descomposició en factors primers:

```sage
223344891012288664.factor()
```

Si hi posem un nombre negatiu, ens afegirà $-1$ als factors:

```sage
(-2019).factor()
```

Aplicat a un nombre primer, naturalment, obtindrem el propi nombre. Però
també podem preguntar específicament si un nombre és primer o no ho és,
amb la funció `is_prime()`. Mireu si són primers 138283 i 237761.

-- begin hide
```sage
print(138283.is_prime())
print(237761.is_prime())
```
-- end hide

Fixeu-vos que el resultat és True o False. Es diu que aquesta funció
*retorna* un valor booleà (Boolean value, el nom prové de George Boole, matemàtic anglès (1815-1864)).

I ja que estem amb nombres primers, quin és el següent primer d'un
primer (o de qualsevol nombre)? Això ho contesta la funció
`next_prime()`:

```sage
next_prime(1000)
```

Si volem saber quin és el nombre primer que està, per
exemple, a la posició 1500 de la llista de tots els primers, podem fer:

```sage
nth_prime(1500)
```

Si fem `prime_range(1000,1097)` obtindrem tots els
primers entre 1000 i 1096:

```sage
prime_range(1000,1097)
```

Fixeu-vos que l'extrem esquerra està inclòs, però l'extrem dret no (el nombre 1097 és primer, i no surt a la llista). Això és així en general en els rangs del **SageMath**. Per exemple, a les llistes, `[a:b]`
selecciona els elements de la posició `a` fins just abans de la posició
`b`. En canvi, si fem servir la notació `[a..b]` o `[a,b..c]` els extrems sí que s'hi inclouen.

Proveu també la funció `divisors()`, que ens dona tots els divisors d'un
número. Per exemple, per obtenir tots els divisors de 240, fem:

```sage
240.divisors()
```

## Simplificacions

Un dels problemes típics que ens trobem fent matemàtiques, tant amb
llapis i paper com amb ordinador, és haver obtingut una expressió
complicada després d'uns llargs càlculs, sospitar que l'expressió es pot
simplificar, i no veure com.

Aquesta sembla una tasca perfecta per a un manipulador algebraic, i
el **SageMath** ens ho permet fer. Cal tenir en compte però que no
és senzill explicar-li a l'ordinador els nostres processos mentals quan
simplifiquem, i que de vegades tampoc ens posaríem d'acord entre
nosaltres sobre quina és la expressió "més simplificada possible".

El **SageMath** ja fa de manera automàtica
algunes simplificacions. Per exemple, avalueu $\dfrac{b(b-1)}{b(b+1)}$.
La divisió per $b$ a dalt i a baix és automàtica.

Fem-li fer una simplificació senzilla, que no es fa automàticament:

```sage
A = x^x / x
A.simplify()
```

No obstant, no sempre `simplify()` té èxit:

```sage
var('b')
A = (b^2-1) / (b-1)
A.simplify()
```

I tampoc "veu" la igualtat $\sin^2 x+\cos^2 x=1$:

```sage
var('x y')
B = (sin(x))^2 + (cos(x))^2
B.simplify()
```

Diguem que la funció `simplify()` és prudent, i
no toca res si no som més específics. En canvi

```sage
A.simplify_rational()
```

```sage
C = 1 / (x+1) - 1 / (x-1)
C.simplify_rational()
```

```sage
B.simplify_trig()
```

Hi ha moltes més funcions per simplificar de manera específica. Hi ha
també la funció `simplify_full()`, que intenta successivament diverses
simplificacions. Totes les expressions anteriors se simplifiquen amb
`simplify_full()`; per tant, sembla aconsellable començar amb ella.

De tota manera, `simplify_full()` no ho fa tot. Per exemple,

```sage
D = 2 * log(sqrt(2) + 1) + 2 * log(sqrt(2) - 1)
show(D)
```
```sage
show(D.simplify_full())
```
```sage
show(D.simplify_log())
```

Sou capaços de simplificar encara més l'ultima expressió obtinguda?

-- begin hide
```sage
D.simplify_log().simplify_full()
```
-- end hide


## La instrucció `collect`

Una altra tasca habitual amb què ens trobem els matemàtics en la
manipulació d'expressions que contenen símbols indeterminats és la
d'agrupar llurs sumands en termes de les potències d'una de les
variables, per tal d'obtenir una representació com a *polinomi* respecte
aquesta variable. La funció `collect()` intenta fer això, com podeu
comprovar en les instruccions següents.

```sage
var('y')
A = x^3-3*x^2*y+x^2-2*x*y-x-y^2*x+y^3+y^2-1
print(A)
print(A.collect(x))
print(A.collect(y))
```

L'expressió de la qual es vol fer `collect` no ha de ser necessàriament
una variable.

```sage
((x+y+sin(x))^2).expand()
```

```sage
_.collect(sin(x))
```

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
quadrats_primers=[ k^2 for k in [2..29] if k.is_prime() ]
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
T=(1,2,3,4,5)
print(T[-1])
T[-1]=10
```
a part d'imprimir 5 ens surt un error que diu "'tuple' object does not
support item assignment". En canvi, observeu que amb llistes
no obtenim cap error:

```sage
L=[1,2,3,4,5]
print(L[-1])
L[-1]=10
print(L)
```

Com que les tuples són immutables, podrieu pensar que no podem posar una
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
`len`), i afeguir o treure un element donat (amb `remove` dona error si no
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

Les *keys* poden ser números, cadenes (strings), tuples de números o
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



## Exercicis


### Exercici 1


Obteniu una aproximació numèrica per a l'expressió
$\displaystyle{\frac {3 + \pi }{7 -
\sqrt{13}}}$ amb 10, 20 i 30 xifres.

-- begin hide
```sage
a = (3 + pi) / (7 - sqrt(13))
print(a.n(digits=10))
print(a.n(digits=20))
print(a.n(digits=30))
```
-- end hide

### Exercici 2


Considereu les assignacions $$\begin{gathered}
A=30\\
B=2\\
C=3/4\\
D=0.254\end{gathered}$$

-- begin hide
```sage
A = 30
B = 2
C = 3/4
D = .254
```
-- end hide

Doneu el valor exacte i una aproximació
numèrica per a les expressions:

- $(2A-B)^{-2}$

-- begin hide
```sage
print((2*A-B)^(-2))
print(((2*A-B)^(-2)).n(digits=10))
```
-- end hide

- $\cos(A+2C)$

-- begin hide
```sage
print(cos(A+2*C))
print(cos(A+2*C).n(digits=10))
```
-- end hide


- $\dfrac{1}{A+3D}$

-- begin hide
```sage
print(1 / (A + 3*D))
print((1 / (A + 3*D)).n(digits=10))
```
-- end hide


### Exercici 3


Utilitzeu la funció de substitució o d'aproximació numèrica per a
verificar si algun dels nombres $1$, $2$ o $3$ és solució de
l'equació $x^3-16x^2+51x-36=0$.

-- begin hide
```sage
var('x')
f = x^3 - 16*x^2 + 51*x - 36
print(f.subs(x = 1)) # És solució
print(f.subs(x = 2)) # No - val 10
print(f.subs(x = 3)) # És solució
```
-- end hide

### Exercici 4


Per a un valor del paràmetre $A$ arbitrari, les dues arrels del
polinomi $p(x)= x^2 -2 A x+1$ són
$$s_1=A+\sqrt{A^2-1} \quad \text{i} \quad s_2= A-\sqrt{A^2-1}$$

Substituint la indeterminada $x$ del polinomi $p(x)$ per $s_1$ i
$s_2$ (i fent la manipulació addicional que calgui), verifiqueu
l'afirmació anterior.

-- begin hide
```sage
var('x A')
p = x^2 - 2*A*x + 1
s1 = A + sqrt(A^2-1)
s2 = A - sqrt(A^2-1)
print(p.subs(x=s1))
print(p.subs(x=s2))
print(p.subs(x=s1).simplify_full())
print(p.subs(x=s1).simplify_full())
```
-- end hide

### Exercici 5


Doneu el desenvolupament de $(x+1)^n$ per a
$n=1,\ 2,\ 3,\ 4\ \text{i }23$.


-- begin hide
```sage
var('x')
print([((x+1)^n).expand() for n in [1, 2, 3, 4, 23]])
```
-- end hide

### Exercici 6

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

# Exercici 7

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
S = {n^2 + m^3 for n in range(10) for m in range(5)}.intersect(set(range(1,101)))
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


 
