# Guida al NAT su Cisco Packet Tracer

## Indice
1. [Concetti base](#concetti-base)
2. [Static NAT](#static-nat)
3. [Dynamic NAT](#dynamic-nat)
4. [PAT / NAT Overload](#pat--nat-overload)
5. [Port Forwarding (Static NAT con porte)](#port-forwarding-static-nat-con-porte)
6. [Comandi di verifica e troubleshooting](#comandi-di-verifica-e-troubleshooting)
7. [Comandi per rimuovere/modificare configurazioni](#comandi-per-rimuoveremodificare-configurazioni)
8. [Errori comuni](#errori-comuni)

---

## Concetti base

Prima di configurare il NAT bisogna sempre definire:

- **Inside**: l'interfaccia/rete privata (LAN)
- **Outside**: l'interfaccia/rete pubblica (verso Internet/ISP)

Comandi base per entrare in configurazione:

```
enable
configure terminal
```

Per assegnare i ruoli alle interfacce (obbligatorio per QUALSIASI tipo di NAT):

```
interface g0/0
 ip nat inside
 exit

interface g0/1
 ip nat outside
 exit
```

> ⚠️ Senza `ip nat inside` e `ip nat outside` il NAT NON funziona, anche se le ACL e le altre regole sono corrette.

---

## Static NAT

Mappa **1 IP privato ↔ 1 IP pubblico** in modo permanente. Usato tipicamente per server interni (web server, mail server, ecc.) che devono essere raggiungibili dall'esterno.

### Comando

```
ip nat inside source static <ip-privato> <ip-pubblico>
```

### Esempio

Server interno con IP `192.168.1.10`, che deve essere visto dall'esterno come `203.0.113.10`:

```
enable
configure terminal

interface g0/0
 ip nat inside
 exit

interface g0/1
 ip address 203.0.113.1 255.255.255.0
 ip nat outside
 exit

ip nat inside source static 192.168.1.10 203.0.113.10

end
write memory
```

### Caratteristiche
- Mappatura sempre attiva, non scade mai
- 1:1 fisso (consuma 1 IP pubblico per ogni mappatura)
- Va bene quando hai pochi server da esporre

---

## Dynamic NAT

Mappa **un pool di IP privati** verso **un pool di IP pubblici**, in modo dinamico (assegnazione "a chiamata", non fissa). Serve sempre una **ACL** per definire quali IP privati possono essere tradotti.

### Sintassi generale

```
access-list <numero> permit <rete-privata> <wildcard-mask>

ip nat pool <nome-pool> <ip-inizio> <ip-fine> netmask <subnet-mask>

ip nat inside source list <numero-acl> pool <nome-pool>
```

### Esempio

Rete privata `192.168.1.0/24` che deve uscire usando IP pubblici dal range `203.0.113.10` a `203.0.113.20`:

```
enable
configure terminal

! Definisco quali IP privati possono essere tradotti
access-list 1 permit 192.168.1.0 0.0.0.255

! Definisco il pool di IP pubblici disponibili
ip nat pool POOL_PUBBLICO 203.0.113.10 203.0.113.20 netmask 255.255.255.0

! Collego ACL e pool
ip nat inside source list 1 pool POOL_PUBBLICO

! Imposto le interfacce
interface g0/0
 ip nat inside
 exit

interface g0/1
 ip address 203.0.113.1 255.255.255.0
 ip nat outside
 exit

end
write memory
```

### Caratteristiche
- Più host privati possono uscire, ma solo finché ci sono IP pubblici liberi nel pool
- Se gli host privati > IP nel pool, alcuni host non riusciranno a fare NAT finché non si liberano IP
- Le traduzioni hanno un timeout (default 24h per UDP/TCP, scadono se inattive)

---

## PAT / NAT Overload

**PAT (Port Address Translation)**, conosciuto anche come **NAT Overload**, è il tipo PIÙ USATO in laboratorio e nella realtà: permette a **TUTTI** gli host della LAN di uscire usando **UN SOLO IP pubblico**, distinguendo le sessioni tramite le porte (es. 192.168.1.10:1234 → 203.0.113.1:5000).

### Sintassi - variante con interfaccia (la più comune)

Usa l'IP assegnato dinamicamente o staticamente all'interfaccia outside:

```
access-list <numero> permit <rete-privata> <wildcard-mask>

ip nat inside source list <numero-acl> interface <interfaccia-outside> overload
```

### Esempio (variante interfaccia)

```
enable
configure terminal

access-list 1 permit 192.168.1.0 0.0.0.255

interface g0/0
 ip nat inside
 exit

interface g0/1
 ip address 203.0.113.1 255.255.255.0
 ip nat outside
 exit

ip nat inside source list 1 interface g0/1 overload

end
write memory
```

### Sintassi - variante con pool (overload su un pool)

Utile quando vuoi che tutta la LAN esca tramite uno o più IP pubblici diversi da quello dell'interfaccia:

```
access-list <numero> permit <rete-privata> <wildcard-mask>

ip nat pool <nome-pool> <ip-inizio> <ip-fine> netmask <subnet-mask>

ip nat inside source list <numero-acl> pool <nome-pool> overload
```

### Esempio (variante pool)

```
enable
configure terminal

access-list 1 permit 192.168.1.0 0.0.0.255

ip nat pool POOL_PAT 203.0.113.5 203.0.113.5 netmask 255.255.255.0

ip nat inside source list 1 pool POOL_PAT overload

interface g0/0
 ip nat inside
 exit

interface g0/1
 ip address 203.0.113.1 255.255.255.0
 ip nat outside
 exit

end
write memory
```

### Caratteristiche
- 1 solo IP pubblico (o pochi) per TUTTA la LAN
- Usa la combinazione IP+porta per distinguere le sessioni (fino a ~65000 sessioni simultanee per IP pubblico, in teoria)
- È quello che usa anche il tuo router di casa verso Internet

---

## Port Forwarding (Static NAT con porte)

Variante dello Static NAT che permette di **redirigere una porta specifica** verso un host interno, lasciando l'IP pubblico condiviso per altri usi (tipicamente combinato con PAT).

Esempio tipico: rendere visibile dall'esterno un web server interno sulla porta 80.

### Sintassi

```
ip nat inside source static tcp <ip-privato> <porta-privata> <ip-pubblico> <porta-pubblica> extendable
```
oppure (usando l'IP dell'interfaccia outside)
```
ip nat inside source static tcp <ip-privato> <porta-privata> interface <interfaccia-outside> <porta-pubblica> extendable
```

### Esempio

Web server interno `192.168.1.50` sulla porta `80`, esposto sull'IP pubblico `203.0.113.1` sempre sulla porta `80`:

```
enable
configure terminal

interface g0/0
 ip nat inside
 exit

interface g0/1
 ip address 203.0.113.1 255.255.255.0
 ip nat outside
 exit

! PAT per la rete interna (così gli altri host hanno comunque accesso a Internet)
access-list 1 permit 192.168.1.0 0.0.0.255
ip nat inside source list 1 interface g0/1 overload

! Port forwarding per il web server
ip nat inside source static tcp 192.168.1.50 80 interface g0/1 80 extendable

end
write memory
```

> 💡 La keyword `extendable` è praticamente sempre necessaria quando si combinano più regole statiche di tipo port-forwarding sullo stesso router, o quando si combina static port forwarding e PAT.

### Esempio con più servizi sullo stesso IP pubblico, porte diverse

Server FTP interno `192.168.1.51` porta `21`, server Web `192.168.1.50` porta `80`, entrambi raggiungibili dall'IP pubblico `203.0.113.1`:

```
ip nat inside source static tcp 192.168.1.50 80 interface g0/1 80 extendable
ip nat inside source static tcp 192.168.1.51 21 interface g0/1 21 extendable
```

---

## Comandi di verifica e troubleshooting

Da modalità privilegiata (`enable`, NON serve `configure terminal`):

```
show ip nat translations
```
Mostra la tabella delle traduzioni attive (Inside local / Inside global / Outside local / Outside global).

```
show ip nat statistics
```
Mostra statistiche generali: numero di traduzioni attive, hit/miss, configurazione interfacce inside/outside, ecc.

```
show running-config
```
Mostra tutta la configurazione, comprese le righe NAT, ACL, interfacce.

```
show ip interface brief
```
Verifica rapidamente IP e stato (up/down) delle interfacce — utile per controllare che inside/outside abbiano l'IP corretto e siano attive.

```
debug ip nat
```
Mostra in tempo reale i pacchetti tradotti (utile in laboratorio per capire se il traffico passa dal NAT). Da disattivare con `no debug ip nat` o `undebug all`.

### Test pratico
Da un PC nella rete interna, fare `ping` verso un IP esterno (es. l'IP dell'interfaccia outside del router ISP, o un server simulato esterno), poi controllare su router con `show ip nat translations`: si dovrebbe vedere una riga con la traduzione dell'IP del PC.

---

## Comandi per rimuovere/modificare configurazioni

Per cancellare una regola NAT, basta ripetere il comando con `no` davanti:

```
! Rimuove uno static NAT
no ip nat inside source static 192.168.1.10 203.0.113.10

! Rimuove una regola dynamic NAT/PAT
no ip nat inside source list 1 interface g0/1 overload

! Rimuove un pool
no ip nat pool POOL_PUBBLICO 203.0.113.10 203.0.113.20 netmask 255.255.255.0

! Rimuove ip nat inside/outside da un'interfaccia
interface g0/0
 no ip nat inside
 exit
```

### Pulire le traduzioni attive (forzare il refresh della tabella)

```
clear ip nat translation *
```
Cancella TUTTE le traduzioni dinamiche attive (utile dopo aver cambiato configurazione, per evitare che le vecchie traduzioni restino "appese").

Per cancellare una singola traduzione:
```
clear ip nat translation inside 192.168.1.10 203.0.113.10
```

---

## Errori comuni

1. **Dimenticare `ip nat inside` o `ip nat outside`** sulle interfacce → il NAT non scatta mai, anche se le ACL/pool sono corretti.
2. **ACL troppo permissiva o troppo restrittiva**: usare sempre la wildcard mask corretta (es. `0.0.0.255` per una /24, NON `255.255.255.0`).
3. **Pool con range di IP insufficiente** per il numero di host (in Dynamic NAT senza overload).
4. **Dimenticare `overload`** quando si vuole far uscire più host con un solo IP pubblico → senza `overload` il router accetta solo 1 traduzione per IP del pool, dando errore "no available addresses" quando il secondo host tenta di uscire.
5. **Dimenticare `extendable`** quando si combinano regole statiche con porte e PAT.
6. **Interfaccia outside senza IP configurato** (o con IP sbagliato/non corrispondente alla subnet del link verso l'ISP).
7. **Routing mancante**: il NAT non sostituisce il routing! Serve comunque una rotta (statica o tramite routing dinamico, es. `ip route 0.0.0.0 0.0.0.0 <next-hop>`) per raggiungere l'esterno.
8. **Salvare la configurazione**: ricordarsi sempre `write memory` (o `copy running-config startup-config`) altrimenti al riavvio del router in Packet Tracer si perde tutto.

---

## Riepilogo comandi rapidi (cheat sheet)

```
! Ruoli interfacce (sempre necessario)
interface <int>
 ip nat inside / ip nat outside

! Static NAT
ip nat inside source static <ip-priv> <ip-pub>

! Static NAT con porte (port forwarding)
ip nat inside source static tcp <ip-priv> <porta-priv> interface <int-outside> <porta-pub> extendable

! Dynamic NAT
access-list <n> permit <rete> <wildcard>
ip nat pool <nome> <ip-start> <ip-end> netmask <mask>
ip nat inside source list <n> pool <nome>

! PAT (overload) con interfaccia
access-list <n> permit <rete> <wildcard>
ip nat inside source list <n> interface <int-outside> overload

! PAT (overload) con pool
ip nat pool <nome> <ip> <ip> netmask <mask>
ip nat inside source list <n> pool <nome> overload

! Verifica
show ip nat translations
show ip nat statistics

! Pulizia traduzioni
clear ip nat translation *

! Rimozione
no <comando-da-rimuovere>
```
