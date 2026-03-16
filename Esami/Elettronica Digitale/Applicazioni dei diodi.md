#uni 
Un applicazione tipica dei diodi è quella della **rettificazione** della tensione. La rettificazione è un operazione che porta una corrente alternata in una corrente continua, o comunque porta una tensione che oscilla attorno ad uno 0 di potenziale a stare tutta sopra tale 0. Circuiti di questo tipo sono utili ad esempio per ottenere generatori di tensione stabili dalla comune alimentazione casalinga (311 V di picco, in Italia, dal valore efficace 220 V moltiplicato per √ 2), che non solo è in corrente alternata sinusoidale, ma è anche piena di disturbi ed interferenze date dalla rete elettrica.

# Circuiti di rettificazione
# Rettificatore ideale
Vediamo il circuito del *rettificatore ideale*:
![[rettificatore_ideale.png]]
Notiamo due componenti fondamentali:
- il *generatore di tensione*: descritto da una funzione sinusoidale $V_S = V_m\sin(\omega t), \quad \omega = 2 \pi f$
- La resistenza $R_L$ detta resistenza di *carico*,(*load* in inglese). Rapprenseta l'applicazione effettiva che sfrutta il voltaggio rettificato.

Per studiare il circuito prendiamo due range temporali diversi, calcolati a partire dall'espressione $V_S$: uno dove questa è positiva, uno dove è negativa.
![[grafico_tensione_alternata.png]]
analizziamo un range temporale alla volta: 
- $t_0 < t < t_1$
	In questo caso risulte che $D$ è posto fra un potenziale positivo e la terra. Segue che il diodio sarà in conduzione quindi $V_u = V_S = V_m\sin(\omega t)$
	**Verifica**:
	Essendo $D$ in conduzione controlliamo la corrente $I_D = \frac{V_S}{R_L} > 0$ , ipotesi soddisfatta.
- $t_1 < t < t_2$ 
	Adesso $D$ si trova a polarizzazione negativa, sarà probabilmente in interdizione. Si conclude che $V_u = 0$
	**Verifica**:
	$V_D = -5V < V_{\gamma} \approx 0.7V$, ipotesi verificata

Abbiamo ottenuto un circuito che per tensioni negative vale zero.
![[grafico_tensione_alternata_rettificato_ideale.png]]
# Rettificatore a caduta costante
Vediamo adesso il *rettificatore a caduta costante*
![[rettificatore_caduta_costante.png]]
Le prime condizioni di prima variano nel seguente modo: 
- $V_u = V_S - V_{\gamma}$
- $I_D = \frac{V_S - V_{\gamma}}{R_L}$ 
Quindi adesso $V_u$ vale zero solo quando $V_S > V_{\gamma}$ questo ha due conseguenze:
1. Un *ritardo di conduizione* sul fronte dell'onda che parte da $0V$ 
2. Un *anticipo di spegnimento* sul fronte dell'onda che va ricadere su $0V$ 
Entrambi i fenomeni sono di durata $\Delta t$  che vale: $$ V_S = V_{\gamma} = V_m\sin(\omega t), \quad \sin(\omega t) = \frac{V_{\gamma}}{V_m}, \quad wt = 
\sin^{-1}\left (\frac{V_{\gamma}}{V_m} \right )$$$$ \Rightarrow \Delta t = \frac{1}{\omega}\sin^{-1}\left ( \frac{V_{\gamma}}{V_m} \right) $$
Graficando la soluzione ottenuta: 
![[grafico_tesione_alternata_rettificato_caduta_costante.png]]
Abbiamo che i picchi del segnale dopo il rettificatore sono stati ridotti (appunto dalla caduta costante), e abbiamo dei ritardi e degli anticipi della regione in cui il segnale viene troncato al di sopra di 0 V (in sostanza passiamo più tempo vicino agli 0 V).