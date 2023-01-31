---
title: Teoria della calcolabilità
tags: #critto
---
##### 2022/09/20 #date 
## Teoria della Calcolabilità
Si occupa delle questioni fondamentali circa la potenza e le limitazioni dei sistemi di calcolo, l'origine risale alla prima metà del XX secolo, quando i logici matematici iniziarono ad esplorare i concetti di <span style="color:#ff82b2"><i>computazione</i></span>, <span style="color:#ff82b2"><i>algoritmo</i></span> e <span style="color:#ff82b2"><i>problema risolvibile per via algoritmica</i></span>, e dimostrarono l'esistenza di problemi che non ammettono un algoritmo ($\implies$<span style="color:#ff82b2"><i>Problemi indecidibili</i></span>)
### Problemi Computazionali
Sono i problemi formulati matematicamente di cui cerchiamo una soluzione algoritmica, e sono classificati in
- <span style="color:#ff82b2"><b>Problemi decidibili:</b></span> richiedono una risposta binaria interpretata come 'decisione'
	- <span style="color:#ff82b2"><i>Problemi trattabili:</i></span> risolvibili con costo polinomiale
	- <span style="color:#ff82b2"><i>Problemi intrattabili:</i></span> risolvibili con costo esponenziale
- <span style="color:#ff82b2"><b>Problemi indecidibili:</b></span> problema la cui funzione non è calcolabile per l'assenza di un algoritmo
É stato dimostrato che le funzioni incalcolabili sono infinitamente meno numerose di quelle non calcolabili.
### Calcolabilità e complessità
> [!quote] Calcolabilità
> 
> Nozioni di <span style="color:#ff82b2"><i></i></span> e di *problemi non decidibili*.
> Ha lo scopo di classificare problemi in <span style="color:#ff82b2"><i>risolvibili</i></span> e <span style="color:#ff82b2"><i>non risolvibili</i></span>

> [!quote] Complessità
> 
> Nozione di <span style="color:#ff82b2"><i>algoritmo efficiente</i></span> e di <span style="color:#ff82b2"><i>problema intrattabile</i></span>
> Ha lo scopo di classificare problemi in <span style="color:#ff82b2"><i>facili</i></span> e <span style="color:#ff82b2"><i>difficili</i></span>

## Esistenza di problemi Indecidibili
### Insiemi Numerabili
Due insiemi $A$ e $B$ hanno lo stesso numero di elementi$\iff$si può stabilire una <span style="color:#ff82b2"><b>corrispondenza biunivoca</b></span> tra i loro elementi.
Un insieme è [[Glossary#Insiemi Numerabili|numerabile]] (possiede un'infinità numerabile di elementi)$\iff$i suoi elementi possono essere messi in corrispondenza biunivoca con i <span style="color:#ff82b2"><i>numeri naturali</i></span>

> [!example] Example 
> 
> - Insieme dei numeri naturali $\mathbb{N}$
> - Insieme dei numeri interi $\mathbb{Z}$
> $$
> \begin{array}{l}
> \begin{array}{lcl}
> 	n\leftrightarrow 2n+1 && n\geq 0&&&&&&&&&&&&&&&&&&&&&&&&&&&\\
> 	n\leftrightarrow 2|n| && n<0\\
> \end{array}&&&&\\
> 0, -1, 1,-2,2,-3,3,-4,4,...
> \end{array}
> $$
> - Insieme dei numeri naturali pari
> $$
> \begin{array}{lccl}
> 	2n\leftrightarrow n && 0,2,4,6,8,...&&&&&&&&&&&&&&&&&&&&&&&&&&&&
> \end{array}
> $$
> - Insieme delle stringhe finite di simboli di un alfabeto finito
### Enumerazione di sequenze
Si vogliono elencare in un ordine ragionevole le sequenze di lunghezza finita costruite su un alfabeto finito. Le sequenze non sono in ordine finito, quindi non si potrà completare la sequenza.
<span style="color:#ff82b2"><b>Obiettivo:</b></span> raggiungere qualsiasi sequenza $\sigma$ arbitrariamente scelta in un numero finito di passi. $\sigma$ deve dunque trovarsi a <span style="color:#ff82b2"><i>distanza finita</i></span> dall'inizio dell'elenco
#### Ordinamento canonico
Si stabilisce un ordine tra i caratteri dell'alfabeto, si ordinano le sequenze in ordine di lunghezza crescente e, a pari lunghezza, in ordine "alfabetico". Una sequenza $s$ arbitraria si troverà tra quelle di $|s|$ caratteri, in posizione alfabetica tra queste.
> [!example] Example
> 
> $$
> \begin{array}{l}
> 	\Gamma=\{a,b,c,...,z\}\\
> 	a,b,c,...,z\\
> 	aa, ab,...,az,ba,...,bz,...,za,...zz&&&&&&&&&&&&&&&&&&&&&&&&&&&&\\
> 	aaaa,...
> \end{array}
> $$ 

Ad una sequenza arbitraria corrisponde il numero che ne indica la posizione nell'elenco. A un numero naturale *n* corrisponde la sequenza che occupa la posizione *n* nell'elenco.
> [!tip] Observation
> 
> La numerazione delle sequenze è possibile perché esse sono di <span style="color:#ff82b2"><b>lunghezza finita</b></span> anche se illimitata, cioè per un qualunque intero $\textcolor{#ff82b2}{d}$ scelto a priori esistono sequenze di lunghezza maggiore di $\textcolor{#ff82b2}{d}$. Per sequenze di lunghezza infinita la numerazione non è possibile.

### Insiemi non numerabili
Sono tutti gli insiemi non equivalenti a $\mathbb{N}$.
> [!example] Example 
> 
> - insieme dei numeri reali
> - insieme dei numeri reali compresi nell'intervallo aperto $(0,1)$
> - inisieme dei numeri reali compresi nell'intervallo chiuso $[0,1]$
> - insieme di tutte le linee del piano
> - insieme delle funzioni in una o più variabili
> - ...

L'insieme dei <span style="color:#ff82b2"><b>problemi computazionali</b></span> NON è numerabile.
### Diagonalizzazione
$$F=\{funzioni f|f:\mathbb{N}\rightarrow\{0,1\}\}$$
Ogni funzione $f\in F$ può essere rappresentata da una sequenza infinita
$$
\begin{array}{lr}
	x & 0 & 1 & 2 & 3 & 4 & ... & n & ... \\ 
	f(x) & 0 & 1 & 2 & 3 & 4 & ... & n & ...
\end{array}
$$
o, se possibile, da una regola finita di costruzione
$$
f(x)=\begin{cases}
	0 & x & pari\\ 
	1 & x & dispari
\end{cases}
$$
> [!abstract] Theorem 
> 
> L'insieme $\textcolor{#ff82b2}{F}$ non è numerabile
> <span style="color:#ff82b2"><b>Dimostration:</b></span>
> Assumiamo per assurdo che $\textcolor{#ff82b2}{F}$ sia numerabile, possiamo quindi enumerare tutte le funzioni. Assegnamo ad ogni $f\in F$ un numero progressivo nella numerazione, e costruiamo una tabella (infinita) di tutte le funzioni
> Consideriamo la funzione $g\in F$ $$g(x)=\begin{cases}0&f_x(x)=1 \\ 1&f_x(x)=0\end{cases}$$
> $g$ non corrisponde ad alcuna delle $f_i$ della tabella: <span style="color:#ff82b2"><i>diferisce da tutte nei valori posti sulla diagonale principale</i></span>
> ![Tabella Diagonalizzazione](Crittografia/assets/diagonalizzazione.png)
> <span style="color:#ff82b2"><i>Per assurdo:</i></span> $\exists j \text{ t.c. }g(x)=f_j(x)$ allora $g(j)=f_j(j)$, ma per definizione $$g(j)=\begin{cases} 0&f_j(j)=1 \\ 1 & f_j(j)=0\end{cases}$$ cioè $g(j)\ne f_j(j)\implies$ <span style="color:#ff82b2"><b>Contraddizione</b></span>
> Per qualunque numerazione scelta, esiste sempre almeno una funzione esclusa $\implies$ $\textcolor{#ff82b2}{F}$ <span style="color:#ff82b2"><i>non numerabile</i></span>
> Si possono considerare linee arbitrarie che attraversano la tabella toccando tutte le righe e tutte le colonne esattamente na volta, e definire funzioni che cassumono in ogni punto unvalore opposto a quello incontrato sulla linea
> $\implies$ <span style="color:#ff82b2"><b>Esistono infinite funzioni di F escluse da qualsiasi numerazione</b></span>

Non essendo $F=\{f:\mathbb{N}\rightarrow \{0,1\}\}$ numerabile $\implies$ <span style="color:#ff82b2"><i>L'insieme dei problemi computazionali non è numerabile</i></span>
### Il problema della rappresentazione
L'informatica rappresenta tutte le sue entità (quindi anche gli algoritmi) in forma digitale, come <span style="color:#ff82b2"><i>sequenze finite di simboli di alfabeti finiti</i></span> (e.g. $\{0,1\}$).
### Il concetto di algoritmo
La formulazione di un [[Glossary#Algoritmo|algoritmo]] dipende dal modello di calcolo utilizzato
- programma per un modello matematico astratto, come una Macchina di Turing
- algoritmo per in pseudocodice per RAM
- programma in linguaggio C per un PC
Qualsiasi modello si scelga, gli algoritmi devono essere descrittivi, ossia rappresentati da <span style="color:#ff82b2"><i>sequenze finite di caratteri di un alfabeto</i></span>. Gli algoritmi sono possibilmente finiti, ma numerabili.
## Problemi indecidibili
Esistono dunque problemi non calcolabili, i problemi che si presentano spontaneamente sono tutti calcolabili, non è stato facile individuare un problema che non lo fosse, Touring nel 1930 elabora il <span style="color:#ff82b2"><b>problema dell'arresto</b></span>.
### Problema dell'arresto
È un problema posto in forma decisionale 
$$
Arresto:\text{\{}Istanze\text{\}}\rightarrow \text{\{}0,1\text{\}}
$$
Per i problemi decisionali, la calcolabilità è in genere chiamata <span style="color:#ff82b2"><b>decidibilità</b></span>

>Presi ad arbitrio un *algoritmo **A*** e i suoi dati in input **D**, decidere **in tempo finito** se la computazione di **A** su **D** termina o no

Si tratta quindi di un algoritmo che indiaga sulle proprietà di un altro algoritmo trattato come dato in input. Possiamo usare lo stesso alfabeto per codificare algoritmi e i loro dati di ingresso (sequenze di simboli dell'alfabeto). Una stessa sequenza può quindi essere interpretata sia come un programma che come un dato di ingresso di un altro programma.
Un algoritmo $\textcolor{#ff82b2}{A}$, comunque formulato, può operare sulla rappresentazione di un altro algoritmo $\textcolor{#ff82b2}{B}$, possiamo calcolare $\textcolor{#ff82b2}{A(B)}$, in particolare può avere senso calcolare $\textcolor{#ff82b2}{A(A)}$.
> [!danger] <b>Exercice</b>: Problema dell'arresto
> 
> Consiste nel chiedersi se un generico programma termina la sua esecuzione, oppure "va in loop", ovvero continua a ripetere la stessa sequenza di istruzioni all'infinito (supponendo di non avere limiti di tempo e memoria)

Se ad esempio, dobbiamo stabilire se un intero `p>1` è primo
```
function primo (p)
	fattore = 2;
	while (p % fattore != 0)
		fattore++;
	return (fattore == p)
```
termina sicuramente (la guardia del `while` diventa falsa quando `fattore=p`).

Considerando invece il seguente codice
```
function Goldbach()
	n = 2;
	do {
		n = n + 2;
		controesempio = true;
		for (p = 2; p <= n - 2; p++) {
			q = n - p;
			if (primo(p) && primo(q))
				controesempio = false;
		}
	} while (!controesempio);
	return n;
```
L'algoritmo <span style="color:#ff82b2"><b>Goldbach</b></span> scandisce in ordine crescente i numeri naturali pari maggiori di 2, fino a trovare un numero che <span style="color:#ff82b2"><i>NON</i></span> sia eesprimibile come la <span style="color:#ff82b2"><i>somma di due numeri primi</i></span>. Se e quando accade, stampa il numero e termina.
> [!quote] Congettura di Goldbach (XVIII secolo)
> 
> "ogni numero intero pari $\textcolor{#ff82b2}{n\geq 4}$ è la somma di due numeri primi"
> - Congettura <span style="color:#ff82b2"><b>falsa</b></span> $\rightarrow$ `Goldbach()` si arresta
> - Congettura <span style="color:#ff82b2"><b>vera</b></span> $\rightarrow$ `Goldbach()` <span style="color:#ff82b2"><i>NON</i></span> si arresta

Turing ha dimostrato che riuscire a dimostrare se un programma arbitrario si arresta e termina la sua esecuzione non è solo un'impresa ardua, ma in generale è <span style="color:#ff82b2"><b>impossibile</b></span>.
> [!abstract] Theorem
> 
> Il problema dell'arresto è <span style="color:#ff82b2"><b>indecidibile</b></span>
> <span style="color:#ff82b2"><b>Dimostration:</b></span>
> Se il problema dell'arresto fosse decidibile, allora esisterebbe un algoritmo `arresto` che, presi $\textcolor{#ff82b2}{A}$ e $\textcolor{#ff82b2}{D}$ come dati di input, determina in <span style="color:#ff82b2"><b>tempo finito</b></span> le risposte

> [!tip] Observation
> 
> L'algoritmo `arresto` non può consentire in un algoritmo che simuli la computazione $\textcolor{#ff82b2}{A(D)}$ se $\textcolor{#ff82b2}{A}$ non si arresta su $\textcolor{#ff82b2}{D}$, `arresto` non sarebbe in grado di rispondere <span style="color:#ff82b2"><i>NO</i></span> in tempo finito.
> Non può esistere un algoritmo che decida in *tempo finito* se un algoritmo arbitrario termina la sua computazione su dati arbitrari, ciò non significa che non si possa decidere in tempo finito la terminazione di algoritmi particolari, il problema è indecidibile su una coppia $\textcolor{#ff82b2}{\langle A,D\rangle}$ scelta arbitrariamente.
> L'algoritmo `arresto` costituirebbe uno strumento estremamente potente, permetterebbe infatti di dimostrare congetture anccora aperte sugli interi.

## Problemi indecidibili
Altri problemi lo sono, ad esempio è indecidibile stabilire l'equivalenza tra due programmi (se per ogni possibile input, producono lo stesso output)
#### Lezione di Turing:
> Non esistono algoritmi che decidono il comportamento di altri algoritmi esaminandoli dall'esterno, cioè senza passare dalla loro simulazione.

## Modelli di calcolo e calcolabilità
### La tesi di Church-Turing
Tutti i modelli di calcolo risolvono esattamente la stessa classe di problemi, dunque <span style="color:#ff82b2"><b>si equivalgono</b></span> nella possibilità di risolvere problemi, pur operando con diversa efficienza.
><span style="color:#ff82b2"><b>La decidibilità è una prorpietà del problema</b></span>

Incrementi qualitativi alla struttura di una macchina o alle istruzioni di un linguaggio di programmazione servono <span style="color:#ff82b2"><i>solo</i></span> ad abbassare il tempo di esecuzione e a rendere più agevole la programmazione.
