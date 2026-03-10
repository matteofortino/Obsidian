#uni 
# Circuito Aperto
Questo tipo di giunzione è alla base di elementi di circuteria fondamentali come i diodi.

I diodi sono elementi che permetto il passaggio di corrente in una sola direzione dipendentemente dalla loro polarizzazione.
Sono formati da due blocchi di silicio drogati (uno con drogaggio P e uno con drogaggio N).

Vi sono due terminali che prendono il nome di **anodo**(A, lato P) e **catodo**(K, lato N).
![[Diodo.png]]

Come abbiamo visto durante il [[Drogaggio del silicio]], avremmo il lato N con una prevalenza di elettroni liberi e il lato P con una prevalenza di lacune.

## Diffusione
Il principio di diffusione ci afferma che avremo uno transizione di lacune e di elettroni liberi in versi opposti, dando origine ad una zona $$\begin{matrix}W = \sqrt{\frac{2 \epsilon_s}{q}\left (\frac{1}{N_A} + \frac{1}{N_D} \right) (V_0 - V_{ak})} \\ \epsilon_s = \text{costante dielettrica del silicio}\end{matrix}$$
(i dati sono spiegati in seguito)!
detta *zona di svuotamento* dove il materiale non sarà più neutro dal punto di vista della carica.

Si viene quindi a formate un campo elettrico $\vec E$ diretto da N verso P, che si va ad opporre alla diffusione di elettroni e lacune. 

La caduta di tensione sulla zona di svuotamento agisce come un barriera di potenziale che deve essere superata dagli elettroni per diffondere nella zona P e dalle lacune per raggiungere la zona N. 
Possiamo vederla come una condizione di equilibrio dinamico nella quale la *corrente di diffusione* $I_D$ è compesata da una corrente uguale e opposta di drift $I_S$ dovuta al campo eletrico creato dallo squilibrio di carica.

## Potenziale elettrico
La caduta di potenziale a circuito aperto può essere espressa in funzione delle concentrazioni di droganti nella zona P ($N_A$) e nella zona N ($N_D$), della temperatura $T$ e della concentrazione intrinseca di portatori $n_i$: $$ V_0 = \frac{K_bT}{q}ln \left ( \frac{N_aN_D}{n_i^2} \right) $$
Nel silicio si osservano valori di $V_0$ into agli 0.6-0.8$V$. 
Se proviamo a misurare questa tensione otteniamo zero perché è compensata dai *potenziali di contatto*, se ciò non accaddesse sarebbe possibile far passare corrente dall'interno all'esterno, trarremmo quindi energia da una giunzione $pn$ in equilibrio termico con l'esterno. Si violerebbe il **secondo principio della termodinamica**.

Questa differenza di potenzile agisce da barriera rispettando la relazione $$ \vec E = - \frac{d\phi}{dx}$$
il grafico relativo sia alle lacune che agli elettroni:
![[andamento_del_potenziale_per_elettroni_e_lacune.png]]


# Polarizzazione inversa
Consideriamo il caso in alla giunzione sia connesso un generatori di tensione continua con il terminale positivo collegato alle regione N e quello negativo collegato alle regione P
![[polarizzazione_inversa.png]]
Il profilo potenziale è favorevole al passaggio di corrente da N a P. 
Quindi dalle lacune in N in P e dagli elettroni di P in N. Notiamo che sono entrambi gli elementi minoritari dei rispettivi drogaggi quindi la corrente che scorre è limitata dall quantità di portatori. Di fatto è molto piccola.

Concludiamo che in polarizzazione inversa la corrente è molto piccola e proprozionale solo al crescere della temperatura.

# Polarizzazione diretta
Consideriamo il caso in alla giunzione sia connesso un generatori di tensione continua con il terminale positivo collegato alle regione P e quello negativo collegato alle regione N.
![[polarizzazione_diretta.png]]k
La tensione applicata tende a far circolare una corrente dalla zona P alla zona N. Dualmente a prima sono gli elettroni in N a spostarsi in P e le lacune in P a spostarsi in N.
Notiamo che in questo caso sono gli elementi maggioritari dei rispettivi drogaggi, scorre quindi una corrente proporzionale al valore della tensione applicata non più limitata dal numero dei portatori.

Poichè l'energia termica posseduta dai portatori è distribuita secondo la distrubuzione di Fermi-Dirac, la corrente avrà un andamento esponenziale rispetto alla tensione applicata.

>[!Legge di Schockley]
>$$I = I_S\left ( e^{\frac{V}{\eta V_T}} -1 \right )$$
>Con $V_T = \frac{K_bT}{q}$, $\eta \in [1,2]$, $I_S \in [10^{-9}, 10^{-15}]A$ 


un immagine riassuntiva dei concetti
![[Pasted image 20260310165639.png]]
Sfrutta le relazioni:
- $\frac{dE}{dx} = -\frac{\rho(x)}{\epsilon}$
- $E = -\frac{d\phi}{dx}$ 