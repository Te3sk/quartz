---
title: Ricerca Euristica
tags: #infoai
---
La <span style="color:#ff82b2"><i>ricerca esaustiva non è praticabile</i></span> in problemi di complessità esponenziale. Usiamo conoscenza del problema ed esperienza ed esperienza per riconoscere i cammini più promettenti: usiamo una stima del costo futuro ed evitiamo di generare gli altri.
La conoscenza euristica aiuta a fare scelte oculate: non evita la ricerca ma la riduce, consente in genere di trovare una buona soluzione in tempi accettabili e sotto certe condizioni garantisce completezza e ottimalità.
# Funzione di valutazione euristica
La conoscenza del problema è data da una <span style="color:#ff82b2"><b>funzione di valutazione</b></span> $\textcolor{#ff82b2}{f}$, che include una <span style="color:#ff82b2"><b>funzione</b></span> $\textcolor{#ff82b2}{h}$ detta <span style="color:#ff82b2"><b>funzione di valutazione euristica:</b></span>
$$
\textcolor{#ff82b2}{h}:n\to\mathbb{R}
$$
La funzione si applica al nodo ma dipende solo dallo stato (`n.Stato`)
$$
\textcolor{#ff82b2}{f(n)}=g(n)+h(n)
$$
dove $g(n)$ è il costo del cammino visto con [[IntroAI/Problem Solving#Ricerca di costo uniforme (UC)|UC]]. Manteniamo la notazione in `n` per uniformità con $g$.
## Esempi di h
- Nell'[[IntroAI/Romania_example|Esempio del viaggio in romania]], la città più vicina, o la città più vicina alla meta in linea d'area (tabella esterna)
- Il numero di caselle fuori posto nel gioco dell'otto
- Il vantaggio in pezzi nella dama o negli scacchi
# Algoritmi Best-First
Resta l'algoritmo [[IntroAI/Problem Solving#Ricerca di costo uniforme (UC)|UC]], da ora in poi cambierà solo la funzione che assegna le probabilità ai nodi.
In questo caso $\textcolor{#ff82b2}{f}$ (<span style="color:#ff82b2"><i>stima di costo</i></span>) viene usata per la coda di priorità, questa determina la strategia di ricerca e ad ogni passo fa sì che si scelga il nodo sulla frontiera per cui il valore della $f$ è migliore.
> [!Warning] Nota
> 
> - In caso di un'euristica che stima la distanza della soluzione, "*migliore*" significa <span style="color:#ff82b2"><i>minore</i></span>
> - <span style="color:#ff82b2"><b>Greedy best-first</b></span> è un caso speciale: $f=h$, quindi si usa solo $h$

[[IntroAI/Romania_example#Algoritmo Best-First|Esempio del viaggio in romania]]
# Algoritmo A
> [!quote] Algoritmo A
> 
> É un algoritmo <span style="color:#ff82b2"><b>Best First</b></span> con una funzione di valutazione dello stato del tipo
> $$
> \begin{array}{c}
> 	\textcolor{#ff82b2}{f(n)=g(n) + h(n)} &
> 	\text{ , con } \textcolor{#ff82b2}{h(n)\geq0} \text{ e } \textcolor{#ff82b2}{h(goal)=0}
> \end{array}
> $$
> Dove:
>- $\textcolor{#ff82b2}{g(n)}$ è il costo del cammino percorso per raggiungere $n$
>- $\textcolor{#ff82b2}{h(n)}$ è una stima del costo per raggiungere da $n$ un nodo $goal$

Vedremo casi particolari dell'algoritmo A:
- Se $h(n)=0$ ($f(n)=g(n)$) si ha la <span style="color:#ff82b2"><i>Ricerca Uniforme</i></span> (UC)
- Se $g(n)=0$ ($f(n)=h(n)$) si ha il <span style="color:#ff82b2"><i>Gready best-first</i></span>
> [!example] Esempio di A per f=g+h
> 
> Esempio nel gioco dell'otto
> ![Esempio nel gioco dell'otto](IntroAI/assets/pictures/A_f=g+h.png)
> $$
> \begin{array}{lrr}
> 	f(n)=\#\text{mosse fatte} + \#\text{caselle fuori posto} \\
> 	f(start = 0+7) & \text{Dopo }\leftarrow, \downarrow,\uparrow,\rightarrow &f=4+7 \\
> 	f(\text{goal state})=?+0 && \small{\text{stesso stato, } g \text{ è cambiata}}
> \end{array}
> $$

## Completezza di A
> [!summaries] Theorem
> L'algoritmo $A$ con la condizione
> $$
> g(n)\geq d(n)\cdot \varepsilon \text{ (}\varepsilon>0\text{ costo minimo arco)}
> $$
> <span style="color:#ff82b2"><b>è completo</b></span>

La condizione ci garantisce che non si verifichino situazioni del tipo
![Conseguenza della condizione](IntroAI/assets/pictures/A_condition.png)
e che il costo lungo un cammino non cresca "*abbastanza*", se cresce abbastanza possiamo fermare quel path per costo alto di $g$.
#### Dimostrazione
Sia $[n_0n_1n_2....n'...n_k=goal]$ un cammino soluzione, sia $n'$ un nodo della frontiera su un cammino soluzione: prima o poi $n'$ sarà espanso, infatti esistono solo un numero finito di nodi $x$ che possono essere aggiunti alla frontiera con $f(x)\leq f(n')$. Questo viene garantito dalla condizione sulla crescita di $g$ nel teorema, t.c. non esista una catena infinita di archi e nodi che possa aggiungere con costo sempre $\leq f(n')$.
Se non si trova una soluzione prima, $n'$ verrà espanso e i suoi successori verranno aggiunti alla frontiera. Tra questi anche il suo successore sul cammino soluzione. Il ragionamento si può ripetere fino a dimostrare che anche il nodo $goal$ sarà selezionato per l'espansione.
# Algoritmo A*
## La stima ideale
La funzione di valutazione ideale (<span style="color:#ff82b2"><i>oracolo</i></span>) è
$$
\textcolor{#ff82b2}{f^*(n)=g^*(n)+h^*(n)}
$$
con:
- $\textcolor{#ff82b2}{g^*(n)}$: costo del cammino minimo da $radice$ fino a $n$
- $\textcolor{#ff82b2}{h^*(n)}$: costo del cammino minimo da $n$ a $goal$
- $\textcolor{#ff82b2}{f^*(n)}$: costo del cammino minimo da $radice$ a $goal$, passando da $n$
Normalmente vale $g(n)\geq g^*(n)$ ($\small\text{costo cammino}\geq\text{cammino migliore}$) e $h(n)$ è una stima di $h^*(n)$, si può andarein sottostima (e.g. linea d'area) o in sovrastima della soluzione
## Definizione
> [!quote] Euristica Ammissibile 
> 
> $\forall n | h(n)\leq h^*(n)$,   $h$ è una <span style="color:#ff82b2"><b>sottostima</b></span>

Ad esempio l'euristica della distanza in linea d'area
> [!quote] Algoritmo A*
> 
> Un algoritmo A in cui $h$ è una funzione euristica ammissibile

> [!summaries] Theorem
> 
> Gli algoritmi $A^*$ sono <span style="color:#ff82b2"><b>ottimali</b></span>
> <span style="color:#ff82b2"><b>Corollario:</b></span> [[IntroAI/Problem Solving#Ricerca in ampiezza - BF|BF]]$^{(+)}$ e [[IntroAI/Problem Solving#Ricerca di costo uniforme (UC)|UC]] sono ottimali ($h(n)=0$)

[[IntroAI/Romania_example#Itinerario con A*|Esempio del viaggio in romania]]

## Osservazioni
- Rispeto a Greedy Best-First, la componente $g$ fa si che abbandonino cammini che vanno troppo in profondità
- <span style="color:#ff82b2"><i>Sottostime</i></span> e <span style="color:#ff82b2"><i>Sovrastime</i></span>
	- Una <span style="color:#ff82b2"><i>sottostima</i></span> ($h$) può farci compiere del lavoro inutile (tenendo di conto anche candidati non buoni), però non ci fa perdere il cammino migliore. Una funzione che
	- Una funzione che qualche volta <span style="color:#ff82b2"><i>sovrastima</i></span> può farci perdere la soluzione ottimale. Viene fatto un taglio di cammini per sovrastima perché non penso che quel cammino sia promettente, ma potrei tagliare una possibile soluzione di costo minimo
## Ottimalità di A*
Nel casi di ricerca a/su albero, <span style="color:#ff82b2"><i>l'uso di un'euristica ammissibile è sufficiente a garantire l'ottimalità</i></span> di $A^*$. 
Nel caso di ricerca su grafo (con [[IntroAI/Problem Solving#Ricerca di costo uniforme (UC)|UC]] come visto) serve una <span style="color:#ff82b2"><i>proprietà più forte</i></span>: la <span style="color:#ff82b2"><b>consistenza</b></span> (o <span style="color:#ff82b2"><b>monotonicità</b></span>):
	Serve per evitare il rischio di scartare candidati ottimi se sono già stati esplorati, si vuole evitare di non considerare/far sparire al momento dell'espansione alcuni candidati ottimali.
	Vorremmo assicurarci che il primo espanso sia sempre il migliore, così possiamo tenere la lista degli esplorati e non rischiamo di perdere l'ottimo.
	In [[IntroAI/Problem Solving#Ricerca di costo uniforme (UC)|UC]] c'è l'`if` finale che non ci fa rischiare di perdere l'ottimo.
## Euristica Consistente (o monotona)
> [!quote] Euristica consistente
> - $h(goal)=0$
> - $\forall n|h(n)\leq c(n,a,n')+h(n')$ dove $n'$ è un successore di $n$, ne segue che $f(n)\leq f(n')$

> [!Warning] Nota
> Se $h$ è consistente la $f$ non decresce mai lungo i cammini, da cui il termine <span style="color:#ff82b2"><b>monotona</b></span> 

É quindi una distanza che <span style="color:#ff82b2"><b>rispetta la disuguaglianza trianfolare</b></span>.
### Proprietà
> [!summaries] Theorem
> Un'euristica monotona è <span style="color:#ff82b2"><b>ammissibile</b></span>

Ne esistono anche di non ammissibili che sono monotone ma solo rare.
- Le euristiche monotone garantiscono che <span style="color:#ff82b2"><i>la soluzione meno costosa venga trovata per prima</i></span> e quindi sono ottimali anche nel caso di ricerca sul grafo
- Non si devono recuperare fra gli antenati nodi con costo minore
- C'è una <span style="color:#ff82b2"><i>lista degli esplorati</i></span>, se uno stato già esplorato è sul cammino ottimo, posso evitare di inserire il corrente ripetuto senza perdere l'ottimalità
	`if figlio.Stato` non è nella lista e non è in frontiera `then frontiera=Inserisci(figlio, frontiera)` 
- Per la <span style="color:#ff82b2"><i>frontiera</i></span>, volendo evitare stati ripetuti, resta l'`if` finale di [[IntroAI/Problem Solving#Ricerca di costo uniforme (UC)|UC]]
	`if figlio.Stato` è in frontiera con `Costo-cammino` più alto `then` sostituisci quel nodo frontiera col figlio
## Ottimalità di A* (con h consistente)
1. Se $h(n)$ è consistente, i valori di $f(n)$ lungo un cammino sono decrescenti:
$$
\begin{array}{l}
	\text{Se } h(n)\leq c(n,a,n')+h(n')
		&&\text{def. consistenza}\\
	g(n) + h(n)\leq \textcolor{#ff82b2}{g(n) + c(n,a,n')} + h(n)
		&& \text{sommando }g(n)\\
	\text{ma siccome } \textcolor{#ff82b2}{g(n)+c(n,a,n')}=g(n')\\
	g(n) + h(n)\leq g(n')+h(n')\\
	\textcolor{#ff82b2}{f(n)\leq f(n')}
		&&\rightarrow f \textcolor{#ff82b2}{\text{ monotona}} &&&&&&&&&&&
\end{array}
$$
2. Ogni volta che $A^*$ seleziona un nodo ($n$) per essere espanso, il cammino ottimo a tale nodo è stato trovato:
		se così non fosse, ci sarebbe un altro nodo $m$ della frontiera sul cammino ottimo (a $n$, ancora da trovare con un cammino ottimo), con $f(m)$ minore, ma ciò non è possibile perché tale nodo sarebbe già stato espanso
3. Quando si seleziona il nodo goal il cammino è ottimo $[h=0,f=C^*]$,
## Perché A* è vantaggioso
- $A^*$ espande tutti i nodi con $f(n)<C^*$  ($C^*$ = costo ottimo)
- $A^*$ espande alcuni nodi con $f(n)=C^*$
- $A^*$ <span style="color:#ff82b2"><i>non espande alcun nodo con</i></span> $\textcolor{#ff82b2}{f(n)>C^*}$
Quindi alcuni nodi (e loro sottoalberi) non verranno considerati per l'espansione, ma resteranno ottimali.
	<span style="color:#ff82b2"><b>Puring</b></span>: scegliamo un'$h$ opportuna, più alta possibile tra le ammissibili, che ci fa tagliare molto.
Più è alta $h$ più $f(n)$ deborda e più sono i nodi che non espandiamo perché superano $C^*$, però $h$ <span style="color:#ff82b2"><i>deve essere ammissibile</i></span>.
> [!Warning] Nota
> Più $f$ è aderente alla stima ottimale, più si taglia (gli ovali sono più stretti). Cercheremo quindi una $h$ il più alta possibile tra le ammissibili. Se la troviamo molto bassa molti nodi resteranno minori di $C^*$, quindi li espandiamo tutti.
> Il <span style="color:#ff82b2"><b>puring</b> dei sotto-alberi</span> è il punto focale: non li abbiamo già in memoria e evitiamo di generarli.

## Riassunto A*
- L'algoritmo è quello degli schemi [[IntroAI/Problem Solving#Ricerca di costo uniforme (UC)|UC]]
- Si usa $f=g+h$ per la <span style="color:#ff82b2"><i>coda con priorità</i></span>
	- $h$ e $g$ soddisfano il [[IntroAI/InfoSearch#Completezza di A|teorema di completezza]]
	- $h$ è una funzione euristica ammissibile 
## Bilancio su A*
- $A^*$ è <span style="color:#ff82b2"><b>completo</b></span>: discende dalla completezza di $A$ ($A^*$ è un algoritmo $A$ particolare)
- $A^*$ con euristica monotona è <span style="color:#ff82b2"><b>ottimale</b></span>
- $A^*$ è <span style="color:#ff82b2"><b>ottimamente efficiente</b></span>: a parità di euristica nessun altro algoritmo espande meno nodi (senza rinunciare a ottialità)
<span style="color:#ff82b2"><b>Problemi:</b></span> l'<span style="color:#ff82b2"><i>occupazione di memoria</i></span> nel caso pessimo resta $\textcolor{#ff82b2}{O(b^{d+1})}$, a causa della frontiera
## Sottocasi speciali di A
- Se $h(n)=0$ $[f(n)=g(n)]$ si ha [[IntroAI/Problem Solving#Ricerca di costo uniforme (UC)|UC]] - $g$ non basta (si può migliorare)
- Se $g(n)=0$ $[f(n)=h(n)]$ si ha [[IntroAI/InfoSearch#Algoritmi Best-First|Greedy best-first]] - $h$ non basta (già visto all'inizio)
### Dijkstra - UC vs. A^*
Entrambi gli algoritmi si scontrano con il muro, ma $\textcolor{#ff82b2}{A^*}$ <span style="color:#ff82b2"><i>con l'euristica</i></span> va direttamente verso il percorso ottimo. Dikstra ci mette più tempo perché prova più strade contemporaneamente.
![senza euristica](IntroAI/assets/pictures/dijkstraUC.png) ![con euristica](IntroAI/assets/pictures/dijkstraAstar.png)
## Costruire le euristiche di A*
### Valutazione di funzioni euristiche
A parità di ammissibilità, un'euristica può essere più efficiente di un'altra nel trovare il cammino soluzione migliore (quindi visita meno nodi). Questo dipende da <span style="color:#ff82b2"><i>quanto informata è l'euristica</i></span> (dal grado di informazione posseduto)
$$
\begin{array}{lcl}
	h(n)=0 && \text{minimo di informazione (BF o UC)}\\
	h^*(n) && \text{massimo di informazione (oracolo)} &&&&&&&&&&&&&&
\end{array}
$$
In generale, per le euristiche ammissibili
$$
0\leq h(n)\leq h^*(n)
$$
### Più inforamta, più efficiente
> [!summaries] Theorem
> 
> Se $h_1\leq h_2$, i nodi espansi da $A^*$ con $\textcolor{#ff82b2}{h_2}$ sono un sottoinsieme di quelli espansi da $A^*$ con $h_1$.
> $A^*$ espande tutti i nodi con $f(n)=g(n)+h(n)<C^*$, e sono meno per un'$h$ maggiore
> Se $h_1\leq h_2$, $A^*$ con $h_2$ è almeno efficiente quanto $A^*$ con $h_1$

Un'euristica più informata (accurata) riduce lo spazio di ricerca, ma è tipicamente più costosa da calcolare.
### Confronto di euristiche ammissibili
Due possibili euristiche ammissibili per il gioco dell'8:
- $h_1$: conta il numero delle caselle fuori posto
- $h_2$: somma delle [[Glossary#Distanze Manhattan|distanze Manhattan]] delle caselle fuori posto
$h_2$ è più informata di $h_1$, infatti $\textcolor{#ff82b2}{\forall n|h_1(n)\leq h_2(n)}$. Diciamo che $\textcolor{#ff82b2}{h_2}$ <span style="color:#ff82b2"><b>denomina</b></span> $\textcolor{#ff82b2}{h_1}$
![Euristiche ammissibili nel gioco dell'otto](IntroAI/assets/pictures/euristicheAmmissibiliGioco8.png)
$$
\begin{array}{l}
	h_1=7\\
	h_2=4+2+2+2+2+0+3+3=18&&&&&&&&&&&&&&&
\end{array}
$$
## Costo ricerca vs. Costo euristica
![Diagramma che confronta il costo della ricerca con il costo dell'euristica](IntroAI/assets/pictures/costoRicercaVsCostoEuristica.png)
## Misura del potere euristico
<span style="color:#ff82b2"><b>Fattore di diramazione effettivo:</b></span> $\textcolor{#ff82b2}{\mathbf{b^*}}$
	$N$: numero di nodi generati
	$d$: profondità della soluzione
$\textcolor{#ff82b2}{\mathbf{b^*}}$ è il fattore di diramazione di un albero uniforme con $N+1$ nodi, e risolve l'equazione
$$
N+1=b^*+(b^*)^2+\dots+(b^*)^d
$$
Sperimentalmente una buona euristica ha un $b^*$ abbastanza vicino a $1$ ($\leq 1.5$)
> [!example]
> 
> $d=5$    $N=52$
> $\textcolor{#ff82b2}{\mathbf{b^*}}= 1.92$

> [!example] Example: gioco dell'otto
> 
> | $d$ |                         ID                         |            $A^*(h_1)$            |            $A^*(h_2)$            |
> |:---:|:--------------------------------------------------:|:--------------------------------:|:--------------------------------:|
> | $2$ |          $10(\textcolor{#ff82b2}{2.43})$           |  $6(\textcolor{#ff82b2}{1.79})$  |  $6(\textcolor{#ff82b2}{1.79})$  |
> |  4  |          $112(\textcolor{#ff82b2}{2.87})$          | $13(\textcolor{#ff82b2}{1.48})$  | $12(\textcolor{#ff82b2}{1.45})$  |
> |  6  |          $680(\textcolor{#ff82b2}{2.73})$          | $29(\textcolor{#ff82b2}{1.34})$  | $18(\textcolor{#ff82b2}{1.30})$  |
> |  8  |         $6384(\textcolor{#ff82b2}{2.80})$          | $39(\textcolor{#ff82b2}{1.33})$  | $25(\textcolor{#ff82b2}{1.24})$  |
> | 10  |         $47127(\textcolor{#ff82b2}{2.79})$         | $93(\textcolor{#ff82b2}{1.38})$  | $39(\textcolor{#ff82b2}{1.22})$  |
> | 12  |        $3644035(\textcolor{#ff82b2}{2.78})$        | $227(\textcolor{#ff82b2}{1.42})$ | $73(\textcolor{#ff82b2}{1.24})$  |
> | 14  | Nodi generati: $\textcolor{#ff82b2}{\mathbf{b^*}}$ | $539(\textcolor{#ff82b2}{1.44})$ | $113(\textcolor{#ff82b2}{1.23})$ |
> | ... |                         -                          |               ...                | ...                                 |
> Sono riportati i nodi generati e il fattore di diramazione effettivo ($\textcolor{#ff82b2}{\mathbf{b^*}, \text{ pink}}$). I dati sono mediati, per $d$, su 100 istanze del problema.

### Capacità di esplorazione
Migliorando di poco l'euristica si riesce, a partità di nodi espansi, a raggiungere una <span style="color:#ff82b2"><i>profondità doppia</i></span> di esplorazione mosse, quindi:
- tutti i problemi di IA (o quasi) sono di complessità esponenziale, ma ce ne sono di vari ordini di grandezza
- L'euristica può migliorare di molto la capacità di esplorazione dello spazio degli stati rispetto alla ricerca cieca
- Migliorando anche di poco l'euristica si riesce ad esplorare uno spazio molto più grande (più in profondità)
# Inventare un'euristica
Alcune strategia per ottenere euristiche ammissibili sono:
- [[IntroAI/InfoSearch#Rilassamento del problema|Rilassamento del problema]]
- [[IntroAI/InfoSearch#Massimizzazione di euristiche|Massimizzazione di euristiche]]
- [[IntroAI/InfoSearch#Euristiche da sottoproblemi|Euristiche da sottoproblemi]]
- [[IntroAI/InfoSearch#Database di pattern disgiunti|Database di pattern disgiunti]]
- [[IntroAI/InfoSearch#Combinazione lineare|Combinazione lineare]]
- [[IntroAI/InfoSearch#Apprendere dall'esperienza|Apprendere dall'esperienza]]
## Rilassamento del problema
Nel gioco dell'otto, è possibile muovere una tessera da A a B se B è adiacente ad A e se B è libera
$h_1$ e $h_2$ sono <span style="color:#ff82b2"><i>calcoli della distanza esatta della soluzione</i></span> in versioni semplificate del puzzle:
- $h_1$: sono sempre ammessi scambia  piacimenti tra le caselle
	Si eliminano entrambe le restrizioni, il costo di soluzione (numero di mosse) è il numero delle caselle fuori posto
- $h_2$: sono ammessi spostamenti anche su caselle occupate, purché adiacenti
	Si elimina solo una delle due restrizioni, il costo di soluzione è la somma delle [[Glossary#Distanze Manhattan|distanze di Manhattan]]

## Massimizzazione di euristiche
Se si hanno una serie di euristiche ammissibili $h_1,h_2,\dots, h_k$ <span style="color:#ff82b2"><i>senza che nessuna "domini" un'altra</i></span>, allora conviene prendere il massimo dei loro valori $\textcolor{#ff82b2}{\Rightarrow h(n)=max\{h_1(n), h_2(n),\dots,h_k(n)\}}$.
Se le $h_i$ sono ammissibili, allora anche $h$, che domina tutte le altre, lo è.

## Euristiche da sottoproblemi
![Euristiche da sottoproblemi nel gioco dell'otto](IntroAI/assets/pictures/euristicheDaSottoproblemi.png)
Il costo della soluzione ottima al sottoproblema (ordinare 1,2,3,4) è una sottostima del costo per il problema nel suo complesso
<span style="color:#ff82b2"><b>Databese di pattern:</b></span> memorizzare ogni istanza del sottoproblema con il relativo costo della soluzione.
Si può questo database per calcolare $h_{DB}$, estraendo dal database la configurazione corrispondente allo stato completo corrente
#### Sottoproblemi multipli
Potremmo fare la cosa per gli altri sottoproblemi (5-6-7-8, 2-4-6-8, ...) ottenendo altre euristiche ammissibili, per poi prendere il valore massimo, anch'esso ammissibile. Si ouò sommarle ed ottenere un'euristica ancora più accurata?

## Database di pattern disgiunti
In generale non si possono sommare euristiche di sottoproblemi per ottenerne una più accurata, perché le soluzione ai sottoproblemi interferiscono e <span style="color:#ff82b2"><i>la somma delle euristiche in generale non è ammissibile</i></span> (potremmo sovrastimare avendo avuto aiuti mutui).
Si deve eliminare il costo delle mosse che contribuiscono all'altro sottoproblema. I <span style="color:#ff82b2"><b>database di pattern disgiunti</b></span> consentono di sommare costi (euristiche additive).

## Combinazione lineare
Quando diverse caratteristiche influenzano la bontà di uno stato, si può usare una combinazione lineare:
$$
h(n)=c_1x_1(n)+c_2x_2(n)+\dots+c_kx_k(n)
$$
> [!example]
> 
> gioco dell'8: $h(n)=c_1 \#fuori-posto + c_2 \#coppie-scambiate$
> scacchi: $h(n)=c_1\cdot vant-pezzi+c_2\cdot pezzi-attac.+c_3\cdot regina +...$

Il peso dei coefficienti può essere aggiustato con l'esperienza, anche qui apprendendo automaticamente da esempi di gioco.
$h(goal)=0$, ma questo non rende automatiche l'ammissibilità e la consistenza.

## Apprendere dall'esperienza
Si fa girare il programma raccogliendo come dati delle coppie $<stato,h^*>$, questi dati vengono usati per apprendere a predire la $h$ con <span style="color:#ff82b2"><i>algoritmi di apprendimento induttivo</i></span> (da istanze note stimiamo $h$ in generale).
Questi algoritmi di apprendimento si concentrano su caratteristiche salienti dello stato $(feature,x_i)$

# Algoritmi evoluti basati su A* - Migliorare l'occupazione di memoria
## Beam search
Nel Best First viene tenuta tutta la frontiera, se <span style="color:#ff82b2"><i>l'occupazione di memoria è eccessiva</i></span> si può ricorrere a questa variante.
La <span style="color:#ff82b2"><b>beam search</b> <i>tiene ad ogni passo solo i </i></span>$\textcolor{#ff82b2}{k}$ <span style="color:#ff82b2"><i>nodi più promettenti</i></span>, dove $k$ è detto <span style="color:#ff82b2"><i>l'ampiezza del raggio</i></span> (beam).
La beam search <span style="color:#ff82b2"><b>non è completa</b></span>.
## IDA* - A* con approfondimento iterativo
Si combina $A^*$ con [[IntroAI/Problem Solving#Approfondimento iterativo (ID)|ID]]: ad ogni iterazione ricerca <span style="color:#ff82b2"><i>in profondità</i></span> con un limite (*cut off*) dato dal valore della <span style="color:#ff82b2"><i>funzione</i></span> $\textcolor{#ff82b2}{f}$ (e non dalla profondità).
Il limite $\textcolor{#ff82b2}{f-limit}$ viene aumentato ad ogni iterazione, fino a trovare la soluzione.
Il <span style="color:#ff82b2"><b>punto critico</b></span> è di quanto viene aumentato $f-limit$
É cruciale la scelta dell'incremento per garantirne <span style="color:#ff82b2"><b>l'ottimalità</b></span>:
- <span style="color:#ff82b2"><i>costo delle azioni fisso</i></span>: il limite viene incrementato del costo delle azioni
- <span style="color:#ff82b2"><i>costi delle azioni variabili</i></span>: si potrebbe ad ogni passo fissare il limite successivo al valore minimo delle $f$ scartate all'iterazione precedente, in quanto superavano il limite.
### Analisi di IDA*
É <span style="color:#ff82b2"><b>completo</b></span> e <span style="color:#ff82b2"><b>ottimale</b></span>:
- Se le azioni hanno <span style="color:#ff82b2"><i>costo costante</i></span> $\textcolor{#ff82b2}{k}$ (caso tipico 1) e $f$-limit viene incrementato di $k$
- Se le azioni hanno <span style="color:#ff82b2"><i>costo variabile</i></span> e l'incremento di $f$-limit è $\leq\varepsilon$ (minimo costo degli archi)
- Se il nuovo $f$-limit$=\text{minimo valore }f\text{ dei nodi generati ed esclusi all'iterazione precedente}$
L'<span style="color:#ff82b2"><b>occupazione in memoria</b></span> è di $\textcolor{#ff82b2}{\mathbf{O(bd)}}$ (da [[IntroAI/Problem Solving#Ricerca in Profondità (DF)|DF]]).
## Best-First ricorsivo (RBFS)
É simile al [[IntroAI/Problem Solving#Versione ricorsiva|DF ricorsivo]]: cerca di usare meno memoria facendo del lavoro in più.
Tiene traccia ad ogni livello del <span style="color:#ff82b2"><i>migliore percorso alternativo</i></span>. Invece di fare [[Glossary#Backtracking|backtraking]] in caso di fallimento interrompe l'esplorazione quando trova un nodo promettente (secondo $f$). 
Nel tornare indietro si ricorda il miglior nodo che ha trovato nel sottoalbero esplorato, per poterci eventualmente tornare.
La <span style="color:#ff82b2"><i>memoria</i></span> è lineare nella profondità della soluzione ottima.
[[IntroAI/Romania_example#Best First ricorsivo|Esempio del viaggio in romania]]
### Algoritmo
```pseudo-code
function Ricerca-Best-First-Ricorsiva(problema) 
		returns soluzione oppure fallimento
	// all'inizio f-limite è un valore primo molto grande
	return RBFS(problema, CreaNodo(problema.Stato-iniziale), infty)

function RBFS (problema, nodo, f-limite)
		returns soluzione oppure falliento e un nuovo limite all'f-costo
	if problema.TestObiettivo(nodo.Stato) then return Soluzione(nodo)
	successori = []
	for each azione in problema.Azioni(nodo.Stato) do
		//genera i successori
		aggiungi Nodo-Figlio(problema, nodo, azione) a successori 
	if successori è vuoto then return fallimento, infty
	for each s in successori do
		// valuta i successori
		s.f=max(s.g + s.h, nodo.f) // modo per rendere monotona f
	loop do
		migliore = il nodo con f minimo tra i successori
		if migliore.f > f_limite then return fallimento, migliore.f
		alternativa = nodo con f minimo tra i successori
		risultato, igliore.f = RBFS (problema, migliore, min(f_limite, alternativa))
		if risultato != fallimento then return risultato
```
## A* con memoria limitata - Versione semplice
L'idea è quella di <span style="color:#ff82b2"><i>utilizzare al meglio la memoria diponibile</i></span>: $\textcolor{#ff82b2}{\mathbf{SMA^*}}$ procede come $A^*$ fino ad esaurimento della memoria disponibile. A questo punto <span style="color:#ff82b2"><i>"dimentica" il nodo peggiore</i></span>, dopo avere aggiornato il valore del padre.
A parità di $f$ si sceglie il nodo miglio più recente e si dimentica il peggore più vecchio.
É <span style="color:#ff82b2"><b>ottimale</b></span> se <span style="color:#ff82b2"><i>il cammino soluzione sta in memoria</i></span>.

In algoritmi a memoria limitata ($IDA^*$ e $SMA^*$) le limitazioni della memoria possono portare a compiere molto lavoro inutile. É difficiel stimare la complessità temporale effettiva. Le limitazioni di memoria possono rendere un problema intrattabile dal punto di vista computazionale.