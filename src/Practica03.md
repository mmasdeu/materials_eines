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

# Gràfiques


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
dibcirc=plot(1-x^2/2, -pi/2, pi/2, color='red', legend_label='1-x^2')
dibcos=plot(cos(x), -pi/2, pi/2, color='green', legend_label='cos(x)')
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
point(\[(0,2), (-1,-3), (3,1.5)\],\
    xmin=-1.5, xmax=3.5, ymin=-3.5, ymax=2.5,\
    color='red', marker=\"s\", size=25, aspect_ratio=1)
```

A la mateixa pàgina d'abans `2D Plotting` de
[doc.sagemath.org](doc.sagemath.org) podeu veure totes les opcions
de la funció `point()`.

Si en comptes de representar punts isolats volem que apareguin els
segments que uneixen els punts, utilitzem la funció `line()`. L'argument
principal és la llista de punts, com en el cas anterior. Per exemple:

```sage
line(\[(0,2), (-1,-3), (3,1.5)\],\
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
interseccio`
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


