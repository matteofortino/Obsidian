#uni
# Concorrenza
Con il termine *processi concorrenti* viene indicato un insieme di processi la cui esecuzione si sovrappone nel tempo.
In un sistema *multiprocessore* puo' accadere che un processo inizio prima della fine di una altro (*overlapping*).
In un sistema *monoprocessore* puo' accadere che due processi "spezzino" la loro esecuzione (*interleaving*).

I processi concorrenti posso comportarsi come:
- *Indipendenti*
- *Interagenti*
Un insieme di processi si dice *indipendente* quando nessuno di esse puo' influenzare l'esecuzione degli altri. Processi che non condividono dati e che non si scambiano informazioni sono indipendenti.
Il comportamento di un processo indipendente e' *riproducibile* cioe', mantenendo gli stessi dati in ingresso il programma produce sempre lo stesso risultato in qualunque momento o condizione sia eseguito.

I processi *interagenti* hanno la possibilita' di *influenzarsi vicendevolmente* durante l'esecuzione. Questo puo' avvenire in due modi:
- *Esplicito*: mediante scambi di messagi o segnali temporali (*cooperazione*)
- *Implicito*: tramite *competizione* per la stessa risorsa
