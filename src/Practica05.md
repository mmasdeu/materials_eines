---
jupyter:
  title: 'Pràctica 5: Complements a la programació en Python'
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
    display_name: SageMath 9.7
    language: sage
    name: sagemath

---

# Complements a la programació en Python


## Optimització

A vegades és important aconseguir que una funció s'executi el màxim de ràpid possible.
En aquests casos, potser ens cal triar entre dues o més possibles implementacions,
i per fer-ho és útil poder mesurar el temps que es triga en executar un bloc de codi.

La manera més fàcil de veure el temps que triga una certa funció és la comanda `%time`,
que podem posar davant de qualsevol càlcul. Per exemple:

```sage
%time 2 + 2
```

Observem que ens mostra quatre nombres diferents. La primera fila ("CPU times")
ens diu quant temps li ha dedicat el processador, repartit entre la part que es dedica
a realment el càlcul i la part que es dedica a gestionar la interacció amb el sistema.
La segona fila ("Wall time") ens diu el temps "de rellotge de paret" que ha passat. Aquest temps
sempre serà més gran que el "CPU time", perquè el nostre ordinador a més de fer el càlcul que li
hem demanat també està fent altres coses (per exemple, potser esteu escoltant música, o mirant
una pel·lícula,...).

Una variant d'aquesta comanda és `%timeit`, que executa la instrucció diverses vegades i fa un promig.

```sage
%timeit 2 + 2
```

Pot ser que vulguem veure aquesta informació dins d'una funció que estem programent. Aleshores
podem fer servir les funcions `cputime()` i `walltime()`, que tenen el mateix funcionament
però tracten un o l'altre recompte de temps com abans. Ho veurem amb un exemple. Primer, simularem
que tenim una classe molt gran (amb un mil·lió d'alumnes), cadascun amb el seu NIU. Desarem les
notes en una llista els elements de la qual seran tuples `(NIU, nota)`.

```sage
notes_eines = [(randint(0, 10**7),randint(0,10)) for _ in range(10**6)]
```

Fem una funció que ens digui quina nota ha tret un alumne amb un NIU donat:

```sage
def quina_nota(NIU_alumne):
    for NIU, nota in notes_eines:
        if NIU_alumne == NIU:
            return nota
    raise ValueError(f"El {NIU_alumne = } no s'ha trobat.")
```

```sage
quina_nota(1234567)
```

```sage
quina_nota(notes_eines[50000][0])
```

Suposem ara que volem fer una funció que ens digui la mitjana de les notes de 10 alumnes a l'atzar. Ho podríem fer així:

```sage
def mitjana(n = 10): # Valor per defecte
    suma = 0
    num_notes = len(notes_eines)
    total_notes = 0
    while total_notes < n:
        try:
            suma += quina_nota(randint(0, num_notes-1))
            total_notes += 1
        except ValueError:
            pass
    return RR(suma) / n
```

```sage
%time mitjana()
```

Per millorar les funcions anteriors, podem mirar el temps que triga la funció `quina_nota` a cada iteració. Modificarem la funció `mitjana` de la següent manera.


```sage
def mitjana(n = 10): # Valor per defecte
    suma = 0
    num_notes = len(notes_eines)
    total_notes = 0
    t0 = cputime() # Es desa un enter que indica el temps
    while total_notes < n:
        try:
            suma += quina_nota(randint(0, num_notes-1))
            total_notes += 1
            t1 = cputime()
            print(f"L'última crida ha durat {1000*(t1 - t0)} milisegons. Notes acumulades: {total_notes}.")
            t0 = t1
        except ValueError:
            pass
    return RR(suma) / n
```

```sage
mitjana()
```

Una altra funcionalitat que ens pot ser útil és `%prun` (de *profiler run*). Ens mostra un informe que ens diu quantes vegades s'ha cridat cada funció, i quin temps s'hi ha trigat.

```sage
%prun mitjana()
```

Si ens fixem en la sortida de la comanda anterior, podem veure que la funció `quina_nota` no és gaire ràpida. Podem intentar-la millorar i, de fet, n'hi ha prou amb fer servir un diccionari per accedir ràpidament a les notes:
```sage
dict_notes = dict(notes_eines)
def quina_nota(NIU_alumne):
    try:
        return dict_notes[NIU_alumne]
    except KeyError:
        raise ValueError(f"El {NIU_alumne = } no s'ha trobat.")
```

Podem veure el resultat:

```sage
mitjana()
```

## Fitxers

A vegades no volem escriure els resultats per pantalla, sino que els necessitem en un fitxer (per poder-los enviar a algú, o per afegir-los a un document que estem escrivint, per exemple). També ens pot convenir llegir un fitxer on hi tenim dades que volem estudiar. Una primera manera és amb les funcions `load()` i `save()` que ens proporciona el **SageMath**. Suposem que tenim una llista (o qualsevol objecte de **SageMath**):

```sage
L = [5,3,4,1,6,7,1,9,4,3,2]
```

La podem desar amb:
```sage
save(L, 'lamevallista')
```

Aquesta comanda crea un fitxer anomenat `lamevallista.sobj`. Quan vulguem el podem recuperar amb:

```sage
M = load('lamevallista')
```

L'extensió `.sobj` indica que es tracta d'un "objecte Sage". Alguns objectes ens permeten desar-los com a imatge (per exemple el resultat d'una funció `plot`). Això ho podem fer especificant una extensió vàlida (`.png`, `.pdf`, `.svg`, ...):

```sage
G = plot(sin(x), 0, 2*pi)
save(G, 'sinus.png')
```

Ara bé, aquestes imatges han perdut la informació que venia del **SageMath**, i per tant no les podrem tornar a convertir a objectes de **SageMath**.

Encara que el format `.sobj` és útil per desar informació de **SageMath** quan aturem el Kernel, no és un format útil per compartir, ja que sense el **SageMath** no el podrem llegir. Tornant al cas de la llista, ens podria interessar escriure un fitxer de text (o una fulla excel,...) amb la informació que hi tenim. D'alguna manera, volem poder "imprimir" en el fitxer, igual que la funció `print` ens imprimeix en pantalla. Això es pot aconseguir de la manera següent:

```sage
f = open('llista.txt', 'w', encoding='utf-8') # 'w' indica que volem escriure (write).
for i in L:
    f.write(f'{i}\n') # Cal un String
f.close()
```

La primera línia obre el fitxer en mode escriptura. Li hem d'indicar la codificació (`utf-8`) perquè els caràcters especials s'escriguin bé. Ens retorna un objecte (`f`) de tipus "file", que té mètodes com ara el `.write()`, que és el que de fet escriu. Quan acabem, hem de tancar el fitxer (si no, es poden perdre dades). Això és fa amb el mètode `.close()`.

És molt important tancar els fitxers, i a vegades pot passar que ens en descuidem, o que abans de tancar-los es produeixi un error i no arribem a la instrucció de tancar. Per evitar problemes, hi ha una estructura millor que ens garanteix que passi el que passi el fitxer es tancarà. L'exemple anterior es faria:
```sage
with open('llista2.txt', 'w', encoding='utf-8') as f:
    for i in L:
        f.write(f'{i}\n') # Cal un String
```

Per llegir un fitxer, ho fem de manera semblant:
```sage
with open('llista.txt', 'r', encoding='utf-8') as f:
    for line in f:
        print(line)
```

## Classes


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

Si apliques `zip(*llista)` on `llista` és una llista, el que fa és "esborrar" els parèntesis de la llista per poder fer-li el zip. (S'utilita per desfer un zip, per exemple.) Fixeu-vos la diferència entre fer un zip amb `*` o sense `*`

```sage
list(zip(*[(1, 1, 2), (2, 3, 4)]))
```

```sage
list(zip((1, 1, 2), (2, 3, 4)))
```


Una classe (a Python) és una manera de poder agrupar dades i funcions
alhora, creant un nou tipus d'objecte i les formes de manipular-lo (i
per tant forma part del que s'anomena programació orientada objectes).
El **SageMath** i el **Python** són, de
fet, en el llenguatge de la programació orientada a objectes. Ja hem
vist molts exemples en els capítols anteriors: per exemple, els enters
de Sage són una classe, i tenen molts atributs i mètodes associats com
.factors(), .is_prime(), etc. Un altre exemple que hem vist és la classe
de les matrius, la classe dels vectors, i tants d'altres.

En aquesta secció veurem com podem crear nosaltres mateixos una classe
(senzilla), i com podem assignar-los atributs i mètodes amb ella.

El que farem serà crear la classe dels triangles en el pla. Primer de
tot hem de pensar com donarem un triangle. Hem optat per a definir-lo
com un conjunt de tres punts del pla $\mathbb{R}^2$ (això inclourà els
triangles "degenerats" formats per tres punts alineats, però en principi
no ens causarà problemes).

Així un triangle es crearà donant tres punts, com `T1 = Triangle([0,0],[0,12],[16,12])`.

Per a definir una classe posem `class` i el nom de la classe. Per
exemple, per a definir la classe `Triangle` utilitzarem `class`
`Triangle`. Després cal inicialitzar la classe amb `__init__` cal dir el
nom que utilitzarem internament a la classe (tradicionalment s'utilitza
self, però no caldria!), i les dades que les defineixen (els tres
punts).

Dins de la mateixa classe podem definir funcions aplicades al objecte.
Per exemple, en aquest cas la funció `area` calcula l'àrea del triangle
donat pels tres vèrtexs. ``

## Una classe per treballar amb triangles

Considerem el següent codi:
```sage
class Triangle:
    def __init__(self, punt1,punt2,punt3):
        self._p1 = punt1
        self._p2 = punt2
        self._p3 = punt3
        self.vertexs = (punt1, punt2, punt3)

    def baricentre(self):
        return (sum(a)/3 for a in zip(self.vertexs))
```

```sage
T = Triangle((0,0), (0,12), (16,12))
print(f'{T.vertexs = }')
print(f'{T.baricentre() = }')
```

El que hem fet ha estat crear un triangle format pels vèrtexs a $(0,0)$,
$(0,12)$ i $(16,12)$, i hem calculat el seu baricentre.


A part de calcular l'àrea podem incorporar moltes altres funcions. Per a
fer-ho primer crearem una funció per a calcular la distància entre dos
punts de $\mathbb{R}^n$:


```sage
def distancia(P, Q):
    '''Calcula la distància entre dos punts de R^n'''
    return((sum((xi-yi)**2 for xi, yi in zip(P, Q))**0.5))
```

```sage
distancia((1,2),(2,3))
```

Ara definirem de nou la classe dels `Triangle`:


```sage
class Triangle:
    def __init__(self, punt1,punt2,punt3):
        self._p1 = punt1
        self._p2 = punt2
        self._p3 = punt3
        self.vertexs = (punt1, punt2, punt3)

    def area(self):
        p1 = self._p1
        p2 = self._p2
        p3 = self._p3
        p123 = (p2[0] - p1[0]) * (p3[1] - p1[1])
        p132 = (p2[1] - p1[1]) * (p3[0] - p1[0])
        return abs((p123-p132) / 2)

    def costats(self):
        p = self.vertexs
        pp = [[a for a in p if a != b] for b in p]
        return sorted([distancia(*a) for a in pp])

    def perimetre(self):
        return sum(self.costats())

    def baricentre(self):
        p = self.vertexs
        return (sum(a)/3 for a in zip(*p))

    def inradi(self):
        return 2 * self.area() / self.perimetre()

    def circumradi(self):
        return prod(self.costats()) / (4 * self.area())

    def __eq__(self, other):
        return self.costats() == other.costats()

    def __ne__(self, other):
        return not self.__eq__(other)
```

```sage
T1 = Triangle([0,0],[0,12],[16,12])
print(f'{T1.vertexs = }')
print(f'{T1.costats() = }')
print(f'{T1.perimetre() = }')
print(f'{T1.area() = }')
print(f'{T1.baricentre() = }')
print(f'{T1.inradi() = }')
print(f'{T1.circumradi() = }')
```

Hem definit també igualtat de triangles; comprovem-ho amb un triangle traslladat

```sage
T11 = Triangle([1,0],[1,12],[17,12])
T11 == T1
```


Una de les propietats interessants de les classes en Python és
l'herència. Així, un pot definir una classe a partir d'una classe
prèviament definida. Per exemple, podem definir una nova classe dels
triangles rectangles donant només els dos catets (que situarem amb un
vèrtex a l'origen).


Definim triangle rectangle com una classe a partir de la classe dels triangles

```sage
class TriangleRectangle(Triangle):
    def __init__(self, catet1, catet2):
        Triangle.__init__(self,(0,0), (0,catet1), (catet2,catet1))
        self._catet1 = catet1
        self._catet2 = catet2
    def catets(self):
        return self._catet1, self._catet2
    def hipotenusa(self):
        c1, c2 = self.catets()
        return (c1**2 + c2**2)**.5
```

```sage
T2 = TriangleRectangle(12,16)
print(T2.vertexs)
```

```sage
print(T1.vertexs == T2.vertexs)
```

```sage
T1 == T2
```

```sage
print('Hipotenusa =', T2.hipotenusa())
```


```sage
print('Hipotenusa =', T1.hipotenusa())
```

Tot i que tenen els mateixos vèrtexs, i de fet són iguals (com a triangles), el triangle `T1` no té definida la hipotenusa, ja que no està definit com a triangle rectangle.



### Observacions

- Les variables `_p1`, `_p2` i `_p3` són variables
privades (no s'haurien de fer servir fora de la classe), tot i que a **Python** es considera que tothom és adult i sap què fa, així que no estan amagades del tot. Les podem fer "més privades" amb un doble guió baix (`__p1`, etc) i aleshores el seu nom real canvia a `__Triangle_p1`, fet que les fa més difícils d'utilitzar accidentalment.

- Per definició dos objectes són iguals si estan formats per les mateixes
dades. Si volem donar una altra definició d'igualtat dins d'una classe
(el que de fet vindria a ser matemàticament com definir una relació
d'equivalència en el conjunt de les classes donades), es pot fer
expressament definint `__eq__` (i `__ne__` per a diferent).

- Per exemple, podem voler considerar que dos triangles són el mateix
triangle si coincideixen al moure un a sobre de l'altre (i girar-lo o
fer el simètric si cal). Això és equivalent a definir la igualtat entre
triangles si tenen els mateixos costats (com a llista ordenada de menor
a major), que és el què hem implementat.



## Referències

Si voleu tenir més informació sobre programació amb Python podeu
consultar

[How to Think Like a Computer Scientist: Learning with Python
3](http://openbookproject.net/thinkcs/python/english3e/) de Peter
Wentworth, Jefrey Elkner, Allen B. Downey i Chris Meyers.

## Exercicis


### Exercici 1


Comproveu quina de les dues maneres
d'eliminar repeticions d'una llista de nombres a l'atzar és més ràpida. Una,
convertint la llista en conjunt i tornant-la a convertir en llista.
L'altra, fent una funció que fa una còpia de la llista, i creant una
nova llista escollint el mínim i traient tots els elements de la
còpia de la llista repetits, fins que la llista sigui buida.


-- begin hide


```sage
reset()
```

La primera funció:

```sage
def unic1(llist):
    cllist = copy(llist)
    nllist = []
    while len(cllist) > 0:
        a = min(cllist)
        nllist.append(a)
        while a in cllist:
            cllist.remove(a)
    return nllist
```

La segona:

```sage
def unic2(llist):
	return list(set(llist))
```

Creem una llista a l'atzar de 1000 nombres l'1 al 30


```sage
llist = [randint(1,30) for i in range(1000)]
```

Processem la llista amb la primera funció comptant el temps

```sage
%time V = unic1(llist)
```

El mateix amb el segon mètode

```sage
%time W = unic2(llist)
```

-- end hide

### Exercici 2

Genereu un fitxer que contingui tres columnes (separades amb tabulador), amb $n$, $n^2$ i $n^3$ per $n$ des de $1$ fins a $500$. Feu una funció prengui com a paràmetre el nom d'un fitxer, i comprovi que ha estat generat de la forma indicada anteriorment.

-- begin hide

Per generar l'arxiu podem fer-ho amb el següent bloc de codi:

```sage
with open('out.txt','w') as f:
    for n in [1,2..500]:
        f.write(f'{n}\t{n**2}\t{n**3}')
```

La funció següent comprova un fitxer, i si és incorrecte en retorna també el motiu.
```sage
def comprova(fname):
    with open(fname,'r') as f:
        for i, line in enumerate(f):
            V = line.split('\t')
            if len(V) != 3:
                return False, f"La línia {i} = \"{line}\" no té el nombre correcte d'entrades"
            if sage_eval(V[0]) != i+1:
                return False, f'A la línia {i} = "{line}" la primera entrada és incorrecta'
            if sage_eval(V[1]) != (i+1)**2:
                return False, f'A la línia {i} = "{line}" la segona entrada és incorrecta'
            if sage_eval(V[2]) != (i+1)**3:
                return False, f'A la línia {i} = "{line}" la tercera entrada és incorrecta'
    if i != 500:
        return False, f'El fitxer no té el nombre correcte de línies, en té {i} en comptes de 500'
    return True, None
```

-- end hide

### Exercici 3


Donats dos vectors $u=(u_1,\dots,u_n)$ i $v=(v_1,\dots,v_n)$ de
$\mathbb{R}^n$, diem que $u\le v$ en l'ordre lexicogràfic, si, o bé
són iguals, o bé $u_1 < v_1$, o bé existeix un $i\le n$ tal que
$u_j=v_j$ per a tot $j < i$ i $u_i < v_i$. Feu una funció `ordlex(u,v)`
que comprovi que $u$ i $v$ són vectors de la mateixa llargada, i si
no ho són doni un error `TypeError` i si ho són retorni `True` si
$u\le v$ en l'ordre lexicogràfic, i si no retorni `False`.


-- begin hide
Una possible manera. Ho he fet amb "llistes de nombres", no vectors

```sage
def ordlex(u,v):
    '''Retorna cert si u <= v en ordre lexicogràfic'''
    if type(u) != list or type(v)!=list:
        raise TypeError('No són llistes de nombres')
    if len(u) != len(v):
        raise TypeError('No tenen la mateixa llargada')
    # Amb una sola línia:
    # return next((ui < vi for ui, vi in zip(u,v) if ui != vi), True)
    # Amb for i ifs:
    for ui, vi in zip(u, v):
        if ui < vi:
            return True
        if ui > vi:
            return False
    return True
```

Observeu que es compleix el que es demana, ja que si $u[1] < v[1]$, llavors retorna True a la primera iteració, si $u[j]=v[j]$ per tot $j < i$ i $u[i] < v[i]$, llavors retorna True a la iteració número $j$, si fa totes les iteracions i surt del for és que $u=v$, i retorna True, i si no passa res d'això retorna False

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
-- end hide

### Exercici 4


Definiu una funció tal que, donades dues parelles de punts diferents
del pla $\mathbb{R}^2$, $\{p_1,p_2\}$ i $\{q_1,q_2\}$, determini si
el segment obert $r_1$ entre la primera parella talla o no el
segment obert $r_2$ entre la segona parella. La funció ha d'acceptar
com a dades dos conjunts formats per dos elements cadascun, i els
elements han de ser punts de $\mathbb{R}^2$ (com a llistes, o com a
tuples, etc). La resposta he de ser True si tallen, False si no.

Indicació: Per a fer-ho podeu utilitzar que els punts del segment
obert que uneix dos punts del pla $p_1$ i $p_2$ són els de la forma
$tp_1+(1-t)p_2$, on $0 < t < 1$. Per tant, si tenim ara una altre
parella de punts $p_3$ i $p_4$, volem comprovar si hi ha o no
$0 < s,t < 1$ de manera que $$tp_1+(1-t)p_2=sp_3+(1-s)p_4.$$ Utilitzant
la regla de Cramer això es tradueix a una desigualtat entre
determinants: el determinant $A$ de la matriu que té com a columnes
(o files) $p_1-p_2$ i $p_4-p_3$ ha de ser diferent de $0$ (per tal
que no siguin parał.els o coincidents), i, si denotem per $B$ el
determinant de la matriu que té com a columnes (o files) $p_4-p_2$ i
$p_4-p_3$ i per $C$ el mateix però amb columnes (o files) $p_1-p_2$
i $p_4-p_2$, llavors $$0 < \frac{B}{A} < 1 \text{ i } 0<\frac{C}{A} < 1$$
(doncs aquests quocients corresponen a la $t$ i la $s$ de la
equació).

-- begin hide

He fet una funció que comprova si les dades són conjunts, si tenen dos elements, si cada elements té llargada 2 i després he convertit els "punts" a vectors de $\mathbb{R^2}$. He calculat els determinants i comprovo si $A=0$, després si tenen el mateix signe tots (amb la funció `sign`), i si els quocients són $\ge 1$, i si es compleix alguna d'elles la resposta és `False`, i sino la resposta és `True`.


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
    elif A.sign() != B.sign() or A.sign() != C.sign():
        return False
    elif B/A >= 1 or C/A >= 1:
        return False
    t = B/A
    return True
```

A més he fet una funció Tallen que a més a més retorni el punt de tall (o bé `None` si no)

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
        return False, None
    elif A.sign()!=B.sign() or A.sign()!=C.sign():
        return False, None
    elif B/A >= 1 or C/A >= 1:
        return False, None
    t = B/A
    return True, t*V[0]+(1-t)*V[1]
```

Un exemple

```sage
S = {(2.1,2),(2.3,1)}
T = {(2.1,1),(2.3,2)}
b, pt = Tallen(S,T)
```

```sage
line(S) + line(T) + point(pt,color='red',size=30)
```

```sage
S = {(2.1,2),(2.3,2)}
T = {(2.1,1),(2.3,1)}
b, pt = Tallen(S,T)
b
```

```sage
line([v for v in S])+line([v for v in T])
```
-- end hide

### Exercici 5

Definiu una classe `Isosceles`, formada per triangles isòsceles donats
per la base i l'altura, definida a partir de la classe `Triangle`
definida a dalt.

-- begin hide
```sage
class TriangleIsosceles(Triangle):
    def __init__(self, base, altura):
        Triangle.__init__(self, (0, 0), (0, base), (altura, base/2))
        self.base = base
        self.altura = altura
    def area(self):
        return(self.base * self.altura)

T = TriangleIsosceles(10,10)
show(T.area())
```
-- end hide


### Exercici 6


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
