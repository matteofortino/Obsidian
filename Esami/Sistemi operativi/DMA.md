---
id: DMA
aliases: []
tags: []
---
#uni
# Introduzione
Il **DMA (Direct Memory Access)** è una tecnica che permette a dispositivi periferici (es. dischi, schede di rete, schede audio) di trasferire dati direttamente dalla/alla memoria principale senza coinvolgere la CPU in ogni singola operazione di I/O.  
L’obiettivo è **ridurre il carico della CPU** e aumentare l’efficienza dei trasferimenti dati.

# Motivazioni

- Senza DMA, ogni trasferimento I/O → memoria richiede il coinvolgimento diretto della CPU (tecnica _Programmed I/O_ o _Interrupt-driven I/O_).
    
- Con DMA, il dispositivo comunica con la memoria in modo autonomo, mentre la CPU può occuparsi di altre operazioni.
    
- Fondamentale quando:
    - La quantità di dati è elevata (es. trasferimento di blocchi da disco).
    - È richiesta alta velocità (es. streaming audio/video).
    - Si vuole minimizzare la latenza e il tempo morto della CPU.
# Architettura di un sistema con DMA

Il sistema include un **DMA Controller (DMAC)**, che gestisce i trasferimenti.  
Componenti principali:
- **CPU**: inizializza il trasferimento (setta registri del DMAC: indirizzo sorgente, indirizzo destinazione, numero di parole da trasferire, direzione).
- **DMA Controller**: coordina il flusso dati tra periferica e memoria.
- **Bus di sistema**: condiviso tra CPU e DMAC (arbitraggio necessario).
- **Periferiche**: dispositivi I/O che richiedono trasferimenti.
# Modalità di funzionamento del DMA
1. **Burst Mode (o Block Transfer)**
    - Il DMAC prende il controllo del bus e trasferisce un intero blocco di dati.
    - Pro: molto veloce.
    - Contro: la CPU rimane in attesa senza accesso al bus.
2. **Cycle Stealing**
    - Il DMAC "ruba" cicli di bus alla CPU.
    - La CPU e il DMAC si alternano nell’uso del bus.
    - Pro: la CPU continua a lavorare (più lenta, ma non ferma).
3. **Transparent Mode (o "Hidden DMA")**
    - Il trasferimento avviene solo quando la CPU non utilizza il bus.
    - Pro: nessun impatto sulle prestazioni della CPU.
    - Contro: trasferimento più lento.
# Sequenza tipica di un trasferimento DMA
1. La **CPU inizializza** il DMAC (indirizzi, dimensione del blocco, direzione).
2. La periferica segnala una richiesta di DMA al DMAC.
3. Il DMAC richiede il controllo del bus (tramite segnale _HOLD_).
4. La CPU rilascia il bus (rispondendo con _HLDA_ = Hold Acknowledge).
5. Il DMAC gestisce il trasferimento direttamente memoria ↔ periferica.
6. Al termine, il DMAC notifica la CPU tramite **interrupt**.
# Tipi di trasferimento DMA
- **Memoria ↔ Periferica**: il caso più comune.
- **Memoria ↔ Memoria**: usato in sistemi evoluti, per spostare dati senza CPU.
- **Periferica ↔ Periferica**: raro, ma possibile in alcune architetture.
# Vantaggi del DMA
- Riduce notevolmente il carico della CPU.
- Aumenta la velocità dei trasferimenti massivi.
- Ottimizza le prestazioni in applicazioni real-time (audio, video, rete).
# Svantaggi e complessità
- Richiede **hardware dedicato** (DMAC).
- Aumenta la complessità dell’arbitraggio del bus.
- Possibili problemi di **coerenza della cache** (la CPU potrebbe avere dati in cache non aggiornati).
- Gestione degli **interrupt** e sincronizzazione più complessa.
# Esempi pratici
- Trasferimento di blocchi da **hard disk/SSD alla RAM**.
- Acquisizione audio/video in tempo reale.
- Trasferimento di pacchetti da **scheda di rete alla memoria**.
- Sistemi embedded con microcontrollori (es. ARM Cortex-M ha controller DMA integrati).
# Conclusione

Il DMA è una tecnica chiave per i sistemi moderni che richiedono **alta velocità e parallelismo** nell’I/O. Comprendere il suo funzionamento è fondamentale in architettura dei calcolatori e nei sistemi embedded.
