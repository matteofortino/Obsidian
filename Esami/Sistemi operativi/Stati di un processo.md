#uni
# Sistema Monoprogrammato
In un sistemo operativo monoprogrammato ci sono 2 stati:
- *Attivo*: processo che non ha terminato il suo life-time
- *Bloccato*: in attesa del verificarsi di un evento
il passaggio attivo   -> bloccato e' detto **sospensione**
il passaggio bloccato -> attivo e' detto **riattivaione**

Alla creazione di un processo **DEVE** essere allocato in memoria un [[Descrittore di processo]].

Duranta il suo life time puo' richedere risorse esterne. In questo caso il processo deve aspettare di avere tutte le risorse (*i/o-burst*) prima di poter continuare ad eseguire le sue istruzioni.

Un processo bloccato e' in attesa di un evento detto **Interruzione** che porta alla riattivazione del processo. Le interruzione provenienti da disositivi esterni sono dette *interruzioni esterne*. 
Il *[[DMA]]* (Direct Memory Access) puo far si che che la memoria di un processo cambi anche se il processo e' bloccato
# Sistema Multiprogrammato
In un sistema multiprogrammato il numero di processi e' sempre maggiore del numero di *CPU* (esempi con sistema monoprocessore).
Gli stati di un processo sono:
- *Pronto*: in attesa di una CPU libera
- *Esecuzione*: utilizza la CPU
- *Bloccato*: in attesa del verificarsi di un evento
i passaggi:
- Pronto -> Esecuzione: assegnazione della CPU
- Esecuzione -> Pronto: revocazione della CPU (*preemption*)
- Esecuzione -> Bloccato: sospensione
- Bloccato -> Pronto: riattivazione
**Nota Bene**: il passaggio di riattivazione, a differenza di un sistema monoprogrammato, fa si che il processo sia in attesa di un CPU ma non e' detto che gli venga assegnata subito.

![[stati di un processo.svg | 700]]
Lo stato pronto e' implementato secondo una coda di processi pronti che vengono mandati in esecuzione da uno *schedulatore* in base a un criterio di precedenza. Nel nostro caso il sistema e' *prioritario* quindi ai processi viene assegnato un certo livello di priorita' e lo schedulatore manda in esecuzione i processi a priorita' piu' alta.
