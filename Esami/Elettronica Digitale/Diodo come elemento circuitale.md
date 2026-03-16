#uni 
Abbiamo finito di affrontare le caratteristiche fisiche relative al *diodo*. Iniziamo quindi a concentrarci sul suo comportamento a livello circuitale. 

Ricordiamo che la corrente che scorre su un diodio è governata dalla ![[Giunzione pn#^leggeDiShockley]]
dalla quale notiamo che se $V_D < 0 \to I_D \approx I_S, \quad V_D > 0 \to I_D = I_Se^{\frac{V_D}{\eta V_T}}$ 
# Breakdown 
Se si applica un atensione inversa sufficentemente grande, entrano in gioco nuovo meccanismi in grado di far transitare una corrente molto più grande i $I_S$ attraverso la giunzione.
Questo fenonemo è detto di **breakdown** (non da intendersi come vera e propria rottura del diodo).

I diodi in generale hanno tensioni di breakdown molto alte, in caso contrario perderebbero la loro funzione principale ovvero quella di impedire il passagio di corrente in polarità inversa.

Esistiono due tipi di breakdown: 
- *Breakdown Zener*: si verifica a tensioni $|V_{BR}|< 5V$ 
- *Breakdown Valanga*: si verifca a tensioni $|V_{BR}| > 7V$
I diodi che hanno tensione di breakdown fra 5 e 7 $V$ sfruttano entrambi i maccenismi in combinazione. 

I diodi con tensioni di breakdown volumente basse sono chiamati diodi di Zener indipendente dal meccanismo di breakdown che sfruttano.
## Effetto Zener
Il breakdown Zener deriva da un fenomeno abbanstanza compelssi di *tunneling interbanda* che non affronteremo.
Possiamo però descriverlo, semplificando, come la conseguenza della **rottura dei legami covalenti** nella zona di svuotamento causata dall'elevato campo elettrico.
Questo da luogo a un *gran numero di portatori liberi*. Per piccoli incrementi della tensione inversa si avranno forti aumenti della corrente.

## Effetto valanga
Il breakdown valanga si verifica invece quando il campo elettrico nella zona di svuotamento può *accellerare* i portatori minoritari che attraversano la zona stessa fino ad una velocità tale da **rompere i legami covalenti** degli atomi con cui collidono. I portatori così liberati vengono a loro volta accellerati e causano altre rottur creando un effetto a *valanga*.
Questo fenomento vale sia per le lacune che per gli elettroni.


# Analisi circuitale 
L'elemento circuitale che descrive un diodo è: 
![[diodo_circuitale.png]]
La freccia descrive il verso della corrente in polarizzazione diretta.
Il terminale di sinistra corrisponde a P, di conseguenza quello di desta corrisponde a N.

I diodi di Zener citanti in precendeza si identificano:
![[diodio_zener_circuitale.png]]

Prendiamo in esempio un semplice circuito.
![[primo_esempio_di_circuito_con_diodo.png]]
Applivando la seconda legge di Kirchhoff alla Maglia e ricondando l'equazione di Shockely possiamo scrivere $$ \begin{cases}
V_{AA} = RI_D + V_D \\
I_D = I_S e^{\frac{V_D}{\eta V_T}}
\end{cases}
$$
Non possiamo risolvere il circuito con i metodi visti in corsi precedenti perchè il diodio è un elemento **non lineare**.

Possiamo sempre ricorrere al metodo numerico e svolgere i calcoli ma, in presenza di circuiti più complessi la risoluzione diventa insidiosa.

## Metodo Grafico
Ricorriamo al metodo grafico, disegnando per l'appunto il grafico dell'andamento della correnti in fuzinoe della tensione.

Mettendo insieme le due equzioni otteniamo 
![[rappresentazione_grafica_andamento_corrente_primo_circuto_con_diodo.png]]
Ovviamente il punto di intersezione fra le due curve rappresenta la soluzione del nostro sistema.

La retta di equazione $I_D = -\frac{V_D}{R} + \frac{V_AA}{R}$ è detta **retta di carico**.


## Modelli per segnali grandi
Questi modelli si basano sull'approssimazione del diodo in elementi circuitali lineari in modo da poter risolvere il circuito con metodi che già conosciamo. 

Si presenta un problema, il diodo si comporta differentemente in caso di polarizzazione diretta i inversa ma non noi sappiano in quale si trova prima di risovlere il circuito.
Siamo costretti a fare un ipotesi la quale, se a fine risoluzione si presenterà errata, andrà modificata e il circuito dovrà essere nuovamente risolto.
Esistono tre modelli principali:
- *Modello a caduta costante*: 
	- ON: batteria (generatore ideale di tensione)
	- OFF: circuito aperto
- *Modello a diodio ideale*: 
	- ON: cortocircuito
	- OFF: circuito aperto
- *Modello a tratti*:
	- ON: batteria + resistenza (in serie)
	- OFF: circuito aperto
x