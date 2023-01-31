---
title: Teoria della Complessità
tags: #critto
---
##### 2022/09/22 #date 
# Decidibilità e Trattabilità
- [[Glossary#Problemi indecidibili|Problemi indecidibili]]
- [[Glossary#Problemi intrattabili|Problemi intrattabili]]
- [[Glossary#Problemi Trattabili|Problemi trattabili ("facili")]]
- [[Glossary#Problemi presumibilmente intrattabili|Problemi presumibilmente intrattabili]]
# Algoritmi polinomiali e esponenziali
Studiamo la dimensione dei dati trattabili in funzione dell'incremento della velocità dei calcolatori:
Consideriamo i calcolatori $C_1, \:C_2$ (dove $C_2$ è $k$ volte più veloce di $C_1$) e di avere a disposizione un tempo $t$
$n_1$: dati trattabili nel tempo $t$ su $C_1$
$n_2$: dati trattabili nel tempo $t$ su $C_2$
> [!tip] Observation 
> 
> Usare $C_2$ per un tempo $t$, equivale a usare $C_1$ per un tempo $k\times t$

Consiederiamo poi un algoritmo <span style="color:#ff82b2"><i>polinomiale</i></span> che risolve il problema in $c\:\:n^s$ secondi ($c,\:s$ costanti)
$$
\begin{array}{c}
	\begin{array}{llcl}
		C_1:&c\cdot n_1^s=t&\rightarrow&n_1=(\frac{t}{c})^{\Large\frac{1}{s}} \\
		C_2:&c\cdot n_2^s=k\cdot t&\rightarrow&n_2=(k{\large\frac{t}{c}})^{\Large\frac{1}{s}}=k^{\Large\frac{1}{s}}\cdot({\large\frac{t}{c}})^{\Large\frac{1}{s}}
	\end{array} \\
	n_2=n_1+log_2\cdot k&&&&&&&&&&
\end{array}
$$
C'è quindi un <span style="color:#ff82b2"><b>miglioramento</b></span> di un fattore additivo $log_2\,k$
> [!example] Example
> 
>per $k=10^9$ possiamo solo <span style="color:#ff82b2"><i>sommare</i></span> $log_2{10^9}\sim 30$ al numero di dati trattabili a pari tempo di calcolo

# Problemi
Un <span style="color:#ff82b2"><b>problema</b></span> $\Pi$ è formato da:
- $\textcolor{#ff82b2}{I}$: insieme delle <span style="color:#ff82b2"><i>istanze</i></span> in ingresso
- $\textcolor{#ff82b2}{S}$: insieme delle <span style="color:#ff82b2"><i>soluzioni</i></span>
## Tipologie di problemi
- <span style="color:#ff82b2"><b>Problemi decisionali:</b></span> Richiedono una risposta binaria ($\textcolor{#ff82b2}{S=\text{\{}0,1\text{\}}}$) e le istanze possono essere <span style="color:#ff82b2"><i>positive</i></span> (o accettabili)($x\in I, t.c. \Pi (x)=1$) o <span style="color:#ff82b2"><i>negative</i></span> ($x\in I, t.c. \Pi (x)=0$)
- <span style="color:#ff82b2"><b>Problemi di ricerca:</b></span> Data un'istanza $\textcolor{#ff82b2}{x}$, richiedono di restituire una soluzione $\textcolor{#ff82b2}{s}$ (e.g. trovare un cammino tra due vertici, trovare il mediano tra un insieme di elementi)
- <span style="color:#ff82b2"><b>Problemi di ottimizzazione:</b></span> Data un'istanza $\textcolor{#ff82b2}{x}$, si vuole trovare la migliore soluzione $\textcolor{#ff82b2}{s}$ tra tutte le possibili soluzioni (e.g. ricerca della clique di dimensione massima, ricerca del cammino minimo fra due nodi di un grafo)
### Problemi Decisionali
La teoria della complessità computazionale è definitia principalmente in termini di <span style="color:#ff82b2"><i>problemi di decisione</i></span>. Essendo la risposta binaria, non ci si deve preoccupare del tempo richiesto per restituire la soluzione e tutti il tempo è speso esclusivamente per il calcolo. La dificcoltà di un problema è già presente nella sua versione decisionale.
Molti problemi di interesse pratico sono però <span style="color:#ff82b2"><i>problemi di ottimizzazione</i></span>, è possibile esprimere un problema di ottimizzazione in forma decisionale, chiedendo l'esistenza di una soluzione che soddisfi una certa proprietà.
Il problema di ottimizzazione è quindi <span style="color:#ff82b2"><i>almeno tanto difficile quanto</i></span> il corrispondente problema decisionale. Caratterizzare la complessità del problema decisionale permette quindi di dare almeno una limitazione inferiore alla complessità del problema di ottimizzazione.
# Classi di Complessità
Dato un problema decisionale $\textcolor{#f77ead}{\Pi}$ ed un algoritmo $\textcolor{#ff82b2}{A}$, diciamo che $\textcolor{#ff82b2}{A}$ risolve $\textcolor{#f77ead}{\Pi}$ se, data un'istanza di input $\textcolor{#ff82b2}{x}$ 
$$
\textcolor{#ff82b2}{A(x)=1\iff\Pi(x)=1}
$$
$\textcolor{#ff82b2}{A}$ risolve $\textcolor{#f77ead}{\Pi}$ in <span style="color:#ff82b2"><i>tempo</i></span> $\textcolor{#ff82b2}{t(n)}$ e <span style="color:#ff82b2"><i>spazio</i></span> $\textcolor{#ff82b2}{s(n)}$ se il tempo di esecuzione e l'occupazione di memoria di $\textcolor{#ff82b2}{A}$ sono rispettivamente $t(n)$ e $s(n)$.
## Classi Time e Space
Data una qualunque funzione $f(n)$
> [!quote] Time($\textbf{f(n)}$)
> 
> è l'insieme dei <span style="color:#ff82b2"><b>problemi decisionali</b></span> che possono essere risolti in <span style="color:#ff82b2"><b>tempo</b></span> $\textcolor{#f77ead}{\textbf{O(f(n))}}$

> [!quote] Space($\textbf{f(n)}$)
> 
> è l'insieme dei <span style="color:#ff82b2"><b>problemi decisionali</b></span> che possono essere risolti in <span style="color:#ff82b2"><b>spazio</b></span> $\textcolor{#f77ead}{\textbf{O(f(n))}}$

### Classe P
<span style="color:#ff82b2"><i>Algoritmo polinomiale (tempo)</i></span>
Esistono due costanti $\textcolor{#f77ead}{c}$, $\textcolor{#f77ead}{n_0}>0$ t.c. il numero di passi elementari è al più $\textcolor{#f77ead}{n^c}$ per ogni input di dimensione $n$ e per ogni $n>n_0$
> [!quote] Classe P
> 
> è la classe dei problemi <span style="color:#ff82b2"><b>risolvibili in tempo polinomiale</b></span> nella dimensione *n* dell'istanza di ingresso

### Classe PSpace
<span style="color:#ff82b2"><i>Algoritmo polinomiale (spazio)</i></span>
Esistono due costanti $\textcolor{#f77ead}{c}$, $\textcolor{#f77ead}{n_0}>0$ t.c. il numero di celle di memoria utilizzate è al più $\textcolor{#f77ead}{n^c}$ per ogni input di dimensione $n$ e per ogni $n>n_0$
> [!quote] Classe PSpace
> 
> è la classe dei <span style="color:#ff82b2"><b>problemi risolivibili in spazio polinomiale</b></span> nella dimensione *n* dell'istanza di ingresso

### Classe Exp-Time
> [!quote] Classe Exp-time
> 
> è la classe dei <span style="color:#ff82b2"><b>problemi risolvibili in tempo esponenziale</b></span> nella dimensione *n* dell'istanza di ingresso

### Relazioni tra classi
$$
P\subseteq PSpace
$$
Infatti un <span style="color:#ff82b2"><i>algoritmo polinomiale</i></span> può avere accesso al più ad un numero polinomiale di locazioni di memoria (in ordine di grandezza)
$$
PSpace \subseteq Exp-Time
$$
Non è noto (ad oggi) se le inclusioni siano proprie, l'unico risultato di separazione dimostrato fin'ora riguarda solo P e Exp-Time, esiste quindi un problema che può essere risolto in tempo esponenziale, ma per cui il tempo non è sufficiente.
## Algoritmo per CLIQUE
Si considerano tutti i sottoisniemi di vertici in ordine di cardinalità decrescente, e si verifica se formano una clique di dimensione almeno $\textcolor{#ff82b2}{l}$. Se $\textcolor{#ff82b2}{n}$ è il numeri di vertici, al caso peggiore l'algoritmo esamina $\textcolor{#f77ead}{2^n}$ <span style="color:#ff82b2"><i>sottoinsiemi,</i></span> quindi $\textcolor{#ff82b2}{CLIQUE\in ExpTime}$, non è noto un algoritmo polinomiale.
## Algoritmo per Cammino Hamiltoniano
Si considerano tutte le permutazioni di vertici, e si verifica se i vertici in quell'ordine sono a due a due adiacenti. Se $\textcolor{#ff82b2}{n}$ è il numero di vertici, al caso peggiore l'algoritmo esamina $\textcolor{#f77ead}{n!}$ <span style="color:#ff82b2"><i>permutazioni</i></span>, Quindi $\textcolor{#ff82b2}{\text{Cammino Hamiltoniano}\in ExpTime}$. Non è noto un algoritmo polinomiale.
## SAT
Dato un insieme $\textcolor{#ff82b2}{V}$ di variabili Booleane abbiamo:
- <span style="color:#ff82b2"><b>Letterale:</b></span> variabile o sua negazione
- <span style="color:#ff82b2"><b>Clausola:</b></span> disgiunzione (<span style="color:#ff82b2"><i>OR</i></span>) di letterali
Un espressione Booleana su $\textcolor{#ff82b2}{V}$ si dice <span style="color:#ff82b2"><b>forma normale congiuntiva (FNC)</b></span> se è espressa come <span style="color:#ff82b2"><b>congiunzione di clausole</b></span> (<span style="color:#ff82b2"><i>AND</i></span> di <span style="color:#ff82b2"><i>OR</i></span> di letterali)
> [!example] Example
> 
> $$
> \begin{array}{l}
> 	\textcolor{#f77ead}{V=\{x,y,z,w\}}\\
> 	FNC:(x\lor\overline{y}\lor z)\land (\overline{x}\lor w)\land y &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
> \end{array}
> $$

Data una espressione in forma normale congiuntiva, verificare se esiste una assegnazione di valori di verità alle variabili che rende l'espressione vera.
> [!example] Example
> 
>La formula
>$$
>(x\lor\overline{y}\lor z)\land(\overline{x}\lor w)\land y
>$$
>è soddisfatta dall'assegnazione
>$$
>\begin{array}{cccc}
>	x = 1 &
>	y = 1 &
>	z = 0 &
>	w = 1
>\end{array}
>$$
### Algoritmo per SAT
Si considerano tutti i $\textcolor{#f77ead}{2^n}$ assegnamenti di valore alle *n* variabili e per ciascuno si verifica se la formula è vera, quindi $\textcolor{#ff82b2}{SAT\in ExpTime}$, non è noto un algoritmo polinomiale.

##### 2022/09/27 #date
# Clique, cammino Hamiltoniano, SAT
Non sappiamo se la ricerca esaustiva è necessaria
## Problemi decisionali e certificati
In un <span style="color:#ff82b2"><b>problema decisionale</b></span> siamo interessati a verificare se un'istanza del problema soddisfa una certa proprietà.
Per alcuni problemi, per le <span style="color:#ff82b2"><b>istanze accettabili (positive)</b></span> $\textcolor{#f77ead}{x** è possibile fornire un <span style="color:#ff82b2"><b>certificato</b></span> $\textcolor{#f77ead}{y}$ che possa convincerci del fatto che l'istanza soddisfa la proprietà ed è dunque un'istanza accettabile.
### Certificato
Un certificato è un <span style="color:#ff82b2"><b>attestato breve di esistenza</b></span> di una soluzione con determinate proprietà. Si definisce solo per le istanze accettabili. Infatti, in generale, non è facile costruire <span style="color:#ff82b2"><b>attestati di non esistenza</b></span>
> [!example] Example
> 
> * Certificato per CLIQUE $\rightarrow$ sottoinsieme di $\textcolor{#ff82b2}{k}$ <span style="color:#ff82b2"><i>vertici</i></span>, che forma la clique
> * Certificato per cammino Hamiltoniano $\rightarrow$ <span style="color:#ff82b2"><i>permutazione degli</i></span> $\textcolor{#ff82b2}{n}$ <span style="color:#ff82b2"><i>vertici</i></span> che definisce un cammino semplice
> * Certificato per SAT $\rightarrow$ Un'assegnazione di verità alle variabili che renda vera l'espressione
> * Certificato per UNSAR $\rightarrow$ è vero che nessun assegnamento di valore rende vera l'espressione? Non è sufficiente esibire un'assegnazione di valori di verità alle variabili. In questo caso è difficile esprimere anche un certificato che sia 'breve'

### Verifica
<span style="color:#ff82b2"><b>Idea:</b></span> utilizzare il costo della verifica di un certificato (una soluzione) per un'istanza accettabile (positiva) per caratterizzare la complessità del problema stesso
> [!quote] Verifica
> 
> Un problema $\Pi$ <span style="color:#ff82b2"><b>è verificabile in tempo polinomiale</b></span> se
> 1. Ogni istanza accettabile $x$ di $\Pi$ di lunghezza $n$ ammette un certificato $y$ di lunghezza polinomiale in $n$
> 2. Esiste un algoritmo di verifica polinomiale in $n$ e applicabile ad ogni coppia $<x, y>$, che permette di attestare che $x$ è accettabile

## Classe P e NP
> [!quote]  Classe Polinomiale Non-deterministico
> 
> NP è la classe dei problemi decisionali verificabili in tempo polinomiale

Un certificato contiene un'informazione molto prossimo alla soluzione, quindi qual'è l'interesse di questa definizione?
* La teoria della verifica è utile per far luce sulle gerarchie di complessità dei problemi, non aggiunge nulla alla possibilità di risolverli efficientemente
* Chi ha una soluzione può verificare in tempo polinomiale che l'istanza è accettabile
* Chi non ha una soluzione (certificato), può individuarla in tempo esponenziale considerando tutti i casi possibili con una ricerca esaustiva
P è incluso in NP, ogni problema in P ammette un certificato verificabile in tempo polinomiale.
![P&PNP.Image](Crittografia/assets/P&NP.jpg)

Non sappiamo se dobbiamo per forza fare la ricerca esaustiva quando abbiamo un problema come i precedenti. Si congettura $P \neq NP$, è possibile individuare i problemi più difficili all'interno della classe NP, ovvero quelli candidati ad appartenere a NP se $P\neq NP$
> [!quote] Problemi NP-Completi
> 
> Sono i problemi <span style="color:#ff82b2"><b>Più difficili</b></span> all'interno della classe NP.
> * Se esiste un algoritmo polinomiale per risolvere solo uno di questi problemi, allora tutti i problemi in NP potrebbero essere risolti in tempo polinomiale, e dunque $P=NP$
> * Quindi tutti i problemi NP-Completi sono risolvibili in tempo polinomiale oppure nessuno lo è

## Riduzioni polinomiali
$\Pi _1 e \Pi _2 =$ problemi decisionali
$I_1 e I_2 =$ insiemi delle istanze di input di $\Pi _1$ e  $\Pi _2$ 
$\Pi _1$ si riduce in tempo polinomaile a $\Pi _2$
$$\Pi _1 \le _ p\Pi _2$$
se esiste una funzione $f:I_1 \le _p I_2$ calcolabile in tempo polinomiale tale che, per ogni istanza di $\Pi _1$, $\textcolor{#ff82b2}{x}$ <span style="color:#ff82b2"><b>è un'istanza accettabile di</b></span> $\textcolor{#ff82b2}{\Pi _1}$ <span style="color:#ff82b2"><b>IFF</b></span> $\textcolor{#ff82b2}{f(x)}$ <span style="color:#ff82b2"><b>è un'istanza accettabile di</b></span> $\textcolor{#ff82b2}{\Pi _2}$.
Se esiste un algoritmo per risolvere $\Pi _2$ potremmo utlizzarlo per risolvere $\Pi _1$
$$
\begin{array}{llll}
	\Pi_1\leq_p \Pi_2 & \text{AND} & \Pi_2 \in P \implies \Pi_1 \in P
\end{array}
$$
![RiduzioniPolinomiali](Crittografia/assets/RiduzioniPolinomiali.jpg)
> [!quote] Problemi NP ardui
> 
> Un problema $\Pi$ si dice <span style="color:#ff82b2"><b>NP-ardue</b></span> se
> $$\forall \Pi ' \in NP, \Pi ' \le_p\Pi'$$

> [!quote] Problemi NP completi
> 
> Un problema decisionale $\Pi$ si dice <span style="color:#ff82b2"><b>NP-Completo</b></span> se 
> $$\Pi \in NP \forall \Pi ' \in NP, \Pi ' \le_p \Pi$$

Dimostrare che un problema è in NP può essere facile, <span style="color:#ff82b2"><b>basta esibire un certificato polinomiale</b></span>, non è altrettanto facil edimostrare che un problema $\Pi$ è NP-arduo o NP-completo
* Bisogna dimostrare che tutti i problemi in NP si riducono polinomialmente a $\Pi$
* In realtà la prima dimostrazione di NP-completezza aggira il problema

## Teorema di Cook (1971)
<span style="color:#ff82b2"><b>SAT è NP completo</b></span>
Cook ha mostrato che, dati un qualunque problema $\Pi$ in NP ed una qualunque istanza $x$ per $\Pi$, si può costruire un'espressione Booleana in forma normale congiuntiva che descrive il calcolo di un algoritmo per risolvere $\Pi$ su $x$. L'espressione è vera **iff** l'algoritmo restituisce 1.

Un problema decisionale $\Pi$ è <span style="color:#ff82b2"><b>NP-Completo</b></span> se $\Pi\in NP$ e $SAT \le_p \Pi$ (o qualsiasi altro problema NP-Completo). Infatti 
$$
\begin{array}{llll}
	\forall & \Pi' \in NP, & \Pi'\leq_p SAY & e & SAT\leq_p\Pi
\end{array}
$$Quindi $\textcolor{#ff82b2}{\Pi'\le_p\Pi}$
### Clique è NP-completo
$$
\begin{array}{lll}
	SAT & \leq_p & CLIQUE
\end{array}
$$
Data un'espressione booleana F in forma normale congiuntiva con $k$ clausole, <span style="color:#ff82b2"><b>è possibile costruire in tempo polinomiale</b></span> un grafo $G$ che contiene una clique di $k$ vertiici iff $F$ è soddisfacibile.
### Problemi NP equivalenti
* $SAT\le_pCLIQUE\Rightarrow \text{CLIQUE è NP-Completo}$
* $\text{SAT è NP-Completo}\Rightarrow CLIQUE\le_pSAT$
* Tutti i problemi $NP$ completi sono tra loro $NP\:\:equivalenti$
* Sono tutti facili, o tutti difficili
#### Gerarchia delle classi
![GerarchiaClassi](Crittografia/assets/GerarchiaClassi.jpg)
### Famosi problemi NP-Completi
#### Copertura di vertici
Una copertura di vertici (<span style="color:#ff82b2"><i>vertex cover</i></span>) di un grafo $G=(V,E)$ è un insieme di vertici $C\subseteq V$ tale che $\forall\:\:(u, v)\in E$, almeno uno tra $u$ e $v$ appartiene a $C$. 
Dati $G$ e un intero $k$, verificare se esiste una copertura di vertici di $G$ di dimensioni al più $k$.
#### Commesso viaggiatore
Dati un grafo completo $G$ con pesi $w$ sugli archi ed un intero $k$, verificare se esiste un ciclo di peso al più $k$ che attraversa ogni vertice una ed una sola volta.
#### Colorazione
Dati un grafo $G$ ed un intero $k$, verificare se è possibile colorare i vertici di $G$ con al più $k$ colori tali che due vertici adiacenti non siano dello stesso colore.
#### Somme di sottoinsiemi
Dati un insieme $S$ di numeri naturali ed un intero $t$ verificare se esiste un sottoinsieme di $S$ i cui elementi sommano esattamente a $t$.
#### Zaino
Dati un intero $k$, uno zaino di capacità $c$, e $n$ oggetti di dimensioni $s_1, ...,s_n$ cui sono associati profitti $p_1, ..., p_n$, verificare se esiste un sottoinsieme degli oggetti di dimensione al più $c$ che garantisca profitto almeno $k$.
## Problemi di ottimizzazione NP-Hard
Se la soluzione ottima è troppo difficile da ottenere, una soluzione quasi ottima ottenibbile facilmente è buona abbastanza. A volte, avere una soluzione esatta non è strattamente necessario, ci si accontenta di una soluzione che non si discosti troppo da quella ottima, si possa calcolare in tempo polinomiale.
## Le classi co-P e co-NP
Profonda differenza tra certificare l'esistenza o la non-esistenza di una soluzione
> [!example] Example: problema del ciclo Hamiltoniano
> 
> * una permutazione di vertici (per certificare l'esistenza)
> * per la non-esistenza è difficile dare un certificato polinomiale che indichi direttamente questa proprietà
> $\Pi$: problema decisionale
> $co\Pi$: problema <span style="color:#ff82b2"><b>complementare</b></span> (accetta tutte e sole istanze rifiutate da $\Pi$)

> [!quote] La classe co-P
> 
> è la classe dei **problemi decisionali $\Pi$** per cui
> $$co\Pi\:\in\:P$$
> $P=co\!-\!P$(risolvo il problema, e complemento il risultato)

> [!quote] La classe co-NP
> 
> è la classe dei <span style="color:#ff82b2"><b>problemi decisionali</b></span> $\Pi$ per cui
> $$co\text{-}\Pi\in NP$$
> $$\begin{array}{ccc}NP&?&co-NP\end{array}$$
> Si congettura che le due classi siano diverse, se questa congettura è vera allora $P\ne NP$
> ![classeCoNP](Crittografia/assets/classeCoNP.jpg)	

##### 2022/10/10 #date 
## Classe di complessità RP (Random Polinomial)
Rappresenta la classe dei problemi decisionali verificabili in tempo polinomiale randomizzato (<span style="color:#ff82b2"><i>Test di Primalità</i></span> $\textcolor{#f77ead}{\in}$ <span style="color:#ff82b2"><i>Test di Primalità</i></span>).
Verificabile in tempo polinomiale vuol dire che
	$\Pi$: problema decisionale
	$z$: istanza di input
	$y$: certificato probabilistico di $x$
		-> $y$ è di lunghezza al più polinomiale in ($x$)
		-> $y$ è esatratto perfettamente a caso da un insieme associato a $x$
### Algoritmo di verifica
$A(x,y)$ -> in tempo polinomiale attesta con certezza se l'istanza $x$ <span style="color:#ff82b2"><i>non</i></span> possiede la proprietà testata ($\pi(x)=0$, $x$ è un istanza non accettata)
$$
oppure
$$
attesta che $x$ è accettabile con probabilità $>\frac{1}{2}$ ($\frac{3}{4}$ per la primalità)

$$
\textcolor{#f77ead}{\boldsymbol{P\subseteq RP\subseteq NP}}
$$
Si congettura che valga $\boldsymbol{\subsetneq}$, ma non è stato dimostrato.