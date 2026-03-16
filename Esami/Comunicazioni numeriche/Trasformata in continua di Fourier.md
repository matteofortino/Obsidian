#uni 
Dato un segnate $x(t)$ ad *energia finita*, si definisce **TCF** di x(t)
>[!Trasformata di Fourier]
>$$ X(f) = \int_{- \infty}^{+ \infty} x(t)e^{-j2 \pi ft}dt $$

anche detta *equazione di analisi*, in quanto permetti di passare allo **spettro delle frequenze** del segnale per analizzarlo
Dualemente di definisce l'antitrasformata
>[!Antitrasformata di Fourier]
> $$ x(f) = \int_{- \infty}^{+ \infty} X(f)e^{j2 \pi ft}df $$

anche detta *equazione di sintesi*, in quanto permette di ricostruire un segnale partendi dallo spettro delle frequenze.

# Modulo e fase
In quanto la trasformata di un segnale è sempre complessa possiamo deviderla nelle sue componenti polari *modulo* e *fase*: $$ X(f) = |X(f)|e^{\angle X(f)} $$ dove: 
- $|X(f)|$ ci da lo spettro di *ampiezza* del segnale
- $\angle X(f)$ lo spettro di *fase* del segnale.

Per i segnali $x(t) \in \mathbb{R}$ valgono le seguenti simmetrie:
- $|X(-f)| = |X(f)|$, simmetria *pari*
- $\angle X(-f) = -\angle X(f)$ , simmetri *dispari*
L'insieme di queste due proprietà è definito come **simmetria hermitiana** che generalizzata $$ X^*(f) = \int \left ( x(t)e^{- 2 j \pi f t} \right )^* dt = \int x(t)e^{2 j \pi f t}dt  = X(-f) $$
Vediamo degli esempi con alcuni segnali importanti
## Impulso rettangolare
Ricordiamo la definizione$$
x(t) = 
\begin{cases}
	1, \quad |t| < \frac{\tau}{2} \\
	\frac{1}{2}, \quad |t| = \frac{\tau}{2} \\
	0, \quad |t| > \frac{\tau}{2}
\end{cases}
$$

e calcoliamo la trasformata $$
X(f) = \int_{-\infty}^{\infty} x(t) e^{-i 2 \pi ft} \, dt = \int_{-\frac{\tau}{2}}^{\frac{\tau}{2}} e^{-i 2 \pi f t} \, dt
= - \frac{1}{i 2 \pi f} e ^{-i 2 \pi f t} \Big|^{\frac{\tau}{2}}_{-\frac{\tau}{2}} = - \frac{1}{i 2 \pi f} \left( e^{-i \pi f \tau} - e^{i 2 \pi f \tau} \right)
$$
$$
= - \frac{1}{i 2 \pi f} \cdot - 2 i \sin (\pi f t) = \frac{\sin(\pi f \tau)}{\pi f} 
$$

Se definiamo **seno cardinale** $$
\mathrm{sinc}(\alpha) = \frac{\sin(\pi \alpha)}{\pi \alpha} \ \forall \alpha \neq 0, \quad \mathrm{sinc}(0) = 1
$$ 
Abbiamo quindi trovato la trasformata *fondamentale*:
$$
x(t) = \mathrm{rect} \left( \frac{t}{\tau} \right) \, \Leftrightarrow \, X(f) = \frac{\sin(\pi f \tau)}{\pi f} \cdot \frac{\tau}{\tau} = \tau \mathrm{sinc}(f \tau)
$$
## Esponenziale monolatero
Definito come $$
x(t) = e^{-\frac{t}{\tau}} \cdot u(t)
$$
dove $u(t)$ è il comune gradino di Heaviside.
Questo si ottiene direttamente applicando la definizione di trasformata di Fourier:
$$
X(f) = \int_{-\infty}^{\infty} x(t) e^{-i 2 \pi f t} \, dt = \int_0^{\infty} e^{-\frac{t}{\tau}} e^{-i 2 \pi f t} \, dt = \int_0^\infty e^{-t\left( \frac{1}{\tau} + i 2 \pi f \right)} \, dt = \frac{e^{-t\left( \frac{1}{\tau} + i 2 \pi f \right)}}{\frac{1}{\tau} + i 2 \pi f} \Big|^{\infty}_0
$$
$$
= \frac{\tau}{1 + i 2 \pi f \tau}
$$
# Teorema di Parseval 
Riguardo alla trasformata di Fourier e alle definizione di energia e potenza enunciamo l'importate
>[!Teorema di Parseval]
>$$ E_x = \int |x(t)|^2 dt  = \int |X(f)|^2 df$$

Il quale eguaglia l'energia di un segnale nello spettro del tempo a quello delle frequenze. 
Questo si può dimostrare prendendo l'energia del segnale in dominio tempo:
$$
E_x = \int_{-\infty}^{\infty} x(t) x^*(t) \, dt
$$
e riportando la $x^*(t)$ nella forma di sintesi vista prima:
$$
= \int_{-\infty}^{\infty} x(t) \left( \int_{-\infty}^{\infty} X^*(f) e^{-i 2 \pi f t} \, df  \right) dt
= \int_{-\infty}^{\infty} X^*(f) \left( \int_{-\infty}^{\infty} x(t) e^{-i 2 \pi f t} \, dt \right) df  
$$
$$
= \int_{-\infty}^{\infty} X^*(f) X(f) \, df
$$
Chiamiamo quindi $\mathcal{E}_x(f)$ *densità spettrale di energia*:
$$
\mathcal{E}_x(f) = |X(f)|^2
$$
Riguardo alla *potenza*, vale invece:
$$
P_x = \lim_{T \rightarrow \infty} \frac{E_{xT}}{T} = \lim_{T \rightarrow \infty} \int \frac{|X(f)|^2}{T} \, df = \int \lim_{T \rightarrow \infty} \frac{|X(f)|^2}{T} \, df
$$
da dove troviamo la definizione di $\mathcal{P}_x(f)$ di *densità spettrale di potenza*:
$$
\mathcal{P}_x(f) = \frac{|X(f)|^2}{T}  
$$
