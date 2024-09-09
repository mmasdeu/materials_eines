---
jupyter:
  title : 'Pràctica 10: Exercicis de consolidació'
  authors: [ "name" : "Marc Masdeu", "name" : "Xavier Xarles" ]
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.14.0
  kernelspec:
    display_name: SageMath 10.4
    language: sage
    name: sagemath
---

# Exercici 1

1. Definiu una funció `SumaDivisorsSenars(n)` que retorni la suma dels nombres **senars** que divideixen un nombre enter de sage $n > 0$ (i escrigui un missatge d'error si $n$ no és un enter de sage $\leq 0$). Calculeu `SumaDivisorsSenars(15)` i `SumaDivisorsSenars(1890)`.

2. Definiu una funció `Func(n)` per a $n>1$ un enter, que retorni la llista dels nombres enters no primers $k\leq n$ tals que `SumaDivisorsSenars(k)` és $\ge k$. Calculeu `Func(100)`.

3. Feu una llista $L$ de longitud 15 que comenci amb `L[0]=1069` i de manera que `L[i+1]` sigui igual a  `SumaDivisorsSenars(L[i])` per a $i\ge 0$.

-- begin hide

#### Part 1

```sage
def SumaDivisorsSenars(n):
    try:
	    n = ZZ(n)
    except TypeError:
	    raise TypeError('n ha de ser un enter de Sage')
	if n <= 0:
        raise ValueError('n ha de ser positiu')
    return sum(d for d in divisors(n) if d % 2 == 1)
```

```sage
SumaDivisorsSenars(15)
```

```sage
SumaDivisorsSenars(1890)
```

#### Part 2

```sage
Func = lambda n : [k for k in srange(2,n+1) if not k.is_prime() and SumaDivisorsSenars(k) >= k]
```

```sage
Func(100)
```

#### Part 3

Una manera:

```sage
L = []
a = 1069
for _ in range(15):
    L.append(a)
    a = SumaDivisorsSenars(a)
print(L)
print(len(L))
```

Una altra sense usar la variable auxiliar `a`:

```sage
L = [1069]
for _ in range(14):
    L.append(SumaDivisorsSenars(L[-1]))
print(L)
print(len(L))
```
-- end hide



# Exercici 2


Les parelles de primers bessons són parelles de primers de la forma $(p,p+2)$. Definiu una funció que permeti fer una llista de les parelles de primers bessons menors que un màxim donat.

-- begin hide
```sage
def primersbessons(n):
    """ Retorna la llista de parelles de primers bessons fins a n """
	n = ZZ(n)
    if n % 2 != 0:
        raise ValueError("El valor donat no és un enter parell")
    return [(p,p+2) for p in srange(3,n) if p.is_prime() and (p+2).is_prime()]
```

```sage
primersbessons(20)
```

```sage
primersbessons(100)
```

Observeu que si li passem un senar o un nombre que no sigui del tipus enter, dona un error.

```sage
primersbessons(7)
```

```sage
primersbessons(12/2)
```

-- end hide


# Exercici 3


La conjectura de Goldbach afirma que tot enter parell més gran que 2 es pot escriure com la suma de dos primers. Per exemple, $6=3+3$, $12=7+5$ o $64=17+47$. Aquestes particions com a suma de dos primers s'anomenen particions de Goldbach.  Creeu una funció Goldbach(n) que retorni totes les possibles particions de Goldbach de $n$ (sense importar l'ordre). Si denotem per $r(2k)$ el nombre de particions de Goldbach de $2k$, la conjectura afirma que $r(2k)>0$ per a tot $k>1$. Representeu en un gràfic els valors $(k,r(2k))$ per a $k\in [2,2000]$.

-- begin hide
Emetem un misstage d'error del tipus `TypeError` si no és un enter de Sage o `ValueError` si el nombre no és parell i $>4$.

```sage
def Goldbach(n):
    """ Retorna la llista de parelles de primers senars que sumen n """
    if type(n) != Integer:
        raise TypeError('Ha de ser un enter de Sage')
    if n % 2 == 1 or n < 6:
        raise ValueError(f'El nombre {n} ha de ser un enter parell més gran que 4')
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


```sage
Goldbach('a')
```

```sage
Goldbach(-3)
```

```sage
Goldbach(10)
```
-- end hide

# Exercici 4


Donat $k\in \mathbb{N}$, la funció phi d'Euler, $\varphi(k)$ és una funció que es defineix fàcilment en termes aritmètics, i que es pot calcular com $$\varphi(k)=\prod_{i=1}^r (p_i-1)\,p_i^{\alpha_i-1}$$ on $k=p_1^{\alpha_1}\cdots p_r^{\alpha_r}$ és la descomposició de $k$ en primers diferents. 

1. Factoritzeu un enter qualsevol fent `A = factor(...)` i observeu com s'estructura `A`, mirant per exemple la factorització i el contingut de `A[1]`.

2. Useu això per a construir una funció que calculi $\varphi(k)$ per a qualsevol natural $k$.

3. Comproveu que el resultat coincideix amb el de la instrucció `euler\_phi(k)`.

-- begin hide
Calculo un valor del qual se la seva factorizació per veure com és el resultat d'aplicar la funció factor()

```sage
n = 2**3 * 3**2 * 5
n
```

```sage
A = factor(n)
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

Veiem que, tot i el que ens ensenya quan li demanem que ens ho mostri, de fet la factorització d'un nombre es comporta com una llista de tuples de la forma (a,b), on a és el primer i b la potència en la que divideix el nombre.

```sage
def Lamevaphi(n):
    """ Una versió de la fi d'Euler """
    if type(n) != Integer:
        raise TypeError('n ha de ser un enter de Sage')
	if n<2:
        raise ValueError('Ha de ser un enter més gran que 1')
    A = factor(n)
    ans = 1
    for p, e in A:
        ans *= (p-1) * p**(e-1)
    return ans
```

```sage
Lamevaphi(n)
```

```sage
euler_phi(n)
```

Una altre versió més comprimida, usant la funció `prod` que té el Sage.

```sage
def Unaaltrephi(n):
    """ Una versió de la fi d'Euler """
    if type(n) != Integer:
        raise TypeError('n ha de ser un enter de Sage')
	if n<2:
        raise ValueError('Ha de ser un enter més gran que 1')
	return prod((p-1) * p**(e-1) for p, e in factor(n))
```

```sage
Unaaltrephi(n)
```
-- end hide


# Exercici 5


Considereu un joc d'atzar en el que es pot apostar entre dues opcions diferents igual de probables (cara o creu, parells o senars en la ruleta,...) de tal forma que cada cop que es guanya es recupera l'aposta i s'obté un premi de la mateixa quantitat (per tant, s'augmenta el capital amb un import igual a l'aposta que s'ha fet). És una creença força estesa entre els addictes al joc que l'estratègia consistent a fixar una aposta base, mantenint aquest import mentre es va guanyant i doblant l'aposta cada cop que es perd, condueix a l'èxit, ja que cada cop que es guanya després d'una ratxa dolenta es recupera tot el que s'havia perdut en les tirades anteriors i mentre es va guanyant s'acumulen beneficis. Per tal de comprovar si això és cert, feu una simulació d'aquest joc utilitzant com a model del fet de guanyar o perdre el resultat de la instrucció randint(0,1), fixant un capital inicial de $100$ unitats, una aposta base de 1 unitat i repetint el joc mentre es tinguin diners per apostar (el jugador s'arruïna) o s'arribi a acumular un capital de $1000$ unitats (moment en el qual el jugador es dona per satisfet). Per tal de veure l'evolució del joc, feu que mentre es realitza la simulació es vagi guardant en una llista el capital acumulat fins el moment, de tal forma que, al final, es pugui dibuixar un gràfic de l'evolució d'aquest capital.

Un exercici més complet consisteix a repetir moltes vegades l'experiment comptant al final la proporció de vegades que s'acaba guanyant la quantitat que satisfà al jugador.

-- begin hide

Tenim dues opcions respecte el mètode: o bé quan dobles l'aposta la mantens si guanyes, o bé si guanyes retornes a apostar 1.

Fem el primer cas (executeu-ho varies vegades per a veure com va canviant

```sage
ap=1
tot=100
T=[tot]
while tot > ap and tot < 1000:
    tot -= ap
    T.append(tot)
    tir = randint(0,1)
    if tir == 1:
        tot += 2*ap
    else:
        ap *= 2
print(tot)
```

```sage
points([(i,T[i]) for i in range(len(T))])
```

```sage
T
```

Per a veure més casos he fet una funció aposta de manera que respon `True` si i només si es guanya la aposta, i el guany final.

```sage
def aposta():
    ap = 1
    tot = 100
    while tot > ap and tot < 1000:
        tot -= ap
        tir = randint(0,1)
        if tir == 1:
            tot += 2*ap
        else:
            ap *= 2
    return tot >= 1000, tot - 100
```

```sage
aposta()
```

Ara repeteixo el joc 1000 vegades a veure el percentatge de vegades que es guanya.

```sage
t=0
total=0
for k in range(1000):
    ap, guany = aposta()
    if ap:
        t += 1
    total += guany
print(t/1000.*100)
print(total)
```

Em surt aproximadament entre 2% i 4%, però alguns cops es guanya diners.


El mateix però amb la segona versió del mètode.

```sage
def aposta():
    ap = 1
    tot = 100
    while tot > ap and tot < 1000:
        tot -= ap
        tir = randint(0,1)
        if tir == 1:
            tot += 2*ap
            ap = 1
        else:
            ap *= 2
    return tot >= 1000, tot-100

t = 0
total = 0
for k in range(1000):
    ap, guany = aposta()
    if ap:
        t += 1
    total += guany
print(t/1000.*100)
print(total)
```
-- end hide

# Exercici 6

En aquest exercici veurem que és possible treballar amb relacions d'equivalència i quocients per a conjunts finits. Considerarem un conjunt finit (de *Python*, o sigui construït via `{ }` o be via `set( )`). Les següents funcions han de respondré un booleà que sigui `True` o `False`, depenent de la veracitat o no del que es vol comprovar.

1. Recordeu que una relació en un conjunt $S$ és un subconjunt de $S^2$. Per a poder definir $S^2$ com a conjunt podeu utilitzar $$\{(a,b) \text{ for a in }S \text{ for b in }S\}.$$ Definiu una funció es\_relacio de SageMath tal que donats dos conjunts $S$ i $R$ comprovi si $R$ és una relació de $S$; de pas pot comprovar també si els dos son conjunts amb $type(S) =set$. Comproveu que `es_relacio({1},{(1,1)})` respon `True` i que `es_relacio({1},{1})` respon `False`.

2. Definiu funcions  `es_reflexiva`,  `es_simetrica`, i  `es_transitiva` que comprovi primer si li passem una relació i després si la relació és reflexiva, simètrica, i transitiva respectivament. Definiu després una funció `es_equivalencia` que cridi a cadascuna de les funcions anteriors i respongui si és o no d'equivalencia.

3. Definiu una funció `classe(a,S,R)` que calculi, després de comprovar si $R$ és una relació d'equivalencia de $S$, el conjunt de tots els elements de $S$ que estan a la classe de $a\in S$, i una funció `quocient(S,R)` que respongui un conjunt (subconjunt de $S$) que contingui un representant de cada classe de $S/R$.

4. Definiu una funció `projeccio(S,R)` tal que, donat un conjunt `S` i una relació `R` retorni un diccionari on les claus siguin els elements de `S` i els valor sigui l'element de `quocient(S,R)` que està relacionat amb ell, després de comprovar si $R$ és una relació d'equivalència de $S$.

5. Apliqueu aquestes funcions al conjunts $S=set([1..5])$ amb la relació corresponent a $a<b$ i a la relació corresponent a $a \leq b$, i al conjunt `set(Zmod(10))` amb la relació $(a,b)\in R  \Leftrightarrow 2a = 2b$.

-- begin hide

### Part 1

```sage
def es_relacio(S,R):
    """Comprova si R és un subconjunt de S^2"""
    if type(S) != set or type(R) !=set:
        raise TypeError('No són conjunts')
	return all(r1 in S and r2 in S for r1, r2 in R)
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

### Part 2

```sage
def es_reflexiva(S,R):
    """ Comprova si R es una relacio reflexiva de S"""
    if not es_relacio(S,R):
        print('No és una relacio')
        return False
	return all((a,a) in R for a in S)
```

```sage
def es_simetrica(S,R):
    """ Comprova si R es una relació simmètrica de S"""
    if not es_relacio(S,R):
        print('No es una relacio')
        return False
	return all((r2,r1) in R for r1, r2 in R)
```

```sage
def es_transitiva(S,R):
    """ Comprova si R es una relació transitiva de S"""
    if not es_relacio(S,R):
        print('No és una relacio')
        return False
	return all((a1, b2) in R for a1, a2 in R for b1, b2 in R if a2 == b1)
```

```sage
def es_equivalencia(S,R):
    """ Comprova si R es una relació d'equivalència de S"""
    if not(es_relacio(S,R)):
        print('No és una relacio')
        return False
    return es_reflexiva(S,R) and es_simetrica(S,R) and es_transitiva(S,R)
```

### Part 3

```sage
def classe(a,S,R):
    """ Calcula el subconjunt de S dels equivalents a a per R"""
    if a not in S:
        raise ValueError("l'element no és de S")
    if not es_equivalencia(S,R):
        raise ValueError("No és d'equivalencia")
    return {s2 for s1, s2 in R if s1 == a}
```

```sage
def quocient(S,R):
    """ Calcula un subconjunt de S que conté un element per a cada classe respecte R"""
    if not es_equivalencia(S,R):
        raise ValueError("No es d'equivalencia")
    Sc = copy(S)
    Qs = {}
    while len(Sc) > 0:
        a = Sc.pop()
        ca = classe(a,S,R)
        Qs.add(a)
        Sc = Sc.difference(ca)
    return Qs
```


### Part 4


He fet una funció que a cada element de `S`, respon l'element del quocient que li correspon.

```sage
def representant_quocient(s,Qs,S,R):
    """ Per a cada s de S, calcula quin element de Qs és equivalent a s"""
	cl = classe(s,S,R) # és important definir cl fora del "for". Si no, la recalcularem moltes vegades
    for a in Qs:
        if a in cl:
            return a
```

Una manera més ràpida és fer servir la funció `next`, que ens dona el primer element d'un iterador:

```sage
def representant_quocient(s,Qs,S,R):
    """ Per a cada s de S, calcula quin element de Qs és equivalent a s"""
	cl = classe(s,S,R)
	return next(a for a in Qs if a in cl)
```

```sage
def projeccio(S,R):
    """ Retorna un diccionari on les claus és S i els valors el quocient"""
    Qs = quocient(S,R)
    return {s : representant_quocient(s,Qs,S,R) for s in S}

```

### Part 5

```sage
S = set([1..10])
S2 = {(a,b) for a in S for b in S}
R = {s for s in S2 if s[0] < s[1]}
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
S = set([1..10])
S2 = {(a,b) for a in S for b in S}
R = {s for s in S2 if s[0] <= s[1]}
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
S = set(Zmod(10))
S2 = {(a,b) for a in S for b in S}
R = {s for s in S2 if 2*s[0] == 2*s[1]}
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
PS = projeccio(S,R)
```

```sage
PS[3]
```

```sage
PS[5]
```

-- end hide
