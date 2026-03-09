#uni
Quando un nuovo processo prende il controllo della CPU viene eseguita un serie di operazioni che prende il nome di *cambio di contesto*.
Tali operazioni sono:
1. Salvataggio del contesto del processo in esecuzione nel suo descrittore
2. Inserimento del descrittore nella coda dei processi bloccati o pronti
3. Selezione di un altro processo tra quelli con coda pronti e caricamento del nome di tale processo nel registro processo in esecuzione
4. Caricamento del contesto del nuovo processo nei registri del processore

I punti 1 e 4 sono detti *Salvataggio stato* e *Caricamento stato*
Quindi supponiamo che ci sia un processo P0 in esecuzione e che venga interrotto da un processo P1. Succedono le seguenti operazioni:
1. Lo stato del processo P0 viene salvato
2. Lo stato del processo P1 viene caricato
3. Il processo P1 viene eseguito
4. Lo stato del processo P1 viene salvato
5. Lo stato del processo P0 viene caricato
6. Il processo P0 continua la sua esecuzione

**NOTA BENE**: il cambio di contesto invalida la cache in quanto le informazioni salvate in cache dal processo precendente non e' detto che debbano essere accessibili al processo attualmente in esecuzione.

Un cambio di contensto puo' richedere onerosi trasferimenti da/verso la memoria secondaria per allocare o deallocare gli spazi per gli indirizzi dei processi. 
