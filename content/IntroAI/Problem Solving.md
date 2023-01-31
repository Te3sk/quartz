---
title: Agenti risolutori di problemi
alias: problem solving
tags: #introai
---
# Agenti risolutori di problemi
Questi agenti adottano il paradigma della <span style="color:#ff82b2"><i>risoluzione dei problemi come</i> <b>ricerca</b> <i>in uno</i> <b>spazio di stati</b></span> (**Problem solving**). Sono [[IntroAI/Agents#Agenti basati su modello|Agenti con modello]] (storia delle percezioni e degli stati) che adottano una rappresentazione <span style="color:#ff82b2"><i>atomica dello stato</i></span>. Sono particolari [[IntroAI/Agents#Agenti con obiettivo|agenti con obiettivo]], che pianificano l'intera sequenza di mosse prima di agire.
## Il processo di risoluzione
1. Determinazione di un obiettivo (un insieme di stati in cui l'obiettivo è soddisfatto)
2. Formulazione del problema
	- rappresentazione degli stati
	- rappresentazione delle azioni
3. Determinazione della soluzione mediante <span style="color:#ff82b2"><i>ricerca</i></span>.
4. Esecuzione del piano
## Formulazione del problema
Un problema può essere definito formalmente mediante 5 componenti:
1. Stato iniziale
2. Azioni possibili in $s$: $Azioni(s)$
3. Modello di transizione:$$
	\begin{array}{l}
		Risultato: stato\times azione\to stato\\
		Risultato(s,a)=s'\text{ uno stato \textcolor{#ff82b2}{successore}}&&&&&&&&&&&&&&&&&&&&&&&
	\end{array}
	$$
4. Test obiettivo: un insieme di stati obiettivo, $Goal-test: stato\to\{true,false\}$
5. Costo del cammino: la somma dei costi delle azioni (costo dei passi), dove il costo di ogni passo è $c(s,a,s')$ ed è sempre $c(s,a,s')\geq 0$

Dall'1 al 3 viene definito implicitamente lo <span style="color:#ff82b2"><b>spazio degli stati</b></span>. Definirlo esplicitamente può essere molto oneroso, come in quasi tutti i problemi di AI. Questo perché l'agente non conosce già tutto in anticipo, per alcuni problemi servirebbe uno spazio enorme. Dobbiamo quindi trovare la soluzione ottima in uno spazio definito implicitamente.
## Algoritmi di ricerca
> [!quote] Ricerca
> 
> Il processo che cerca una sequenza di azioni che raggiunge l'obiettivo è detto <span style="color:#ff82b2"><b>ricerca</b></span>

Gli algoritmi di ricerca prendono in input un problema e restituiscono un <span style="color:#ff82b2"><b>cammino soluzione</b></span>, i.e. un cammino che porta dallo stato iniziale a uno stato goal.
<span style="color:#ff82b2"><b>Misura delle prestazioni:</b></span> $\text{costo totale} = \text{costo ricerca} + \text{costo del cammino soluzione}$
Noi valuteremo gli algoritmi sul costo della ricerca, poi ottimizzeremo sul secondo. Uno è il costo per pianificare e l'altro per fare il viaggio (dobbiamo ottimizare questo). Se il costo della ricerca è troppo più alto può anche essere impossibile trovare una soluzione.
# Esempi di problemi
Linkare da pag 8 a pag 17 delle [[IntroAI/assets/slides/3-2023-problem_solving.pdf|slide]]
