---
title: Cifrari Storici
tags: #critto
---
##### 2022/10/10 #date 
Lo <span style="color:#ff82b2"><b>scopo</b></span> dei cifrari storici era di consentire comunicazioni <span style="color:#ff82b2"><i>sicure</i></span> tra poche persone, sono tutti stati forzati. La <span style="color:#ff82b2"><b>cifratura</b></span> e la <span style="color:#ff82b2"><b>decifrazione</b></span> erano realizzate con carta e penna ed i <span style="color:#ff82b2"><b>messaggi da cifrare</b></span> erano frasi di senso compiuto in linguaggio naturale. L'<span style="color:#ff82b2"><b>Alfabeto</b></span> era formato dalle 26 lettere $A,B,C,\cdots,X,Y,Z$.
## Principi di Bacone
(XIII secolo)
Le funzioni $C$ e $D$ devono essere <span style="color:#ff82b2"><i>facili da calcolare</i></span>, è impossibile ricavare $D$ se $C$ non è nota ed il crittogramma $c=C(m)$ deve apparire <span style="color:#ff82b2"><i>innocente</i></span>.
## Cifrario di Cesare
È il più antico cifrario di concezione moderna. L'<span style="color:#ff82b2"><b>idea</b></span> di base è che il <span style="color:#ff82b2"><i>crittogramma</i></span> $\textcolor{#f77ead}{c}$ è ottenuto dal <span style="color:#ff82b2"><i>messaggio in chiaro</i></span> $\textcolor{#f77ead}{m}$ sostituendo ogni lettera di $\textcolor{#f77ead}{m}$ con quella <span style="color:#ff82b2"><i>tre posizioni più avanti nell'alfabeto</i></span>
![[Crittografia/assets/CdiCesare.png]]
Non ha una chiave segreta, la segretezza dipendeva dalla conoscenza del metodo. Scoprire il metodo di cifratura significa comprometterne irrimediabilmente l'impiego. Il cifrario era destinato <span style="color:#ff82b2"><b>all'uso ristretto</b></span> di un gruppo di conoscenti.
### Generalizzato
Può essere trasformato aumentandone la sicurezza:
* invece di rotare l'alfabeto di 3 posizioni, possiamo rotarlo di una quantità arbitraria $\textcolor{#f77ead}{k}$, $\textcolor{#f77ead}{1\leq k \leq 25}$ (26 lascia inalterato il messaggio)
* in questo caso $\textcolor{#f77ead}{k}$ è la <span style="color:#ff82b2"><i>chiave segreta</i></span> del cifrario
### Formulazione matematica
$\textcolor{#f77ead}{pos(X)}$: posizione nell'alfabeto della lettera $\textcolor{#f77ead}{X}$, chiave $\textcolor{#f77ead}{k}$, $1\leq k \leq 25$.

<span style="color:#ff82b2"><b>Cifratura di</b></span> $\textcolor{#f77ead}{\boldsymbol{X}}$<span style="color:#ff82b2"><b>:</b></span> lettera $Y$ t.c. $\textcolor{#f77ead}{pos(Y)=(pos(X)+k)\text{ mod }26}$

<span style="color:#ff82b2"><b>Decifratura di</b></span> $\textcolor{#f77ead}{\boldsymbol{Y}}$<span style="color:#ff82b2"><b>:</b></span> lettera $X$ t.c. $\textcolor{#f77ead}{pos(X)=(pos((Y)-k)\text{ mod }26)}$
> [!example] Example 
> 
>$k=10$, cifratura di $R$, $pos(R)=17$
>
>$(17+10)\:mod\:26=1=pos(B)$           $R\to B$
### Realizzazione fisica
Due dischi concentrici:
- <span style="color:#ff82b2"><i>disco interno:</i></span> lettere dell'alfabeto in chiaro
- <span style="color:#ff82b2"><i>disco esterno:</i></span> lettere cifrate
### Crittoanalisi
Se si conosce la struttura del cifrario, si possono applicare in breve tempo tutte le chiavi possibili ($25$) a un crittogramma per decifrarlo e scoprire contemporaneamente la chiave segreta. Questo cifrario è <span style="color:#ff82b2"><i>inutilizzabile</i></span> ai fini crittografici.
### Osservazione
Gode della <span style="color:#ff82b2"><b>proprietà commutativa</b></span>: data una sequenza di chiavi e di operazione di cifratura e decifratura, l'ordine delle operazione può essere permutato arbitrariamente senza modificarne il crittogramma finale.
Date due chaivi, $k_1$ e $k_2$, e una sequenza $s$
$$
\begin{array}{l}
	C(C(s,k_2), k_1)=C(s,k_1+k_2)\\
	D(D(s,k_2),k_1)=D(s,k_1+k_2)
\end{array}
$$
Una sequenza di operazioni di cifratura e decifrazione può essere ridotta ad una sola operazione di cifratura o decifrazione. <span style="color:#ff82b2"><b>Comporre più cifrari non aumenta la sicurezza del sistema</b></span>.
## Classificazioni dei cifrari storici
- **[[Crittografia/Cifrari a Sostituzione]]:** sostituisco ogni lettera del testo in chiaro con una o più lettere dell'alfabeto secondo una regola prefissata
	- [[Crittografia/Cifrari a Sostituzione#Cifrari Monoalfabetici|Cifrari Monoalfabetici]]: Alla stessa lettera del messaggio corrisponde una stessa lettera nel crittogramma (e.g. cifrario di cesare)
	- [[Crittografia/Cifrari a Sostituzione#Cifrari Polialfabetici|Cifrari Polialfabetici]]: Alla stessa lettera del messaggio corrisponde una lettera scelta in un insieme di lettere possibili, secondo una regola opportuna, a seconda della posizione o del contesto in cui appare la lettera nel messaggio
- **[[Crittografia/Cifrari a Trasposizione]]:** permutano le lettere del testo in chiaro secondo una regola prefissata
