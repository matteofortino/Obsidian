---
id: Teoria di Python
aliases: []
tags: []
---
#uni
# Sequenze
Le stringe sono in realtà delle sequenze

## Lunghezza
```python
s = "rabbit"
len(s) -> 6 
```

## Minimo e massimo
```python
s = "rabbit"
min(s) -> 'a'
max(s) -> 't'

s = "12 67"
min(s) -> ' '
max(s) -> '7'
```

## Indicizzazione
```python
s = "rabbit"
s[0] -> 'r'
s[len(s) - 1] -> 't'
s[-1] -> 't'
s[-2] -? 'i'
```

## Slicing
```python
s = "rabbit"
s[1:3] -> "ab"
```
L'elemento "start" è incluso, l'elemento "end" no

Si può aggiungere in terzo numero detto *stride*
```python
s = "rabbit"
s[0:-1:2] -> "rbi"
```

## Formatting
Metodo base con il carattere '+'
```python
name = "Matteo"
age = 20
print("My name is " + name + " and my age is " + age)
```
Usando il metodo *format*
```python
name = "Matteo"
age = 20
print("My name is {} and my age is {}".format(name, age))
```
Usando le *f-strings*
```python
name = "Matteo"
age = 20
print(f"My name is {name} and my age is {age}")
```
# Liste
Sono una sequenza di oggetti che possono avere tipi differenti
```python
# lista di interi
l = [1, 2, 3, 4, 5]

#lista di tipi diversi fra loro
l = [1, "matteo", [1, 2]]
```
## Mutabilità e Immutabilità
Le stringhe sono tipi **Immutabili**
Le liste sono tipi **Mutabili** 

Per via di ciò quando effettuo dei cambiamenti su una stringa viene creato un nuovo oggetto di tipo stringa

Gli assegnamenti creano delle referenze agli oggetti non copie. Questo può portare a comportamenti indesiderati
```python
x = [1, 2, 3]
y = [1, x, 3]

print(x) -> [1, 2, 3]
print(y) -> [1, [1, 2, 3], 3]
x[0] = 999
print(x) -> [999, 2, 3]
print(y) -> [1, [999, 2, 3], 3]
```

Possiamo dire a python esplicitamente di creare una copia
```python
x = [1, 2, 3]
y = [1, x.copy(), 3]

print(x) -> [1, 2, 3]
print(y) -> [1, [1, 2, 3], 3]
x[0] = 999
print(x) -> [999, 2, 3]
print(y) -> [1, [1, 2, 3], 3]
```
## Iteratori
Tutto quello che può essere usato con un for loop è un **Iteratore(iterable)**
```python 
r = range(4)
iterator = iter(r)
next(iterator) -> 0
next(iterator) -> 1
next(iterator) -> 2
next(iterator) -> 3
```

# List Comprehensions
They are a way to build an ew list by running an expression on each item in a sequence, one at a time. 

They can always be written as an explicit for loop but they have some advantages: 
1. Less code 
2. Easier to read
3. Possibly faster
4. More *"Pythonic"*
```python
r1 = [1, 3, 5, 9]
r2 = [12, 18, 11, 10]
r3 = [25, 27, 22, 21]

m = [r1, r2, r3]

# for loop way
first_column = []
for i in range(len(m)):
	first_column.append(m[i][0])
	
# list comprrhension way 
firts_column = [r[0] for r in m]
```

# Tuple 
Le tuple sono un altro tipo di oggetti immutabili
```python
my_tuple = (1, "a", [1, 2])

my_tuple = 1, "a", [1, 2]

type(my_tuple) -> <class 'tuple'>

```
supportano tutte le operazioni che supportano le liste tranne l'assegnazione

Si puo fare anche l'operazione inversa *Unpacking*
```python
t = (1,2,3)
(a, b, c) = t
a -> 1
b -> 2
c -> 3
```
## Zip function
Takes iterables and return and iterator of tuples
```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9]
letters = ["a", "b" , "c", "d", "e", "f" , "g", "h", "i", "j"]

z = zip(letters, numbers)
for i in z:
	print(f"i: {i}, number: {i[0]}, letter: {i[1]})
	
for i, j in z: 
	print(f" number: {i}, letter: {j})

```


# Decoratori
Sono una potente feature che permettono di modificare il comportamento delle funzioni o dei metodi senza acmbiare il codice sorgente.
Un decoratore è una funzione che prende come argomento un' altra funzione. Restituisce la funzione modificata
```python
def uppercase_decorator(func):
	def wrapper:
		original_result = func()
		modified_result = original_result.upper()
		return modified_result
	return wrapper
	
```
Un decoratoe può essere applicatato usando il carattere "@". 
```python
def uppercase_decorator(func):
	def wrapper:
		original_result = func()
		modified_result = original_result.upper()
		return modified_result
	return wrapper
	
@uppercase_decorator
def greet():
	return "Hello!"	
	
print(greet()) # -> outpout: HELLO!
```

La sintassi vista è corretta per funzioni che non richedono argomenti. Vediamo adesso il caso in cui la funzione richede dei paramentri
```python
def uppercase_decorator(func):
	def wrapper(*args, **kwargs):
		original_result = func(*args, *kwargs)
		modified_result = original_result.upper()
		return modified_result
	return wrapper
	
@uppercase_decorator
def greet(name):
	return f"Hello {name}!"	
	
print(greet("Matteo")) # -> outpout: HELLO, MATTEO!
```
Definisco quindi un decoratore genrico che prendi un numero di paramentri dinamico. 
Un problema che si verica adesso è che il *metadati* della funzioni originali vengono persi, la funzione originale non può più essere acceduta. 
```python
import functools
def uppercase_decorator(func):
	@functools.wraps(func) #preserve metadata
	def wrapper(*args, **kwargs):
		original_result = func(*args, *kwargs)
		modified_result = original_result.upper()
		return modified_result
	return wrapper
	
@uppercase_decorator
def greet(name):
	"""Funzione che ringrazia il nome passato"""
	return f"Hello {name}!"	
	
print(greet("Matteo").__name__) # Output: greet (senza il wrap il nome sarebbe warpper)
print(greet("Matteo").__doc__) # Output: Funzione che ringrazia il nome passato
```

Il decoratore a sua volta può accettare paramentri, questo si fa aggiungendo un terzo livello di indentazione. 
```python
def repeat(n=1):
	def decorator(func):
		@functools.wraps()
		def wrapper(*args, **kwargs):
			result = None
			for _ in range(n):
				result = func(*args, **kwargs)
			return result 
		return wrapper 
	return decorator

@repeat(n=3)
def say_hello(name):
	print(f"Hello, {name}!")b
```

# Yield From 
Il comando *yeild from* permette di delegare delle azione ad un altro generatore.


