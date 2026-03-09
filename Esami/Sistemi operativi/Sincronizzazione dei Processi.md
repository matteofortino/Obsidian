---
number headings: auto, first-level 1, max 6, 1.1
---
#uni
# 2 Tipi di interazione tra processi
Esistono tre tipi di interazione: 
1. **Cooperazione**: sincronizzazione diretta o esplicita
2. **Competizione**: sincronizzazione indiretta o implicita
3. **Interferenza**: errori dipendenti dal tempo (*time-dependet error*)
## 2.1 Sistemi a memoria comune
In un sistema a **memoria comune** succede che i processi, pur avendo risorse private, possono *condividere strutture dati con altri processi* del sistema.

Questo modello ha dei problemi: 
- mutua esclusione
- sincronizzazione
La stessa risorsa non puo' essere acceduta contemporaneamente da piu' di un processo. 
## 2.2 Sistemi a memoria locale
Assimilabile ad una rete di calcolatori. Dal punto di vista del singolo calcolatore lui ha la sua *memoria accedibile solo da lui*, **condivide** frammenti con gli altri calcolatori grazie allo **scambio di messaggi**.

# 3 Mutua Esclusione
esempio:
```cpp
T stack[n];
int top -= 1;

void Inserimento(T y) {
	top++;
	stack[top] = y;
}

T Prelievo() {
	T temp;
	temp = stack[top];
	top--;
	return temp;
}
```
possibile sequenza di esecuzione:
```cpp
t0: top++;             (p1)
t1: temp = stack[top]; (p2)
t2: top--;             (p1)
t3: stack[top] = y;    (p1)
```
notiamo che con questo flusso di esecuzione ci sono due **inconsistenze**: 
1. viene copiato in temp un valore nullo
2. viene sovrascritto il top della pila con y
Questi errori avvengono perche' non e' stata garantita **l'atomicita'** delle fuzioni *Inserimento* e *Prelievo*

Viene implementato un sistema basato su dei **lock** per permettere l'atomicita'

```armasm
lock(x):
	TSL registro, x (Test and Set Lock)
	CMP registro, 0
	JNE lock
	RET
	
unlock(x):
	MOV x, 0
	RET
```

# 4 Semafori
- Un **semaforo** $s$ e' una variabile alla quale e' associato un *intero non negativo* $s.value$ con valore iniziale $s_0 \geq 0$. 
- Al semaforo e' associata una *lista di attesa* $s.queue$ nella quale sono posti i descrittori dei processi che attendono l'autorizzazione al procedere. 
- Sul semaforo sono ammesse solo due operazione (*primitive*) utilizzabili con meccanismo della system call: 
	- **wait($s$)**
	- **signal($s$)** 
```cpp
void wait(s) {
	if(s.value == 0)
		<il processo viene sospeso e suo descrittore inserito in s.queue>;
	else
		s.value = s.value - 1;
}

void signal(s) {
	if (<esisite almeno un processo nella coda s.queue>)
		<descrittore del primo processo estratto da s.queue e inserito in coda pronti>;
	else
		s.value = s.value + 1;
}
```
# 5 Comunicazione
## 5.1 Memoria Comune
- Il processo produttore deposita un messaggio in un buffer
- Il processo consumatore preleva il messagio dal buffer
```
produttore -> | buffer | -> consumatore
```
Devo quindi definire due funzioni per permettere l'**inserimento** e la **lettura** di un messagio dal buffer.
Notiamo che queste funzioni hanno sezioni critiche quindi dovranno essere **atomiche** per garantire il c*orretto flusso di esecuzione*.

Il problema della comunicazione non puo' essere risolto solo con la mutua esclusione per i seguenti motivi:
1. il produttore non deve inserire un messagio nel buffer se questo e' pieno
2. il consumatore non deve prelevare un messagio dal buffer se questo e' vuoto

vengono utilizzati due semafori:
- spazione_disponibile  => valore iniziale 1
- messaggio_disponibile => valore iniziale 0
```cpp
// Processo produttore
do {
	<produzione del nuovo messagio>;
	wait(spazio_disponibile);
	<deposito del messagio nel buffer>;
	signal(messagio_disponibile);
} while(!fine);
```
```cpp
// Processo consumatore
do {
	wait(messagio_disponibile);
	<prelievo del messaggio dal buffer>;
	signal(spazio_disponibile);
	<consumo del messaggio>
} while(!fine);
```
 Nel caso di piu' produttore o consumatori le fasi di inseriemento e prelievo del messagio devono essere rese atomiche con i semafori di mutua esclusione.
## 5.2 Memoria Locale
Anche in questo caso per garantire la comunicazione vi e' uno scambio di messagi tra processi. 

Il meccanismo di comunicazione usato e' detto **IPC**(*Inter-Process-Communication*) offerto dal nucleo del sistema.
Si basa sull'utilizzo di due primitive: 
- **send**(destinazione.messagio): spedeisce un messagio ad una determinata destinazione
- **receive**(origine.messagio): riceve un messagio da una detirmanata origine o da tutte le origini.
Il formato del messagio che viene inviato e' il seguente:
![[formato_messagio.svg]]
### 5.2.1 Comunicazione simmetrica
Avviene attravaerso una canale di comunicazione **diretto**.
```cpp
// Processo produttore P
pid C = ...;
main() {
	msg M;
	do {
		produci(&M);
		...;
		send(C.M);
	} while (!fine);
}
// Processo consumatore C
pid P = ...;
main() {
	msg M; 
	do {
		receive(P, &M);
		consuma(M);
	} while (!fine);
}
```
### 5.2.2 Comunicazione asimmetrica
Avviene tramite un canale di comunicazione **indiretto**. 
I messagio sono depositati e prelevati da un struttura dati detta **porta**.
In quest contensto la porta prende il nome di **mailbox** ed e' gestita dal sistema operativo.

```cpp
// Processo produttore P
pid C = ...;
main() {
	msg M;
	do {
		produci(&M);
		...;
		send(C.M);
	} while (!fine);
}
// Processo consumatore C
... ;
main () {
	msg M; 
	pid id;
	do {
		receive(&id.M);
		...;
		consuma(M);
	}while (!fine);
}
```