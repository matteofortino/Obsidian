#uni 
Iniziamo a vedere semplici circuiri in logica a diodi, a partite dalla realizzazione di alcune **porte logiche**. 

# Porta AND
Prendiamo in esame il circuito che implementa una porta **AND**. 
![[circuito_porta_AND.png]]
Abbiamo quindi due diodi collegati ai capi $A$ e $B$, dei queli assumiamo di poter variare il voltatto $V_A$ e $V_B$. $V_{cc}$ è invece la sorgete di alimentazione.

| $V_A$ | $V_B$ | $V_u$ |
| ----- | ----- | ----- |
| 5V    | 5V    | 5V    |
| 0V    | 5V    | 0V    |
| 5V    | 0V    | 0V    |
| 0V    | 0V    | 0V    |

Queste sono tutte le possibili combinazioni. 
Condideriamo i seguenti casi: 
- $V_A = 5V$, $V_B = 5V$, $V_{cc} = 5V$ la situazione si modellizza come segue:
	![[circuito_AND_entranbi_interdetti.png]]
	Abbiamo quindi che entrabi i diodi hanno un voltaggio a destra pari a $5V$. Notiamo che a sinistra avranno una tensione $V' = V_{cc} - IR < V_{cc}$ , come ipotesi possiamo quindi assumenre che i diodi siamo interdetti.
	A questo punto se i diodi sono interdetti nel circuito non scorre corrente, non vi è quindi nessuna caduta di pontezile sulla resistenza $R$ da cui si può ricavere il valore di $V_u = 5V$
	**Verifica**:
	I diodio sono stati presi interdetti quindi a voltaggio zero o a polarizzazione inversa. Si ha quindi che $V_{D_A}, V_{D_B} < 0V \Rightarrow V_{D_A}, V_{D_B} < V_{\gamma} \approx 0.7V$ 

- $V_A = 0V$, $V_B = 5V$, $V_{cc} = 5V$ la situazione si modelizza come segue: 
	![[circuito_AND_interdetto_e_conduzione.png]]
	Per il diodo $D_{B}$ valfono le considerazione fatte prima quindi lo ipotiziamo come circuito aperto.
	Pe il diodo $D_A$ adesso abbiamo che a destra a la tensione più bassa in assoluto $0V$, è quindi ragionevole ipotizzarlo in conduzione quindi come un cortocircuito.
	Si nota immediatamente che adesso $V_u$ e la terra sono collegati in cortocircuito quindi $V_u = 0$ 
	**Verifica**: 
	Il diodo $D_A$ è stato preso in conduzione ed essendoci adesso un percorso chiuso scorre nel circuito una corrente $I_{D_A} = \frac{V_{cc}}{R} > 0$
	Per il diodio $D_B$ si applica il ragionamento di prima $V_{AK} = V_{D_B} = -5V < V_{\gamma}$ 

Date queste due prove si dimostra immediatamente che fino a che uno dei due diodio è collegato a terra la tensione relativa a $V_u$ sarà sempre zero volt.

# Porta OR
Vediamo adesso il circuito che rappresenta una porta OR:
![[circuito_porta_or.png]]
In modo analogo a come fatto precedentemente possiamo ricavare la tabella di valori di $V_u$ in base ai valori di $V_A$ e $V_B$. 

| $V_A$ | $V_B$ | $V_u$ |
| ----- | ----- | ----- |
| 5V    | 5V    | 5V    |
| 0V    | 5V    | 5V    |
| 5V    | 0V    | 5V    |
| 0V    | 0V    | 0V    |
Questo perchè fino a che uno dei due diodi ha a sinistra una tensione pari a $5V$ sarà in conduzione in quanto a destra è attacco a massa. 

La nostra uscita $V_u$ è quindi collagata in corto circuito ad una sorgente di potenzile quindi $V_u = 5V$.
Solo quando entrambe i diodi sono a terra allora entrambi saranno interdetti a $V_u = 0$
