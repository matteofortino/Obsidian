#uni 
Partiamo subito enunciano il **principio fondamentale del calcolo combinatorio**

Se tutti i $k_i$ sono uguali allora $M = K^N$.

Presi invece N oggetti e disposti k a k, il numero totale di *disposizioni semplici* (ovvero senza ripetizioni): $$ D_{N,k} = \frac{N!}{(N - k)!}$$
il numero di permutazioni semplici invece è $$ P_{N,k} = N! $$
il numero di combinazioni semplici è $$ C_{N,k} = \frac{D_{N,k}}{k!} = \frac{N!}{k!(N - k)!} = \binom{N}{k} $$
# Esperimento aleatorio composto
Dati due eventi $A_1$ e $A_2$, aventi spazi campioni $\Omega_1$ e $\Omega_2$. Vogliamo calcolare la probabilità $P[A_1 \cdot A_2$].
In generale non esiste una formula ma nel caso di eventi indipendenti si dimostra che $$ P[A_1 \times A_2] = P[A_1] \cdot P[A_2] $$
considerando come spazio campione $\Omega = \Omega_1 \cdot \Omega_2$ (prodotto cartesiano fra insiemi)
Questo succede perchè sono indipendenti tutti gli eventi del tipo $$ (A_1 \times \Omega_2), (\Omega_1\times A_2) $$
# Prove ripetute binarie e indipendenti
Consideriamo un esperimento aleatorio con uno spazio campione $\Omega = \{A, \overline A \}$, costituito da due risultati soltanto: $P(A) = p$ e $P(\overline A) = 1 - p = q$.

Consideriamo poi l'esperimento composto ottenuto ripentendo $N$ volte lo stesso esperimento e facendo in modo che ciascuna prova sia **indipendente** dalle altre.

Vogliamo calcolatore $P(E) (probabiltà che $A$ si verifichi $k$ volte in un ordine qualsiasi). Si dimostra che la probabiltà del verificarsi di una singola sequenza è 
$$P[E_i] = p^kq^{N-k}$$
La probabiltà totale: 
$$
  P[E] = \binom{N}{K}p^kq^{N-k}
$$
Conosciuta anche come **formula di Bernoulli**.