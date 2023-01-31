# λ-Calcolo
```toc
```
##### 2022/09/20 #date 
## Teoria astratta degli operatori
Prendiamo ad esempio l'espressione $x-y$, abbiamo 3 principali modi di interpretarla:
1. Una quantità di $x, y$
2. Una funzione $f(x)=x-y$
3. Una funzione $f(y)=x-y$
Il $\lambda$-calcolo **è sistematico**, quindi vuole eliminare l'ambiguità e definire a quale interpretazione si rifersice.
> [!def] Notazione di Church
> $\lambda x.t \Rightarrow$ rappresentazione di una funzione $t$ in $x$

Questa notazione torna comoda quando si usano operatori più generali. Supporta bene sia funzioni in input che funzioni in output
> [!example] 
>* $f(x)=x-y\Rightarrow \lambda x.x-y$ | $f(0)\Rightarrow (\lambda x.x-y)(0)$
>* Funzione che prende in input una funzione ($y$) e un argomento ($x$) e restituisce la funzione applicata due volte all'argomento: $\lambda y.\lambda x.y(y(x)) \Rightarrow \lambda z.z + z \rightarrow (\lambda y.\lambda x. y(y(z)))(\lambda z.z+z)$
>* Riprendendo il caso precedente, un'altra interpretazione è una funzione in 2 variabili
>$$h(x, y) =x-y \Rightarrow \lambda x.\lambda y.x-y \Rightarrow h(a,b)=a-b\Rightarrow$$
>$$\Rightarrow (\lambda x.\lambda y.x-y)(a)(b)=(\lambda y.a-y)(b)=a-b$$

> [!def] Curring
> Passaggio da $f$ a 2 variabili in una funzione a una variabile di ordine superiore
> $$\lambda x,y.t \Rightarrow \lambda x.\lambda y.t$$

Per **funzione di ordine superiore** si intende una funzione che può prendere in input e restituire in output altre funzioni.
## λ-termini
I $\lambda$-termini si basano su regole matematiche che si riflettono computazionalmente nei programmi. Possiamo quindi considerare i programmi come $\lambda$-termini e la sua esecuzione come una chiusura simmetrica di $\beta$-riduzione (una catena di $\beta$-riduzioni).
$$
\begin{array}{lr}
\text{programmi}=\lambda\text{-termini} &&&&&&
\text{esecuzione}= \rightarrow_\beta^*
\end{array}
$$
> [!def] 
> L'insieme $\Lambda$ dei $\lambda$-termine è definito dalla BNF come:
> $$s.t:=x|(\lambda x.t)|(ts)$$
Dove la notazione $ts$ corrisponde a $t(s)$
 
 Il $\lambda$-termine $s$, applicato a $t$, può essere:
 * una **variabile** ($x$)
 * una **$\lambda$-astrazione** ($(\lambda x.t)$)
 * un'**applicazione** ($(t.s)$)
È importante l'uso delle parentesi per indicare le giuste priorità. Due facili esempi di $\lambda$-termine sono:
* $(\lambda x.x)\Rightarrow$ **procedura identità**, restituisce l'input, non è possibile scriverla con la notazione matematica *classica*
* $(\lambda x.y)\Rightarrow$ **procedura costante**
	* $((\lambda y.y)(\lambda x.(x,y)))$
	* $(x(\lambda y(\lambda x.x)))$ in questo caso la prima $x$ non viene calcolata in $y$
	* $(x(\lambda y(\lambda z.z)))$ per evitare confusioni è meglio usare identificatori diversi
* $tsuv\rightarrow (ts)(uv) oppure (t(su)v)\Rightarrow$ le espressioni lineari sono alberi, le parentesi definiscono le priorità
> [!exc] 
> Determinare quale delle seguenti espression sono $\lambda$-termine
> $\lambda$, $(\lambda x)$, $x+y$, $\lambda (x,y).x$, $x$, $y$, $xx$, $\lambda x.(x(x(\lambda y)))$

> [!exc] 
> 1. Spiegare la differenza tra $(x(yz))$ e $((xy)z)$
> 2. Concludere che l'applicazione non è associativa
> 3. E la simmetria? $(ts)$ è uguale a $(st)$?

### Convenzioni Notazionali
##### Parentesi
Omettiamo parentesi e stipuliamo che l'applicazione si associ a sinistra
* $tus=((tu)s)$
* $(\lambda x.t)= \lambda x.t$
	 Quindi $\lambda x.ts=(\lambda x.(ts))$ ma $\lambda x.ts \neq (\lambda x.t)s$
##### Argomenti multipli
Scriviamo $\lambda x_1...x_n t$ in luogo di $\lambda x_1.\lambda x_2.\lambda x_3...\lambda x_n.t$ 
##### Uguaglianza
Scriviamo $t=s$ per dire che $t$ ed $s$ sono sintatticamente identici
> [!example] 
> $\lambda xyz.xz(zy)$ è un $\lambda$-termine, come anche $\lambda xyz.x(yz)$ e $\lambda xyz.xyz$. Gli ultimi due sono diversi perché, per convenzione, senza parentesi la priorità è in ordine
> $\lambda xyz.x(yz)\Rightarrow (A\rightarrow C)\rightarrow (B\rightarrow C) \rightarrow C$
> $\lambda xyz.xyz \Rightarrow B\rightarrow (A\rightarrow C) \rightarrow B \rightarrow A \rightarrow C$

### Semantica 
Se $t$ rappresenta una funzione e $s$ rappresenta un argomento, allora $(ts)$ rappresenta il **risultato** (se c'è) dell'applicare la funzione rappresentata da $t$ all'argomento rappresentato da $s$.
$(\lambda x.t)$ rappresenta la **funzione** il cui valore su un argomento rappresentato da un $\lambda$-termine $s$ è calcolato <u>sostituendo</u> $s$ ad $x$ in $t$.
## Sostituzione
**Obiettivo:** Definire una *procedura formale* per calcolare (le funzioni rappresentate da) $\lambda$-termini, in accordo con la semantica informale
**Idea:** Consideriamo il polinomio $p(x)=x^2+3x-6$
			in $\lambda$-rotazione: $p= \lambda x.x^2+3x-6$
		Per calcolare $P(5)$ scrivete $5^2+3\cdot 5-6$
Per calcolare $(\lambda x.t)s$, si sostituisce ogni occorrenza di $x$ in $t$ con $s$
$$(\lambda x.t)s \rightarrow _\beta t[s/x]$$
### Variabili libere e legate 
 $x$ può apparire molte volte in un termine, e con ruoli diversi
 $$(\lambda x.xy)x$$
In questo caso abbiamo le due $x$ dentro la parentesi come **variabili legate**, e quella fuori come **variabile libera**
> [!def] Variabili libere
> L'insieme $FV(t)$ delle variabili libere di $t$ è definito ricorsivamente da
> $FV(x)={x}$
> $FV(ts)=FV(t)\cup FV(s)$
> $FV(\lambda x.t)=FV(t)\setminus {x}$
> La $\lambda$ è un **binder**. Una variabile $x$ è libera in un termine $t$ se $x\in FV(t)$

Come nel caso precedente, una variabile può essere sia libera che legata. Inoltre, una variabile può essere legata da più $\lambda$, come nel caso $\lambda x.(yx(\lambda x.x))$, normalmente però, questa è equivalente a $\lambda x.(yx(\lambda z.z))$
> [!exc] 
> Calcolare $FV(xv(\lambda yz.yv)w)$

##### 2022/09/22 #date 
$$p(x) = x^2 + 3x -6 = (\lambda x.x^2+3x + 6)5 = 5^2 + 15 +6$$
Sostituiamo quindi tutte le occorrenze _libere_ di $x$ in $t$ con $s$, dove $s$ è nel caso sopra. La notazione che usiamo è $x[t/x]=s$.
>[!example] se $x \neq y$
>$x[5/x]=5$
>$y[5/x]=y$
>$(t  u)[3/x]=t[5/x]u[s/x]$
>$(\lambda x.t)[5/x]=\lambda x.t$
>$(\lambda y.t)[5/x]=\lambda y.t[5/x]$

Il **problema** è che per $\lambda x.t \approx \lambda y.t[y/x]$, quindi per risolverlo facciamo _renaming_ usando una variabile nuvoa:
> $(\lambda x.t)[s/x]=\lambda x.t$
> $(\lambda x.t)[s/x]=\lambda y.t[s/x]$            se $y\notin FV(s)$
> $(\lambda x.t)[5/x]=(\lambda z.t[z/y])[s/x]=$           con $z\notin FV(t)$ e $z \notin FV(s)$
> $=\lambda z.t[z/y][3/x]$

> [!example] 
> $(\lambda y.x)[y/x] = (\lambda z.y)[y/x] = \lambda z.y$
> in questo caso sostituiamo la x ( a den)

>[!exc] 
> *  $(\lambda y.x (\lambda w.vwx)) [uv/x]$
> * $\lambda y.x (\lambda x.x)$

##### 2022/09/29 #date 
## Programmare in λ-calcolo
### Codifica del λ-calcolo
Definiamo il linguaggio $\mathcal{F}$ come $$e:= x\,|\,n\,|\,e+e\,|\,e*e\,|\,ite(e,e,e)\,|\,x\implies e\,|\,e\;\;e\,|\,\textcolor{blue}{ricorsione}$$
Nella definizione del linguaggio tengo solo l'essenziale, successivamente definisco lo *zucchero sintattico* 
($\implies let\:\: x = e \text{ equivale a } (\lambda x.e')e$)
### Codifica dei Booleani
**Obiettivo:** $\begin{cases} \text{ite}(true, \triangle, \square) = \triangle \\ \text{ite}(false, \triangle, \square) = \square \end{cases} \implies \lceil ite\rceil\:\lceil true\rceil \:t\:s\: \rightarrow_\beta^*\:t$ 
- $\overline{true} = \lambda xy.x$
- $\overline{false} = \lambda xy.y$
- $\overline{ite}= \lambda b \lambda t \lambda s.bts$
	$\implies e:= \overline{true} \,|\, \overline{false} \,|\, ite(e,e,e)$
		$ite(true, e_1,e_2) \rightarrow e_1$
		$ite(false, e_1, e_2) \rightarrow e_2$
> [!example] 
>$$
>\begin{array}{l}
>	\overline{ite}\:\:\:\overline{true}\:\:t\:\:s \\
>	=(\lambda b.\lambda x.\lambda y.bxy)\:\:\overline{true}\:\:t\:\:s \\
>	\rightarrow_\beta (\lambda x.\lambda y.\:\overline{true}\:\:x\:\:y)\:t\:\:s \\
>	\rightarrow_\beta (\lambda y.\overline{true}\:\:t\:\:y)\:s \\
>	\rightarrow_\beta \overline{true} \:ts =\:(\lambda x\,y.x)\:t\:\:s \\
>	\rightarrow_\beta (\lambda y.t)s \rightarrow_\beta t 
>\end{array}
>$$

### Codifica dei numeri naturali
Consideriamo i numeri come **iterazioni** di operazioni.
- $\overline{0}= \lambda f.\lambda x.x$
- $\overline{1}= \lambda f.\lambda x.\lambda f$
- $\overline{2}= \lambda f.\lambda x.\lambda f(f_x)$
- ...
- $\overline{n}= \lambda f.\lambda x.f^n x$ <- **Numerale di Church**
### Successore
$\overline{\textcolor{#2e80f2}{succ}}\:\:\overline{n}\rightarrow_\beta^* \overline{n+1}\implies \lambda n.\lambda f.\lambda x.\lambda f(nfx) \rightarrow_\beta^*$
		$\rightarrow_\beta^* \lambda f.\lambda x.f(\overline{n}fx)\rightarrow_\beta^* \lambda f.\lambda x.f(f^n x) = \textcolor{#f77ead}{\overline{n+1}}$
### Addizione
	$$\textcolor{#2e80f2}{\overline{ADD}} = \lambda n.\lambda m.\lambda f.\lambda x\:\: n\:f(m\:f\:x)f^n(f^m x)f^{n+m}x$$
Vedremo che data una funzione $\mathcal{F}:\mathbb{N}^* \rightarrow\mathbb{N},\:\:\exists\;\overline{F}\in\Lambda$
									$\forall\:m,n_A\:\:\:F(n, n_k)=m \:\:\:\:\:\:\:\: \overline{F} n_1\:\:n_k \rightarrow_\beta^* \overline{m}$
> [!example] 
>$$
>\begin{array}{rcl}
>	\overline{succ}\:\overline{n} \rightarrow_\beta \overline{n+1} \\
>	(\lambda n.\lambda f.\lambda x.f(nfx))\overline{n} &
>	\rightarrow_\beta & 
>	\lambda f.\lambda x.f(\overline{n}fx) \\
>	& \rightarrow_\beta & 
>	\lambda f.\lambda x.f(f^n x)= \overline{n+1} \\
>	\\
>	\overline{add}\:\overline{n}\:\overline{m} \rightarrow_\beta^* \overline{n+m}
>\end{array}$$

##### 2022/09/30 #date 
### Ricorsione
$$
\begin{array}{l}
	F = \boxed{\:\:\:...F...\:\:\:} \\
	F = \lambda n.ite(Eq.n\:\:\:0, 1, MULT \:\:\:n\:\:\:(F(PRED(n))))
\end{array}
$$
Vogliamo trovare una funzione che, ad esempio per il *fattoriale*, faccia $t=_\beta \lambda n.ite(n==0, 1, MULT\:\:\:n\:\:\:(t(n-1))$. Cerchiamo quindi una $F:\Lambda \rightarrow \Lambda$ t.c.
$$
\begin{array}{rl}
	\text{\textcolor{#f77ead}{\textbf{Codifica Ricorsione:}}} &
	e ::= 0\,|\,succ(e)&&&&&&&&&&&&\\
	& \forall\,e.P(e) \Leftarrow P(z)\:\wedge\:\forall\:x.P(x)\rightarrow P(succ(x)) \\
	&\varphi(A)=\{Z\}\:\cup\:\{succ(a)\,|\,a\in A\}\\
	& \varphi(\emptyset) = \{Z\} \\
	& \varphi(\varphi(\emptyset)) = \{Z, succ(Z)\} \\
	&\varphi(\varphi(\varphi(\emptyset))) = \{Z, succ(Z), succ(succ(Z))\}\\
	& F: Exp \rightarrow \mathbb{N}\\
	&\varphi(A)=A \:\:\:\:\:\:\:\:\:\:\:\: \varphi(Exp)=Exp\\
	&\varphi(Exp\:\cup\:\{succ(succ...)\})\\
	&\varphi(B)=B\implies Exp\leq B
\end{array}
$$
$$
	\begin{array}{rcl}
		fix\:\:t & \rightarrow_\beta^* & (fix\:\:t) &&&&&&&&&&&&&&&&&&\\
		fix\:\:s & \rightarrow_\beta^* & (fix\:\:s)\\
		&\rightarrow_\beta^* & \lambda n.ite(n==0,1,MULT\:\:n(t(n-1)))
	\end{array}
	$$
$$
\textcolor{#2e80f2}{
	\begin{array}{l}
		(fix\:s)2 \rightarrow_\beta^* ite(2==0,1,2\cdot(fix\:s)(2-1)) \rightarrow_\beta^* \\
		\rightarrow_\beta^* 2\cdot((fix\:s)1) \rightarrow_\beta^*  \\
		\rightarrow_\beta^* 2\cdot(s(fix\:s)1) \rightarrow_\beta^* \\
		\rightarrow_\beta^* 2\cdot 1 \cdot((fix\:s)\:0\:) \rightarrow_\beta 2\cdot 1\cdot 1 \rightarrow_\beta 2
	\end{array}
}
$$

> [!def] Fattoriale
> $$
> \begin{array}{rcl}
> 	\textcolor{#f77ead}{\text{FACT}}\:\:n & \textcolor{#f77ead}{=} &
> 	ite (n == 0, 1, n\cdot (FACT(n-1))) \\
> 	\textcolor{#f77ead}{\text{FACT}} & \textcolor{#f77ead}{=} &
> 	\lambda n.e[\textcolor{#f77ead}{\text{FACT}}] \\
> 	&& \lambda f.\lambda n.e \\
> 	&& fix (\lambda f.\lambda n.e)
> \end{array}
> $$

> [!the]
> $$
> \begin{array}{l}
> 	\forall\:F\in\Lambda\:\:\:\exists\:t\in\Lambda\:.\:Ft=_\beta t \\
> 	\implies fix\:\:F \rightarrow_\beta^* F\:(fix\:F)\implies t=fix\:\:F
> \end{array}
> $$

## Combinatore di Curry
$$Y\triangleq \lambda f.(\lambda x.f(x\:x))(\lambda x.f(x\:x))$$ Se passo a $Y$ un $\lambda$-termine 
$$
\begin{array}{rcl}
\implies Y_F & \rightarrow_\beta & (\lambda x.F(x\:x)(\lambda x.F(x\:x)) \\
& \rightarrow_\beta & (F(\Delta_F\:\Delta_F)) \\
& =_\beta & F(YF)
\end{array}
$$
Questo termine va bene per dimostrare il problema, ma si rivela scomodo per eseguire effettivamente la ricorsione ($fix\:F \rightarrow F(fix\:F)$). Introducimo quindi un termine $\Theta$:
$$
\begin{array}{rrcl}
	&A & \triangleq & \lambda xy.y(xxy)AF) &&&&&&&&&&&&&&&&&&\\
	&\Theta & \triangleq & A\:A \\
	\implies & \Theta F & = & (\lambda xy.y(xxy)AF) \\
	&& \rightarrow_\beta & (\lambda y.y(\frac{A\,A}{\Theta} y))F \\
	&& \rightarrow_\beta & F(AAF) \rightarrow_\beta F(\Theta F) \textcolor{#f77ead}{\implies} \boldsymbol{fix \triangleq \Theta}
\end{array}
$$
> [!example] 
>$$
>\begin{array}{ll}
>	t,s::=n\,|\,b\,|\,ite\,|\,\lambda x.t\,|\,ts\,|\,\Theta\,|\,Y &&&&&&&&&&&&&&&&&&&\\
>	\rightarrow \text{scrivere } FACT\{Y\:\Theta\} & \implies exp\{Y\:\Theta\}\\
>	\rightarrow MOLT\{Y\:\Theta\} & \implies pred\{T\:\Theta\}	
>\end{array}
>$$
## Liste
$e:=\underline{nil}\,|\,e::e\,|\,...\textcolor{#2e80f2}{\rightarrow example \rightarrow}[1,2,3]\Rightarrow (1::(2::(3::\underline{nil})))$
**Codifiche:**
- $\underline{nil}\triangleq \lambda x.\lambda y.y$
- $\underline{cons}\triangleq \lambda h.\lambda t.\lambda x.\lambda y.ht$ (concatenazione)

> [!exc] Esercizi
>Dato $t,s:=\underline{n}\,|\,\underline{b}\,|\,\underline{ite}\,|\,\lambda x.t\,|\,ts\,|\,\Theta\,|\,Y$, scrivere e dimostrare:
>- $FACT$ (usando $Y$ e $\Theta$)
>- $MULT$ (usando $Y$ e $\Theta$)
>- $EXP$ (usando $Y$ e $\Theta$ e volendo con i numerali di Church)
>- $PRED$ (usando $Y$ e $\Theta$ assumendo che $PRED\:\:\underline{0}\rightarrow\underline{0}$)
>- $ADDLIST\:(n_1,...,n_k)=\sum_{i=1}^{k}n_i$
>- $PRODLIST\:(n_1,...,n_k)=\prod_{i=1}{k}n_i$
>- $MAP\:\lambda (...$
>		$MAP\:f(MAP\:gl)=_\beta MAP(f\,\circ\,g)l$
>- $f\,g=_\beta f\cdot g$

##### 2022/10/04 #date 
## Chiamata per Valore/Riferimento
- $MULT(0,x)\triangleq 0$    $MULT(m+!n)\triangleq ADD(n,MULT(m,n))$
	$$
	\begin{array}{c}
		\lambda f.\lambda m.\lambda n.ite(Eq\:\:n\:\:0,0,ADD\:\:n(f(PRED\:\:m)n)) \\
		\begin{array}{cc}
		Y(\lambda f.t)\:\:\:(\text{MULT}_Y)&\:\:\:\:\: \Theta(\lambda f.t)\:\:\:(\text{MULT}_\Theta)
		\end{array}
	\end{array}
	$$
> [!def] Lemma
> $$\forall\:t\:\:\:\:\:\:\Theta \rightarrow_\beta^* t(\Theta t)$$

> [!example] 
>$$
>\begin{array}{lll}
>MULT_\Theta \:\:\:22 & = \Theta(\lambda f.t)22 & 
>\rightarrow_\beta^*>t(MULT_\Theta)22 \\
>&&\rightarrow_\beta^* 
>ite(Eq\:\:2\:\:0,0,1\:\:ADD\:\:2(MULT_\Theta(PRED\:\:2)2)) \\
>&& \rightarrow_\beta^* ADD\:\:2(MULT_\Theta\:\:1\:\:2) \\
>&& \rightarrow_\beta^* ADD\:\:2(ADD\:\:2\:\:0) \rightarrow_\beta 4
>\end{array}
>$$

##### 2022/10/06 #date 
**Chiamate:** $(x^2+2z+3)(2+3)$ qui abbiamo 5 valori nella chiamata per valore (*CbN*)
1. $t\downarrow_{CbN}$   $t\uparrow^{CbV}$
2. *Teorema:* se $t \rightarrow_\beta^* s \not\to$ allora $t \xrightarrow{CbV}_\beta^* s$. Se vogliamo fare una chiamata per è necessario che $s$ non riduca a nulla.
3. La *Call by Value* è più efficiente
	1. La *Call by Need* è uguale alla *Call by Name* dal punto di vista di calcolo ($\xrightarrow{CbNeed}\:=\:\xrightarrow{CbN}$)
$$
\Large{\begin{array}{cc}
	\frac{t \:\:\xrightarrow{CbV}\:\:t'}{ts \:\:\xrightarrow{CbV}_\beta t's} &&&& 
	\frac{s \:\:\xrightarrow{CbV}_\beta\:\:s' \:\:\:\: t\not\to}{ts \:\:\xrightarrow{CbV}_\beta \:\:ts'}
\end{array}}
$$
> [!example] 
> $K(II)((\lambda x.xx)((\lambda y.yz)u)$
> Chiamiamo tutto $K(II)((\lambda x.xx)((\lambda y.yz)u)$ $M$, e $((\lambda x.xx)((\lambda y.yz)u)$ $N$. Dobbiamo ridurre prima il termine più a sinistra ($K(II)$), al suo interno vale lo stesso, siccome $k$ non riduce a nulla $\implies$
> $$
> \begin{array}{lr}
> 	\begin{array}{lcl}
> 		M & \xrightarrow{CbV}_\beta & (KI)N \\
> 		& \rightarrow_\beta & (\lambda y.I)N \\
> 		& \rightarrow_\beta & (\lambda y.I)((\lambda x.xx)(uz)) \\
> 		& \rightarrow_\beta & (\lambda y.I)((uz)(uz)) \\
> 		& \rightarrow_\beta & I
> 	\end{array}
> 	&\text{non si riduce a nulla,} \\ 
> 	&\text{quindi possiamo applicare} \\
> 	&\text{a seconda regola e ridurre} \\
> 	&\text{i termini a destra}&\\\
> \end{array}
> $$

##### 2022/10/11 #date 
## Linguaggio aritmetico nel λ-Calcolo
Il $\lambda$-calcolo non è tipato, quindi possiamo sempre applicare un termine ad un altro, anche se questo a volte non ha senso ($5 \:\: 5$, non ha senso applicare un numerale ad un altro). Possiamo allora quindi aggiungere un tipo con cui posso *riconoscere le funzioni*.
$$
\begin{array}{l}
	t,s:=n|x|b|ite(t\:t\:t)| \lambda x.t| \\
	T,S:=Nat|Bool| \textcolor{#f77ead}{T\to S}
\end{array}
$$
Dove:
- $n$ è un numerale
- $b$ è un booleano
- $Nat$ è il tipo dei numeri Naturali
- $Bool$ è il tipo dei booleani
> [!example] 
> - $Nat\to Bool$ non è una funzione di ordine superiore
> - $Nat\to(Nat\to Bool)$ è una funzione di ordine superiore
> - $(Nat\to Nat)\to(Nat\to(Nat\to Nat))$ è una funzione di ordine superiore

### Definizione Tipi
$$
\begin{array}{c}
	T \begin{cases}
		costruttori \rightsquigarrow \textcolor{#f77ead}{Introduzione} \\
		costruttori \rightsquigarrow \textcolor{#f77ead}{Eliminazione}
	\end{cases}\\
	&\\
	\begin{array}{lr}
		\frac{}{
			\LARGE{\Gamma\:\vdash\:s\::\:T\to S}
		} & 
		\frac{
			\LARGE{\Gamma \:\vdash\: s\::\:T}
		}{
			\LARGE{\Gamma \:\vdash\: ts\::\:s}
		} &
		\frac{}{
			\LARGE{\lambda\, x\,:\,T.\,t\to s}
		} &
		\frac{
			\LARGE{\Gamma,\,x\,:\,T \:\vdash\:t\,:\,s}
		}{
			\LARGE{\Gamma \:\vdash\: \lambda\,x.T\::\:T\to S}
}
	\end{array}
\end{array}
$$


