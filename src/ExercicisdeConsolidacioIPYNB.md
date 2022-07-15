---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.14.0
  kernelspec:
    display_name: SageMath 9.2
    language: sage
    name: sagemath
---

# Exercicis de consolidació


<font color=blue> 1. L'objectiu d'aquest exercici és obtenir un gràfic dels punts del pla $(x,y)$ determinats per l'equació $x^{2} + x\, y + y^{2} - 6 \, x=0$ (una el·lipse). Per tal d'aconseguir això seguiu l'esquema següent:

<font color=blue>     (i) Calculeu quins són els valors de $y$ que solucionen aquesta equació per a cada valor de $x$ donat utilitzant la instrucció solve. (Obtindreu dues solucions possibles per a cada $x$).

<font color=blue>     (ii) Convertiu aquestes solucions en funcions per tal de poder avaluar, en funció d'un $x$ donat, els valors de $y$ que donen punts de la corba que es vol representar.

<font color=blue>     (iii) Ja haureu notat que, com que l'equació és de grau $2$, les funcions anteriors no es podran avaluar per a $x$ arbitraris (hi ha una arrel quadrada que, segons el valor de $x$, no es podrà calcular). Determineu els valors de $x$ per als que és possible avaluar les funcions que heu introduït a l'apartat anterior (és a dir els valors de $x$ per als que hi ha algun punt de la forma $(x,y)$ que pertany a la corba).

<font color=blue>     (iv) Combineu en un sol dibuix el gràfic de les dues funcions, restringint el domini de les $x$ a la regió on té sentit fer els càlculs.



(i)

```sage
var('x y')
f=x^2 + x*y + y^2 - 6*x
```

```sage
sols=solve(f==0,y)
sols
```

(ii)

```sage
F=[y.subs(s).function(x) for s in sols]
F
```

(iii)

```sage
solve(-3*x^2 + 24*x>=0,x)
```

(iv)

```sage
plot(F,0,8,color="blue")
```

<font color=blue> 2. Considereu les successions determinades, donats dos nombres positius $a$, $b$, per
$$
S_{k}= \sqrt[k]{a^{k}+b^{k}},\qquad T_{k}=\frac{a^{k}-b^{k}}{a^{k}+b^{k}}
$$
Per tal de poder experimentar amb els seus valors definiu dues funcions de tres arguments (a,b,k) que donin, respectivament, els valors de $S_{k}$ i $T_{k}$ per a un parell $(a,b)$ donat. A continuació feu una llista dels $25$ primers termes d'aquestes dues successions triant $2$ o $3$ parelles $(a,b)$ diferents. Finalment, després dels resultats dels experiments, quin límit conjectureu que tenen cada una d'aquestes dues successions?

```sage
def Sk(a,b,k):
    return ((a**k+b**k)**(1/k)).n()
def Tk(a,b,k):
    return ((a**k-b**k)/(a**k+b**k)).n()
```

```sage
a=2
b=3
[Sk(a,b,k) for k in srange(1,25)]
```

```sage
[Tk(a,b,k) for k in srange(1,25)]
```

De fet només m'interessa l'últim element de la llista.

```sage
a=2
b=5
[Sk(a,b,k) for k in srange(1,25)][-1]
```

```sage
[Tk(a,b,k) for k in srange(1,25)][-1]
```

```sage
a=5
b=11
[Sk(a,b,k) for k in srange(1,25)][-1]
```

```sage
[Tk(a,b,k) for k in srange(1,25)][-1]
```

```sage
a=11
b=5
[Sk(a,b,k) for k in srange(1,25)][-1]
```

```sage
[Tk(a,b,k) for k in srange(1,25)][-1]
```

Observeu que no ens cal calcular tots els elements de la llista per a trobar l'últim!


Podem deduïr "experimentalment" que Sk tendeix al major dels dos nombres, mentre que Tk tendeix a -1 si a<b i a 1 si a>b


<font color=blue> 3. Considereu la successió $a_{k}$ definida per les condicions
$$
a_{0}=3,\ a_{k+1}=\frac{a_{k}^{2}-1}{2 a_{k}-3}
$$
Feu una llista prou llarga dels seus valors per tal de poder conjecturar quin serà el seu límit.
Què passa si canvieu el valor inicial $a_{0}$?


El que he fet ha estat anar repetint fins que la diferència entre un i l'anterior sigui $<10^{-10}$. El primer valor $a0$ és per tal que es superi la condició del while.

```sage
a=3.
a0=a+1
A=[a]
while (abs(a-a0)>10^(-10)):
    a0=a
    a=(a^2-1)/(2*a-3)
    A.append(a)
A
```

Podeu comprovar que el darrer valor és una solució de l'equació $(x-1)^2-x$:

```sage
(a-1)^2-a
```

```sage
a=-2.
a0=a+1
A=[a]
while (abs(a-a0)>10^(-10)):
    a0=a
    a=(a^2-1)/(2*a-3)
    A.append(a)
A
```

```sage
(a-1)^2-a
```

```sage
a=5.
a0=a+1
A=[a]
while (abs(a-a0)>10^(-10)):
    a0=a
    a=(a^2-1)/(2*a-3)
    A.append(a)
A
```

```sage
(a-1)^2-a
```

Podeu veure que la resposta és sempre una arrel de (x-1)^2-x, si es comença amb un nombre més gran que 3/2 ens dona la gran, i si és més petit dóna la petita (si es comença amb 1.5 dona error (o infinit))

```sage
a=1.5
a0=a+1
A=[a]
while (abs(a-a0)>10^(-10)):
    a0=a
    a=(a^2-1)/(2*a-3)
    A.append(a)
A
```

També ho podríem haver fet amb un llista sense la $a$ i la $a0$. Primer fem un primer càlcul per a tenir dos valors i comparar amb el while. 

```sage
A=[3.]
A.append((A[-1]^2-1)/(2*A[-1]-3))
while (abs(A[-1]-A[-2])>10^(-10)):
    A.append((A[-1]^2-1)/(2*A[-1]-3))
A
```

<font color=blue> 4. Les parelles de primers bessons són parelles de primers de la forma $(p,p+2)$. Definiu una funció que permeti fer una llista de les parelles de primers bessons menors que un màxim donat.

```sage
def primersbessons(n):
    """ Retorna la llista de parelles de primers bessons fins a n """
    if type(n)!=Integer or n%2!=0:
        print("El valor donat no és un enter parell")
        return None
    return [(p,p+2) for p in srange(3,n) if p.is_prime() and (p+2).is_prime()]
```

```sage
primersbessons(20)
```

```sage
primersbessons(100)
```

Observeu que si li passem un senar o un nombre que no sigui del tipus enter, dóna un error.

```sage
primersbessons(7)
```

```sage
primersbessons(12/2)
```

Pels errors, de fet és millor usar la funció raise, que justament serveix per quan hi ha un error. 

```sage
def primersbessons(n):
    if type(n)!=Integer or n%2!=0:
        raise TypeError("El valor donat no és un enter parell")
    return [(p,p+2) for p in srange(3,n) if p.is_prime() and (p+2).is_prime()]
```

```sage
primersbessons(7)
```

<font color=blue> 5. La conjectura de Goldbach afirma que tot enter parell més gran que 2 es pot escriure com la suma de dos primers. Per exemple, $6=3+3$, $12=7+5$ o $64=17+47$. Aquestes particions com a suma de dos primers s'anomenen particions de Goldbach.  Creeu una funció Goldbach(n) que retorni totes les possibles particions de Goldbach de $n$ (sense importar l'ordre). Si denotem per $r(2k)$ el nombre de particions de Goldbach de $2k$, la conjectura afirma que $r(2k)>0$ per a tot $k>1$. Representeu en un gràfic els valors $(k,r(2k))$ per a $k\in [2,2000]$. 


```sage
def Goldbach(n):
    """ Retorna la llista de parelles de primers senars que sumen n """
    if type(n)!=Integer or not(2.divides(n)) or n<6:
        print('Ha de ser un enter parell més gran que 4')
        return None
    return [(p,n-p) for p in prime_range(3,n//2+1) if (n-p).is_prime()]
```

```sage
Goldbach(6)
```

```sage
Goldbach(100)
```

```sage
Goldbach(26)
```

```sage
pt=[(k,len(Goldbach(2*k))) for k in srange(3,2000)]
```

```sage
points(pt)
```

Aquesta versió emet un misstage d'error del tipus TypeError si no és un enter de Sage o ValueError si el nombre no és parell i $>4$.

```sage
def Goldbach(n):
    """ Retorna la llista de parelles de primers senars que sumen n """
    if type(n)!=Integer:
        raise TypeError('Ha de ser un enter de Sage')
    if not(2.divides(n)) or n<6:
        raise ValueError('El nombre '+str(n)+' ha de ser un enter parell més gran que 4')
    return [(p,n-p) for p in prime_range(3,n//2+1) if (n-p).is_prime()]
```

```sage
Goldbach('a')
```

```sage
Goldbach(-3)
```

```sage
Goldbach(10)
```

<font color=blue> 6. Donat $k\in \mathbb{N}$, la funció phi d'Euler, $\varphi(k)$ és una funció que es defineix fàcilment en termes aritmètics, i que es pot calcular com $$\varphi(k)=\prod_{i=1}^r (p_i-1)\,p_i^{\alpha_i-1}$$ on $k=p_1^{\alpha_1}\cdots p_r^{\alpha_r}$ és la descomposició de $k$ en primers diferents. 

<font color=blue>    (i) Factoritzeu un enter qualsevol fent A=factor(....) i observeu com s'estructura A, mirant per exemple la factorització i el contingut de A[1].

<font color=blue>    (ii)  Useu això per a construir una funció que calculi $\varphi(k)$ per a qualsevol natural $k$.

<font color=blue>    (iii)  Comproveu que el resultat coincideix amb el de la instrucció euler\_phi(k).




Calculo un valor del qual se la seva factorizació per veure com és el resultat d'aplicar la funció factor()

```sage
n = 2**3*3**2*5
n
```

```sage
A=factor(n)
A
```

```sage
A[0]
```

```sage
A[1]
```

```sage
list(A)
```

Veiem que, tot i el que ens ensenya quan li demanem que ens ho mostri, de fet la factorització d'un nombre és una llista de tuples de la forma (a,b), on a és el primer i b la poténcia en la que divideix el nombre. 

```sage
def Lamevaphi(n):
    """ Una versió de la fi d'Euler """
    if type(n)!=Integer or n<2:
        print('Ha de ser un enter més gran que 1')
    A=factor(n)
    p=1
    for a in A:
        p*=(a[0]-1)*a[0]**(a[1]-1)
    return p
```

```sage
Lamevaphi(n)
```

```sage
euler_phi(n)
```

Una altre versió més comprimida, usant la funció prod que té el Sage i amb un raise. 

```sage
def Unaaltrephi(n):
    """ Una versió de la fi d'Euler """
    if type(n)!=Integer or n<2:
        raise TypeError('Ha de ser un enter més gran que 1')
    return prod((a[0]-1)*a[0]**(a[1]-1) for a in factor(n))
```

```sage
Unaaltrephi(n)
```

```sage

```

```sage

```

<font color=blue> 7. Considereu un joc d'atzar en el que es pot apostar entre dues opcions diferents igual de probables (cara o creu, parells o senars en la ruleta,$\ldots$) de tal forma que cada cop que es guanya es recupera l'aposta i s'obté un premi de la mateixa quantitat (per tant, s'augmenta el capital amb un import igual a l'aposta que s'ha fet). És una creença força estesa entre els addictes al joc que l'estratègia consistent a fixar una aposta base, mantenint aquest import mentre es va guanyant i doblant l'aposta cada cop que es perd, condueix a l'èxit, ja que cada cop que es guanya després d'una ratxa dolenta es recupera tot el que s'havia perdut en les tirades anteriors i mentre es va guanyant s'acumulen beneficis. Per tal de comprovar si això és cert, feu una simulació d'aquest joc utilitzant com a model del fet de guanyar o perdre el resultat de la instrucció randint(0,1), fixant un capital inicial de $100$ unitats, una aposta base de <font color=blue> 1 unitat i repetint el joc mentre es tinguin diners per apostar (el jugador s'arruïna) o s'arribi a acumular un capital de $1000$ unitats (moment en el qual el jugador es dóna per satisfet). Per tal de veure l'evolució del joc, feu que mentre es realitza la simulació es vagi guardant en una llista el capital acumulat fins el moment, de tal forma que, al final, es pugui dibuixar un gràfic de l'evolució d'aquest capital.

 <font color=blue>   Un exercici més complet consisteix a repetir moltes vegades l'experiment comptant al final la proporció de vegades que s'acaba guanyant la quantitat que satisfà al jugador.


Tenim dues opcions respecte el mètode: o bé quan dobles l'aposta la mantens si guanyes, o bé si guanyes retornes a apostar 1. 

Fem el primer cas (executeu-ho varies vegades per a veure com va canviant9

```sage
ap=1
tot=100
T=[tot]
while (tot>ap) and (tot<1000):
    tot-=ap
    T.append(tot)
    tir=randint(0,1)
    if tir==1:
        tot+=2*ap
    else:
        ap*=2
print(tot)
```

```sage
points([(i,T[i]) for i in range(len(T))])
```

```sage
T
```

Per a veure més casos he fet una funció aposta de manera que respont True si i només si es guanya la aposta, i el guany final.

```sage
def aposta():
    ap=1
    tot=100
    while (tot>ap) and (tot<1000):
        tot-=ap
        tir=randint(0,1)
        if tir==1:
            tot+=2*ap
        else:
            ap*=2
    return tot>=1000, tot-100
```

```sage
aposta()
```

Ara repeteixo el joc 1000 vegades a veure el % de vegades que es guanya.

```sage
t=0
total=0
for k in range(1000):
    ap,guany=aposta()
    if ap:
        t+=1
    total+=guany
print(t/1000.*100)
print(total)
```

Em surt aproximadament entre 2% i 4%, però alguns cops es guanya diners


El mateix però amb la segona versió del mètode.

```sage
def aposta():
    ap=1
    tot=100
    while (tot>ap) and (tot<1000):
        tot-=ap
        tir=randint(0,1)
        if tir==1:
            tot+=2*ap
            ap=1
        else:
            ap*=2
    return tot>=1000, tot-100

t=0
total=0
for k in range(1000):
    ap,guany=aposta()
    if ap:
        t+=1
    total+=guany
print(t/1000.*100)
print(total)
```

```sage

```

```sage

```

```sage

```

<font color=blue> 7. En aquest exercici veurem que es possible treballar amb relacions d'equivalencia i quocients per a conjunts finits. Considerarem un conjunt finit (de Python, o sigui construït via $\{\ \}$ o be via set( )). Les següents funcions han de respondré un booleà que sigui True o False, depenen de la veracitat o no del que es vol comprovar. 

<font color=blue><font color=blue>    (i)  Recordeu que una relació en un conjunt $S$ és un subconjunt de $S^2$. Per a poder definir $S^2$ com a conjunt podeu utilitzar $$\{(a,b) \text{ for a in }S \text{ for b in }S\}.$$ Definiu una funció es\_relacio de SageMath tal que donats dos conjunts $S$ i $R$ comprovi si $R$ és una relació de $S$; de pas pot comprovar també si els dos son conjunts amb $type(S) =set$. Comproveu que es_relacio({1},{(1,1)}) respon True i que es_relacio({1},{1}) respon False.
 
<font color=blue>    (ii) Definiu funcions  es\_reflexiva,  es\_simetrica, i  es\_transitiva que comprovi primer si li passem una relació i després si la relació és reflexiva, simètrica, i transitiva respectivament. Definiu després una funció es\_equivalencia que cridi a cadascuna de les funcions anteriors i respongui si és o no d'equivalencia. 
    
<font color=blue>      (iii) Definiu funcions \texttt{classe(a,S,R)} que calculi, després de comprovar si $R$ és una relació d'equivalencia de $S$, el conjunt de tots els elements de $S$ que estan a la classe de $a\in S$, i una funció \texttt{quocient(S,R)} que respongui un conjunt (subconjunt de $S$) que contingui un representant de cada classe de $S/R$. 
    
<font color=blue>      (iv)  Definiu una funció \texttt{projeccio(S,R)} tal que, donat un conjunt \texttt{S} i una relació \texttt{R} retorni un diccionari on les claus siguin els elements de \texttt{S} i els valor sigui l'element de \texttt{quocient(S,R)} que està relacionat amb ell, després de comprovar si $R$ és una relació d'equivalència de $S$.
    
<font color=blue>      (v)  Apliqueu aquestes funcions al conjunts $S=set([1..5])$ amb la relació $(a,b)\in R \Leftrightarrow a\lt b$ i a la relació $(a,b)\in R \Leftrightarrow a\le b$, i al conjunt $set(Zmod(10))$ amb la relació $(a,b)\in R  \Leftrightarrow 2a=2b$. 



(i)

```sage
def es_relacio(S,R):
    """Comprova si R és un subconjunt de S^2"""
    if type(S)!=set or type(R) !=set:
        print('No són conjunts')
        return False
    S2={(a,b) for a in S for b in S}
    return R.issubset(S2)
```

Comprovem alguns casos fàcils

```sage
es_relacio(1,2)
```

```sage
es_relacio({1},{1})
```

```sage
es_relacio({1},{(1,1)})
```

(ii)

```sage
def es_reflexiva(S,R):
    """ Comprova si R es una relacio reflexiva de S"""
    if not(es_relacio(S,R)):
        print('No és una relacio')
        return False
    D={(a,a) for a in S}
    return D.issubset(R)
```

```sage
def es_simetrica(S,R):
    """ Comprova si R es una relació simmètrica de S"""
    if not(es_relacio(S,R)):
        print('No es una relacio')
        return False
    for s in R:
        i=(s[1],s[0])
        if not(i in R):
            return False
    return True
```

Una manera més concisa però més lenta i que ocupa més espai

```sage
def es_simetrica2(S,R):
    """ Comprova si R es una relació simmètrica de S"""
    if not(es_relacio(S,R)):
        print('No es una relacio')
        return False
    Ri={s for s in R if (s[1],s[0]) in R}
    return R==Ri
```

```sage
def es_transitiva(S,R):
    """ Comprova si R es una relació transitiva de S"""
    if not(es_relacio(S,R)):
        print('No és una relacio')
        return False
    for s in R:
        Ts=[t for t in R if t[0]==s[1]]
        for t in Ts:
            st=(s[0],t[1])
            if not(st in R):
                return False
    return True
```

```sage
def es_equivalencia(S,R):
    """ Comprova si R es una relació d'equivalència de S"""
    if not(es_relacio(S,R)):
        print('No és una relacio')
        return False
    return es_reflexiva(S,R) and es_simetrica(S,R) and es_transitiva(S,R)
```

(iii)

```sage
def classe(a,S,R):
    """ Calcula el subconjunt de S dels equivalents a a per R"""
    if not(a in S):
        print("l'element no és de S")
        return 
    if not(es_equivalencia(S,R)):
        print("No és d'equivalencia")
        return 
    return {s[1] for s in R if s[0]==a}
```

```sage
def quocient(S,R):
    """ Calcula un subconjunt de S que conté un element per a cada classe respecte R"""
    if not(es_equivalencia(S,R)):
        print("No es d'equivalencia")
        return 
    Sc=copy(S)
    Qs=set()
    while len(Sc)>0:
        a=Sc.pop()
        ca=classe(a,S,R)
        Qs.add(a)
        Sc=Sc.difference(ca)    
    return Qs
```

(iv)


He fet una funció que a cada element de S, respon l'element del quocient que li correspon.

```sage
def representant_quocient(s,Qs,S,R):
    """ Per a cada s de S, calcula quin element de Qs és equivalent a s"""
    for a in Qs:
        if a in classe(s,S,R):
            return a
```

```sage
def projeccio(S,R):
    """ Retorna un diccionari on les claus és S i els valors el quocient"""
    Qs=quocient(S,R)
    PS={s:representant_quocient(s,Qs,S,R) for s in S}
    return PS
```

(v)

```sage
S=set([1..10])
S2={(a,b) for a in S for b in S}
R={s for s in S2 if s[0]<s[1]}
```

```sage
es_relacio(S,R)
```

```sage
es_reflexiva(S,R)
```

```sage
es_simetrica(S,R)
```

```sage
es_transitiva(S,R)
```

```sage
es_simetrica2(S,R)
```

```sage
S=set([1..10])
S2={(a,b) for a in S for b in S}
R={s for s in S2 if s[0]<=s[1]}
```

```sage
es_relacio(S,R)
```

```sage
es_reflexiva(S,R)
```

```sage
es_simetrica(S,R)
```

```sage
es_transitiva(S,R)
```

```sage
es_simetrica2(S,R)
```

```sage
S=set(Zmod(10))
S2={(a,b) for a in S for b in S}
R={s for s in S2 if 2*s[0]==2*s[1]}
```

```sage
es_equivalencia(S,R)
```

```sage
es_simetrica2(S,R)
```

```sage
QS=quocient(S,R)
QS
```

```sage
classe(0,S,R)
```

```sage
classe(1,S,R)
```

```sage
[classe(a,S,R) for a in QS]
```

```sage
PS=projeccio(S,R)
```

```sage
PS[3]
```

```sage
PS[5]
```

<font color= blue> 8. Considereu la família de matrius $A_t \in M_{3x4}(\mathbb{Q})$, depenent d'un parametre $t$, donada per
$$
A_t=\begin{pmatrix} 
 1 & t-1  & -1 & -2 \\
-1 & 3 & t-1 & 3 \\
t + 1 & -t-2 & -1 & -5
\end{pmatrix}
$$

<!-- #region deletable=false editable=false -->
<font color= blue> (a) Calculeu el rank de la matriu $A_t$ en funció del paràmetre t.
<!-- #endregion -->

```sage
R.<t>=QQ['t']
```

```sage
At=matrix(3,4,[ 1 , t-1  , -1 , -2 ,
-1 , 3 , t-1 , 3 ,
t + 1 , -t-2 , -1 , -5])
At
```

```sage
Ae=At.echelon_form()
```

```sage
gcd(Ae.row(2))
```

```sage
print('El rank es 3 excepte si t=1 que es 1')
```

<!-- #region deletable=false editable=false -->
<font color= blue> (b) Calculeu una base del espai generat per les files de $A_t$ per a $t=100$ i pels $t$'s on el rang de $A_t$ no és $=3$.
<!-- #endregion -->

```sage
At.subs(t=100).row_space().basis()
```

```sage
At.subs(t=1).row_space().basis()
```

<!-- #region deletable=false editable=false -->
<font color= blue> (c) Calculeu en funció de $t$ les solucions del sistema d'equacions que té com a matriu ampliada la matriu $A_t$, o sigui el sistema 
$$
\begin{pmatrix} 
 1 & t-1  & -1  \\
-1 & 3 & t-1  \\
t + 1 & -t-2 & -1 
\end{pmatrix}
\begin{pmatrix} 
 x_0 \\
x_1 \\
x_2
\end{pmatrix}=
\begin{pmatrix} 
 -2 \\
3 \\
 -5
\end{pmatrix}
$$
<!-- #endregion -->

```sage
var('t')
```

```sage
At=matrix(3,4,[ 1 , t-1  , -1 , -2 ,
-1 , 3 , t-1 , 3 ,
t + 1 , -t-2 , -1 , -5])
At
```

```sage
Ae=At.echelon_form()
```

```sage
show(Ae)
```

```sage
show('Si t no es 1 la solucio es')
show('x0=' + str(Ae[0,3].full_simplify()))
show('x1=' + str(Ae[1,3].full_simplify()))
show('x2=' + str(Ae[2,3].full_simplify()))
```

```sage
A1=At.subs(t=1)
```

```sage
A1.echelon_form()
```

```sage
show('Si t=1 la solucio es')
show(x0==x2-2)
show(x1==x2/3+1/3)
```

```sage

```

```sage

```

```sage

```

<font color=blue> 9. Considereu la funció $f$ definida per $f(x)=1.1 x^3+0.9 x^2-0.02 x-0.1$.
    
<font color=blue>            (a) Feu els gràfics necessaris per tal de posar de manifest que l’equació $f(x) = 0$ té tres solucions reals.
    
<font color= blue>        (b) Calculeu amb find_root() les aproximacions numèriques d’aquestes tres solucions i poseu-les en una llista ordenada de nom Sol. 
    
<font color= blue>        (c) Feu un gràfic de la funció f entre $-1$ i $1$ en el que surti en blau la part on $f(x) > 0$, en vermell la part on $f(x) < 0$ i surti una marca de color verd en els punts on $f(x) = 0$.
    


(a)

```sage
f(x)=1.1*x^3+0.9*x^2-0.02*x-0.1
```

```sage
plot(f(x),-0.6,-0.4)
```

```sage
plot(f(x),-0.4,1)
```

Veiem que té dues arrels entre -0.6 i -0.4, i una arrel entre 0.2 i 0.4.


(b)


He fet varies proves per tal de trobar un interval que al posar-lo al find_root em retorni el valor buscat.

```sage
r1=find_root(f,-1,-0.54)
r2=find_root(f,-0.54,-0.5)
r3=find_root(f,0,2)
Sol=[r1,r2,r3]
print('Les arrels són', Sol)
```

(c)

```sage
plotvermell = plot(f(x),-1,r1,color='red')+plot(f(x),r2,r3,color='red')
plotblau = plot(f(x),r1,r2,color='blue')+plot(f(x),r3,1,color='blue')
plotverd = points([(r,f(r)) for r in Sol], color='green')
plotvermell + plotblau + plotverd
```

```sage

```

<font color= blue> 10. (a) Definiu una funció SumaDivisorsSenars(n) que retorni la suma dels nombres **senars** que divideixen un nombre enter de sage $n\gt 0$ (i escrigui un missatge d'error si $n$ no és un enter de sage $\le 0$). Calculeu SumaDivisorsSenars(15) i SumaDivisorsSenars(1890).
    
<font color= blue> (b) Definiu una funció Func(n) per a $n>1$ un enter, que retorni la llista dels nombres enters no primers $k\le n$ tals que SumaDivisorsSenars$(k)$ és $\ge k$. Calculeu Func(100). 
    
<font color= blue> (c) Feu una llista $L$ de longitut 15 que comenci amb $L[0]=1069$ i de manera que $L[i+1]$ sigui igual a  SumaDivisorsSenars$(L[i])$ per a $i\ge 0$.


(a)

```sage
def SumaDivisorsSenars(n):
    if not(type(n)==Integer) or (n <=0):
        print('No es un enter de Sage >0')
        return
    return sum(d for d in divisors(n) if is_odd(d))
```

```sage
SumaDivisorsSenars(15) 
```

```sage
SumaDivisorsSenars(1890)
```

(b)

```sage
def Func(n):
    return [k for k in srange(2,n+1) if not(k.is_prime()) and SumaDivisorsSenars(k)>=k]
```

```sage
Func(100)
```

(c)


Una manera

```sage
L = []
a = 1069
for _ in range(15):
    L.append(a)
    a = SumaDivisorsSenars(a)
print(L)
print(len(L))
```

Una altra sense usar la variable auxiliar 'a'

```sage
L=[1069]
for _ in range(14):
    L.append( SumaDivisorsSenars(L[-1]))
print(L)
print(len(L))
```

```sage

```

```sage

```
