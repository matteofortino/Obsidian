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