---
jupyter:
  title : 'Pràctica 1: Manipulacions bàsiques'
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
# Manipulacions bàsiques


## Posada en marxa

Arrenqueu `SageMath Notebook` o `Jupyter Lab` de la manera
adequada segons la vostra instal·lació. S'obrirà el navegador d'internet per
omissió que tinguem a l'ordinador (o es crearà una nova pestanya si ja
estava obert). Dins el navegador veureu funcionant *Jupyter Lab*, que és un programari per editar i executar *notebooks*.

Els notebooks són com llibretes on podem escriure instruccions i
comentaris, i que després podrem guardar en fitxers.

El que ens ensenya Jupyter d'entrada és un panell amb una llista de
fitxers del nostre directori base (*Home*), que en Linux és típicament
`/home/usuari` i en Windows `C:\Users\usuari`. Si ja tenim notebooks
guardats en el directori base o algun dels seus subdirectoris, els podem
obrir des d'aquest panell.

Creeu un notebook nou anant a `File -> New -> Notebook` en el panell del Jupyter Lab,
a dalt a la dreta, i començarem a treballar escollint com a *Kernel* el **Sage 10.6**.

Haureu observat que just abans d'obrir-se el Jupyter Lab, s'ha obert també
el que s'anomena una *consola* o *terminal*, una petita finestra on van
apareixent informacions que de moment no ens interessen. No hem de tancar
aquesta consola fins que no acabem de treballar amb el
*SageMath*.

Més endavant veurem com canviar el directori base, i què és l'arbre de
directoris (*directory tree*) d'un ordinador.


## SageMath com a calculadora

Al clicar la icona (a l'apartat de Notebooks) que diu `SageMath` se'ns obre una pestanya nova, que conté el
notebook que estem creant. Escriurem instruccions en el rectangle on
posa `In [ ]`. Per executar una instrucció hem de fer **Shift** + **Enter**. Si fem
simplement **Enter**, generem una línia nova per escriure més instruccions, que
s'executaran després totes a la vegada.

Mireu de fer els primers càlculs investigant com escriure les
expressions següents. Tingueu en compte que els resultats de les
operacions surten en la forma més exacta possible segons el context.
Estem fent *càlcul simbòlic*, i no *càlcul numèric*.

(i) $\dfrac{23568}{23456}$

-- begin hide
```sage
23568 / 23456
```
-- end hide

(ii) $23456^{450}$

-- begin hide
```sage
23456^450
```
-- end hide


(iii) $\dfrac{2}{3}+\dfrac{1}{4}-\dfrac{4}{8}$

-- begin hide
```sage
2 / 3 + 1 / 4 - 4 / 8
```
-- end hide

(iv) $\sqrt{72\,}$

-- begin hide
```sage
sqrt(72)
```
-- end hide

(v) $\dfrac{1}{\sqrt{2\, }}$

-- begin hide
```sage
1/sqrt(2)
```
-- end hide

(vi) $2.345\times 5.89701$

-- begin hide
```sage
2.345 * 5.89701
```
-- end hide

(vii) $\sqrt[3]{64\,}$

-- begin hide
```sage
64.nth_root(3)
```
-- end hide

(viii) $\sqrt[4]{902.8654\,}$

-- begin hide
```sage
902.8654.nth_root(4)
```
-- end hide

(ix) $2\times 4- 5\times 3$

-- begin hide
```sage
2*4 - 5*3
```
-- end hide

(x) $2\times (4- 5)\times 3$

-- begin hide
```sage
2*(4-5)*3
```
-- end hide

(xi) $3^{4^{5}}$

-- begin hide
```sage
3^(4^5)
```
-- end hide

(xii) $\left(3^{4}\right)^{5}$

-- begin hide
```sage
(3^4)^5
```
-- end hide



Com és habitual, els símbols `+, -, *, /` representen suma, resta, producte
i divisió; els exponents s'introdueixen utilitzant el circumflex `^`; la
"coma decimal" és en realitat el "punt decimal" (recordeu-ho per
sempre!) i, naturalment, els parèntesis agrupen operacions.


Si ens equivoquem, no és problema. Podem sempre sobreescriure la
instrucció i tornar-la a executar.


Les funcions trigonomètriques, exponencials i logarítmiques també són
accessibles de forma immediata, com es pot comprovar amb els exemples
següents (copieu directament les expressions). Alguna d'elles provoca un
error (per què?).


```sage
sin(0)
```


```sage
sin(5*pi/6)
```


```sage
cos(0)
```


```sage
cos(0) / sin(0)
```


```sage
tan(pi/2)
```


```sage
cot(pi/3)
```


```sage
e^2
```


```sage
exp(2)
```

```sage
log(2.)
```


```sage
ln(2.)
```

```sage
log(2., 10)
```


```sage
cosh(1.)
```


```sage
(exp(1.)+exp(-1.))/2.
```


```sage
i^2
```


Noteu que les constants famoses ja venen predefinides: $\pi$ s'escriu
`pi`; el nombre $e$ es pot posar com `e` o com `exp(1)`; la unitat
imaginària $i=\sqrt{-1}$ es pot introduir com `i` o `I`.


**Nota:** En les instruccions de **SageMath** els
espais en blanc no solen tenir importància. És el mateix `3*5 + 2*7` que
`3*5+2*7`, o que `3 * 5 + 2 * 7`. O també que `3 * 5+2 * 7`, però
aquesta última versió és de molt mal estil, perquè visualment sembla que
estiguem fent una altra cosa.


## Guardar i tancar

Anem a guardar la feina feta i tancar el SageMath de manera neta. El més
ràpid és fer el següent:

-   `File -> Save`

-   `File -> Close and Halt`

-   Tancar les pestanyes del Jupyter.

-   Tancar la consola que se'ns havia obert al principi.

Si heu estat treballant en una aula de la facultat i us voleu guardar el
fitxer que heu creat, el trobareu al escriptori. Haurà quedat
guardat amb un nom tipus `Untitled.ipynb` o similar. Podeu canviar el
nom a `Practica01.ipynb` o al que vulgueu (però no canvieu el final
`.ipynb`). Copieu-lo a un pen-drive o envieu-vos-el per correu
electrònic.


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



## Aproximació decimal

En qualsevol càlcul on apareguin valors amb decimals s'obtindrà un
resultat també amb decimals. L'aproximació decimal d'una expressió
qualsevol s'obté amb la funció
`numerical_approx( )`, que afortunadament es pot abreviar `n( )`, o
també `N( )`. Per exemple, es pot obtenir una aproximació de $\pi$ amb
250 xifres decimals mitjançant:

```sage
n(pi, digits=250)
```

També es pot escriure d'aquesta manera que sembla més estranya:

```sage
pi.n(digits=250)
```

Aquí hem de pensar que `pi` és un objecte que té unes propietats. Una de
les propietats és la funció `n( )`. Aquesta funció `n( )` admet a més un
argument anomenat `digits`, que pot prendre valors enters. Tot aquest
llenguatge el veurem més endavant, però acostumeu-vos a aquesta sintaxi.



## Variables i objectes simbòlics

En qualsevol moment es pot usar una *variable* per guardar un *valor*,
mitjançant una *assignació* `variable = valor`,
on el `=` és *l'operador d'assignació*. Un cop feta una assignació,
*SageMath* substituirà cada aparició de
l'identificador `variable` pel seu valor associat `valor`. Les
instruccions següents donen un exemple on s'assigna a l'identificador
`R` el valor de l'arrel quadrada de 2, a continuació es calcula el valor
numèric del doble
d'aquest valor.

```sage
R = sqrt(2)
2*R
```
A vegades és útil utilitzar el símbol `_` (guió baix, blanc subratllat,
*underscore*). Serveix per representar *l'últim resultat que s'ha
obtingut* i això ens evita introduir una variable per guardar
expressions si ja no s'utilitzaran més.

```sage
_.n()
```

Haureu vist que la instrucció d'assignació no produeix cap resultat en
pantalla. Aquest és el comportament normal. Si es vol veure quin valor
té associat una variable es pot imprimir (per pantalla) el seu valor:

```sage
print(R)
```

Aprofitem per introduir aquí la funció `latex()`, que transforma la
resposta de **SageMath** en instruccions de LaTeX, que
us pot ser útil més endavant. De fet, la instrucció `view()` ens mostra el resultat visual de compilar l'expressió LaTeX:

```sage
latex(R)
```

```sage
view(R)
```


Cada cop que es fa una assignació nova a una variable s'oblida el valor
que tenia abans. La instrucció `reset(’variable’)` permet esborrar l’assignació a variable. Com a argument de `reset( )` podem posar la llista de les variables que
es volen desassignar. Si no es posa cap argument en `reset( )`
desapareixen totes les assignacions que hàgim fet. Com a exemple proveu
el següent:

```sage
a = 2
b = 2
c = 4
print(a, b, c)
```

```sage
expr = (x+b)^a+c
print(expr)
```

```sage
reset('a b')
```

**Atenció:** Es pot aplicar `reset()` a més d'una variable,
fent `reset('a,b')`, amb una coma entre les variables o amb un espai,
fent `reset('a b')`, però no amb una coma i un espai:
`reset('a, b')` no funciona.

```sage
a
```

```sage
c
```

Tot i així, la següent instrucció sí que funciona, ja que l'assignació
s'havia produït abans del `reset`:

```sage
print(expr)
```

La instrucció `reset('b')` deixa el nom `b` indefinit, tant si li havíem
assignat un valor (per exemple, `b=3`) com si l'havíem declarat variable
(amb `var('b')`). Si després l'usem en alguna expressió, sortirà el
missatge d'error `name 'b' is not defined`:

```sage
reset('b')
f = b + 3
```

L'expressió `del b` té el mateix efecte que `reset('b')`, però només funciona
amb una variable i sense cometes.


Quan es vol treballar amb equacions (on hi apareixen "incògnites") o
expressions que depenen de paràmetres indeterminats (per exemple,
polinomis) caldrà introduir objectes simbòlics (que són com ara
variables però que no tenen cap valor assignat). El símbol `x` ja té
aquest paper per omissió, com potser heu observat en l'exemple anterior;
però per a qualsevol altre símbol caldrà fer una *declaració* explícita.
La instrucció que genera, per exemple, el paràmetre indeterminat `y` per
tal de poder definir el polinomi en dues variables
$x^3 +3x^2 y^3-2 y^2 x +3y +5$ és

```sage
var('y')
```


Després podem fer:

```sage
polinomi = x^3 + 3*x^2*y^3 - 2*y^2*x + 3*y + 5
polinomi
```

Quan "desassignem" una variable, passa a ser un objecte simbòlic:

```sage
a = 5
print(a)
var('a')
print(a)
```


Les llistes de símbols que podem posar com a argument en la funció
`var( )` poden ser separats per blancs o per
comes, i marcats amb cometes simples (tecla d'apòstrof) o dobles. És a
dir, són equivalents `'a b'  "a b"  'a,b'  "a,b"`


## Substitucions

Sovint volem conèixer el valor d'una expressió quan un dels símbols que
conté pren un valor concret. La funció de
**SageMath** que permet fer això és `subs( )` i
s'aplica com una *propietat* o *mètode* de l'expressió, de la manera que
ja hem vist abans amb els dígits de $\pi$.

La funció `subs( )` no modifica ni l'expressió ni el valor de les
variables que se substitueixen. Només ens dona el valor resultant si es
fa la substitució.

```sage
var('a b c')
E = 2*a - 3*b + c^2
print(E)
print(E.subs(a=-1))
print(E.subs(b=11, c=2))
print(E.subs(a=-1, b=2, c=4))
print(E)
print(a, b, c)
```

Com veieu, es poden imprimir diverses coses en la mateixa línia,
posant-les dins un print, separades per una coma. També podem substituir variables per altres variables:

```sage
print(E.subs(a=b))
print(E.subs(b=a))
print(E.subs(a=b, b=a))
print(E.subs(a=b).subs(b=a))
```

```sage
var('u')
print(E.subs(a=(u-1)^2))
```

Opcionalment, però potser queda menys clar, es pot ometre la referència
explícita a `subs( )`, com ara en:

```sage
E(a=-1)
E(a=-1, b=2, c=4)
```


# Més Manipulacions Bàsiques



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
print(is_prime(138283))
print(is_prime(237761))
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
divisors(240)
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
f(x) = cos(pi*x) + 7
print(f)
```

(que generarà un símbol `f` que reacciona com la funció definida per la
condició $f(x)=\cos(\pi\, x)+7$ o, més concretament, qualsevol expressió
de la forma `f(v)` s'avaluarà com `cos(pi*v) + 7`). De forma que es poden
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
g(t) = 2*t^2 - 1
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
h(x,y) = x^2 - x*y + ln(x^2+y^2)
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
g = (x^2-y^2+sin(x*y)).function(x,y)
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

Dibuixeu la gràfica de $y=\sin x$ per a dos períodes complets.

### Exercici 7

Dibuixeu $y=3\, x^4-6\, x^2$ per al domini $[-10,10]$ amb l'escala
   automàtica per a les ordenades (eix de les $y$). Després d'observar
    la gràfica, editeu el domini i el recorregut per tal de veure amb
    claredat els talls de la gràfica amb l'eix d'abscisses. Feu una
    estimació d'aquests talls amb la informació del dibuix.


### Exercici 8

Dibuixeu la gràfica de l'expressió polinòmica
    $$p= x^5-2\,x^4+x^3-x^2+1$$ i determineu (a partir de la informació
    visual) el nombre de solucions que té l'equació
    $$x^5-2\,x^4+x^3-x^2+1=0.$$

### Exercici 9

Localitzeu els zeros del polinomi $$x^3-34 \, x^2+4$$ a partir del
    dibuix adequat (**atenció!!!** en té tres.)


### Exercici 10

Dibuixeu la gràfica de la funció donada per
    $$g(x)= \left| 2\, x+3 \right|- \left| 3-x \right|$$ i determineu
    (amb la precisió que permeti el dibuix) els punts $x$ on
    $g(x)\ge 8$.\
    (Idea: Potser és més pràctic fer la gràfica de $g(x)-8$).


### Exercici 11

Representeu els punts del pla $(1,4)$, $(-2,-3)$, $(4,-5)$ i
    $(-6,5)$ en color vermell i utilitzant un símbol diferent del que
    surt per omissió.

    Dibuixeu els segments que uneixen aquests punts en color verd, i
    representeu-ho tot en un dibuix únic.

