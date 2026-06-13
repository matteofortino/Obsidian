# Guida Completa alle ACL su Cisco Packet Tracer

## Cosa sono le ACL

Le Access Control List (ACL) sono liste di regole (Access Control Entries, ACE) che permettono o negano il traffico in base a criteri come indirizzo IP sorgente/destinazione, protocollo e porta. Vengono applicate alle interfacce dei router in direzione **in** (ingresso) o **out** (uscita).

Regole fondamentali:
- Le ACE vengono valutate **in ordine** dall'alto verso il basso; alla prima corrispondenza il processo si ferma.
- Esiste sempre una **deny implicita** alla fine di ogni ACL (`deny any` / `deny ip any any`), anche se non scritta.
- Una ACL senza almeno una `permit` blocca **tutto** il traffico.
- Le **standard ACL** (numeri 1-99, 1300-1999) filtrano solo per IP sorgente.
- Le **extended ACL** (numeri 100-199, 2000-2699, o con nome) filtrano per IP sorgente/destinazione, protocollo, porte.
- Vanno applicate il più vicino possibile alla **destinazione** per le extended, e il più vicino possibile alla **sorgente** per le standard (best practice generale, non obbligo).

---

## Wildcard Mask

La wildcard mask è l'inverso bit a bit della subnet mask. Indica quali bit devono corrispondere esattamente (0) e quali sono "don't care" (1).

| Subnet Mask | Wildcard Mask |
|---|---|
| 255.255.255.255 | 0.0.0.0 |
| 255.255.255.0 | 0.0.0.255 |
| 255.255.0.0 | 0.0.255.255 |
| 255.0.0.0 | 0.255.255.255 |
| 0.0.0.0 | 255.255.255.255 |

Calcolo veloce: `255 - byte_subnet = byte_wildcard` per ogni ottetto.

Esempi pratici:
- Una singola host `192.168.1.10`: wildcard `0.0.0.0` (oppure si usa la keyword `host`)
- Una rete `192.168.1.0/24`: wildcard `0.0.0.255`
- Tutto il traffico: si usa la keyword `any` (equivale a `0.0.0.0 255.255.255.255`)
- Una rete `172.16.0.0/16`: wildcard `0.0.255.255`
- Una porzione di rete `192.168.1.0/26` (primi 64 host): wildcard `0.0.0.63`

---

## ACL Standard

### Sintassi numerata (1-99 / 1300-1999)

```
Router(config)# access-list [numero] [permit|deny] [sorgente] [wildcard-mask] [log]
```

Esempi:

```
! Permette solo l'host 192.168.10.5
Router(config)# access-list 10 permit host 192.168.10.5

! Permette tutta la rete 192.168.10.0/24
Router(config)# access-list 10 permit 192.168.10.0 0.0.0.255

! Nega un singolo host, ma alla fine c'è una deny implicita su tutto
Router(config)# access-list 10 deny host 192.168.10.5
Router(config)# access-list 10 permit any
```

### Sintassi nominata

```
Router(config)# ip access-list standard [NOME]
Router(config-std-nacl)# permit 192.168.10.0 0.0.0.255
Router(config-std-nacl)# deny any
```

### Applicazione all'interfaccia

```
Router(config)# interface g0/0
Router(config-if)# ip access-group 10 [in|out]
```

oppure con nome:

```
Router(config-if)# ip access-group NOME_ACL in
```

---

## ACL Extended

### Sintassi numerata (100-199 / 2000-2699)

```
Router(config)# access-list [numero] [permit|deny] [protocollo] [sorgente] [wildcard-sorgente] [destinazione] [wildcard-destinazione] [operatore porta] [established] [log]
```

Protocolli comuni: `ip`, `tcp`, `udp`, `icmp`, `eigrp`, `ospf`, `gre`.

Operatori porta: `eq` (uguale), `gt` (maggiore), `lt` (minore), `neq` (diverso), `range` (intervallo).

### Casistiche con esempi

**1. Bloccare il traffico HTTP da una rete a un server specifico**

```
Router(config)# access-list 100 deny tcp 192.168.1.0 0.0.0.255 host 10.0.0.100 eq 80
Router(config)# access-list 100 permit ip any any
```

**2. Permettere solo il traffico Telnet/SSH verso un router da una rete di management**

```
Router(config)# access-list 101 permit tcp 192.168.100.0 0.0.0.255 any eq 22
Router(config)# access-list 101 permit tcp 192.168.100.0 0.0.0.255 any eq 23
Router(config)# access-list 101 deny tcp any any eq 22
Router(config)# access-list 101 deny tcp any any eq 23
Router(config)# access-list 101 permit ip any any
```

**3. Bloccare il ping (ICMP) tra due reti**

```
Router(config)# access-list 102 deny icmp 192.168.1.0 0.0.0.255 192.168.2.0 0.0.0.255 echo
Router(config)# access-list 102 deny icmp 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255 echo-reply
Router(config)# access-list 102 permit ip any any
```

**4. Permettere FTP (porte 20 e 21) da una rete a un server**

```
Router(config)# access-list 103 permit tcp 192.168.1.0 0.0.0.255 host 10.0.0.50 eq 20
Router(config)# access-list 103 permit tcp 192.168.1.0 0.0.0.255 host 10.0.0.50 eq 21
Router(config)# access-list 103 deny ip any any
```

**5. Permettere un range di porte (es. applicazione custom 5000-5010)**

```
Router(config)# access-list 104 permit tcp any host 10.0.0.20 range 5000 5010
Router(config)# access-list 104 deny ip any any
```

**6. Permettere solo il traffico di ritorno per connessioni TCP iniziate dall'interno (`established`)**

```
Router(config)# access-list 105 permit tcp any 192.168.1.0 0.0.0.255 established
Router(config)# access-list 105 deny ip any any
```
La keyword `established` permette i pacchetti con flag ACK o RST impostati, tipici delle risposte a connessioni già iniziate.

**7. Negare l'accesso a una subnet specifica ma permettere tutto il resto**

```
Router(config)# access-list 106 deny ip 192.168.1.0 0.0.0.255 192.168.2.0 0.0.0.255
Router(config)# access-list 106 permit ip any any
```

### Sintassi nominata

```
Router(config)# ip access-list extended NOME_ACL
Router(config-ext-nacl)# permit tcp 192.168.1.0 0.0.0.255 host 10.0.0.100 eq 80
Router(config-ext-nacl)# deny ip any any
```

Vantaggio della sintassi nominata: si possono inserire/rimuovere singole ACE specificando un numero di sequenza, senza riscrivere tutta la ACL.

```
Router(config-ext-nacl)# 5 permit tcp any any eq 443
Router(config-ext-nacl)# no 10
```

### Applicazione all'interfaccia

```
Router(config)# interface g0/1
Router(config-if)# ip access-group 100 out
```

---

## Verifica e Troubleshooting

```
! Mostra tutte le ACL configurate con contatori di match
Router# show access-lists

! Mostra una ACL specifica
Router# show access-lists 100

! Mostra le ACL applicate alle interfacce
Router# show ip interface g0/0

! Mostra la configurazione corrente
Router# show running-config
```

Comandi per rimuovere:

```
! Rimuove l'intera ACL
Router(config)# no access-list 100

! Rimuove la ACL dall'interfaccia
Router(config-if)# no ip access-group 100 in
```

---

## Errori Comuni da Evitare

1. **Dimenticare la `permit` finale**: se serve permettere traffico oltre quello negato, va sempre aggiunta una riga `permit ip any any` (o equivalente), altrimenti la deny implicita blocca tutto.

2. **Ordine delle ACE**: una regola più generica scritta prima di una più specifica può "mangiare" il match prima che si arrivi alla regola specifica. Esempio errato:
```
access-list 100 permit ip any any
access-list 100 deny tcp any any eq 80   ! Questa riga non verrà MAI applicata
```

3. **Wildcard mask sbagliata**: usare la subnet mask al posto della wildcard mask è l'errore più frequente (es. scrivere `255.255.255.0` invece di `0.0.0.255`).

4. **Direzione errata (`in` vs `out`)**: `in` filtra il traffico che entra nell'interfaccia dal segmento collegato, `out` filtra il traffico che esce dall'interfaccia verso il segmento collegato. Va sempre ragionato dal punto di vista del router.

5. **Una sola ACL per interfaccia per protocollo per direzione**: su una stessa interfaccia, per IPv4, si può applicare solo una ACL in ingresso e una in uscita.

6. **ACL extended applicata troppo lontano dalla sorgente**: il traffico viene comunque instradato fino al punto di applicazione, sprecando banda, anche se poi viene scartato.

---

## Esempio Completo: Scenario con 3 Reti

Topologia: `LAN1 (192.168.1.0/24) -- R1 -- LAN2 (192.168.2.0/24)` e `R1 -- Internet (g0/2)`.

Requisiti:
- LAN1 può accedere a tutto.
- LAN2 può accedere solo a Internet (porta 80/443), non a LAN1.
- Nessuno da Internet può iniziare connessioni verso LAN1 o LAN2.

```
! ACL per LAN2 -> blocca accesso a LAN1, permette solo web verso l'esterno
Router(config)# access-list 110 deny ip 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255
Router(config)# access-list 110 permit tcp 192.168.2.0 0.0.0.255 any eq 80
Router(config)# access-list 110 permit tcp 192.168.2.0 0.0.0.255 any eq 443
Router(config)# access-list 110 deny ip any any

! ACL per traffico entrante da Internet -> permette solo risposte a connessioni avviate dall'interno
Router(config)# access-list 120 permit tcp any 192.168.1.0 0.0.0.255 established
Router(config)# access-list 120 permit tcp any 192.168.2.0 0.0.0.255 established
Router(config)# access-list 120 deny ip any any

! Applicazione
Router(config)# interface g0/1
Router(config-if)# description LAN2
Router(config-if)# ip access-group 110 in

Router(config)# interface g0/2
Router(config-if)# description Internet
Router(config-if)# ip access-group 120 in
```
