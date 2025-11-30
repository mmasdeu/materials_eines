---
jupyter:
  title: 'Pràctica 6: El model Parent / Element de Sage'
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

# Problemes de la solució anterior

En aquesta pràctica millorarem la classe `Adic` que hem implementat anteriorment, per tal
que la interacció amb les altres parts de *SageMath* sigui més fluïda. Comencem per
recordar com tenim definida aquesta classe (oblidarem el cas particular dels diàdics, i
ens centrarem a implementar els àdics més generals).

```sage
class Adic:
    def __init__(self, p, a, n = None):
        p = ZZ(p)
        if not p.is_prime():
            raise ValueError('p ha de ser primer')
        self.p = p
        if n is None:
            x = QQ(a)
            a, b = x.numerator(), x.denominator()
            n = -b.valuation(p) # Estem suposant que el denominador és potència de p
            if b != p**-n:
                raise ValueError(f'{x} no és un àdic, perquè té denominador {b} que no és potència de {p}')
            if n == 0: # Resulta que tenim un enter
                n = a.valuation(p)
                a /= p**n        
        else:
            a, n = ZZ(a), ZZ(n)
            if a % p == 0:
                raise ValueError(f'{a = } no pot ser divisible per {p = }')
        self.mantissa = ZZ(a)
        self.exp = ZZ(n)

    def __repr__(self):
        return f'{self.mantissa} · {self.p}^{self.exp}'

    def racional(self):
        return QQ(self.mantissa * self.p**self.exp)
    
    def __add__(self, other):
        if self.p != other.p:
            raise ValueError('No es poden sumar àdics de diferents p')
        return Adic(self.p, self.racional() + other.racional()) # Penseu una implementació millor
    
    def __mul__(self, other):
        if self.p != other.p:
            raise ValueError('No es poden multiplicar àdics de diferents p')
        return Adic(self.p, self.mantissa * other.mantissa, self.exp + other.exp)
```

Recordem que aquesta definició ens permet fer certes operacions de manera prou còmode:

```sage
a = Adic(3, 2 / 3)
b = Adic(3, -1/9)
print(f'{a = }, {b = }, {a + b = }, {a * b = }')
```

En canvi, no podem fer:
```sage
2 * a # Multiplicació per escalar
```

```sage
a / 3 # Divisió per escalar, si és permesa
```

```sage
1 + a # Suma amb escalar
```

Tampoc podem crear polinomis amb coeficients àdics, o matrius/vectors amb coeficients àdics.

# El model parent / element

La solució que escull *SageMath* és introduir un nou model per les diferents estructures matemàtiques
que volem manipular. Es tracta de tenir un tipus especial per l'estructura (**Parent**), i un tipus pels elements
d'aquesta estructura (**Element**). Ho podem veure en acció amb polinomis (`PolynomialRing`), matrius (`MatrixSpace`),
vectors (`VectorSpace`),... i intentarem replicar-ho amb els àdics.

Primer, importem algunes classes de les quals haurem d'heredar:

```sage
from sage.structure.parent import Parent
from sage.structure.element import Element
from sage.structure.unique_representation import UniqueRepresentation
```

Això ja ens permet crear el conjunt dels àdics. Com que es poden sumar, restar i multiplicar, formarien
un anell, però això no ho aprofitarem aquí (**SageMath** ens pot ajudar, però no el deixarem).

```sage
class Adic(Element): # Heredem de la classe Element
    def __init__(self, parent, a, n = None): # el parent passa a ser un paràmetre
        p = parent.p # El primer el podem agafar del parent
        if n is None:
            x = QQ(a)
            a, b = x.numerator(), x.denominator()
            n = -b.valuation(p) # Estem suposant que el denominador és potència de p
            if b != p**-n:
                raise ValueError(f'{x} no és un àdic, perquè té denominador {b} que no és potència de {p}')
            if n == 0: # Resulta que tenim un enter
                n = a.valuation(p)
                a /= p**n        
        else:
            a, n = ZZ(a), ZZ(n)
            if a % p == 0:
                raise ValueError(f'{a = } no pot ser divisible per {p = }')
        self.mantissa = ZZ(a)
        self.exp = ZZ(n)
        # L'inicialitzem com a Element, li hem de dir qui és el Parent:
        Element.__init__(self, parent)

    def _repr_(self): # Canviem a un sol subratllat
        return f'{self.mantissa} · {self.parent().p}^{self.exp}'

    def _latex_(self): # Fa que el show() funcioni
        if self.exp > 0:
            return f'{self.mantissa} \\cdot {self.parent().p}^{self.exp}'
        elif self.exp < 0:
            a, sgn = self.mantissa.abs(), self.mantissa.sign()
            if sgn >= 0:
                return f'\\frac{{{self.mantissa}}}{{{self.parent().p}^{-self.exp}}}'
            else:
                return f'-\\frac{{{a}}}{{{self.parent().p}^{-self.exp}}}'
        else:
            return latex(self.mantissa)

    def racional(self):
        return QQ(self.mantissa * self.parent().p**self.exp)
    
    def _add_(self, other): # passem a un sol subratllat, i podem assumir que els parents són iguals
        return self.__class__(self.parent(), self.racional() + other.racional()) # Penseu una implementació millor
    
    def _mul_(self, other): # passem a un sol subratllat, i podem assumir que els parents són iguals
        return self.__class__(self.parent(), self.mantissa * other.mantissa, self.exp + other.exp)

class Adics(UniqueRepresentation, Parent):
    def __init__(self, p):
        p = ZZ(p)
        if not p.is_prime():
            raise ValueError('p ha de ser primer')
        self.p = p
        self.element_class = Adic # Aquí li quin són els elements
        Parent.__init__(self, base=QQ)

    # OBLIGATORI: constructor d'elements
    def _element_constructor_(self, x, n=None):
       return self.element_class(self, x, n)        
```

Amb aquesta implementació ja podem fer moltes coses.

```sage
R = Adics(3)
a = R(2 / 9)
b = R(-1/27)
print(f'{a = }, {b = }, {a + b = }, {a * b = }')
```

```sage
show(a)
show(b)
show(R(45))
show(R(7))
```

Si volem permetre operacions com ara `a + 2`, o `5 * a`, cal que li diguem a **SageMath** com ha de pensar
el 2 o el 5 com elements àdics. Això ho podem fer amb un nou mètode del `Parent`. Aprofitem també per afegir representació
tant com a cadena com *LaTeX* pel `Parent` també:

```sage
class Adics(UniqueRepresentation, Parent): # Heredem de les classes UniqueRepresentation i Parent
    def __init__(self, p):
        p = ZZ(p)
        if not p.is_prime():
            raise ValueError('p ha de ser primer')
        self.p = p
        self.element_class = Adic # Aquí li quin són els elements        
        Parent.__init__(self, base=QQ)

    # Representació amb cadena. ULL: Només un subratllat!
    def _repr_(self):
        return f'Adics({self.p})'
    
    # Representació LaTeX. ULL: Només un subratllat!
    def _latex_(self):
        return f'\\mathcal{A}_{latex(self.p)}'
    
    # OBLIGATORI: constructor d'elements
    def _element_constructor_(self, x, n=None):
       return self.element_class(self, x, n)

    # OBLIGATORI: coerció des d'altres tipus, permet escriure 2 * a, o a + 3,...
    def _coerce_map_from_(self, S):
        # Permetem la coerció d'elements de la base (o que s'hi puguin coercionar)
        return self.base().has_coerce_map_from(S)
```

```sage
R = Adics(3)
a = R(2 / 9)
b = R(-1/27)
print(f'{a = }, {b = }, {a + b = }, {a * b = }')
```

```sage
a + 2
```

```sage
5 + b
```

```sage
8 * a
```

Podem afegir també un mètode per fer potències:

```sage
class Adics(UniqueRepresentation, Parent): # Heredem de les classes UniqueRepresentation i Parent
    def __init__(self, p):
        p = ZZ(p)
        if not p.is_prime():
            raise ValueError('p ha de ser primer')
        self.p = p
        self.element_class = Adic # Aquí li quin són els elements        
        Parent.__init__(self, base=QQ)

    # Representació amb cadena. ULL: Només un subratllat!
    def _repr_(self):
        return f'Adics({self.p})'
    
    # Representació LaTeX. ULL: Només un subratllat!
    def _latex_(self):
        return f'\\mathcal{A}_{latex(self.p)}'
    
    # OBLIGATORI: constructor d'elements
    def _element_constructor_(self, x, n=None):
       return self.element_class(self, x, n)

    # OBLIGATORI: coerció des d'altres tipus, permet escriure 2 * a, o a + 3,...
    def _coerce_map_from_(self, S):
        # Permetem la coerció d'elements de la base (o que s'hi puguin coercionar)
        return self.base().has_coerce_map_from(S)

class Adic(Element): # Heredem de la classe Element
    def __init__(self, parent, a, n = None): # el parent passa a ser un paràmetre
        p = parent.p # El primer el podem agafar del parent
        if n is None:
            x = QQ(a)
            a, b = x.numerator(), x.denominator()
            n = -b.valuation(p) # Estem suposant que el denominador és potència de p
            if b != p**-n:
                raise ValueError(f'{x} no és un àdic, perquè té denominador {b} que no és potència de {p}')
            if n == 0: # Resulta que tenim un enter
                n = a.valuation(p)
                a /= p**n        
        else:
            a, n = ZZ(a), ZZ(n)
            if a % p == 0:
                raise ValueError(f'{a = } no pot ser divisible per {p = }')
        self.mantissa = ZZ(a)
        self.exp = ZZ(n)
        # L'inicialitzem com a Element, li hem de dir qui és el Parent:
        Element.__init__(self, parent)

    def _repr_(self): # Canviem a un sol subratllat
        return f'{self.mantissa} · {self.parent().p}^{self.exp}'

    def _latex_(self): # Fa que el show() funcioni
        if self.exp > 0:
            return f'{self.mantissa} \\cdot {self.parent().p}^{self.exp}'
        elif self.exp < 0:
            a, sgn = self.mantissa.abs(), self.mantissa.sign()
            if sgn >= 0:
                return f'\\frac{{{self.mantissa}}}{{{self.parent().p}^{-self.exp}}}'
            else:
                return f'-\\frac{{{a}}}{{{self.parent().p}^{-self.exp}}}'
        else:
            return latex(self.mantissa)

    def racional(self):
        return QQ(self.mantissa * self.parent().p**self.exp)
    
    def _add_(self, other): # passem a un sol subratllat, i podem assumir que els parents són iguals
        return self.__class__(self.parent(), self.racional() + other.racional()) # Penseu una implementació millor
    
    def _mul_(self, other): # passem a un sol subratllat, i podem assumir que els parents són iguals
        return self.__class__(self.parent(), self.mantissa * other.mantissa, self.exp + other.exp)

    def _pow_int(self, n): # Una implementació de les potències enteres
        if n == 0:
            return self.__class__(self.parent(), 1, 0)
        elif n < 0:
            if self.mantissa.abs() != 1:
                raise ValueError('La potència no és un àdic perquè la mantissa no és ±1')

        mantissa = self.mantissa**n
        exponent = n * self.exp
        return self.__class__(self.parent(), mantissa, exponent)

```

```sage
R = Adics(3)
a = R(2 / 9)
```

```sage
a^5
```

```sage
a^0
```

Les potències negatives només es poden fer si la mantissa és més o menys 1:

```sage
a^-5
```

```sage
b = R(27)
```

```sage
b^10
```

```sage
b^-10
```

Finalment, podem afegir codi per poder comparar àdics. Cal importar una funció:
```sage
from sage.structure.richcmp import richcmp
```

```sage
class Adic(Element): # Heredem de la classe Element
    def __init__(self, parent, a, n = None): # el parent passa a ser un paràmetre
        p = parent.p # El primer el podem agafar del parent
        if n is None:
            x = QQ(a)
            a, b = x.numerator(), x.denominator()
            n = -b.valuation(p) # Estem suposant que el denominador és potència de p
            if b != p**-n:
                raise ValueError(f'{x} no és un àdic, perquè té denominador {b} que no és potència de {p}')
            if n == 0: # Resulta que tenim un enter
                n = a.valuation(p)
                a /= p**n        
        else:
            a, n = ZZ(a), ZZ(n)
            if a % p == 0:
                raise ValueError(f'{a = } no pot ser divisible per {p = }')
        self.mantissa = ZZ(a)
        self.exp = ZZ(n)
        # L'inicialitzem com a Element, li hem de dir qui és el Parent:
        Element.__init__(self, parent)

    def _repr_(self): # Canviem a un sol subratllat
        return f'{self.mantissa} · {self.parent().p}^{self.exp}'

    def _latex_(self): # Fa que el show() funcioni
        if self.exp > 0:
            return f'{self.mantissa} \\cdot {self.parent().p}^{self.exp}'
        elif self.exp < 0:
            a, sgn = self.mantissa.abs(), self.mantissa.sign()
            if sgn >= 0:
                return f'\\frac{{{self.mantissa}}}{{{self.parent().p}^{-self.exp}}}'
            else:
                return f'-\\frac{{{a}}}{{{self.parent().p}^{-self.exp}}}'
        else:
            return latex(self.mantissa)

    def racional(self):
        return QQ(self.mantissa * self.parent().p**self.exp)
    
    def _add_(self, other): # passem a un sol subratllat, i podem assumir que els parents són iguals
        return self.__class__(self.parent(), self.racional() + other.racional()) # Penseu una implementació millor
    
    def _mul_(self, other): # passem a un sol subratllat, i podem assumir que els parents són iguals
        return self.__class__(self.parent(), self.mantissa * other.mantissa, self.exp + other.exp)

    def _pow_int(self, n): # Una implementació de les potències enteres
        if n == 0:
            return self.__class__(self.parent(), 1, 0)
        elif n < 0:
            if self.mantissa.abs() != 1:
                raise ValueError('La potència no és un àdic perquè la mantissa no és ±1')

        mantissa = self.mantissa**n
        exponent = n * self.exp
        return self.__class__(self.parent(), mantissa, exponent)

    def _richcmp_(self, other, op):
        return richcmp(tuple([self.mantissa, self.exp]), tuple([other.mantissa, other.exp]), op)

```

```sage
R = Adics(5)
R(3, 1) == 15
```

## Exercicis

### Exercici 1

Implementeu els enters mòdul $n$, on $n$ és un natural arbitrari.

### Exercici 2

Implementeu els polinomis de Laurent, que són expressions de la forma
$$
f(x) = \sum_{i=n}^m a_i x^i,
$$
on $n<=m$ són enters arbitraris. Tot polinomi de Laurent s'expressa de manera única com
$f = x^r g$, on $g$ és un polinomi (habitual) tal que $g(0) \neq 0$.