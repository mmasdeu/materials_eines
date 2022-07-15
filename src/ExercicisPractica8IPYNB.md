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

## Exercicis Pràctica 8


<font color=red> Exercici 1. </font> <font color=green> Definiu una funció de SageMath  que si li dones una llista finita de nombres et retorna una llista ordenada i sense repeticions (Indicació: transformeu-la a un conjunt, i retorneu la llista ordenada amb sorted). 

```sage
def OrdNR(L):
    return sorted(list(set(L)))
```

```sage
OrdNR([5,7,3,2.1,1,3,6,,1,1,6,1,3,1,7])
```

Ho provo amb una llista de 30 nombres a l'atzar entre 1 i 30.

```sage
OrdNR([randint(1,30) for _ in range(30)])
```

(He usat _ com a nom de la variable que s mou en el range, doncs no ens cal per res, només per repetir una cosa 30 cops). 


<font color=red> Exercici 2. </font> <font color=green>Com que les llistes de Python comencen pel zero, el que és a vegades un inconvenient,
ens aniria molt bé poder usar una 'nova' versió de les llistes que comenci per l'1.
Expliqueu com podem fer això amb diccionaris, i creeu una funció tal que, donada
una llista ens retorni un diccionari amb la mateixa llista però ara començant per 1.

```sage
def nova_llista(L):
    return { i+1:L[i] for i in range(len(L))}
```

```sage
nL=nova_llista([1,2,3,4])
```

```sage
nL[3]
```

<font color=red> Exercici 3. <font color=green>  Es diu que un parell d'enters positius $m$ i $n$ són *amics* si la suma dels divisors (propis) de cada un és igual a l'altre. Definiu una funció que permeti decidir si una parella $(m,n)$ és una parella d'amics o no. Utilitzeu la funció per a localitzar totes les parelles d'amics menors que $1000$.

```sage
def Amics(a,b):
    da=sum(divisors(a))-a
    db=sum(divisors(b))-b
    return da==b and db==a
```

```sage
Amics(12,15)
```

```sage
Amics(6,6)
```

```sage
Amics(220, 284)
```

```sage
for a in srange(2,1000):
    for b in srange(a,1000):
        if Amics(a,b):
            print(a,b)
```

<font color=black> Si voleu que no surtin els nombres perfectes (i per tant que les parelles siguin $a\lt b$), canvieu el segon bucle

```sage
for a in srange(2,1000):
    for b in srange(a+1,1000):
        if Amics(a,b):
            print(a,b)
```

<font color=red> Exercici 4: <font color=green>  Definiu una funció de SageMath, anomenada **xxgcd**, que accepti tres enters $a$, $b$, $c$ com a argument, i retorni el màxim comú divisor $d=\text{mcd}(a,b,c)$ i els coeficients $\lambda$, $\mu$ i $\nu$ d'una identitat de Bézout per a aquests números de forma que
\[
d=\lambda\times a+\mu\times b+\nu\times c
\] 
Sabeu modificar **xxgcd** per tal que accepti com argument una llista d'enters, de longitud arbitrària i doni com a resultat els coeficients d'una identitat de Bézout?



```sage
def xxgcd(a,b,c):
    d,l,m=xgcd(a,b)
    d,r,n=xgcd(d,c)
    return d,r*l,r*m,n
```

```sage
xxgcd(4*7,2*7,3*7)
```

Modificació per a llistes arbitraries (amb una funció recursiva, que es crida a si mateixa amb un element menys a cada llista).

```sage
def xxgcd(L):
    if len(L)==1:            # Si la llista només té un element, retornem l'element i una llista que conté el 1
        return L[0],[1]
    else:
        a=L[-1]              # Ultim element de la llista
        L2=L[:-1]            # La llista menys el ultim element
        d,S=xxgcd(L2)        # Apliquem la funció a la llista
        d,l,m=xgcd(d,a)      # Apliquem el xgcd al gcd de la llista L2, anomentat l, i a.
        SN=[l*s for s in S]  # Multipliquem tots els elements de la llista S pel coefficient que li toca a l
        SN.append(m)         # Afeguim a SN el coefficient que li toca a a
        return d,SN
```

```sage
L=[2*3*5*7,2*3*5*11,2*3*11*7,2*11*5*7]
print(L)
d,S=xxgcd(L)
print(S)
sum([L[i]*S[i] for i in range(len(L))])==d
```

<font color=red> Exercici 5: <font color=green>  Tenint en compte que *crt()* només dóna una solució particular del problema de congruències que es plantegi, definiu una extensió d'aquesta funció (*xcrt*) que, amb les mateixes dades, doni com a resultat una llista de dos elements: el valor de la solució particular i el mínim comú múltiple dels mòduls.


```sage
def xcrt(C,M):
    return [crt(C,M),lcm(M)]
```

Exemple

```sage
xcrt([2,4,8,18],[6,8,20,50])
```

```sage

```

<font color=red> Exercici 6: <font color=green>  Definiu una funció de SageMath que accepti com a arguments dos enters $a$ i $m$, i doni com resultat la llista de potències $\{{\overline a}^k \mid k\geq 0 \}\subseteq \mathbb{Z}/m\mathbb{Z}$.

```sage
# He distingit entre el cas que es unitat, doncs aleshores la maxima k que surt és el ordre multiplicatiu, de quan no ho és (però de fet no calia). Al enunciat diu "llista de les potencies", però escriu un conjunt, així que he decidit que doni una llista ordenada. He assumit que a partir que surt una potencia repetida, les següent potències també son repetides, però aixó caldria demostrar-ho, no? 

def potenciesmodm(a,m):
    Zm=Zmod(m)
    ab=Zm(a)
    if ab.is_unit():
        S=[ab^k for k in srange(ab.multiplicative_order())]
        return(S)
    else:
        S=[1]
        k=1
        while ab^k not in S:
            S.append(ab^k)
            k+=1
        return S
```

```sage
potenciesmodm(3,100)
```

```sage
potenciesmodm(1,100)
```

```sage
potenciesmodm(10,100)
```

```sage
potenciesmodm(5,100)
```

<font color=red> Exercici 7: <font color=green>  Feu una llista dels elements invertibles a $\mathbb{Z}/24\mathbb{Z}$ amb l'ordre multiplicatiu de cadascun d'ells. 

```sage
Z24=Zmod(24)
L=Z24.list_of_elements_of_multiplicative_group()
L
```

```sage
[[l,Z24(l).multiplicative_order()] for l in L]
```

<font color=red> Exercici 8: <font color=green>  Considereu l'anell $\mathbb{Z}/257\mathbb{Z}$.


<font color=green>  (a) Comproveu que 257 és primer i que, per tant, tots els elements no nuls de $\mathbb{Z}/257\mathbb{Z}$ són invertibles.


Comprovem que és primer.

```sage
is_prime(257)
```

També podem preguntar si és un cos.

```sage
ZN=Zmod(257)
ZN.is_field()
```

<font color=green>  (b) Trobeu un element $\bar a\in \mathbb{Z}/257\mathbb{Z}\setminus\{\bar 0\}$ amb ordre multiplicatiu màxim. 


Calculem el màxim dels ordres del grup d'unitats (=invertibles) de l'anell $\mathbb{Z}/257\mathbb{Z}$, que com que és un cos, són tots execepte el $0$. Hi ha un teorema que ens diu que, per $\mathbb{Z}/p\mathbb{Z}$ amb $p$ un primer, l'ordre sempre  és un divisor de $p-1=257-1=256$, i un altre teorema (més difícil) que diu que hi ha sempre algun element d'ordre $p-1$. Per tant l'ordre màxim ha de sortir $256$. 

```sage
max([a.order() for a in ZN.unit_group()])
```

```sage
for a in ZN:
    if a!=0 and a.multiplicative_order()==256:
        show(a)
        break
```

Una altra manera

```sage
[a for a in ZN if a!=0 and a.multiplicative_order()==256][0]
```

Un buscant-lo a l'atzar.

```sage
a = ZN(randint(1,256))
while(a.multiplicative_order()!=256):
    a = ZN(randint(1,256))
print(a)
```

O un altre amb el mètode random_element()

```sage
a = ZN.random_element()
while(a == 0 or a.multiplicative_order()!=256):
    a =  ZN.random_element()
print(a)
```

<font color=red> Exercici 9: <font color=green>  Hi ha molts jocs de màgia que estan basats en propietats de divisibilitat. Un d'aquests és la possibilitat de recuperar la data de naixement d'algú a partir del resultat de l'operació següent: si aquesta data és $d$-$m$ on $d$ representa el dia i $m$ el mes calcula 
$$
v= 12\times d + 31\times m
$$
Per exemple, al dia 3 de febrer li correspondrà el valor $v= 12\times 3+31\times 2=98$.

Definiu una funció que recuperi les dades $(d,m)$ a partir del valor $v$ i proveu amb els resultats dels vostres companys.
 



Una funció buscant-lo sistemàticament (no és molt eficient, però no importa).

```sage
def diames(r):
    for d in srange(1,32):
        for m in srange(1,13):
            if (12*d+31*m)==r:
                return d,m
```

```sage
diames(12*13+31*11)
```

Fixeu-vos que hi ha nombres que no poden sortir, i la funció no respondrà res. 

```sage
diames(12)
```

Una altra manera molt millor es calcular els inversos de 12 mòdul 31 i de 31 mòdul 12 i observar que el dia es el residu de multiplicar v per el invers de 12 mòdul 31, i el més el residu de multiplicar v per el invers de 31 mòdul 12

```sage
Zmod(31)(12)^(-1)
```

```sage
Zmod(12)(31)^(-1)
```

```sage
def diames(r):
    return (13*r%31),(7*r%12)
```

```sage
diames(12*13+31*11)
```

Si ho provem ara amb un nombre que no pot sortir, veure-ho que surt un més o un dia impossible (com 0)

```sage
diames(31)
```
