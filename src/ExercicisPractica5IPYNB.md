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

# Exercicis pràctica 5 


## Exercicis a dins de la pràctica


Si volem fer una llista L dels primers menors que 20 en ordre invers (de gran a petit)

```sage
L=[]
a=20
while a>0:
    if a.is_prime():
        L.append(a)
    a-=1
print(L)
```

```sage
L = []
for p in srange(20,1,-1):
    if p.is_prime():
        L.append(p)
L
```

```sage
[p for p in srange(20,1,-1) if p.is_prime()]
```

 Proveu de fer el mateix per a trobar el nombre primer immediatement superior a $a$.


Opció 1: amb un while. Aquí cal saber que hi ha infinits primers, i per tant el while s'acaba algun dia.

```sage
a = 1009
p = a+1
while not p.is_prime():
    p += 1
print(p)
```

Opció 2: amb un for; el problema és saber fins a on hem d'anar per a torbar un nombre primer. Aquí podem fer servir per exemple un resultat que diu que per a tot $n>1$, hi ha sempre un nombre primer $p>n$ amb $p\le 2n$ (es diu postulat de Bertrand, i podeu buscar informació per internet).  

```sage
for p in srange(a+1,2*a+1):
    if p.is_prime():
        break
print(p)
```

La funció/atribut corresponent del SageMath és next_prime()

```sage
a.next_prime()
```

```sage
next_prime(a)
```

## Exercici 1


Una solució amb el for

```sage
s=0
for p in [3..100]:
    if p.is_prime() and (p-1)%4==0:
            s += p
print(s)
```

Una solució amb el while, on he usat que és el mateix dir que al restar 1 sigui múltiple de 4 que al dividir per 4 doni reste 1

```sage
s=0
p=2
while(p<101):
    if p%4==1:
        s+=p
    p=p.next_prime() 
print(s)
```

Una solució més compacte amb comprensió de llistes 

```sage
sum(p for p in srange(100) if p.is_prime() and p%4==1)
```

## Exercici 2

```sage
def Fib(k):
    '''Calcula la llista dels nombres de Fibonacci fins el k-èssim'''
    if type(k) != Integer or k<0:
        print("No és un valor admissible")
        return                # Si no és un enter >=0, fem que no retorni res
    if k == 0:
        return [1]            #Per a k=0 retorna només la llista amb el 1. 
    F=[1,1]                   #Inicialitzem la llista amb els dos primers valors
    for i in range(k-1):      #El bucle va fins a k-1 ja que el 0 i el 1 ja estan
        f=F[i]+F[i+1]
        F.append(f)
    return(F)
```

```sage
Fib(2)
```

```sage
Fib(15)
```

```sage
Fib(0)
```

```sage
Fib(-3)
```

Fem una funció que retorna la grafica dels quocients. 

```sage
def llistaFib(N):
    F=Fib(N)
    llista=[(k,F[k]/F[k-1]) for k in [1..N]]
    return points(llista)
```

```sage
llistaFib(20)
```

## Exercici 3


Farem una llista anomenada Portes de manera que tindra un 0 en el lloc $i$ si la porta $i+1$ està tancada, i un 1 si està oberta. Recordem que les llistes comencen amb el 0, però nosaltres volem enumerar les portes del 1 al 100.

```sage
Portes=[0 for a in range(100)]
```

Farem un bucle amb el for per a fer les 100 passades. Si el iterador es diu $i$, que es mourà de $0$ a $99$, recordem que haurem de fer els passos de llargada $i+1$. Després anomenem $r$ la porta en que començarem, i anem canviant el valor de la porta $r+j*(i+1)$ fins que surti més gran que 100. Fixeu-vos que si $a$ és 0 o 1, aleshores $1-a$ té el valor oposat. 

```sage
for i in range(100):
    r=i                        #La primera porta en el primer pas en la iteració i està en el lloc i de la llista
    while(r<100):
        Portes[r]=1-Portes[r]  #Obrim o tanquem la porta en el lloc r
        r+=i+1                 #Saltem anem a buscar la porta que està i+1 llocs de distància
print(Portes)
```

Observeu que les úniques portes que queden obertes són les que corresponen als nombres quadrats.

```sage
[i+1 for i in range(100) if Portes[i] == 1]
```

Fixeu-vos que el nombre de vegades que passem per una porta és igual al nombre de divisors que té (incloent el 1 i ell mateix). 

```sage
[number_of_divisors(i+1).mod(2) for i in range(100)] == Portes
```

He fet una nova versió utilitzant dos bucles for: el primer controla les passades, el segon el procés de tancar i obrir portes. Noteu que 

```sage
PortesN=[0 for a in range(100)]
for i in [1..100]:
    for r in [i..100,step=i]:        #la r es mou des del lloc i al lloc 100 de i en i.
        PortesN[r-1]=1-PortesN[r-1]  #Obrim o tanquem la porta r-èssima, que es troba en el lloc r-1 de la llista
print(PortesN)
print("Comprovem que surt la mateixa llista : ",PortesN==Portes)
```

## Exercici 4


Com que volem fer un pas aleatori, escollim un nombre aleatori entre 1 i 4, i després assinem el pas corresponent al nombre. El que hem fet és definir una variable que sigui un boobleà (bo), i que passi a ser False quan retornem al (0,0) o bé haguem fet el nombre de passos que hem dit previament (=1000).

```sage
pt=[0,0]                   #Punt inicial. Es una llista doncs les tuples no es poden modificar un de les coordenades.
llista=[pt]                #Llista de punts per on passarem
fi=1000                    #Nombre màxim de passos a fer.
bo=True                    #Boobleà que val True mentre no tornem a (0,0) o fem fi passos
while(bo):                 
    pas=randint(1,4)
    if pas ==1: 
        pt[0]=pt[0]+1
    elif pas==2:
        pt[1]=pt[1]+1
    elif pas==3:
        pt[0]=pt[0]-1
    else:
        pt[1]=pt[1]-1
    llista.append(copy(pt))                    #Afegim una copia del punt, per assegurar que no canviarà al canviar el punt
    bo=(pt!=[0,0]) and (len(llista)< fi)       #El booleà valdrà False si pt=[0,0] o hem fet fi o més passos. 
print(len(llista))
```

Penseu: perquè hem fet així i no hem posat directament 

while ((pt!=[0,0]) and (len(llista)< fi) ): ?


Proveu de fer el mateix amb un for i un break

```sage
line(llista)
```

Poso una altra manera per veure si ho veieu més clar


Podriem fer el mateix creant una llista de passos i fent un primer pas abans. 

```sage
pt=[0,0]                   #Punt inicial. Es una llista doncs les tuples no es poden modificar un de les coordenades.
llista=[pt]                #Llista de punts per on passarem
fi=1000                    #Nombre màxim de passos a fer.
passos=[[1,0],[0,1],[-1,0],[0,-1]]
pas=randint(0,3)           #Fem un primer pas fora del while per poder posar el boobleà al while
pt = [pt[i]+passos[pas][i] for i in range(2)]
llista.append(copy(pt))                    
while((pt!=[0,0]) and (len(llista)< fi)):                 
    pas=randint(0,3)
    pt = [pt[i]+passos[pas][i] for i in range(2)]
    llista.append(copy(pt))                    #Afegim una copia del punt, per assegurar que no canviarà al canviar el punt
print(len(llista))
```

## Exercici 5


(a) Feu un programa verh() que tingui tres arguments (per tal d'executar la funció es posarà verh(alpha, x0, np)): el paràmetre alpha corresponent al $\alpha\in[0,4]$ de la funció $f(x)=\alpha\,(1-x)\,x$, x0 que serà un punt inicial de $[0,1]$ i np el nombre total de passos; i que el resultat sigui el diagrama de teranyina corresponent.


He fet la funció amb un "docstring" (o sigui, una breu descripció del que fa al principi), i també que vagi comprovant que els valors donats compleixin el que es demana, i en cas contrari no retorni res. 

```sage
def verh(alpha,x0,np):
    '''Donats un valor alpha de [0,4], un punt inicial de [0,1] i el nombre iteraccions 
    fa la grafica demanada en el Exercici 5'''
    if alpha<0 or alpha>4:
        print("El primer valor "+ str(alpha) + " està fora del interval [0,4]")
        return
    if x0<0 or x0>1:
        print("El segon valor  "+ str(x0) + "  està fora del interval [0,1]")
        return     
    if type(np) != Integer and np<1:
        print("El tercer valor  "+ str(np) + "  no és un enter >0")
        return
    f(x)=alpha*(1-x)*x                             # Definició de la funció f(x)
    llista=[[x0,0],[x0,x0]]                        # La llista de punts comença així segons les condicions 1. i 2. 
    for i in range(np):                            # Fem un for que es repetirà np cops 
        x1=f(x0)                                   # Calculem x1=f(x0)
        llista.append([x0,x1])                     # Afegim [x0,x1] 
        llista.append([x1,x1])                     # Afegim [x1,x1] 
        x0=x1                                      # El nou x0 serà el anterior x1
    funcio=plot(f(x), 0, 1)                        # Plot de la funció
    recta=plot(x,0,1)                              # Plot de la recta y=x
    return(funcio+recta+line(llista,color='red'))  # Retornem la suma dels plots incloent el creat de la llista 
```

(b) Començant pel valor $x_0=0.1$, estudieu el resultat del procés anterior en els casos de 
$\alpha=2.3$, $2.7$, $3.2$, $3.5$ i $3.8$.

```sage
verh(2.3,0.1,10)
```

```sage
verh(2.3,0.2,10)
```

```sage
verh(2.3,0.3,10)
```

```sage
verh(2.3,0.4,10)
```

```sage
verh(2.3,0.7,10)
```

```sage
verh(2.3,0.9,10)
```

Amb els altres valors de alpha només he posat que surt per x0=0.1

```sage
verh(2.7,0.1,20)
```

```sage
verh(3.2,0.1,20)
```

```sage
verh(3.5,0.1,20)
```

```sage
verh(3.8,0.1,20)
```

Representeu també, en un gràfic diferent,els valors successius de $x_k$ (fent una línia que uneixi els punts $(k, x_k)$).


He fet una nova funció que només va calculant els $x_k$ i crea una llista amb $(k,x_k)$

```sage
def verh2(alpha,x0,np):
    '''Donats un valor alpha de [0,4], un punt inicial de [0,1] i el nombre iteraccions 
    fa la grafica demanada en el Exercici 5'''
    if alpha<0 or alpha>4:
        print("El primer valor "+ str(alpha) + " està fora del interval [0,4]")
        return
    if x0<0 or x0>1:
        print("El segon valor  "+ str(x0) + "  està fora del interval [0,1]")
        return     
    if type(np) != Integer and np<1:
        print("El tercer valor  "+ str(np) + "  no és un enter >0")
        return
    f(x)=alpha*(1-x)*x
    llista=[[0,x0]]
    for i in [1..np]:
        x0=f(x0)
        llista.append([i,x0])                  
    return(line(llista,color='red'))
```

```sage
verh2(3.8,0.1,20)
```

(c) Introduïu un paràmetre a la funció que determini si  la sortida és el diagrama de teranyina, o la gràfica d'evolució dels resultats $(k,x_k)$.


He posat un paramètre booleà que es diu control que és True (si volem que surti un diagrama teranyina) o False (si volem que surti només la gràfica d'evolució. He creat la llista que ja feia per la funció verh, i amb aquesta llista , si control és True, o bé retorno el plot del diagrama o, si control és False 

```sage
def verht(alpha,x0,np,control):
    '''Donats un valor alpha de [0,4], un punt inicial de [0,1] i el nombre iteraccions 
    fa un diagrama teranyina o bé la gràfica d evolució depenen de si control és True or no'''
    if alpha<0 or alpha>4:
        print("El primer valor "+ str(alpha) + " està fora del interval [0,4]")
        return
    if x0<0 or x0>1:
        print("El segon valor  "+ str(x0) + "  està fora del interval [0,1]")
        return     
    if type(np) != Integer and np<1:
        print("El tercer valor  "+ str(np) + "  no és un enter >0")
        return
    f(x)=alpha*(1-x)*x
    llista=[[x0,0],[x0,x0]]
    for i in range(np):
        x1=f(x0)
        llista.append([x0,x1])
        llista.append([x1,x1])
        x0=x1
    if control:
        funcio=plot(f(x), 0, 1)
        recta=plot(x,0,1)
        return(funcio+recta+line(llista,color='red'))
    llista2 = [[i,llista[2*i+1][0]] for i in range(np)]  #Utilitzo que la llista té els valors [xk,xk] al lloc 2*k+1
    return(line(llista2))
```

```sage
verht(3.8,0.1,20,False)
```

```sage
verht(3.8,0.1,20,True)
```

```sage

```

(d) Elimineu el paràmetre $np$ i feu que el procés s'aturi en estabilitzar-se, amb un
error menor a $10^{-3}$, en un punt fix o un cicle de longitud 2 o 4, mostrant-lo per
pantalla.


La dificultat principal està en com mirar si es compleixen les condicions: per mirar si s'estabilitza sols cal veure si dos valors consecutius són iguals (de fet, si la seva resta en valor absolut és menor de $10^{-3}$). el problema és com veure si hi ha un cicle de longitud 2 o 4; el que faig és utilitzar que tenim els valors anteriors guardats a la llista: sols cal mirar enrere de 2 en 2 a la llista. Tot això ho faig en una condició en el while, que és el punt clau del programa. 


Utilitzo una funció molt útil, la funció all, que a una llista de booleans retorna True si i només si tots els valors són True. N'hi ha una altra que és la funció any, que retorna True si algun valor és True. Exemple:


```sage
L=[True,True,False]
print(all(L))
print(any(L))
```

També utlitzo que si posem llista[-n] per un valor de n, ens retorna el enèssim valor començant per l'últim (per tant, llista[-1] és l'últim valor de la llista) 


Finalment, noteu que no podem demanar el valor -9 d'un llista que no tingui com a mínim 9 elements, per això hi ha un if en la creació de la llista de booleans, per assegurar que la llista és prou llarga.

```sage
def verhs(alpha,x0):
    '''Donats un valor alpha de [0,4], un punt inicial de [0,1] 
    fa la grafica demanada en el Exercici 5 (d)'''
    if alpha<0 or alpha>4:
        print("El primer valor "+ str(alpha) + " està fora del interval [0,4]")
        return
    if x0<0 or x0>1:
        print("El segon valor  "+ str(x0) + "  està fora del interval [0,1]")
        return     
    f(x)=alpha*(1-x)*x
    llista=[[x0,0],[x0,x0]]
    x1=f(x0)
    llista.append([x0,x1])
    llista.append([x1,x1])
    while(all([abs(llista[-1][0]-llista[-2*i-1][0])>10^(-3) for i in [1,2,4] if len(llista)>2*i+1])):
        x0,x1=x1,f(x1)
        llista.append([x0,x1])
        llista.append([x1,x1])
    funcio=plot(f(x), 0, 1)
    recta=plot(x,0,1)
    return(funcio+recta+line(llista,color='red'))
```

Podeu experimentar amb la funció si us bé de gust. 

```sage
verhs(3.07,0.4)
```

```sage
verhs(3.9,0.7)
```

```sage

```
