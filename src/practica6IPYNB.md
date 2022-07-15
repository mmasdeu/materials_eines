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

# Càlcul infinitessimal bàsic


### Transcripció dels exemples en el PDF de la pràctica 6


# Límits

```sage
var('k')
limit(k/(k+1),k=infinity)
```

```sage
limit(k*(sqrt(k+1)-sqrt(k-1))/sqrt(k),k=infinity)
```

```sage
limit(k.factorial()/k^k,k=oo)
```

```sage
limit((-1)^k*sqrt(k)*sin(k^k)/(k+1),k=oo)
```

```sage
limit((sqrt(k+1) - sqrt(k) + 1)^sqrt(k),k=oo)
```

```sage
limit((k^3-8*k+7)/(200*k+1024),k=-oo)
```

```sage
limit(e^k,k=oo)
```

```sage
limit(e^k,k=-oo)
```

Més límits

```sage
limit(sin(x)/x,x=0)
```

```sage
limit((x-sin(x))/x^3,x=0)
```

```sage
limit((1-cos(x))/x^2,x=0)
```

```sage
limit((x^3 + 3*x^2-x-3)/(x-1),x=1)
```

```sage
limit(e^(-1/x^2)/x^5,x=0)
```

```sage
limit(e^(1/x),x=0)
```

```sage

```

Restriccions en les variables

```sage
 limit(cos(2*pi*k), k=infinity)
```

```sage
var('k')
assume(k,'integer')
limit(cos(2*pi*k),k=oo)
```

```sage
assumptions()
```

Casos 

```sage
var('a')
f(x)= exp(a/x)
limit(f(x),x=0)
```

```sage
assume(a>0)
limit(f(x),x=0)
```

```sage
limit(f(x),x=0,dir='+')
```

```sage
limit(f(x),x=0,dir='-')
```

```sage
forget(a>0)
assume(a<0)
limit(f(x),x=0)
```

```sage
limit(f(x),x=0,dir='+')
```

```sage
limit(f(x),x=0,dir='-')
```

```sage
forget(a<0)
```

# Derivades


Hi ha dues maneres de fer derivades:

```sage
diff(x^3-2*x^2+4*x-5,x)
```

```sage
(x^3-2*x^2+4*x-5).diff(x)
```

Tot i que si només hi ha una variable no cal especificar la variable

```sage
diff(x^3-2*x^2+4*x-5)
```

```sage
(x^3-2*x^2+4*x-5).diff()
```

També es poden derivar funcions.

```sage
f(x)=tan(a*x^2)
df=f.diff()
show(df)
```

Derivades d'ordre superior

```sage
ddf=diff(df)
show(ddf)
```

```sage
show(diff(f(x),x))        # Derivada,
show(diff(f(x),x,x))      # Segona derivada,
show(diff(f(x),x,x,x))    # Tercera derivada...
```

```sage
show(diff(f(x),x,2))        # Segona Derivada,
show(diff(f(x),x,3))        # Tercera derivada,
show(diff(f(x),x,12))       # Dotzena derivada...
```

Derivades en dues o més variables

```sage
g(x,y)=y*exp(2*x^2+x-1)
diff(g(x,y),x)
```

```sage
diff(g(x,y),y)
```

```sage
diff(g(x,y),x,x)
```

```sage
diff(g(x,y),y,y)
```

```sage
diff(g(x,y),x,y)
```

```sage
diff(g(x,y),y,x)
```

# Estudi gràfic d'una funció


Volem estudiar la funció  $f(x)= \dfrac{x^{3} + 6 \, x^{2} + 12 \, x + 8}{x^{2} + 4 \, x + 3}$.

```sage
f(x)=(x^3+6*x^2+12*x+8)/(x^2+4*x+3)
```

Domini. Hem extret el polinomi del denominador usant "denominator"

```sage
slcns=solve(f(x).denominator()==0,x)
slcns
```

Com que ens interessa tenir una llista de arrels, cal extreure les solucions del que em obtingut.

```sage
disc=[x.subs(sol) for sol in slcns]
disc
```

Fixeu-vos que si tenim un polinomi de grau més gran haurem de demanar solucions numèriques, com a:

```sage
solve(x^7-2*x+1==0, x, to_poly_solve=True)
```

El problema és que ens surten arrels que no són reals (són nombres complexos). Per arreglar això li podem dir al Sagemath que la x és una variable real

```sage
assume(x,"real")
solve(x^7-2*x+1==0, x, to_poly_solve=True)
```

També li podem demanar les arrels als reals amb doble precisió (surt una llista de parelles (arrel, multiplicitat)).

```sage
(x^7-2*x+1).roots(ring=RDF)
```

Limits al infinit

```sage
show(limit(f(x),x=oo))
show(limit(f(x),x=-oo))
```

Assímptotes obliqües

```sage
m1=limit(f(x)/x,x=oo);show(m1)
m2=limit(f(x)/x,x=-oo);show(m2)
```

```sage
n1=limit(f(x)-m1*x,x=oo);show(n1)
n2=limit(f(x)-m2*x,x=-oo);show(n2)
```

Tenim la mateixa assímptota als dos costats. La definim. 

```sage
 asob(x)=m1*x+n1
```

Assímptotes verticals

```sage
for a in disc:
    show("Limit quan x->", a, " per la dreta:", limit(f(x),x=a,dir='+'))
    show("Limit quan x->", a, " per l'esquerra:", limit(f(x),x=a,dir='-'))
```

Zeros i signe

```sage
solve(f(x)==0,x)  # Zeros de f(x)
```

```sage
solve(f(x)>0,x)   # On la funció és positiva
```

```sage
solve(f(x)<0,x)   # On és negativa
```

Creixement i decreixement 

```sage
df=diff(f)
show(df)
show(df.simplify_full()) # Versió compacta de la derivada
```

```sage
solve(df(x)==0,x)  # Punts crítics de f(x)
```

```sage
solve(df(x)>0,x)   # On la funció creix
```

```sage
solve(df(x)<0,x)   # On decreix
```

Punts crítics

```sage
sptscrt=solve(df(x)==0,x)
pcrt=[x.subs(ss) for ss in sptscrt]
show(pcrt)
```

Punts critics i discontinuïtats

```sage
cridcr=sorted(disc+pcrt)
show(cridcr)
```

Concavitat i convexitat

```sage
ddf=diff(f,2)
show(ddf)
show(ddf.simplify_full())
```

```sage
solve(ddf(x)==0,x)
```

```sage
assume(x,'real')
solve(ddf(x)==0,x)
```

```sage
solve(ddf(x)>0,x)   # On la funció és convexa

```

```sage
solve(ddf(x)<0,x)   # On la funció és còncava
```

```sage
gf=plot(f,-8,5,ymin=-10,ymax=10,detect_poles='show')
ga=plot(asob,-8,5,color='grey')
gf+ga
```

# Integrals


Integrals definides

```sage
f(x)=x*sin(x)
var('a b')
integral(f(x),x,a,b)
```

```sage
integral(f(x),x,0,3)
```

```sage
integral(f(x),x,0,3).n()
```

```sage
integral(f(x),x,1.,2.)
```

```sage
integral(e^(-x^2),x,-infinity,infinity)
```

Visualitació de l'àrea

```sage
plot(f(x),(x,0,4))+plot(f(x),(x,1,2),fill='axis')

```

```sage
integral(f(x),x,1,2).n()

```

```sage
plot(sin(x)/x,(x,-50,50),fill='axis')
```

```sage
integral(sin(x)/x,x,-50,50).n()
```

Integrals indefinides o primitives

```sage
integral(f(x),x)

```

```sage
h(x)=integral(f(x),x)
diff(h(x),x)
```

Càcul amb paràmetres

```sage
var('m')
integral(x^m,x)
```

```sage
assume(m!=-1)  
integral(x^m,x)
```

```sage
integral(x^(-1),x)
```
