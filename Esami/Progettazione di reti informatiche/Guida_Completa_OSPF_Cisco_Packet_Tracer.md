# Guida Completa OSPF su Cisco Packet Tracer

# Indice
1. Cos'è OSPF
2. Concetti fondamentali
3. Prerequisiti
4. Comandi base
5. Configurazione OSPF singola area
6. Configurazione multi-area
7. Router ID
8. Passive Interface
9. Default Route
10. Ridistribuzione (Redistribution)
11. Costi OSPF
12. Reti point-to-point e broadcast
13. Verifiche e troubleshooting
14. Esempi completi
15. Errori comuni

---

# 1. Cos'è OSPF

OSPF (Open Shortest Path First) è un protocollo di routing dinamico di tipo Link-State.

Caratteristiche:

- Standard aperto (RFC)
- Utilizza l'algoritmo SPF (Dijkstra)
- Supporta VLSM e CIDR
- Convergenza veloce
- Supporta aree gerarchiche

---

# 2. Concetti fondamentali

## Area 0

È la backbone area.

Tutte le altre aree devono essere collegate direttamente o logicamente all'Area 0.

Esempio:

```text
Area 1 ---- Area 0 ---- Area 2
```

## Router ID

Identificatore univoco del router.

Formato:

```text
1.1.1.1
2.2.2.2
10.10.10.10
```

## Neighbor

Due router OSPF diventano vicini (neighbors) se:

- Stessa area
- Stessa subnet
- Hello Timer uguale
- Dead Timer uguale
- Stesso tipo di autenticazione

---

# 3. Prerequisiti

Configurare gli indirizzi IP.

Esempio:

```bash
enable
configure terminal

interface gigabitEthernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown

interface gigabitEthernet 0/1
ip address 10.0.0.1 255.255.255.252
no shutdown
```

---

# 4. Comandi base OSPF

Entrare nel processo OSPF:

```bash
router ospf 1
```

Dove:

```text
1 = Process ID
```

Il Process ID è locale al router.

---

# 5. Configurazione OSPF Singola Area

## Topologia

```text
R1 ----- R2 ----- R3
```

### R1

```bash
router ospf 1
network 192.168.1.0 0.0.0.255 area 0
network 10.0.12.0 0.0.0.3 area 0
```

### R2

```bash
router ospf 1
network 10.0.12.0 0.0.0.3 area 0
network 10.0.23.0 0.0.0.3 area 0
```

### R3

```bash
router ospf 1
network 10.0.23.0 0.0.0.3 area 0
network 192.168.3.0 0.0.0.255 area 0
```

---

# 6. Wildcard Mask

OSPF usa wildcard mask e NON subnet mask.

Formula:

```text
255.255.255.255
-
Subnet Mask
=
Wildcard
```

Esempi:

| Subnet Mask | Wildcard |
|------------|-----------|
|255.255.255.0|0.0.0.255|
|255.255.255.252|0.0.0.3|
|255.255.0.0|0.0.255.255|

---

# 7. Configurazione tramite Interface

Versioni IOS più recenti:

```bash
interface g0/0
ip ospf 1 area 0
```

oppure

```bash
interface serial0/0/0
ip ospf 1 area 0
```

---

# 8. Router ID

Configurazione manuale:

```bash
router ospf 1
router-id 1.1.1.1
```

Applicare:

```bash
clear ip ospf process
```

Verifica:

```bash
show ip ospf
```

---

# 9. Passive Interface

Serve per pubblicizzare una rete senza creare adiacenze.

```bash
router ospf 1
passive-interface g0/0
```

Tutte passive:

```bash
passive-interface default
```

Riattivare:

```bash
no passive-interface g0/1
```

---

# 10. Multi Area OSPF

Esempio:

```text
LAN1
 |
R1
 |
Area 1
 |
R2
 |
Area 0
 |
R3
```

R2 diventa ABR (Area Border Router).

Configurazione:

```bash
router ospf 1
network 10.1.1.0 0.0.0.255 area 1
network 10.0.0.0 0.0.0.3 area 0
```

---

# 11. OSPF Cost

Formula:

```text
Cost = Bandwidth Reference / Bandwidth Interface
```

Visualizzare:

```bash
show ip ospf interface
```

Modificare:

```bash
interface g0/0
ip ospf cost 10
```

---

# 12. Bandwidth

Modifica indiretta del costo.

```bash
interface serial0/0/0
bandwidth 64
```

Verifica:

```bash
show interfaces serial0/0/0
```

---

# 13. Default Route

Creare route:

```bash
ip route 0.0.0.0 0.0.0.0 200.0.0.1
```

Pubblicarla:

```bash
router ospf 1
default-information originate
```

Sempre pubblicata:

```bash
default-information originate always
```

---

# 14. Redistribuzione

Statiche dentro OSPF:

```bash
router ospf 1
redistribute static subnets
```

RIP dentro OSPF:

```bash
router ospf 1
redistribute rip subnets
```

---

# 15. Autenticazione OSPF

## Plain Text

```bash
interface g0/0
ip ospf authentication
ip ospf authentication-key cisco
```

## MD5

```bash
interface g0/0
ip ospf message-digest-key 1 md5 cisco
ip ospf authentication message-digest
```

---

# 16. Timer Hello e Dead

Visualizzazione:

```bash
show ip ospf interface
```

Modifica:

```bash
interface g0/0
ip ospf hello-interval 5
ip ospf dead-interval 20
```

---

# 17. Priorità DR/BDR

Visualizzare:

```bash
show ip ospf neighbor
```

Impostare:

```bash
interface g0/0
ip ospf priority 255
```

Mai DR:

```bash
ip ospf priority 0
```

---

# 18. Network Types

## Broadcast

Ethernet:

```bash
ip ospf network broadcast
```

## Point-to-Point

```bash
ip ospf network point-to-point
```

## NBMA

```bash
ip ospf network non-broadcast
```

---

# 19. Sommario Rotte

ABR:

```bash
router ospf 1
area 1 range 192.168.0.0 255.255.252.0
```

ASBR:

```bash
summary-address 172.16.0.0 255.255.0.0
```

---

# 20. Stub Area

Configurazione:

```bash
router ospf 1
area 1 stub
```

Su tutti i router dell'area.

---

# 21. Totally Stub Area

ABR:

```bash
area 1 stub no-summary
```

Router interni:

```bash
area 1 stub
```

---

# 22. NSSA

ABR:

```bash
area 1 nssa
```

Router interni:

```bash
area 1 nssa
```

Totally NSSA:

```bash
area 1 nssa no-summary
```

---

# 23. Verifiche Principali

Visualizzare vicini:

```bash
show ip ospf neighbor
```

Tabella routing:

```bash
show ip route
```

Database:

```bash
show ip ospf database
```

Interfacce:

```bash
show ip ospf interface
```

Processo:

```bash
show ip protocols
```

---

# 24. Troubleshooting

## Nessun Neighbor

Controllare:

```bash
show ip ospf neighbor
```

Possibili cause:

- Area diversa
- Subnet diversa
- Hello timer diverso
- Dead timer diverso
- Autenticazione diversa

---

## Route mancanti

Controllare:

```bash
show ip route
show ip ospf database
```

---

## Reset processo

```bash
clear ip ospf process
```

---

# 25. Esempio Completo da Esame

R1:

```bash
enable
configure terminal

interface g0/0
ip address 192.168.1.1 255.255.255.0
no shutdown

interface s0/0/0
ip address 10.0.12.1 255.255.255.252
clock rate 64000
no shutdown

router ospf 1
router-id 1.1.1.1
network 192.168.1.0 0.0.0.255 area 0
network 10.0.12.0 0.0.0.3 area 0
```

R2:

```bash
router ospf 1
router-id 2.2.2.2
network 10.0.12.0 0.0.0.3 area 0
network 10.0.23.0 0.0.0.3 area 0
```

R3:

```bash
router ospf 1
router-id 3.3.3.3
network 10.0.23.0 0.0.0.3 area 0
network 192.168.3.0 0.0.0.255 area 0
```

Verifica:

```bash
show ip ospf neighbor
show ip route
show ip ospf database
```

---

# 26. Comandi da Ricordare per Verifiche ed Esami

```bash
router ospf 1
network X.X.X.X W.W.W.W area N

show ip ospf
show ip ospf neighbor
show ip ospf database
show ip ospf interface
show ip route
show ip protocols

router-id X.X.X.X

passive-interface
no passive-interface

default-information originate

area stub
area nssa

ip ospf cost

clear ip ospf process
```
