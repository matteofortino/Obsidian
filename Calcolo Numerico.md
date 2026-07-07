# Riepilogo Completo di Calcolo Numerico (Revisionato)

Questo documento riassume i concetti chiave, i metodi, le formule, le condizioni di convergenza, la complessità algoritmica e le stime dell'errore, integrando i sistemi non lineari e correggendo le formule di quadratura.

---

## 1. Teoria degli Errori
### 1.1 Rappresentazione in Virgola Mobile e Precisione
- **Equazione/Formula**: $x = \text{sign}(x) \cdot \beta^b \cdot \sum_{i=1}^{m} \alpha_i \beta^{-i}$ (rappresentazione normalizzata in base $\beta$ con mantissa a $m$ cifre ed esponente $b$).
- **Precisione di Macchina ($u$)**: Limite superiore dell'errore relativo di arrotondamento. 
  - Per *arrotondamento*: $u = \frac{1}{2}\beta^{1-m}$
  - Per *troncamento*: $u = \beta^{1-m}$
- **Formula dell'Errore**: 
  - Errore Totale: $\Delta f = f_{ca}(x_{ca}) - f(x)$ dove interviene sia l'errore algoritmico (operazioni di macchina) sia l'errore trasmesso dai dati.
  - Errore trasmesso (linearizzato): $\Delta d \approx \sum_{i=1}^n \frac{\partial f}{\partial x_i} \Delta x_i$.
  - Coefficiente di amplificazione dell'errore relativo (condizionamento): $\rho_i = \frac{\partial f}{\partial x_i} \frac{x_i}{f(x)}$.

---

## 2. Sistemi di Equazioni Lineari

### 2.1 Metodi Diretti (Gauss, Fattorizzazione LU)
- **Equazione/Metodo**: $Ax = b \implies LUx = b$. Scomposizione della matrice in $A = LU$ (Lower/Upper triangolari).
- **Condizioni di validità**: La fattorizzazione $LU$ classica richiede che tutti i minori principali di testa di $A$ siano non singolari. Se non soddisfatta o per garantire stabilità numerica contro la crescita degli elementi, si usa il **Pivoting** ($PA = LU$).
- **Complessità Algoritmica**: 
  - Fattorizzazione $LU$ (o eliminazione di Gauss): $\approx \frac{2}{3}n^3$ operazioni floating-point (flop).
  - Sostituzione in avanti ($Ly = b$) e all'indietro ($Ux = y$): $2n^2$ flop complessivi.
- **Analisi dell'Errore e Condizionamento**: L'errore relativo sulla soluzione è limitato dal numero di condizionamento della matrice:
  $$\frac{\|\delta x\|}{\|x\|} \le \kappa(A) \frac{\|\delta b\|}{\|b\|}, \quad \text{dove } \kappa(A) = \|A\| \cdot \|A^{-1}\|$$

### 2.2 Metodi Iterativi (Jacobi, Gauss-Seidel)
- **Equazione/Metodo**: Scrittura del sistema come $x^{(k+1)} = B x^{(k)} + c$. 
  - *Jacobi* (splitting $A = D - L - U$): $B_J = D^{-1}(L+U)$
  - *Gauss-Seidel*: $B_{GS} = (D-L)^{-1}U$
- **Condizioni di Convergenza**: Condizione necessaria e sufficiente è che il raggio spettrale della matrice di iterazione sia strettamente minore di uno: $\rho(B) < 1$.
  - *Condizioni sufficienti*: $A$ a dominanza diagonale stretta (garantisce convergenza sia per Jacobi che per GS). Se $A$ è simmetrica e definita positiva, Gauss-Seidel converge sempre.
- **Ordine di Convergenza**: Lineare ($p=1$). La velocità di convergenza asymptotica dipende da $R = -\log_{10}(\rho(B))$.
- **Formula dell'Errore**: 
  - Errore all'iterazione $k$: $e^{(k)} = B^k e^{(0)}$.
  - Stima a posteriori: $\|x^{(k)} - x\| \le \frac{\|B\|}{1 - \|B\|} \|x^{(k)} - x^{(k-1)}\|$.

---

## 3. Equazioni e Sistemi Non Lineari

### 3.1 Metodo di Bisezione (Scalare)
- **Metodo**: $x_n = \frac{a_n + b_n}{2}$ su intervalli dimezzati progressivamente.
- **Condizioni di Convergenza**: $f(x)$ continua in $[a, b]$ e $f(a)\cdot f(b) < 0$ (Teorema degli zeri). La convergenza è globale nell'intervallo.
- **Ordine di Convergenza**: Lineare ($p=1$) con fattore di riduzione costante $C = 0.5$.
- **Formula dell'Errore**: $|x_n - \alpha| \le \frac{b-a}{2^{n+1}}$.

### 3.2 Metodo di Newton (o delle Tangenti - Scalare)
- **Equazione/Metodo**: $x_{n+1} = x_n - \frac{f(x_n)}{f'(x_n)}$
- **Condizioni di Convergenza**: $f \in C^2$, $f'(\alpha) \neq 0$ (radice semplice), e punto iniziale $x_0$ sufficientemente vicino alla radice $\alpha$ (convergenza locale).
- **Ordine di Convergenza**: 
  - Quadratica ($p=2$) per radici semplici.
  - Lineare ($p=1$) per radici multiple di molteplicità $m$ (l'ordine torna quadratico usando il metodo modificato: $x_{n+1} = x_n - m \frac{f(x_n)}{f'(x_n)}$).
- **Formula dell'Errore**: $x_{n+1} - \alpha \approx \frac{f''(\alpha)}{2 f'(\alpha)} (x_n - \alpha)^2$.

### 3.3 Metodo delle Secanti (Scalare)
- **Equazione/Metodo**: $x_{n+1} = x_n - f(x_n) \frac{x_n - x_{n-1}}{f(x_n) - f(x_{n-1})}$
- **Condizioni di Convergenza**: Convergenza locale, richiede due punti iniziali $x_0, x_1$ vicini a $\alpha$.
- **Ordine di Convergenza**: Superlineare ($p = \frac{1+\sqrt{5}}{2} \approx 1.618$).
- **Complessità**: Richiede solo 1 nuova valutazione di funzione per passo (non calcola la derivata).

### 3.4 Metodo di Newton-Raphson per Sistemi Pluridimensionali
- **Equazione del metodo**: Per risolvere un sistema di $n$ equazioni in $n$ incognite $F(x) = 0$ (con $F: \mathbb{R}^n \to \mathbb{R}^n$):
  1. Si risolve il sistema lineare per la correzione $s^{(k)}$:  
     $$J_F(x^{(k)}) s^{(k)} = -F(x^{(k)})$$
  2. Si aggiorna la soluzione:  
     $$x^{(k+1)} = x^{(k)} + s^{(k)}$$
  dove $J_F(x)$ è la **matrice Jacobiana** definita da $(J_F)_{ij} = \frac{\partial f_i}{\partial x_j}$.
- **Condizioni per la convergenza e ordine**:
  - *Convergenza*: Locale. Richiede che $F \in C^2$ in un intorno dello zero $\alpha$, che lo Jacobiano $J_F(\alpha)$ sia invertibile (non singolare) e che il vettore iniziale $x^{(0)}$ sia abbastanza vicino ad $\alpha$.
  - *Ordine di convergenza*: Quadratica ($p = 2$).
- **Complessità algoritmica**: Ad ogni iterazione occorre calcolare ed elementarizzare $n^2$ derivate parziali, valutare le $n$ funzioni del vettore $F$ e risolvere un sistema lineare pieno $n \times n$. Costo computazionale della risoluzione lineare: $O(n^3)$ flop per iterazione.
- **Formula dell'errore**: $\|x^{(k+1)} - \alpha\| \le C \|x^{(k)} - \alpha\|^2$.

### 3.5 Metodo di Jacobi-Newton (Sistemi)
- **Equazione del metodo**: Variante "quasi-Newton" o di disaccoppiamento usata per evitare la risoluzione del sistema lineare completo $n \times n$. Ogni componente viene aggiornata indipendentemente applicando un singolo passo di Newton unidimensionale rispetto alla propria variabile:
  $$x_i^{(k+1)} = x_i^{(k)} - \frac{f_i(x_1^{(k)}, \dots, x_n^{(k)})}{\frac{\partial f_i}{\partial x_i}(x_1^{(k)}, \dots, x_n^{(k)})}, \quad \text{per } i = 1, \dots, n$$
- **Condizioni per la convergenza e ordine**:
  - *Convergenza*: Locale. Richiede che lo Jacobiano del sistema sia fortemente a dominanza diagonale nell'intorno della soluzione, in modo che l'operatore di punto fisso associato risulti una contrazione.
  - *Ordine di convergenza*: Lineare ($p = 1$).
- **Complessità algoritmica**: Estremamente bassa. Non risolve sistemi lineari accoppiati, richiede solo il calcolo dei componenti sulla diagonale principale dello Jacobiano ($\frac{\partial f_i}{\partial x_i}$). Complessità algebrica per passo: $O(n)$ invece di $O(n^3)$.
- **Formula dell'errore**: $\|x^{(k+1)} - \alpha\| \le L \|x^{(k)} - \alpha\|$ con costante di contrazione $0 \le L < 1$.

---

## 4. Calcolo di Autovalori e Autovettori

### 4.1 Metodo delle Potenze
- **Equazione/Metodo**: $y^{(k)} = A x^{(k-1)}$, seguito da normalizzazione $x^{(k)} = \frac{y^{(k)}}{\|y^{(k)}\|}$. Calcola l'autovalore dominante $\lambda_1$.
- **Condizioni di Convergenza**: Matrice diagonalizzabile con un unico autovalore di modulo strettamente massimo: $|\lambda_1| > |\lambda_2| \ge \dots \ge |\lambda_n|$.
- **Ordine di Convergenza**: Lineare ($p=1$). La velocità dipende dal rapporto di separazione $\left|\frac{\lambda_2}{\lambda_1}\right|$.
- **Complessità**: Un prodotto matrice-vettore per iterazione: $O(n^2)$ operazioni (se $A$ è piena).

---

## 5. Interpolazione e Approssimazione

### 5.1 Interpolazione Polinomiale di Lagrange e Newton
- **Equazione/Metodo**: Polinomio $P_n(x)$ di grado $\le n$ su $n+1$ nodi. Formulazione di Newton tramite differenze divise:
  $$P_n(x) = f[x_0] + f[x_0, x_1](x-x_0) + \dots + f[x_0, \dots, x_n]\prod_{i=0}^{n-1}(x-x_i)$$
- **Formula dell'Errore**: 
  $$E_n(x) = f(x) - P_n(x) = \frac{f^{(n+1)}(\xi)}{(n+1)!} \omega_n(x), \quad \text{dove } \omega_n(x) = \prod_{i=0}^n (x - x_i)$$
- **Criticità (Fenomeno di Runge)**: Su nodi equispaziati, al crescere di $n$, l'errore può divergere vistosamente ai bordi dell'intervallo. Soluzione: uso dei **nodi di Chebyshev**.

---

## 6. Integrazione Numerica (Formule di Newton-Cotes)

Le formule di Newton-Cotes approssimano $\int_a^b f(x) dx$ interpolando $f(x)$ su nodi equispaziati.

### 6.1 Formula dei Trapezi
- **Forma Semplice** (1 intervallo $[a,b]$, nodi $x_0=a, x_1=b$, ampiezza $h = b-a$):
  $$\int_a^b f(x) dx \approx \frac{b-a}{2} [f(a) + f(b)]$$
  - *Grado di precisione*: 1 (esatta per polinomi di grado $\le 1$).
  - *Formula dell'Errore Semplice*: $E_T = -\frac{(b-a)^3}{12} f''(\xi) = -\frac{h^3}{12} f''(\xi)$.
- **Forma Composta** (Suddivisione di $[a,b]$ in $n$ sottointervalli di ampiezza $h = \frac{b-a}{n}$, nodi $x_i = a + ih$):
  $$\int_a^b f(x) dx \approx \frac{h}{2} \left[ f(a) + 2 \sum_{i=1}^{n-1} f(x_i) + f(b) \right]$$
  - *Formula dell'Errore Composto*: $E_{TC} = -\frac{b-a}{12} h^2 f''(\xi) = O(h^2)$.

### 6.2 Formula di Simpson (Cavalieri-Simpson)
- **Forma Semplice** (1 intervallo $[a,b]$ diviso in due metà da $x_1 = \frac{a+b}{2}$. Ponendo $h = \frac{b-a}{2}$):
  $$\int_a^b f(x) dx \approx \frac{h}{3} [f(a) + 4f(x_1) + f(b)] = \frac{b-a}{6} \left[ f(a) + 4f\left(\frac{a+b}{2}\right) + f(b) \right]$$
  - *Grado di precisione*: 3 (esatta per polinomi di grado $\le 3$ grazie alla simmetria).
  - *Formula dell'Errore Semplice*: $E_S = -\frac{h^5}{90} f^{(4)}(\xi) = -\frac{(b-a)^5}{2880} f^{(4)}(\xi)$.
- **Forma Composta** (Suddivisione in un numero **pari** di sottointervalli $n = 2m$ di ampiezza $h = \frac{b-a}{2m}$):
  $$\int_a^b f(x) dx \approx \frac{h}{3} \left[ f(x_0) + 4 \sum_{j=1}^{m} f(x_{2j-1}) + 2 \sum_{j=1}^{m-1} f(x_{2j}) + f(x_{2m}) \right]$$
  - *Formula dell'Errore Composto*: $E_{SC} = -\frac{b-a}{180} h^4 f^{(4)}(\xi) = O(h^4)$.

---

## 7. Metodi Numerici per Equazioni Differenziali Ordinarie (ODE)

### 7.1 Metodi a un Passo (Problemi ai Valori Iniziali - Cauchy)
- **Eulero Esplicito**: $y_{n+1} = y_n + h f(t_n, y_n)$. Ordine locale $O(h^2)$, ordine globale $O(h)$ ($p=1$).
- **Eulero Implicito**: $y_{n+1} = y_n + h f(t_{n+1}, y_{n+1})$. Ordine $p=1$, ma incondizionatamente stabile (A-stabile).
- **Runge-Kutta (RK4 classico)**: Metodo a 4 stadi, ordine globale $p=4$. Ottimo compromesso accuratezza/stabilità.
- **Condizioni di Convergenza**: Garantita dal *Teorema di Equivalenza di Lax*: Consistenza (errore locale di troncamento infinitesimo per $h \to 0$) + Zero-stabilità (condizione di Lipschitz sulla funzione di incremento).

### 7.2 Metodi Multistep e Barriere di Dahlquist
- **Equazione**: $\sum_{j=0}^k \alpha_j y_{n+j} = h \sum_{j=0}^k \beta_j f_{n+j}$.
- **Stabilità e Barriere**:
  - *Prima Barriera*: Un metodo lineare a più passi stabile di ordine $k$ non può avere ordine di consistenza $p > k+2$ (se $k$ è pari) o $p > k+1$ (se $k$ è dispari).
  - *Seconda Barriera*: Nessun metodo lineare multistep *esplicito* può essere A-stabile. Inoltre, il massimo ordine per un metodo implicito A-stabile è $p = 2$ (la formula trapezoidale, o di Crank-Nicolson, è la più accurata tra questi).
- **Metodi Predictor-Corrector**: Coppie di metodi (un esplicito per predire $\hat{y}_{n+k}$ e uno implicito per correggerlo). Spesso implementano formule basate sulla **correzione di Milne** per stimare accuratamente l'errore locale ad ogni passo senza valutazioni aggiuntive.