---
titolo: Esempio del viaggio in Romania
alias:
- romania
- esempio romania
- Esempio del viaggio in romania
- esempio problema
- viaggio
tags: #introai
---
![Esempio romania img 1](IntroAI/assets/romEx/romEx1.png)
# Formulazione
<span style="color:#ff82b2"><b>Stati:</b></span> le città
- <span style="color:#ff82b2"><i>Stato iniziale:</i></span> la città da cui si parte - $In(Arad)$
- <span style="color:#ff82b2"><i>Azioni:</i></span> spostarsi da su una città vicina collegata - $Azioni(In(Arad)) = \{Go(Sibiu), Go(Zerind) ...\}$
- <span style="color:#ff82b2"><i>Modello di transizione:</i></span> $Risultato(In(Arad), Go(Sibiu))=In(Sibiu)$
- <span style="color:#ff82b2"><i>Test Obiettivo:</i></span> $\{In(Bucarest)\}$
- <span style="color:#ff82b2"><i>Costo del cammmino:</i></span> somma delle lunghezze delle strade
Lo <span style="color:#ff82b2"><b>Spazio degli Stati</b></span> coincide con la rete (grafo) di collegamenti tra le città, cioè il grafo di stati collegati da azioni, rappresentabile in modo esplicito in questo caso semplice, tramite la mappa.
L'<span style="color:#ff82b2"><b>Astrazione dai dettagli</b></span> è essenziale per "<i>modellare</i>".
# Ricerca della soluzione
![Illustrazione ricerca della soluzione](IntroAI/assets/romEx/romEx2-ricercaSoluzione.png)
Si parte dallo stato iniziale ($Arad$) e si espande:
![espansione di uno stato](IntroAI/assets/romEx/romEx3-espansione.png)
# Ricerca sul grafo
![Ricerca sul grafo nell'esempio del viaggio in Romania](IntroAI/assets/romEx/romEx4-ricercaSulGrafo.png)
La ricerca sul grafo esplora uno stato al più una volta.
La <span style="color:#ff82b2"><b>frontiera</b> separa i nodi <b>esplorati</b> da quelli <b>inesplorati</b></span>, ogni cammino dallo stato iniziale a inesplorati deve attraversare uno stato della frontiera.
# Algoritmo Best-First
![Algoritmo Best-first nell'esempio dell'ititnerario in Romania](IntroAI/assets/romEx/romEx5-bestFirst.png)
$$
\textcolor{#ff82b2}{f=h}
$$
Per andare da Arad a Bucarest, l'algoritmo <span style="color:#ff82b2"><b>Greedy best-first</b></span> seguendo l'euristica, fa fare $Arad\to Sibiu\to Fagaras\to Bucarest$ con un costo di $450$, ma <span style="color:#ff82b2"><b>non è la soluzione ottima</b></span>, che invece sarebbe $Arad\to Sibiu\to Rimnicu\to Pitesti\to Bucarest$ ($418$).
# Itinerario con A*
![Itinerario con A*](IntroAI/assets/romEx/romEx6-Astar.png)
Il costo di ogni nodo è la somma del cammino fatto per arrivarci più il valore della tabella moltiplicato a $g(n)$. 
Non espandiamo Fagaras e andiamo subito a Rimnicu ed espandiamo quello mettendo i suoi figli in frontiera. Dopo però viene ripreso in considerazione Fagaras e viene espanso, ma essendo la visita posticipata si visitano prima i figli di Rimnica, si sceglie Pitesti e da li Bucarest. <span style="color:#ff82b2"><i>Questo è il percorso migliore</i></span>
La cosa più importante non è quello che abbiamo fatto, ma quello che NON abbiamof atto, ovvero NON abbiamo espanso Timisorara e Zerind (la prima generazione di figli insieme a Sibiu). Possiamo lasciare indietro e inesplorati interi sotto-alberi.
# Best First ricorsivo
![Esempio dell'albero](IntroAI/assets/romEx/romEx7-rbfs.png)