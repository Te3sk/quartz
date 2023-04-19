---
title: Sequenze Random e prevedibili
tags: #critto
---
##### 2022/09/27 #date 
## Teoria algoritmica della casualità
$$
\begin{array}{c}
	\textit{"Nel lanciare una moneta 100 volte, le sequenze regolari, sono}\\
	\textit{incomparabilmente meno numerose delle irregolari"}\\
	\text{- La Place}\\
\end{array}
$$
> [!example] Example 
> 
> Data la sequenza $\textcolor{#ff82b2}{\textbf{h }}=11111111111111111111$
> 
> La probabilità che da 20 lanci esca sempre testa (sequenza $\textcolor{#ff82b2}{h}$) è $\textcolor{#f77ead}{20^{-20}}=(\frac{1}{2})^{20}$, ed è uguale a quella di ogni altra sequenza "irregolare" (sequenza $\textcolor{#ff82b2}{h'}$)
> 
> $\textcolor{#f77ead}{A_h}$: Algoritmo che genera la sequenza *h* (di lunghezza $\textcolor{#ff82b2}{n}$)
> 
> Per generare la sequenza $\textcolor{#ff82b2}{h}$ abbiamo:
> 
> $A_h<h\text{genera \textcolor{#f77ead}{n 1}}$ 
> 
> $|h|=n$
> $$\textcolor{#f77ead}{A_h} = O(\log{n}) + O(1) = O(\log{n})$$
> Dove $\textcolor{#f77ead}{|A_h|}$ è la lunghezza della codifica binaria di $A_h$ e $\textcolor{#f77ead}{O(\log{n})}$ serve per scrivere *n*.
> Per generare la sequenza *h'* abbiamo:
> $$\textcolor{#f77ead}{|A_{h'}|}=n + O(1)=O(1)$$

<span style="color:#ff82b2"><b>Informalmente,</b></span> diciamo che una sequenza binaria è detta <span style="color:#ff82b2"><b>casuale</b></span> se <span style="color:#ff82b2"><i>NON</i></span> ammette un algoritmo di generazione la cui rappresentazione binaria sia più corta della sequenza stessa
## Sistemi / Modelli di calcolo
I [[Glossary#Sistemi di Calcolo|sistemi di calcolo]] sono <span style="color:#ff82b2"><b>numerabili</b></span> $\implies \exists\:S_1,S_2,S_3,...S_i,...$
Scegliamo il sistema $\textcolor{#f77ead}{S_i}$, dove $\textcolor{#ff82b2}{p}$ è il programma di $S_i$ che genera la sequenza $\textcolor{#ff82b2}{h}$
> [!quote] Complessità di Kolmogorv di h nel sistema $S_i$
> 
> $$
> \textcolor{#f77ead}{K_{S_i}(h)}=\min\{|p|\text{t.c.}S_i(p)=h\}
> $$

Consideriamo il <span style="color:#ff82b2"><b>Sistema di Calcolo universale</b></span> $\textcolor{#f77ead}{S_u}$, il quale può simulare qualsiasi altro sistema di calcolo. Se voglio la sequenza $\textcolor{#ff82b2}{h}$ con $\textcolor{#f77ead}{S_n}$, posso simulare $\textcolor{#f77ead}{S_i}\Rightarrow$
$$
\begin{array}{lcr}
	S_n(<i,p>)=S_i(p)=h &&&
	q = <i,p>
\end{array}
$$
dove:
- $\textcolor{#f77ead}{i}$ è l'indice del sistema di calcolo da simulare
- $\textcolor{#f77ead}{p}$ è il programma per $S_i$
- $\textcolor{#f77ead}{q}$ è il programma che genera $h$ in $S_n$
$$
\begin{array}{c}
	\begin{array}{lr}
		S_i(p)=h &&
		S_n(q)=S_n(<i,p>)=h
	\end{array} \\\\
	|q|=|i|+|p|=|p|+O(log\,(i))=|p|+c_i \\\\
	K_{S_n}(h)\leq K_{S_i}(h)+c_i
\end{array}
$$
Dove:
- $\textcolor{#f77ead}{O(log\,(i))} = \textcolor{#f77ead}{c_i}$ dipende da $i$ e non dalla sequenza $h$
- Nell'ultima disequazione, scriviamo=
	- $\textcolor{#f77ead}{=}$ per le sequenze generate simulando $S_i$ non avendo programmi più brevi per $S_i$
	- $\textcolor{#f77ead}{<}$ per le sequenze generate con programmi più brevi, simulando sistemi di calcolo diversi da $S_i$
> [!quote] Sequenza Casuale secondo Kolmogorov
> 
> Una sequenza *h* è [[Glossary#Casualità|casuale]] se $K(h)\geq |h|-\lceil log_2\,|h|\rceil$

### Esistenza di sequenze casuali (di ogni lunghezza)
> [!abstract] Theorem
> 
> $\forall n,\exists$ sequenze casuali di lunghezza $\textcolor{#ff82b2}{n}$


 <span style="color:#ff82b2"><b>Dimostration:</b></span>
 Poniamo
 
 $$
 \begin{array}{c}
 \textcolor{#f77ead}{S} = \text{\#sequenze di lunghezza } n=2^n \\
 \textcolor{#f77ead}{T} = \text{\#sequenze NON casuali di lunghezza } n
 \end{array}
 $$
 
 L'obiettivo è dimostrare che $T<S$
 
 $$
 \begin{array}{cc}
 	\textcolor{#f77ead}{N}=\text{\#sequenze di lunghezza minore di } n - \lceil log n \rceil = \\
 	=\sum_{i=0}^{n-\lceil log\;n\rceil} 2^i = 2^{n-\rceil log\;n\rceil}-1 &
 \end{array}
 $$
 
 La risultante è una serie geometrica e le $N$ sequenze contengono anche tutti i programmi che generano le $T$ sequenze non casuali
 
 $$
 \begin{array}{l}
 	\implies S=2^n\qquad\text{e}\qquad N = 2^{n-\lceil log\;n\rceil - 1} \implies N < S \implies \\
 	\implies T \leq N \implies T \leq N < S \implies \textcolor{#f77ead}{\textbf{T}<\textbf{S}}
 \end{array}
 $$
$\implies \boldsymbol{\textcolor{#f77ead}{\exists}}$ <span style="color:#ff82b2"><b>sequenze casuali di lunghezza</b></span> $\textcolor{#f77ead}{\text{\textbf{n}}\boldsymbol\;{\forall}\;\textbf{n}}$
##### 2022/10/03 #date 
## Teorema
> <i>Il problema di stabilire se una sequenza è casuale secondo Kolmogorov è </i> *[[Glossary#Problemi indecidibili|indecidibile]]*

<span style="color:#ff82b2"><b>Dim:</b></span> Per assurdo, esista un algoritmo `RANDOM` t.c.
$$
RANDOM(A)= \begin{cases}
	0 & \text{se non è casuale} \\
	1 & \text{se è casuale}
\end{cases}
$$
e lo passiamo ad un algoritmo `PARADOSSO`
```
PARADOSSO(A)
	for binary h<-1 to ∞ do {
		if (|h| - log_2(|h|) > |P| && RANDOM(h) == 1)
			return(h)
	}
```
Dove `P` è la sequenza binaria che rappresenta il programma complessivo, e $|P|=|PARADOSSO|+|RANDOM|$. Notiamo che il programma paradosso termina perché esistono sequenze casuali di ogni lunghezza. Nell'`if` le condizioni sono:
1. `h` è casuale
2. $|h|-\lceil log h \rceil > |P|$
`h` non può però soddisfarre entrambe le condizioni contemporaneamente, per la definizione di sequenza casuale secondo Kolmogorov. Si dovrebbe quindi generrare un algoritmo che è sia casuale che non casuale 
	$\implies \nexists$ l'algoritmo `Random`
## Generazione di sequenze Casuali binarie
> [!quote] Sequenza binaria casuale
> 
> Sequenza di bit t.c.
> 1. $P(0)=P(1)=\frac{1}{2}$ 
> 		$\textcolor{#f77ead}{\rightarrow}$ <span style="color:#ff82b2"><i>cambiabile con</i></span> $\textcolor{#f77ead}{\rightarrow}P(0)>0 \wedge P(1)>0 \text{ e immutabili durante il processo}$
> 2. La generazione di un bit è indipendente da quello dei bit precedenti

> [!example] Example 
> 
> Consideriamo la sequenza $111111010011011110110001$, posso non considerare le coppie $00$ e $11$ e sostituire $01\to 0$ e $10\to 1$
> $$
> 	111111 \textcolor{#f77ead}{01}0011 \textcolor{#f77ead}{01} 11 \textcolor{#f77ead}{10} 1100 \textcolor{#f77ead}{01}
> $$
> Ottengo quindi la sequenza casuale $\textcolor{#ff82b2}{0010}$ dove $\textcolor{#ff82b2}{0}$ e $\textcolor{#ff82b2}{1}$ sono <span style="color:#ff82b2"><i>equiprobabili</i></span>

### Generatori Lineari
<span style="color:#ff82b2"><b>Input:</b></span> <span style="color:#ff82b2"><i>Seme</i></span> (sequenza base, scelta casualmente) $$
	x_0 \Rightarrow x_i=(a_ix_{i-1}+b)\text{ mod }m
$$
Dove $a, b, m$ sono detti <span style="color:#ff82b2"><i>parametri di generazione</i></span>. Con lo stesso seme verrà creata la setsa sequenza.
> [!example] Example 
> 
> $a=7$, $b=7$, $m=9$, $x_0=3$
> $\rightarrow$ $x_i=(7x_{i-1}+7)\text{ mod } 9$
> $\rightarrow$ $\textcolor{#f77ead}{3},1,5,6,4,8,0,7,2,\textcolor{#f77ead}{3},...$
> Condinuando ottengo la ripetizione della stessa sequenza

Il meglio che posso fare con il generatore lineare è generare tutti i valori compresi tra $0$ e $n-1$. Per ottenere sempre <span style="color:#ff82b2"><i>almeno una permutazione</i></span>, si pongono delle condizioni al generatore lineare:
1. $MCD(b,m) =1 \rightarrow b$ e $m$ [[Glossary#Coprimi|coprimi]]
2. $(a-1)$ dividibile $\forall$ fattore primo di $m$
3. $(a-1)$ deve essere $<4$ se anche $m$ lo è
### Genereatore polinomiale
$$
x_i=(a_ix_{i-1}^b+a_2x_{i-1}^{t-1}+\dots+a_tx_{i-1}+a_{t+1})\text{ mod } m
$$
I generatori lineari e polinomiali superano i test statistici. Considerando un <span style="color:#ff82b2"><i>seme da</i></span> $\textcolor{#f77ead}{s}$ <span style="color:#ff82b2"><i>bit</i></span>, possiamo generare al più $\textcolor{#f77ead}{2^s}$ <span style="color:#ff82b2"><i>sequenze</i></span>, considerando poi un <span style="color:#ff82b2"><i>periodo</i></span> lungo $\textcolor{#f77ead}{m}$, si ottengono $\textcolor{#f77ead}{2^m}$ <span style="color:#ff82b2"><i>sequenze</i></span> ($s<<m\implies 2^s<<2^m$).
## Generatori crittograficamente sicuri
Un generatore crittograficamente sicuro se supera il <span style="color:#ff82b2"><b>test di prossimo bit</b></span>
> [!quote] Test di Prossimo bit
> 
> Un generatore supera il test di prossimo bit se $\nexists$ un algoritmo polinomiale in geado di prevedere l'$(i+1)$-esimo bit della sequenza, a partire dalla conoscenza degli $i$ bit precedenti già generati, con probabilità $>\frac{1}{2}$.

### Tecnica 1: Costruzioni con funzioni One-Way
[[Glossary#Funzioni one-Way|Funzioni One-Way]]
$$
x_0\text{, } x_1=f(x_0)\text{, }x_2=f(x_1)=f(f(x_0))=f^2(x_0),...\implies x_i=f(x_{i-1})=f^i(x_0)
$$
Da $x_i$ posso generare $x_{i+1}=f(x_i)$ in tempo polinomiale, è <span style="color:#ff82b2"><i>difficile</i></span> risalire a $x_i$ da $x_{i+1}\Leftarrow x_i=f^{-1}(x_i+1)$.
L'<span style="color:#ff82b2"><b>idea</b></span> è quella di generare la sequenza e consumarla arl contrario.
## Generatori BBS
Supera il test di prossimo bit:
- $f$:quadrato in modulo $\to$ <span style="color:#ff82b2"><i>facile</i></span>
- $f^{-1}$: radice in modulo $\to$ <span style="color:#ff82b2"><i>difficile</i></span> se si lavora in <span style="color:#ff82b2"><i>modulo di un numero complicato</i></span> 
$n=p\cdot q$ dove $p$ e $q$ sono numeri primi t.c. $p \text{ mod } 4=q \text{ mod } 4=3\implies MCD(2\lfloor q/2\rfloor+1,2\lfloor p/2\rfloor+1)=1$
1. Si sceglie $y$ coprimo con $n$ e si calcola $\textcolor{#f77ead}{x_0=y^2 \text{ mod } n}$, usiamo $x_0$ come <span style="color:#ff82b2"><b>seme</b></span>
2. Si calcola la sequenza di $m<n$ interi: $\textcolor{#f77ead}{x_i=(x_{i-1})^2 \text{ mod } n}$
3. Si usa un predicato <span style="color:#ff82b2"><i>hard-core</i></span> della funzione one-way se:
	- $b(x)$ è un predicato hard-core se:
	- $b(x)$ è facile da calcolare conoscendo $x$
	- $b(x)$ è difficile da prevedere (con prob $>\frac{1}{2}$) se si conosce $f(x)$
> [!example] Example 
> 
> $$
> \begin{array}{l}
> 	n=77=7\cdot 11\\
> 	y=9\to x_0=9^2 \text{ mod } 77=4 \\
> 	y = 10\to x_0 = 23 \\
> 	\text{Priorità di }x\text{ in BBS }\rightarrow x_i=(x_{i-1})^2 \text{ mod } n \rightarrow b(x) = \text{Priorità di } x_i &&&&&&&&&&&&&&&&&&
> \end{array}
> $$

##### 2022/10/04 #date 
## Generatore Pseudocasuale basato su Cifrari Simmetrici
$r$ = # bit delle parole prodotte dal cifrario
$m$ = # parole da produrre (sequenze di $r\cdot m$ bit)
$s$ = seme casuale di $r$ bit
$k$ = chiave segreta del cifrario
```
GENERATORE(s,m)
	d = rappresentazione in r bit di data e ora;
	y = C(d,k);
	z = s;
	for (i = 1; i <= m; i++) {
		x_i = C(y⊕z, k);         //⊕$\rightarrow$XOR bit a bit
		z = C(y⊕x_i, k);
		comunica x_i all'esterno;
	}
```
## Problema di testare se un numero è primo
Il <span style="color:#ff82b2"><i>brute force</i></span> più "efficiente" è controllare se, dato $n$, c'è un divisore compreso tra $0$ e $\sqrt{n}$. Questo è però polinomiale nel valore di $n$, ma esponenziale nelle sue cifre. È stato poi scoperto un algoritmo polinomiale nelle cifre che però è di grado $\approx n^5\to$ <span style="color:#ff82b2"><b>molto lento</b></span>.
Si usa qundi un algoritmo <span style="color:#ff82b2"><b>randomizzato</b></span>, che però non da sempre un risultato corretto.
Esistono due tipi di algoritmi randomizzati:
- <span style="color:#ff82b2"><b>Las Vegas:</b></span> (e.g. QuickSort) forniscono risultati <span style="color:#ff82b2"><i>sicuramente certi</i></span> in tempo <span style="color:#ff82b2"><i>probabilmente breve</i></span>
- <span style="color:#ff82b2"><b>Monte Carlo:</b></span> generano risultati <span style="color:#ff82b2"><i>probabilmente corretti</i></span>, in tempo <span style="color:#ff82b2"><i>sicuramente breve</i></span>. Perché sia affidabile, si richiede che la probabilità di errore sia matematicamente misurabile e si deve poter rendere arbitrariamente piccola.
## Test di Primalità di Miller e Robin
Dato $\textcolor{#f77ead}{N}$ un numero <span style="color:#ff82b2"><i>intero</i></span> di $\textcolor{#f77ead}{n}$ <span style="color:#ff82b2"><i>bit</i></span>, dispari, abbiamo $N-1= \textcolor{#f77ead}{2^w\cdot z}$ con $z$ <span style="color:#ff82b2"><i>dispari</i></span>. Questo test ha due predicati necessri ma non sufficienti:
- <span style="color:#ff82b2"><b>P1 $\rightarrow$</b></span> Si scelga una $y$ tale che $2\leq y \leq N-1$ in modo arbitrario (<span style="color:#ff82b2"><i>testimone</i></span>). È necessario che $MCD(N,y)=1$
- <span style="color:#ff82b2"><b>P2 $\rightarrow$</b></span> $2\leq y \leq N-1\implies y^z \:\:mod \:\:N=1\implies\exists\:i,\,0\leq i \leq w$ tale che $(y^z)^{2^i} mod N = 1$
Miller e Robin hanno dimostrato che i numeri composti che le soddisfano entrambi sono pochi.
> [!quote] Lemma di Miller e Robin
> 
> Se $N$ è composto, il numero di interi composti tra $1$ e $N-1$ che soddisfano i predicati <span style="color:#ff82b2"><b>P1</b></span> e <span style="color:#ff82b2"><b>P2</b></span> è minore di $\frac{N}{4}$.
> 
> Se $N$ è composto, la probabilità di scegliere a caso un <span style="color:#ff82b2"><i>testimone</i></span> $y$ che rende veri <span style="color:#ff82b2"><b>P1</b></span> e <span style="color:#ff82b2"><b>P2</b></span> è $<\frac{1}{4}$

L'idea del test di primalità è ripetere più volte il test con testimoni diversi per ridurre la probabilità di errare.
Dato un $y$ scelto a caso
- Se uno dei due predicati è falso $\rightarrow$ $\textcolor{#f77ead}{N}$ <span style="color:#ff82b2"><i>è sicuramente composto</i></span>
- Se <span style="color:#ff82b2"><b>P1</b></span> e <span style="color:#ff82b2"><b>P2</b></span> sono enstrambi veri $\rightarrow$ $\textcolor{#f77ead}{N}$ <span style="color:#ff82b2"><i>è primo</i></span> con probabilità $<\frac{3}{4}$ $\rightarrow$ $N$ è composto con probabilità $\geq\frac{1}{4}$
Iterando per $k$ volte (scegliendo casualmente il termine $k$), se per $k$ volte i due predicati sono veri, $\textcolor{#f77ead}{N}$ <span style="color:#ff82b2"><i>è primo con probabilità di errore</i></span> $\textcolor{#f77ead}{<(\frac{1}{4})^k}$.
```
VERIFICA(N,y) //verifica se N è composto
	if ((P1 = = false) && (P2 = = false)) RETURN 1; //N sicuramente composto
	else return 0; //N è probabilmente primo (prob >= 3/4)

TEST_MR(N,k) //k = numero di volte che voglio iterare
	for(i=1;i<=k;++i) {
		scegli a caso y ∈ [1, N-1];
		if (VERIFICA(N,Y)==1) return 0; //N è composto
	}
	RETURN 1; //N è probabilmente primo
```
### Analisi della complessità
$$
N-1=2^w\cdot z
$$
- $w$ e $z$ si ricavano in un tempo polinomiale (al più $log_2N$ divisori per $2$)
- #### Calcolo di p1
	$MCD(N,y)$ si calcola in tempo polinomiale con l'algoritmo di Euclide ($O(log^3N)$)
	##### 2022/10/10 #date 
- #### Calcolo di P2$\rightarrow$
	$x=y^2 \text{ mod } N$
	1. Si scompone l'esponente $z$ in una somma di potenza di $2\implies z=\sum_{i=0}^tk_i2^i$ con $k_i \in \{0,1\}$ e $t=\lfloor \log_2{z}\rfloor=\Theta(\log{z})$
	2. Si calcolano tutte le potenze 
	$$y^{2^i} \text{ mod } N=(y^{2^{i-1}}) \text{ mod } N$$
		come quadrato della potenza precedente. $t$ "<span style="color:#ff82b2"><i>quadratura</i></span>", $1\leq i\leq t$, $\Theta (\log{z})$
	3.Si calcola $x=y^z \text{ mod } N$ come $$x=(\prod_{i:k_i=1} y^{2^i} \text{ mod } N)\text{ mod } N$$ al più $t$ moltiplicazioni$\implies O(\log{z})$. Polinomiale nello spazio dei dati, la complessità è al più cubica
Nel predicato <span style="color:#ff82b2"><b>P2</b></span> devo fare due operazioni
$$
\begin{array}{lr}
	t^z \text{ mod } N=1 &
	N-1=2^w
\end{array}
$$
e valutare 
$$
\begin{array}{c}
	y^z \text{ mod } N=-1 \text{ OR } y^{2\cdot z} \text{ mod }N=-1 \text{ OR }\cdots y^{2^i} \text{ mod }N=-1 \cdots \text{ OR }\cdots\\
	 \cdots y^{2^{2-1}\cdot z} \text{ mod }N=y^{\frac{N-1}{2} \text{ mod }N=-1}
\end{array}
$$
Ogni termine si può calcolare come quadrato del precedente ($O(w)\text{ elementi al quadrato} \implies w=O(\log{N})$). La valutazione richiede $\textcolor{#f77ead}{O(\log{N)}}$ moltiplicazioni, l'algoritmo è <span style="color:#ff82b2"><b>polinomiale</b></span>.
## Generazione di Numeri Primi (grandi)
<span style="color:#ff82b2"><b>Idea:</b></span> Per generare un numero <span style="color:#ff82b2"><i>primo</i></span>, genero un numero casuale e testo la primalità, se risulta composto, eseguo il test sul dispari successivo ($n+2$). Il seguente algoritmo genera un numero primo di almeno $n$ bit con probabilià di errore $<(\frac{1}{4})^4$
```
PRIMO(n,k) //n è il numero di bit del numero primo da generare
	S = sequenza di n-2 bit prodotti da un 
		generatore binario pseudocasuale
	N = numero intero che corrisponde alla 
		sequenza binaria 1S1 //intero dispari a N cifre binarie
	while (TEST_MR(N,k) == 0) {
		//se TEST_MR ritorna 0 si riesegue sul dispari successivo
		N=N+2;
	}
	RETURN N;
```
### Analisi della complessità
La complessità dipende dal numero di iterazioni del `while` $\rightarrow$ il numero di iterazioni del while è <span style="color:#ff82b2"><b>polinomiale in</b></span> $\textcolor{#f77ead}{\boldsymbol{log\:\:N}}$
>[!abstract] Teorema della densità dei numeri primi sull'asse $\mathbf{N}$
>
>Il numero di interi primi $e<N$ tende a $\frac{N}{log_e\:N}$ per $n\to\infty$.
>
>Per $N$ abbastanza grande, in un suo intorno di ampiezza $\log_e{N}$, cade mediamente un numero primo