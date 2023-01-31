# Sistema di Tipi
```toc
```

##### 2022/10/04 #date 
## Linguaggi
I linguaggi si valutano su due livelli:
- **Sintassi(** frasi **):** comandi, istruzioni, espressioni, ... Sono le regole per frasi sintatticamente corrette
- **Semantica (** operazionale **):** computazione generata da una frase ben formata
La semantica introduce il **sistema di transizione**, nel paradigma funzionale, si usano solo espressioni *e*, quindi il sistema di transizione è logicamente fiù facile (dinamicamente $e\to e'$)
## Linguaggio d'esempio
Usiamo il linguaggio definito come
$$
\begin{array}{l}
	\xi \ni e := x|\underline{n}|\text{"}s\text{"}| e+e|e\cdot e|e^\frown e|let\:x=e\:in\:e \\
	P=\{e \in \xi | FV(e)=\emptyset\}
\end{array}
$$
	dove $e^\frown e$ indica la *concatenazione* e $P$ l'insieme dei programmi.
Abbiamo quindi che
$$
\begin{array}{lclr}
	e =_\alpha e' & (\lambda y.e')e \\
	let\:x = e\text{ in }e & \text{è } =_\alpha \text{a} &
		let\:y=e\text{ in } e[y/x] & \text{con } y\notin FV(e')\\
\end{array}
$$
*cose da riprendere*
## Tipi
> [!def] Sistema di Tipi
> Un sistema di tipi è un meccanismo che opera *...riprendere*

$$
\begin{array}{lr}
	T \ni \tau := num |str &
	\Large{\frac{e_1 \:\: num \:\:\:\:\:\:\:\:e_2 \:\: num}{e_1+e_2 \:\:num}}
\end{array}
$$
Il **Giudizio del tipo** è definito come $\Gamma \vdash e \:\:\tau$, quindi ad esempio: $x:str.y:str\vdash x^\frown y:str$. Abbiamo due tipi di regole:
### Regole di Introduzione
$$
\begin{array}{lcr}
	{\frac{}{\LARGE{\Gamma, x:\tau \:\vdash \: x:\tau}}} &
	\frac{}{\LARGE{\Gamma\:\vdash\:\underline{n}\:\:num}} &
	\frac{}{\LARGE{\Gamma\:\vdash\:\text{"s"}\:\:str}}	
\end{array}
$$
### Regole di eliminazione
$$
\begin{array}{lcr}
	\frac{
		\Large{\Gamma\:\vdash\:e_1 \:\:num \:\:\:\:\:\:\Gamma\:e_2 \:\:num}
	}{
		\Large{\Gamma\:\vdash\:e_1+2_2 \:\:\: num}
	} &
	\frac{
		\Large{\Gamma\:\vdash\:e_1 \:\: str \:\:\:\:\:\:\Gamma\:e_1 \:\:str}
	}{
		\Large{\Gamma\:\vdash\:e_1^\frown e_2 \:\:str}
	} &
	\frac{
		\Large{\Gamma\:x\,:\,\tau\:\vdash\:e_2 \:\:\sigma}
	}{
		\Large{\Gamma\:\vdash\:let\:x\,=\,e_1\text{ in }e_2 \:\:\sigma}
	}
\end{array}
$$
## In pratica
Per ogni forma sintattica delle espressioni si ha una regola di tipo
$$
\Gamma\:\vdash\:e\begin{cases}
	x \\
	\text{"s"} \\
	\underline{n}\\
	let \:\:x=e_1\text{ in }e_2 &&&&&&&&&&&&&&&&&&&&&&&&&&&&\\
	e+e \\
	e^\frown e
\end{cases}
$$
Ho quindi un algoritmo per applicare le regole, le quali sono **invertibili**, quindi da una conclusione posso sempre tornare alla premessa.
> [!the]
> Se $\Gamma\:\vdash\:e\,:\,\tau$, e se $e=e_1+e_2$, allora abbiamo la conclusione $\textcolor{#f77ead}{\cdots riprendere \cdots}$ univocamente le premesse 

##### 2022/10/07 #date 
Definiamo quindi un linguaggio
$$
\begin{array}{l}
	t,s=x|\underline{n}|\stackrel{\hbox{\small{\textcolor{#f77ead}{booleani}}}}{\underline{b}}|succ \:\:t|pred \:\:t|ite(t \:\:t \:\:t)|let \:\:x=t\text{ in }t\\
	v,w:=x|\underline{n}|\underline{b}
\end{array}
$$
e dato un alfabeto $\Sigma$, le parole che possiamo usare sono una parte di $\Sigma \rightsquigarrow\Sigma^*$.
Per essere un tipo, l'appartenenza di (un oggetto ad un insieme$\implies$)un programma a un tipo deve essere evidente -> **Statico**. In pratica prima si definisce la proprietà di interesse, poi il relativo sistema di tipi. Nel nostro caso usiamo una **semantica operazionale**: non essendoci possibilità di ricorsione, la computazione termina sempre, ma non è detto che termini come vogliamo. Questo dipende da se l'espressione è **ben scritta**. Vogliamo quindi che sia rispettata la seguente proprietà:
> [!def] Type Safety
> $$
> \begin{array}{c}
> 	t=v \\
> 	\Gamma\:\vdash\:t:T\implies\text{oppure}\implies\exists\,s.t \rightarrow s \wedge \Gamma\:\vdash\:s:T
> \end{array}$$

### Semantica operazionale
(Questo tipo di semantica è solo per l'esecuzione)
$$
\begin{array}{cc}
	&\text{Regola per l'assegnamento} \\
	\frac{}{
		\LARGE{let \:\:x\,=\,v \:\:s\to s[v/x]}
	} &
	\frac{
		\LARGE{t\to t'}
	}{
		\LARGE{let \:\:x=t \:\:in \:\:s\to let \:\:x=t' \:\:in \:\:s}
	}
\end{array}
$$
In generale quando si fa la semantica operazionale si danno *due tipologie di regole*: i linguaggi sono fatti di **valori** e di **costruttori**, quando si fa la semantica operazionale si fa il costruttore che prende un valore. Quindi 
$$
\frac{
	\large{t\to s}
}{
	\large{costruttore(t)\to costruttore(s)}
}
$$
formalmente l'operazione di riduzione $\to s \:\:T\times T$ va da termini a termini, quindi ha senso usarlo anche al di fuori del $\lambda$-calcolo.
> [!example] 
> - $ite(true, t, s)\to t$
> - $succ(\underline{n})\to\underline{n+1}$
> - $let \:\:x=v \:\:in \:\:s\to s[v/x]$
> - $\frac{\Large{t\to s}}{\Large{pred(t)\to pred(s)}}$
## Ambiente
L'**ambiente $\textcolor{#f77ead}{\boldsymbol{\Gamma}}$** è definito come $\Gamma\,:\,Var\to Type$ dove $dom(\Gamma)$ è finito. Definendo
- $\Gamma(x)=num$
- $\Delta(x)=bool$
- $x=num$
- $y=bool$
e scrivendo $\Gamma, \Delta$, se gli do una variabile presente nell'ambiente $\Gamma$, si comporta come definita in $\Gamma$, e lo stesso vale per $\Delta$. Per poter definire $\Gamma, \Delta$, i due ambienti devono essere disgiunti (se $x\notin FV(\Gamma)\implies x\in FV(\Delta)$ e viceversa). Considerando l'espressione $x:num\:\vdash\:succ(x):num$, anche aggiungednovi $\textcolor{#f77ead}{y:bool}\:\:x:num\:\vdash\:succ(x):num$, che fa parte dell'ambiente $\Delta$, non cambia proprio perché le variabili libere in $\Gamma$ sono legate in $\Delta$ e viceversa.
Avendo le funzioni
$$
\begin{array}{lr}
	f:A\to B \\
	& \text{con } A\bigcap A'=\emptyset &&&&&&&&&&&&&&&&&&&&&&&&&&&&\\
	g:A'\to B'
\end{array} 
$$
posso definire
$$
\begin{array}{c}
	f+g:A\bigcup A'\to B\bigcup B' \text{ dove }f+g=\begin{cases}
		f(x) & se \:\:x\in A \\
		g(x) & se \:\:x\in A'
	\end{cases}
\end{array} 
$$
Questo concetto si traduce in
$$
(\Gamma, \Delta) = \begin{cases}
\Gamma(x) & se \:\:x\in dom(\Gamma) \\
\Delta(x) & se \:\:x\in dom(\Delta)
\end{cases}
$$
Devo sempre poter definire $\Gamma \:\: x:T\:\vdash\:x:T$, quindi in ogni ambiente devo sempre poter dire che $\Gamma \:\: n:num\:\vdash\:n:num$ oppure $\Delta \:\:b:bool\:\vdash\:b:bool$.
### Regole del nostro sistema
#### Regola per *Ite*
$$
\frac{
	\large{\Gamma\:\vdash\:t:bool \:\:\:\:\:\:\Gamma\:\vdash\:s:T \:\:\:\: \Gamma\:\vdash\:u:\textcolor{#f77ead}{T}}
}{
	\large{\Gamma\:\vdash\:ite(t,s,u):\textcolor{#f77ead}{T}}
}
$$
$s$ e $u$ possono avere qualunque tipo, il tipo restituito da $ite$ dipende dall'esecuzione, quindi viola il principio $\implies$ Si deve chiedere che $s$ e $u$ abbiano lo stesso tipo.
Se un termine è tipato, allora l'esecuzione va a buon fine, ma non è detto il contrario. L'analisi del tipo è **corretta** ma **non completa** (come tutte le anallisi sintattiche).
#### Regola per il *let*
$$
\frac{
	\large{\Gamma\:t:T \:\:\:\:\:\: \Gamma\:x:T\:\vdash\:s:S}
}{
	\large{\Gamma\:\vdash\:let \:\:x=t \:\:in \:\:s:S}
}
$$
### Teorema dell'indebolimento
> [!the]
>$$
>\frac{
>	\large{\Gamma\:\vdash\:t.T}
>}{
>	\large{\Gamma \:\:x.s\:\vdash\:t:T}
>}
>$$
>Se $\Gamma\:\vdash\:t:T$ è derivabile, allora $\Gamma\:x.x:S\:\vdash\:t:T$ è derivabile

**Dimostrazione** per induzione su $\Gamma\:\vdash\:t:T$
- *Caso base:*
	$\Gamma\:\vdash\:t:T$ è un'istanza dell'assioma per i numerali, quindi $t=\underline{n}$ e $T=num\implies\Gamma\:\vdash\:n:\underline{num}$. Devo costruire un albero per $\Gamma x.s \:\vdash\:\underline{n}:num$
$$
\begin{array}{ll}
	\rightarrow\Gamma \:\vdash\: n:\underline{num} &
	\rightarrow \frac{
		\Large{\Gamma \:\vdash\: t:T}
	}{
		\Large{\Gamma \:\vdash\: succ():T}
	} \\
	\rightarrow\Gamma \:\vdash\:b:\underline{bool} &
	\rightarrow\frac{
		\Large{\Gamma \:\vdash\:t:bool \:\:\:\:\:\:\Gamma \:\vdash\:s_1:S \:\:\:\:\Gamma \:\vdash\:s_2:S}
	}{
		\Large{\Gamma \:\vdash\:ite(t,s_1,s_2):S}
	} \\
	\rightarrow\Gamma \:\:x:T \:\vdash\:x:T &
	\rightarrow \frac{
		\Large{\Gamma \:\vdash\:t:T \:\:\:\:\:\: \Gamma \:\: x:T \:\vdash\:s:S}
	}{
		\Large{\Gamma \:\vdash\:let\:\:x:t \:\:in \:\:s:S }
}&&&&&&&&&&&&&
\end{array}
$$
- *Caso induttivo:*
	Dai tre casi del caso base ricaviamo 3 casi possibili, essendo $\Gamma \:\vdash\: t:T$ un'istanza della regola per $succ$
$$
\begin{array}{cc}
	t = succ(s) & \\
	& \frac{
		\Large{\Gamma \:\vdash\:s:num}
	}{
		\Large{\Gamma \:\vdash\:succ(s):num}
	} \\
	T=\underline{num} &
\end{array}
$$
### Considerazioni sul design del sistema di tipi
Per determianre se $t$ ha tipo $T$, devo andare a guardare il tipo dei sotto termini, procedo analiticamente.
##### 2022/10/13 #date 
## Derivazione di tipi
$T,S:=Nat|Bool|T\to S$
$t,s:=x|n|b|ite(t,t,t)|t+t|let\:x=t\:in\:t|\lambda x.t|t\:t$
$v,w:=x|n|b|\lambda x.t$
**Regole di tipo:**
$$
\begin{array}{cr}
	\frac{
		\LARGE{\Gamma,x:s\:\vdash\:t:T} 
	}{
		\LARGE{\Gamma\:\vdash\:\lambda x:s.t.s\to T}
	} & (1) \\ \\
	\frac{
		\LARGE{\Gamma\:\vdash\:t:S\to T\:\:\:\:\Gamma\:\vdash\:s:S}
	}{
		\LARGE{\Gamma\:\vdash\:t\:s:T}
	} & (2)
\end{array}
$$
**Classico esercizio d'esame:** fare la derivazione di tipo di un dato termine 
-> $ite(true, \lambda x.x+s,\lambda y.y+2)$
$$
\frac{
	\frac{}{\Large{\emptyset\:\vdash\:true:Bool}},\:\:
	\frac{
		\frac{
			\LARGE{x:\textcolor{#f77ead}{Nat}\:\vdash\:x:Nat\:\:\:x:\textcolor{#f77ead}{Nat}\:\vdash\:s:Nat}
		}{
			\LARGE{x:\textcolor{#f77ead}{Nat}, \vdash\:x+s:\textcolor{#2e80f2}{Nat}}
		}
	}{
		\Large{\emptyset\:\lambda x.x+s:\textcolor{#2e80f2}{Nat}}
	},\:
	\frac{\text{suo albero di derivazione}}{
		\Large{\emptyset\:\vdash\:\lambda y.y+2:\textcolor{#f77ead}{Nat}}
	}
}{
	\vdash\:ite(true, \lambda x.x\:\vdash\:s,\lambda y.y+2):\textcolor{#f77ead}{Nat}\to \textcolor{#2e80f2}{Nat}
}
$$
-> $\Delta\triangleq \lambda x.xx \:\:\:\:\: \Omega=\Delta\Delta$
$$
\frac{
	\frac{
		\Large{x:T_1 \:\vdash\:x:U\to T_2 \:\:\:\:\:\:
		\frac{}{
			\LARGE{x:T_1 \:\vdash\:x:U}
		}}
	}{
		\Large{x:T_1 \:\vdash\:xx:T_2}
	}
}{
	\emptyset \:\vdash\: \lambda x.xx:T\to T_1
}
$$