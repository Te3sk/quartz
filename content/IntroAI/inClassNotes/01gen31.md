prima esercitazione 9 febbraio
# valutazione di una strategia 
- 
- ottimalità -> trova anche la soluzione migliore (costo minore)
- 
- 
## ricerca ampiezza (BF)
questo algoritmo potrebbe generare nodi con stati già visitati.
Il problema viene ripassato perché la situazione cambia (es. potrebbe cambiare il costo).
### ricerca-grafo in ampiezza
É la stessa ma non esplora nodi giù esplorati (parti in verde diverse). É stata aggiunta una lista (costosa) per tenere memoria di chi abbiamo esplorato. Quando si fa il pop, quindi lo levo e passo ai figli, il nodo viene aggiunto alla lista degli esplorati. Prima di fare il test obbiettivo e di allargare la frontiera, si controlla che non sia nella lista esplorati e che non sia già in frontiera.
Non si può tenere una tabella statica che contiene tutti gli stati possibili (con delle flag) perché sono troppi e non avrebbe senso mantenere una tabella così grande (probelma di <u>efficienza</u>).
### Analisi della complessità
---
- $b$ ($\text{branch}$) = 
- $d$ ($\text{depth}$) = nodo più vicino alla radice
- $m$ ($\text{max}$) =
---
-> <span style="color:#ff82b2"><b>ricerca in ampiezza (analisi)</b></span>
	Questo algoritmo scala male, risolve solo piccole istanze, ed è particolarmente fragile dal punto di vista della memoria
## Ricerca in profondità (DF)
- $b$ = fattore di diramazione
- $m$  = lunghezza max dei cammini sullo spazio degli stati
Può trovarsi in un ciclo "senza saperlo" e rimanerci all'infinito. Il risparmio in memoria è MOLTO più efficiente.
La versione *a grafo* dell'analisi in profondità perde i vantaggi di memoria per mantenere la lista degli esplorati.
### versione ricorsiva
è ancora più efficente nella gestione di memoria perché mantiene solo il cammino corrente
## ricerca in profondità limitata
limitiamo il numero di livelli $l$ da esplorare. Anche con il limite è completa, per problemi in cui è nota la profondità delle soluzioni (lo fa). Se $d$ (profondità nodi obiettivo) $< l$ è completo e possibile usarlo.
Non è ottimale, $l$ è il limite superiore, potrebbe non restituire il risultato migliore.
## Approfondimento iterativo (ID)
sfrutta la ricerca in profondità ma non assume di conoscere $l$, quindi l'equivalente di $l$ si incrementa di 1 ad ogni livello di profondità e ad ogni giro controlla tutti i nodi sistematicamente. 
Anche se fa molte ripetizioni, perché ad ogni giro li riguarda tutti, è un buon compromesso tra BF e DF in termini di efficienza. In spazio non è esponenziale. In tempo uguale.
è completo e ottimale se il costo degli operatori è fisso. Se però non esiste una soluzione ...
# direzione della ricerca
facendo una ricerca bidirezionale si ha $O(b^{\frac{d}{2}})$ sia in tempo che in spazio
# tre soluzioni
in ordine > di costo e efficacia
- non tornare nello stato da cui si proviene
- non creare cammini con cicli: detto per bf
- non generare i nodi con stati già visitati: è la lista degli esplorati, è il caso completo ma con questo costo
# ricerca di costo uniforme (UC)
algoritmo importante (nella prossima lezione discutiamo questo)
abbiamo 2 elementi principali
- coda con priorità dei nodi
- presenta un test **posticipato** (non quando si genera ma quando si estrae), anche se nell'algoritmo è prima.
è una generalizzazione della ricerca in altezza ma tiene conto del costo diverso tra passi. Si scoglie il nodo di costo minore sulla frontiera e si espande su contorni di costo uniforme.
