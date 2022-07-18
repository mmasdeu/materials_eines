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
**2. Més Manipulacions Bàsiques**
:::

*Aclariments sobre l'ús de *`reset()`** :

La instrucció `reset('b')` deixa el nom `b` indefinit, tant si li havíem
assignat un valor (per exemple, `b=3`) com si l'havíem declarat variable
(amb `var('b')`). Si després l'usem en alguna expressió, sortirà el
missatge d'error `name 'b' is undefined`.

Es pot aplicar `reset()` a més d'una variable, fent `reset('a,b')`, amb
una coma entre les variables o amb un espai, fent fent `reset('a,b')`,
però no amb una coma i un espai: `reset('a, b')` no funciona.

Fent `reset()` deixem indefinits tots els valors que havíem assignat o
declarat variables fins al moment.

La expressió `del(b)` fa el mateix que `reset('b')`, però només funciona
amb una variable i sense cometes. Millor no utilitzar-la.

to 5cm

*Més coses sobre la Jupyter Notebook App:*

El [**SageMath**]{style="color: blue"} té dues parts que cal diferenciar
bé: Una és la Jupyter NoteBook App, que és la interfície on escrivim
instruccions de [**SageMath**]{style="color: blue"}. L'altra és el
*kernel*, que és el motor computacional que executa les ordres. Cada
notebook té un kernel executant-se al darrera, independent dels altres
notebooks que puguin estar oberts. Podem aturar el kernel sense tancar
la pestanya del notebook; o tancar la pestanya sense aturar el kernel (i
per tant tornar a obrir el notebook amb el kernel en el mateix estat).

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
    `New -> SageMath`, feu `Rename...` i poseu `Practica02` o el que
    vulgueu.

Recordeu que la manera neta de sortir del
[**SageMath**]{style="color: blue"} és fent `File -> Close and Halt` i
després tancant la pestanya. Podem aturar un kernel amb el menú
`Kernel -> Shutdown`, mantenint obert el notebook. Si fem
`Restart -> Run all`, es tornen a executar totes les instruccions *en
l'ordre en què estan en aquest moment*. Les ceł.les quedaran renumerades
dels del principi.

to 5cm

# La instrucció `expand`

A la pràctica anterior vam fer la substitució $a=(u-1)^2$ en l'expressió
$E=2a-3b+c^2$. Repetiu-ho i observeu que el quadrat queda tal qual
$(u-1)^2$. Si volem desenvolupar aquest quadrat, necessitem la funció
`expand()`.

1.  `E.subs(a=(u-1)^2).expand()`

2.  `expand(E.subs(a=(u-1)^2))`

Observeu altre cop que les dues sintaxis són equivalents. Hem
d'acostumar-nos a veure les dues formes.

Desenvolupeu les expressions $(a+b)^2$, $(a+b)^3$, $(a+b)^4$,
$(a+b+c)^2$, $(a+b)(c+d)$, $(a+b)^2(c+d)^2$. Recordeu que aplicant
`show()`, la sortida serà més agradable de llegir.

# La instrucció `factor`

La operació inversa a l'anterior és descompondre una expressió en
factors. Per fer això s'utilitza la funció `factor()`. Tingueu en compte
però que descompondre en factors és un concepte profund matemàticament
parlant, com veureu durant els estudis. Factoritzar depèn del "context"
matemàtic en què ens movem.

De tota manera, podem veure el que passa sense entrar en detalls subtils
(un cop més, es pot usar aquesta sintaxi o bé posar l'expressió a
factoritzar com a argument de la funció `factor()`):

1.  `(x^2-2*x+1).factor()`

2.  `var('a')`

3.  `(x^2-2*a*x+a^2).factor()`

4.  `(x^2-x-1).factor()`

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

1.  223344891012288664.factor()

Si hi posem un nombre negatiu, ens afegirà $-1$ als factors:

1.  -2019.factor()

Aplicat a un nombre primer, naturalment, obtindrem el propi nombre. Però
també podem preguntar específicament si un nombre és primer o no ho és,
amb la funció `is_prime()`. Mireu si són primers 138283 i 237761.
Fixeu-vos que el resultat és True o False. Es diu que aquesta funció
*retorna un valor booleà (Boolean value[^1])*.

I ja que estem amb nombres primers, quin és el següent primer d'un
primer (o de qualsevol nombre)? Això ho contesta la funció
`next_prime()`. Si volem saber quin és el nombre primer que està, per
exemple, a la posició 1500 de la llista de tots els primers, podem fer
`nth_prime(1500)`. Si fem `prime_range(1000,1097)` obtindrem tots els
primers entre 1000 i 1096. Proveu totes aquestes funcions.

Proveu també la funció `divisors()`, que ens dona tots els divisors d'un
número. Per exemple, obtingueu tots els divisors de 1024, i compareu amb
la seva factorització en primers.

# Simplificacions

Un dels problemes típics que ens trobem fent matemàtiques, tant amb
llapis i paper com amb ordinador, és haver obtingut una expressió
complicada després d'uns llargs càlculs, sospitar que l'expressió es pot
simplificar, i no veure com.

Aquesta sembla una tasca perfecta per a un *Computer Algebra System* com
el [**SageMath**]{style="color: blue"}. Cal tenir en compte però que no
és senzill explicar-li a l'ordinador els nostres processos mentals quan
simplifiquem, i que de vegades tampoc ens posaríem d'acord entre
nosaltres sobre quina és la expressió "més simplificada possible".

El [**SageMath**]{style="color: blue"} ja fa de manera automàtica
algunes simplificacions. Per exemple, avalueu $\dfrac{b(b-1)}{b(b+1)}$.
La divisió per $b$ a dalt i a baix és automàtica.

Fem-li fer una simplificació senzilla, que no es fa automàticament:

1.  `A = x^x / x`

2.  `A.simplify()`

No obstant, comproveu que `simplify()` fracassa amb
$\frac{b^2-1}{b-1}=b+1$. I tampoc "veu" la igualtat
$\sin^2 x+\cos^2 x=1$. Diguem que la funció `simplify()` és prudent, i
no toca res si no som més específics. Proveu ara:

1.  `A=(b^2-1)/(b-1)`

2.  `A.simplify_rational()`

3.  `B=1/(x+1)-1/(x-1)`

4.  `B.simplify_rational()`

5.  `C=sin(x)^2+cos(x)^2`

6.  `C.simplify_trig()`

Hi ha moltes més funcions per simplificar de manera específica. Hi ha
també la funció `simplify_full()`, que intenta successivament diverses
simplificacions. Totes les expressions anteriors se simplifiquen amb
`simplify_full()`; per tant, sembla aconsellable començar amb ella.

De tota manera, `simplify_full()` no ho fa tot. Per exemple,

1.  D=2\*log(sqrt(2) + 1) + 2\*log(sqrt(2) - 1)

2.  D.show()

3.  D.simplify_full().show()

4.  D.simplify_log().show()

Sou capaços de simplificar encara més l'ultima expressió obtinguda?

# La instrucció `collect`

Una altra tasca habitual amb què ens trobem els matemàtics en la
manipulació d'expressions que contenen símbols indeterminats és la
d'agrupar llurs sumands en termes de les potències d'una de les
variables, per tal d'obtenir una representació com a *polinomi* respecte
aquesta variable. La funció `collect()` intenta fer això, com podeu
comprovar en les instruccions següents. En aquestes instruccions veureu
que hi apareix el símbol `_` (guió baix, blanc subratllat,
*underscore*). Serveix per representar *l'últim resultat que s'ha
obtingut* i això ens evita introduir una variable per guardar
expressions si ja no s'utilitzaran més.

1.  `var('y')`

2.  `A=x^3-3*x^2*y+x^2-2*x*y-x-y^2*x+y^3+y^2-1`

3.  `A.collect(x)`

4.  `show(_)`

5.  `A.collect(y)`

6.  `show(_)`

L'expressió de la qual es vol fer "collect" no ha de ser necessàriament
una variable. Aquí tenim un exemple. Proveu successivament (i recordeu
que afegint `.show()` veurem molt millor els resultats)

1.  `((x+y+sin(x))^2)`

2.  `((x+y+sin(x))^2).expand()`

3.  `((x+y+sin(x))^2).expand().collect(sin(x))`

# Llistes

Una coł.lecció d'expressions separades per comes i delimitades per
claudàtors (parèntesis quadrats, *square brackets*) és el que en
[**SageMath**]{style="color: blue"} s'anomena una *llista*. Les llistes
serveixen per guardar en una variable única una sèrie d'expressions o
valors que conceptualment considerem relacionats d'alguna manera o tenen
una identitat comuna. Per exemple, la llista

1.  A=\[cos(t), sin(t), t\]

podria representar les tres coordenades d'una partícula que es mou en
l'espai, en funció del temps $t$. (Marginalment, quina figura descriu
aquesta particula?)

Es pot accedir individualment a cada element d'una llista a través del
seva posició a la llista: `A[k]` selecciona l'element $k$-èssim. Però
ATENCIÓ!: La primera posició correspon a $k=0$. Això és així tant en
[**SageMath**]{style="color: blue"} com en llenguatge C. Els elements
individuals de la llista anterior s'obtenen per tant fent `A[0]`,
`A[1]`, `A[2]`. Proveu-ho.

Proveu `A[3]`. Els [**SageMath**]{style="color: blue"} ens indicarà
amablement `list index out of range` (el C no serà tan amable).

Per concatenar dues llistes, és a dir, posar els elements d'una a
continuació dels de l'altra n'hi haurà prou amb *sumar-les*, ja que
l'operador `+` està programat per reconèixer aquesta situació.
Considerem per exemple les velocitats de la partícula anterior i
concatenem-les amb el vector de posicions:

1.  B=\[-sin(t), cos(t), 1\]

2.  C=A+B

3.  show(C)

Naturalment, no és el mateix `A+B` que `B+A`. La "suma" de llistes no és
commutativa.

Podem obtenir una llista amb part d'una altra llista, expressant un rang
d'índexs dins els claudàtors, i si posem una posició negativa accedim
als elements de la llista començant pel final:

1.  C\[1:5\]

2.  C\[1:5:2\]

3.  C\[-3\]

Experimenteu més per acabar d'entendre com funciona.

Com és d'esperar, cadascun dels elements d'una llista es pot modificar
individualment:

1.  `C[2] = t^2`

2.  `C[5] = 2*t`

3.  `show(C)`

Es poden afegir elements al final d'una llista amb la funció `append()`,
inserir un element nou davant d'una posició donada amb `insert()`, i
eliminar un element amb `remove()`. Comproveu el funcionament d'aquestes
funcions i experimenteu altres possibilitats a partir de les
instruccions

1.  `D=[1,t,t^2,t^3]`

2.  `D.insert(1,t^(1/2))`

3.  `show(D)`

4.  `D.remove(t^2)`

5.  `show(D)`

6.  `D.remove(t^9)`

(noteu que intentar eliminar un element que no és a la llista produeix
un missatge d'error).

Freqüentment necessitem fer llistes els elements de la qual segueixen
una pauta o fórmula, depenent d'un valor que va variant. Per exemple,
per fer una llista dels quadrats dels enters entre $1$ i $10$, farem

1.  `quadrats=[ k^2 for k in [1..10] ]`

Els claudàtors exteriors indiquen que estem fent una llista. La llista
estarà formada pels valors $k^2$, amb $k$ variant dins de la llista de
naturals de 1 a 10, que podem construir d'aquesta manera abreviada amb
els "dos punts suspensius" entre el principi i el final.

Les fórmules per a la construcció de llistes poden ser molt complicades.
Es pot fer que l'índex `k` avanci des de $a$ fins a $b$ (amb
$\text a\le k<b$), incrementant-se cada pas en $c$ unitats, utilitzant
l'expressió `range(a,b,c)`. Així els quadrats dels múltiples de $3$
(fins al quadrat de $18$) s'obtenen amb:

1.  `quadrats_m3=[ k^2 for k in range(3,19,3) ]`

2.  `show(quadrats_m3)`

També es poden afegir restriccions al rang dels índexs. Per exemple, la
instrucció següent generarà la llista dels quadrats dels nombres primers
entre $2$ i $29$

1.  `quadrats_primers=[ k^2 for k in [2..29] if k.is_prime() ]`

La funció `len()` compta quants elements té una llista.

1.  `len(quadrats_primers)`

I el resultat ens diu que hi ha 10 nombres primers entre 2 i 29.

# Exercicis {#exercicis .unnumbered}

En aquest punt, i abans de continuar, mireu de realitzar aquests
d'exercicis.

1.  Obteniu una aproximació numèrica per a l'expressió
    $\displaystyle{\frac {3 + \pi }{7 -
    \sqrt{13}}}$ amb 10, 20 i 30 xifres.

2.  Considereu les assignacions $$\begin{gathered}
    A=30\\
    B=2\\
    C=3/4\\
    D=0.254\end{gathered}$$ Doneu el valor exacte i una aproximació
    numèrica per a les expressions:

    (a) $(2\,A-B)^{-2}$

    (b) $\cos(A+2\,C)$

    (c) $\left(\dfrac{1}{A+3\,D}\right)$

3.  Utilitzeu la funció de substitució o d'aproximació numèrica per a
    verificar si algun dels nombres $1$, $2$ o $3$ és solució de
    l'equació $x^3-16\,x^2+51\,x-36=0$.

4.  Per a un valor del paràmetre $A$ arbitrari, les dues arrels del
    polinomi $p(x)= x^2 -2\, A\,
    x+1$ són
    $$s_1=A+\sqrt{A^2-1\,} \quad \text{i} \quad s_2= A-\sqrt{A^2-1\,}$$
    Substituint la indeterminada $x$ del polinomi $p(x)$ per $s_1$ i
    $s_2$ (i fent la manipulació addicional que calgui), verifiqueu
    l'afirmació anterior.

5.  Doneu el desenvolupament de $(x+1)^n$ per a
    $n=1,\ 2,\ 3,\ 4\ \text{i }23$.

6.  La funció `randint()` genera un nombre enter a l'atzar en el rang
    marcat pels arguments. Per exemple, cada cop que s'executa la
    instrucció `randint(1,6)` s'obté un nombre aleatori entre 1 i 6, amb
    la mateixa probabilitat per a tots (o sigui, estem llançant un *dau
    equilibrat*). Així, la instrucció

    1.  `L=[randint(1,6) for k in [1..100]]`

    o equivalentment

    1.  `L=[randint(1,6) for k in range(100)]`

    simula el llançament de 100 tirades de dau.

    Si fem

    1.  `L=[randint(1,6) for k in range(randint(10,200))]`

    obtindrem una llista de tirades aleatòries de dau, de longitud
    aleatòria entre 10 i 200.

    Amb aquesta última llista:

    (a) Determineu quants elements té.

    (b) Feu dues llistes, a partir dels elements de `L`, una amb els
        elements de les tirades parells i l'altre amb els de les tirades
        imparells.

[^1]: El nom prové de George Boole, matemàtic anglès (1815-1864).
