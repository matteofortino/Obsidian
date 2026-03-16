#uni 
# Processi Aleatori
Esistono processi che si evolvono nel tempo in maniera *non deterministica*, o meglio **aleatoria**. Per affrontare questo tipo di eventi abbiamo bisogno di partire dalla basi della **teoria della probailità**.

Esistono due tipi di modelli matematici:
- *Deterministico*: non vi è alcuna incertezza riguardo il suo comportamento ad ogni istante nel tempo. I sistemi *Lineari Tempo-Invarianti*(**LTI**) ne sono un esempio.
- *Probabilistici*: non è possibile determinare la sua evoluzione a priori.
Quest'ultimo tipo di sistemi sono necessari per la progettazione di sistemi che: 
- Abbiano prestazione affidabili a fronte dell'incertezza
- Siano computazionalmente efficienti
- Convenienti dal punto di vista economico

Osserviamo un sistema di comunicazione wireless.
![[sistema_comunicazione_wireless.png]]
La trasmissione dell'informazione e' soggetta a *interferenze* le cui fonti includono:
- *Rumore*, generato all'interno dei dispositivi elettronici nel trasmettitore e ricevitore
- *Interferenza*, da parte di altri trasmettitori
- *Distorsioni*, ad esempio il fading del canale
Il segnale ricostruito al ricevitore è tipicamente una replica attenuata di quello trasmesso. Il processo di ricostruzione del messagio è modellabile come processo **aleatorio**.

Supponiamo che il canale wireless condiviso utilizzi un protocollo di accesso casuale a tempo suddiviso in slot(*Slotted ALOHA*). 
- Sono presenti $N$ nodi IoT identici.
- In ogni slot, ciascun nodo trasmette con probabilià $p$
- Una trasmissione a successo se esattamente un nodo trasmette nello slot.
![[slotted_aloha_2.png]]
Vogliamo calcolare:
- La probabilità di successo $P_s$ in uno slot
- Determinare il valore ottimale di $p$, detto $p_0$ che massimizza la probabilità di successo in unou slot
- Calcolare tale probabilià massimo $P_S^{MAX}$ 

Si dimosta che $P_s = Np(1 - p)^{N-1}$: 
![[desmos-graph.png]]
Guardando alla funzione troviamo che il valore per cui la funzione è massima è $p_0 = \frac{1}{N}$ quindi $P_S^{MAX} = (1 - 1/N)^{N-1}$ il cui $\lim_{N\to \infty}(1 - 1/N)^{N-1} = \frac{1}{e}$. Si otteine quindi un efficienza del 37%.

# Teoria della probabilità
L'obiettivo è quello di fornire un modello matematico per lo studio di fenomeni non deterministici.

Ricordiamo velocemente l'algebra degli insiemi.
![[algebra_insiemi.png]]

e introduciamo della terminologia: 
- *Esperimento casuale*: procedimento di osservazione dello “stato” finale del sistema sottoposto all’esperimento, che deve essere ipotizzato ripetibile un numero indefinito di volte con le stesse modalità di esecuzione.
- *Risultato*: il risultato dell’esecuzione dell’esperimento.
- *Spazio campione $\Omega$*: l’insieme di tutti i possibili risultati dell’esperimento.
- *Evento*:  insieme dei risultati individuabili attraverso una loro caratteristica comune; un evento è un sottoinsieme dello spazio campione.
- *Evento certo*: lo spazio campione $\Omega$.
- *Evento impossibile*: l'insieme vuoto $\emptyset$.
- *Evento elementare $a$*: un elemento di $\Omega$, un singolo risultato dell'esperimento.
- *Prova*: singola esecuzione dell’esperimento da cui si ottiene un singolo risultato $a$.
## Definizione classica della probabilità
>[!Definizione]
>Probabiltà dell'evento: $$ P(A)  = \frac{m_a}{M} $$ 

Questa definizione vale sotto certe ipotesi:
- I risultati dell'esperimento sono *ugualmente verosimili*
- $M$: numero di elementi dello spazio campione
- $m_a$: numero di elementi favorevoli all'evento $A$
Presenta dei *vantaggi*: 
- Definisce la probabilità in modo **aprioristico**, senza ricorrere a nessuna prova sperimentale.
ma anche degli *svantaggi*: 
- Si assume che i risultati siano tutti ugualmente verosimili, cioè **equiprobabili**; si noti che il concetto di probabilità è proprio ciò che si vuole definire.
- Non si possono gestire quei casi in cui i risultati non sono equiprobabili
## Definizione frequentista della probabilità 
>[!Definizione]
>Frequenza relativa di A: $$ f_n(A) = \frac{n_a}{N} $$
>Probabilità dell'evento A: $$ P(A) = \lim{N \to \infty} f_n(A) = \lim_{N \to \infty} \frac{n_a}{N} $$

dove bisogna ripetere un esperimento $N$ volte e contare il numero di volte $n_a$ in cui si verifica l'evento A.

 *Vantaggi*:
 - Non si basa su ipotesi a priori ma sulla conoscenza a *posteriori* di un dato fenomeno ottenuta in maniera **induttiva** sulla scorta di dati sperimentali. 
 - Definisce la probabilità anche se i possibili risultati dell’esperimento non sono **egualmente probabili**.
 *Svantaggi*:
 - Da un punto di vista matematico *il limite non si può calcolare analiticamente*.
 - Da un punto di vista pratico non è sempre possibile osservare un fenomeno per un numero di volte *sufficientemente grande*.
## Definizione assiomatica della probailità
>[!assiomi]
>Assioma 1 - **Normalizzazione** $$ P[\Omega] = 1 $$
>Assioma 2 - **Non negatività** $$ P[A] \geq 0 $$
>Assioma 3 - **Additività** $$ P[A \cup B] = P[A] + P[B] $$
>solo se $A \cap B = \emptyset$ 

da questi assiomi si possono dimostrare delle proprietà:
1. $P[A^c] = 1 - P[A], \forall A$
2. $P[\emptyset] = 0$ 
3. Se $B \subset A \Rightarrow P[B] \leq P[A]$ 
4. $\{A_1, A_2, ..., A_N\} : \bigcup_{i = 1}^{N} A_i = A \Rightarrow \sum_{i = 1}^{N} P[A_i] = 1$
5. $P[A \cup B] = P[A] + P[B] - P[A \cap B]$

Questa definizione assiomatica obbliga a specificare in modo corretto un *esperimento causale* per il quale bisogna fornire la terna $(\Omega, S,P( \cdot ))$ dove:
- $\Omega$ è lo spazio campione di cardinalità $M$
- $S$ è una classe di sottoinsiemi di $\Omega$ detti *eventi* di cardinalità $2^M$
- $P(\cdot)$ detta **probabiltà** definita sugli eventi di $S$ che associa ad ogni evento $A$ di $S$ un numero non negativo.

# Probabiltà condizionata
Prendiamo in esame in esperimento che coinvolge due eventi $A$ e $B$. Sia $P[A|B]$ la probabilità di $A$ dato che $B$ si è verificato.
>[!Probabilità condizionata]
>$$P[A|B] = \frac{P[A \cap B]}{P[B]}$$

Abbiamo quindi definito un nuovo sistema di probabilità cambiandone la legge ma lasciando invariato lo spazio campione e la classe di eventi. Possiamo quindi verificare i precedenti assiomi:
- *Normalizzazione*: $P[\Omega|B] = \frac{P[\Omega \cap B]}{P[B]} = 1$
- *Non negatività*: $P[A \cap B] \geq 0$,  $P[B] \geq 0$ quindi $P[A|B] \geq 0$ 
- *Additività*: dati $A_1 \cap A_2 = \emptyset$, $P[(A_1 \cup A_2) | B] = P[A_1 | B] + P[A_2 | B]$ 
e definiano le seguenti proprietà: 
- se $A \cap B = \emptyset$ allora $P[A|B] = 0$ 
- se $A \subset B$ allora $A \cap B = A$ quindi $\frac{P[A]}{P[B]} \geq P[A]$ 
- se $B \subset A$ allora $P[A|B] = 1$
- 
In termini di frequenza relativa possiamo dire che: 
$$ P[A|B] = \frac{P[A \cap B]}{P[B]} = \frac{n_{A \cap B}}{n_b} $$

dove $n_{A \cap B}$ è il numero di volte in cui si verificano entrambi gli eventi.

# Teorema della probabiltà totale
>[! Teorema della probabiltà totale]
>dati $\{A_1, A_2, ..., A_k\}$ partizione di $\Omega$ tale che $\bigcup_{i = 1}^{k} A_i = \Omega$  e $A_i \cap A_j = \emptyset,\forall i \neq j$ allora $$ \begin{matrix} B = \bigcup_{i = 1}^{k} (B \cap A_i) \\ P[B] = \sum_{i = 1}^{k} P[B|A_i]\cdot P[A_i]\end{matrix} $$


# Formula di Bayes
>[!Formula di Bayes]
>Noti $P[A|B], P[A], P[B]$ allora $$ P[B|A] = \frac{P[A|B] \cdot P[B]}{P[A]} $$
se gli eventi sono indipendenti si ha che $$ P[B|A] = P[B] \Rightarrow P[A \cap B] = P[A] \cdot P[B]$$


