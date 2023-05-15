---
title: Esercitazione 1 - Problema di Teseo
alias:
- Teseo
- es 1
- esercitazione prima parte
- labirinto di teseo
tags: 
- introai
- esercitazioni
- esercitazioniAI
---
# [Testo Esercitazione](https://drive.google.com/file/d/1Efw8LoOCuZ3bsa8pG9Hv6l0s75h9cyID/view?usp=sharing)
![Mappa labirinto](IntroAI/assets/pictures/Teseo-Labirinto.png)
# 1
## Formulazione PEAS
| Performance                                                | Environment     | Actuators                                                 | Sensors                                                |
| ---------------------------------------------------------- | --------------- | --------------------------------------------------------- | ------------------------------------------------------ |
| Trovare l'uscita (2,4) con il percorso più corto possibile | Labirinto, Muri | Spostamenti da una casella all'altra, percezione dei muri | Conoscenza della casella corrente, percezione dei muri |
## Ambiente
- Osservabile
- Deterministico
- Sequenziale
- Discreto
- MonoAgente
## Agente
- non reattivo
- con stato
- con obiettivo
## Formulazione del problema di ricerca
- Stati: $\textcolor{#ff82b2}{(x,y)}$
- Stato iniziale: $\textcolor{#ff82b2}{(2,1)}$
- Obiettivo: $\textcolor{#ff82b2}{(2,4)}$
- Azioni: $\textcolor{#ff82b2}{\uparrow\text{ }\downarrow\text{ }\rightarrow\text{ }\leftarrow}$ 
	- $Azioni((x,y))\subseteq\{\uparrow,\text{ }\downarrow,\text{ }\rightarrow,\text{ }\leftarrow\}$
- Costo Azione: $\textcolor{#ff82b2}{1}$
- Costo Cammino: <span style="color:#ff82b2"><i>Somma dei costi delle azioni</i></span>
# 2 - spazio degli stati

# 3 - Algoritmi
## BF a grafo
## DF ad albero
## DF ricorsiva ad albero
## DL
## Greedy best first a grafo
## A/ A* a grafo
## Ricerca locale Hill climbing