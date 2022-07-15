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

# Pràctica 9


## 1. Més sobre funcions de Pyhton


Exemple de funció amb valors per defecte

```sage
reset()
def ICos(origen,precisio,maxit=10):
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
```

```sage
ICos(pi/3,0.00001)
```

```sage
ICos(pi/3,0.00001,30)
```

```sage

```

Funcions que donen iteradors

```sage
def primers_acabats_en_1(n,m):
    for p in xsrange(n+1,m):
        if p.is_prime() and (p%10)==1:
            yield p 
```

```sage
it = primers_acabats_en_1(10,30)
next(it)
```

```sage
next(it)
```

```sage

```

## 2. Control d'errors en Python


Exemple d'ús del raise() extret de la llista d'exercicis de consolidació

```sage
def Goldbach(n):
    """ Retorna la llista de parelles de primers senars que sumen n """
    if type(n)!=Integer:
        raise TypeError('Ha de ser un enter de Sage')
    elif is_odd(n):
        raise ValueError('El nombre '+ str(n)+ ' no es parell')
    elif n<6:
        raise ValueError('El nombre '+ str(n)+' es mes petit que 6')
    return [(p,n-p) for p in prime_range(3,n//2+1) if (n-p).is_prime()]
```

```sage
Goldbach(12/2)
```

```sage
Goldbach(11)
```

```sage
Goldbach(4)
```

El mateix exemple amb assert()

```sage
def Goldbacha(n):
    assert(type(n)==Integer),'Ha de ser un enter de sage'
    assert(is_even(n)),'El nombre '+ str(n)+ ' no es parell'
    assert(n>=6),'El nombre '+ str(n)+' es mes petit que 6'
    return [(p,n-p) for p in prime_range(3,n//2+1) if (n-p).is_prime()]
```

```sage
Goldbacha(12/2)
```

```sage
Goldbacha(11)
```

```sage
Goldbacha(4)
```

```sage

```

```sage

```

Exemple d'ús del try except per a definir la funció sin(x)/x

```sage
def sinxpartitperx(a):
    try:
        f=(sin(a)/a).n()
        return f
    except ZeroDivisionError:
        return 1
```

```sage
sinxpartitperx(1)
```

```sage
sinxpartitperx(0)
```

```sage

```

## 3. Mòduls


El mòdul time. Un exemple amb la funció Goldbach.

```sage
from time import perf_counter
```

```sage
inici = perf_counter()
G = Goldbach(100000)
final = perf_counter()
temps_execucio = final-inici
print(temps_execucio)
```

Ara farem amb el mateix però amb una implimentació diferent

```sage
def Goldbach2(n):
    """ Retorna la llista de parelles de primers senars que sumen n """
    if type(n)!=Integer:
        raise TypeError('Ha de ser un enter de Sage')
    elif is_odd(n):
        raise ValueError('El nombre '+ str(n)+ ' no es parell')
    elif n<6:
        raise ValueError('El nombre '+ str(n)+' es mes petit que 6')
    return [(p,n-p) for p in xsrange(3,n//2+1) if p.is_prime() and (n-p).is_prime()]
```

```sage
inici = perf_counter()
G = Goldbach2(100000)
final = perf_counter()
temps_execucio2 = final-inici
print(temps_execucio2)
```

Observeu que aquesta triga més del doble.

```sage
print(temps_execucio2/temps_execucio)
```

```sage

```

```sage

```

## 4. Classes


### Preliminars: zip


La funció zip s'aplica a n llistes o tuples o cadenes, i en fa un iterable de n-tuples, agafant el ièssim de cada llista, des de i=0 fins que s'acaba alguna tupla.

```sage
print([x for x in zip([1,2],[3,4])])
```

```sage
print([x for x in zip([1,2,5],[3,4])])
```

```sage
print([x for x in zip([1,2],[3,4],[5,6])])
```

```sage
print([x for x in zip('abc',[1,2,3])])
```

Si apliques zip(*llista) on llista és una llista, el que fa és "esborrar" els parentesis de la llista per poder fer-li el zip. (S'utilita per desfer un zip, per exemple.) Fixeu-vos la diferència entre fer un zip amb * o sense *

```sage
print([a for a in zip(*[(1, 1, 2), (2, 3, 4)])])
```

```sage
print([a for a in zip((1, 1, 2), (2, 3, 4))])
```

## Una classe per treballar amb triangles


Definim primer una funció que calcula la distancia entre dos punts de $\mathbb{R}^n$, donats com una llista de dues tuples/llistes. 

```sage
def distancia(ps):
    '''Calcula la distància entre dos punts de R^n'''
    if len(ps)!=2: raise TypeError('Ha de ser un llista de dos punts')
    return((sum((x-y)**2 for x,y in zip(*ps))**0.5))
```

```sage
distancia([(1,2),(2,3)])
```

```sage
distancia([[1,2],[2,3]])
```

```sage

```

Definim la classe dels triangles formada per tres punts del pla (tres llistes/tuples de llargada 2 formada per nombres)

```sage
class Triangle:
    def __init__(self, punt1,punt2,punt3):
        self.__p1 = punt1
        self.__p2 = punt2
        self.__p3 = punt3
        self.vertexs=[punt1,punt2,punt3]
    def area(self):
        p1=self.__p1
        p2=self.__p2
        p3=self.__p3
        p123=(p2[0]-p1[0])*(p3[1]-p1[1])
        p132=(p2[1]-p1[1])*(p3[0]-p1[0])
        area=abs((p123-p132)/2)
        return(area)
    def costats(self):
        p=self.vertexs
        pp=[[a for a in p if a!=b] for b in p]
        return sorted([distancia(a) for a in pp])
    def perimetre(self):
        return sum(self.costats())
    def baricentre(self):
        p=self.vertexs
        centre=[sum(a)/3 for a in zip(*p)]
        return(centre)
    def inradi(self):
        return(2*self.area()/self.perimetre())
    def circumradi(self):
        return(prod(self.costats())/(4*self.area()))
    def __eq__(self, other):
        return self.costats() == other.costats()
    def __ne__(self, other):
        return not self.__eq__(other)
```

```sage
T1=Triangle([0,0],[0,12],[16,12])
print('Vertexs =', T1.vertexs)
print('Costats =',T1.costats())
print('Perímetre =',T1.perimetre())
print('Àrea =',T1.area())
print('Baricentre =',T1.baricentre())
print('Inradi =',T1.inradi())
print('Circumradi =',T1.circumradi())
```

Hem definit també igualtat de triangles; comprovem-ho amb un triangle traslladat

```sage
T11=Triangle([1,0],[1,12],[17,12])
T11 == T1
```

```sage

```

```sage

```

Definim triangle rectangle com una classe a partir de la classe dels triangles

```sage
class TriangleRectangle(Triangle):
    def __init__(self, catet1,catet2):
        Triangle.__init__(self,[0,0], [0,catet1], [catet2,catet1])
        self.catet1=catet1
        self.catet2=catet2
    def hipotenusa(self):
        return((self.catet1**2+self.catet2**2)**0.5)
```

```sage
T2=TriangleRectangle(12,16)
print(T2.vertexs)
```

```sage
print(T1.vertexs==T2.vertexs)
```

```sage
T1 == T2
```

```sage
print('Hipotenusa =', T2.hipotenusa())
print('Hipotenusa =', T1.hipotenusa())

```

Tot i que tenen els mateixos vèrtexs, i de fet són iguals (com a triangles), el triangle T1 no té definida la hipotenusa, doncs no està definit com a triangle rectangle.

```sage

```

```sage

```

```sage

```

```sage

```

```sage

```

```sage

```

```sage

```
