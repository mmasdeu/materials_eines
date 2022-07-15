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

# 1. Definir funcions 

```sage
f(x)=cos(pi*x)+3
show(f)
```

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

```sage
g(t)=2*t^2-1
show(g)
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

```sage
v=5
u
```

```sage
v
```

```sage
F(u,v)=u*v
```

```sage
u
```

```sage
v
```

```sage
g(y)
```

```sage
var('y')
```

```sage
g(y)
```

```sage
h(x,y)=x^2-x*y+ln(x^2+y^2)
```

```sage
show(h)
```

```sage
h(2,3)
```

```sage
h(1,x)
```

## 1.1 Gràfics de funcions

```sage
reset()
f(x)=x*e^(-x)
plot(f,(-1,4))
```

```sage
plot(x*e^(-x),(-1,4))
```

```sage
plot(f(x),(-1,4))
```

## 1.2 Convertir expressions en funcions

```sage
ee=ln(3*x^3+sqrt(x-1))
```

```sage
f=ee.function(x)
show(f)
```

```sage
f(1.2)
```

```sage
var('y')
show(f(y^2+1))
```

```sage
g=(x^2-y^2+sin(x*y)).function(x,y)
show(g)
```

```sage
show(g(2,pi))
```

```sage
g=(x^2-y^2+sin(x*y)).function(y,x)
show(g(2,pi))
```

```sage
show(g)
```

```sage
h=((x+y)*sin(pi*x)-cos(y))
hf=h.function(y)
show(hf)
show(hf(pi/2))
```

## 1.3 Diferències entre expressions on hi apareixen indeterminades i funcions

```sage
reset()
var('y z')
fyz=y^2-3*y*z+2*z^2
f(y,z)=y^2-3*y*z+2*z^2
show(fyz)
show(f)
```

```sage
y=2; z=3
show(fyz)
show(f(y,z))
show(f(2,3))
```

```sage
y*fyz
```

# 2. Elements d'un programa


## 2.1. Condicionals 


Un exemple en que no s'executa el que hi ha dins del condicional

```sage
var('a b')
a=3
if a==4: 
    b=a+1
    print("Modificant b")
show(a)
show(b)
```

El mateix exemple però ara si que s'executa

```sage
a=4
if a==4: 
    b=a+1
    print("Modificant b")
show(a)
show(b)
```

Un exemple de condicionals dins de condicionals

```sage
if a!=3:
    if b>4:
        if a*b<10:
            print(a)
        print(b)
print(a*b)
```

Un exemple de utilització del else

```sage
a=2
if a<3: 
    print (str(a) +' es menor que 3')
else:
    print (str(a) + ' no es menor que 3')
```

```sage

```

Utilització del elif: elif és una contracció de else if i estrictament no cal. 

```sage
a=3
if a>0: 
    print(1)
elif a<0:
    print(-1)
elif a==0:
    print(0)
else:
    print("No s'ha pogut comparar")

```

El mateix sense usar el elif

```sage
if a>0: 
    print(1)
else:
    if a<0:
        print(-1)
    else:
        if a==0:
            print(0)
        else:
            print("No s'ha pogut comparar")
```

```sage

```

```sage

```

```sage

```

## 2.2. Iteracions/Repeticions/Bucles


### For


La suma dels quadrats dels nombres de 1 a 100

```sage
suma=0
for k in [1..100]:
    suma+=k^2
k,suma
```

El mateix usant range

```sage
suma=0
for k in range(1,101):
    suma+=k^2
k,suma
```

El mateix amb comprensió de llistes i la funció sum

```sage
sum([a^2 for a in range(1,101)])
```

o encara més breu sense crear la llista, només amb l'iterador (ho comentarem més endavant)

```sage
sum(a^2 for a in range(1,101))
```

En general a Sage és recomanable usar srange (que és range de Sage). La diferencia és que srange produeix enters de sage, i range enters de python (que no tenen certs mètodes predefinits). Per exemple si volem sumar els quadrats dels nombres primers menors que 100 podem fer 

```sage
sum(a^2 for a in range(101) if a.is_prime())
```

que no funciona perquè son enters de Python, però el mateix amb srange si que funciona. 

```sage
sum(a^2 for a in srange(101) if a.is_prime())
```

```sage

```

```sage

```

### While


El while permet repetir un procés mentre es compleixi una condició. Per exemple el següent codi calcular la llista dels primers menors que 20 en ordre invers (de gran a petit), i els posa en una llista $L$.

```sage
L=[]                   # Inicialització de la llista
a=20                   # Inicialització del index 
while a>0:             # S'executarà mentre a sigui positiu
    if a.is_prime():   # Si el nombre a és primer s'executa el condicional
        L.append(a)    # Afeguim el nombre a a la llista 
    a-=1               # Restem 1 a a 
L
```

Penseu com fer el mateix amb el for, o amb comprensió de llistes. Hi ha moltes possibles solucions!

```sage

```

Compte! Si fem malament el while potser que es formi un bucle infinit. És un dels problemes més típics del while, i sovint és molt difícil de detectar! El exemple del PDF l'he escrit més endavant... 


Un exemple d'utilització del break (tot i que és preferible no utilitzar el break si ho podem fer d'una manera més clara)

```sage
a = 1000
for p in [a..1,step=-1]:
    if p.is_prime():
        break
print(p)
```

```sage
p = a 
while not p.is_prime():
    p -= 1
print(p)
```

```sage
a.previous_prime()
```

Proveu de fer el mateix per a trobar el a trobar el nombre primer immediatement superior a $a$.

```sage

```

## 3. Funcions 


Definim una funció que ens diu si un nombre és primer i dóna residu 1 si el dividim per 3 (equivalentment, al restar-li 1 és divisible per 3). La funció a%b és el residu de dividir a entre b.

```sage
def primer3(n):
    if n.is_prime() and (n%3 ==1):
        return True
    else:
        return False
```

```sage
primer3(4)
```

```sage
primer3(7)
```

```sage
primer3(11)
```

Més concisament

```sage
def primer3(n):
    return n.is_prime() and (n%3 ==1)
```

```sage
primer3(100)
```

```sage
primer3(13)
```

```sage
primer3(-11)
```

```sage
primer3('a')
```

```sage
def primer3(n):
    if type(n) != Integer: 
        return False
    return n.is_prime() and (n%3 ==1)
```

```sage
primer3('a')
```

El podem utilitzar per a fer la llista dels primers fins a 100 cumplint la condició.

```sage
[a for a in srange(101) if primer3(a)]
```

```sage

```

Les funcions poden no necessitar cap input ni retornar res. 

```sage
def hola():
    print('Hola que tal!')
```

```sage
hola()
```

El while que es proposa en el text per a calcular el nombre real $x$ tal que $cos(x)=x$

```sage
di=1e-8 # 10^(-8)
s=0.
sa=s+1.
k=0
while abs(s-sa)>di:
    k+=1; 
    sa=s
    s=cos(s)
    show('Iteracio ', k,', s=', s)
sa,s
```

Ara el mateix amb una funció (incorporant un màxim de iteraccions possibles): origen és el valor inicial, precisió la precisió buscada, maxit el màxim de iteraccions que volem fer. He tret que s'imprimeixi cada cop que fa una iteració. 

```sage
reset()
def ICos(origen,precisio,maxit):
    s=origen.n()
    sa=s+1
    k=0
    print ("***********************************")
    while abs(s-sa)>precisio and k<maxit:
        k+=1; sa=s; s=cos(s)
        show('Iteracio  ', k,', s=', s)
    if k==maxit:
        show("S'ha arribat al maxim d'iteracions previstes!!!")
        show("No es segur que el valor sigui prou ajustat")
        show("El resultat correspon a l'ultim valor obtingut.")
    else:
        show("Despres de ", k, " iteracions,")
        show("les dues ultimes aproximacions difereixen en ")
        show(abs(s-sa).n(digits=3), ", i son:")
        show(sa," , ",s)
    print ("***********************************")
    return s
valor=ICos(pi/3,0.000001,100)
valor

```

```sage
ICos(pi/3,0.00001,20)
```

Noteu que les variables dins de la funció són "locals". Això vol dir que no modifiquen el valor de variables de fora que puguin tenir el mateix nom. 

```sage
k=10
ICos(2,0.001,20)
k
```

### 3.1 Fer experiments amb programes: la conjectura de Collatz


Fem una funció de calcula la llista de nombres que s'obté al aplicar la funció fins arribar a 1.

```sage
def tresxmesun(k):
    if type(k)!=Integer or k<=0:
        print ("No es poden fer els calculs.")
    else:
        valor=k
        llista=[valor]
        while valor!=1:
            if valor%2==0:
                valor=valor/2
            else:
                valor=3*valor+1
            llista.append(valor)
        return llista
```

```sage
tresxmesun(10)
```

```sage
tresxmesun(31)
```

## 3.2 Resultats diferents d’una mateixa funció

```sage
def aprox(func,xini,it,ctrl):
    llista=[(xini+1/k,func(xini+1/k)) for k in [1..it]]
    if ctrl:
        return llista
    else:
        return points(llista)
```

```sage
f(x)=sin(x)/x
aprox(f,0.,25,True)
```

```sage
pts=aprox(f,0.,25,False)
graf=plot(f,(0,1),color="red")
(pts+graf).show(aspect_ratio=1,xmin=0,ymin=0)
```

```sage

```

```sage

```
