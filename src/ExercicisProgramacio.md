---
jupyter:
  title: 'Exercicis de programació en Python'
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
    display_name: SageMath 10.02
    language: sage
    name: sagemath

---

## Exercicis curts en Sagemath

Els següents exercicis els hauríeu de saber fet tots d'alguna manera o altra. Recomanem que els treballeu fins que us surtin, i només després mireu si voleu les solucions per tal de veure solucions potser més concises o el·laborades. 


### Exercici 1


Feu una funció Mou que admeti com a input una llista i retorni la llista que s'obté de l'original posant el primer element de la llista al final. Per exemple, Mou([1,2]) retorna [2,1], Mou([1,2,3]) retorna [2,3,1], Mou([1]) retorna 1 i Mou([]) retorna [].


-- begin hide

Una solució molt curta:

```sage
def Mou(L):
    return L[1:]+L[:1]
```

-- end hide

### Exercici 2

Feu una funció Quad que admeti un nombre natural i retorni el nombre de maneres que es pot expressar el natural com a suma de dos quadrats. Per exemple Quad(0) retorna 1 doncs només es pot fer com 0=0^2+0^2, i Quad(1) retorna 2, doncs 1=1^2+0^2=0^2+1^2, Quad(2) retorna 1, Quad(3) retorna 0, i Quad(325) retorna 6.  Si voleu podeu utilitzar el mètode is_square() que aplicat a un enter de Sage retorna True o False depenen de si el nombre és o no un quadrat. 

-- begin hide

Una funció minimal amb el mètode is_square()

```sage
def Quad(n): 
    return len([i for i in srange(n+1) if (n-i^2).is_square()])

```


```sage

```

-- end hide

### Exercici 3

Trobeu tres nombres naturals a, b i c tals que 201 = a^6 + b^4 + c^2.

-- begin hide

Usant la funció is_square() i comprehensió de llistes. Fixeu-vos que $3^6>201$ i que $4^4>201$, per això hem posat els límits que hi ha en els srange.  

```sage

[(a,b,sqrt(201-a^6-b^4)) for a in srange(4) for b in srange(5) if (201-a^6-b^4).is_square()][0]

```
També es pot fer amb for i un break (aquest cop, sense el mètode is_square()). 

```sage

for a in range(4):
    for b in range(5):
        if a**6+b**4 <= 201: 
            for c in range(16):
                if a**6 + b**4 + c**2 == 201:
                    print((a,b,c))
                    break

```


-- end hide

### Exercici 4

Feu la funció NoRepetits que accepti un llista com a entrada i retorni el conjunt d'elements que surten a L només un cop. Per exemple, NoRepetits([1,1,2]) retorna {2}, NoRepetits([1,1]) retorna {} i NoRepetits([1,2,1,3]) retorna {2,3}. 

-- begin hide

Una manera controlant l'index de l'element, i creant la llista obtinguda treient l'element d'aquell index de la llista. 

```sage

def NoRepetits(L): 
    return {L[i] for i in range(len(L)) if not(L[i] in L[:i]+L[i+1:])}
```

Una altra manera, creant la llista formada pels elements iguals a un de donat. 

```sage

def NoRepetits(L): 
    return {a for a in set(L) if len([b for b in L if a==b])==1} 

```

-- end hide

### Exercici 5

Defineix una funció EsOrdenada que admeti una llista formada per nombres i retorni un boobleà cert o fals depenen de si la llista és ordenada o no. Per exemple, EsOrdenada([1,2,3]) ha de retornar True, EsOrdenada([3,2,1]) ha de retornar False, EsOrdenada([1]) ha de retornar True i EsOrdenada([]) ha de retornar True.

-- begin hide

El primer que cal observar és que per tal que una llista $L$ estigui ordenada en tenim prou en veure que $L[i]\le L[i+1]$ per a tot $i$ (i no cal veure que $L[i]\le L[j]$ per a tot $i<j$. 

Primer fem una versió usant la funció all, que a una llista/tupla/conjunt de booleans, retorna True si i només si tots són True. 

```sage

def EsOrdenada(L): 
    return all(L[i]<=L[i+1] for i in range(len(L)-1))
```

Primer fem una versió usant la funció all, que a una llista/tupla/conjunt de booleans, retorna True si i només si tots són True. 

```sage

def EsOrdenada(L): 
    return all(L[i]<=L[i+1] for i in range(len(L)-1))
```

A la següent versió usem un mètode de les llistes, pop(i), que si poses un index i d'una llista L retorna l'element L[i] i esborra l'element i-èssim de la llista. Si es posa L.pop(), retorna i esborra de L l'últim element de la llista L. Enlloc de comparar des de l'inici, el que fem es comparar des del final. Observeu que si la llista té 0 o un elements, ja està ordenada. 

```sage

def EsOrdenada(L): 
    while len(L)>1 :
        a = L.pop()
        if a < L[-1]:
            return False
    return True

```
Encara una versió, aquest cop recursiva, i sense usar el mètode pop(). 

```sage

def EsOrdenada(L): 
    if len(L) < 2 :
        return True
    if L[0] > L[1]:
            return False
    return EsOrdenada(L[1:])

```



-- end hide

### Exercici 6

Fes una funció Pal que retorni un booleà dient cert si el nombre és palindròmic en base 10. Un nombre és palindròmic en base b si escric en base b és el mateix nombre llegit d'esquerra a dreta que de dreta a esquerra. Per exemple, 1234321 és palindròmic, i també 12121, però 1321 no ho és. 

-- begin hide

Una manera molt curta, convertint el nombre a cadena (string), i girant la cadena usant comprehensió. 

```sage
def Pal(n): 
    return str(n)[::-1]==str(n)
```

Una altra manera, usant el mètode digits() que a un nombre natural retorna la llista dels seus digits en base 10 (posats de menor a major). El mètode digits(b), retorna la llista dels digits en base b. 

```sage
def Pal(n): 
    d = n.digits()
    return d[::-1]==d
```
 
  Si sabéssim com extreure el primer i l'últim digit, podríem fer una funció recursiva. Això ho fem calculant $m=\log(n,10).n().\floor()$, que ens dona la màxima potència de 10 menor que n (floor() és la part entera per sota), i després anant traient $10^m$ el màxim nombre de vegades que podem, i comptant quants cops ho fem. Òbviament ho podríem haver fet amb str i amb digits. També fem servir que si el nombre és menor que 10, té un sol digit i per tant és palindròmic. 


```sage
def Pal(n): 
    if n<10:
        return True
    m = log(n,10).n().floor()
    for i in range(9):
        n -= 10 ** m
        if n < 10 ** m:
            break
    d = i + 1
    if d != n % 10:
        return False
    n = (n - d) // 10 
    return Pal(n)

```

-- end hide

