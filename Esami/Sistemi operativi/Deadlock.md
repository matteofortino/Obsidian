#uni 
# Il problema di Deadlock
Un deadlock si verifica quando vi e' un insieme di processi in attesa di ricevere una risorsa da un processo che fa esso stesso parte dell'insieme.

Questo problema si verifica se queste 4 condizioni sono vere contemporaneamente: 
- **Mutua esclusione**: solo un processo alla volta puo' usare una risorsa.
- **Possesso e attesa**: un processo che ha gia' acquisito almeno una risorsa va in attesa di altre risorse in possesso di altri processi.
- **No permessi di revoca**: una risorsa puo' essere rilasciata solo volontariamente dal processo che la possiede una volta che la sua esecuzione termina.
- **Attesa circolare**: esiste un insime di processi $\{P_0, P_1, ..., P_n\}$ tale per cui il processo $P_0$ e' in attesa di $P_1$, $P_1$ in attesa di $P_2$, $P_{n-1}$ in attesa di $P_n$ e $P_n$ in attesa di $P_0$.  
# Metodi per la gestione del Deaklock
Ci sono 3 modi per gestire/prevenire lo stato di deadlock: 
- Assicurarsi che il sistema non entri **mai** in uno stato di deadlock
- Permettere al sistema di entrare in uno stato di deadlock a patto di sapersi recuperare
- Ignorare il problema e pretendere che il deadlock non si verifichi mai. 
L'ultima soluzione e' utilizzata damolti sistemi operativi tra i quali vi e' anche UNIX
# Prevenzione del Deadlock
**Mutua esclusione**:
- non e' richiesta per riorse non condivisibili, ma e' obbligatorio per le risorse condivisibili
**Possesso e attesa**:
- deve garantire che qualunque processo che richeda un risorsa non ne possegga altre.
**No permessi di revoca**: 
- se un processo che e' in possesso di alcune risorse ne richede altre che non possono essergli immediatamente allocate, allora tuttre le risorse di cui e' in possesso vengono deallocate
- le risorse revocate sono aggiunte alla lista delle risorse per le quali il processo e' in attesa
- il processo torna in eseguzione solo quano puo' acquisire tutte le sue precedenti risorse e in piu' quelle nuove
**Attesa circolare**:
- Impone un ordinamento totale di tutti i tipi di risorse e richiede che ogni processo richieda le proprie risorse in un ordine crescente di enumerazione
- se $P$ possiede $R_k$ non puo' richiedere $R_{k-i}$
## Evitare il deadlock
La prevenzione richede che il sistema abbia a disposizione delle informazione addizionali a priori
- Il metodo piu' utile e facile richede che ogni porcesso dichiari il **massimo numero** di risorse di ogni tipo delle quali potrebbe avere bisogno
- L'algoritmo di prevenzione del deadlock esamina dinamicamente lo stato delle risorse allocate per assicurare che non ci sia una *attesa circolare*
- Lo stato delle risorse allocate e' definito dal numbero di risorse disponibili e allocate, e la **domanda massima** dei processi
# Stato sicuro
Quando un processo richede una risorsa disponibile, il sistema deve decidere se l'allocazione immediata di essa lascia il sistema in uno **stato sicuro**.

Il sistema e' in uno stato sicuro se esiste una sequenza $<P_1,P_2,...,P_n>$ di **tutti** i processi nel sistema *che per ogni $P_i$ le risorse che $P_i$ puo' sempre richiedere sono soddisfatte dalle risorse disponibili al momento e da tutte le risorse in possesso di tutti i $P_j$ con $j < i$ .*

Cio' significa: 
- Se le risorse di cui $P_i$ ha bisogno non sono immediatamente disponibili, allora $P_i$  puo' aspettare finche' tutti i $P_j$ hanno finito.
- Quando $P_j$ ha finito, $P_i$ puo' ottenere tutte le risorse di cui ha bisogno, eseguire, rilasciare le risorse allocare e terminare.
- Quando $P_i$ finisce $P_{i+1}$ puo' ottenere le risorse ed eseguire e cosi via...
# Fatti di base
1. Se un sistema e' in uno stato sicuro $\implies$ no deadlocks
2. Se un sistema e' in uno stato non sicuro $\implies$ possibilita di deadlocks
3. Evitare il deadlock $\implies$ assicura che tutto il sistema non entri mai in uno stato non sicuro
# Algoritmi per evitare il deadlock
Si dividono in due tipi:
- Istanza singola del tipo di una risorsa
	- grafico di allocazione delle risorse
- Istanze multiple del tipo di una risorsa
	- Algoritmo del banchiere
## Grafico di allocazione risorse
Segue i seguenti passaggi: 
1. Il **claim edge** $P_i \rightarrow R_j$  indica che il processo $P_i$ potrebbe richedere la risorsa $R_j$ (linea tratteggiata)
2. Il claim edge si converte in un **request edge** quando il processo richede una risorsa
3. Il request edge si converte in un **assignement edge** quando la risorsa viene allocato al processo
4. Quando una risorsa viene rilasciata da un processo, l'assignement edge si ricoverte in un claim edge
5. Le risorse devono essere richeste a priori nel sistema
![[diagramma_allocazione_risorse.svg]]
La richesta puo' essere concessa solo se la conversione da un **request edge** a un **assignemet edge** non fa si che si formi un ciclio nel grafico
## Algoritmo del banchiere
