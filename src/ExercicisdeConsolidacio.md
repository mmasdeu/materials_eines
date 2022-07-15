::: center
**Exercicis de consolidació**
:::

1.  L'objectiu d'aquest exercici és obtenir un gràfic dels punts del pla
    $(x,y)$ determinats per l'equació $x^{2} + x\, y + y^{2} - 6 \, x=0$
    (una eł.lipse). Per tal d'aconseguir això seguiu l'esquema següent:

    (i) Calculeu quins són els valors de $y$ que solucionen aquesta
        equació per a cada valor de $x$ donat utilitzant la instrucció
        `solve`. (Obtindreu dues solucions possibles per a cada $x$).

    (ii) *Convertiu* aquestes solucions en funcions per tal de poder
         avaluar, en funció d'un $x$ donat, els valors de $y$ que donen
         punts de la corba que es vol representar.

    (iii) Ja haureu notat que, com que l'equació és de grau $2$, les
          funcions anteriors no es podran avaluar per a $x$ arbitraris
          (hi ha una arrel quadrada que, segons el valor de $x$, no es
          podrà calcular). Determineu els valors de $x$ per als que és
          possible avaluar les funcions que heu introduït a l'apartat
          anterior (és a dir els valors de $x$ per als que hi ha algun
          punt de la forma $(x,y)$ que pertany a la corba).

    (iv) Combineu en un sol dibuix el gràfic de les dues funcions,
         restringint el domini de les $x$ a la regió on té sentit fer
         els càlculs.

2.  Considereu les successions determinades, donats dos nombres positius
    $a$, $b$, per
    $$S_{k}= \sqrt[k]{a^{k}+b^{k}\,}\,,\qquad T_{k}=\frac{a^{k}-b^{k}}{a^{k}+b^{k}}$$
    Per tal de poder experimentar amb els seus valors definiu dues
    funcions de tres arguments `(a,b,k)` que donin, respectivament, els
    valors de $S_{k}$ i $T_{k}$ per a un parell $(a,b)$ donat. A
    continuació feu una llista dels $25$ primers termes d'aquestes dues
    successions triant $2$ o $3$ parelles $(a,b)$ diferents. Finalment,
    després dels resultats dels experiments, quin límit conjectureu que
    tenen cada una d'aquestes dues successions?

3.  Considereu la successió $a_{k}$ definida per les condicions
    $$a_{0}=3\,,\ a_{k+1}=\frac{(a_{k})^{2}-1}{2\, a_{k}-3}$$ Feu una
    llista prou llarga dels seus valors per tal de poder conjecturar
    quin serà el seu límit. Què passa si canvieu el valor inicial
    $a_{0}$?

4.  Les parelles de primers bessons són parelles de primers de la forma
    $(p,p+2)$. Definiu una funció que permeti fer una llista de les
    parelles de primers bessons menors que un màxim donat.

5.  La conjectura de Goldbach afirma que tot enter parell més gran que 4
    es pot escriure com la suma de dos primers senars. Per exemple,
    $6=3+3$, $12=7+5$ o $64=17+47$. Aquestes particions com a suma de
    dos primers s'anomenen particions de Goldbach. Creeu una funció
    `Goldbach(n)` que retorni totes les possibles particions de Goldbach
    de $n$ (sense importar l'ordre). Si denotem per $r(2k)$ el nombre de
    particions de Goldbach de $2k$, la conjectura afirma que $r(2k)>0$
    per a tot $k>1$. Representeu en un gràfic els valors $(k,r(2k))$ per
    a $k\in [3,2000]$.

6.  Donat $k\in \mathbb{N}$, la funció *phi d'Euler*, $\varphi(k)$ és
    una funció que es defineix fàcilment en termes aritmètics, i que es
    pot calcular com
    $\varphi(k)=(p_1-1)\,p_1^{\alpha_1-1}\cdots (p_r-1)\, p_r^{\alpha_r-1}$
    on $k=p_1^{\alpha_1}\cdots p_r^{\alpha_r}$ és la descomposició de
    $k$ en primers diferents.

    (i) Factoritzeu un enter qualsevol fent `A=factor(....)` i observeu
        com s'estructura `A`, mirant per exemple la factorització i el
        contingut de `A[0]`.

    (ii) Useu això per a construir una funció que calculi $\varphi(k)$
         per a qualsevol natural $k$.

    (iii) Comproveu que el resultat coincideix amb el de la instrucció
          `euler_phi(k)`

7.  Considereu un joc d'atzar en el que es pot apostar entre dues
    opcions diferents igual de probables (cara o creu, parells o senars
    en la ruleta,...) de tal forma que cada cop que es guanya es
    recupera l'aposta i s'obté un premi de la mateixa quantitat (per
    tant, s'augmenta el capital amb un import igual a l'aposta que s'ha
    fet). És una creença força estesa entre els addictes al joc que
    l'estratègia consistent a fixar una aposta base, mantenint aquest
    import mentre es va guanyant i doblant l'aposta cada cop que es
    perd, condueix a l'èxit, ja que cada cop que es guanya després d'una
    ratxa dolenta es recupera tot el que s'havia perdut en les tirades
    anteriors i mentre es va guanyant s'acumulen beneficis. Per tal de
    comprovar si això és cert, feu una simulació d'aquest joc utilitzant
    com a model del fet de guanyar o perdre el resultat de la instrucció
    `randint(0,1)`, fixant un capital inicial de $100$ unitats, una
    aposta base de $1$ unitat i repetint el joc mentre es tinguin diners
    per apostar (el jugador s'arruïna) o s'arribi a acumular un capital
    de $1000$ unitats (moment en el qual el jugador es dóna per
    satisfet). Per tal de veure l'evolució del joc, feu que mentre es
    realitza la simulació es vagi guardant en una llista el capital
    acumulat fins el moment, de tal forma que, al final, es pugui
    dibuixar un gràfic de l'evolució d'aquest capital. Un exercici més
    complet consisteix a repetir moltes vegades l'experiment comptant al
    final la proporció de vegades que s'acaba guanyant la quantitat que
    satisfà al jugador.

8.  En aquest exercici veurem que es possible treballar amb relacions
    d'equivalencia i quocients per a conjunts finits. Considerarem un
    conjunt finit construït via $\{\ \}$ o be via `set( )`. Les següents
    funcions han de respondre un booleà que sigui `True` o `False`,
    depenen de la veracitat o no del que es vol comprovar.

    (i) Recordeu que una relació en un conjunt $S$ és un subconjunt de
        $S^2$. Per a poder definir $S^2$ com a conjunt podeu utilitzar
        ` {(a,b) for a in S for b in S}`. Definiu una funció
        `es_relacio` de [**SageMath**]{style="color: blue"} tal que
        donats dos conjunts $S$ i $R$ comprovi si $R$ és una relació de
        $S$; de pas pot comprovar també si els dos son conjunts amb
        `type(S)!=set`.

    (ii) Definiu funcions `es_reflexiva`, `es_simetrica`, i
         `es_transitiva` que comprovi primer si li passem una relació i
         després si la relació és reflexiva, simètrica, i transitiva
         respectivament. Definiu després una funció `es_equivalencia`
         que cridi a cadascuna de les funcions anteriors i respongui si
         és o no d'equivalencia.

    (iii) Definiu funcions `classe(a,S,R)` que calculi, després de
          comprovar si $R$ és una relació d'equivalència de $S$, el
          conjunt de tots els elements de $S$ que estan a la classe de
          $a\in S$, i una funció `quocient(S,R)` que respongui un
          conjunt (subconjunt de $S$) que contingui un representant de
          cada classe de $S/R$.

    (iv) Definiu una funció `projeccio(S,R)` tal que, donat un conjunt
         `S` i una relació `R` retorni un diccionari on les claus siguin
         els elements de `S` i els valor sigui l'element de
         `quocient(S,R)` que està relacionat amb ell, després de
         comprovar si $R$ és una relació d'equivalència de $S$.

    (v) Apliqueu aquestes funcions al conjunts $S=set([1..5])$ amb la
        relació $(a,b)\in R \Leftrightarrow a<b$ i a la relació
        $(a,b)\in R \Leftrightarrow a\le b$, i al conjunt $set(Zmod(10)$
        amb la relació $(a,b)\in R  \Leftrightarrow 2a=2b$.

9.  Considereu la família de matrius $A_t \in M_{3x4}(\mathbb{Q})$,
    depenent d'un paràmetre $t$, donada per $$A_t=\begin{pmatrix} 
        1 & t-1  & -1 & -2 \\
        -1 & 3 & t-1 & 3 \\
        t + 1 & -t-2 & -1 & -5
    \end{pmatrix}$$

    (a) Calculeu el rank de la matriu $A_t$ en funció del paràmetre t.

    (b) Calculeu una base del espai generat per les files de $A_t$ per a
        $t=100$ i pels $t$'s on el rang de $A_t$ no és $=3$.

    (c) Calculeu en funció de $t$ les solucions del sistema d'equacions
        que té com a matriu ampliada la matriu $A_t$, o sigui el sistema
        $$\begin{pmatrix} 
                1 & t-1  & -1  \\
                -1 & 3 & t-1  \\
                t + 1 & -t-2 & -1 
            \end{pmatrix}\begin{pmatrix} 
            x_0 \\
            x_1 \\
            x_2
        \end{pmatrix}=
        \begin{pmatrix} 
        -2 \\
        3 \\
        -5
        \end{pmatrix}$$

10. Considereu la funció $f$ definida per
    $f(x)=1.1 x^3+0.9 x^2-0.02 x-0.1$.

    (a) Feu els gràfics necessaris per tal de posar de manifest que
        l'equació $f(x) = 0$ té tres solucions reals.

    (b) Calculeu amb find_root() les aproximacions numèriques d'aquestes
        tres solucions i poseu-les en una llista ordenada de nom Sol.

    (c) Feu un gràfic de la funció f entre $-1$ i $1$ en el que surti en
        blau la part on $f(x) > 0$, en vermell la part on $f(x) < 0$ i
        surti una marca de color verd en els punts on $f(x) = 0$.

11. (a) Definiu una funció `SumaDivisorsSenars(n)` que retorni la suma
        dels nombres **senars** que divideixen un nombre enter de sage
        $n>0$ (i escrigui un missatge d'error si $n$ no és un enter de
        sage $\le 0$). Calculeu `SumaDivisorsSenars(15)` i
        `SumaDivisorsSenars(1890)`.

        \<

    (b) Definiu una funció Func(n) per a $n>1$ un enter, que retorni la
        llista dels nombres enters no primers $k\le n$ tals que
        `SumaDivisorsSenars(k)` és $\ge k$. Calculeu `Func(100)`.

    (c) Feu una llista $L$ de longitut 15 que comenci amb $L[0]=1069$ i
        de manera que $L[i+1]$ sigui igual a `SumaDivisorsSenars(L[i])`
        per a $i\ge 0$.
