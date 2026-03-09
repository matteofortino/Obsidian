Abbiamo visto i fenomeni di *generazione e ricombinazione termica* e la conducibilità del silicio $\sigma = nq\mu_n + pq\mu_p$.

Facciamo un calcolo con dei dati reali:
- $\mu_n= 1390 \frac{cm^2}{V \cdot s}$
- $\mu_p= 480 \frac{cm^2}{V \cdot s}$
- $n = p = 10^{10} cm^{-3}$ 
concludiamo che $\sigma \approx 3 \cdot 10^{-6} [\Omega \cdot cm]^{-1}$ quindi $\rho = \frac{1}{\sigma} \approx \frac{1}{3} \cdot 10^6 [\Omega \cdot cm]$.
Il silicio non è un buon conduttore, eseguiamo quindi un'operazione di **drograggio**. 

# Drogaggio tipo N
In questo tipo di drogaggio vengono usati elementi del gruppo 5, ovvero con cinque elettroni nell'orbitale più esterno. I più comuni sono il fosforo (P) e l'arsenico (As). 

Quando andiamo ad insierire un atomo di fosforo all'interno del reticolo di silicio osserviamo che quattro elettroni formeranno il legame covalente mentre un elettrone rimarrà libero.
![[drogaggio_tipo_n.png]]
Si arriva alla conclusione che in silicio drogato il numero di elettroni liberi non è più uguale al numero di lacune.

In questo caso il fosforo è detto **atomo donatore**, in quanto dona un suo elettrone e si carica positivamente.

La concentrazione di elettroni liberi $N_d^+$ varia fra $10^{14}$ e $10^{21}$, si nota anche che all'equilibro $n \approx N_d$ , $p = \frac{n_i^2}{N_d}$. 

In un drogaggio di questo tipo abbiamo *maggiorato* gli $n$ e *minorato* le $p$.

# Drogaggio tipo P
Dualmente a prima si usano elementi del gruppo 3, ovvero elementi con 3 elettroni nell'orbitale più esterno. Comunemente l'elemento più utilizzato è il boro (B).

Introducento un atomo di boro all'interno del reticolo di silicio noteremo la formazione di tre legami covalenti e la formazione di una lacuna in quanto "manca" un elettrone per poter formare il legame.
![[drogaggio_tipo_p.png]]
La conclusione che traiamo è la stessa, soltanto che in questo tipo di drogaggio sono *maggiorate* le $p$ e *minorati* gli $n$.

Questo accade perchè a temperatura ambiente un elettrone si sposta verso il boro che lo accetta, si carica quindi negativamente.

Il boro viene anche chiamato **atomo accettore**.

La concentrazione di elettroni liberi $N_a^-$ varia fra $10^{14}$ e $10^{21}$, si nota anche che all'equilibro $p \approx N_a$ , $n = \frac{n_i^2}{N_a}$. 

# Considerazioni sulla conducibilità
Riprendiamo la formula $$ \sigma = qn\mu_n + qp\mu_p = f(T)$$
Abbiamo che $\sigma$ stessa dipende dalla temperatura $T$ sulla base delle contentrazione di $n$ e $p$ . Si scopre che anche $\mu_n$ e $\mu_p$ dipendono da essa, in particolare: 
- $\mu_n$ e $\mu_p$ dimnuiscono all'aumentare di $T$ (sono proporzionali a $T^{-\frac{3}{2}}$).
- $p$ e $n$, va condiderato il caso di drogaggio o meno
	- *silicio intriseco*: $p$, $n$ aumentano esposenzialmente seguento la legge ![[Basi fisiche dei dispositivi a semiconduttore#^concentrazioneintrinseca]] si ha che $\sigma$ aumenta in media in quanto la mobilità dimunuscice meno di quanto aumentino le concentrazioni
	- *silicio drograto*: per entrambi i casi la concentrazione di conduttori maggioritari rimane costante, invece quella dei conduttori minoritari aumenta con $T$ ma molto poco.
	  Questo fa sì che in media $\sigma$ diminuisca con $T$.
# Corrente di diffusione
Per via dell'**agitazione termica** quando si ha un *gradiente di contentrazione* si verifica il fenomeno della **diffusione** che porta alla formazione di correnti dette *correnti di diffusione*. 

In generale la densità di carica è legata alla variazione di flusso sono legati dalla seguente legge. 
>[!Legge di Flick]
>$$\vec{J} = -D \nabla \phi$$
>Dove D è la costante di diffusione di $\phi$ 


Prendiamo un materiale e stimiano una concentrazione di elettroni che varia sull'asse $x$ come una funzione $n(x)$ che ha un picco in $x = 1$. Una funzione possibile è: $$ n(x) = e^{-x}, x \geq 0$$
In questo caso nell'intorno di $x = 0$ si ha una zona ad altra generazione ricombinazione termica. Per diffusione avremo che delle cariche $e$ si sposteranno in direzione $x >> 0$ causando quindi una corrente nel verso opposto.

## Diffusione elettronica
Stimiano il valore di $\vec{J}$ per i soli elettroni. $$\vec J = -(-e)D_n(\frac{\partial n}{\partial x})\hat i_x = eD_n\frac{\partial n}{\partial x}\hat i_x$$
Dove $D_n$ è la costante di diffusione, analoga alla mobilità e $\frac{\partial n}{\partial x}$ è la versione bidimensionale di $\nabla n$.
## Diffusione di lacune
ragionamento analogo a quello fatto per gli elettroni con la differenza che le lacune sono portatori positivi. $$ \vec J = -eD_p(\frac{\partial p}{\partial x})\hat i_x = -eD_p\frac{\partial p}{\partial x}\hat i_x $$
Dove $D_p$ è la costante di diffusione, analoga alla mobilità e $\frac{\partial p}{\partial x}$ è la versione bidimensionale di $\nabla p$.

## Costanti di diffusione
Vogliamo poter stimare il valore delle *costanti di diffusione*, ci viene in aiuto la
>[!Legge di Einstein delle costanti di diffusione]
>$$ \frac{D_n}{\mu_n} = \frac{D_p}{\mu_p} =\frac{K_bT}{e} = V_T $$

dove $V_T$ è la *tensione termica* che per $T \approx 300K$ vale circa $26mV$

# Combinazione delle correnti di drif e diffusione
Chiamando $\vec J_{drift}$ la correnti di drift e $\vec J_{diffusione}$ la corrente di diffusione abbiamo che:
- *Portatori negativi*: $\vec J_N = \vec J_{N, Drift} + \vec J_{N, diffusione} = ne\mu_n\vec E + eD_N\frac{\partial n}{\partial x}\hat i_x$
- *Portatori positivi*: $\vec J_P = \vec J_{P, Drift} + \vec J_{P, diffusione} = pe\mu_p\vec E - eD_P\frac{\partial p}{\partial x}\hat i_x$
Quindi la densita di corrente complessiva sarà $$ \vec J = \vec J_N + \vec J_P =  ne\mu_n\vec E + eD_N\frac{\partial n}{\partial x}\hat i_x +  pe\mu_p\vec E - eD_P\frac{\partial p}{\partial x}\hat i_x$$
