---
jupyter:
  title : 'Pràctica 3: Cadenes i Gràfiques de funcions'
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

# Cadenes i Gràfiques de funcions

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

Molt sovint volem construïr cadenes a partir de variables que tenim declarades.
Igual que fa la funció `print()`, **SageMath** sap convertir qualsevol objecte en
una cadena, i podem forçar-lo a fer-ho amb `str()`:

```sage
temperatura = 38
str(temperatura)
```

Això ens permet construïr cadenes

```sage
print('Avui hem arribat a ' + str(temperatura) + 'graus!')
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

## Definir funcions

Per tal de definir un objecte de **SageMath** que
reaccioni com una funció tal i com s'entén des del punt de vista
matemàtic (un objecte $f$ que admet arguments, com per exemple $x$, i
tal que, en funció del valor d'aquest argument, produeix un valor $f(x)$
lligat a fer una sèrie de càlculs dependents de $x$), la construcció més
senzilla és la de les que es denominen *funcions simbòliques*. S'obté un
objecte d'aquest tipus quan es fa una assignació com, per exemple, la
següent:

```sage
f(x)=cos(pi*x)+7
print(f)
```

(que generarà un símbol `f` que reacciona com la funció definida per la
condició $f(x)=\cos(\pi\, x)+7$ o, més concretament, qualsevol expressió
de la forma `f(v)` s'avaluarà com `cos(pi*v)+7`). De forma que es poden
avaluar expressions del tipus:

```sage
f(1/2)
```

```sage
f(1/3)
```

```sage
f(0.5)
```
```sage
f(0.5).n()
```

Noteu que aquesta construcció és molt semblant, però no del tot
equivalent, a escurçar la substitució (`subs`) d'una variable simbòlica
per un valor concret dins d'una expressió. Teniu en compte, no obstant,
que en la definició d'una funció no cal que la variable sigui una
variable simbòlica ja definida amb una instrucció `var` (el que
s'interpreta i queda guardat és el mecanisme de càlcul del valor
resultant en funció del valor introduït). Per tant, no hi ha haurà cap
inconvenient en definir una funció utilitzant com a nom de la seva
variable qualsevol que es vulgui. Per exemple:

```sage
g(t)=2*t^2 - 1
print(g)
```
```sage
g(x)
```
```sage
g(2)
```
```sage
g(1.345)
```

```sage
v=3.01
g(v)
```

Tot i això, aquesta construcció crea la variable que s'usa per definir
la funció. En particular, si aquesta variable està emmagatzemant un
valor, aquest es perdrà.

```sage
v = 5
```
```sage
u
```

```sage
v
```

```sage
F(u,v) = u*v
```
```sage
u
```
```sage
v
```


Evidentment, si es vol avaluar la funció prenent com argument un nom
sense valor assignat o sense haver declarat el símbol formal, apareix un
error fins que no donem una opció vàlida per a l'argument.

```sage
g(y)
```
```sage
var('y')
g(y)
```

El nombre d'arguments (variables) d'una funció és arbitrari i, per tant,
és perfectament raonable fer la definició següent:

```sage
h(x,y) = x^2-x*y+ln(x^2+y^2)
print(h)
```
```sage
h(2,3)
```
```sage
h(1,x)
```

## Convertir expressions en funcions

Hi ha situacions en les que s'ha obtingut una certa expressió, que depèn
d'un o més paràmetres, i es vol utilitzar aquest resultat com una funció
d'aquests paràmetres (o, potser, només d'alguns d'ells). La instrucció
que permet obtenir aquest resultat és la que apareix a la plantilla
`expresssio.function(variables)` (que es podria llegir com: "l'expressió
*tal* com a funció de les variables *qual*") i es pot comprovar el seu
funcionament en els exemples que venen a continuació.

```sage
expr = ln(3*x^3 + sqrt(x-1))
f = expr.function(x)
print(f)
```
```sage
f(1.2)
```
```sage
var('y')
print(f(y^2+1))
```

El mecanisme també és vàlid amb més d'una variable com a:

```sage
g=(x^2-y^2+sin(x*y)).function(x,y)
print(g)
```

```sage
print(g(2,pi))
```

L'ordre és important !
```sage
g=(x^2-y^2+sin(x*y)).function(y,x)
print(g(2,pi))
```

```sage
print(g)
```


I no cal que tots els paràmetres es converteixin en arguments de la
funció.

```sage
h = (x+y)*sin(pi*x)-cos(y)
hf = h.function(y)
print(hf)
```
```sage
print(hf(pi/2))
```


## Diferències entre expressions on hi apareixen indeterminades i funcions

Tot i que en molts casos no hi ha cap diferència pràctica entre definir
una funció o introduir una expressió que depèn de variables simbòliques
cal tenir en compte que aquests dos tipus d'objecte no són equivalents i
el seu comportament pot ser totalment diferent. Noteu, per exemple, com
els resultats d'aquestes instruccions semblen una mica contradictoris:

```sage
reset()
var('y z')

fyz = y^2 - 3*y*z + 2*z^2

f(y,z) = y^2 - 3*y*z + 2*z^2

print(fyz)
```
```sage
print(f)
```

```sage
y = 2
z = 3
print(fyz)
```
```sage
print(f(y,z))
```
```sage
print(f(2,3))
```

ja que, encara que assignem respectivament els valors $2$ i $3$ a les
variables `y` i `z` l'expressió `fyz` no reflecteix aquesta situació. És
més, si considereu una operació del tipus

```sage
y * fyz
```

podeu quedar una mica sorpresos. De fet, descobrireu que la definició
del símbol `fyz` *recorda* que els símbols `y` i `z` són variables
simbòliques i, aleshores, en l'expressió `y * fyz` hi ha dues `y`
diferents: la que té assignat el valor $2$ i la simbòlica que està
*dins* `fyz`. Naturalment, tal i com calia esperar, l'expressió `f(y,z)`
produeix el mateix resultat que `f(2,3)` encara que en el moment de la
seva definició s'utilitzin les variables simbòliques `y`, `z` per a
designar els arguments.


## Gràfica d'una funció

Quan volem fer la gràfica d'una expressió que depèn d'una variable, la
funció de **SageMath** que fa la feina és
`plot()`. La instrucció de l'exemple següent dibuixarà el resultat de
l'expressió $3x^{2}-8$ per als valors de $x$ entre $-5$ i $5$:

```sage
plot(3*x^2-8, x, -5, 5)
```

En realitat, especificar la variable és innecessari i s'obté el mateix
resultat amb `plot(3*x^2-8, -5, 5)`. Com altres vegades, `plot()` també
es pot aplicar com un *mètode* associat a l'objecte que volem dibuixar:

```sage
(3*x^2-8).plot(-5, 5)
```

La instrucció `plot` és prou flexible com per saber si l'argument que
s'introdueix és una funció. El seu mecanisme intern ja s'ocupa d'avaluar
aquest argument de la forma convenient. Per posar un exemple senzill:

```sage
reset()
f(x)= x*e^-x

plot(f,(-1,4))
```

donarà el mateix resultat que fer

```sage
plot(x*e^-x,(-1,4))
```
o també
```sage
f(x) = x*e^-x
plot(f(x),(x,-1,4))
```

## Asímptotes

Quan s'executa una instrucció `plot` el programa procura representar
tots els valors corresponents a l'interval de la variable que s'ha
especificat i ajusta les escales dels eixos per tal que el dibuix
resultant aparegui complet a la pantalla. De vegades, aquest
funcionament pot fer molt difícil interpretar el resultat. Per exemple,
si es vol representar $x/(x-2)$ per a $x$ entre $-5$ i $5$, la
instrucció estàndard

```sage
(x/(x-2)).plot(x, -5, 5)
```

ens fa un dibuix molt poc útil, puix que al voltant de $2$ els valors de
l'expressió tendeixen a $+\infty$ i $-\infty$, i l'escala del dibuix fa
que les altres característiques de la gràfica siguin inapreciables.

Per tal de millorar la situació, es poden especificar els valors màxim i
mínim de l'expressió que es representaran, utilitzant les opcions
`ymin`, `ymax`. Per exemple, restringint el rang de les $y$ als valors
entre $-15$ i $15$:

```sage
(x/(x-2)).plot(x, -5, 5, ymin=-15, ymax=15)
```

Si es vol ser del tot rigorós, està clar que la línia vertical que
apareix sobre $x=2$ no hauria de formar part de la gràfica, perquè en
$x=2$ tenim una divisió per zero. Sabem que en realitat aquesta línia
representa una *asímptota vertical*. Es pot fer que la instrucció
`plot()` eviti aquests artefactes utilitzant l'opció `detect_poles` com
en

```sage
(x/(x-2)).plot(x, -5, 5, ymin=-15, ymax=15, detect_poles=true)
```

Si es vol que es posin de manifest les asímptotes, sense que formin part
de la gràfica, es pot utilitzar la mateixa opció amb la forma
alternativa següent:

```sage
(x/(x-2)).plot(x, -5, 5, ymin=-15, ymax=15, detect_poles='show')
```

Ara l'asímptota apareix ressaltada però queda clar que no és part dels
valors de la expressió. Cal dir que amb expressions més complicades pot
ser que l'opció `detect_poles=’show’` no funcioni.

## Escales dels eixos

Hem vist que quan s'executa una instrucció `plot`, en principi l'escala
de cadascun dels eixos s'ajusta per tal que les dades que s'han calculat
apareguin en un rectangle de mides predefinides. L'escala dels dos eixos
pot ser diferent per tant. Per exemple, si s'intenta dibuixar mitja
circumferència com la gràfica de $\sqrt{1-x^{2}\,}$ per a $x$ entre $-1$
i $1$ utilitzant la instrucció:

```sage
plot(sqrt(1-x^2), x, -1, 1)
```

apareix en forma d'el·lipse i no de circumferència. Per tal que es vegi
la circumferència cal tenir la mateixa escala als eixos d'abscisses i
d'ordenades. Podem especificar escales iguals amb l'opció
`aspect_ratio`:

```sage
plot(sqrt(1-x^2), x, -1, 1, aspect_ratio=1)
```

El valor d'aquest argument és la *proporció* entre les escales dels dos
eixos. Per tant, es pot utilitzar per deformar el dibuix en el sentit
que es cregui convenient:

```sage
plot(sqrt(1-x^2), x, -1, 1, aspect_ratio=2)
```

```sage
plot(sqrt(1-x^2), x, -1, 1, aspect_ratio=1/2)
```

## Diverses gràfiques en un sol dibuix

Si es vol representar més d'una expressió en el mateix dibuix podem
utilitzar com a primer argument de la funció `plot` una *llista*
d'expressions:

```sage
plot([1-x^2/2, cos(x)], -pi/2, pi/2)
```

Si es vol diferenciar bé les dues gràfiques, es poden generar per
separat, cadascuna d'elles amb un color diferent, per exemple, i a
continuació combinar els dos dibuixos "sumant-los" (La línia aquí està dividida per tal de no sortir dels marges del text, però és *una sola* instrucció. En el **SageMath** podeu escriure un *backslash* (`\`) seguit immediatament de *Retorn* si voleu dividir una instrucció en diverses línies.)

```sage
plot(1 - x^2/2, -pi/2, pi/2, color='red') + \
    plot(cos(x), -pi/2, pi/2, color='green')
```

O, si interessa guardar cadascun dels dibuixos per separat,

```sage
dibcirc = plot(1-x^2/2, -pi/2, pi/2, color='red', legend_label='1-x^2')
dibcos = plot(cos(x), -pi/2, pi/2, color='green', legend_label='cos(x)')
show(dibcirc + dibcos)
```

Es poden utilitzar molts colors diferents. La variable predefinida
`colors` conté una llista dels noms dels colors, juntament amb el seu
equivalent en codificació RGB. En realitat no és una llista en el sentit
que hem vist a la pràctica anterior, sinó una estructura que s'anomena
*dictionary*, que relaciona *keys* (en aquest cas noms de colors) amb
*values* (codificació RGB de cada color). Podeu veure l'aspecte que té
escrivint l'ordre `colors`. Es pot obtenir una llista només amb les
*keys*, i ordenada alfabèticament, fent `sorted(colors)`, o millor
`show(sorted(colors))`.

Aquests són els colors predefinits, però es pot usar qualsevol altre
color mitjançant la seva codificació RGB:

```sage
plot(cos(x), -pi/2, pi/2, rgbcolor=(.9,.6,.5), legend_label='cos(x)')
```


No tots els colors que surten en el diccionari `colors` funcionen
dins la funció `plot`, però sempre podem copiar les coordenades RGB i
posar-les en l'opció `rgbcolor=`. Fixeu-vos que en aquest cas no s'han
de posar les cometes `'`.



## Help!

Podem demanar ajuda sobre l'ús d'una instrucció de
**SageMath** mitjançant la funció `help()`. Per
exemple, `help(plot)`, o bé `plot?` (la sortida és lleugerament
diferent) donen informació sobre la funció `plot()`.

```sage
plot?
```

De tota manera, no es tracta d'informació completa sobre totes les
opcions, sinó més aviat d'informació tècnica sobre com està definida la
funció internament.

Una possibilitat millor, si estem connectats a internet, és posar en un
buscador\
`sagemath plot`\
i segurament anirem a parar a l'explicació pertinent del website
oficial\
<http://doc.sagemath.org>\
on hi ha molta més informació i exemples. Proveu-ho.

Veureu que sota el títol `2D Plotting` hi ha moltes funcions que fan
dibuixos en dos dimensions. En particular, sota l'epígraf `plot()`,
trobareu totes les opcions d'escala, color, gruix i tipus de línia,
transparència, etc.

Per exemple, l'opció `legend_label='...'` permet escriure, dins de les cometes,
instruccions de LaTeX, i queda més bonic. També hi ha l'opció `legend_color='...'`, que si es
fa coincidir amb el color de l'opció `color` crea un efecte visual
interessant:

```sage
plot(cos(x), -pi/2, pi/2, rgbcolor=(.9,.6,.5), legend_label='cos(x)',\
legend_color=(.9,.6,.5))
```


## Punts i línies

A part de representar gràfiques de funcions, podem marcar punts de forma
individual indicant llurs coordenades, i també dibuixar segments entre
punts.

La funció `point()` permet representar punts o llistes de punts del pla.
Les coordenades de cadascun dels punts que es volen representar
s'introdueixen com un parell del tipus `(a,b)` (inclosos els
parèntesis). Hi ha opcions per al color (`color`), el símbol que
representa cada punt (`marker`) i la mida d'aquest punt (`size`):


```sage
point((1,1))
```

```sage
point([(0,2), (-1,-3), (3,1.5)],\
    xmin=-1.5, xmax=3.5, ymin=-3.5, ymax=2.5,\
    color='red', marker="s", size=25, aspect_ratio=1)
```

A la mateixa pàgina d'abans `2D Plotting` de
[doc.sagemath.org](doc.sagemath.org) podeu veure totes les opcions
de la funció `point()`.

Si en comptes de representar punts isolats volem que apareguin els
segments que uneixen els punts, utilitzem la funció `line()`. L'argument
principal és la llista de punts, com en el cas anterior. Per exemple:

```sage
line([(0,2), (-1,-3), (3,1.5)],\
    xmin=-1.5, xmax=3.5, ymin=-3.5, ymax=2.5,\
    color='red', aspect_ratio=1)
```

Com ja podeu suposar, el mecanisme de "sumar" dibuixos permet incloure
en un mateix objecte gràfic rectes, corbes i punts. Com a mostra podeu
veure el següent, on es representen (amb una estrella de color blau) els
punts d'intersecció de la recta $y=-3x+5$ (de color verd) amb la
paràbola $y= 9-x^2$ (de color vermell).

```sage
recta = plot(-3*x+5, -3, 5, color='green'); recta
parabola=plot(9-x^2, -3, 5, color='red'); parabola
interseccio=point([(-1,8), (4,-7)], color='blue', marker="*", size=100)
(recta+parabola+interseccio).show(xmin=-15, xmax=15, aspect_ratio=1)
```

Observeu que el dibuix final s'ha generat amb una instrucció `show()`,
que permet modificar alguna de les opcions dels dibuixos individuals.

## Exercicis

1.  Dibuixeu la gràfica de $y=\sin x$ per a dos períodes complets.



2.  Dibuixeu $y=3\, x^4-6\, x^2$ per al domini $[-10,10]$ amb l'escala
    automàtica per a les ordenades (eix de les $y$). Després d'observar
    la gràfica, editeu el domini i el recorregut per tal de veure amb
    claredat els talls de la gràfica amb l'eix d'abscisses. Feu una
    estimació d'aquests talls amb la informació del dibuix.



3.  Dibuixeu la gràfica de l'expressió polinòmica
    $$p= x^5-2\,x^4+x^3-x^2+1$$ i determineu (a partir de la informació
    visual) el nombre de solucions que té l'equació
    $$x^5-2\,x^4+x^3-x^2+1=0.$$



4.  Localitzeu els zeros del polinomi $$x^3-34 \, x^2+4$$ a partir del
    dibuix adequat (**atenció!!!** en té tres.)



5.  Dibuixeu la gràfica de la funció donada per
    $$g(x)= \left| 2\, x+3 \right|- \left| 3-x \right|$$ i determineu
    (amb la precisió que permeti el dibuix) els punts $x$ on
    $g(x)\ge 8$.\
    (Idea: Potser és més pràctic fer la gràfica de $g(x)-8$).



6.  Representeu els punts del pla $(1,4)$, $(-2,-3)$, $(4,-5)$ i
    $(-6,5)$ en color vermell i utilitzant un símbol diferent del que
    surt per omissió.

    Dibuixeu els segments que uneixen aquests punts en color verd, i
    representeu-ho tot en un dibuix únic.



7.  Feu una llista amb les coordenades dels sis vèrtexs
	de l'hexàgon regular inscrit en la circumferència de radi $1$, que són
	els punts de la forma
	$$\big(\cos(\tfrac{2k\pi}{6}),\sin(\tfrac{2k\pi}{6})\big), \quad\text{per a $k=0,\dots,5$}\ .$$
	Utilitzant la llista anterior i una instrucció `line`, dibuixeu aquest
	hexàgon (Tingueu en compte que també és molt possible que existeixi una
    instrucció del tipus `polygon`).



8. Definiu un diccionari on les claus siguint els nombres enters
   des de 2 fins a 10, i els valors els seus quadrats.



9. Expliqueu com ho farieu per crear una nova llista d'una llista `L` que tingui els mateixos elements que `L` però sense repeticions.



10. Donada una cadena `C`, expliqueu com crear un conjunt que contingui les lletres de `C`. Podeu fer un diccionari tal que les claus siguin les lletres de `C` i els valors el nombre de vegades que hi surt cada lletra? Proveu-ho per `C='Mississipi'`.
