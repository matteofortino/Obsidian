# Guida Completa STP (Spanning Tree Protocol) su Cisco Packet Tracer

## Indice
1. [Concetti base](#concetti-base)
2. [Tipi di STP](#tipi-di-stp)
3. [Comandi di verifica](#comandi-di-verifica)
4. [Configurazione VLAN e Trunk (prerequisiti)](#configurazione-vlan-e-trunk-prerequisiti)
5. [Configurazione PVST+ / Root Bridge](#configurazione-pvst--root-bridge)
6. [Modifica Priority manuale](#modifica-priority-manuale)
7. [Modifica Cost delle porte](#modifica-cost-delle-porte)
8. [Modifica Port-ID (porta secondaria)](#modifica-port-id-porta-secondaria)
9. [PortFast](#portfast)
10. [BPDU Guard](#bpdu-guard)
11. [BPDU Filter](#bpdu-filter)
12. [Root Guard](#root-guard)
13. [Loop Guard / UDLD](#loop-guard--udld)
14. [Rapid PVST+ (802.1w)](#rapid-pvst-8021w)
15. [MST - Multiple Spanning Tree (802.1s)](#mst---multiple-spanning-tree-8021s)
16. [EtherChannel e STP](#etherchannel-e-stp)
17. [Troubleshooting comuni](#troubleshooting-comuni)
18. [Tabella riassuntiva comandi](#tabella-riassuntiva-comandi)

---

## Concetti base

STP (IEEE 802.1D) previene i loop a livello 2 in reti con collegamenti ridondanti, bloccando dinamicamente le porte ridondanti.

**Elementi chiave:**
- **Root Bridge**: switch con il Bridge ID più basso (Priority + MAC)
- **Bridge ID (BID)** = Priority (default 32768) + MAC Address
- **Stati delle porte (STP classico)**: Blocking → Listening → Learning → Forwarding (Disabled)
- **Ruoli delle porte**: Root Port (RP), Designated Port (DP), Non-Designated/Blocking Port

**Costi di default (porte):**

| Velocità | Costo (802.1D) | Costo (Long, 802.1t) |
|----------|----------------|----------------------|
| 10 Mbps  | 100            | 2.000.000            |
| 100 Mbps | 19             | 200.000              |
| 1 Gbps   | 4              | 20.000               |
| 10 Gbps  | 2              | 2.000                |

---

## Tipi di STP

| Tipo | Standard | Note |
|------|----------|------|
| STP | IEEE 802.1D | Originale, lento (50s convergenza) |
| PVST+ | Cisco proprietario | Un'istanza STP per ogni VLAN |
| Rapid-PVST+ | IEEE 802.1w | Convergenza rapida (~6s), un'istanza per VLAN |
| MST | IEEE 802.1s | Più VLAN mappate su poche istanze |

> **Nota Packet Tracer**: di default gli switch Cisco in PT usano **PVST+**. Si può passare a Rapid-PVST+ o MST.

---

## Comandi di verifica

Da eseguire in modalità privilegiata (`enable`) o anche in configurazione, utili per controllare lo stato prima/dopo le modifiche.

```
Switch> enable
Switch# show spanning-tree
Switch# show spanning-tree vlan 10
Switch# show spanning-tree summary
Switch# show spanning-tree interface fastEthernet 0/1
Switch# show spanning-tree root
Switch# show spanning-tree bridge
Switch# show interfaces trunk
Switch# show vlan brief
```

**Esempio output `show spanning-tree vlan 1`:**
```
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.42B5.A001
             Cost        19
             Port        1 (FastEthernet0/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     00D0.D3A4.5B02
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Root FWD 19        128.1    P2p
Fa0/2            Desg FWD 19        128.2    P2p
Fa0/3            Altn BLK 19        128.3    P2p
```

---

## Configurazione VLAN e Trunk (prerequisiti)

Prima di configurare STP è utile avere VLAN e trunk attivi tra switch.

```
Switch> enable
Switch# configure terminal

! Creazione VLAN
Switch(config)# vlan 10
Switch(config-vlan)# name VENDITE
Switch(config-vlan)# exit

! Configurazione porta trunk
Switch(config)# interface fastEthernet 0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan 1,10,20
Switch(config-if)# exit
```

---

## Configurazione PVST+ / Root Bridge

Per default PVST+ è già attivo. Per impostare manualmente il Root Bridge:

### Metodo 1: comando `spanning-tree vlan X root primary`

```
SW1(config)# spanning-tree vlan 1 root primary
SW1(config)# spanning-tree vlan 10 root primary
```

Questo comando abbassa automaticamente la priority a 24576 (o un valore inferiore al root attuale).

### Metodo 2: comando `root secondary` (backup root)

```
SW2(config)# spanning-tree vlan 1 root secondary
```

Imposta la priority a 28672, da usare su uno switch di backup.

**Esempio completo: SW1 root per VLAN 1 e 10, SW2 backup**
```
SW1(config)# spanning-tree vlan 1,10 root primary
SW2(config)# spanning-tree vlan 1,10 root secondary
```

---

## Modifica Priority manuale

Range valido: **0 - 61440**, multipli di **4096**.

```
SW1(config)# spanning-tree vlan 1 priority 4096
```

**Valori multipli accettati:**
```
0, 4096, 8192, 12288, 16384, 20480, 24576, 28672, 32768 (default), 36864, 40960, 45056, 49152, 53248, 57344, 61440
```

**Esempio - rendere SW3 root per VLAN 20:**
```
SW3(config)# spanning-tree vlan 20 priority 0
```
> Priority 0 = massima priorità possibile (vince sempre come root, a parità di MAC).

---

## Modifica Cost delle porte

Usato per influenzare quale porta diventa Root Port quando ci sono priority uguali.

```
SW2(config)# interface fastEthernet 0/2
SW2(config-if)# spanning-tree cost 5
SW2(config-if)# exit
```

**Per ripristinare il valore di default:**
```
SW2(config-if)# no spanning-tree cost
```

**Specificare il cost per singola VLAN (PVST+):**
```
SW2(config-if)# spanning-tree vlan 10 cost 15
```

**Esempio scenario**: due collegamenti da SW2 verso il Root Bridge, entrambi FastEthernet (cost 19 di default). Per forzare l'utilizzo della porta Fa0/1 come Root Port:
```
SW2(config)# interface fastEthernet 0/2
SW2(config-if)# spanning-tree cost 25
SW2(config-if)# exit
```
Ora Fa0/1 (cost 19) avrà cost totale minore di Fa0/2 (cost 25) e verrà eletta Root Port.

---

## Modifica Port-ID (porta secondaria)

Quando cost e priority del bridge sono identici, si usa la **Port Priority** (default 128) per decidere la porta. Range: **0-240**, multipli di **16**.

```
SW2(config)# interface fastEthernet 0/1
SW2(config-if)# spanning-tree port-priority 96
SW2(config-if)# exit
```

**Per VLAN specifica:**
```
SW2(config-if)# spanning-tree vlan 10 port-priority 64
```

---

## PortFast

Da abilitare **solo su porte access connesse a host finali** (PC, server), mai su porte verso altri switch, per evitare loop temporanei.

### Su singola interfaccia
```
Switch(config)# interface fastEthernet 0/3
Switch(config-if)# spanning-tree portfast
Switch(config-if)# exit
```

### Su tutte le porte (globale - sconsigliato senza criterio)
```
Switch(config)# spanning-tree portfast default
```

**Verifica:**
```
Switch# show running-config interface fastEthernet 0/3
```
Output atteso: presenza della riga `spanning-tree portfast`.

**Messaggio di warning tipico in PT:**
```
%Warning: portfast should only be enabled on ports connected to a single
host. Connecting hubs, concentrators, switches, bridges, etc. to this
interface when portfast is enabled, can cause temporary bridging loops.
Use with CAUTION
```

---

## BPDU Guard

Disabilita (err-disable) una porta PortFast se riceve un BPDU, prevenendo che venga collegato uno switch non autorizzato.

### Su singola interfaccia (richiede PortFast attivo)
```
Switch(config)# interface fastEthernet 0/3
Switch(config-if)# spanning-tree portfast
Switch(config-if)# spanning-tree bpduguard enable
Switch(config-if)# exit
```

### Globale (si applica solo a porte PortFast)
```
Switch(config)# spanning-tree portfast bpduguard default
```

**Per riportare una porta in err-disable allo stato up:**
```
Switch(config)# interface fastEthernet 0/3
Switch(config-if)# shutdown
Switch(config-if)# no shutdown
Switch(config-if)# exit
```

**Verifica stato porta:**
```
Switch# show interfaces fastEthernet 0/3 status
```
Output: `err-disabled` nella colonna Status se il BPDU Guard ha agito.

---

## BPDU Filter

Impedisce l'invio/ricezione di BPDU sulla porta (la porta non partecipa più a STP). Usare con cautela.

### Su singola interfaccia
```
Switch(config)# interface fastEthernet 0/4
Switch(config-if)# spanning-tree bpdufilter enable
Switch(config-if)# exit
```

### Globale (solo su porte PortFast)
```
Switch(config)# spanning-tree portfast bpdufilter default
```

---

## Root Guard

Impedisce che una porta diventi Root Port, proteggendo da switch esterni che potrebbero diventare root indesiderati. Se riceve BPDU "superiori", la porta va in stato `root-inconsistent`.

```
Switch(config)# interface fastEthernet 0/5
Switch(config-if)# spanning-tree guard root
Switch(config-if)# exit
```

**Verifica:**
```
Switch# show spanning-tree inconsistentports
```

**Rimozione:**
```
Switch(config-if)# no spanning-tree guard
```

---

## Loop Guard / UDLD

Protegge da loop causati da link unidirezionali.

### Loop Guard su singola interfaccia
```
Switch(config)# interface fastEthernet 0/6
Switch(config-if)# spanning-tree guard loop
Switch(config-if)# exit
```

### Loop Guard globale (solo porte non edge, non root)
```
Switch(config)# spanning-tree loopguard default
```

> Nota: il supporto a UDLD può essere limitato in Packet Tracer rispetto a un IOS reale.

---

## Rapid PVST+ (802.1w)

Convergenza molto più rapida rispetto a STP classico (secondi invece di 30-50 secondi). Stati porta: Discarding, Learning, Forwarding.

### Attivazione globale
```
Switch(config)# spanning-tree mode rapid-pvst
```

> Va configurato **su tutti gli switch** della rete per coerenza, anche se è retrocompatibile (uno switch in Rapid-PVST+ può comunicare con uno in PVST+, ma senza i benefici della rapidità su quel segmento).

**Verifica modalità attiva:**
```
Switch# show spanning-tree summary
```
Output:
```
Switch is in rapid-pvst mode
Root bridge for: VLAN0001
...
```

**Esempio configurazione completa Rapid-PVST+ con root primario:**
```
SW1(config)# spanning-tree mode rapid-pvst
SW1(config)# spanning-tree vlan 1 root primary

SW2(config)# spanning-tree mode rapid-pvst
SW2(config)# spanning-tree vlan 1 root secondary

SW3(config)# spanning-tree mode rapid-pvst
```

---

## MST - Multiple Spanning Tree (802.1s)

Permette di mappare più VLAN su una singola istanza STP, riducendo il carico computazionale.

### Configurazione base

```
Switch(config)# spanning-tree mode mst
Switch(config)# spanning-tree mst configuration
Switch(config-mst)# name REGIONE1
Switch(config-mst)# revision 1
Switch(config-mst)# instance 1 vlan 10,20
Switch(config-mst)# instance 2 vlan 30,40
Switch(config-mst)# exit
```

> **IMPORTANTE**: tutti gli switch della stessa regione MST devono avere **stesso nome, stessa revisione e stessa mappatura VLAN->istanza**, altrimenti vengono considerati regioni MST diverse.

### Impostare root per un'istanza MST
```
Switch(config)# spanning-tree mst 1 root primary
Switch(config)# spanning-tree mst 1 priority 4096
```

### Verifica
```
Switch# show spanning-tree mst
Switch# show spanning-tree mst configuration
Switch# show spanning-tree mst 1
```

**Esempio output `show spanning-tree mst configuration`:**
```
Name      [REGIONE1]
Revision  1     Instances configured 3

Instance  Vlans mapped
--------  ---------------------------------------------------------------
0         1,5,11-19,21-29,31-39,41-4094
1         10,20
2         30,40
```

---

## EtherChannel e STP

Aggregare più link fisici in un unico link logico, visto come una sola porta da STP (evita che STP blocchi i link ridondanti tra gli stessi due switch).

### Configurazione (PAgP - Cisco) su entrambi gli switch
```
SW1(config)# interface range fastEthernet 0/1 - 2
SW1(config-if-range)# channel-group 1 mode desirable
SW1(config-if-range)# exit

SW1(config)# interface port-channel 1
SW1(config-if)# switchport mode trunk
SW1(config-if)# exit
```

### Configurazione (LACP - standard 802.3ad)
```
SW1(config)# interface range fastEthernet 0/1 - 2
SW1(config-if-range)# channel-protocol lacp
SW1(config-if-range)# channel-group 1 mode active
SW1(config-if-range)# exit
```

### Verifica
```
SW1# show etherchannel summary
SW1# show spanning-tree
```
> In output, l'interfaccia comparirà come `Po1` (Port-channel 1) invece delle singole Fa0/1 e Fa0/2.

---

## Troubleshooting comuni

| Problema | Possibile causa | Comando di verifica/soluzione |
|----------|------------------|-------------------------------|
| Porta in stato BLK su entrambe le estremità di un link | Loop fisico ridondante, comportamento normale STP | `show spanning-tree` - verificare ruoli (Altn/Backup) |
| Porta in `err-disabled` | BPDU Guard ha rilevato BPDU su porta access | `shutdown` + `no shutdown` sull'interfaccia |
| Root Bridge inaspettato | Priority più bassa su switch errato | `show spanning-tree root`, poi `spanning-tree vlan X priority` |
| Convergenza lenta (30-50s) | STP classico (802.1D) | Migrare a `spanning-tree mode rapid-pvst` |
| Switch non riceve BPDU | BPDU Filter attivo per errore | `no spanning-tree bpdufilter enable` |
| Topologia STP differente da quanto previsto | Mismatch configurazione MST tra switch | `show spanning-tree mst configuration` su tutti gli switch, verificare nome/revisione/mapping identici |
| Trunk non passa traffico VLAN | VLAN non permessa sul trunk | `show interfaces trunk`, poi `switchport trunk allowed vlan add X` |

---

## Tabella riassuntiva comandi

| Comando | Funzione |
|---------|----------|
| `show spanning-tree` | Mostra stato STP per tutte le VLAN |
| `show spanning-tree vlan X` | Stato STP per una VLAN specifica |
| `show spanning-tree summary` | Riepilogo modalità STP e root bridge |
| `show spanning-tree root` | Info sul root bridge |
| `show spanning-tree interface X` | Stato STP per una specifica interfaccia |
| `spanning-tree vlan X priority N` | Imposta priority bridge per VLAN X |
| `spanning-tree vlan X root primary` | Rende lo switch root per VLAN X |
| `spanning-tree vlan X root secondary` | Rende lo switch backup root per VLAN X |
| `spanning-tree cost N` | Imposta cost della porta |
| `spanning-tree vlan X cost N` | Imposta cost porta per VLAN X |
| `spanning-tree port-priority N` | Imposta priorità della porta |
| `spanning-tree portfast` | Abilita PortFast su interfaccia |
| `spanning-tree portfast default` | Abilita PortFast su tutte le interfacce |
| `spanning-tree bpduguard enable` | Abilita BPDU Guard su interfaccia |
| `spanning-tree portfast bpduguard default` | BPDU Guard globale su porte PortFast |
| `spanning-tree bpdufilter enable` | Abilita BPDU Filter su interfaccia |
| `spanning-tree guard root` | Abilita Root Guard |
| `spanning-tree guard loop` | Abilita Loop Guard |
| `spanning-tree mode pvst` | Imposta modalità PVST+ (default) |
| `spanning-tree mode rapid-pvst` | Imposta modalità Rapid-PVST+ |
| `spanning-tree mode mst` | Imposta modalità MST |
| `spanning-tree mst configuration` | Entra in configurazione MST |
| `instance X vlan Y` | Mappa VLAN Y su istanza MST X |
| `spanning-tree mst X root primary` | Root per istanza MST X |
| `channel-group N mode desirable/active` | Crea EtherChannel (PAgP/LACP) |

---

## Esempio scenario completo (3 switch, ring topology)

**Topologia**: SW1 - SW2 - SW3 - SW1 (anello), VLAN 1 e VLAN 10.

```
! ===== SW1 =====
SW1> enable
SW1# configure terminal
SW1(config)# vlan 10
SW1(config-vlan)# exit
SW1(config)# interface range fastEthernet 0/1 - 2
SW1(config-if-range)# switchport mode trunk
SW1(config-if-range)# exit
SW1(config)# spanning-tree mode rapid-pvst
SW1(config)# spanning-tree vlan 1,10 root primary
SW1(config)# end

! ===== SW2 =====
SW2> enable
SW2# configure terminal
SW2(config)# vlan 10
SW2(config-vlan)# exit
SW2(config)# interface range fastEthernet 0/1 - 2
SW2(config-if-range)# switchport mode trunk
SW2(config-if-range)# exit
SW2(config)# spanning-tree mode rapid-pvst
SW2(config)# spanning-tree vlan 1,10 root secondary
SW2(config)# end

! ===== SW3 =====
SW3> enable
SW3# configure terminal
SW3(config)# vlan 10
SW3(config-vlan)# exit
SW3(config)# interface range fastEthernet 0/1 - 2
SW3(config-if-range)# switchport mode trunk
SW3(config-if-range)# exit
SW3(config)# spanning-tree mode rapid-pvst
SW3(config)# end
```

**Verifica finale (su SW3, che dovrebbe avere una porta bloccata/alternate):**
```
SW3# show spanning-tree vlan 1
SW3# show spanning-tree vlan 10
```

Una delle due porte trunk di SW3 risulterà in stato `Altn` (Alternate) / Discarding per evitare il loop nell'anello.
