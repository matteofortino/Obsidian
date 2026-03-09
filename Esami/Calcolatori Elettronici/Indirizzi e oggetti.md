---
number headings: auto, first-level 1, max 3, 1.1
---
#uni
Tutti gli indirizzi sono relativi a un bus e tutti i componenti collegati al bus, in grado di ripondere a richesti di lettura o scrittura, devono avere degli indirizzi assegnati in modo univoco.

I possibili indirizzi su un bus vanno da $0$ a $2^n -1$  per qualche $n$ che dipende dal bus.

> NOTA BENE: tutti gli indirizzi di un bus sono sempre raggiungibili anche se non sono collegati a nulla

Le operazioni sugli indirizzi vanno sempre considerate in modulo $2^n$.
Gli indirizzi hanno un ordinamento naturale per il quale dato un indrizzo $x$ il successivo e' $x + 1$ e il precedente e' $x -1$. 

Dato che queste operazioni sono da intendersi modulo $2^n$ l'indirizzo che precede $0$ e' $2^n - 1$

# 1 Scostamenti (offset)
Dati due indirizzi $x$ e $y$, l'*offset* di $y$ rispetto a $x$ e' definito come $(y - x)  \mod  2^n$ . l'offset conta gli indirizzi che e' necessario saltare partendo da $x$ per arrivare a $y$.

## 1.1 caso particolare
Se rappresentiamo gli offset come numeri complemento alla radice du $n$ bit diventa indifferente considerarli con o senza segno.

# 2 Intervalli (range)
Un *intervallo* e' una sequenza di indirizzi. Ci sono vari modi per rappresentare un insieme, la convenziona usata da noi chiede di rapprensentare un intervallo specificnado il **primo** indirizzo che ne fa parte, sia $x$ e il primo indirizzo successivo a $x$ che non ne fa parte, sia $y$. 
$$ [x,y) = \left\{ i | x \leq i < y \right\}$$
Noi ci limiteremo a cosiderare casi dove $x < y$ sempre. Va tenuto conto di un solo caso $[x, 0)$ che secondo la definizione e' vuoto ma per noi e' da intenrdersi "da $x$ fino all fine".

# 3 Allineamenti
Definizioni: 
- un indirizzo e' *allineato* a $2^b$ se e' multiplo di $2^b$ 
- un intervallo e' allinato a $2^b$ se il suo indirizzo base e' allineato $2^b$ 
- se la lunghezza di un intervallo e' una potenza di 2, si dice che quell'intervallo e' *allineato* *naturalmente* se e' allineato alla sua lunghezza

Un indirizzo allineato a $2^b$ ha i $b$ bit meno significativi nulli.

# 4 Confini (boundaries)
Dato $b \leq n$  tutti gli indirizzi allineati a $2^b$ son detti *confini* di $2^b$. I confini si trovano partendo da 0 e procedento ad offset di $2^b$. Questi confini delimitano degli intervalli di indirizzi, allineati naturalmente, che sono chiamati *regioni* *naturali* di $2^b$. 
Gli $n -b$ bit piu' significativi indicano il **numero di regione**. Un indirizzo quindi e' identificato dal suo numero di regione e dal suo offset all'interno della regione.
E' immediato in hardware trovare numero di regione e offset, vediamo come si fa in software.
Prendiamo come $n$ la dimensione in bit di un *unsigned long* che e' pari a 64.

Numero di regione: 
```c
r = x >> b;
```
Offset: 
```c
o = x & ((1UL << b) - 1);
```
Base della regione a cui appartiene $x$:
```c
c = x & ~((1UL << b) - 1);
```

Dato un intervallo non vuoto $[x, y)$ vogliamo trovare la prima regione toccata dall'intervallo e la prima non toccata. 

Per la prima regione toccata basta vedere dove cade $x$. 
Per la prima regione non toccata dipende, se $y$ e' nella stessa regione di $x$ la prima non toccata e' la prossima, se $y$ e' cade su un confine e' lei stessa la prima regione non toccata. 

```c
nt = (y + (1UL << b) - 1) >> b;
```
In questo caso sommiano a $y$ il valore di $2^b - 1$, se $y$ e' su un confine $y + 2^b - 1$ si trova nella stessa regione, altrimenti in quella successiva.

Possiamo anche sommare uno al numero di regione in cui cade $y - 1$ 
```c
nt = ((y - 1) >> b)) + 1;
```


