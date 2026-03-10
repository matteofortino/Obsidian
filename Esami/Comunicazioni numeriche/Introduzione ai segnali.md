#uni 

*Cosa è un segnale?*
Una segnale è una qualunque grandezza fisica variabile cui è associata una certa **informazione**.

Se ne distinguono due tipi: 
- *Deterministici*: noti a priorio, approssimabili con funzioni analitiche come seno e coseno.
- *Aleatori*: non noti a prioprio, rappresentabili statisticamente come il segnale **rumore**.
Prendiamo in esempio una onda quadra $x(t)$ e un elettrocardigramma. 

L'onda quadra è un segnale *determinato* in quanto il suo valore è noto per ogni $t$, l'elettrocardigramma non è noto prima che venga misurato, ciò lo rende *aleatorio*.

Notiamo anche che l'onda quadra è un segnale **discreto** rispetto al tempo, l'elettrocardiogramma è contrariamente **continuo** rispetto al tempo.

Si pone il problema di passare da segnali continui (come si trovano in natura) a segnali discreti.

# Sistema di registrazione
Prendiamo in esempio un sistema di registrazione.

![[sistema_di_registazione.png]]

Il dispositivo riceve in ingresso un segnale continuo, lo amplifica (non cambiandone la forma intrinseca) e poi passa attraverso un interruttore che si chiude con periodo $T$ multiplo di $t$. Questo ci permette di passare ad un segnale discreto. 

Durante la fase di amplificazione non abbiamo perdita di informazione e nemmeno durante la fase di campionamento se viene rispettato il
>[! Teoremo del campionamento (Nyquist-Shannon)]
>$$\frac{1}{T} \geq 2B$$
>dove B è la banda del segnale

Se questa condizione è verificate non si ha perdita di informazione.

Dobbiamo ancora portare il segnale in forma **digitale**. Per fare ciò viene eseguita un'operazioe di *quantizzazione* nella quale vengono scelti un numero di livelli $L$ detti *livelli di quantizzazione*.
![[stati_di_un_segnale.png]]

Selto il numero di livelli $L$ il nuermo di bit neccasario per rappresentare l'informazione sarà $n_{bit} = log_2(L)$.
Notiamo che questa operazione introduce forzatamente dell'errore.

Un sistema di *registrazione* è quindi comunemente strutturaro così:
![[sistema_di_registrazione.png]]

Più in generale un sistema di *comunicazione*:
![[sistema_di_comunicazinoe.png]]

# Segnali complessi
I segnali che studieremo non saranno solo reale ma anche complessi.$$ z(t) = a(t) + ib(t) = c(t)e^{i\theta(t)}$$
Definiamo subito due già viste grandezze fondamentali *Potenza* ed *Energia*.

**Potenza istantanea normalizzata** di un segnale $x(t)$ $$P(t)=|x|^2(t)$$
**Energia normalizzata** di un segnale $x(t)$ $$ E = \int_{t_i}^{t_f} P(t)dt = \int_{t_i}^{t_f} |x|^2(t)dt $$
Prendiamo in esempio una batteria ideale:
![[segnale_batteria_ideale.png]]
Notiamo subito che la definizione di energia per questo segnale è mal posta in quanto è *infinita*, non da quindi informazioni utili del segnale stesso.

Definiamo la **Potenza Media**: $$ P_{xT} = \frac{E_{xT}}{T} $$
Dove $T$ è l'intervallo di misurazione.
Adesso estendiamola per un segnale qualunque $$ P_{xT} = \lim_{T \to \infty} \frac{1}{T} \int_{-\frac{T}{2}}^{\frac{T}{2}}|x(t)|^2dt$$
Da questa definizione osserviamo che:
- Un segnale ad energia *finita* a potenza median *nulla*
- Un segnale a potenza medina *finita* a energia *infinita*

## Esponenziale monolatero
Prendiamo come segnale di esempio $x(t) = e^{-\frac{t}{T}}u(t)$ 
```desmos-graph
left=-1; right=10;
---
y=e^{-x} | x >= 0
```
Questo segnale ha energia finita: $$ E_x = \int_0^{+\infty} e^{-\frac{2t}{T}}dt = \frac{T}{2}$$
## Seno e Coseno
Prendiamo dei segnali: $$
\begin{cases}
x(t) = A\sin(2\pi f_o t) \\
y(t) = A\cos(2 \pi f_o t) \\
x(t) = y(t - \frac{T_0}{4})
\end{cases}
$$
Le funzioni periodiche hanno energia *infinita* e quindi potenza media *finita*.
$$
P_{\cos} = \frac{1}{T} \int_{-\frac{T}{2}}^{\frac{T}{2}} A^2cos^2(s \pi f_0 t) dt =  \frac{A^2}{T} \int_{-\frac{T}{2}}^{\frac{T}{2}} \frac{1}{2} - \frac{cos(2s \pi f_0 t)}{2} dt = \frac{A^2}{2}
$$
Si dimostra lo stesso per il seno
## Impulso rettangolare
Consideriamo un segnale
$$
x(t) = \text{rect} \left (\frac{1}{T} \right)= \begin{cases} 1, |t| < \frac{T}{2} \\ \frac{1}{2}, |t| = \frac{T}{2} \\ 0, |t| > \frac{T}{2} \end{cases}
$$
si dimostra facilemnte che l'energia di questo segnale è pari a $T$.
