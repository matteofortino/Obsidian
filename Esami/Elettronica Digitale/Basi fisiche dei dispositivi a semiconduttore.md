# Conduzione
Iniziamo parlando del fenomeno della conduzione attraverso un materiale metallico.
Supponiamo di avere un parallelepipedo di sezione $S$ e di lunghezza $L$

Dalla **Seconda legge di Ohm** $$R = \rho \frac{L}{S} $$
l'abilità di un materiale di condurre cariche è inversamente proporsionale alla *resistenza* 

Dove $\rho$ è una costante specifica del materiale detta *resistività*

Definiamo $\sigma = \frac{1}{\rho}$ detta *conducibilità* quindi otteniamo $$ R = \frac{L}{\sigma S}$$ 
In base alla resistività si distinguono tre categorie di materiali: 
- *conduttore* $\rho < 10^{-2}\Omega cm$
- *isolante* $\rho > 10^5 \Omega cm$
- *semi-conduttore* $10^{-2} <\rho < 10^5 \Omega cm$
# Moto della carica nel materiale
Immaginiamo di avere il nostro materiale conduttore, inizialmente senza applicare alcun campo elettrico $\vec{E} = 0$.

Le cariche all'interno del materiale non sono ferme ma sono in movimento per effetto della temperatura e compiono un **Moto Browniano**, ovvero un moto caotico.

Presa una sezione di materiale $S'$ si osserva che il numero di cariche che attraversano la sezione in un tempo $\Delta t$ in un verso è pari al numeri di cariche che la attrevarsano nel verso opporto.

Notiamo quindi che la corrente $i = \frac{dq}{dt}$ è pari a zero.

Se immergiamo il materiale in un campo elettrico $\vec{E} \neq 0$ l'elettrone continua il suo *moto caotico* (eventualmente urtando ostacoli o altri elettroni) spostandosi man mano lungo la stessa direzione ma con verso opposto rispetto al campo elettrico.

Prendendo nuovamente la superficie $S'$ adesso le cariche la attreversano nel medesimo verso quindi la corrente $i = \frac{dq}{dt} \neq 0$

# Velocità di Deriva
## Modello di Drude
Descrive la **conduzione elettrica** nei metalli *trattando gli elettroni come un gas classico di particelle* libere che urta ioni fissi. Basato sulla teoria cinetica dei gas, spiega la resistività, la **conducibilità** DC/AC e l'effetto Hall, assumendo urti istantanei e assenza di interazioni tra elettroni.

![[grafico_velocità_elettrone.png]]

Il grafico rappresenta la velocità di un elettrone rispetto al tempo all'interno del materiale.

Essendo il campo elettrico $\vec{E}$ costante si ha un incremento lineare della velocità.
Durante gli urti gli elettroni perdono la loro velocità.

Possiamo calcolare una velocità media proporzionale al campo elettrico: $$ \vec{v}_{media} = \vec{v}_{drift} = -\mu_n \vec{E}$$
Dove il segno meno sta ad indicare il moto in verso opposto al campo elettrico e la quantità $\mu_n$ la *mobilità* ossia la capacità di un elettrone di muoversi all'interno del materiale.


Definiamo $N$: numero di elettroni all'interno del materiale.
Misuriamo in un intervallo $$\Delta T = \frac{L}{\vec{v}_{drift}}$$
La corrente $$I = \frac{\Delta Q}{\Delta T} = q\frac{N}{\Delta T} = q\frac{N}{L}\vec{v}_{drift}$$
Ricaviamo la densità di corrente $$ J = \frac{I}{S} = \frac{qNv_{drift}}{LS} = \frac{Nq}{\text{Volume}}v_{drift} = nqv_{drift} = nq\mu_nE = \sigma E$$
Notiamo con attenzione $$J = \sigma E \Longrightarrow \sigma = nq\mu_n$$ 
$\sigma$ è sempre positiva.
Il legame fra $J$ ed $E$ è detto **Legge microscopica di Ohm**. 

## Semiconduttori
Il semiconduttore di nostro interesse è il **Silicio**(Si). 
Una grande differenza riguarda i legami che per i metalli sono legami metallici invece per il silicio sono *covalenti*

![[struttura_silicio.png]]

In questo schema gli elettroni condivisi fra i vari atomi di silicio sono liberi di muoversi ma ristretti all'interno di una regione in quanto devono mantenere il legame.

Se ci troviamo a temperatura $T = 0K$ anche se viene applicato un campo elettrico $\vec{E}$ gli elettroni *non avranno abbastanza energia per rompere i legami* di fatto non scorrerà nessuna corrente.

A temperatura ambiente gli elettroni possiedono *Energia Termica* che permetta la rottura del legame. Si genera quindi un **elettrone libero**. Questo processo di rottura è detto **Generazione termica**

Possiamo stimare la concentrazione di elettroni liberi nel silicio 

$$n_i = BT^{\frac{3}{2}}e^{-\frac{E_g}{2K_bT}}$$^concentrazioneintrinseca
 
che aumenta esponenzialmente con la temperatura.
A $T \approx 300K , n_i \approx 10^{10}cm^{-3}$ 

Esiste anche il processo inverso detto **Ricombinazione Termica** nel quale gli elettroni liberi passano in una zona di legame e ve ne rimango bloccati riformando il legame.

Se applichiamo un campo elettrico $\vec{E}$ gli elettroni iniziano a rompere legami generando:
- un elettrone libero
- una *lacuna*
Una lacuna è una *particella fittizia* con carica opposta a $e$ e una sua mobilità $\mu_p$ vanno quindi considerate nel calcolo della conducibilità $$\sigma = n\mu_nq + p\mu_pq$$ dove $p$ è il numero di lacune.

Notiamo immediatamente che $n = p$ in quanto si formano a coppie quindi $$\sigma = nq(\mu_n + \mu_p)$$
Gli elettroni si muovono in verso opposto al campo elettrico generando una corrente. 

Concludiamo con la
>[! Legge Azione di massa]
>$$ n \cdot p = n_i^2 $$ 

un risultato che può sembrare banale per il silicio intrinseco ma che vedremo si potrà estendere anche oltre.