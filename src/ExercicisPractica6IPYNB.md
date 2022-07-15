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

# Exercicis Pràctica 6


Exercici 1:  Calculeu
$$
\lim_{k\to \infty} \left( \frac{\ln(k+1)}{\ln(k)}\right)^{(k\, \ln(k))}
$$

```sage
var('k')
lim((ln(k+1)/ln(k))(k*ln(k)),k=infinity)
```

Exercici 2: Calculeu els límits de la forma:

(a) $\lim\limits_{x\to 3} \exp\left(\dfrac{a\, x}{x-3}\right)$

(b) $\lim\limits_{x\to 2} \dfrac{x-a}{|x-b|}$

(c) $\lim\limits_{x\to 0} \exp\left( \dfrac{a+\exp(-1/x^{2})}{x^{2}} \right)$

Tenint en compte com varia el resultat segons el valor dels paràmetres $a$, $b$ i com són els límits laterals en cada cas.


(a) $\lim\limits_{x\to 3} \exp\left(\dfrac{a\, x}{x-3}\right)$

```sage
forget()
var('a x')
assume(a>0)
show('limit quan a>0 per la dreta =\t ',lim(exp((a*x)/(x-3)),x=3,dir='+'))
```

```sage
show("limit quan a>0 per l'esquerra =\t ",lim(exp((a*x)/(x-3)),x=3,dir='-'))
```

```sage
forget(a>0)
assume(a<0)
show('limit quan a<0 per la dreta =\t ', lim(exp((a*x)/(x-3)),x=3,dir='+'))
```

```sage
show("limit quan a<0 per l'esquerra =\t ",lim(exp((a*x)/(x-3)),x=3,dir='-'))
```

(b) $\lim\limits_{x\to 2} \dfrac{x-a}{|x-b|}$

```sage
forget()
var('a b x')
show('limit quan b no és 2 =\t ',lim((x-a)/(abs(x-b)),x=2))
```

```sage
assume(a>2)
show('limit quan a> 2 i b és 2 =\t ',lim((x-a)/(abs(x-2)),x=2))
```

```sage
forget()
assume(a<2)
show('limit quan a<2 i b és 2 = \t ',lim((x-a)/(abs(x-2)),x=2))
```

(c) $\lim\limits_{x\to 0} \exp\left( \dfrac{a+\exp(-1/x^{2})}{x^{2}} \right)$

```sage
forget()
assume(a>0)
show('limit quan a> 0  = \t',lim(exp((a+exp(-1/x^2))/x^2),x=0))
```

```sage
forget()
assume(a<0)
show('limit quan a<0  = \t ',lim(exp((a+exp(-1/x^2))/x^2),x=0))
```

```sage
forget()
show('limit quan a és 0  = \t',lim(exp((exp(-1/x^2))/x^2),x=0))
```

Exercici 3:  Considereu la funció determinada per $$h(x)=\dfrac{20}{x^2+4}.$$Calculeu la primera i segona derivades $h'(x)$ i $h''(x)$ i representeu conjuntament les tres funcions.

Observareu que, el punt on el pendent a la gràfica de $h(x)$ és màxim, és en efecte un màxim de $h'(x)$ i a més s'anul·la $h''(x)$. Determineu aquest punt. Sabeu com serà $h'''(x)$ en aquest punt? 

```sage
reset()
var('x')
h(x)=20/(x^2+4)
```

```sage
dh=h.diff()
ddh=dh.diff()
show(dh)
show(ddh)
```

```sage
gh=plot(h,-5,5,ymin=-5,ymax=6,  legend_label='Funció f')
gdh=plot(dh,-5,5,ymin=-5,ymax=6,color='red',legend_label='Derivada de f')
gddh=plot(ddh,-5,5,ymin=-5,ymax=6,color='green', legend_label='Segona derivada de f')
gh+gdh+gddh
```

```sage
S=solve(ddh(x)==0,x)
S=[x.subs(s) for s in S]
show("Punts on s'anul·la la segona derivada \t ", S)
```

```sage
dddh=ddh.diff()
pdddh = plot(dddh,-5,5,ymin=-5,ymax=6)
ptdddh = point([[s,dddh(s)] for s in S],color='red',title='Grafica de la tercera derivada amb els dos punts',size=25)
pdddh+ptdddh
```

```sage
ddddh=dddh.diff()
show('Valor de la tercera derivada en aquests dos punts = \t', {ddddh(s) for s in S}) 
```

Exercici 4:  Definiu la funció h de dues variables corresponent a 
$$
h(x,y)= \frac{x\, \mathrm{e}^{x\,y}\, \sin(y^{2})}{\ln(x^{2}+y^{2}+2)}
$$
i determineu

```sage
h(x,y)= (x*exp(x*y)*sin(y^2))/(ln(x^2+y^2+2))
```

```sage
show(h)
```

La funció corresponent a la derivada de $h$ respecte la variable $x$.

```sage
show(diff(h,x))
```

La funció corresponent a la derivada de $h$ respecte la segona variable ($y$).

```sage
show(diff(h,y))
```

Els valor de la derivada de $h$ respecte $x$ per a $x=1$, $y=1/2$.

```sage
diff(h,x)(1,1/2)
```

```sage
diff(h,x)(1.,1./2)
```

La funció que s'obté derivant $h$ respecte $x$ dues vegades.

```sage
show(diff(h,x,x).simplify())
```

Exercici 5:  Creeu una funció de 'SageMath' anomenada tangent, que accepti com a paràmetres una funció $f(x)$, un punt $a$ i una distància $h>0$, i doni com a resultat el gràfic de la funció $f(x)$ junt amb la seva recta tangent en el punt $(a,f(a))$ en l'interval $[a-h,a+h]$.  


```sage
def tangent(f,a,h):
    f=f.function(x)
    dfa=diff(f(x),x).subs(x==a)
    rtan(x)=dfa*(x-a)+f.subs(x==a)
    gt=plot(rtan,a-h,a+h,color='red')
    gf=plot(f,a-h,a+h)
    return gt+gf
```

```sage
tangent(x^3,3,1)
```

```sage
f(x)=exp(x^(-2))
tangent(f,5,.5)
```

Exercici 6:  Milloreu la informació que mostra el gràfic de la funció 
$$f(x)= \dfrac{x^{3} + 6 \, x^{2} + 12 \, x + 8}{x^{2} + 4 \, x + 3}$$
que surt al text marcant els punts on hi ha extrems relatius i fent que les regions de creixement i decreixement tenguin colors diferents.

```sage
f(x)=(x^3+6*x^2+12*x+8)/(x^2+4*x+3)
slcns=solve(f(x).denominator()==0,x)
disc=[x.subs(sol) for sol in slcns]
m1=limit(f(x)/x,x=oo)
n1=limit(f(x)-m1*x,x=oo)
asob(x)=m1*x+n1
df=diff(f)
sptscrt=solve(df(x)==0,x)
pcrt=[x.subs(ss) for ss in sptscrt]
cridcr=sorted(disc+pcrt)
creixement=solve(df(x)>0,x)
decreixement=solve(df(x)<0,x)
```

```sage
show('els punts crítics (on pot canviar la derivada) són \t', cridcr)
```

```sage
show('llocs on la fucnió creix  \t', creixement)
show('llocs on la fucnió decreix \t', decreixement)
```

```sage
show('Observem que la funció decreix del primer punt crític al últim punt crític, que són els extrems relatius')
extrems = [cridcr[0],cridcr[-1]]
show('Els extrems relatius són  \t  ', set(extrems))
```

```sage
gfce=plot(f,-8,cridcr[0],ymin=-10,ymax=10,detect_poles='show',color='green')
gfcd=plot(f,cridcr[-1],5,ymin=-10,ymax=10,detect_poles='show',color='green')
gfd=plot(f,cridcr[0],cridcr[-1],ymin=-10,ymax=10,detect_poles='show',color='blue')
ga=plot(asob,-8,5,color='grey')
pcridcr=point([(s,f(s)) for s in extrems],xmin=-8, xmax=5,color='red', marker="s", size=25)
gfce+gfcd+gfd+ga+pcridcr
```

Exercici 7:  Realitzeu l'estudi complet de la representació gràfica de la funció 
$$
g(x)= \frac{x^3-2}{x^2-3\,x+1}
$$

```sage
reset()
assume(x,'real')
g(x)=(x^3-2)/(x^2-3*x+1)
```

```sage
slcns=solve(g(x).denominator()==0,x)
disc=[x.subs(sol) for sol in slcns]
show(disc)
```

```sage
show(limit(g(x),x=oo))
m1=limit(g(x)/x,x=oo)
show(m1)
```

```sage
show(limit(g(x),x=-oo))
m2=limit(g(x)/x,x=-oo)
show(m2)
```

```sage
n1=limit(g(x)-m1*x,x=oo)
n2=limit(g(x)-m1*x,x=-oo)
show(n1)
show(n2)
asob(x)=m1*x+n1
```

```sage
dg=diff(g)
sptscrt=solve(dg(x)==0,x)
pcrt=[x.subs(ss) for ss in sptscrt]
show(pcrt)
cridcr=sorted(disc+pcrt)
cridcr=[c.n() for c in cridcr]
show(cridcr)
```

```sage
show([c.n() for c in cridcr])
```

```sage
solve(dg(x)>0,x) # On la funció creix
```

```sage
solve(dg(x)<0,x) # On la funció decreix
```

```sage
ddg=diff(g,2)
show(ddg)
show(ddg.simplify_full())
```

```sage
solve(ddg(x)==0,x)
```

```sage
solve(ddg(x)>0,x) # On la funció és convexa
```

```sage
solve(ddg(x)<0,x) # On la funció és còncava
```

```sage
gce=plot(g,-3,cridcr[0],ymin=-20,ymax=30,detect_poles='show',color='green')
gcd=plot(g,cridcr[-1],8,ymin=-20,ymax=30,detect_poles='show',color='green')
gd=plot(g,cridcr[0],cridcr[-1],ymin=-20,ymax=30,detect_poles='show',color='blue')
ga=plot(asob,-3,8,color='grey')
pcridcr=point([(s,g(s)) for s in pcrt], xmin=-3, xmax=8,color='red', marker="s", size=25)
gce+gcd+gd+ga+pcridcr
```

Exercici 8: Representeu l'àrea de la regió del pla que queda entre els gràfics de $y=\sin(x)$ i $y=\cos(x)$ corresponent als $x$ entre $0$ i $2\, \pi$ en els que $\cos(x)\le\sin(x)$ dibuixant els gràfics d'aquestes dues funcions en $[0,2\, \pi]$ i ombrejant la regió. (Cal que investigueu una mica en la documentació de \texttt{plot} per aconseguir un bon resultat).


```sage
s(x)=sin(x)
c(x)=cos(x)
```

Primer faig la grafica per a veure aproximadament on es tallen.

```sage
plot([s,c],0,2*pi)
```

Calculo les x's on es tallen les gràfiques amb find_root (numèricament).

```sage
r1=find_root(s(x)-c(x),0,1)
r2=find_root(s(x)-c(x),3,5)
```

Afegeixo al plot anterior el ombrejat entre s i c entre els punts r1 i r2.

```sage
plot([s,c],0,2*pi)+ plot(s,r1,r2, fill=c)
```

Determineu el valor de l'àrea d'aquesta regió calculant la integral corresponent.

```sage
integral(s(x)-c(x),x,r1,r2)
```

```sage

```
