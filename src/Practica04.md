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

::: center
**4. Resoldre equacions**
:::

*Més coses sobre la pràctica anterior:*

1\. No tots els colors que surten en el diccionari `colors` funcionen
dins la funció `plot`, però sempre podem copiar les coordenades RGB i
posar-les en l'opció `rgbcolor=`. Fixeu-vos que en aquest cas no s'han
de posar les cometes `'`.

2\. L'opció `legend_label='...'` permet escriure, dins de les cometes,
instruccions de LaTeX, i queda més bonic. Per exemple, podeu provar
`$1-x^2$` i `$\cos x$` en les instruccions al final de la pàgina 3 de la
pràctica anterior. També hi ha l'opció `legend_color='...'`, que si es
fa coincidir amb el color de l'opció `color` crea un efecte visual
interessant.

3\. A l'exercici 6 de la pràctica 3, l'últim paràgraf correspon a un
altre exercici, que vam suprimir. Per si el voleu fer, aquí està
l'enunciat complet: Feu una llista amb les coordenades dels sis vèrtexs
de l'hexàgon regular inscrit en la circumferència de radi $1$, que són
els punts de la forma
$$\big(\cos(\tfrac{2k\pi}{6}),\sin(\tfrac{2k\pi}{6})\big), \quad\text{per a $k=0,\dots,5$}\ .$$
Utilitzant la llista anterior i una instrucció `line`, dibuixeu aquest
hexàgon.

to 5cm

*Més coses sobre la Jupyter Notebook App:*

1\. Quan arrenca la Jupyter Notebook App, veiem una pestanya amb el
directori base personal del nostre sistema operatiu, que anomenem en
anglès *home*. Típicament, en Windows és el directori
`C:\Users\nom_usuari`, i en Linux o MacOS és `/home/nom_usuari`. La
Jupyter no ens deixarà sortir d'aquest directori i els seus
subdirectoris.

Si ens interessa que el directori base de la Jupyter sigui un altre, per
exemple, `D:\Mates`, cal obrir des del menú inici el `SageMath Shell`, i
escriure, a la consola que s'obre, l'ordre\
`sage-sethome 'D:\Mates'`\
Després tanquem tot el que tinguem obert relacionat amb el
[**SageMath**]{style="color: blue"} i el proper cop tindrà efecte el
canvi.

2\. En el menú del notebook, `File -> Download As` permet convertir el
notebook a altres formats, com HTML o PDF. O al mateix format de
notebook (`.ipynb`) per guardar-lo en algun altre lloc.

to 5cm

# Equacions i inequacions

Si ens inventem una equació una mica complicada, segurament ningú la
sabrà resoldre. Tampoc el [**SageMath**]{style="color: blue"} serà capaç
de fer-ho. Però sí que té guardats molts trucs per trobar l'expressió
exacta de la solució o solucions de moltes equacions. I quan no és capaç
de trobar les solucions exactes, pot normalment trobar valors aproximats
amb molta precisió, si l'ajudem una mica.

Per començar a treballar, cal tenir en compte que per tal de construir
una equació en [**SageMath**]{style="color: blue"} s'utilitza
*l'operador de comparació* `==` . A més, cal que tingueu en compte que
una equació és un objecte com qualsevol altre tipus d'expressió i pot
estar guardada en un variable. Per tant, la instrucció

1.  `eq1 = x^3-5*x^2+23 == 2*x^2+4*x-8`

guarda en la variable `eq1` l'equació que en notació ordinària
escriuríem com $$x^3-5\, x^2+23 = 2\, x^2+4\, x-8 \ .$$ Podeu veure com
en la instrucció anterior hi ha dues menes d'igual: el `=` que correspon
a una assignació i el `==` que correspon a l'operació de comparació que
ens permet construir l'objecte "equació". També podeu veure el que la
funció `show` aplicada a un objecte de tipus equació mostra a la
pantalla.

1.  show(eq1)

De la mateixa forma, es poden crear inequacions canviant l'operador
lògic `==` per `<=`, `<`, `>=`, `>`, amb significat evident. Les
notacions `<>` i `!=` serveixen per indicar *no igualtat*. Recomenem
usar la segona, que és tal com s'escriu en llenguatge C.

1.  ineq = abs(x-5) \<= abs(x)+4

2.  show(ineq)

Una igualtat o desigualtat, quan està determinat si és certa o falsa (o
sigui, quan no hi ha símbols indeterminats), retorna un valor lògic
`true` o `false`. Per exemple, l'expressió `3 < 5` retorna un valor
`true`, mentre que `7 < 6` retorna `false` (comproveu-ho).

El C també utilitza `!` com operador de negació (el que transforma un
valor `true` en un valor `false` i viceversa). En canvi, en
[**SageMath**]{style="color: blue"} l'operador de negació és `not`.
Fixeu-vos bé en els resultat de les instruccions següents:

1.  show(3 \< 5)

2.  show(not 7 \> 6)

3.  show(0 != 0)

4.  show(not true)

5.  show(not false)

El dos costats d'una equació es poden extreure per separat (sense
modificar l'objecte inicial) amb les instruccions `lhs` i `rhs`,
abreviatura de *left-hand side* i *right-hand side*.

1.  show(eq1.lhs())

2.  show(eq1.rhs())

o sigui que podem construir una equació equivalent a la primera posant
tots els termes a un costat i deixant zero a l'altre (en un cert sentit,
és la forma "normalitzada" d'una equació):

1.  eq2 = eq1.lhs()-eq1.rhs()==0

2.  show(eq2)

Fent la gràfica de l'expressió de l'esquerra podem tenir una aproximació
visual de les solucions, com ja vam veure a la pràctica anterior.
Busqueu-les:

1.  plot(eq2.lhs())

# Solucions exactes

La funció `solve()` s'utilitza per (intentar) resoldre equacions.
Consderem aquesta: $3x^3-4x^2-43x+84=0$.

1.  `eq3 = 3*x^3-4*x^2-43*x+84==0`

2.  solucions = solve(eq3,x)

3.  solucions

4.  show(solucions)

Observeu que el `show()` aquí ens amaga el fet que `solve()` retorna una
*llista de equacions*. Això ens permet fer fàcilment la comprovació que
són solució, usant `subs()`:

1.  eq3.subs(solucions\[0\])

O bé

1.  bool(eq.subs(solucions\[0\]))

En lloc de provar les solucions una a una, podem aprofitar que estan en
una llista per comprovar-les totes a la vegada amb la funció `bool()`,
que retorna un valor *booleà*[^1].

1.  \[eq3.subs(solucions\[k\]) for k in range(0,len(solucions))\]

2.  \[bool(eq3.subs(solucions\[k\])) for k in range(len(solucions))\]

També podem substituir les solucions en qualsevol altra expressió. Per
exemple, anem a veure quins són els valors que pren l'expressió
$x^{3}-2x^{2}+ 3x-7$ avaluada en les solucions de l'equació
$3x^{2}-4x+3=0$.

1.  `expr = x^3-2*x^2-3*x+7`

2.  `eq4 = 3*x^2-4*x-3==0`

3.  `solucions = solve(eq4,x)`

4.  `show(solucions)`

5.  `show([expr.subs(s).expand() for s in solucions])`

Proveu també sense el `expand()`.

No podem aplicar la funció `n()` d'aproximació numèrica directament a
una llista, però sí construir una llista amb les aproximacions
numèriques de cada element. Fixeu-vos com s'utilitza aquí la
substitució, i que és coherent amb el que hem vist fins ara.

1.  `[x.subs(s).n() for s in solucions]`

I el mateix podem fer amb l'avaluació de `expr` en les solucions de
`eq4`:

1.  show(\[expr.subs(s).n() for s in solucions\])

Per cert, pensant un moment el que tenen a veure entre sí les funcions
$x^{3}-2x^{2}+ 3x-7$ i $3x^{2}-4x+3$, què és el que hem calculat?

## Solucions múltiples

Quan alguna de les solucions d'una equació polinòmica té multiplicitat,
la funció `solve()` és capaç de descobrir-ho i podem obtenir aquesta
informació activant l'opció booleana `multiplicities`.

1.  `solve(x^3-11*x^2+7*x+147==0, x)`

2.  `solve(x^3-11*x^2+7*x+147==0, x, multiplicities=True)`

En aquest cas simple, ho podem veure també amb la factorització del
polinomi.

1.  `factor(x^3-11*x^2+7*x+147).show()`

## Solucions complexes

La funció `solve()` també pot donar les solucions complexes d'una
equació. Per exemple, l'equació $x^3-1=0$ només té una solució real
(l'arrel cúbica de 1). Però hi ha dues solucions més dins del conjunt
dels nombres complexos. Per tant, en els complexos, el número 1 té tres
arrels cúbiques diferents:

1.  `show(solve(x^3-1==0,x))`

En molts casos les expressions que apareixen poden resultar força
difícils de llegir i interpretar. Considerem el problema de buscar les
arrels del polinomi $x^3-34x^2+4$.

1.  `poli = x^3-34*x^2+4`

2.  `arrels = solve(poli==0, x)`

3.  `show(arrels)`

Sembla que ens doni tres solucions amb part real i part imaginària. Però
en aquest cas es pot comprovar, prenent una aproximació numèrica, que
les solucions són tres nombres reals (havíem dibuixar la gràfica
d'aquest polinomi a la pràctica anterior).

1.  `show([x.subs(s).n() for s in arrels])`

Encara es veu part imaginària, però és zero dins dels marges d'error de
l'aproximació numèrica. Podeu augmentar el nombre de dígits i encara és
més clar.

I ho podem confirmar encara millor si fem primer una simplificació:

1.  `show([x.subs(s).simplify_full().n() for s in arrels])`

Es poden trobar exactament totes les solucions d'una equació polinomial
de grau fins a quatre. Però ja per a grau tres la fórmula és horrible:

1.  `var('a b c d')`

2.  `show(solve(a*x^3+b*x^2+c*x+d==0, x))`

## Equacions no polinomials

Hi ha equacions no polinomials que també es poden resoldre exactament.
Moltes altres no. La funció `solve()` fa el que pot. Considereu
$5 e^{x/4}=43$. Aquesta equació no és polinòmica, però a mà podem aïllar
fàcilment la $x$ i expressar l'única solució que té com el logaritme
d'alguna cosa. També `solve()` la troba:

1.  `show(solve(5*e^(x/4)==43, x) )`

Considereu ara trobar les solucions de $\sin x = 1/2$. Sabem de memòria
que $\pi/6$ és solució,

1.  solve(sin(x)==1/2, x)

però també sabem que n'hi ha infinites, perquè el sinus és una funció
periòdica.

Es pot millorar el resultat anterior afegint una opció a la instrucció
que farà que el resultat sigui una representació de totes les solucions.

1.  solve(sin(x)==1/2, x, to_poly_solve='force')

2.  show(\_)

Els $z$ amb subíndex que apareixen s'han d'interpretar com nombres
enters qualssevol. És a dir: Per a qualssevol nombres enters que hi
posem en cadascun d'aquests $z$, s'obté una solució de l'equació.

Aquesta opció és útil no només per equacions amb funcions
trigonomètriques. Per exemple, segur que podeu trobar a mà les dues
solucions de $\big|1-|1-x|\big|=10$; però `solve()` no pot amb les
opcions per omissió:

1.  solve(abs(1-abs(1-x)) == 10, x)

2.  `solve(abs(1-abs(1-x)) == 10, x, to_poly_solve='force')`

Per veure més opcions i exemples d'ús de `solve()`, busqueu a internet\
`sagemath symbolic equations and inequalities`

# Solucions aproximades

Quan la funció `solve()` no pot determinar les solucions d'una equació
només queda el recurs d'intentar obtenir-ne aproximacions numèriques.
Per posar un exemple, es pot mirar de resoldre l'equació polinòmica
$$x^5-2\,x^3-17\,x^2-6\,x+2=0$$ (de grau $5$ i, per tant, amb una
solució real com a mínim) i veurem com `solve()` no pot determinar cap
solució a no ser que es permeti a aquesta funció donar aproximacions
numèriques amb l'opció `to_poly_solve`.

1.  `eq4 = x^5-2*x^3-17*x^2-6*x+2==0`

2.  `solve(eq4, x)`

3.  `solve(eq4, x, explicit_solutions=True)`

4.  `solve(eq4, x, to_poly_solve=True)`

(I d'aquesta forma apareixen les aproximacions de les solucions
complexes i tot!)

Naturalment, les coses es compliquen encara més si l'equació no és
polinòmica. Per exemple, la funció `solve()` no coneix cap algorisme que
li permeti descobrir solucions d'una equació com $$x^3+1=e^x$$ que té,
com a mínim, la solució òbvia $x=0$.

1.  `eq5 = x^3+1==e^x`

2.  `solve(eq5, x)`

3.  `solve(eq5, x, to_poly_solve=True)`

Observeu que la segona línia fa alguna cosa, però no resol l'equació. I
l'algorisme invocat per `to_poly_solve` no ens dóna res.

No queda més remei que usar la funció `find_root()`, que calcula una
aproximació per *mètodes numèrics*. Els mètodes numèrics habitualment
involucren un procediment d'aproximacions successives, per al qual cal
donar un valor inicial el més proper possible a la solució, o bé un
interval on buscar la solució.

Per tal de trobar la solució $x=0$ de `eq5` li donarem un interval que
contingui el zero:

1.  eq5.find_root(-0.5, 0.5)

Noteu que el resultat és una molt bona aproximació de zero.

Per trobar les altres solucions, feu les gràfiques que us permetin
determinar intervals on n'hi hagi només una, i useu l'instrucció
anterior amb els diferents intervals. Quantes en trobeu?

Un altre exemple en el que trobar totes les solucions dóna una mica de
feina és l'equació $$\frac{x^2}{20}-10 x= 15\cos(x+15)$$ Un gràfic de
les dues corbes corresponents als dos costats de l'equació, sobre un
domini més o menys raonable, mostra de forma immediata que cal buscar
una solució entre $1$ i $2$:

1.  `eq6 = x^2/20-10*x == 15*cos(x+15)`

2.  `plot(eq6.lhs(), -10, 10, color='red') + plot(eq6.rhs(), -10, 10)`

3.  eq6.find_root(1, 2)

Però l'aparent recta que surt en el dibuix no és tal. És un tros de
paràbola, que forçosament ha de tornar a pujar en algun moment. Podem
veure a mà (o explorant més la gràfica) que tornarà a tocar l'eix
d'abscisses en $x=200$, i per tant hi haurà una solució de `eq6` no
gaire lluny d'allà.

1.  plot(eq6.lhs(), 150, 250, color='red') + plot(eq6.rhs(), 150, 250)

O, acostant una mica més el zoom,

1.  plot(eq6.lhs(), 195, 205, color='red') + plot(eq6.rhs(), 195, 205)

Queda clar que la solució està només una mica més enllà de $200$.

1.  eq6.find_root(200, 200.5)

# Sistemes d'equacions

La funció `solve()` també es pot utilitzar per a resoldre sistemes
d'equacions. És suficient introduir el sistema com una llista
d'equacions (`[eq1, eq2, … ]`) i explicitar de quines incògnites es vol
obtenir la solució. En particular, per sistemes d'equacions lineals tot
funciona molt bé. Aquí tenim un parell d'exemples:

1.  var('y')

2.  sistema = \[ 3\*x+2\*y==3, x-y==-4 \]

3.  solve(sistema, x, y)

```{=html}
<!-- -->
```
1.  var('y z')

2.  sistema2 = \[ x+y+z==1, 3\*x+y==3, x-2\*y-z==0 \]

3.  solucio2 = solve(sistema2, x, y, z); show(solucio2)

Podeu comprovar que, efectivament, els valors corresponen a la solució
del sistema fent les substitucions corresponents

1.  \[eq.subs(solucio2) for eq in sistema2\]

Per a sistemes lineals indeterminats, el resultat conté els paràmetres
lliures que calguin.

1.  var('y z')

2.  sistema3 = \[ 2\*x-y+4\*z==1, 3\*x+y-z==2 \]

3.  solucio3 = solve(sistema3, x, y, z); show(solucio3)

4.  \[eq.subs(solucio3) for eq in sistema3\]

Si ja sabem que el sistema és indeterminat, es pot fer que els
paràmetres lliures siguin determinades incògnites forçant a solucionar
respecte les altres

1.  solve(sistema3, x, y)

2.  solve(sistema3, x, z)

Si demanem la solució d'un nombre massa petit d'incògnites no s'obté
res:

1.  solve(sistema3, x)

Quan els sistemes no són lineals, el problema és molt més complicat però
la funció `solve()` farà el que pugui. Per exemple, en la intersecció de
dues *còniques* (corbes de grau $2$) donarà, normalment, les quatre
solucions possibles (comptant les complexes).

Es pot obtenir les solucions amb radicals com en l'exemple següent:

1.  var('y')

2.  `conica1 = 2*x^2+3*y^2-2*x+y-5`

3.  `conica2 = 3*x^2-y^2+2*x+y-1`

4.  `show(solve([conica1==0, conica2==0], x, y))`

O una solució numèrica com en aquest altre:

1.  `conica3 = 2*x^2+y^2-2*x+y-1`

2.  `conica4 = x^2-y^2+2*x+y-1`

3.  `show(solve([conica3==0, conica4==0], x, y))`

# Exercicis {#exercicis .unnumbered}

1.  Calculeu totes les solucions de les equacions:

    (a) $x^3-5x^2-29x+105 =0$

    (b) $x^3 -175+25x-7x^2=0$

    (c) $\mathrm{e}^{x^2}=1-x^2$

2.  Determineu els valors de $x$ que satisfan:

    (a) $|4x^2-3x+1|>0$

    (b) $3<|2x-5|\leq 6$

3.  Determineu totes les solucions que es troben a l'interval
    $[-7\pi, 11\pi]$ de les equacions:

    (a) $2\cos x  + 1=0$

    (b) $\sin^2(2x) -1 =0$

4.  Partint d'un gràfic significatiu, doneu aproximacions de *totes* les
    solucions reals de l'equació $$x^5-4x^3+3x^2+7x-1=0$$

5.  Feu un gràfic en què apareguin les corbes $y= 10-x^2$ i
    $y=4\sin(2x) +5$ per als valors de $x$ entre $-5$ i $5$. Després

    (a) Observant el gràfic, feu una estimació del valor de les
        abscisses ($x$) dels dos punts de tall de les corbes que
        s'aprecien.

    (b) Doneu una aproximació numèrica acurada d'aquests dos valors amb
        `find_root`.

    (c) Doneu els valors de l'ordenada ($y$) corresponents al dos valors
        anteriors avaluant qualsevol de les dues expressions.

    (d) Feu la gràfica de les dues corbes anteriors al voltant del punt
        $(1,9)$, on sembla que estiguin a prop una de l'altra, per tal
        de confirmar que no hi ha cap altre punt de tall en el domini
        que s'ha fixat al principi.

6.  Doneu l'expressió general de les solucions del sistema
    $$\left.\begin{aligned}
    x+2y +z &= 2\\
    3x+y &= 1
    \end{aligned}
    \right\}$$ i calculeu, com a mínim, tres solucions concretes.

[^1]: Els valors lògics (true, false) també s'anomenen booleans, en
    referència a George Boole (1815-1864).
