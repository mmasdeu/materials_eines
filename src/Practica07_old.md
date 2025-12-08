---
jupyter:
  title : 'Pràctica 7: Càlcul infinitesimal bàsic'
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

# Càlcul infinitesimal bàsic

## Límit d'una expressió

Per tal de calcular límits, tant si es tracta d'expressions que depenen
d'un índex que tendeix a infinit (successions) o d'una variable contínua
que tendeix a un cert valor (funcions d'una variable)
**SageMath**  proporciona la funció `limit`. La
construcció més simple d'una instrucció `limit` serà de la forma
`limit(expr,x=lmt)`, on `expr` és l'expressió de la qual se'n vol
obtenir un límit, `x` la variable i `lmt` el punt cap on tendeix aquesta
variable (pot ser infinit, que es representa per `infinity` o, si voleu
ser curts, amb `oo`, dues `o` minúscules, amb el signe `+`/`-`
corresponent si cal). En els exemples següents podeu veure alguns casos
en els que obtindreu directament el resultat que, segurament, ja us
espereu:

```sage
var('k')

limit(k/(k+1),k=infinity)
```

```sage
limit(k*(sqrt(k+1)-sqrt(k-1))/sqrt(k),k=infinity)
```

```sage
limit(k.factorial()/k^k,k=oo)
```

```sage
limit((-1)^k*sqrt(k)*sin(k^k)/(k+1),k=oo)
```

```sage
limit((sqrt(k+1) - sqrt(k) + 1)^sqrt(k),k=oo)
```

```sage
limit((k^3-8*k+7)/(200*k+1024),k=-oo)
```

```sage
limit(e^k,k=oo)
```

```sage
limit(e^k,k=-oo)
```

I en alguns casos en què la variable no tendeix a infinit:

```sage
limit(sin(x)/x,x=0)
```

```sage
limit((x-sin(x))/x^3,x=0)
```

```sage
limit((1-cos(x)) / x^2,x=0)
```

```sage
limit((x^3 + 3*x^2-x-3)/(x-1),x=1)
```

```sage
limit(e^(-1/x^2)/x^5,x=0)
```

Probablement haureu notat que, per defecte,
**SageMath** (ni cap altre programari de càlcul
simbòlic) no té mecanismes *per veure* en els límits amb la variable
tendint a infinit si el problema correspon a una variable contínua o a
un índex enter. És per això que si calculeu una expressió del tipus

```sage
limit(cos(2 * pi * k), k=infinity)
```

pensant que els múltiples de $2\pi$ tenen el cosinus igual a $1$ i,
per tant, esperant que el resultat sigui aquest, veureu que la resposta
serà que no existeix tal límit ja que la suposició inicial només és
vàlida si $k$ és un enter. Per tal de poder tractar aquestes situacions
cal *fer suposicions* sobre el contingut de les variables o dels
paràmetres que apareixen en el càlcul. La funció que permet fer això és
`assume` i en l'exemple anterior es podria utilitzar de la forma
següent:

```sage
var('k')
assume(k,'integer')
limit(cos(2 * pi * k),k = oo)
```

Cada cop que s'executa una instrucció `assume` s'afegeix una restricció
nova sense oblidar les anteriors. Si es vol conèixer en qualsevol moment
quines són les restriccions actives es pot utilitzar `assumptions()`
(que amb una llista de variables com argument dona les restriccions
corresponents a aquestes variables). Si s'han de modificar les
*suposicions* que s'han fet, convé *oblidar* les que hi hagi en un
determinat moment per tal de crear les noves. Si no es fa així, i mentre
no s'introdueixi una suposició incompatible amb les anteriors, les
suposicions noves s'afegeixen a les existents. Per tal d'oblidar
**totes** les restriccions que s'han fet fins un cert moment n'hi ha
prou amb la instrucció `forget()`, si només es vol *oblidar* algunes de
les restriccions caldrà especificar-les individualment amb el mateix
format que s'ha utilitzat en les instruccions `assume`.

Una altra de les opcions de la funció `limit` que és important tenir en
compte és la dels límits laterals. No és infreqüent tenir un límit que,
estrictament parlant, no existeix encara que potser algun dels dos
límits laterals (o tots dos) sí que està definit. Per tal de calcular
límits per la dreta o per l'esquerra n'hi haurà prou afegint l'opció
`dir=’+’` o `dir=’-’` (amb el significat usual) a la instrucció `limit`
corresponent.

Les línies següents mostren una situació on l'ús d'aquests mecanismes
mostra les diferents situacions que poden aparèixer

```sage
var('a')

f(x) = exp(a/x)

limit(f(x),x=0)
```

```sage
assume(a > 0)

limit(f(x),x=0)
```
```sage
limit(f(x),x=0,dir='+')
```
```sage
limit(f(x),x=0,dir='-')
```
```sage
forget(a > 0)
assume(a < 0)
limit(f(x),x=0)
```
```sage
limit(f(x),x=0,dir='+')
```
```sage
limit(f(x),x=0,dir='-')
forget(a < 0)
```

**Nota:** Teniu en compte que el mecanisme del `assume` no és
infal·lible. Hi ha moltes situacions en les que resulta molt difícil
incloure dins els càlculs d'una funció concreta aquestes restriccions.

## Derivades

La funció `diff` permet obtenir la derivada d'una expressió qualsevol
respecte de la variable que es vulgui. Així, per exemple, es pot fer

```sage
diff(x^3-2*x^2+4*x-5,x)
```
```sage
(x^3-2*x^2+4*x-5).diff(x)
```

Encara que, en realitat, si l'expressió només conté una variable, no cal
especificar respecte què es vol derivar ja que
**SageMath** ja ho dedueix pel seu compte.

```sage
diff(x^3-2*x^2+4*x-5)
```

```sage
(x^3-2*x^2+4*x-5).diff()
```

També es poden derivar *funcions* i el resultat és la *funció derivada*:

```sage
f(x)=tan(a*x^2)
df = f.diff()
show(df)
```

(Noteu que, com que **SageMath** *recorda* que la
variable de la funció `f` és `x` no cal especificar-ho a la instrucció
que calcula la derivada).



Si s'han de calcular derivades d'ordre superior no cal encadenar
instruccions `diff` com seria
```sage
ddf = diff(df)
show(ddf)
```

Es pot especificar com un argument addicional després de la variable

```sage
diff(f(x),x) # Derivada,

diff(f(x),x,x) # Segona derivada,

diff(f(x),x,x,x) # Tercera derivada...
```

O més en general,

```sage
diff(f(x),x,2) # Segona derivada,
```
```sage
diff(f(x),x,3) # Tercera derivada,
```
```sage
diff(f(x),x,12) # Dotzena derivada.
```

La funció `diff` permet combinar derivades d'una funció amb més d'una
variable. Per exemple, si tenim la funció
$g(x,y)= y e^{2x^2+x-1}$ podem pensar que $y$ és un
paràmetre fixat, i calcular la derivada de $g$ fent variar $x$:

```sage
g(x,y)=y*exp(2*x^2+x-1)
diff(g(x,y),x)
```

Però si es vol pensar la $x$ com a paràmetre i derivar $g$ com a funció
que depèn de $y$ n'hi ha prou amb:

```sage
diff(g(x,y),y)
```

Ara noteu que la idea de derivada primera o segona es pot combinar entre
les dues variables.

```sage
diff(g(x,y),x,x)
```
```sage
diff(g(x,y),y,y)
```
```sage
diff(g(x,y),x,y)
```
```sage
diff(g(x,y),y,x)
```

El fet que aquestes dues ultimes derivades coincideixin no és casual
(tot i que, estrictament parlant, tampoc és sempre cert).

## Estudi del gràfic d'una funció

Un dels problemes típics d'una assignatura de càlcul infinitesimal
elemental és el de determinar totes les característiques possibles del
comportament d'una funció (asímptotes, creixement, extrems, convexitat,
punts d'inflexió,...). En primera aproximació, el resultat d'una
instrucció `plot` i una mica de tempteig ja és suficient per a obtenir
resultats força satisfactoris però, en realitat, la raó de ser d'un
exercici d'aquest tipus és el fet que sense fer alguns límits,
solucionar algunes equacions i calcular unes quantes derivades no serà
possible obtenir de forma precisa aquest tipus d'informació. Com heu
vist, **SageMath** (i qualsevol eina de càlcul
simbòlic d'un cert nivell) proporciona totes les eines necessàries per
realitzar una tasca d'aquest tipus.


Per tal de veure com es van utilitzant els recursos de
**SageMath** de forma sistemàtica mirem d'obtenir
les característiques del gràfic de la funció $f$ determinada per la
fórmula
$f(x)= \dfrac{x^{3} + 6 x^{2} + 12 x + 8}{x^{2} + 4  x + 3}$.

*Observem primer quin és el domini màxim de definició de la funció*.
Clarament només tindrem un problema per avaluar la funció si el que es
troba al denominador és zero. Calculem, doncs, aquests punts i els
guardem en una llista `disc`:

```sage
slcns = solve(x^2+4*x+3 == 0, x)

disc = [x.subs(sol) for sol in slcns]
disc
```

Observeu que l'expressió $x^{2} + 4 x + 3$ que hem hagut d'analitzar
(corresponent al denominador) l'hem extret *manualment* de l'expressió
de $f(x)$. Si bé es podria haver utilitzat `f(x).denominator()` per
calcular aquest denominador, no sempre els punts que busquem vindran
d'igualar un denominador a zero. Això fa que la tasca de sistematitzar
el càlcul del domini màxim d'una funció necessiti, en molts casos,
aquesta intervenció manual.


Fixeu-vos que si tenim un polinomi de grau més gran haurem de demanar
solucions numèriques, com a:
```sage
solve(x^7-2*x+1==0, x, to_poly_solve=True)
```

on en surten totes les arrels, també les arrels que no són nombres
reals.

Per evitar això podem dir-li al **SageMath** que
la variable x només prendrà valors reals posant:

```sage
assume(x,"real")

solve(x^7-2*x+1==0, x, to_poly_solve=True)
```

També li podem demanar les arrels als reals amb doble precisió (surt una
llista de parelles (arrel, multiplicitat)):

```sage
(x^7-2*x+1).roots(ring=RDF)
```

de la que podem extreure fàcilment la llista de arrels reals.

*Un cop determinat aquest domini podem calcular els límits de la funció
quan la variable s'aproxima als seus extrems*. Les instruccions

```sage
f(x)=(x^3+6*x^2+12*x+8)/(x^2+4*x+3)

print(f'{limit(f(x),x=oo) = }')

print(f'{limit(f(x),x=-oo) = }')
```

mostren que no hi ha asímptotes horitzontals ja que són respectivament
$+\infty$ i $-\infty$. No obstant, com que els límits

```sage
m1=limit(f(x)/x,x=oo)
m2=limit(f(x)/x,x=-oo)
print(f'{m1 = }, {m2 = }')
```

existeixen i no són zero, hi ha l'opció que existeixin asímptotes
obliqües amb aquests valors com pendents. Per tal d'obtenir l'equació
d'aquestes asímptotes s'hauran de calcular els límits

```sage
n1=limit(f(x)-m1*x,x=oo)
print(f'{n1 = }')

n2=limit(f(x)-m2*x,x=-oo)
print(f'{n2 = }')
```

i, aleshores, es podrà assegurar que el gràfic és asimptòtic a la recta
$y=m_{1} x+n_{1}$ quan $x\to +\infty$ i a la recta $y=m_{2} x+n_{2}$
quan $x\to -\infty$. Definim, doncs, una funció que permeti guardar
l'expressió d'aquesta recta asimptòtica amb

```sage
asob(x) = m1 * x + n1
```

Per altra banda, en els punts on no es pot avaluar la funció que s'han
guardat a la variable `disc` i s'han determinat abans,

```sage
for a in disc:
    print(f"Límit quan x-> {a} per la dreta = {limit(f(x), x=a, dir='+')}")
    print(f"Límit quan x-> {a} per l'esquerra = {limit(f(x), x=a, dir='-')}")
```

mostra l'existència de les dues asímptotes verticals corresponents i el
comportament de la funció al seu voltant.

Els zeros de la funció es calcularan usant la instrucció `solve()`, que
també permet determinar els intervals on el signe de la funció és un o
l'altre.

```sage
solve(f(x)==0,x) # Zeros de f(x)
```
```sage
solve(f(x)>0,x) # On la funció és positiva
```
```sage
solve(f(x)<0,x) # On és negativa
```

*Ara, per estudiar el creixement i decreixement de la funció, és qüestió
de repetir el mateix estudi per a la funció derivada*:

```sage
df = diff(f)
show(df)
show(df.simplify_full()) # Versió compacta de la derivada
```

Observeu que el domini de definició de $f'(x)$ no ha canviat respecte el
de $f(x)$.

Calculem ara els punts crítics i els intervals de creixement i
decreixement de la funció:
```sage
solve(df(x) == 0,x) # Punts crítics de f(x)
```
```sage
solve(df(x) > 0,x) # On la funció creix
```
```sage
solve(df(x)<0,x) # On decreix
```

Observareu que hi ha tres punts crítics (màxims o mínims locals dels
valors de la funció) que es poden guardar en una llista:

```sage
sptscrt = solve(df(x)==0,x)

pcrt= [ x.subs(ss) for ss in sptscrt ]

print(pcrt)
```

Per altra banda, els intervals on la funció creix o decreix estaran
limitats per aquests punts crítics i els punts de discontinuïtat. Es pot
fer una llista d'aquests valors amb:

```sage
cridcr = sorted(disc+pcrt)

print(cridcr)
```

Per últim es pot passar a estudiar la concavitat i la convexitat de la
funció usant la segona derivada:

```sage
ddf = diff(f,2)

show(ddf)

show(ddf.simplify_full())
```

De nou, el domini de definició de $f''(x)$ no ha canviat. Però si
calculem els punts crítics de la derivada ($f''(x)=0$),

```sage
solve(ddf(x)==0,x)
```

obtenim un valor real i dos valors estranys que de fet és corresponen a
nombres complexos; aquests valors no ens interessen! Es poden evitar els
dos valor complexos de bon principi fent l'assumpció de que $x$ és real:

```sage
assume(x,'real')

solve(ddf(x)==0,x)
```
```sage
solve(ddf(x)>0,x) # On la funció és convexa
```
```sage
solve(ddf(x)<0,x) # On la funció és concava
```

I es veu que el punt $x=-2$ és un punt d'inflexió. Totes aquestes propietats es poden visualitzar al gràfic de la funció:

```sage
gf=plot(f,-8,5,ymin=-10,ymax=10,detect_poles='show')

ga=plot(asob,-8,5)

gf+ga
```

Els valors extrems de la variable $x$ ($-8$ i $5$) així com es valors
màxim i mínim de la $y$ s'han triat tenint en compte els resultats que
han anat sortint al llarg dels càlculs anteriors.

## Integrals

El càlcul integral amb **SageMath** és prou
intuïtiu. Donada, per exemple, la funció determinada per
$f(x)=x\sin(x)$, per calcular la integral definida
$\displaystyle\int_a^b f(x) dx$ es pot fer

```sage

f(x)=x*sin(x)

var('a b')

integral(f(x),x,a,b)
```

Els límits d'integració els hem introduït com una variable però,
òbviament, els podem canviar per qualsevol valor real o expressió (també
són vàlids $\pm\infty$).

```sage
integral(f(x),x,0,3)
```
```sage
integral(f(x),x,0,3).n()
```
```sage
integral(f(x),x,1.,2.)
```
```sage
integral(e^(-x^2),x,-infinity,infinity)
```

Recordeu que, si $f(x)$ és una funció positiva, la integral definida
entre $a$ i $b$ ens dona l'àrea entre la gràfica de la funció i l'eix
$x$. Això es pot estendre a funcions amb valors negatius entenent l'àrea
per sota de la funció com a àrea negativa.

Això es pot il·lustrar usant l'opció `fill()` en una instrucció
`plot()`. Mireu a l'ajuda les diferents possibilitats que ens ofereix.

```sage
plot(f(x),(x,0,4))+plot(f(x),(x,1,2),fill='axis')
```
```sage
integral(f(x),x,1,2).n()
```
```sage
plot(sin(x)/x,(x,-50,50),fill='axis')
```
```sage
integral(sin(x)/x,x,-50,50).n()
```


Si es vol calcular una *primitiva* de $f(x)$, és a dir, una funció
$F(x)$ tal que $F'(x)=f(x)$, simplement s'eliminen els límits
d'integració a la sintaxi anterior.

```sage
h(x)=integral(f(x),x)
print(h(x))
```
```sage
diff(h(x),x)
```

Ara bé, noteu que el resultat obtingut no té en compte la constant
d'integració $C$, de forma que, si el que volem és una primitiva
concreta, l'haurem d'ajustar. Per exemple, sabem que la funció
$F(x)=\displaystyle\int_0^x f(t) dt$ és la primitiva de $f(x)$ que
compleix $F(0)=0$. Observem que això no és cert per a la primitiva que
calcula **SageMath** en l'exemple anterior, però
que es pot ajustar fàcilment.

```sage
show(h(0))
F(x)=h(x)-h(0)
```

Es poden fer els càlculs d'algunes integrals treballant amb paràmetres,
tot i això, en alguns casos caldrà fer suposicions sobre aquests com ves
veu a l'exemple següent:

```sage
var('m')
integral(x^m,x)
```
```sage
assume(m != -1)
integral(x^m,x)
```
```sage
integral(x^(-1),x)
```

## Exercicis


### Exercici 1


Calculeu
$$
\lim_{k\to \infty} \left( \frac{\ln(k+1)}{\ln(k)}\right)^{(k \ln(k))}
$$

-- begin hide
```sage
var('k')
lim((ln(k+1)/ln(k))(k*ln(k)),k=infinity)
```
-- end hide

### Exercici 2


Calculeu els límits de la forma:

1. $\lim\limits_{x\to 3} \exp\left(\dfrac{a x}{x-3}\right)$

2. $\lim\limits_{x\to 2} \dfrac{x-a}{|x-b|}$

3. $\lim\limits_{x\to 0} \exp\left( \dfrac{a+\exp(-1/x^{2})}{x^{2}} \right)$

Tenint en compte com varia el resultat segons el valor dels
paràmetres $a$, $b$ i com són els límits laterals en cada cas.


-- begin hide


(a) $\lim\limits_{x\to 3} \exp\left(\dfrac{a\, x}{x-3}\right)$

```sage
forget()
var('a x')
assume(a>0)
show('limit quan a>0 per la dreta =\t ',lim(exp((a*x)/(x-3)),x=3,dir='+'))
```

```sage
show("limit quan a>0 per l'esquerra =\t ",lim(exp((a*x)/(x-3)),x=3,dir='-'))
```

```sage
forget(a>0)
assume(a<0)
show('limit quan a<0 per la dreta =\t ', lim(exp((a*x)/(x-3)),x=3,dir='+'))
```

```sage
show("limit quan a<0 per l'esquerra =\t ",lim(exp((a*x)/(x-3)),x=3,dir='-'))
```

(b) $\lim\limits_{x\to 2} \dfrac{x-a}{|x-b|}$

```sage
forget()
var('a b x')
show('limit quan b no és 2 =\t ',lim((x-a)/(abs(x-b)),x=2))
```

```sage
assume(a>2)
show('limit quan a> 2 i b és 2 =\t ',lim((x-a)/(abs(x-2)),x=2))
```

```sage
forget()
assume(a<2)
show('limit quan a<2 i b és 2 = \t ',lim((x-a)/(abs(x-2)),x=2))
```

(c) $\lim\limits_{x\to 0} \exp\left( \dfrac{a+\exp(-1/x^{2})}{x^{2}} \right)$

```sage
forget()
assume(a>0)
show('limit quan a> 0  = \t',lim(exp((a+exp(-1/x^2))/x^2),x=0))
```

```sage
forget()
assume(a<0)
show('limit quan a<0  = \t ',lim(exp((a+exp(-1/x^2))/x^2),x=0))
```

```sage
forget()
show('limit quan a és 0  = \t',lim(exp((exp(-1/x^2))/x^2),x=0))
```

-- end hide

### Exercici 3


Considereu la funció determinada per $h(x)=\dfrac{20}{x^2+4}$.
Calculeu la primera i segona derivades $h'(x)$ i $h''(x)$ i
representeu conjuntament les tres funcions.

Observareu que, el punt on el pendent a la gràfica de $h(x)$ és
màxim, és en efecte un màxim de $h'(x)$ i a més s'anuł.la $h''(x)$.
Determineu aquest punt. Sabeu com serà $h'''(x)$ en aquest punt?


-- begin hide


```sage
reset()
var('x')
h(x)=20/(x^2+4)
```

```sage
dh=h.diff()
ddh=dh.diff()
show(dh)
show(ddh)
```

```sage
gh=plot(h,-5,5,ymin=-5,ymax=6,  legend_label='Funció f')
gdh=plot(dh,-5,5,ymin=-5,ymax=6,color='red',legend_label='Derivada de f')
gddh=plot(ddh,-5,5,ymin=-5,ymax=6,color='green', legend_label='Segona derivada de f')
gh+gdh+gddh
```

```sage
S=solve(ddh(x)==0,x)
S=[x.subs(s) for s in S]
show("Punts on s'anul·la la segona derivada \t ", S)
```

```sage
dddh=ddh.diff()
pdddh = plot(dddh,-5,5,ymin=-5,ymax=6)
ptdddh = point([[s,dddh(s)] for s in S],color='red',title='Grafica de la tercera derivada amb els dos punts',size=25)
pdddh+ptdddh
```

```sage
ddddh=dddh.diff()
show('Valor de la tercera derivada en aquests dos punts = \t', {ddddh(s) for s in S}) 
```


-- end hide

### Exercici 4


Definiu la *funció* `h` de dues variables corresponent a
$$h(x,y)= \frac{x e^{xy} \sin(y^{2})}{\ln(x^{2}+y^{2}+2)}$$
i determineu

1. La *funció* corresponent a la derivada de `h` respecte la
        variable `x`.

2. La *funció* corresponent a la derivada de `h` respecte la segona
        variable (`y`).

3. Els valor de la derivada de `h` respecte `x` per a `x=1`,
        `y=1/2`.

4. La *funció* que s'obté derivant `h` respecte `x` dues vegades.


-- begin hide

```sage
h(x,y) = (x*exp(x*y)*sin(y^2))/(ln(x^2+y^2+2))
show(h)
```

La funció corresponent a la derivada de $h$ respecte la variable $x$:

```sage
show(diff(h,x))
```

La funció corresponent a la derivada de $h$ respecte la segona variable ($y$):

```sage
show(diff(h,y))
```

Els valor de la derivada de $h$ respecte $x$ per a $x=1$, $y=1/2$:

```sage
diff(h,x)(1,1/2)
```

```sage
diff(h,x)(1.,1./2)
```

La funció que s'obté derivant $h$ respecte $x$ dues vegades:

```sage
show(diff(h,x,x).simplify())
```

-- end hide


### Exercici 5
Creeu una funció de **SageMath** anomenada
`tangent`, que accepti com a paràmetres una funció $f(x)$, un punt
$a$ i una distància $h>0$, i doni com a resultat el gràfic de la
funció $f(x)$ junt amb la seva recta tangent en el punt $(a,f(a))$
en l'interval $[a-h,a+h]$.

-- begin hide
```sage
def tangent(f,a,h):
    f=f.function(x)
    dfa=diff(f(x),x).subs(x==a)
    rtan(x)=dfa*(x-a)+f.subs(x==a)
    gt=plot(rtan,a-h,a+h,color='red')
    gf=plot(f,a-h,a+h)
    return gt+gf

show(tangent(x^3,3,1))
show(tangent(exp(x^-2),5,.5))
```
-- end hide

### Exerici 6


Milloreu la informació que mostra el gràfic de la funció
$f(x)= \dfrac{x^{3} + 6  x^{2} + 12  x + 8}{x^{2} + 4  x + 3}$
que surt al text marcant els punts on hi ha extrems relatius i fent
que les regions de creixement i decreixement tenguin colors
diferents.

-- begin hide

```sage
f(x)=(x^3+6*x^2+12*x+8)/(x^2+4*x+3)
slcns=solve(f(x).denominator()==0,x)
disc=[x.subs(sol) for sol in slcns]
m1=limit(f(x)/x,x=oo)
n1=limit(f(x)-m1*x,x=oo)
asob(x)=m1*x+n1
df=diff(f)
sptscrt=solve(df(x)==0,x)
pcrt=[x.subs(ss) for ss in sptscrt]
cridcr=sorted(disc+pcrt)
creixement=solve(df(x)>0,x)
decreixement=solve(df(x)<0,x)
```

```sage
show('els punts crítics (on pot canviar la derivada) són \t', cridcr)
```

```sage
show('llocs on la fucnió creix  \t', creixement)
show('llocs on la fucnió decreix \t', decreixement)
```

```sage
show('Observem que la funció decreix del primer punt crític al últim punt crític, que són els extrems relatius')
extrems = [cridcr[0],cridcr[-1]]
show('Els extrems relatius són  \t  ', set(extrems))
```

```sage
gfce=plot(f,-8,cridcr[0],ymin=-10,ymax=10,detect_poles='show',color='green')
gfcd=plot(f,cridcr[-1],5,ymin=-10,ymax=10,detect_poles='show',color='green')
gfd=plot(f,cridcr[0],cridcr[-1],ymin=-10,ymax=10,detect_poles='show',color='blue')
ga=plot(asob,-8,5,color='grey')
pcridcr=point([(s,f(s)) for s in extrems],xmin=-8, xmax=5,color='red', marker="s", size=25)
gfce+gfcd+gfd+ga+pcridcr
```

-- end hide

### Exerici 7


Realitzeu l'estudi complet de la representació gràfica de la funció
$$g(x)= \frac{x^3-2}{x^2-3x+1}$$

-- begin hide

```sage
reset()
assume(x,'real')
g(x)=(x^3-2)/(x^2-3*x+1)
```

```sage
slcns=solve(g(x).denominator()==0,x)
disc=[x.subs(sol) for sol in slcns]
show(disc)
```

```sage
show(limit(g(x),x=oo))
m1=limit(g(x)/x,x=oo)
show(m1)
```

```sage
show(limit(g(x),x=-oo))
m2=limit(g(x)/x,x=-oo)
show(m2)
```

```sage
n1=limit(g(x)-m1*x,x=oo)
n2=limit(g(x)-m1*x,x=-oo)
show(n1)
show(n2)
asob(x)=m1*x+n1
```

```sage
dg=diff(g)
sptscrt=solve(dg(x)==0,x)
pcrt=[x.subs(ss) for ss in sptscrt]
show(pcrt)
cridcr=sorted(disc+pcrt)
cridcr=[c.n() for c in cridcr]
show(cridcr)
```

```sage
show([c.n() for c in cridcr])
```

```sage
solve(dg(x)>0,x) # On la funció creix
```

```sage
solve(dg(x)<0,x) # On la funció decreix
```

```sage
ddg=diff(g,2)
show(ddg)
show(ddg.simplify_full())
```

```sage
solve(ddg(x)==0,x)
```

```sage
solve(ddg(x)>0,x) # On la funció és convexa
```

```sage
solve(ddg(x)<0,x) # On la funció és còncava
```

```sage
gce=plot(g,-3,cridcr[0],ymin=-20,ymax=30,detect_poles='show',color='green')
gcd=plot(g,cridcr[-1],8,ymin=-20,ymax=30,detect_poles='show',color='green')
gd=plot(g,cridcr[0],cridcr[-1],ymin=-20,ymax=30,detect_poles='show',color='blue')
ga=plot(asob,-3,8,color='grey')
pcridcr=point([(s,g(s)) for s in pcrt], xmin=-3, xmax=8,color='red', marker="s", size=25)
gce+gcd+gd+ga+pcridcr
```

-- end hide

### Exercici 8

Representeu l'àrea de la regió del pla que queda entre els gràfics
de $y=\sin(x)$ i $y=\cos(x)$ corresponent als $x$ entre $0$ i
$2\, \pi$ en els que $\cos(x)\le\sin(x)$ dibuixant els gràfics
d'aquestes dues funcions en $[0,2 \pi]$ i ombrejant la regió. (Cal
que investigueu una mica en la documentació de `plot` per aconseguir
un bon resultat).

Determineu el valor de l'àrea d'aquesta regió calculant la integral
corresponent.

-- begin hide

```sage
s(x)=sin(x)
c(x)=cos(x)
```

Primer faig la grafica per a veure aproximadament on es tallen.

```sage
plot([s,c],0,2*pi)
```

Calculo les x's on es tallen les gràfiques amb find_root (numèricament).

```sage
r1=find_root(s(x)-c(x),0,1)
r2=find_root(s(x)-c(x),3,5)
```

Afegeixo al plot anterior el ombrejat entre s i c entre els punts r1 i r2.

```sage
plot([s,c],0,2*pi)+ plot(s,r1,r2, fill=c)
```

Determinem el valor de l'àrea d'aquesta regió:

```sage
integral(s(x)-c(x),x,r1,r2)
```

-- end hide
