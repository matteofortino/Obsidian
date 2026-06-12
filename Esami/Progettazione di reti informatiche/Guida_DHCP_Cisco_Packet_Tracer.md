# DHCP su Cisco Packet Tracer – Guida Completa

## Indice
1. Cos'è il DHCP
2. Configurazione DHCP su Router Cisco
3. Configurazione DHCP su Server
4. DHCP Relay (`ip helper-address`)
5. Esclusione indirizzi
6. Più pool DHCP
7. DHCP per più VLAN
8. Verifica e troubleshooting
9. Comandi utili
10. Esempi completi
11. Errori comuni

---

# 1. Cos'è il DHCP

DHCP (Dynamic Host Configuration Protocol) assegna automaticamente:

- Indirizzo IP
- Subnet Mask
- Gateway predefinito
- DNS Server
- Altri parametri di rete

Senza DHCP, ogni host deve essere configurato manualmente.

---

# 2. Configurazione DHCP su Router Cisco

## Scenario

Rete: 192.168.1.0/24

Router:
- IP interfaccia: 192.168.1.1

Client:
- DHCP automatico

## Passo 1 – Configurare l'interfaccia

```cisco
enable
configure terminal

interface fastethernet 0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown

exit
```

## Passo 2 – Escludere indirizzi

```cisco
ip dhcp excluded-address 192.168.1.1
```

Più indirizzi:

```cisco
ip dhcp excluded-address 192.168.1.1 192.168.1.20
```

## Passo 3 – Creare il pool DHCP

```cisco
ip dhcp pool LAN
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 8.8.8.8
exit
```

---

# 3. Configurazione DHCP tramite Server

Packet Tracer contiene un Server dedicato.

## Procedura

1. Inserire un Server.
2. Desktop → IP Configuration.
3. Impostare IP statico.
4. Services → DHCP.
5. Attivare DHCP.

Esempio:

Pool Name:
```
LAN
```

Default Gateway:
```
192.168.1.1
```

DNS:
```
8.8.8.8
```

Start IP:
```
192.168.1.100
```

Subnet Mask:
```
255.255.255.0
```

Maximum Users:
```
100
```

---

# 4. DHCP Relay (ip helper-address)

## Quando serve

Il server DHCP si trova in una rete diversa.

Client:
```
192.168.1.0/24
```

Server DHCP:
```
192.168.2.10
```

Router tra le due reti.

## Configurazione

Interfaccia lato client:

```cisco
interface fastethernet 0/0
 ip address 192.168.1.1 255.255.255.0
 ip helper-address 192.168.2.10
 no shutdown
```

Il router inoltra le richieste DHCP.

---

# 5. Esclusione indirizzi

## Singolo indirizzo

```cisco
ip dhcp excluded-address 192.168.1.1
```

## Intervallo

```cisco
ip dhcp excluded-address 192.168.1.1 192.168.1.50
```

Utilissimo per:

- Router
- Switch
- Server
- Stampanti

---

# 6. Più Pool DHCP

## Pool Uffici

```cisco
ip dhcp pool UFFICI
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8
```

## Pool Laboratorio

```cisco
ip dhcp pool LAB
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 8.8.4.4
```

---

# 7. DHCP con VLAN

## Scenario

VLAN 10:
```
192.168.10.0/24
```

VLAN 20:
```
192.168.20.0/24
```

## Subinterfacce

```cisco
interface fa0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

interface fa0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
```

## Pool VLAN 10

```cisco
ip dhcp pool VLAN10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8
```

## Pool VLAN 20

```cisco
ip dhcp pool VLAN20
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 8.8.8.8
```

---

# 8. Verifica

## Visualizzare i pool

```cisco
show ip dhcp pool
```

## Lease assegnati

```cisco
show ip dhcp binding
```

## Statistiche

```cisco
show ip dhcp server statistics
```

## Configurazione

```cisco
show running-config
```

---

# 9. Troubleshooting

## Verificare interfacce

```cisco
show ip interface brief
```

Controllare che siano:

```text
up/up
```

## Verificare pool

```cisco
show ip dhcp pool
```

## Verificare lease

```cisco
show ip dhcp binding
```

## Verificare relay

```cisco
show running-config
```

Cercare:

```cisco
ip helper-address
```

---

# 10. Esempio Completo Base

```cisco
enable
configure terminal

interface fa0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown

ip dhcp excluded-address 192.168.1.1 192.168.1.20

ip dhcp pool LAN
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 8.8.8.8
exit
```

Client:

Desktop → IP Configuration → DHCP

---

# 11. Esempio Completo con Due Reti

```cisco
interface fa0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown

interface fa0/1
 ip address 192.168.20.1 255.255.255.0
 no shutdown

ip dhcp pool RETE10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8

ip dhcp pool RETE20
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 8.8.8.8
```

---

# 12. Comandi DHCP da Memorizzare

```cisco
ip dhcp excluded-address
ip dhcp pool
network
default-router
dns-server
lease
ip helper-address
show ip dhcp pool
show ip dhcp binding
show ip dhcp server statistics
show ip interface brief
```

---

# 13. Comando Lease

Durata assegnazione IP.

## Giorni

```cisco
lease 7
```

## Giorni ore minuti

```cisco
lease 7 12 30
```

## Lease infinita

```cisco
lease infinite
```

---

# 14. Cancellare Configurazioni DHCP

## Eliminare un pool

```cisco
no ip dhcp pool LAN
```

## Eliminare esclusione

```cisco
no ip dhcp excluded-address 192.168.1.1
```

## Cancellare binding

```cisco
clear ip dhcp binding *
```

---

# 15. Checklist Esame Packet Tracer

1. Configura IP interfacce.
2. Esegui `no shutdown`.
3. Escludi IP statici.
4. Crea il pool DHCP.
5. Imposta gateway.
6. Imposta DNS.
7. Verifica con `show ip dhcp binding`.
8. Imposta i PC su DHCP.
9. Testa con `ping`.
10. Se il server è remoto usa `ip helper-address`.

Fine guida.
