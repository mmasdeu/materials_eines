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
    display_name: SageMath 9.6
    language: sage
    name: sagemath

---
# Manipulacions bàsiques


## Posada en marxa

Arrenqueu `SageMath Notebook`, des del menú inici o de la manera
adequada segons la vostra instal·lació. S'obrirà el navegador d'internet per
omissió que tinguem a l'ordinador (o es crearà una nova pestanya si ja
estava obert). Dins el navegador veureu funcionant *Jupyter Notebook*, que és un programari per editar i executar *notebooks*.

Els notebooks són com llibretes on podem escriure instruccions i
comentaris, i que després podrem guardar en fitxers.

El que ens ensenya Jupyter d'entrada és un panel amb una llista de
fitxers del nostre directori base (*Home*), que en Linux és típicament
`/home/usuari` i en Windows `C:\Users\usuari`. Si ja tenim notebooks
guardats en el directori base o algun dels seus subdirectoris, els podem
obrir des d'aquest panel.

Creeu un notebook nou anant a `New -> SageMath` en el panel del Jupyter,
a dalt a la dreta, i començarem a treballar.

Haureu observat que just abans d'obrir-se el Jupyter, s'ha obert també
el que s'anomena una *consola* o *terminal*, una petita finestra on van
apareixent informacions que de moment no ens interessen. No hem de tancar
aquesta consola fins que no acabem de treballar amb el
*SageMath*.

Més endavant veurem com canviar el directori base, i què és l'arbre de
directoris (*directory tree*) d'un ordinador.


## SageMath com a calculadora

Al fer `New -> SageMath` se'ns obre una pestanya nova, que conté el
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
fent fent `reset('a b')`, però no amb una coma i un espai:
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
separades per una coma. També podem substituir variables per altres variables:

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


## Guardar i tancar

Anem a guardar la feina feta i tancar el SageMath de manera neta. El més
ràpid és fer el següent:

-   `File -> Save and checkpoint`

-   `File -> Close and Halt`

-   Tancar les pestanyes del Jupyter.

-   Tancar la consola que se'ns havia obert al principi.

Si heu estat treballant en una aula de la facultat i us voleu guardar el
fitxer que heu creat, el trobareu al escriptori. Haurà quedat
guardat amb un nom tipus `Untitled.ipynb` o similar. Podeu canviar el
nom a `Practica01.ipynb` o al que vulgueu (però no canvieu el final
`.ipynb`). Copieu-lo a un pen-drive o envieu-vos-el per correu
electrònic.

Seguirem estudiant el funcionament de la Jupyter Notebook App en
properes pràctiques.
