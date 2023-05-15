---
title: ML - SVM
alias:
- Support Vector Machine
tags: introai
---
# Introduzione
Un <span style="color:#ff82b2"><b>classificatore</b></span> derivato dalla teoria **teoria di apprendimento statistico** di Vapnik. Dopo anni di sviluppo teorico, l'SVM diventa famoso quando, usando immagini come input, da un'<span style="color:#ff82b2"><b>accuratezza</b></span> paragonabile al <span style="color:#ff82b2"><i>SotA neural-network</i></span>.
Oggi l'SVM è largamente usata intutti i campi del supervised learning e per la regressione
# I nostri obiettivi sulle SVM
1. <span style="color:#ff82b2"><b>Max Margin Classifier</b></span>
	Controllo della <span style="color:#ff82b2"><b>complessità</b></span> del modello con un <span style="color:#ff82b2"><i>approccio di ottimizzazione</i></span>, per approssimare direttamente il rischio strutturale della minimizzazione.
2. <span style="color:#ff82b2"><b>Kernel</b></span>
	Usare efficientemente l'espansione di base lineare con il kernel, otteniamo quindi un altro approccio <span style="color:#ff82b2"><b>flessibile</b></span> per il supervised learning non lineare
3. <span style="color:#ff82b2"><b>Pratice</b></span>
	Evitare le tipiche malinterpretazioni nell'uso dell'SVM, sono convizioni troppo ottimistiche sull'overfitting e sull'evitamento del corso dimensionale
## 1. Margin Example
[image pag.11 of 3.5]
[image pag.12 of 3.5]
Non tutti gli hyperplane che risolvono le task sono uguali, variando l'hyperplane di separazione, varia anche il margine.
> [!quote] Margin
> 
>Il <span style="color:#ff82b2"><b>margine</b></span> è (il doppio) la distanza tra la separazione dell'hyperplane e i punti dei dati più vicini

<span style="color:#ff82b2"><i>Intuitivamente:</i></span> la massima distanza tra i punti dei dati
> [!quote] Support Vector
> 
> [image pag.13 of 3.5]
> $$
> 	\begin{array}{ccc}
> 		\textcolor{#ff82b2}{x_i} & t.c. & \textcolor{#ff82b2}{|w^Tx_p+b|=1}
> 	\end{array}
> $$

Tutti i punti sono classificati correttamente se $\textcolor{#ff82b2}{(w^Tx_i+b)y_i\geq1}\text{ }\forall \text{ i dati } \textcolor{#ff82b2}{l}$.
### Toward margin example opt. problem & Canonical rap. of hyperplane and SV
Consideriamo il problema di <span style="color:#ff82b2"><i>apprendere un modello lineare</i></span> per una classificazione binaria $\stackrel{quindi}{\textcolor{#ff82b2}{\implies}}$ una funzione $\textcolor{#ff82b2}{h:\mathbb{R}^N\to\{0,1\}}$, $\textcolor{#ff82b2}{h(x)=sign(wx+b)}$ basata sugli esempi $\textcolor{#ff82b2}{(x_p,y_p)}$ nel training set.
> [!quote] Training Problem
> 
> Trovare $\textcolor{#ff82b2}{(w,b)}$ tali che tutti i punti sono classificati correttamente e i margini sono <span style="color:#ff82b2"><i>massimizzati</i></span>

##### Rappresentazione Canonica dell'hyperplane
$$
\begin{array}{c}
	\textcolor{#ff82b2}{(x_p,y_p)}\text{ classificati correttamente }\textcolor{#ff82b2}{\forall p}
	\\
	{\large\textcolor{#ff82b2}{\iff}}
	\\
	\begin{array}{cccc}
		\textcolor{#ff82b2}{\textcolor{#ff82b2}{0}}\text{ if }\textcolor{#ff82b2}{y_p=1}
		&
		\wedge
		&
		\textcolor{#ff82b2}{w^Tx_p+b<0}\text{ if }\textcolor{#ff82b2}{y_p=1}
		&
		\textcolor{#ff82b2}{\forall p}
	\end{array}
\end{array}
$$
Senza perdita di generalità, è possibile <span style="color:#ff82b2"><i>ridimensionare</i></span> $\textcolor{#ff82b2}{w}$ così che i punti più vicini all'hyperplane che soddifa $\textcolor{#ff82b2}{|w^Tx_p+b|=1}\stackrel{quindi}{\textcolor{#ff82b2}{\implies}}$ al Support vector
$$
\begin{array}{ccc}
	\textcolor{#ff82b2}{|w^Tx_p+b|\geq1}\text{ if }\textcolor{#ff82b2}{y_p=1}
	& \wedge &
	\textcolor{#ff82b2}{w^Tx_p+b\leq1}\text{ if }\textcolor{#ff82b2}{y_p=-1\text{ }\forall p}
	\\
	& \textcolor{#ff82b2}{\iff} &
	\\
	\textcolor{#ff82b2}{(w^Tx_p+b)y_p\geq1\text{ }\forall p}
	&
	\leftarrow
	&
	\begin{array}{r}
		\text{\textcolor{#ff82b2}{condizioni:} tutti i punti sono}
		\\
		\text{classificati correttamente}
	\end{array}
\end{array}
$$
### Two useful fact
> [!quote] Definition
> 
> $$
> \begin{array}{}
> 	\textcolor{#ff82b2}{margin\propto{\large\frac{2}{|w|}}}
> 	&&
> 	\text{con }\textcolor{#ff82b2}{|w|^2=(w^Tw)}\text{, norma}
> 	\\ \\
> 	\begin{array}{cccccc}
> 		\text{massimizzare i margini}
> 		&
> 		\textcolor{#ff82b2}{\iff}
> 		&
> 		\text{minimizzare }\textcolor{#ff82b2}{|w|}
> 		&
> 		\textcolor{#ff82b2}{\iff}
> 		&
> 		\text{minimizzare }\textcolor{#ff82b2}{{\large\frac{|w|^2}{2}}}
> 	\end{array}
> \end{array}
> $$

> [!quote] Definition
> 
> <span style="color:#ff82b2"><b>VC-dim</b></span> dell'SVM è <span style="color:#ff82b2"><i>inverso al margine</i></span> $\stackrel{quindi}{\textcolor{#ff82b2}{\implies}}$ è decrescente con margini alti
> - Controllo della complessità del modello con i margini
> - Lo connettiamo con l'<span style="color:#ff82b2"><b>SLT lecture</b></span>

L'<span style="color:#ff82b2"><b>hyperplane ottimo</b></span> che massimizza i margini e risolve il problema di training.
### Hard margin SVM
> [!quote] Training Problem
> Trovare $\textcolor{#ff82b2}{(w,b)}$ t.c. tutti i punti di training sono classificati correttamente e i margini sono massimizzati
> 	<span style="color:#ff82b2"><b>Forma Primordiale:</b></span>
> 	minimizzare $\textcolor{#ff82b2}{\large\frac{|w|^2}{2}}\stackrel{quindi}{\textcolor{#ff82b2}{\implies}} \textcolor{#ff82b2}{w^tw}$ t.c. $\textcolor{#ff82b2}{(w^tx_p+b)y_p\geq1\text{ }\forall p=1,...,l}$

É una <span style="color:#ff82b2"><i>minimizzazione diretta della complessità del modello</i></span>, tenendo la soluzione nei vincoli. La funzione obiettivo è convessa in $\textcolor{#ff82b2}{w}$.
#### Dual problem
> [!quote] Dual formulation of training
> 
> Massimizzare$_\alpha\sum_i\alpha_i-{\Large\frac{\sum_{ij}\alpha_i\alpha_jy_iy_jx_i^tx_j}{2}}$   $i,j=1,...,l$
> con $\textcolor{#ff82b2}{\alpha_i\geq0, \sum_i\alpha_iy_i=0}$

Dobbiamo trovare un $\textcolor{#ff82b2}{\alpha_p}$ ottimale, con $\textcolor{#ff82b2}{p=1,...,l}$ (<span style="color:#ff82b2"><i>Moltiplicatore di Lagrange</i></span>) con la <span style="color:#ff82b2"><b>programmazione quadratica</b></span>. Il fatto che sia convessa implica un'<span style="color:#ff82b2"><i>unica soluzione</i></span>, inoltre il costo computazionale scala con $\textcolor{#ff82b2}{l}$ invece che con $\textcolor{#ff82b2}{n}$ (con il numero dei dati invece che con la loro dimensione).
La doppia formulazoine (calcolando i valori di $\alpha$) ci consente di <span style="color:#ff82b2"><i>mostrare i Support Vectors</i></span> e una forma speciale della soluzione.
##### Solution
Con $\alpha$ (calcolata in forma doppia) possiamo calcolare $\textcolor{#ff82b2}{(w,b)}$ 
$$
\begin{array}{c|c|cc}
	\textcolor{#ff82b2}{w=\sum_p\alpha_py_px_p}
	&
	p=1,...,l
	&
	\textcolor{#ff82b2}{b=y_k-w^tx_k}
	&
	\text{for any }\textcolor{#ff82b2}{\alpha_k>0}
\end{array}
$$
> [!quote] Definition
> 
> $$
> \textcolor{#ff82b2}{
> 	h(x)=sign(w^Tx+b){\large=}
> 	sign(
> 		\sum_{p=1}^l\alpha_py_px_p^Tx+b
> 	)
> 	{\large=}
> 	\textcolor{#fc0202}{\boxed{\textcolor{#ff82b2}{
> 		sign(\sum_{p\in SV}\alpha_py_px_p^Tx+b)
> 	}}}
> }
> $$

> [!quote] Definition
> 
> 1. $\textcolor{#ff82b2}{\large\alpha<>0}\iff$ <span style="color:#ff82b2"><b>Support Vectors</b></span>
> ($\alpha_p\ne0\to$ is a support vector)
> La soluzione è (spesso) sparsa e formulata solo in termini di SVs. L'hyperplane dipende solo dal support vector
> 2. <span style="color:#ff82b2"><i>una forma speciale della soluzione:</i></span> non è neanche necessario calcolare $\textcolor{#ff82b2}{w,b}$ esplicitamente per classificare i punti

#### Role of support vector
[image pag.19 of 3.5]
$$
\textcolor{#ff82b2}{h(x)=sign(\sum_{p\in SV}\alpha_py_px_p^Tx+b)}
$$
L'hyperplane dipende solo dai support vectors
#### Role of inner support
$$
\textcolor{#ff82b2}{h(x)=sign(\sum_{p\in SV}\alpha_py_p{\textcolor{#fc0202}{\boxed{\textcolor{#ff82b2}{x_p^Tx}}}}+b)}
$$
I dati vengono inseriti sotto forma di prodotti scalari di coppie di punti
### Soft Margin
Gli hard margin possono essere troppo restrittivi per tutti i punti, <span style="color:#ff82b2"><i>alcuni errori possono essere ammessi</i></span> per la tolleranza del disturbo dei dati e per fornire un margine maggiore. La soluzione è ammettere errori introducendo <span style="color:#ff82b2"><b>slack-variables</b></span>.
> [!quote] Primal training problem
> 
> minimizzare ${\large\frac{|w|^2}{2}}\textcolor{#fc0202}{+C\cdot\sum_p\xi_p}$
> tale che $(wx_p+b)y_p\geq1 \textcolor{#fc0202}{-\xi_p}$ e $\xi_p\geq0$  $\forall p$

$\xi_p$ positivo indica un errore o margini troppo piccoli, $\textcolor{#fc0202}{C}>0$ <span style="color:#ff82b2"><i>guida il numero di errori ammessi</i></span> (l'indice di $\xi_p$ è calcolato dal risolutore).
$\textcolor{#fc0202}{C}$ <span style="color:#ff82b2"><i>è un hyperparameter definito dall'utente</i></span>
	$C$ basso $\to$ sono ammessi troppi errori di training $\to$ possibile <span style="color:#ff82b2"><b>overfitting</b></span>
	$C$ alto $\to$ non sono ammessi errori di training $\to$ margini piccoli $\to$ possibile <span style="color:#ff82b2"><b>overfitting</b></span>
## 2. kernel
Usare efficientemente le espansioni lineari di basis con kernel, quindi ottentiamo un altro approccio flessibile per il supervised learning non lineare.
### Mapping to High-Dimensional Space
Mappare i punti dei dati nello spazio di input <span style="color:#ff82b2"><i>in un grande spazio di caratteristiche</i></span>, dove possono essere separate linearmente.
[image pag.28 of 3.5]
I separatori lineari qui corrispondono a un separatore non lineare in uno spazio originale.
$$
\textcolor{#ff82b2}{
	\begin{array}{c}
		\Phi:\mathbb{R^2}\to\mathbb{R}^3\\
		(x_1,x_2)^T\mapsto(x^2_1,\sqrt{2}x_1x_2,x_2^2)^T
	\end{array}
}
$$
Usando [[IntroAI/ML/ML_LinearModels#Generalizzazione - LBE|LBE]] $\phi(x)$ invece di $x$ per trattare con task non lineari riferito agli input. Comunque sappiamo che usando uno <span style="color:#ff82b2"><i>spazio delle caratteristiche di grandi dimensioni</i></span> può essere <span style="color:#ff82b2"><i>computazionalmente irrealizzabile</i></span> e più importante può essere <span style="color:#ff82b2"><i>facilmente portata a overfitting</i></span> senza controllarne le dimensioni dello spazio e la complessità del classificatore:
$$
\textcolor{#ff82b2}{
	h_w(x)=sign(\sum_kw_k\phi_k(x))
}
$$
Vedremo l'<span style="color:#ff82b2"><b>approccio kernel</b></span> per gestire (**implicitamente**) lo spazio delle caratteristiche nel <span style="color:#ff82b2"><i>contesto di modelizzazione regolarizzata</i></span> (dove la complessità dipende dai margini, non strettamente dalla dimensione di input): così, grazie alla regolarizzazione automatizzata dell'SVM, la <span style="color:#ff82b2"><i>complessità del classificatore può essere mantenuta piccola</i></span> nonostante la dimensione del nuovo <span style="color:#ff82b2"><i>spazio delle caratteristiche</i></span>.

### Use Φ(x) instead of x
In SVM <span style="color:#ff82b2"><i>non è necessario calcolare</i></span> $\textcolor{#ff82b2}{w}$ e <span style="color:#ff82b2"><i>il dato entra in forma di</i> <b>prodotti scalari</b> <i>di coppie di punti</i></span>.
$$
\textcolor{#ff82b2}{
\begin{array}{c}
	\begin{array}{ccccc}
		h_w(x)= sign(\sum_kw_k\phi_k(x))
		&&&&
		h(x)=sign(\sum_{p\in SV}\alpha_py_p \textcolor{#fc0202}{\boxed{\textcolor{#ff82b2}{x_p^Tx}}}+b)	
		\\
		\searrow
		&&&&
		\swarrow
	\end{array}
	\\
	h(x)=sign(\sum_{p\in SV}\alpha_py_p \textcolor{#fc0202}{\boxed{\textcolor{#ff82b2}{\phi^T(x_p)\phi(x))}}}
\end{array}
}
$$
e non è neanche necessario calcolare direttamente $\phi$
> [!quote] Definition
> 
> $$
> \textcolor{#ff82b2}{h(x)=sign(\sum_{p\in SV}\alpha_py_p \textcolor{#fc0202}{\boxed{\textcolor{#ff82b2}{K(x_p,x)}}})}
> $$
> $\stackrel{quindi}{\textcolor{#ff82b2}{\implies}}$ possiamo <span style="color:#ff82b2"><i>implicitamente gestire lo spazio delle ipotesi</i></span> con una <span style="color:#ff82b2"><b>funzione Kernel</b></span>

#### Kernel
> [!quote] Kernel
> 
> Un <span style="color:#ff82b2"><b>kernel</b></span> $k:\mathbb{R}^n\times\mathbb{R}^n\to\mathbb{R}$ è una funzione tale che qualche spazio di Hilbert $\textcolor{#ff82b2}{X}$ (possibilmente di grande dimensione) e una funzione $\textcolor{#ff82b2}{\phi:\mathbb{R}^n\to X}$ esistono con
> $$
> k(x_i, x_j)=\phi(x_i)^T\phi(x_j)
> $$
> $\stackrel{quindi}{\textcolor{#ff82b2}{\implies}}$ un kernel è potenzialmente una <span style="color:#ff82b2"><b>scorciatoia</b></span> per calcolare il prodotto scalare efficientemente anche in spazi di grandi dimensioni

Usiamo la <span style="color:#ff82b2"><i>funzione</i></span> $\textcolor{#ff82b2}{k}$ per calcolare direttamente il prodotto scalare nello <span style="color:#ff82b2"><b>spazio delle ipotesi</b></span>.
> [!example] Esempio
> 
> L'esempio di prima può essere efficientemente calcolato in $\mathbb{R}^2$ invece in $\mathbb{R}^3$:
> $$
> \begin{array}{lcr}
> 	\begin{array}{cc}
> 		\phi:\mathbb{R}^2\to\mathbb{R}3
> 		\\ \\
> 		(x_1, x_2)^T\mapsto(x_1^2,\sqrt{2}x_1x_2,x_2^2)^T
> 	\end{array}
> 	&&
> 	\begin{array}{c}
> 		\textcolor{#ff82b2}{\phi(x_i)^T\phi(x_j)=(x_i^T,x_j)}
> 		\\
> 		{\small\text{Computed in 2-dimension (not in 3)}}
> 	\end{array}
> \end{array}
> $$

##### Kernel popolari ben conosciuti
> [!quote] Kernel Lineare
> 
> $\textcolor{#ff82b2}{K(x_i,x_j)=x_i^Tx_j}$
> - Si mappa $\Phi:x\to\phi(x)$, dove $\phi(x)$ è $x$ stessa

> [!quote] Kernel Polinomiale
> 
> dell'ordine di $\textcolor{#ff82b2}{p:K(x_i,x_j)=(1+x_i^Tx_j)^k}$
> - Si mappa $\Phi:x\to\phi(x)$, dove $\phi(x)$ ha dimensione esponenziale in $k$

> [!quote] Kernel RBF (funzione radial-basis) Gaussiano
> 
> $\textcolor{#ff82b2}{K(x_i,x_j)=e^{-{\Large\frac{||x_i-x_j||^2}{2\sigma^2}}}}$
> - Si mappa $\Phi:x\to\phi(x)$, dove $\phi(x)$ è a **infinite dimensioni**

Il Kernel RBF è una scelta molto popolare: si noti che ha un <span style="color:#ff82b2"><b>hyperplane</b></span> ($\textcolor{#ff82b2}{\sigma}$) molto flessibile, può essere usato per fare confini decisionali intorno a ogni punto del training set.
$$
\sigma\text{ basso}\implies

\begin{array}{c}
	\text{Kernel a}\\
	\text{punta stretta}
\end{array}
\implies
\begin{array}{c}
\text{i pattern sono simili solo se molto vicini}\\
\text{e il classificatore risponde}\\
\text{con la classe del punto più vicino}
\end{array}
$$
Il design del nuovo kernel per tipi di dato speciali è tutt'ora un argomento di ricerca.

### SVM completata
Scegliamo il <span style="color:#ff82b2"><i>parametro di compromesso</i></span> $\textcolor{#ff82b2}{C}$, e la <span style="color:#ff82b2"><i>funzione kernel</i></span> $\textcolor{#ff82b2}{K}$ (e il suo hyper-parametro). Risolviamo poi l'ottimizzazione del problema per trovare $\textcolor{#ff82b2}{\alpha}$
- Il costo computazionale scala con $l$ invece che con $n$ (con il numero dei dati e non con la loro dimensione)
- Mudolarità: cambiamo solo il Kernel (con lo stesso risolutore)
Il <span style="color:#ff82b2"><b>modello finale</b></span> risulta:
$$
\textcolor{#ff82b2}{h(x)=sign(\sum_{p\in SV}a_py_pK(x_p, x))}
$$
## 2. Pratica - evitare misinterpretazioni
- Ci può essere <span style="color:#ff82b2"><b>overfitting</b></span> senza una cauta selezione degli <span style="color:#ff82b2"><i>hyperparametri</i></span> dell'SVM ($C, Kernel, Kernel\text{ }parameters$)
- Trattamento implicito degli <span style="color:#ff82b2"><b>spazi di grandi dimensioni</b></span> nello spazio delle caratteristiche e che non sono nello spazio di input. 
- La <span style="color:#ff82b2"><b>tecnica di validazione</b></span> vede molto lontano nella selezione del modello ($\text{\textcolor{#9172dd}{es.} C, Kernel, kernel hyperparametri}$) e la valutazione del modello può essere usata rigorosamente al meglio
#### Dalla guida LIBSVM
> Proponiamo che i principianti provino questa procedura per prima:
> - Trasformare formato del dato di un software SVM
> - Condurre un ridimensionamento semplice sul dato
> - Considerare il Kernel RBF $K(x,y)=e^{-\gamma||x-y||^2}$, dove $\gamma={\large\frac{1}{(2\sigma^2)}}$
> - Usare la valutazione incrociata per trovare il miglior parametro $C$ e $\gamma$
> - Usare il miglior parametro $C$ e $\gamma$ per allenare tutto il training set
> - Testare su un set separato ed esterno
#### Dettagli
- Processare i dati
	- $\{red, green, blue\}\to(0,0,1),(0,1,0),(1,0,0)$ $[1-of-k]$
	- Ridimensionamento dei valori lineare nel range $[-1,+1]or[0,1]$
	- $\textcolor{#9172dd}{es.}\text{ }{\large\frac{v-min}{max-min}}$ 
- Griglia di selezione per la selezione del modello ($\textcolor{#9172dd}{es.}\text{ }C$ e $\gamma$ in RBF)
	- Con una tabella sulle combinazioni di tutte le possibili crescite di valori (esponenziali) per trovare buoni intervalli
$$
\begin{array}{cc}
\textcolor{#9172dd}{es.}
&&&
C=2^{-5},2^{-3},...,2^{15}
&
\gamma=2^{-15}, 2^{-13},...,2^{3}
\end{array}
$$
	Quindi può essere eseguita una ricerca su griglia più precisa
## Conclusioni
L'SVM è uno strumento avanzato del ML molto utile e popolare. Le <span style="color:#ff82b2"><i>prestazioni</i></span> sono <span style="color:#ff82b2"><i>spesso buone, ma non sempre il confronto è favorevole</i></span>, riferito ad altri metodi di ML.
Combina l'uso efficiente dell'<span style="color:#ff82b2"><i>espansione di base lineare</i></span> col <span style="color:#ff82b2"><b>kernel</b></span> con l'<span style="color:#ff82b2"><b>approccio di margine massimo</b></span> per combinare modelli flessibili e controllo della complessità.
La modularità del kernel apre nuove possibilità, ammettiamo inoltre l'SVM al fuori della valutazione necessaria.