---
title: Algebra modulare
tags: #critto
---
##### 2022/10/25 #date 
Viene usata in molti algoritmi crittografici per 
- ridurre lo spazio dei numeri su cui si opera, e quindi aumentare la velocità di calcolo
- rendere difficili problemi computazionali che sono semplici nell'algebra non modulare
Dati due interi $a$ e $b\geq 0$ e un $n\geq 0$
$$
\begin{array}{c}
	a \equiv b\text{ mod }n \iff \exists\:k\in\mathbf{N}\:t.c.\:\:a=b+k\,n \\
	(a\text{ mod }n=b\text{ mod }n)
\end{array}
$$
Nelle relazioni di congruenza la notazione $\textcolor{#ff82b2}{mod\:\:n}$ si riferisce all'*intera relazione*, nelle relazioni di uguaglianza la stessa notazione si riferisce solo al membro dove appare.

Dato y $n\in \mathbf{N}$ intero positivo, $Z_n=\{0,1,2,...,n-1\}$ e $Z_n^*\subseteq Z_n$ è l'insieme degli elementi di $Z_n$ *co-primi* con $n$.
Se $n$ è primo, $Z_n^*=\{1,2,...,n-1\}$. Se non è primo, calcolare $Z_n^*$ è computazionalmente difficile, richiede tempo proporzionale al *valore* di $n$ (per confrontare $n$ con gli elementi di $Z_n$) quindi esponenziale nella lunghezza della sua rappresentazione.
## Proprietà
- $(a+b) \textcolor{#ff82b2}{\text{ mod }m}=(a \textcolor{#ff82b2}{\text{ mod }m} + b \textcolor{#ff82b2}{\text{ mod }m})\textcolor{#ff82b2}{\text{ mod }m}$
- $(a-b)\textcolor{#ff82b2}{\text{ mod }m}=(a\textcolor{#ff82b2}{\text{ mod }m}-b\textcolor{#ff82b2}{\text{ mod }m})\textcolor{#ff82b2}{\text{ mod }m}$
- $(a\times b)\textcolor{#ff82b2}{\text{ mod }m}=(a \textcolor{#ff82b2}{\text{ mod }m}\times b \textcolor{#ff82b2}{\text{ mod }m})\textcolor{#ff82b2}{\text{ mod }m}$
- $a^{r\times s}\textcolor{#ff82b2}{\text{ mod }m}=(a^r\textcolor{#ff82b2}{\text{ mod }m})^s\textcolor{#ff82b2}{\text{ mod }m}$ - con $r$ e $s$ interi positivi
> [!exp] 
> - $(12456\times 3678)\text{ mod }10=45813168\text{ mod }10=8$
> - $(12456\times 3678)\text{ mod }10=(6\times 8)\text{ mod }10=8$

## Funzione di Eulero
La funzione di eulero $\textcolor{#2e80f2}{\phi(n)}$ è definita come il numero di interi *minori di $\textcolor{#ff82b2}{n}$ e co-primi* con esso
$$\phi(n)=|Z_n^*|$$
se $n$ è **primo** -> $\phi(n)=n-1$
> [!the]
> $n$ composto $\Rightarrow \phi (n)=n(1-\frac{1}{p_1})...(1-\frac{1}{p_k})$
> $p_1,...p_k$ sono fattori primi di $n$, presi senza molteplicità

> [!the]
> $n$ prodotto di due numeri primi (semiprimi)
> $$n=p\:q\:\Rightarrow \phi (n)=(p-1)(q-1)$$

### Teorema di Eulero
> [!the] Teorema di Eulero
> Per $n>1$ e per ogni $a$ **primo** con $n$, $$a^{\phi(n)} \equiv 1\text{ mod }n$$

> [!the] Piccolo Teorema di Fermat
> Per $n$ primo e $\forall\: a\in Z_n^*$ $$a^{n-1}\equiv 1\text{ mod }n$$

## Calcolo dell'inverso del modulo
Per qualunque $a$ primo con $n$,
$$
\begin{array}{lcr}
	a\times a^{\phi (n)-1}\equiv1\text{ mod }n && \textcolor{#ff82b2}{\text{teorema di Eulero}} \\
	a\times a^{-1}\equiv 1\text{ mod }n && \textcolor{#ff82b2}{\text{definizione di inverso}}
\end{array}
$$
quindi
$$\textcolor{#2e80f2}{a^{-1}=a^{\phi(n)-1}\text{ mod }n}$$
L'inverso $a^{-1}$ di $a$ modulo $n$ si può dunque calcolare per esponenziazione di $a$, ma occorre conoscere $\phi(n)$, cioè fattorizzare $n$.
> [!dan]
> Esistenza e unicità dell'inverso sono garantiti se e solo se $\textcolor{#ff82b2}{a}$ e $\textcolor{#ff82b2}{n}$ sono co-primi -> $mcd(a,n)=1$

### Equazione ax = b mod n
> [!the]
> L'equazione $a\,x\equiv b\text{ mod }n$ ammette soluzione se e solo se $\textcolor{#ff82b2}{mcd(a,n)}$ *divide* $\textcolor{#ff82b2}{b}$.
> In questo caso si hanno esattamente $\textcolor{#ff82b2}{mcd(a,n)}$ soluzioni distinte

> [!the] Corollario
> L'equazione $a\:x\equiv b\text{ mod }n$ ammette **un'unica soluzione**
> $$\iff$$
> $a$ e $n$ sono co-primi, cioè $mcd(a,n)=1$
> $$\iff$$
> esiste l'inverso $a^{-1}$ di $a$

L'equazione ammette quindi *esattamente una soluzione(l'inverso di $\textcolor{#ff82b2}{a}$)* se e solo se $\textcolor{#ff82b2}{a}$ *e* $\textcolor{#ff82b2}{n}$ *sono primi tra loro*. L'inverso si può calcolare come
$$\textcolor{#2e80f2}{a^{-1}=a^{\Phi(n)-1}\text{ mod }n}$$
ma occorre conoscere $\Phi(n)$, cioè fattorizzare $n$ (*problema difficile*)
### Algoritmo di Euclide esteso
L'algoritmo di Euclide per il *calcolo del mcd* si può estendere per risolvere l'equzione in due incognite $\to a\:x+b\:y=mcd(a,b)$
```
FUNCTION EXTENDED_EUCLID (a,b)
	IF (b=0) THEN RETURN <a,1,0>
	ELSE
		<d', x', y'> = EXTENDED_EUCLID(b, a mod b);
		<d, x, y> = <d', y', x' - [a/b]y'>
		REUTURN <d,x,y>
 ```
 La funzione `EXENTED_EUCLID` restituisce una delle triple di valori `<mcd(a,b), x, y>` con `x`,`y` tali che $ax+by=mcd(a,b)$. Quindi $d=mcd(a,b)$
 La **complessità** è *logaritmica* nel valore di `a` e `b`, quindi *polinomiale* nella dimensione dell'input.
 ### Calcolo dell'inverso x=a^-1 mod b
 $ax\equiv 1\text{ mod }b\iff ax=bz+1$ per un opportuno valore di $z\iff \textcolor{#2e80f2}{ax+by=mcd(a,b)}$, dove $y=-z$ e $\textcolor{#2e80f2}{mcd(a,b)=1}$.
 $x$ è il valore dell'inverso, e si può calcolare eseguendo `EXTENDED_EUCLID(a,b)` e restituendo il *secondo valore della tripla*.
 > [!example] 
 > $x=5^{-1}\text{ mod }132\to 5x+132y=1$
 > $E\_E (5,132)\to<1,\textcolor{#ff82b2}{53},...>$
 > $E\_E (132, 5)\to <1,-2,1+2*26> = <1,-2,53>$
 > $E\_E (5,2)\to <1,1,0-1*2> = <1,1,-2>$
 > $E\_E (2,1)\to <1,0,1-0*2> = <1,0,1>$
 > $E\_E (1,0)\to <1,1,0>$
 
 ## [[Glossary#Generatori|Generatori]]
 
 > [!the] Teorema di Eulero
 > $1\in Z_n^*$ è **generatore** per $k=\Phi(n)$
 > Per ogni generatore, $\textcolor{#ff82b2}{a^k\equiv 1\text{ mod }n} \:\:\:\:\:\:1\leq k < \Phi(n)$
 
 Non per tutti i valori di $n$, $Z_n^*$ ha generatori, ad es. $Z_8^*$ non ammette generatori
 > [!the]
 > Se $\textcolor{#ff82b2}{n}$ è un numero *primo*, $\textcolor{#ff82b2}{Z_n^*}$, ha *almeno un generatore*
 
 Per $n$ primo, non tutti gli elementi di $Z_n^*$ sono suoi generatori (1 non è mai un generatore, e altri elementi possono non esserlo)$\implies$*Per* $\textcolor{#ff82b2}{n}$ *primo, i generatori di* $\textcolor{#ff82b2}{Z_n^*}$ *sono in totale* $\textcolor{#ff82b2}{\Phi(n-1)}$
### Problemi sui generatori rilevanti in crittografia
- **Determinare un generatore di** $\textcolor{#ff82b2}{Z_n^*}$**,** $\textcolor{#ff82b2}{n}$ **numero primo**:
Si possono provare per $a$ tutti gli interi in $[2,n-1]$ fino a trovare il generatore $\to$ *tempo esponenziale nella dimensione di* $\textcolor{#ff82b2}{n}$. Il problema si ritiene computazionalmente *difficile*. Risolto in pratica con gli algoritmi randomizzati (con alta probabilità di successo)
- **Calcolo del logaritmo discreto**
Risolvere nell'incognita $\textcolor{#ff82b2}{x}$ l'equazione $\textcolor{#2e80f2}{a^x\equiv b\text{ mod }n}$, con $n$ primo. L'equazione ammette una soluzione per ogni valore di $b$ se e solo se $\textcolor{#ff82b2}{a}$ *è un generatore di* $\textcolor{#ff82b2}{Z_n^*}$. Tuttavia:
	- non è noto a priori in che ordine sono generati gli elementi di $Z_n^*$
	- quindi non è noto per quale valore di $x$ si genera $b \text{ mod }n$
	- un esame diretto della successione richiede tempo esponenziale nella *dimensione* di $n$
	- non è noto un algoritmo polinomiale di soluzione
#### Logaritmo Discreto
Nell'aritmetica modulare le funzioni tendono a comportarsi in modo 'imprevedibile'. La funzione $\textcolor{#ff82b2}{2^x}$ nell'aritmetica ordinaria è una funzione monotona crescente:
| $\textcolor{#ff82b2}{x}$   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   | 10   | 11   | 12   |
| -------------------------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | ---- | ---- | ---- |
| $\textcolor{#ff82b2}{2^x}$ | 2   | 4   | 8   | 16  | 32  | 64  | 128 | 256 | 512 | 1024 | 2048 | 4096 |
|                            |     |     |     |     |     |     |     |     |     |      |      |      |
La funzione $\textcolor{#ff82b2}{2^x \text{ mod }13}$ diventa
| $\textcolor{#ff82b2}{x}$   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   | 10  | 11  | 12  |
| ------------------------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| $\textcolor{#ff82b2}{2^x}$ | 2   | 4   | 8   | 3   | 6   | 12  | 11  | 9   | 5   | 10  | 7   | 1   |

## Algebra modulare
| Algebra NON Modulare                                                                                                                      | Algebra Modulare                                                                                                                             |
| ----------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| $2^x=512, \:\:\: x=?$ Per errore supponiamo $x=8$, $2^8=256 \rightarrow$ è stato scelto un numero *troppo basso*, proviamo $x=9\to$**OK** | $2^x \text{ mod }13=5, x=?$ per errore, supponiamo $x=6$. $2^6 \text{ mod }  13 =12$. Occorre provare tutti i valori di $x$, la risposta è $9$ |

## Funzioni one-way trap-door
Esistono funzioni matematiche che sembrano possedere i requisiti richiesti (proprietà di teoria dei numeri e di algebra modulare). Il loro calcolo risulta incondizionatamente semplice, e la loro inversione è semplice se si dispone di un'informazione aggiuntiva sui dati (cioè di una *chiave privata*). Senza questa informazione, l'inversione richiede la soluzione di un problema *NP-Hard*, o comunque di un problema noto per cui non si conosce un algoritmo polinomiale.
### Fattorizzazione
Calcolare $n=p\times q$ è *facile*: richiede tempo quadraticco nella lunghezza della loro rappresentazione. 
Invertire la funzione, cioè *ricostruire* $\textcolor{#ff82b2}{p}$ *e* $\textcolor{#ff82b2}{q}$ *a partire da* $\textcolor{#ff82b2}{n}$ (univocamente possibile se e solo se $p$ e $q$ sono primi) *richiede tempo (sub)esponenziale*. Per quanto noto fino a oggi, anche se non vi è dimostrazione del fatto che il problema sia NP-Hard. L'esistenza di un algoritmo pollinomiale è assai improbabile, ma non assolutamente da escludere.
**Trap Door:** se si conosce uno dei due fattori (*chiave segreta*) ricostruire l'altro è *facile*.
### Calcolo della radice in modulo
Calcolare $\textcolor{#ff82b2}{y=x^z \text{ mod }s}$, con $x, z,s$ interi, richiede *tempo polinomiale*. Procedendo per successive esponenziazione, si eseguono $\Theta(log_2 z)$ moltiplicazioni.
*Se* $\textcolor{#ff82b2}{n}$ *non è primo*, invertire la funzione e calcolare $\textcolor{#2e80f2}{x=y{\frac{1}{2}} \text{ mod }s}$ richiede *tempo esponenziale* per quanto noto.
#### y=x^z mod s
Se $\textcolor{#ff82b2}{x}$ *è primo con* $\textcolor{#ff82b2}{s}$ e si conosce $\textcolor{#2e80f2}{v=z^{-1} \text{ mod } \Phi(s)}$ (chiave segreta), si determina facilente $x$ calcolando $$\textcolor{#ff82b2}{x=y^v \text{ mod }s}$$
Infatti
$$
\begin{array}{rl}
	\textcolor{#2e80f2}{y^v \text{ mod }s} & =x^{z\:v} \text{ mod }s\\
	&=x^{1+k\:\Phi} \text{ mod }s=x\:x^{k\:\Phi(s)} \text{ mod }s\\
	& \textcolor{#2e80f2}{=x \text{ mod }s} \:\:\:\:\:\:\:\:\:\text{(Teorema di Eulero)}
\end{array}
$$
### Calcolo del logaritmo discreto
Calcolare la potenza
$$ \textcolor{#2e80f2}{y=x^z \text{ mod }s}$$
è *facile*. Invertire rispetto a $z$, cioè trovare una $z$ tale che
$$\textcolor{#ff82b2}{y=x^z \text{ mod }s}$$
dati $x,y,s$ è computazionalmente *difficile*. Gli algoritmi noti hanno la stessa complessità della fattorizzazione, è possibile introdurre una *trap-door*.
## Esponenziazioni Successive
I passi per calcolare nel modo più efficiente
$$
x=y^z\text{ mod }s
$$
1. Si scompone l'esponente $\textcolor{#ff82b2}{z}$ in somme di potenze del due:$$
\textcolor{#ff82b2}{z = 2^{w_1} + 2^{w_2}+\cdots+2^{w_n}}
$$
2. Si calcolano le elevazioni a potenza con le scomposizioni, dove ognuna è il quadrato del precedente $$
\begin{array}{l}
	y^2 = y\cdot y\\
	y^4 = y^2\cdot y^2\\
	\vdots\\
	y^{w_i}= y^{w_{i-1}}\cdot y^{w_{i-1}}\\
	\vdots\\
	y^{w_n} = y^{w_{n-1}}\cdot y^{w_{n-1}}
\end{array}
$$
3. Il risultato è uguale al prodotto delle potenze calcolate $$
\textcolor{#ff82b2}{x = y^{w_1}\cdot y^{w_2}\cdot \dots \cdot y^{w_n}}
$$
In questo modo si eseguono al massimo $w-1$ moltiplicazioni.