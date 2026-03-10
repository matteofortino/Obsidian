#uni 
# Potenza trasmessa e ricevuta
Prendiamo come riferimento una antenna *isotropica* (tramette in tutte le direzioni) che irradia un segnale di potenza $P_t$. 
Scelto un punto a distanza $d$ dall'antenna la potenza ricevuta da quel punto è: $$P_d = \frac{P_t}{4 \pi d^2}$$
La potenza raccolta da un antenna a distanza $d$ di area $A$ è: $$P_r = P_dA = \frac{P_tA}{4 \pi d^2}$$ si ci troviamo in uno spazio libero senza ostacoli.

Dalla teoria dei segnali si dimostra che l'area efficace di un antenna vale $A_e = \frac{\lambda^2}{4 \pi}$ quindi la nostra antenna riceve una potenza pari a: 
>[!Legge di Friis]
>$$P_r = P_dA_e = \frac{P_t \lambda^2}{(4 \pi)^2d^2} = \frac{P_t}{(\frac{4 \pi d}{\lambda})^2} = \beta P_t$$
>con $\beta = \frac{1}{(\frac{4 \pi d}{\lambda})^2}$ e $\lambda = \frac{c}{f_0}$ 

Dove $c$ è la velocità della luce e $f_0$ è la frequenza del segnale. 
$\lambda$ è conuscita anche come **lunghezza d'onda**. 

$\beta$ assume valori compresi fra i -40dBW e i -120dBW [[#Decibel]]. 

Notiamo che $\frac{P_r}{P_t} = \beta$ 

Considerazioni, la potenza ricevuta dall'antenna: 
- *dimiuisce con il quadrato della distanza* 
- *varia con il quadrato della lunghezza d'onda*.

Per mettere in prospettiva ecco una tabella della potenza in funzione della distanza in base ad una determinata frequenza di trasmissione.

| distanza | 3Ghz                | 30Ghz               |
| -------- | ------------------- | ------------------- |
| **25**   | $10^{-8}W$          | $10^{-10}W$         |
| 100      | $6 \cdot 10^{-10}W$ | $6 \cdot 10^{-12}W$ |
| 250      | $10^{-10}W$         | $10^{-12}W$         |

I valore della potenza sono molto piccoli e variano di molti ordini di grandezza; rappresentarli su un piano cartesiano con scala lineare portebbe ad un grafico che non da informazioni utili a prima vista.
Si sceglie per questo motivo di utilizzare una scala logaritmica, per fare ciò ci viene in aiuto la notazione in decibel.
# Decibel
È una unità logaritmica che esprime il rapporto fra due grandezze $$ P_{dB} = 10\log_{10}\left (\frac{P}{P_0}\right) $$
Per un generico segnale vale: $$ P_{x,dB} = 10\log_{10}\left (\frac{|x(t)|^2}{|x(t_0)|^2} \right ) = 20\log_{10} \left ( \frac{|x(t)|}{|x(t_0)|} \right ) $$
Passando alla notazione in decibel con le formule della potenza vista prima otteniamo $$ P_{r,dB} = P_{t, dB} + \beta $$
# Guadagno di antenna
Le antenna genericamente non sono isotrope ma irradiano il segnale lungo un condo di direzione.
Questo compensa la perdita di propagazione e otteniamo  $$ P_r = G_r G_t P_t \beta $$
che in decibel iventa una somma.
Notiamo che se $G_r = G_t = 1$ torniamo al caso isotropo.

## Array di antenne
Per aumentare il guadagno si dispongono più antenna una vicino all'altra, questa configurazione prende il nome di *array di antenne*. 
Si verifica facilmente che possedendo $N$ antenne l'area totale $A_{\text{tot}} = NA_e$ e la potenza ricevuta $P_r = \beta P_t N$.

Il numero di antenne prendendo una configurazione sferica si ottiene $$\begin{matrix} N = \frac{4 \pi d^2}{A_e} = \frac{(4 \pi)^2 d^2}{\lambda^2} = \frac{1}{\beta} \\ A_e = \frac{\lambda^2}{4 \pi} \end{matrix}$$
Di fatti se ricopriamo tutta la sfera con le antenna riceviamo per interno la potenza erogata dalla sorgente.

Si prendiamo in esempio una confugiarzione planare abbiamo che estendendo all'infinito la superfice efficare $P_r = \frac{P_t}{2}$.
