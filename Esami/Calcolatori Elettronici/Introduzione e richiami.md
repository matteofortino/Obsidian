---
id: Introduzione e richiami
aliases: []
tags: []
number headings: auto, first-level 1, max 3, 1.1
---
#uni

L'archittettura generale di un calcolatore moderno e' composta da: 
- **CPU**
- **RAM**
- **I/O**
- **ROM**

I principali argomenti che verranno trattati sono
- *Protezione*
- *Interruzioni*
- *Memoria virtuale*

Cio' ci permettera' di parlare della *multiprogrammazione*, cioe' come eseguire cotemporaneamente piu' programmi in un sistema che ha un solo processo.

# 1 Il bus
Il bus connette tra loro i vari dispositivi in modo che ciasciuno possa comunicare con ciascun altro.
La comunicazione puo' avvenire soltanto tramite operazione di *lettura* o *scrittura* ed entrambe hanno bisogno che venga specificato un **indirizzo**.

Ogni operazione e' richiesta da un componente ed eseguita da un altro.

Per il momento le possibili operazioni sono:
1. la CPU legge e scrive sulla memoria
2. la CPU legge e scrive sull'I/O

In ogni operazione e' l'indirizzo che permette di capire se' coinvolta la memoria o l'I/O. Il discriminante puo' essere operato o riservano indirizzi diversi a ciascuno o definendo degli *spazi di indirizzaento*. Nel secondo caso ogni operazione deve specificare lo spazio di indirizzamento coinvolto.

# 2 La memoria
Si tratta di un sistema organizzato in *Celle*, ogni cella ha la dimensione di *un byte* che e' universalmente fissata a **8 bit**.

Rimangono validi i seguenti punti: 
- Ogni cella contiene una sequenza di bit, il significato di quella sequenza dipense esclusivamente dall'uso che se ne fa.
- La memoria sa eseguire solo operazione di *lettura* e *scrittura*
- Sa esesguire **una sola operazione per volta**
- Per una lettura bisogna specificare l'indirizzo della cella
- Per una scrittura bisogna specificare l'indirizzo della cella e il nuovo contenuto
- La memoria e' completamente passiva
- **tutte** le celle della memoria contengono **sempre** qualcosa
- Ciascuna operazione richede un tempo all'incirca costante
- Nella memoria non c'e' scritto cosa significano i vari numeri, l'intepretazione spetta a chi la legge
# 3 L'I/O (senza interruzioni e DMA)
Come per la memora tutte le possibili operazione sono di lettura e scrittura. Queste operazione conivolgono particolari celle dette **registri** o **porte di I/O**.
Ogni dispositivo viene collegato al sistema tramite un *interfaccia* all'interno della quale si trovano i registri.
Al momento del montaggio ad ogni registro di ogni interfaccia viene associato un indirizzo che lo identifica univocamente.
A differenza della memoria le operazioni posso avere effetti collaterali (la lettura da un registro puo' cambiare lo stato interno di una periferica). Inoltre e' probabile che un registro di $n$ byte puo' essere letto o scritto con un operazione solo su $n$ byte.
Le periferiche di I/O possono cambiare il loro stato interno in seguito ad un interazione con il mondo esterno, questi cambiamenti sono visibili dal calcolatore solo grazie al mutemento del contenuto dei registri. 
# 4 La CPU (senza interruzioni e protezione)

la CPU e' un sistema che sa eseguire una *sequenza di istruzioni*.
Queste istruzioni operano sulla **stato** della CPU che e' contenuto in un insieme di registri (simili alle celle di memoria solo che si trovano all'interno della CPU stessa).
Le istruzioni sono codificate in numeri e sono contenute in memoria. 
Fra i tanti registri delle CPU ce n'e' uno che contiene l'indirizzo della prossima istruzione da eseguire detto **IP (Instruction Pointer).** 
La CPU fa **SOLO** le seguenti cose:
1. preleva dalla memoria l'istruzione puntata dall'IP
2. la decodifica, la esegue e aggiorna l'IP
3. ritorna al punto 1.
Come viene aggiornato l'IP dipende dall'istruzione eseguita. Normalmente viene incrementato per far si che punti all'istruzione successiva ma in caso di istruzione di salto (jmp *indirizzo*) viene aggiornato con l'indirizzo passato.
Punti da tenere a mente: 
- tutto cio' che fa il processore, lo fa perche' sta prelevando o eseguendo una istruzione
- Il processore non smette mai di eseguire istruzioni (tranne se esegue una hlt)
- Le istruzioni sono solo ed esclusivamente quelle del linguaggio macchina
- Tutto quello che vede il processore e' il suo stato corrente e l'istruzione da eseguire. Non ricorda il passato ne prevede il futuro
- Quello che resta di una istruzione dopo la sua esecuzione e' l'effetto che ha avuto sullo stato del processore, della memoria e dell'I/O
- Dualmente tutto cio' che vede un istruzione e' lo stato del processore, della memoria e dell'I/O
# 5 La memoria ROM
In un calcolatore moderno l'I/O non funziona direttamente ma deve essere comandato da un programma.
Come facciamo a caricare un programma in memoria se l'unica via di ingresso ha bisogno di un programma in memoria per funzionare? 
La ROM (memoria di sola lettura) contiene gia' un programma *fisso* che ha lo scopo di caricare il programma vero e proprio da qualche periferica di I/O.

# 6 Il flusso di controllo (senza interruzioni)
Quanto esposto sopra e' tutto cio' che l'*hardware* sa fare. E' il *software* che a orchestrare questi comportamenti elementare per ottenerne di piu' complessi.
L'architettura esiste solo per eseguire il software ed e' lui che comanda, la CPU e' sotto il controllo del software.

Il controllo possiamo dire che "fluisce" da un istruzione all'altra come deciso dal software stesso.
Se vi sono delle *subroutine* $S_1$ e $S_2$ si dice che $S_1$ cede il controllo a $S_2$ traime una `call` e $S_2$ restituisce il controllo a $S_1$ tramite una `ret`

# 7 Hardware e Software
Si puo' trovare difficolta' a fare differenza tra i due,  se ad esempio ci chiediamo se una certa operazione $x$  e' fatta in hardware o software poniamoci le seguenti domande:
1. Se $x$ e' fatta in software vuol dire che esiste un programma in memoria (anche di una solo righa) che fa $x$. So scrivere questo programma?
2. Il software non ha effetto se la CPU non lo esegue. Coma fa il flusso di controllo a passare da $p$ a $x$ nel momento in cui serve eseguila?
Se anche una sola di queste due domande risulta assurda e' probabile che $x$ non sia fatto in software.
