---
title: Rappresentazione matematica di Oggetti
tags: #critto
---
## Alfabeti e sequenze
Dato un [[Glossary#Alfabeto|Alfabeto]] $\Gamma, |\Gamma|=s$ e $\textcolor{#ff82b2}{N}$ <span style="color:#ff82b2"><i>oggetti</i></span> da rappresentare
- $\textcolor{#ff82b2}{d(s,N)}$ è la <span style="color:#ff82b2"><i>lunghezza della sequenza più lunga</i></span>
- $\textcolor{#ff82b2}{d_{min}(s,N)}$ è il valore <span style="color:#ff82b2"><i>minimo di </i></span> $\textcolor{#ff82b2}{d(s,N)}$ <span style="color:#ff82b2"><i>tra tutte le sequenze possibili</i></span>, ovvero il nome che usa meno caratteri (la più efficiente)
Un metodo di rappresentazione è tanto migliore, tanto più $d(s,N)$ si avvicina a $d_{min}(s,N)$.
### Alfabeto unario
$$s=1, \Gamma=\{0\}$$
Le sequenze di rappresentazioni possono essere solo ripetizioni dello $0$,per cambiare nome posso solo cambiare caratteri.
	$\implies d_{min}(1,N) = N\implies$ L'$N$-esimo oggetto ha nomi di $N$ caratteri -> in base 10 un milione non ha $10^6$ caratteri ma ne ha 9
È una rappresentazione estremamente sfavorevole.
### Alfabeto binario
$$s=2, \Gamma=\{0, 1\}$$
Per ogni $k>1$, ci sono $2^k$ sequenze di lunghezza $k$. Il numero totale di sequenze lunghe da $1$ a $k$ è dato da -> $\sum_{i=1}^{k}2^i=2^{k+1}-2$. Se ho $N$ oggetti da rappresentare:
- $2^{k+1}-2\leq N$
- $k\geq log_{2}(N+2)-1$ 
- Il nome più lungo che mi aspetto è di lunghezza logaritmica rispetto al totale di oggetti da rappresentare
$d_{min}(2,N)$ è il minimo intero $k$ che soddisfa tali relazioni:
- $d_{min}(N,s) = \lceil log_{2}(N+2)-1\rceil$ 
- $\lceil log_2N\rceil -1 \leq d_{min}(2,N)\:\leq\:\lceil log_2N\rceil$ 
### Rappresentazioni s-arie
Si possono costruire $N$ sequenze differenti <span style="color:#ff82b2"><i>con</i></span> $\textcolor{#ff82b2}{\lceil log_sN\rceil}$ <span style="color:#ff82b2"><i>caratteri</i></span> e si possono costruire $N$ sequenze differenti <span style="color:#ff82b2"><i>tutte di</i></span> $\textcolor{#ff82b2}{\lceil log_sN\rceil}$ <span style="color:#ff82b2"><i>caratteri</i></span>.
> [!exp] 
> $\Gamma=\{0,1,2,...,9\}$
> con $\lceil log_{10}1000\rceil=3$ caratteri si rappresentano $1000$ sequenze diverse (da $000$ a $999$)

## Rappresentazioni
Usare sequenze della stessa lunghezza è vantaggioso perché per concatenare sequenze di lunghezza diverse è necessario inserire una marca di separazione tra l'una e l'altra, ma questi sono <span style="color:#ff82b2"><b>accorgimenti non necessari</b></span> se la lunghezza delle sequenze è unica e nota.
### Rappresentazioni efficienti
Sono le rappresentazioni che usano un numero massimo di caratteri di <span style="color:#ff82b2"><b>ordine logaritmico</b></span> rispetto alla cardinalità di $\textcolor{#ff82b2}{N}$ (dove N è il numero di caratteri da rappresentare), l'alfabeto deve contenere almeno 2 caratteri.
### Rappresentazione di interi
La notazione posizionale per rappresentare i numeri interi è una rappresentazione efficiente, indipendentemente dalla base $s\geq 2$ scelta. Un intero $N$ è rappresentato con un numero $d$ di cifre tale che $$\lceil log_sN\rceil\:\:\:\leq\:\:d\:\:\leq\:\:\:\lceil log_2N\rceil+1$$

