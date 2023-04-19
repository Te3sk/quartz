---
title: Aritmetica di Macchina
alias:
- aritmeticadimacchina
- rappresentazione dei numeri
- aritmetica di macchina
- primo capitolo
tags: #cn23
---
# Virgola fissa
$$
\fbox{bit di segno | parte intera | parte frazionata}
$$
Un numero di $\textcolor{#ff82b2}{N}$ <span style="color:#ff82b2"><i>bit</i></span> è formato da 
- <span style="color:#ff82b2"><i>1 bit di segno</i></span>
- $\textcolor{#ff82b2}{I}$ <span style="color:#ff82b2"><i>bit di parte intera</i></span>
- $\textcolor{#ff82b2}{N-I-1}$ <span style="color:#ff82b2"><i>bit di parte frazionata</i></span>

> Quanti interi possiamo rappresentare data una base di rappresentazione $\beta$?

> [!summaries] Theorem
> 
> Dato $x\in\mathbb{R}$, $x\ne0$, scelta una base di rappresentazione $\beta$, allora esistono e sono unici:
> - un intero $p\in\mathbb{Z}$ detto <span style="color:#ff82b2"><b>esponente</b></span> della rappresentazione
> - una successione di naturali $\{d_i\}_{i\geq1}$ dette <span style="color:#ff82b2"><b>cifre</b></span> della rappresentazione
> 
> tali che: 
> 1. $d_i\ne0$
> 2. $0\leq d_i\leq\beta-1$
> 3. I $d_i$ non sono tutti uguali a $\beta-1$ per $i>M$
> 
> tali che: $$\large\textcolor{#ff82b2}{x=segno(x)\cdot\beta^p\cdot\sum_{i=1}^{\infty}(d_i\cdot\beta^{-i})}$$
> 
> La proprietà $1$ ci garantisce che <span style="color:#ff82b2"><i>i numeri sono normalizzati</i></span>, la $2$ e la $3$ <span style="color:#ff82b2"><i>garantiscono l'unicità della rappresentazione</i></span>
> 
> Inoltre chiamiamo la parte $\sum_{i=1}^{\infty}(d_i\cdot\beta^{-i})$ <span style="color:#ff82b2"><b>mantissa</b></span>.

> [!example]
> - $\beta=10\Rightarrow0,345=10^0\cdot(3\cdot10^{-1}+4\cdot10^{-1}+5\cdot10^{-1})$
> - $0,\overline{9}=1\Leftarrow0,\overline{9}=\sum_{i=0}^{\infty}10^{-i}=1$
# Floating Point
> [!quote] Insieme dei numeri Floating Point
> 
> $$\textcolor{#ff82b2}{\mathbb{F}(\beta, t, M, m)}$$
> É l'insieme dei numeri di macchina rappresentati in virgola mobile dove:
> - $\beta$ è la base di rappresentazione
> - $t$ è il numero di cifre della rappresentazione
> - $M$ e $m$ rappresentano il range dell'esponente
> 
> $$ \mathbb{F}(\beta, t, M, m)=\{0\}\cup\{x\in\mathbb{R}|x=segno(x)\cdot\beta^p\cdot\sum_{i=1}^{t}(d_i\beta^{-i}), 0\leq d_i\leq\beta-1, d\ne0, -m\leq p\leq M\}$$

Notiamo che rimangono le condizioni $1.$ e $2.$ del teorema precedente e che non serve ripetere la $3.$  dato che questa sommatoria non va a infinito.
Si aggiunge inoltre la condizione $m\leq p\leq M$ che definisce l'<span style="color:#ff82b2"><b>l'insieme dei numeri di macchina</b></span>.

L'insieme dei numeri di macchina è <span style="color:#ff82b2"><b>simmetrico</b></span>: se ha rappresentato un numero $x$, posso rappresentare anche $-x$.

Il <span style="color:#ff82b2"><b>minor numero rappresentabile</b></span> è quello con la minor mantissa ($(0.10...0)\forall\beta$) ed il minor esponente ($-m$) $$ \textcolor{#ff82b2}{w}=\beta^{\textcolor{#ff82b2}{-m}}\cdot (\textcolor{#ff82b2}{0.10...0})_\beta=\beta^{-m-1}$$
Il <span style="color:#ff82b2"><b>maggior numero rappresentabile</b></span> è quello con la mantissa maggiore (tutte le cifre $\beta-1$) e l'esponente maggiore ($M$):
$$
\begin{array}{rl}
	\textcolor{#ff82b2}{\Omega}&=\beta^\textcolor{#ff82b2}{M}(0.\textcolor{#ff82b2}{(\beta-1)\dots(\beta-1)})_\beta=\beta^M\cdot\sum_{i=1}^{t}(\beta-1)\cdot\beta^{-i} =\\
	& =\beta^M\cdot(\beta-1)\sum_{i=1}^t\beta^{-i}= \textcolor{#ff82b2}{\beta^M\cdot(1-\beta^{-t})}
\end{array}
$$
> [!example]
> 
> $$\begin{array}{rcll}\mathbb{F}(2,3,1,1) & \rightarrow & w = \beta^{-1-1}=\beta^{-2}=2^{-2}=\frac{1}{4}=2^{-1}(0.100) \\& \rightarrow & succ(w) = 2^{-1}(0.101)+\frac{1}{4}=\frac{5}{16}\\& \rightarrow & \begin{array}{ll}\Omega & = \beta^M(1-\beta^{-t})=2^1(1-2^{-3}) = \\ & = 2+2^{-2}= 2-\frac{1}{4}=\frac{7}{4}\end{array}\end{array} $$

### Quanti sono i numeri di macchina?
$$
\begin{array}{rl}
\#\mathbb{F}(\beta, t, m, M) &= 1 + 2\cdot(\text{possibili esponenti + possibili mantisse})\\
& = 1 + 2\cdot(m+M+1)\cdot((\beta-1)\cdot\beta^{t-1})
\end{array}
$$
- Il primo 1 lo aggiungiamo per considerare $\large\textcolor{#ff82b2}{\emptyset}$
- Moltiplichiamo la parentesi per 2 per considerare la <span style="color:#ff82b2"><i>simmetria</i></span>, quindi sia i positivi che i negativi
- Le possibili mantisse sono $\textcolor{#ff82b2}{(m+M+1)}$
- I possibili esponenti sono $\textcolor{#ff82b2}{(\beta-1)\cdot\beta^{t-1}}$
### Standard IEEE Double
Sui nostri PC si usa l'aritmetica di macchina di tipo <span style="color:#ff82b2"><b>Standard IEEE Double</b></span>: ogni numero è formato da $\textcolor{#ff82b2}{64\text{ bit}}$ di cui 1 di segno, 11 di esponente e 52 di mantissa.
L'insieme è quindi definito da $$\mathbb{F}(2, 53, 1021, 2024)$$
Ci sono dei casi speciali, se l'esponente è uguale a 1…1, si rappresentano dei numeri speciali:
- Se la <span style="color:#ff82b2"><i>mantissa è uguale a</i></span> $\textcolor{#ff82b2}{0}$, si rappresenta $\infty$, $\pm$ a seconda del segno
- Se la <span style="color:#ff82b2"><i>mantissa</i></span> $\textcolor{#ff82b2}{\ne 0}$, si rappresenta $\textcolor{#ff82b2}{NaN}$
## Limitazioni dell'aritmetica di macchinaj