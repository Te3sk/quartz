---
title: Crittografia a Curve Ellittiche
tags: #critto
---
##### 2022/11/14 #date 
# Necessità
Nei sistemi a chiave pubblica di <span style="color:#ff82b2"><i>prima generazione</i></span>, i <span style="color:#ff82b2"><b>tempi di cifratura</b></span> sono notevolmente <span style="color:#ff82b2"><b>maggiori</b></span> rispetto a quelli di prima generazione.
Nel 1985 nasce l'idea dell'<span style="color:#ff82b2"><b>ECC (eliptic curve cryptography)</b></span>, che sostituisce, negli algoritmi già esistenti, le operazioni basate sull'[[Crittografia/Algebra Modulare|algebra modulare]] con quelle definite sui punti di una <span style="color:#ff82b2"><b>curva ellittica</b></span>.
Il punto di forza della crittografia a curve ellittiche è che, a parità di sicurezza, richiede <span style="color:#ff82b2"><i>chiavi di dimensioni di gran lunga inferiori</i></span>.
# Curve ellittiche
Le curve ellittiche sono curve algebriche descritte da equazioni cubiche.
> [!def] Campo $\mathcal{K}$
> Un insieme non vuoto dotato di due operazioni binari interne, addizione e moltiplicazione, che soddisfano le proprietà associativa, commutativa, di esistenza dell'elemento neutro e di esistenza dell'inverso di ciascun elemento

> [!def] Curva ellittica
> Una curva ellittica su un campo $\mathcal{K}$ è definita come l'insieme
> $$
> 	\textcolor{#ff82b2}{E = \{(x,y)\in K^2 \:|\: y^2 + a\,xy+b\,y=x^3+c\,x^2+d\,x+e\} \cup \{O\}}
> $$
> dove $a,b,c,d,e\in K$

> [!def] Caratteristica di un campo
> É il più piccolo numero naturale $k$ tale che sommando $k$ volte l'elemento neutro moltiplicativo del campo $\mathcal{K}$, si ottiene l'elemento neutro additivo di $\mathcal{K}$

Se un tale $\textcolor{#ff82b2}{k}$ <span style="color:#ff82b2"><i>non esiste</i></span>, la caratteristica è 0 per definizione. Se la caratteristica del campo $\mathcal{K}$ è diversa da $2$ e da $3$, l'equazione che definisce una curva ellittica si può ridurre all'equazione <span style="color:#ff82b2"><i>cubica in forma normale di Weiestrass</i></span>
$$
\begin{array}{ccccr}
	\textcolor{#ff82b2}{y^2=x^3+a\,x^2+b} &&&&
	a,b \in K
\end{array}
$$
É possibile attribuire all'insieme dei punti di una curva la struttura algebrica di un [[Glossary#Gruppo abeliano additivo|gruppo abeliano additivo]], ovvero è possibile definire una legge di composizione interna che permette di associare ad ogni coppia di punti sulla curva, <span style="color:#ff82b2"><i>un terzo punto sempre sulla curva</i></span>.
## Curve ellittiche sui numeri reali
Le curve ellittiche sono <span style="color:#ff82b2"><i>curve algebriche</i></span> descritte da <span style="color:#ff82b2"><i>equazioni cubiche</i></span> (simili a quelle utilizzate per il calcolo della lunghezza degli archi nell'ellissi). La curva sarà costituita dall'insieme $\textcolor{#ff82b2}{E(a,b)}$ dei punti $(x,y)\in\mathbb{R}^2$ che soddisfano la proprietà detta prima, ovvero:
$$
\begin{array}{rl}
\textcolor{#ff82b2}{E(a,b)=\{(x,y)\in\mathbb{R}^2|y^2=x^3+ax+b\}} &&
\text{dove }\:a,b \in \mathbb{R}
\end{array}
$$
$E(a,b)$ contiene il <span style="color:#ff82b2"><i>punto infinito</i></span> $O$ in direzione dell'asse $y$ (la curva ha un asintoto verticale). $\textcolor{#ff82b2}{O}$ <span style="color:#ff82b2"><i>rappresenta l'elemento neutro per l'operazione di addizione</i></span>.
### Grafico
![[Crittografia/assets/curve_ellittiche.png]]
Per le <span style="color:#ff82b2"><b>applicazioni crittografiche</b></span> si assume $\boldsymbol{\textcolor{#ff82b2}{4a^3+27b^2\ne 0}}$, questo assicura che il polinomio cubico $\textcolor{#ff82b2}{x^3+ax+b}$ <span style="color:#ff82b2"><i>non abbia radici multiple</i></span> e che di conseguenza la curva sia <span style="color:#ff82b2"><i>priva di punti singolari come "cuspidi" o "nodi"</i></span> dove non sarebbe definita in modo <span style="color:#ff82b2"><i>univoco la tangente</i></span>.
> [!nb]
> Le curve ellittiche presentano una <span style="color:#ff82b2"><b>simmetria orizzontale</b></span>

Ogni punto $\textcolor{#ff82b2}{P=(x,y)}$ sulla curva che si riflette rispetto all'asse delle ascisse nel punto $\textcolor{#ff82b2}{-P=(x,-y)}$ anch'esso sulla curva. L'immagine speculare rispetto all'asse $x$ del punto all'infinito $O$ è esso stesso $\textcolor{#ff82b2}{-O=O}$.
### Somma di punti
#### Intersezioni con una retta
> [!def] Proprietà
> Una retta interseca una curva ellittica *al più in 3 punti*

Corrisponde all'intersezione tra una curva di terzo grado e una di primo grado (la retta). Sostituendo nell'equazione della curva ($\textcolor{#ff82b2}{y^2=x^3+ax+b}$) l'espressione di $y$ della retta si ottiene un'*equazione di terzo grado in* $\textcolor{#ff82b2}{x}$. Questa ha *3 soluzionei*, reali o complesse, che corrispondono all'ascisse dei punti di intersezione tra la curva ellittica e la retta
- *Una soluzione reale* e *due complesse e coniugate* (quindi un punto reale) $\implies$ **un punto di intersezione**
- *tre soluzioni reali* $\implies$ **tre punti di intersezione**
Quindi se *una retta interseca la curva* $\textcolor{#ff82b2}{E(a,b)}$ *in due punti* $P$ e $Q$ (coincidenti se la retta è tangente) allora *la retta interseca* $\textcolor{#ff82b2}{E(a, b)}$ *anche in un terzo punto* $\textcolor{#ff82b2}{R}$. 
#### Addizione su una curva ellittica
Dati tre punti $P, Q, R\in E(a, b)$, se $P, Q$ e $R$ sono disposti su una retta, si pone
$$\textcolor{#ff82b2}{P+Q+R=O}$$
Da questa definizione si ricava la regola per sommare due punti $P$ e $Q$:
1. Si considera la *retta passante per* $\textcolor{#ff82b2}{P}$ *e* $\textcolor{#ff82b2}{Q}$, oppure la *tangente in* $\textcolor{#ff82b2}{P}$ se $P$ e $Q$ coincidono
2. Si determina il *terzo punto di intersezione* $\textcolor{#ff82b2}{R}$ tra la curva e la retta (o la tangente)
3. Si definisce somma di $P$ e $Q$ il punto simmetrico a $R$ rispetto all'asse dell'ascisse
$$
\textcolor{#ff82b2}{\boldsymbol{P+Q=-R}}
$$
> [!nb]
> La somma è ben definita anche quando $-R$ è un punto della curva

Da questa definizione possiamo ricavare le <span style="color:#ff82b2"><i>regole per sommare due punti</i></span> $\textcolor{#ff82b2}{P \text{ e }Q}$:
- Si considera la retta passante per $P$ e $Q$, oppure l'unica tangente alla curva in $P$ nel caso in cui $P$ e $Q$ coincidano
- Si determina il punto di intersezione $R$ tra la curva e la retta per $P$ e $Q$, oppure tra la curva e la tangente in $P$ se $P=Q$
- Si definisce somma di $P$ e $Q$ il punto simmetrico a $R$ rispetto all'asse delle ascisse, ovvero si pone $\textcolor{#ff82b2}{P+Q=-R}$

> [!def] Proprietà della Somma 
> - <span style="color:#ff82b2"><b>Chiusura:</b></span> $\:\:\forall\:\:P,Q\in E(a,b), P+Q\in E(a,b)$
> - <span style="color:#ff82b2"><b>Elemento Neutro:</b></span> $\:\:\forall\:\:P\in E(a,b), P+Q=O+P$ (infatti le rette passanti per $O$ sono verticali, dunque la retta passante per $P$ e $O$ interseca la curva in $-P$, il cui simmetrico è $P$)
> - <span style="color:#ff82b2"><b>Inverso:</b></span> $\:\:\forall\:\:P\in E(a, b)$, esiste un unico $Q=-P\in E(a,b)$ tale che $P+Q=Q+P=O$
> - <span style="color:#ff82b2"><b>Associatività:</b></span> $\:\:\forall\:\:P,Q,R\in E(a,b), P+(Q+R)=(P+Q)+R$
> - <span style="color:#ff82b2"><b>Commutatività:</b></span> $\:\:\forall\:\:P,Q\in E(a,b), P+Q=Q+P$

<span style="color:#ff82b2"><b>In Pratica:</b></span> siano $\textcolor{#2e80f2}{P=(x_P, y_P)}$ e $\textcolor{#2e80f2}{Q=(x_Q, y_Q)}$ due punti della curva $E(a,b)$
1. $\textcolor{#2e80f2}{P\ne\pm Q}\:\:\:\:\:\:\:\:\:\:\:\:\:\:\:\:\:\boxed{\textcolor{#ff82b2}{\boldsymbol{S=P+Q}}}$
$$\begin{array}{l}x_S=\lambda^2-x_P-x_Q\\y_S=-y_P+\lambda(x_P-x_S)\\\lambda=\frac{(y_Q-y_P)}{(x_Q-x_P)}\leadsto\text{coefficiente angolare della retta per }P\text{ e }Q&&&&&&&\end{array}$$
2. $\textcolor{#2e80f2}{P=Q}\:\:\:\:\:\:\:\:\:\:\:\:\:\:\:\:\:\boxed{\textcolor{#ff82b2}{\boldsymbol{S=P+Q=2P}}}$
$$\begin{array}{l}x_S=\lambda^2-x_P-x_Q\\y_S=-y_P+\lambda(x_P-x_S)\\\lambda=\frac{(3x_P^2+a)}{2y_P}\leadsto\text{coefficiente angolare della tangente alla curva in }P\:\:(y_P\ne0)\\\text{Se }y_P=0,\:\:\:\textcolor{#ff82b2}{\boldsymbol{S=2P=O}}\end{array}$$
3. $\textcolor{#2e80f2}{Q=-P}\:\:\:\:\:\:\:\:\:\:\:\:\:\:\:\:\:\boxed{\textcolor{#ff82b2}{\boldsymbol{S=P+Q=P+(-P)=O}}}$
> [!exp]
> Se $\textcolor{#ff82b2}{4a^3+27b^2\ne0}$, la legge di addizione attribuisce all'insieme dei punti di una curva ellittica $E(a, b)$ la struttura algebrica di un *gruppo abeliano additivo*, che ha per *elemento neutro il punto all'infinito* $\textcolor{#ff82b2}{O}$

## Curve ellittiche su Campi finiti
Gli algoritmi crittografici hanno infatti bisogno di aritmetica veloce e precisa, e non possono utilizzare le curve ellittiche sui reali che richiedono elaborazioni lente e inaccurate a causa degli errori di arrotondamento. Nell'aritmetica modulare alcuni probllemi divengono computazionalmente difficili.
> [!def] Curve ellittiche prime
> Variabili e coefficienti ristretti agli elementi del campo $Z_p$ ($p$ numero primo) $a, b\in Z_p$, $p>3$
> $$
> \textcolor{#ff82b2}{E_p(a,b)=\{(x,y)\in Z_p^2\,|\,y^2 \:\:mod\:\:p=(x^3+ax+b) \:\:mod\:\:p\}\cup\{O\}}
> $$

### Inverso di un punto
Una curva ellittica prima contiene un numero finito di punti, e non è più rappresentata da una curva nel piano bensì da una <span style="color:#ff82b2"><b>nuvola di punti</b></span>. Questi punti vengono riportati nel sottospazio $0\leq x\leq p-1$, $0\leq y\leq p-1$ mentre i valori di $y$ dovrebbero essere simmetrici a coppie rispetto all'asse $x$. Notiamo però che $\textcolor{#ff82b2}{-y \text{ mod }\:\,p=p-y}$, quindi tutti i punti sono rappresentati in $0\leq y\leq p-1$ traslando verso l'alto i valori negativi, e <span style="color:#ff82b2"><i>la curva prima</i></span> $\textcolor{#ff82b2}{E_p(a,b)}$ risulta <span style="color:#ff82b2"><i>simmetrica rispetto alla retta</i></span> $\textcolor{#ff82b2}{y=\frac{p}{2}}$.
Il punto $\textcolor{#ff82b2}{(x,-y \text{ mod }\:\,p)=(x,p-y)}$ definisce <span style="color:#ff82b2"><i>l'inverso di</i></span> $\textcolor{#ff82b2}{P}$ e si indica con $\textcolor{#ff82b2}{-P}$.
> [!def] **Inverso** di un punto P
> $$
> 	\begin{array}{l}
> 		\textcolor{#ff82b2}{P=(x,y)} \in E_p(a,b)\\
> 		\textcolor{#ff82b2}{-P=(x, -y)=(x,p-y)}\in E_p(a,b)
> 	\end{array}
> $$

Se il polinomio $\textcolor{#2e80f2}{x^3+ax+b(mod\:\:p)}$ non ha radici multiple, ovvero se $$\textcolor{#ff82b2}{4a^3+27b^2(mod\:\:p)\ne 0}$$i punti della curva $\textcolor{#ff82b2}{E_p(a,b)}$ formano un <span style="color:#ff82b2"><i>gruppo abeliano additivo finito</i></span>.
Regole e formulazione algebrica della somma definite per le curve ellittiche sui numeri reali si adattano al caso finito, con accortezza di intendere e ed eseguire tutte le operazioni in algebra modulare.
### Somma di punti
Le coordinate del punto $P+Q$ si possono determinare a partire dalle coordinate di $P$ e di $Q$ utilizzando le furmule viste per la somma sulle curve ellittiche reali eseguendole in moduo $p$.
|                         $\textcolor{#2e80f2}{P\ne\pm Q}$ | $\textcolor{#2e80f2}{P=\pm Q}$ |
| --------------------------------------------------------:| ------------------------------ |
| $\lambda = \frac{(y_Q-y_P)}{(x_Q-x_P)}\text{ mod }\:\,p$ | $\lambda=\frac{(3x_P^2+a)}{2y_P} \text{ mod }\:\,p$                               |
## Curve ellittiche binarie
> [!def] 
> Curve i cui coefficienti e le variabili assumono valori nel campo $\textcolor{#ff82b2}{GF(2^m)}$

Sono costituite da $\textcolor{#ff82b2}{2^m}$ <span style="color:#ff82b2"><i>elementi</i></span> che si possono pensare come tutti gli interi binari di $m$ cifre, su cui si può operare mediante l'aritmetica polinomiale modulare.
Caratteristicha del campo $GF(2^m)$ è uguale a 2, l'equazione che definisce la curva ellittica assume una forma leggermente differente (non si può usare la <span style="color:#ff82b2"><i>forma normale di Weierstrass</i></span>). <span style="color:#ff82b2"><b>I protocolli crittografici su curve ellittiche si applicano indistintamente a entrambe le famiglie di curve</b></span>
## Ordine di una curva
L'<span style="color:#ff82b2"><i>ordine di una curva</i></span> è il numero dei suoi punti. Una curva prima $\textcolor{#ff82b2}{E_p(a,b)}$ può avere al più $\textcolor{#ff82b2}{2p+1}$ punti: il punto all'infinito e le $\textcolor{#ff82b2}{p}$ <span style="color:#ff82b2"><i>coppie di punti</i></span> $\textcolor{#ff82b2}{(x,y)}$ *e* $\textcolor{#ff82b2}{(x, p-y)}$ che soddisfano l'equazione
$$
\textcolor{#ff82b2}{
	y^2 \:\:mod\:\:p=(x^3+ax+b)\:\:mod\:\:p
}
$$
al variare di $x$ in $Z_p$.
La curva contiene in genere $\Theta(p)$ punti, considerando i <span style="color:#ff82b2"><i>residui quadratici</i></span> di $Z_p$ (gli elementi del campo che ammettono radice quadrata in $Z_p$).
In genere <span style="color:#ff82b2"><i>non tutti i valori di</i></span> $\textcolor{#ff82b2}{x}$ <span style="color:#ff82b2"><i>in</i></span> $\textcolor{#ff82b2}{Z_p}$ <span style="color:#ff82b2"><i>danno luogo a un residuo quadratico</i></span> nel campo, e quindi la curva non contiene punti con quelle ascisse
Si può dimostrare che <span style="color:#ff82b2"><i>esattamente</i></span> $\textcolor{#ff82b2}{\frac{(p-1)}{2}}$ <span style="color:#ff82b2"><i>elementi di</i></span> $\textcolor{#ff82b2}{Z_p}$ (<span style="color:#ff82b2"><i>oltre lo</i></span> $\textcolor{#ff82b2}{0}$) <span style="color:#ff82b2"><i>sono residui quadratici</i></span>, e ciascuno di essi ha esattamente due radici quadrate in $Z_p$ la cui somma è $p$.
<span style="color:#ff82b2"><i>Valori diversi di</i></span> $\textcolor{#ff82b2}{x}$ <span style="color:#ff82b2"><i>possono generare lo stesso residuo quadratico</i></span> e quindi possono esistere coppie di punti con $x$ diverso e gli stessi valori di $y$.
In generale, ci si può aspettare che la curva sia formata da <span style="color:#ff82b2"><i>circa</i></span> $\textcolor{#ff82b2}{p}$ <span style="color:#ff82b2"><i>punti</i></span> oltre al punto all'infinito.
> [!the] Teoremma di Hasse
> L'<span style="color:#ff82b2"><i>ordine</i></span> $\textcolor{#ff82b2}{N}$ di una curva ellittica $E_p(a,b)$ verifica la disuguaglianza $\textcolor{#ff82b2}{|N-(p+1)|\leq 2\sqrt{p}}$

## Funzione one-way trap-door
Per definire un sistema crittografico a chiave pubblica usando le curve ellittiche occore individuare una <span style="color:#ff82b2"><i>funzione one-way trap-door</i></span> che ne garantisca la sicurezza.
Per le curve ellittiche si definisce una funzione one-way trap-door analoga all'elevamento a potenza/<span style="color:#ff82b2"><b>logaritmo discreto</b></span> nell'algebra modulare. L'operazione di <span style="color:#ff82b2"><i>addizione di punti</i></span> di una curva ellittica su un campo finito presenta analogie con l'operazione di <span style="color:#ff82b2"><i>prodotto modulare</i></span>, fissato un intero positivo $\textcolor{#2e80f2}{k}$:
$$
\begin{array}{lcccr}
\stackrel{\Large{\textcolor{#2e80f2}{
	\begin{array}{c}
		\text{elevamento alla potenza}\\
		k \text{ di un intero in modulo}
	\end{array}
}}}{
	y=g^k \:\:mod\:\:p
} &&
\iff &&
\stackrel{\Large{\textcolor{#ff82b2}{
	\begin{array}{c}
		\text{moltiplicazione scalare per } k\\
		\text{di un punto }P\text{ di una curva ellittica}
	\end{array}
}}}{
	\stackrel{\Large{Q=kP=P+P+\,\cdots\,+P}}{(k\text{ volte})}
}
\end{array}
$$
### Moltiplicazione scalare
Entrambe le operazioni si possono eseguire in tempo polinomale
$$
\begin{array}{lcr}
	\boxed{
		\begin{array}{l}
			\textcolor{#2e80f2}{\underline{\text{Metodo delle quadrature successive}}}\\
			\text{Consente di calcolare }\textcolor{#2e80f2}{y=g^k \:\:mod\:\:p}\text{ con}\\
			\rightarrow \textcolor{#2e80f2}{\Theta(log\:\:k)}\text{ elevamenti al quadrato}\\
			\rightarrow \textcolor{#2e80f2}{O(log\:\:k)}\text{ moltiplicazioni} 
		\end{array}
	}
	&&
	\boxed{
		\begin{array}{r}
			\textcolor{#ff82b2}{\underline{\text{Metodo del raddoppio ripetuto}}}\\
			\text{consente di calcolare }\textcolor{#ff82b2}{Q=kP}\text{ con}\\
			\rightarrow \textcolor{#ff82b2}{\Theta(log\:\:k)}\text{ raddoppi}\\
			\rightarrow \textcolor{#ff82b2}{O(log\:\:k)}\text{ addizioni di punti}
		\end{array}
	}
\end{array}
$$
- Si scrive $k$ come <span style="color:#ff82b2"><i>somma di potenze del 2</i></span>: $k= \textcolor{#ff82b2}{\sum_{i=0}^t k_i2^i}$, dove $k_t\dots k_1k_0$ è la rappresentazione binaria dell'intero $k$, con $t=\lfloor log_2 k\rfloor$
- Si <span style="color:#ff82b2"><i>calcolano</i></span> in successione <span style="color:#ff82b2"><i>i punti</i></span> $\textcolor{#ff82b2}{2P,4P,\dots,2^tP}$, ottenendo ciascuno dal raddoppio del precedente (complessivamente si eseguono $\textcolor{#ff82b2}{t}$ <span style="color:#ff82b2"><i>raddoppi</i></span>)
- Infine si <span style="color:#ff82b2"><i>calcola</i></span> $\textcolor{#ff82b2}{Q}$ come <span style="color:#ff82b2"><i></i></span> $\textcolor{#ff82b2}{2^iP}$, per $0\leq i\leq t$ <span style="color:#ff82b2"><i>tale che</i></span> $\textcolor{#ff82b2}{k_i=1}$
> [!exp]
> $\textcolor{#ff82b2}{Q=13\:\:\:P=(8+4+1)P}$
> Si calcola con 3 raddoppi
> $$
> \begin{array}{l}
> 	\rightarrow 2P\\
> 	\rightarrow 4P=2(2P)\\
> 	\rightarrow 8P=2(4P)=2(2(2P))
> \end{array}
> $$
> e due addizioni
> $$\textcolor{#ff82b2}{Q=P+4P+8P}$$

##### 2022/11/22 #date 
### Logaritmo discreto per le curve ellittiche
$$
\boxed{
\begin{array}{c}
	\text{\textcolor{#2e80f2}{Dati} due punti } \textcolor{#2e80f2}{P}\text{ e }\textcolor{#2e80f2}{Q}\text{ trovare, se esiste,}\\
	\text{il più piccolo intero }\textcolor{#2e80f2}{k}\text{ tale che }\textcolor{#2e80f2}{Q=k\,P}
\end{array}
}
$$
*k*, quando definito, è chiamato <span style="color:#ff82b2"><b>logaritmo in base</b></span> $\textcolor{#ff82b2}{\boldsymbol{P}}$ <span style="color:#ff82b2"><b>del punto</b></span> $\textcolor{#ff82b2}{\boldsymbol{Q}}$ (in analogia al problema del logaritmo discreto su insiemi finiti, operazione inversa dell'elevamento a potenza in modulo).
<span style="color:#ff82b2"><i>Il problema del logaritmo discreto per le curve ellittiche risulta computazionalmente difficile</i></span>
- tutti gli algoritmi noti hanno complessità esponenziale se i parametri della curva sono scelti bene.
- non è stato ancora trovato un algoritmo per risolvere questo problema che migliori il metodo a forza bruta basato sul calcolo di tutti i multipli di $P$ fino a trovare $Q$, improponibile se $k$ è sufficientemente grande.
$$
\begin{array}{lr}
	\boxed{
		\begin{array}{l}
			\text{Funzione \textcolor{#2e80f2}{computazionalmente trattabile} in}\\
			\text{una direzione \textcolor{#2e80f2}{calcolo di }}\textcolor{#2e80f2}{Q}=k\text{ dati }\textcolor{#2e80f2}{P}\text{ e }\textcolor{#2e80f2}{k}\\
			\text{ma praticamente \textcolor{#2e80f2}{intrattabile} nell'altra}\\
			\textcolor{#2e80f2}{\text{calcolo di }k}\text{ dati }\textcolor{#2e80f2}{P \text{ e }Q}=k\,P
		\end{array}
	}
	&
	\boxed{
		\begin{array}{r}
			\textcolor{#ff82b2}{\text{Funzione one-way trap-door}}\\
			\text{su cui basare la sicurezza della}\\
			\text{crittografia su curve ellittiche}
		\end{array}
	}
\end{array}
$$
## Applicazioni in crittografia
Due algoritmi crittografici che usano le curve ellittiche sono riproposizioni degli algoritmi classici basati sul problema del logaritmo discreto, e sono ottenute sostituendo le operazioni dell'algebra modulare con le operazioni sui punti di una curva prima o binaria
$$
\begin{array}{rcl}
	\textcolor{#2e80f2}{\text{prodotto}} & \leadsto & \text{\textcolor{#ff82b2}{somma di punti}}\\
	\text{\textcolor{#2e80f2}{esponenziazione}} & \leadsto & \text{\textcolor{#ff82b2}{moltiplicazione scalare di un}}\\
	&& \text{\textcolor{#ff82b2}{intero per un punto della curva}}
\end{array}
$$
I due nuovi algoritmi garantiscono <span style="color:#ff82b2"><i>maggior sicurezza</i></span>.
### Protocollo DH su curve ellittiche
Mittente e ricevente si accordano pubblicamente su un <span style="color:#ff82b2"><i>campo finito</i></span> e su una <span style="color:#ff82b2"><i>curva ellittica definita su questo campo</i></span>. Scelgono, nella curva, un <span style="color:#ff82b2"><i>punto</i></span> $\textcolor{#ff82b2}{B}$ <span style="color:#ff82b2"><i>di ordine molto grande</i></span>. Ordine di un punto $B$ della curva è il più piccolo intero positivo $n$ tale che $\textcolor{#ff82b2}{nB=O}$.
| $\textcolor{#2e80f2}{\textbf{Mittente}}$                                                         |                                                                                        $\textcolor{#ff82b2}{\textbf{Ricevente}}$ |
|:------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------:|
| sceglie a caso $\textcolor{#2e80f2}{n_A<n}$ come chiave privata                                  |                                  sceglie a caso $\textcolor{#ff82b2}{n_B<n}$ come chiave privata |
| genera una chiave pubblica $\textcolor{#2e80f2}{P_A=n_AB}$                                       |                                       genera una chiave pubblica $\textcolor{#ff82b2}{P_B=n_BB}$ |
| invia $\textcolor{#2e80f2}{P_A}$ in chiaro al ricevente                                          |                                           invia $\textcolor{#ff82b2}{P_B}$ in chiaro al mittente |
| riceve $\textcolor{#ff82b2}{P_B}$ e calcola $\boldsymbol{S=}\textcolor{#2e80f2}{n_AP_B=n_An_BB}$ | riceve $\textcolor{#2e80f2}{P_A}$ e calcola $\boldsymbol{S=}\textcolor{#ff82b2}{n_BP_A=n_Bn_AB}$ |

<span style="color:#ff82b2"><i>Mittente e ricevente condividono lo stesso punto</i></span> $\textcolor{#ff82b2}{S}$ <span style="color:#ff82b2"><i>determinando delle scelte casuali di entrambi</i></span>. Per trasformare $S$ in una chiave segreta $K$ per la cifratura simmetrica convenzionale occorre convertirlo in un numero intero, ad esempio si pone $K=x_S \:\:mod\:\:2^{256}$ ($K\leftarrow$ ultimi $256$ bit dell'ascissa di $S$)
#### Crittoanalisi
- intercetta i messaggi $\textcolor{#ff82b2}{P_A}$ e $\textcolor{#ff82b2}{P_B}$ scambiati in chiaro
- conosce i *parametri della curva* ellittica impiegata e il *punto* $\textcolor{#ff82b2}{B}$
- non è in grado di violare lo schema
	- per calcolare $S$ dovrebbe calcolare $n_A$ dati $B$ e $P_A$, oppure $n_B$ dati $B$ e $P_B$
	- dovrebbe *risolvere il problema del logaritmo discreto per le curve ellittiche*, certamente intrattabili date le dimensioni dei valori coinvolti.
> [!dan]
> Il protocollo è comunque vulnerabile agli attacchi di tipo man-in-the-middle, come il protocollo DH sui campi finiti.

## Scambio di messaggi cifrati: cifrario di [[Crittografia/ElGamal|ElGamal]] su curve ellittiche
Per cifrare un <span style="color:#ff82b2"><i>messaggio</i></span> $\textcolor{#ff82b2}{m}$, codificato come un numero intero usando le curve ellittiche, occorre per prima cosa <span style="color:#ff82b2"><i>trasformare</i></span> $\textcolor{#ff82b2}{m}$ <span style="color:#ff82b2"><i>in un punto di una curva prima o binaria</i></span>, che sarà a sua volta trasformato per in un nuovo punto da usare come testo cifrato.
**Creazione della coppia di chiavi:**
1. Si sceglie un *numero primo* $\textcolor{#ff82b2}{p}$, una *curva ellittica prima* $\textcolor{#ff82b2}{E_p(a,b)}$ e un *punto* $B$ della curva di ordine $n$ elevato
2. Ogni $\textcolor{#2e80f2}{\text{Utente } U}$ genera la propria coppia chave pubblica/chiave privata
	1. *sceglie a caso* un intero $\textcolor{#2e80f2}{n_U<n}$
	2. calcola $P_U=n_UB$
$$
\begin{array}{rl}
	\textcolor{#2e80f2}{\text{Chiave pubblica}} & \textcolor{#2e80f2}{k_{pub}=(P_U)}\\
	\textcolor{#ff82b2}{\text{Chiave privata}} &
	\textcolor{#ff82b2}{k_{priv})(n_U)}
\end{array}
$$
Per cifrare un *messaggio* $\textcolor{#ff82b2}{m}$ codificato come numero intero utilizzando le curve ellittiche occorre per prima cosa *trasformare* $\textcolor{#ff82b2}{m}$ *in un punto di una curva*. Non è noto alcun algoritmo deterministico polinomiale per effettuare tale trasformazione, esistono tuttavia degli algoritmi randomizzati molto efficienti che hanno una probabilità arbitrariamente bassa di fallire (non produrre un punto della curva).
Per <span style="color:#ff82b2"><b>trasformare il messaggio in un punto della curva</b></span> non è ancora noto  alcun algoritmo deterministico polinomiale, esistono però degli algoritmi randomizzati molto efficienti che anno una probabilità arbitrariamente bassa di fallire.
### Algoritmo di Koblitz
Dato $\textcolor{#ff82b2}{m}<p$, trovare $\textcolor{#ff82b2}{P_m\in E_p(a,b)}$
	usando $\textcolor{#ff82b2}{m}$ come ascissa, la *probabilità di trovare un punto della curva* è pari alla probabilità che $m^3+am+b \:\:(mod\:\:p)$ sia un residuo quadratico $\textcolor{#ff82b2}{\sim \frac{1}{2}}$
Si fissa $\textcolor{#ff82b2}{h}$ intero positivo tale che $\textcolor{#ff82b2}{(m+1)h<p}$
```
For i = 0 to h-1 {
	x = m h + i
	se esiste la radice quadrata y di x^3+ax+b (mod p)
		return p_a=(x,y)
}
return failure
```
La procedure si itera al più $\textcolor{#ff82b2}{h}$ <span style="color:#ff82b2"><i>volte</i></span>, la probabilità che, per ogni valore di $x$ considerato, $x^3+ax+b \:\:(mod\:\:p)$ non sia un quadrato è circa $\frac{1}{2}$. Il metodo ha una <span style="color:#ff82b2"><i>probabilità complessiva di successo pari circa a</i></span> $\textcolor{#ff82b2}{1-(\frac{1}{2})^h}$, per risalire al dato $\textcolor{#ff82b2}{P_m=(x,y)}$ basta calcolare $\textcolor{#ff82b2}{m=\lfloor \frac{x}{h}\rfloor}$.
### Algoritmo per lo scambio di chiavi
Anche in questo caso occorre accordarsi su una curva ellittica, su un punto $B$ della curva di ordine $n$ elevato e su un intero $h$ per la trasformazione dei messaggi in punti della curva. Ogni utente $U$ genera la propria coppia $\langle\text{chiave pubblica/chiave privata}\rangle$ scegliendo un intero casuale $n_U<n$ che chiava privata e pubblicando la chiave $P_U=n_UB$.
$$
\begin{array}{rl}
	\boxed{
		\begin{array}{l}
			\textcolor{#ff82b2}{\underline{\text{Cifratura (mittente)}}}\\
			\rightarrow \text{trasforma il messaggio }\textcolor{#ff82b2}{m} \\
			\:\:\:\:\:\text{ in un punto }\textcolor{#ff82b2}{P_m} \text{sulla curva}\\
			\rightarrow \text{sceglie a caso un intero }\textcolor{#ff82b2}{r<n}\\
			\rightarrow \text{calcola la coppia di crittogrammi }\\
			\:\:\:\:\:(\text{punti sulla curva})\\
			\begin{array}{cclcr}
				&&\textcolor{#ff82b2}{V=rb}&&
				\textcolor{#ff82b2}{W=P_m+rP_{Bob}}
			\end{array}	\\
			\rightarrow \text{invia la coppia di punti}\\
			\:\:\:\:\:<V,W> \text{ al ricevente}		
		\end{array}
	}
	&
	\boxed{
		\begin{array}{l}
			\textcolor{#2e80f2}{\underline{\text{Decifrazione (ricevente)}}}\\
			\rightarrow \text{riceve la coppia di punti} <V,W>\\
			\rightarrow \text{ricostruisce il punto }P_m \text{ utilizzando}\\
			\:\:\:\:\: \text{la sua chiave privata }n_{bob}\\
			\begin{array}{cclcr}
				&&&&\textcolor{#2e80f2}{P_m=W-n_{Bob}\,V}
			\end{array}	\\
			\:\:\:\:\:\text{infatti } \textcolor{#2e80f2}{W}-n_{Bob}\textcolor{#ff82b2}{V}= \textcolor{#2e80f2}{P_m+r\,P_{Bob}}-n_{Bob}\textcolor{#ff82b2}{V}=\\
			\:\:\:\:\:=P_m +r\:n_{Bob}\,B-n_{Bob}\,\textcolor{#ff82b2}{r\:\:B}=P_m\\
			\rightarrow \text{trasforma }P_m \text{ nel messaggio }m
		\end{array}
	}
\end{array}
$$
#### Crittoanalisi
Per risalire a $m$, un *crittoanalista* che ha intercettato il crittogramma $<V,W>$ dovrebbe
- *calcolare* $\textcolor{#ff82b2}{r}$ da $\textcolor{#ff82b2}{B}$ e $\textcolor{#ff82b2}{V}$  ($V=r\:B$)
- oppure la chiave privata $\textcolor{#ff82b2}{n_{Bob}}$ da $\textcolor{#ff82b2}{B}$ e $\textcolor{#ff82b2}{P_{Bob}}$  ($P_{Bob}=n_{Bob}\:B$)
cioè dovrebbe risolvere il **problema del logaritmo discreto**.
## Sicurezza della crittografia su curve ellittiche
É legata alla *difficoltà di calcolare il logaritmo discreto* di un punto, problema per cui non è noto alcun algoritmo efficiente di risoluzione.
Nonostante manchi una dimostrazione formale di intrattabilità, questo problema è considerato *estremamente difficile*, in particolare *molto più difficile* dei tradizionali problemi della fattorizzazione degli interi e del logaritmo discreto nell'algebra modulare.
	Per questi problemi esiste un *algoritmo subesponenziale* (**index calculus**) che può essere utilizzato per attacchare sia il protocollo DH che il cifrario RSA. L'algoritmo *index calculus* sfrutta una struttura algebrica dei campi finiti che non è presente sulle curve ellittiche. Ad oggi nessuno è stato capace di progettare algoritmi di tipo index calculus per il problema del logaritmo discreto per le curve ellittiche: quindi i protocolli per tali curve sembrano invulnerabili a questo tipo di attacchi
Il migliore attacco noto (**Pollard** $\textcolor{#ff82b2}{\boldsymbol{\rho}}$) richiede in media $\textcolor{#ff82b2}{O(2^{b/2})}$ operazioni per chiavi di $b$ bit ed è dunque *pienamente esponenziale*.
Esistono attacchi efficaci contro alcune famiglie di curve, ma queste famiglie sono note e facilmente evitate.
### Sicurezza
Tabella di equivalenza fra i livelli di sicurezza dei cifrari simmetrici rispetto ai cifrari a chiave pubblica (NIST)
| TDEA, AES (bit della chiave) | RSA e DH (bit del modulo) | ECC (bit dell'ordine) |
| :----------------------------: | :-------------------------: | :---------------------: |
| 80                           | 1024                      | 160                   |
| 112                          | 2048                      | 224                   |
| 128                          | 3072                      | 256                   |
| 192                          | 7680                      | 384                   |
| 256                          | 15360                     | 512                   |

Riporta la dimensione in bit delle chiavi che garantiscono livelli di sicurezza equivalenti nei tre diversi sistemi
> [!def]
>due sistemi si considerano **di sicurezza equivalente** se è richiesto lo stesso costo computazionale per forzarli

É interessante il confronto tra RSA e ECC
	*La differenza tra i cifrari asimmetrici RSA/DH ed ECC diventa evidente al crescere del livello di sicurezza richiesto*
### "Misura universale di sicurezza"
> Arjen K. Lenstra, Thorsten Kleinjung e Emmanuel Thom
> Universal Security - From Bits and Mips to Pools, Lakes - and Beyond.
> Number Theory and Cryptography, 2013

L'**idea** è quella di misurare la quantità di energia necessaria per forzare un sistema e confrontarla con la quantità di acqua che quell'energia potrebbe far bollire.
Forzare un cifrario *RSA a 228 bit* richiede meno energia di quella necessaria per far bollire un *cucchiaino d'acqua*.
L'energia necessaria per forzare un sistema basato su *curve ellittiche a 228 bit* potrebbe far bollire *tutta l'acqua sulla terra*.
