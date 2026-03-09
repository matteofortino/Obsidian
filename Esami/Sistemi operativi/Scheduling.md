---
id: Scheduling
aliases: []
tags: []
---
-  #uni
Attivita' mediante la quale il sistema operativo effetua delle scelte tra i processi con riguardo a: 
- caricamento in memoria centrale
- assegnazione della CPU
Esistono tre diverse attivta' di scheduling: 
1. *a breve termine*
	- sceglie tra i processi pronti quello a cui assegnare al CPU
	- interviene quando il processo in esecuzione perde il controllo della CPU
2. *a medio termine* (swapping)
	- trasferimento temporaneo in memoria secondaria di processi
	- memoria principale inferiore alla somma delle richieste dei vari processi
3. *a lungo termine*
	- sceglie nella memoria secondaria quali programmi caricare in memoria centrale
	- controlla il grado di multiprogrammazione (numero di processi attivi contemporaneamente)
	- e' un componente importante dei sistemi *batch* multiprogrammati
I processi posso essere descritti
	- **I/O-bound process**: spendono la maggior parte del tempo in operazione I/O che computazioni, caratterizzati da molti ma corti *CPU bursts*
	- **CPU-bound process**: spendono la maggior parte del tempo facendo computazioni che sull' I/O, caratterizzati da pochi ma molto lungi *CPU bursts*
# Livelli di scheduling
Una prima classificazione viene fatta relativamente alla scelta di quali siano gli eventi in seguito ai quali lo scheduler deve intervenire
- **non preemptive** (senza diritto di revoca): lo scheduler interviene esclusivamente quando il processo libera spontaneamente la CPU
- **preemptive** (con diritto di revoca): lo scheduler ha la possibilita' di intervenire per revocare la CPU al processo in eseguzione ed assegnarla ad un altro.
# Algoritmi di Scheduling
- First Come First Served (FCFS) --> non preemptive
- Shortest Job First (SJF) --> non preemptive
- Shortest Remeaning Time First (SRTF) --> preemptive
- Round Robin (RR) --> preemptive
- Schedulazione su base prioritaria
- Schedulazione a code multiple
- Schedulazio di sistemi in tempo reale
	- rate monotonic (RM)
	- earliest deadline first (EDF)
## Valutazione degli algoritmi
Le valutazioni si basano su: 
- Utlizzo della CPU
	- massimizzare l'uso della CPU per unita' di tempo
- Tempo medio di completamento (*turnaround time*)
	- valor medio del tempo intercorso da quando il processo entra in coda pronti alla fine della sua esecuzione
- Produttivita' (*throughput rate*)
	- valor medio dei processi completati per unita' di tempo
- Tempo di risposta
	- valor medio del tempo intercrso da quando il processo entra in coda pronti a quando fornisce la prima risposta
- Tempo di attesa
	- valor medio del tempo trascorso da un processo in coda pronti in attesa di ricevere la CPU
- Rispetto dei vincoli temporali

## First Come First Served (FCFS)
Algoritmo non prioritario senza permessi di revoca della CPU.
Facciamo conto di avere 4 processi in coda pronti e simualiamo l'esecuzione dell'algoritmo
![[fcfs.svg]] 
Il problema sorge quando in coda arrivano prima i processi molto lunghi facendo da "tappo" a quelli piu' brevi

## Shortest Job First (SJF)
Algoritmo prioritario (priorita' fissa) senza permissi di revoca dela CPU.
![[sjf.svg]] 
Una versione piu' ottimizzata e' quella con i permessi di revoca. 
Quando, durante l'esecuzione di un processo, entra in coda pronti un nuovo processo con un **CPU-burst** minore, bisogna eseguire un cambio di contesto.

Gli algoritmi preemptive (con permessi di revoca) riescono ad agire nel momento di aggiunta di un processo in coda pronti raggiunstando immediatamente le priorita garantendo una migliore efficienza.
Uno dei principali *drawbacks* (difetti) degli algoritmi con permessi di revoca su base prioritaria e' la **starvation**. 
Accade quando il sistema e' aperto quindi nuovi processi continuano a fluire in coda pronti. Cio' causa i processi a priorita' molto bassa di essere eseguiti lentamente. 
## Stima del CPU-burst di un processo
Non sempre il valore del CPU-burst e' noto a priori.
Un metodo per stimarlo fa uso della *media esponenziale* che permette di tener conto dela "storia" dei valori misurati nei precedenti intrevalli di esecuzione.
	- t_n: durata del CPU-burst n-mo
	- s_n: la sua stima
	- a: fattore compreso tra 0 e 1
		- a = 0 -> tutte le stime sono uguali
		- a = 1 -> conta solo l'ultima stima
In generale si prende come valore di a = 1/2.
## Shortest Remainig Time First (SRTF)
Algoritmo con permessi di revoca prioritario.
![[srtf.svg]]
All'inseriemento in lista pronti di un nuovo processo viene contrallato i suo CPU-burst e comparato con quello del processo in esecuzione, nel caso il primo sia minore del secondo la CPU viene revocata al processo in esecuzione e messa disponibile a quello nuovo fino al verificarsi di un evento.

In questo algormitmo la priorita' di un processo si va ad innalzare col tempo, questo aiuto a ridurre la probabilita' di avere situazioni di **starvation**.

## Round Robin (RR)
Algoritmo non prioritario con permessi di revoca.
In questo algoritmo viene assegnato un *quanto* di tempo ad ogni processo che indica per quanto tempo di fila quel processo puo' tenere la CPU occupata, alla fine del *quanto* indicato si passa al processo successivo.
![[rr.svg]]
In questa simualzione all'istante 150 la coda pronti e' vuota quindi non viene eseguito il cambio di contesto.
## Sistema a tempo reale (embedded)
Un **sistema a tempo reale** (o **real-time system**) è un sistema informatico progettato per **rispondere a eventi o stimoli esterni entro tempi prestabiliti e prevedibili**. La correttezza di questi sistemi non dipende solo dal risultato logico delle operazioni, ma anche **dal momento in cui i risultati vengono prodotti**.
Quando questi sistemi sono integrati in dispositivi specifici, si parla di **sistemi embedded a tempo reale** (_real-time embedded systems_). Sono tipicamente composti da **hardware dedicato** e **software specializzato**, spesso con risorse limitate (memoria, potenza di calcolo, energia).
### Classificazione
- **Hard real-time**: il mancato rispetto delle scadenze temporali può avere conseguenze gravi (es. airbag, sistemi medici).
- **Soft real-time**: il ritardo riduce le prestazioni, ma non compromette la funzionalità (es. streaming video).
- **Firm real-time**: il risultato in ritardo è inutile, ma non catastrofico (es. controllo industriale).
### Esempi di applicazione
- Sistemi di controllo automobilistici (ABS, airbag)
- Avionica e controllo di volo
- Robotica industriale
- Dispositivi medicali
- Sistemi IoT (Internet of Things)
In sintesi, un sistema a tempo reale embedded è progettato per **garantire risposte tempestive e affidabili** in **ambienti con vincoli temporali rigorosi**.
### Fattore di Utilizzazione
Condizione necessaria per permettere ai processi di essere nel periodo T.
Si calcola $$\LARGE U = \sum_{i = 0}^{n - 1}{\frac{c_i}{t_i}}$$
### Rate Monotonic (RM)
Algoritmo prioritario (*priorita' fissa*) senza permessi di revoca.
In questo caso i processi sono detti **periodici** perche si ripetono nel tempo con periodo t che assumiano essere anche la **deadline** del processo.
La priorita' dei processi viene calcolata in base al suo CPU-burst, piu' e' basso piu' la priorita' e' alta.
E' fondamentale che un processo finisca entro la sua deadline altrimenti **fallisce**. 
Nei sistemi **embedded a tempo reale** si suppone di sapere sempre quanti processi periodici ci sono quindi si stima un periodo di schedulazione T che e' il *minimo comune multiplo* dei vari t_i dei processi.
![[rm.svg]]
In questo esempio il processo Pb non fa in tempo a terminare entro la sua deadline.
### Earliest Deadline First (EDF)
Algoritmo prioritario (*priorita' dinamica*) con permessi di revoca.
Man mano che passa il tempo la deadline di un processo di avvinica quindi la priorita' di quel processo aumenta automaticamente.
![[edf.svg]]
In questo esembio al tempo 8 la priorita' del processo Pb e' piu' alta quindi la sua eseguzione continua permettendogli di rispettare la deadline.s
