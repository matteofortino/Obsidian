#uni 
**LTSpice**(*Simulation Program with Integrated Circuit Emphasis*) 

Nelle prime versioni di *spice* l'input testuale si chiama *netlist* ed è un file testuale proprio come l'output. 
Data una rete simulata restituisce il valore delle tensioni e delle correnti su quel circuito.

In nuove versioni commerciali come *LTSpice* si possono inserire input per via *grafica* e anolagamente ottenere un *output grafico*.

Tre tipi di elaborazione: 
1. *Analisi statica*: soluzione di un sistema di equazioni non lineare.
2. *Analisi dinamica*: soluzione di un sistema di equazioni integro-differenziali lineare che diventa lineare nel dominio "s".
3. *Analisi in transitorio*: soluzione di un sistema di equazioni integro-differenziali non lineare.

# Struttura della netlist
È formata da: 
- Statement di apertura contenente il nome del circuito
- Statement di descrizione della topologia del circuito
- Direttive per specificare l'analisi da compiere
- Direttive per specificare il formato dei dati in uscita
- Direttiva .END per chiudere
I commenti sono preceduti dal carattre "\*"

## Prefissi dimensionali
- P (pico) -12
- N (nano) -9
- U (micro) -6
- M (milli) -3
- K (kilo) 3
- MEG (mega) 6
## Direttive
- .OP: determina il punto di riposo
- .AC (DEC, OCT, LIN) NP fstart fstop 
- .TRAN tstep tstop (tstart, tmax, UIC)
- .PRINT type var1 var2 ... 
Noi useremo direttive grafiche e non testuali
