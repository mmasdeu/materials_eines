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

# Pràctica 8


## Conjunts


Per posar un exemple (estrany), anem a construir el conjunt que conté el número 2, la variable x, el nombre $\pi$, el nombre real $3.2$ i l'anell dels enters. 


```sage
A={2,x ,3.2,ZZ, pi}
show(A)
```

```sage
ZZ in A
```

```sage
show({2}.issubset(A))
```

```sage
A[0]
```

 Si al crear un conjunt posem elements repetits només sortiran una vegada.

```sage
X={1,2,5,5,6,1}
X
```

```sage
X.intersection(A)
```

```sage
X.union(A)
```

```sage
A.difference(X)
```

```sage
len(X)
```

```sage
X.add(3); show(X)
```

```sage
X.remove(3); show(X)
```

```sage
X.discard(3); show(X)
```

```sage
X.pop()
```

```sage
X
```

Si voleu treure un element de X sense que es modifiqui X teniu varies opcions:

```sage
a=X.pop(); X.add(a)
a
```

```sage
list(X)[0]      # Fem una llista dels elements de X i prenem el primer element
```

A l'hora de crear conjunts, podeu usar també la comprehensió, tal com varem fer en les llistes. Per exemple, podem trobar el conjunt dels residus mòdul 17 dels quadrats.

```sage
{(a^2 % 17) for a in srange(17)}
```

Si fem servir conjunts de Sage (amb Set) podem fer alguna cosa amb conjunts infinits.

```sage
Z=Set(ZZ)
show(Z)

```

```sage
2 in Z
```

```sage
pi in Z
```

## Diccionaris

```sage
prova={ 1 : 'a' , 'x': [1,2] , (4,5) : {1,2} }
```

```sage
prova[1]
```

```sage
prova['x']
```

```sage
prova[(4,5)]
```

```sage
prova[(4,5)]={1,2,3}
print(prova)
```

```sage
prova.update({2:'b'})
```

```sage
print(prova)
```

```sage
for k in prova:
    print(k)
    print(prova[k])
```

## Divisió entera


A diferencia del que fem els matemàtics, el residu de dividir $a$ entre $b$ no és sempre positiu, sinò que té el mateix signe que $b$, i està entre 0 i b. 

```sage
124846 % 4
```

```sage
7%4
```

```sage
(-7) % 4
```

```sage
7 % (-4)
```

```sage
(-7) % (-4)
```

A part de les aplicacions purament aritmètiques, la reducció mòdul un enter pot ser molt útil en un exemple com el següent

```sage
var('k')
Clrs=['red','green','blue']
dib=plot(1,0,1,color=Clrs[0],aspect_ratio=1)
for k in [1..10]:
    dib=dib+plot(x^k,x,0,1,color=Clrs[k%3],aspect_ratio=1)
dib
```

Per un altre costat, l'altra part fonamental en una divisió és el quocient corresponent. Per tal d'obtenir aquest valor SageMath disposa d'un operador especial dedicat a la *divisió entera*: //. Podeu veure el seu funcionament en els exemples següents

```sage
24//6
```

```sage
(-25)//3
```

```sage
148//(-5)
```

```sage
(-24)//(-6)
```

En molts problemes de divisibilitat l'únic que interessa és saber si l'enter $a$ divideix $b$ (o si $b$ és múltiple de $a$). En aquests casos es disposa de l'opció *divides* que dóna com a resultat cert o fals segons es verifiqui la condició o no.


```sage
4.divides(24)
```

```sage
4.divides(25)
```

```sage
23445567889.divides(12334455667789900023)
```

Un altre càlcul que apareix sovint és el de determinar tots els divisors d'un nombre donat. Per tal d'obtenir aquest resultat la funció que fa tota la feina és *divisors* com es pot veure en aquest exemple:

```sage
 28.divisors()
```

```sage
divisors(28)
```

Utilitzant aquesta llista és molt fàcil definir una funció molt simple que ens digui si un número qualsevol és **perfecte** (igual a la suma dels seus divisors diferents d'ell mateix, és clar)  

```sage
def esperfecte(k):
    sumdiv=sum(k.divisors())
    return sumdiv==2*k
```

```sage
esperfecte(6)
```

```sage
esperfecte(12)
```

Sabríeu fer, ara, una llista amb els números perfectes menors que 1000?

```sage
[a for a in srange(1,1000) if esperfecte(a)]
```

Per acabar aquest apartat, si en un problema interessen igualment el quocient i el residu de la divisió entre dos enters es poden obtenir les dues dades amb quo\_rem() que dóna com a resultat un parell (q,r) agrupats en un sol objecte:

```sage
567.quo_rem(6)
```

```sage
q,r=567.quo_rem(6)
(q,r)
```

## Màxim comú divisor i mínim comú múltiple


Podem calcular el màxim comú divisor calculant els divisors comuns i el seu màxim.

```sage
a=39854; b=765756
D1=set(a.divisors())
D2=set(b.divisors())
D=D1.intersection(D2)  # Els divisors comuns de a i b
max(D)                 # El màxim  comú divisor de a i b
```

```sage
D
```

Els grecs ja sabien que aquest mètode és molt ineficient. És molt millor utilitzar l'algorisme d'Euclides. 

Aquesta versió és la que hi ha als apunts. Primer es pren els valors absoluts, s'escull el més gran, i després s'aplica el mètode. 

```sage
def euclides(a,b):
    m = max(abs(a),abs(b))
    n = min(abs(a),abs(b))
    r = m % n
    while r>0:
        m = n
        n = r
        r = m % n
    return n
```

```sage
euclides(8,12)
```

Com que de fet no cal que siguin positius, i a més no cal que hi hagi el més gran primer, podem fer la següent que és molt més curta.

```sage
def euclides(a,b):
    r = a % b
    while r!=0:
        a,b = b,r
        r = a % b
    return abs(b)
```

```sage
euclides(8,12)
```

```sage
euclides(2*3*5*7,3*5*7*11)
```

De fet no cal calcular el residu primer, i enlloc de retornar $b$ retornem $a$.

```sage
def euclides(a,b):
    while b!=0:
        a,b=b,a % b
    return abs(a)
```

```sage
euclides(2*3*5*7,3*5*7*11)
```

Finalment, es pot fer una versió recursiva de la funció, que tot i ser més ineficient, és més clara.

```sage
def euclides(a,b):
    if b!=0:
         return euclides(b,a%b)
    return abs(a)
```

```sage
euclides(2*3*5*7,3*5*7*11)
```

En qualsevol cas, SageMath ja porta aquests càlculs incorporats a gcd() que calcula el màxim comú divisor de dos enters o d'una llista tan llarga com es vulgui d'enters. Podeu comprovar, per exemple, com la funció euclides dóna el mateix resultat que gcd.

```sage
 gcd(748*2345,748*4686)
```

```sage
 euclides(748*2345,748*4686)
```

```sage
 gcd(748*3456,748*4520,748*4208)
```

```sage
gcd([748*3456,748*4520,748*4208])
```

```sage
lcm(24,38)
```

```sage
lcm([24,38,18])
```

```sage
24*38/gcd(24,38)
```

Hi ha problemes en els que no n'hi ha prou amb la informació del màxim comú divisor i es volen conèixer els coeficients d'una *identitat de Bézout* per a un parell d'enters (recordeu que una identitat de Bézout per al parell $a$, $b$ consisteix a tenir una igualtat del tipus
$$
\text{mcd}(a,b) = \alpha\times a +\beta \times b\,,
$$
on $\alpha$ i $\beta$ són les coeficients que s'han de determinar i sempre existeixen). Per tal d'obtenir aquest resultat es pot utilitzar la instrucció \texttt{xgcd()} que dóna un resultat format per tres elements que són, respectivament, el màxim comú divisor, $\alpha$ i $\beta$. Per exemple

```sage
 a=39854; b=765756; show(gcd(a,b))
```

```sage
Bezout=xgcd(a,b); show(Bezout)
Bezout[1]*a+Bezout[2]*b==Bezout[0]
```

Ara bé, si el que volem és obtenir identitats de Bézout per a més de dos elements, caldrà fer-ho manualment. Per exemple, si volem calcular una identitat de Bézout pels enters $a=72776$, $b=9944$ i $c=86823$: 

```sage
a=72776; b=9944; c=86823
gcd([a,b,c])
gcd(gcd(a,b),c)
```

Fem una identitat de Bézout per $a$ i $b$, i emmagatzemem els coeficients que obtenim:

```sage
Bez0=xgcd(a,b)
d0=Bez0[0]
alpha0=Bez0[1]
beta0=Bez0[2]
```

De forma que $d_{0}= \alpha_{0} \times a + \beta_{0} \times b$. 
Ara fem una identitat per $d_0$ i $c$, 

```sage
Bez1=xgcd(d0,c)
d1=Bez1[0]
alpha1=Bez1[1]
beta1=Bez1[2]
```

I com que $d_{1} = \alpha_{1} \times d_{0} + \beta_{1} \times c$, obtenim el resultat que volem substituint $d_0$ 

```sage
alpha=alpha1*alpha0
beta=alpha1*beta0
gamma=beta1
print (gcd([a,b,c]),"=",alpha,"*",a,"+",beta,"*",b,"+",gamma,"*",c)
```

## Sistemes de congruències


Un problema que apareix sovint quan es tracta de resoldre situacions en les que s'han de fer reparticions és el de resoldre sistemes de congruències del tipus
\begin{gather*}
x \equiv a\ (\text{mod }\alpha)\\
x \equiv b\ (\text{mod }\beta)\\
\vdots
\end{gather*}
Segurament ja sabeu que totes les solucions d'un d'aquests problemes (si és que n'hi ha alguna) difereixen entre si per múltiples del mínim comú múltiple $m$ del mòduls $\alpha$, $\beta$, \dots i, per tant que es pot dir que són de la forma $s_{0}\ (\text{mod }m)$, on $s_{0}$ és una solució particular qualsevol.

La instrucció de SageMath que dóna les solucions d'aquests problemes és crt() i, per exemple, per a solucionar el problema
\begin{gather*}
x \equiv 2\ (\text{mod }6)\\
x \equiv 4\ (\text{mod }8)\\
\end{gather*}
n'hi haurà prou amb

```sage
s=crt([2,4],[6,8])
print ('Solucio= ', s)
print (s,' mod 6 =', s%6)
print (s,' mod 8 =', s%8)
```

per tal de conèixer una solució particular, i calcular el mínim comú múltiple per tal de saber com construir totes les altres

```sage
m=lcm(6,8)
print ('Solucions =', s,'mod', m)
```

```sage
crt([2,4,8,18],[6,8,20,50])
```

```sage
crt([3,6],[6,8])
```

```sage

```

## Equacions diofàntiques

```sage
var('y z')
assume(x,'integer')
assume(y,'integer')
assume(z,'integer')
tp=solve(x^2 + y^2== z^2,(x,y,z))
show(tp)
```

```sage
ftp(p,q)=tp
for r in [1..10]:
    for s in [1..r-1]:
        print (ftp(r,s))
```

```sage
var('y z')
assume(x,'integer')
assume(y,'integer')
assume(z,'integer')
tp=solve(x^2 + y^2== 3*z^2,(x,y,z))
show(tp)
```

```sage
solve(y^2-x^3-1,(x,y))
```

## Els anells $\mathbb{Z}/n\mathbb{Z}$

```sage
Z26 = Zmod(26)    # És equivalent a Integer(26)
Z26 
```

```sage
Z26.cardinality()           # El nombre d'elements
```

```sage
Z26.addition_table()       # La taula de l'operació suma
```

```sage
Z26.multiplication_table() # Adivineu
```

```sage
a=Z26(765)
b=Z26(8934)
show(a)
show(b)
show(a^177)
show(b^12)
```

```sage
show(a.order())  # L'ordre additiu
show(b.order())
```

```sage
a.parent()
```

Podem avaluar també potencies negatives: el que fa es calcular l'invers (i dóna error si no en té).

```sage
a^-1
```

```sage
b^-1
```

Podem evaluar expressions complicades.

```sage
c=Z26(13)
(3*a^5+(b+1)^(-2)+sqrt(c))^(12)-5
```

Observem que sqrt(13)=13, ja que 

```sage
c^2
```

Podem fer la llista de totes les potencies d'un element (sense repetir). Si l'element és invertible, aquesta llista conté com a últim element el 1, i el nombre d'elements és l'ordre multiplicatiu. Si l'element no es invertible, no. 

```sage
S=[a]
k=2
while a^k not in S:
    S.append(a^k)
    k+=1
S
```

```sage
len(S)
```

```sage
a.multiplicative_order()
```

```sage
S=[b]
k=2
while b^k not in S:
    S.append(b^k)
    k+=1
S
```

```sage
Z26.list_of_elements_of_multiplicative_group()
```

```sage
Z26.unit_group()
```

```sage

```

Com ja sabeu, si $n=p$ és un nombre primer, aleshores $\mathbb{Z}/p\mathbb{Z}$ és un cos, el que vol dir que tots els elements diferents de zero tenen invers (pel producte). Però si voleu treballar amb $\mathbb{Z}/p\mathbb{Z}$ com a cos, hi ha una instrucció millor que posar  Zmod(p) i es posar  GF(p) (que ve de Galois Field, que és com a molts llocs s'anomenen els cossos finits). Compte, però, que no és el mateix posar Zmod(n) que posar GF(n) si $n$ no és un nombre primer: aquesta última ens donarà un error si $n$ no és una potència d'un primer (ja que no hi ha cap cos amb aquest nombre d'elements), i ens donarà un anell diferent si $n$ és una potència d'un primer (ja que $\mathbb{Z}/n\mathbb{Z}$ no és mai un cos si $n$ no és un primer). Per exemple GF(4) ens donarà un cos amb 4 elements que no és $\mathbb{Z}/4\mathbb{Z}$ (això us ho explicaran a segon). 

```sage
Zmod(19).is_field()
```

```sage
Zmod(19) == GF(19)
```

```sage
GF(4)
```

## Equacions mòdul $n$


Encara que per a determinar les solucions d'una equació a $\mathbb{Z}/n\mathbb{Z}$ n'hi hauria prou utilitzant la **força bruta** i anar provant per a tots els casos possibles (només n'hi ha un nombre finit, en algun moment s'acabaran les proves) aquest no és, probablement, el mètode més eficient. La funció solve\_mod() intenta ser una mica més eficient i permet resoldre a $\mathbb{Z}/25\mathbb{Z}$ l'equació $x^{2}-3\,y^{2}=14$ fent

```sage
reset() # Abans hem fet alguns assume()
var('x y')
solve_mod(x^2-3*y^2==14,25)

```

Essencialment es com fer el següent, però de manera més eficient

```sage
L=[]
for x in Zmod(25):
    for y in Zmod(25):
        if x^2-3*y^2==14:
            L.append((x,y))
L
```

Més equacions

```sage
var('x')
solve_mod(x^3-2*x^2-1==0,11)

```

```sage
solve_mod(x^4-2*x^3-6==0,91)

```

```sage
solve_mod(x^3+3*x+1==0,10)
```

```sage
var('y')
```

```sage
solve_mod(x^3-2*x^2-1==y^2,11)
```

```sage
solve_mod(x^3+3*x+1==y^4,10)
```
