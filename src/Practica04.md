---
jupyter:
  title: 'Pràctica 4: Programació orientada a objectes'
  authors:
  - name: Marc Masdeu
  - name: Xavier Xarles
  jupytext:
    notebook_metadata_filter: title,authors
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


# Programació Orientada a Objectes

Fins ara hem vist com programar amb Python amb el què s'anomena el model *imperatiu*: el programa és
un seguit d'instruccions que l'ordinador ha de realitzar. L'objectiu d'aquesta pràctica és introduir un model de programació
basat en diferents *objectes* i com interactuen entre ells.

## Plantejament

Suposem que volem treballar amb els nombres *diàdics*: es tracta d'aquells racionals que es poden escriure
com $a/b$ amb $b$ una potència de $2$. Per exemple, tot enter és un diàdic, però també ho són $-1/2$, $3/8$,
o $-27/64$. Observem que la suma i producte de diàdics segueix essent un diàdic.

Fixem-nos que tot diàdic es pot representar de manera única com un parell $(a,n)$ on $a$ és un enter senar i $n$ és un enter qualsevol:
la parella $(a,n)$ es correspon al racional $a\cdot 2^n$.

Podríem definir doncs:

```sage
def inicialitza_diadic(x):
    x = QQ(x)
    a, b = x.numerator(), x.denominator()
    n = -b.valuation(2) # Estem suposant que el denominador és potència de 2
    if n == 0: # Resulta que tenim un enter
        n = a.valuation(2)
        a /= 2**n
    return (a, n)

def producte_diadics(x, y):
    a, n = x
    b, m = y
    return a * b, n + m
```

Escriviu el codi per la suma de diàdics:

```sage
def suma_diadics(x,y):
    pass
```

I ara les podem fer servir així:

```sage
D = inicialitza_diadic(3/8)
E = inicialitza_diadic(-1/4)
F = producte_diadics(D, E)
print(f'El diàdic {D[0]} · 2^{D[1]} multiplicat pel diàdic {E[0]} · 2^{E[1]} dona el diàdic {F[0]} · 2^{F[1]}.')
```

Ja veiem que seria útil tenir una funció que ens retorni la representació d'un diàdic preparada per imprimir:

```sage
def repr_diadic(x):
    return f'{x[0]} · 2^{x[1]}'
```

Ara ho podem provar:

```sage
print(f'El diàdic {repr_diadic(D)} multiplicat pel diàdic {repr_diadic(E)} dona el diàdic {repr_diadic(F)}.')
```

Hem triat de representar un diàdic com a tupla, perquè així no es pot modificar. Un inconvenient, però, és que
no tenim desat enlloc què significa cadascun dels camps. Ho podríem solucionar amb un diccionari. Els camps $a$ i $n$
els podem anomenar *mantissa* i *exp*onent (al segon semestre veureu per què hem triat aquests noms...):

```sage
D = {'mantissa': 3, 'exp': 3}
E = {'mantissa': -1, 'exp': 2}

def inicialitza_diadic(x):
    x = QQ(x)
    a, b = x.numerator(), x.denominator()
    n = -b.valuation(2) # Estem suposant que el denominador és potència de 2
    if n == 0: # Resulta que tenim un enter
        n = a.valuation(2)
        a /= 2**n
    return {'mantissa' : a, 'exp' : n}
    
def repr_diadic(x):
    return f'{x['mantissa']} * 2^{x['exp']}'

def producte_diadics(x, y):
    return {'mantissa' : x['mantissa'] * y['mantissa'], 'exp' : x['exp'] + y['exp']}
```

Desavantatges que podem trobar en aquesta implementació:

- El diccionari és mutable. Quan el manipulem podem canviar les dades sense voler.
- Ens cal documentar en algun lloc les claus que farà servir el diccionari (i vigilar amb els *typos*)
- A la funció `producte_diadics()` hi ha moltes paraules repetides...

## Classes i objectes

Python ens dona una manera de crear els nostres propis tipus. Els diccionaris
són un tipus genèric, però si Python ens proporcionés un tipus `Diadic` que contingués tota la funcionalitat dels diàdics,
encara seria millor. Aquesta és la funció de les classes.

**Nota:** Podem pensar una classe com un *plànol*, a partir de la qual es creen *objectes* o
*instàncies*. Cadascun d'aquests objectes contindrà dades diferents, però estaran
estructurades tal i com dicti la classe.


```sage
class Diadic:
    pass  

def inicialitza_diadic(x):
    x = QQ(x)
    a, b = x.numerator(), x.denominator()
    n = -b.valuation(2) # Estem suposant que el denominador és potència de 2
    if n == 0: # Resulta que tenim un enter
        n = a.valuation(2)
        a /= 2**n
    d = Diadic()
    d.mantissa = a
    d.exp = n
    return d

def repr_diadic(x):
    return f'{x.mantissa} · 2^{x.exp}'

def producte_diadics(x, y):
    d = Diadic()
    d.mantissa = x.mantissa * y.mantissa
    d.exp = x.exp + y.exp
    return d
```

```sage
D = inicialitza_diadic(3/8)
E = inicialitza_diadic(-1/4)
F = producte_diadics(D, E)
print(f'El diàdic {repr_diadic(D)} multiplicat pel diàdic {repr_diadic(E)} dona el diàdic {repr_diadic(F)}.')
```

**Nota:** Per convenció, els noms de les classes s'escriuen en Majúscula. Les excepcions
són les classes que Python ja ens dona: `list`, `tuple`, `int`, `dict`,...


El codi anterior no és gaire *Pythonic*: encara que hem donat nom als *atributs*
que conformen un diàdic, els hem d'assignar manualment. Una millor versió seria
la següent, que fa servir el mètode especial `__init__`:

```sage
class Diadic:
    def __init__(self, x):
        x = QQ(x)
        a, b = x.numerator(), x.denominator()
        n = -b.valuation(2) # Estem suposant que el denominador és potència de 2
        if n == 0: # Resulta que tenim un enter
            n = a.valuation(2)
            a /= 2**n
        self.mantissa = a
        self.exp = n

def repr_diadic(x):
    return f'{x.mantissa} · 2^{x.exp}'

def producte_diadics(x, y):
    d = Diadic()
    d.mantissa = x.mantissa * y.mantissa
    d.exp = x.exp + y.exp
    return d
```

```sage
D = Diadic(3/8)
E = Diadic(-1/4)
F = producte_diadics(D, E)
print(f'El diàdic {repr_diadic(D)} multiplicat pel diàdic {repr_diadic(E)} dona el diàdic {repr_diadic(F)}.')
```

Una altra avantatge d'aquest punt de vista és l'*encapsulació*: tot el que estigui
relacionat amb els diàdics hauri de pertanyer a la classe `Diadic`.
Per exemple, podem controlar errors:

```sage
class Diadic:
    def __init__(self, x):
        x = QQ(x)
        a, b = x.numerator(), x.denominator()
        n = -b.valuation(2) # Estem suposant que el denominador és potència de 2
        if b != 2**-n:
            raise ValueError(f'{x} no és un diàdic, perquè té denominador {b} que no és potència de 2')
        if n == 0: # Resulta que tenim un enter
            n = a.valuation(2)
            a /= 2**n
        self.mantissa = a
        self.exp = n

def repr_diadic(x):
    return f'{x.mantissa} · 2^{x.exp}'
```

```sage
A = Diadic(5/20)
print(f'Hem definit el diàdic {repr_diadic(A)}')
```

I aquest hauria de donar error:

```sage
B = Diadic(3/20)
```

Seguim amb la idea d'encapsulació: fixem-nos que la feina de generar
un `str` amb dades del diàdic també la podem delegar a la classe, amb el mètode especial `__str__`:

```sage
class Diadic:
    def __init__(self, x):
        x = QQ(x)
        a, b = x.numerator(), x.denominator()
        n = -b.valuation(2) # Estem suposant que el denominador és potència de 2
        if b != 2**-n:
            raise ValueError(f'{x} no és un diàdic, perquè té denominador {b} que no és potència de 2')
        if n == 0: # Resulta que tenim un enter
            n = a.valuation(2)
            a /= 2**n
        self.mantissa = a
        self.exp = n

    def __str__(self):
        return f'{self.mantissa} · 2^{self.exp}'
```

```sage
A = Diadic(5/20)
print(f'Acabem de definir el diàdic {A}')
```

Els *mètodes* `__init__()` i `__str__` els proporciona Python per defecte, i són especials. Per
això porten la doble barra baixa (*double under* o *dunder* en anglès). Però també
podem inventar-nos els nostres propis mètodes. Per exemple, podem convertir un diàdic a un racional així:

```sage
class Diadic:
    def __init__(self, x):
        x = QQ(x)
        a, b = x.numerator(), x.denominator()
        n = -b.valuation(2) # Estem suposant que el denominador és potència de 2
        if b != 2**-n:
            raise ValueError(f'{x} no és un diàdic, perquè té denominador {b} que no és potència de 2')
        if n == 0: # Resulta que tenim un enter
            n = a.valuation(2)
            a /= 2**n
        self.mantissa = a
        self.exp = n

    def __str__(self):
        return f'{self.mantissa} · 2^{self.exp}'

    def racional(self):
        return QQ(self.mantissa * 2**self.exp)
```

```sage
A = Diadic(3/8)
print(f'{A = } és el racional {A.racional()}')
```

**Atenció:** Les classes tenen *atributs* (no pas variables) i *mètodes* (no pas funcions). És
simplement terminologia. Per exemple, la classe `Diadic` té atributs `mantissa` i `exp`, i mètode `racional()`, entre altres.


## Propietats

Encara que ens hem esforçat a fer les comprovacions d'errors quan creem una instància
de `Diadic`, els atributs de la classe es poden modificar a qualsevol lloc del
programa. Per exemple:


```sage
D = Diadic(3/8)
D.mantissa = 4 # Això funciona, i aleshores D no representa un diàdic!!
print(D)
```

Aquest comportament és indesitjable, i hi ha una manera fàcil de millorar-ho.
Es tracta d'afegir mètodes que modifiquin els
atributs, i amagar d'alguna manera els propis atributs. Hi ha dues maneres de fer-ho:

1. Escriure mètodes `get_...()` i `set_...()`.
2. Fent servir el decorador `property`.

El codi queda així, fent servir les dues variants. Primer, amb `get_...` i `set_...`:

```sage
class Diadic:
    def __init__(self, x):
        x = QQ(x)
        a, b = x.numerator(), x.denominator()
        n = -b.valuation(2) # Estem suposant que el denominador és potència de 2
        if b != 2**-n:
            raise ValueError(f'{x} no és un diàdic, perquè té denominador {b} que no és potència de 2')
        if n == 0: # Resulta que tenim un enter
            n = a.valuation(2)
            a /= 2**n
        self._mantissa = a
        self._exp = n

    def set_mantissa(self, a):
        if a % 2 == 1:
            raise ValueError('La mantissa ha de ser senar')
        self._mantissa = a

    def get_mantissa(self):
        return self._mantissa

    def set_exp(self, n):
        self._exp = ZZ(n) # Cal que sigui un enter

    def get_exp(self):
        return self._exp

    def __str__(self):
        return f'{self.get_mantissa()} · 2^{self.get_exp()}'

    def racional(self):
        return QQ(self.get_mantissa() * 2**self.get_exp())
```

```sage
D = Diadic(2/3)
D.set_mantissa(4) # Dona error
```

Ara amb el decorador:

```sage
class Diadic:
    def __init__(self, x):
        x = QQ(x)
        a, b = x.numerator(), x.denominator()
        n = -b.valuation(2) # Estem suposant que el denominador és potència de 2
        if b != 2**-n:
            raise ValueError(f'{x} no és un diàdic, perquè té denominador {b} que no és potència de 2')
        if n == 0: # Resulta que tenim un enter
            n = a.valuation(2)
            a /= 2**n
        self._mantissa = a
        self._exp = n


    @property
    def mantissa(self):
        return self._mantissa

    @mantissa.setter
    def mantissa(self, a):
        a = ZZ(a) # Cal que sigui un enter...
        if a % 2 == 1: # ...senar
            raise ValueError('La mantissa ha de ser senar')
        self._mantissa = a


    @property
    def exp(self):
        return self._exp

    @exp.setter
    def exp(self, n):
        self._exp = ZZ(n) # Cal que sigui un enter

    def __str__(self):
        return f'{self.mantissa} · 2^{self.exp}' # més net a l'hora de cridar

    def racional(self):
        return QQ(self.mantissa * 2**self.exp) # més net a l'hora de cridar
```


**Atenció:** Els mètodes i atributs que comencen amb `_` es consideren privats. Hi ha llenguatges
de programació que no permeten accedir als mètodes/atributs privats des de fora la classe.
Python funciona amb un *pacte de cavallers*: si el programador de la classe hi ha posat
una `_`, vol dir *no ho toquis*. Si hi posa dues barres baixes `__` vol dir que
*no ho toquis, de veritat*. Però en ambdós casos s'assumeix que l'usuari de la classe
és una adult responsable, i no *Python* no s'hi posa.


**Nota:** L'avantatge de fer servir `attribute` i `setter`  és que si la classe ja s'estava utilitzant
no haurem de canviar res del codi. Diem que l'API de la nostra classe no canvia. D'altra
banda, hem d'anar amb compte amb ells *getters* i els *setters*. Quan l'usuari assigna o llegeix un atribut,
no espera que hi pugui haver errors i per tant no programarà els `try...except` corresponents. Això
vol dir que hem de ser molt curosos amb el codi que hi posem, o acabarem causant més problemes dels
què hem resolt. Si el codi fa moltes comprovacions que poden ser problemàtiques, sovint
és més expressiu implementar mètodes de la forma `get_...()` i `set_...()`.

## Sobrecàrrega d'operadors

Fixem-nos que hi ha funcions que són ben pròpies dels diàdics que encara no hem incorporat a la classe.
Quan volem operar amb diàdics, ens aniria bé poder fer servir els operadors habituals `+` i `*`. Això ho podem fer
amb certs mètodes especials, com són `__add__` i `__mul__`:

```sage
class Diadic:
    def __init__(self, x):
        x = QQ(x)
        a, b = x.numerator(), x.denominator()
        n = -b.valuation(2) # Estem suposant que el denominador és potència de 2
        if b != 2**-n:
            raise ValueError(f'{x} no és un diàdic, perquè té denominador {b} que no és potència de 2')
        if n == 0: # Resulta que tenim un enter
            n = a.valuation(2)
            a /= 2**n
        self._mantissa = a
        self._exp = n


    @property
    def mantissa(self):
        return self._mantissa

    @mantissa.setter
    def mantissa(self, a):
        a = ZZ(a) # Cal que sigui un enter...
        if a % 2 == 1: # ...senar
            raise ValueError('La mantissa ha de ser senar')
        self._mantissa = a

    @property
    def exp(self):
        return self._exp

    @exp.setter
    def exp(self, n):
        self._exp = ZZ(n) # Cal que sigui un enter

    def __str__(self):
        return f'{self.mantissa} · 2^{self.exp}' # més net a l'hora de cridar

    def racional(self):
        return QQ(self.mantissa * 2**self.exp) # més net a l'hora de cridar
    
    def __add__(self, other):
        return Diadic(sef.racional() + other.racional()) # Penseu una implementació millor
    
    def __mul__(self, other):
        return Diadic(self.mantissa * other.mantissa, self.exp + other.exp)
```

```sage
D = Diadic(5/20)
E = Diadic(3/8)
print(f'{D = }, {E = }')
print(f'Suma: {D} * {E} = {D + E}')
print(f'Producte: {D} * {E} = {D * E}')
```

Hi ha molts altres mètodes *especials* com aquest. Se'ls anomena **mètodes màgics**, o **dunder**
(de **d**ouble under), i es poden trobar [aquí](https://docs.python.org/3/reference/datamodel.html#special-method-names).

## Herència

Fixem-nos que en comptes de permetre potències de $2$ en el denominador ("invertim el $2$") també podríem permetre potències
d'un primer qualsevol ("invertim un primer $p$"). Per exemple, si invertim el $5$, obtenim els nombres que es poden representar
com $(a,n)$, on $a$ és un enter no divisible per $5$, i $n$ és un enter qualsevol. La parella $(a,n)$ representa llavors el racional $a\cdot 5^n$.

Podem anomenar a aquesta classe els *Adics*:


```sage
class Adic:
    def __init__(self, x, p = 2):
        self.p = p
        x = QQ(x)
        a, b = x.numerator(), x.denominator()
        n = -b.valuation(p) # Estem suposant que el denominador és potència de p
        if b != p**-n:
            raise ValueError(f'{x} no és un àdic, perquè té denominador {b} que no és potència de {p}')
        if n == 0: # Resulta que tenim un enter
            n = a.valuation(p)
            a /= p**n
        self._p = p
        self._mantissa = a
        self._exp = n

    @property
    def p(self):
        return self._p
    
    @p.setter
    def p(self, p):
        p = ZZ(p)
        if not p.is_prime():
            raise ValueError('p ha d eser primer')
        self._p = p

    @property
    def mantissa(self):
        return self._mantissa

    @mantissa.setter
    def mantissa(self, a):
        a = ZZ(a) # Cal que sigui un enter...
        if a % self.p != 0:
            raise ValueError('La mantissa no pot ser divisible per p')
        self._mantissa = a

    @property
    def exp(self):
        return self._exp

    @exp.setter
    def exp(self, n):
        self._exp = ZZ(n) # Cal que sigui un enter

    def __str__(self):
        return f'{self.mantissa} · {self.p}^{self.exp}'

    def racional(self):
        return QQ(self.mantissa * self.p**self.exp)
    
    def __add__(self, other):
        return Diadic(sef.racional() + other.racional()) # Penseu una implementació millor
    
    def __mul__(self, other):
        return Diadic(self.mantissa * other.mantissa, self.exp + other.exp)
```

```sage
D = Adic(13, 7)
E = Adic(6/14, 7)
print(f'{D = }, {E = }')
print(f'Suma: {D} * {E} = {D + E}')
print(f'Producte: {D} * {E} = {D * E}')
```

Ens interessa mantenir la classe `Diadic`, i per evitar repetir codi podem fer que aquesta *heredi* de la classe `Adic`.

```sage
class Diadic(Adic)
    def __init__(self, x):
        super().__init__(self, x, 2)

    def semisuma(self, other): # Pot tenir mètodes propis
        return (self + other) / 2
```

Qualsevol mètode que accepti objectes de tipus `Adic` podrà treballar amb objectes `Diadic`. Això és
el què es coneix com a *polimorfisme*.

## Mètodes de classe

Observem la funció que hem fet servir abans, que ens demana dos diàdics i imprimeix la suma i el producte.
Clarament està relacionada amb els diàdics, i per tant potser la volem incloure la classe. D'altra banda,
no té massa sentit haver de crear un diàdic "de mentida" per accedir al mètode en qüestió.

Els mètodes de classe s'utilitzen quan el mètode que volem implementar no depèn de les
dades de l'objecte en concret, sinó que és comú a tots els objectes. La variable `self`
no hi és, i el primer paràmetre s'anomena `cls` i és la pròpia classe. Queda així:

```sage
class Diadic(Adic)
    def __init__(self, x):
        super().__init__(self, x, 2)

    def semisuma(self, other): # Pot tenir mètodes propis
        return (self + other) / 2

    @classmethod
    def exemple(cls, D, E):
        D = cls(D)
        E = cls(E)
        print(f'{D = }, {E = }')
        print(f'Suma: {D} * {E} = {D + E}')
        print(f'Producte: {D} * {E} = {D * E}')
```

```sage
Diadic.exemple(-1/2, 3/8)
```


## Exercicis


### Exercici 1


Definiu una classe dels Quadrilàters (convexos), determinada donant
$4$ punts del pla.

Observeu primer que si els quadrilàters no són convexos aleshores
els vèrtexs no determinen el quadrilàter. Els següents tres
quadrilàters tenen els mateixos vèrtexs.

![Tres quadrilàters no convexos diferents amb els mateixos
vèrtexs](quadnconvex1.png)

![Tres quadrilàters no convexos diferents amb els mateixos
vèrtexs](quadnconvex2.png)

![Tres quadrilàters no convexos diferents amb els mateixos
vèrtexs](quadnconvex3.png)

Per tant el primer que hem de fer és comprovar si els 4 punts formen
o no un quadrilàter convex, i a més ordenar bé els vèrtexs. Tot això
ho podem fer usant que en un quadrilàter convex els segments
determinats pels vèrtexs oposats es tallen en un punt
(necessàriament a l'interior del quadrilàter). El primer dels dos quadrilàters següents és convex, però el segon no. Ho veiem
amb els següents dos dibuixos.

![Quadrilàter convex amb les diagonals
dibuixades](quadconvex.png)\

![El mateix però amb els vèrtexs mal
posats](quadconvexn.png)\

La classe pot tenir una funció àrea, i funcions que comprovin si el
quadrilàter és un trapezi, si és un paral·lelogram, si és un rombe,
si és un rectangle, si és un quadrat, etc. També podeu provar de
definir igualtat de quadrilàters de manera anàloga a com ho hem fet
pels triangles, però no és tan fàcil.

-- begin hide
Hi ha varis problemes tècnics a resoldre. Per exemple, he decidit que els punts siguin tuples, i per tant els transformo a tuples només començar. A més accepto com a quadrilàter qualsevol llista de 4 punts, però a l'hora d'obtenir els vèrtexs ordenats adequadament, si no determinen un quadrilater convex faig que retorni la llista buida.

Podríem haver decidit que un quadrilàter és una llista de punts i si surt "degenerat" per haver agafat la llista mal ordenada llavors no tingués les altres instàncies. Però llavors hauríem de decidir que fer amb els quadrilàters no convexos.

Primer he definit una funció similar a la de EsTallen per saber si els costats determinades per dos conjunts de dos punts són paral·lels o no.

```sage
def SonParalels(S,T):
    '''
	Donats dos conjunts de dos punts del pla,
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
        self._p1 = tuple(punt1)
        self._p2 = tuple(punt2)
        self._p3 = tuple(punt3)
        self._p4 = tuple(punt4)
    def vertexs(self):
        V = [self._p1,self._p2,self._p3,self._p4]
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
        if len(V) == 0:
            return 
        b, pt = Tallen({V[0],V[2]},{V[1],V[3]})
		assert b
        return pt
    def area(self):
        V = self.vertexs()
        if len(V) == 0:
            return 0
        A1 = Triangle(V[0],V[1],V[2]).area()
        A2 = Triangle(V[0],V[3],V[2]).area()
        return A1+A2
    def trapezi(self): # Mirem si tenen dos costats oposats paral·lels
        V = self.vertexs()
        if len(V) == 0:
            return False
        return SonParalels({V[0],V[1]},{V[2],V[3]}) or SonParalels({V[0],V[3]},{V[1],V[2]})
    # He fet dues instancies de paral·lelogram. Les dues van bé.
    def paralelogramc(self): # Mirem si tenen costats oposats iguals
        C = self.costats()
        if len(C) == 0:
            return False
        return C[0] == C[2] and C[1] == C[3]
    def paralelogram(self):  # Mirem si tenen costats oposats paral·lels
        V = self.vertexs()
        if len(V) == 0:
            return False
        return SonParalels({V[0],V[1]},{V[2],V[3]}) and SonParalels({V[0],V[3]},{V[1],V[2]})
    def rombe(self): #Mirem si tenen tots els costats iguals
        C = self.costats()
        if len(C) == 0:
            return False
        return self.paralelogramc() and C[0] == C[1]
    def rectangle(self): # Paral·lelogram que té dos costats perpendiculars
        V = self.vertexs()
        if len(V) == 0:
            return False
        # Calculem el producte escalar dels dos costats amb vertex V[0]. Ha de ser 0.
        esc = (V[1][0]-V[0][0])*(V[3][0]-V[0][0])+(V[1][1]-V[0][1])*(V[3][1]-V[0][1])
        return self.paralelogram() and esc == 0
    def quadrat(self): #Un quadrat és un rectangle que és un rombe
        return self.rectangle() and self.rombe()
    # He fet una instància que fa un plot del quadrilàter per poder visualitzar el que fem 
    def plot(self):
        V = self.vertexs()
        if len(V) == 0:
            return
        pV = points(V,color='red',axes=False,aspect_ratio=1)
        lV = line(V[:2])
        lV += line(V[1:3])
        lV += line(V[2:])
        lV += line([V[3],V[0]])
        return pV + lV
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
-- end hide