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

# Exercicis de la pràctica 9


### 1. Feu una funció de Sage de manera que, si li passem una funció $f(x)$ d'una variable, valors $a$ i $b$ reals, amb $a<b$, i un nombre de passos $n\ge 1$, divideixi el interval $[a,b]$ en $n$ intervals iguals, i retorni una llista amb elements $(t,f(t))$ on $t$ es mou de $a$ a $b$ (inclosos) passant pels extrems dels intervals ordenadament, i, aquí hi ha la dificultat, si no pot avaluar la funció, es salti el valor. Indicació: useu try except amb el error ValueError. 


Una possible solució: si no ho pot avaluar li dic que 'passi', amb un pass. 

```sage
def valorsf(f,a,b,n):
    valorsx=[a+i*(b-a)/n for i in range(n+1)]
    valorsf=[]
    for x in valorsx:
        try:
            valorsf.append((x,f.subs(x=x)))
        except ValueError:
            pass
    return valorsf
```

Exemple per la funció 1/x

```sage
valorsf(1/x,-2,2,4)
```

Podeu observar que per x=0 no l'ha avaluat

```sage

```

```sage

```

### 2. Comproveu, amb l'ús del mòdul time, quina de les dues maneres d'ordenar una llista de nombres a l'atzar és més ràpida. Una, convertint la llista en conjunt i tornant-la a convertit en llista. L'altra, fent una funció que fa una copia de la llista, i creant una nova llista escollint el mínim i traient tots els elements de la copia de la llista repetits, fins que la llista sigui buida. 

```sage
reset()
```

Funció per ordenar primera

```sage
def ord2(llist):
    cllist = copy(llist)
    nllist = []
    while (len(cllist) > 0):
        a = min(cllist)
        nllist.append(a)
        while (a in cllist):
            cllist.remove(a)
    return nllist
```

<!-- #raw -->
Creem una llista a l'atzar de 1000 nombres del 1 al 30
<!-- #endraw -->

```sage
llist = [randint(1,30) for i in range(1000)]
```

Importem el mòdul time 

```sage
from time import perf_counter
```

Ordenem la llista amb la primera funció comptant el temps

```sage
inici = perf_counter()
print (ord1(llist))
final = perf_counter() 
print(final-inici)
```

El mateix amb el segon mètode

```sage
inici = perf_counter()
print (ord2(llist))
final = perf_counter() 
print(final-inici)
```

```sage

```

```sage

```

### 3. Donats dos vectors $u=(u_1,\dots,u_n)$ i $v=(v_1,\dots,v_n)$ de $\mathbb{R}^n$, diem que $u\le v$ en l'ordre lexicogràfic, si, o bé són iguals, o bé $u_1<v_1$, o bé existeix un $i\le n$ tal que $u_j=v_j$ per a tot $j<i$ i $u_i<v_i$. Feu una funció ordlex(u,v) que comprovi que $u$ i $v$ són vectors de la mateixa llargada, i si no ho són doni un error TypeError, i si ho són retorni True si $u\le v$ en l'ordre lexicogràfic, i si no retorni False.
	


Una possible manera. Ho he fet amb "llistes de nombres", no vectors

```sage
def ordlex(u,v):
    '''Retorna cert si u <= v en ordre lexicogràfic'''
    if type(u)!=list or type(v)!=list:
        raise TypeError('No són llistes de nombres')
    if len(u) != len(v):
        raise TypeError('No tenen la mateixa llargada')
    for i in range(len(v)):
        if u[i] < v[i]:
            return True
        if u[i] > v[i]: 
            return False
    return True
```

Observeu que es cumpleix el que es demana, ja que si $u[1]<v[1]$, llavors retorna True a la primera iteració, si $u[j]=v[j]$ per tot $j<i$ i $u[i]<v[i]$, llavors retorna True a la iteració número $j$, si fa totes les iteracions i surt del for és que $u=v$, i retorna True, i si no passa res d'això retorna False

```sage
u = [1,1,1,1]
v = [1,1,1,2]
```

```sage
ordlex(u,v)
```

```sage
u = [1,1,1,1]
v = [1,1,1,1]
ordlex(u,v)
```

```sage
u = [1,1,1]
v = [1,1,1,1]
ordlex(u,v)
```

```sage
u = (1,1,1,1)
v = [1,1,1,1]
ordlex(u,v)
```

```sage

```

```sage

```

```sage

```

### 4. Definiu una funció tal que, donades dues parelles de punts diferents del pla $\mathbb{R}^2$, $\{p_1,p_2\}$ i $\{q_1,q_2\}$, determini si el segment obert $r_1$  entre la primera parella talla o no el segment obert $r_2$ entre la segona parella. La funció ha d'acceptar com a dades dos conjunts formats per dos elements cadascun, i els elements han de ser punts de $\mathbb{R}^2$ (com a llistes, o com a tuples, etc). La resposta he de ser True si tallen, False si no. 


Indicació: Per a fer-ho podeu utilitzar que els punts del segment obert que uneix dos punts del pla $p_1$ i $p_2$ són els de la forma $tp_1+(1-t)p_2$, on $0< t <1$. Per tant, si tenim ara una altre parella de punts $p_3$ i $p_4$, volem comprovar si hi ha o no $0<s,t<1$ de manera que 
    $$tp_1+(1-t)p_2=sp_3+(1-s)p_4.$$ Utilitzant la regla de Cramer això es tradueix a una desigualtat entre determinants: el determinant $A$  de la matriu que té com a columnes (o files) $p_1-p_2$ i $p_4-p_3$ ha de ser diferent de $0$ (per tal que no siguin para\l.els o coincidents), i, si denotem per $B$ el determinant de la matriu que té com a columnes (o files) $p_4-p_2$ i $p_4-p_3$ i per $C$ el mateix però amb columnes (o files) $p_1-p_2$ i $p_4-p_2$, llavors $$0<\frac{B}{A}<1 \text{ i } 0<\frac{C}{A}<1$$ (doncs aquests quocients corresponen a la $t$ i la $s$ de la equació). 

```sage

```

He fet una funció que comprova si les dades són conjunts, si tenen dos elements, si cada elements té llargada 2 i després he convertit els "punts" a vectors de $\mathbb{R^2}$. He calculat els determinants i comprovo si $A=0$, després si tenen el mateix signe tots (amb la funció sign), i si els quocients són $\ge 1$, i si es cumpleix alguna d'elles la resposta és False, i sino la resposta és True. 


```sage
def EsTallen(S,T):
    '''Donats dos conjunts de dos punts del pla,
    determina si les rectes que formen es tallen o no
    '''
    if type(S) != set or type(T) != set:
        raise TypeError('No són conjunts')
    if len(S) != 2 or len(T) != 2: 
        raise TypeError('Els conjunts no tenen dos elements')
    if any(len(v)!= 2 for v in S.union(T)):
        raise TypeError('Han de tenir dues coordenades')
    E = RR^2
    V = [E(v) for v in S] + [E(v) for v in T]
    A = matrix([V[0]-V[1],V[3]-V[2]]).det()
    B = matrix([V[3]-V[1],V[3]-V[2]]).det()
    C = matrix([V[0]-V[1],V[3]-V[1]]).det()
    if A == 0: 
        return False
    elif A.sign()!=B.sign() or A.sign()!=C.sign(): 
        return False
    elif B/A >= 1 or C/A >= 1:
        return False
    t = B/A
    return True
```

A més he fet una funció Tallen que retorni el punt de tall a més (o bé 0 si no)

```sage
def Tallen(S,T):
    '''Donats dos conjunts de dos punts del pla,
    determina si les rectes que formen es tallen o no
    '''
    if type(S) != set or type(T) != set:
        raise TypeError('No són conjunts')
    if len(S) != 2 or len(T) != 2: 
        raise TypeError('Els conjunts no tenen dos elements')
    if any(len(v)!= 2 for v in S.union(T)):
        raise TypeError('Han de tenir dues coordenades')
    E = RR^2
    V = [E(v) for v in S] + [E(v) for v in T]
    A = matrix([V[0]-V[1],V[3]-V[2]]).det()
    B = matrix([V[3]-V[1],V[3]-V[2]]).det()
    C = matrix([V[0]-V[1],V[3]-V[1]]).det()
    if A == 0: 
        return False, 0
    elif A.sign()!=B.sign() or A.sign()!=C.sign(): 
        return False, 0
    elif B/A >= 1 or C/A >= 1:
        return False, 0 
    t = B/A
    return True, t*V[0]+(1-t)*V[1]
```

Un exemple

```sage
S = {(2.1,2),(2.3,1)}
T = {(2.1,1),(2.3,2)}
b,pt = Tallen(S,T)
```

```sage
line([v for v in S])+line([v for v in T])+point(pt,color='red',size=30)
```

```sage
S = {(2.1,2),(2.3,2)}
T = {(2.1,1),(2.3,1)}
b,pt = Tallen(S,T)
b
```

```sage
line([v for v in S])+line([v for v in T])
```

### 5. Definiu una classe Isosceles, formada per triangles isòsceles donats per la base i l'altura, definida a partir de la classe Triangle definida a dalt. 


Torno a copiar la classe Triangle que hem definit per a poder-la utilitzar

```sage
def distancia(ps):
    '''Calcula la distància entre dos punts de R^n'''
    if len(ps)!=2: raise TypeError('Ha de ser un llista de dos punts')
    return((sum((x-y)**2 for x,y in zip(*ps))**0.5))
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

```

```sage
class TriangleIsosceles(Triangle):
    def __init__(self, base,altura):
        Triangle.__init__(self,[0,0], [0,base], [altura,base/2])
        self.base = base
        self.altura = altura
    def area(self):
        return(self.base*self.altura)
```

```sage
T = TriangleIsosceles(10,10)
```

```sage
T.area()
```

```sage

```

```sage

```

### 6. Definiu una classe dels Quadrilàters, determinada donant $4$ punts del pla. Observeu primer que si els quadrilàters no són convexos aleshores els vèrtexs no determinen el quadrilàter. Per tant el primer que hem de fer és comprovar si els 4 punts formen o no un quadrilàter convex, i a més ordenar bé els vèrtexs. Tot això ho podem fer usant que en un quadrilàter convex els segments determinats pels vèrtexs oposats es tallen en un punt (necessàriament a l'interior del quadrilàter). La classe pot tenir una funció àrea, i funcions que comprovin si el quadrilàter és un trapezi, si és un para\l.lelogram, si és un rombe, si és un rectangle, si és un quadrat, etc. També podeu provar de definir igualtat de quadrilàters de manera anàloga a com ho hem fet pels triangles però no és fàcil. 


Hi ha varis problemes tècnics a resoldre. Per exemple, he decidit que els punts siguin tuples, i per tant els transformo a tuples només començar. A més accepto com a quadrilater qualsevol llista de 4 punts, però a l'hora de obtenir els vèrtexs ordenats adequadament, si no determinen un quadrilater convex faig que retorni la llista buida. 


Podríem haver decidit que un quadrilàter és una llista de punts i si surt "degenerat" per haver agafat la llista mal ordenada llavors no tingués les altres instàncies. Però llavors hauríem de decidir que fer amb els quadrilàters no convexos. 


Primer he definit una funció similar a la de EsTallen per saber si els costats determinades per dos conjunts de dos punts són paral·lels o no. 

```sage
def SonParalels(S,T):
    '''Donats dos conjunts de dos punts del pla,
    determina si els costats que formen son paral·lels o no
    '''
    if type(S) != set or type(T) != set:
        raise TypeError('No són conjunts')
    if len(S) != 2 or len(T) != 2: 
        raise TypeError('Els conjunts no tenen dos elements')
    if any(len(v)!= 2 for v in S.union(T)):
        raise TypeError('Han de tenir dues coordenades')
    E = RR^2
    V = [E(v) for v in S] + [E(v) for v in T]
    A = matrix([V[0]-V[1],V[3]-V[2]]).det()
    return A == 0
```

```sage
class Quadrilater: 
    def __init__(self, punt1,punt2,punt3,punt4):
        self.__p1 = tuple(punt1)
        self.__p2 = tuple(punt2)
        self.__p3 = tuple(punt3)
        self.__p4 = tuple(punt4)
    def vertexs(self):
        V = [self.__p1,self.__p2,self.__p3,self.__p4]
        if EsTallen({V[0],V[2]},{V[1],V[3]}):
            return(V)
        elif EsTallen({V[0],V[1]},{V[2],V[3]}):
            return([V[0],V[2],V[1],V[3]])
        elif EsTallen({V[0],V[3]},{V[1],V[2]}):
            return([V[0],V[1],V[3],V[2]])
        else:
            return([])
    def EsConvex():
        V = self.vertexs()
        return len(V) != 0
    def costats(self):
        V = self.vertexs()
        if len(V)==0:
            return []
        C = [distancia([V[i],V[i+1]]) for i in range(3)] 
        C.append(distancia([V[3],V[0]]))
        return C
    def perimetre(self):
        return sum(self.costats())
    def baricentre(self):
        V = self.vertexs()
        if len(V)==0:
            return 
        b,pt = Tallen({V[0],V[2]},{V[1],V[3]})
        return pt
    def area(self):
        V = self.vertexs()
        if len(V)==0:
            return 0
        A1 = Triangle(V[0],V[1],V[2]).area()
        A2 = Triangle(V[0],V[3],V[2]).area()
        return A1+A2
    def trapezi(self): #Mirem si tenen dos costats oposats paral·lels
        V = self.vertexs()
        if len(V)==0:
            return False
        return SonParalels({V[0],V[1]},{V[2],V[3]}) or SonParalels({V[0],V[3]},{V[1],V[2]})
    # He fet dues instancies de paral·lelogram. Les dues van bé.
    def paralelogramc(self): #Mirem si tenen costats oposats iguals
        C = self.costats()
        if len(C)==0:
            return False
        return C[0]==C[2] and C[1] == C[3]
    def paralelogram(self):  #Mirem si tenen costats oposats paral·lels
        V = self.vertexs()
        if len(V)==0:
            return False
        return SonParalels({V[0],V[1]},{V[2],V[3]}) and SonParalels({V[0],V[3]},{V[1],V[2]})
    def rombe(self): #Mirem si tenen tots els costats iguals
        C = self.costats()
        if len(C)==0:
            return False
        return self.paralelogramc() and C[0] == C[1]        
    def rectangle(self): # Paral·lelogram que té dos costats perpendiculars
        V = self.vertexs()
        if len(V)==0:
            return False
        # Calculem el producte escalar dels dos costats amb vertex V[0]. Ha de ser 0.
        esc = (V[1][0]-V[0][0])*(V[3][0]-V[0][0])+(V[1][1]-V[0][1])*(V[3][1]-V[0][1])
        return self.paralelogram() and esc == 0
    def quadrat(self): #Un quadrat és un rectangle que és un rombe
        return self.rectangle() and self.rombe()
    # He fet una instància que fa un plot del quadrilàter per poder visualitzar el que fem 
    def plot(self):
        V = self.vertexs()
        if len(V)==0:
            return 
        pV = points(V,color='red',axes=False,aspect_ratio=1)
        lV = line(V[:2])
        lV += line(V[1:3])
        lV += line(V[2:])
        lV += line([V[3],V[0]])
        return pV+lV
    # Defineixo igualtat entre Quadrilaters si es poden descomposar en dos triangles iguals.
    # Trenquem el primer d'una manera, i el segon de les 2 possibles maneres
    # Cal comparar el primer triangle del primer quadrilater amb casacun dels 4 del segon
    # i el segon triangle del primer quadrilater amb el complementari del segon. 
    def __eq__(self, other):
            V1 = self.vertexs()
            V2 = other.vertexs()
            T111 = Triangle(V1[0],V1[1],V1[2])
            T112 = Triangle(V1[1],V1[2],V1[3])
            T211 = Triangle(V2[0],V2[1],V2[2])
            T212 = Triangle(V2[1],V2[2],V2[3])
            T221 = Triangle(V2[0],V2[1],V2[3])
            T222 = Triangle(V2[1],V2[2],V2[3])
            bo1 = (T111 == T211) and (T112 == T212)
            bo2 = (T111 == T212) and (T112 == T211)
            bo3 = (T111 == T221) and (T112 == T212)
            bo4 = (T111 == T221) and (T112 == T222)
            return bo1 or bo2 or bo3 or bo4 
    def __ne__(self, other):
        return not self.__eq__(other)
```

```sage

```

Un quadrat

```sage
punt1 = (0,0)
punt2 = (0,1)
punt3 = (1,0)
punt4 = (1,1)
Q1 = Quadrilater(punt1,punt2,punt3,punt4)
```

```sage
Q1.vertexs()
```

```sage
Q1.area()
```

```sage
Q1.costats()
```

```sage
Q1.baricentre()
```

```sage
Q1.trapezi()
```

```sage
Q1.rombe()
```

```sage
Q1.paralelogram()
```

```sage
Q1.paralelogramc()
```

```sage
Q1.rectangle()
```

```sage
Q1.quadrat()
```

```sage
Q1.plot()
```

Un rectangle 

```sage
punt1 = (0,0)
punt2 = (0,2)
punt3 = (1,0)
punt4 = (1,2)
Q1 = Quadrilater(punt1,punt2,punt3,punt4)
```

```sage
Q1.vertexs()
```

```sage
Q1.area()
```

```sage
Q1.costats()
```

```sage
Q1.baricentre()
```

```sage
Q1.trapezi()
```

```sage
Q1.rombe()
```

```sage
Q1.paralelogram()
```

```sage
Q1.paralelogramc()
```

```sage
Q1.rectangle()
```

```sage
Q1.quadrat()
```

```sage
Q1.plot()
```

Un paral·lelogram no rectangle

```sage
punt1 = (0,0)
punt2 = (1,1)
punt3 = (3,0)
punt4 = (4,1)
Q1 = Quadrilater(punt1,punt2,punt3,punt4)
```

```sage
Q1.vertexs()
```

```sage
Q1.area()
```

```sage
Q1.costats()
```

```sage
Q1.baricentre()
```

```sage
Q1.trapezi()
```

```sage
Q1.rombe()
```

```sage
Q1.paralelogram()
```

```sage
Q1.paralelogramc()
```

```sage
Q1.rectangle()
```

```sage
Q1.quadrat()
```

```sage
Q1.plot()
```

Un trapeci no paral·lelogram

```sage
punt1 = (0,0)
punt2 = (1,1)
punt3 = (3,0)
punt4 = (6,1)
Q1 = Quadrilater(punt1,punt2,punt3,punt4)
```

```sage
Q1.vertexs()
```

```sage
Q1.area()
```

```sage
Q1.costats()
```

```sage
Q1.baricentre()
```

```sage
Q1.trapezi()
```

```sage
Q1.rombe()
```

```sage
Q1.paralelogram()
```

```sage
Q1.paralelogramc()
```

```sage
Q1.rectangle()
```

```sage
Q1.quadrat()
```

```sage
Q1.plot()
```

Un de no convex

```sage
punt1 = (0,0)
punt2 = (1,2)
punt3 = (3,0)
punt4 = (1,1)
Q1 = Quadrilater(punt1,punt2,punt3,punt4)
points([punt1,punt2,punt3,punt4])
```

```sage
Q1.vertexs()
```

```sage
Q1.area()
```

```sage
Q1.costats()
```

```sage
Q1.baricentre()
```

```sage
Q1.trapezi()
```

```sage
Q1.rombe()
```

```sage
Q1.paralelogram()
```

```sage
Q1.paralelogramc()
```

```sage
Q1.rectangle()
```

```sage
Q1.quadrat()
```

```sage

```

Un rombe no quadrat

```sage
punt1 = (0,0)
punt2 = (3,4)
punt3 = (5,0)
punt4 = (8,4)
Q1 = Quadrilater(punt1,punt2,punt3,punt4)
```

```sage
Q1.vertexs()
```

```sage
Q1.area()
```

```sage
Q1.costats()
```

```sage
Q1.baricentre()
```

```sage
Q1.trapezi()
```

```sage
Q1.rombe()
```

```sage
Q1.paralelogram()
```

```sage
Q1.paralelogramc()
```

```sage
Q1.rectangle()
```

```sage
Q1.quadrat()
```

```sage
Q1.plot()
```

Un que no és "res" (de fet és un estel o deltoïde)

```sage
punt1 = (0,0)
punt2 = (3,4)
punt3 = (5,0)
punt4 = (6,3)
Q1 = Quadrilater(punt1,punt2,punt3,punt4)
```

```sage
Q1.plot()
```

```sage
Q1.vertexs()
```

```sage
Q1.area()
```

```sage
Q1.costats()
```

```sage
Q1.baricentre()
```

```sage
Q1.trapezi()
```

```sage
Q1.rombe()
```

```sage
Q1.paralelogram()
```

```sage
Q1.paralelogramc()
```

```sage
Q1.rectangle()
```

```sage
Q1.quadrat()
```

```sage

```

```sage

```

Algun exemple de comparació

```sage
punt1 = (0,0)
punt2 = (3,4)
punt3 = (5,0)
punt4 = (6,3)
Q1 = Quadrilater(punt1,punt2,punt3,punt4)
Q2 = Quadrilater(punt2,punt3,punt4,punt1)
```

```sage
Q1 == Q2
```

```sage
punt1 = (1,1)
punt2 = (5,4)
punt3 = (1,6)
punt4 = (4,7)
Q2 = Quadrilater(punt2,punt3,punt4,punt1)
```

```sage
Q1 == Q2
```

```sage
Q1.plot()+Q2.plot()
```

```sage

```
