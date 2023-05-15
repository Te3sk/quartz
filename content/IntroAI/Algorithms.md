---
title: Algoritmi - risoluzione dei problemi come ricerca
alias:
- algo
- elenco algoritmi
- algoritmi prima parte
- 
tags: introai
---
```toc
```
# Algoritmi di ricerca non euristici
## Ricerca ad Albero
``` pseudo-code
FUNCTION Ricerca-Albero (problema)
	inizializza la frontiera con stato iniziale del problema
	LOOP DO
		IF la frontiera è vuota 
			return fallimento
		Scegli* un nodo foglia da espandere e rimuovilo dalla frontiera
		IF il nodo contiene uno stato obiettivo
			RETURN la soluzione corrispondente
		Espandi il nodo e aggiungi i successori alla frontiera
END
```

## Ricerca in ampiezza (BF)
### BF su/ad albero
``` pseudo-code
FUNCTION Ricerca-Ampiezza-A (problema)
	nodo = un nodo con stato problema.stato-iniziale e costo-di-cammino=0
	IF problema.Test-Obiettivo(nodo.Stato)
		RETURN Soluzione(nodo)
	frontiera = coda FIFO con nodo come unico elemento
	LOOP DO
		IF vuota?(frontiera)
			RETURN fallimento
		nodo = POP(frontiera)
		FOR EACH azione IN problema.Azioni(nodo.stato) DO
			figlio = Nodo-Figlio(problema, nodo, azione)
			IF Problema.TestObiettivo(figlio.stato)
				RETURN Soluzione(figlio)
			frontiera = Inserisci(figlio, frontiera)
END
```
### BF su grafo
``` pseudo-code
FUNCTION Ricerca-Ampiezza-G (problema)
	nodo = nodo con stato problema.stato-iniziale e costo-cammino = 0
	IF problema.TestObiettivo(nodo.Stato)
		RETURN Soluzione (nodo)
	frontiera = coda FIFO con nodo come unico elemento
	esplorati = insieme vuoto
	LOOP DO 
		IF Vuota?(frontiera)
			RETURN fallimento
		aggiungi nodo.Stato a esplorati
		FOR EACH azione IN problema.Azioni(nodo.Stato) DO
			IF figlio.Stato non è in esplorati e non è in frontiera
				IF Problema.TestObiettivo(figlio.Stato)
					RETURN Soluzione(figlio)
				frontiera = Inserisci(figlio, frontiera)
END			
```
### Analisi della complessità
- <span style="color:#ff82b2"><b>Completo</b></span>
- <span style="color:#ff82b2"><b>Se</b></span> gli operatori hanno tutti lo stesso costo $k$ $\stackrel{quindi}{\textcolor{#ff82b2}{\implies}}$ $\textcolor{#ff82b2}{g(n)=k\cdot depth(n)}$ dove $g(n)$ è il costo del cammino per arrivare a $n$ $\textcolor{#ff82b2}{\implies}$ <span style="color:#ff82b2"><b>Ottimale</b></span>
- <span style="color:#ff82b2"><b>Complessità nel tempo</b></span> (nodi generati)
$$
T(b,d)=1+b+b^2+\dots+b^d\to \textcolor{#ff82b2}{O(b^d)}
$$
- <span style="color:#ff82b2"><b>Complessità in spazio</b></span> (nodi in memoria)
$$
\textcolor{#ff82b2}{O(b^d)}\text{ [frontiera]}
$$
dove $b$ è il fattore di diramazione e $d$ la profondità dell'albero
## Ricerca in profondità (DF)
### DF su albero
``` pseudo-code
FUNZIONE Ricerca-Profondità-A (problema)
	nodo = Nodo con stato problema.stato-iniziale e costo-di-cammino 0
	IF Problema.TestObiettivo(nodo.Stato)
		RETURN Soluzione(nodo)
	frontiera = coda FIFO con nodo come unico elemento
	LOOP DO
		IF vuota?(frontiera)
			RETURN fallimento
		nodo = POP(frontiera)
		FOR EACH azione IN problema.Azioni(nodo.stato) DO
			figlio = nodo.Figlio (problema, nodo, azione)
			IF figlio.Stato non è in frontiera
				IF Problema.TestObiettivo(figlio.stato)
					RETURN Soluzione(figlio)
			frontiera = Inserisci(figlio, frontiera)
END
```
### DF su grafo
``` pseudo-code
FUNCTION Ricerca-Ampiezza-G (problema)
	nodo = nodo con stato problema.stato-iniziale e costo-cammino = 0
	IF problema.TestObiettivo(nodo.Stato)
		RETURN Soluzione (nodo)
	frontiera = coda FIFO con nodo come unico elemento
	esplorati = insieme vuoto
	LOOP DO 
		IF Vuota?(frontiera)
			RETURN fallimento
		aggiungi nodo.Stato a esplorati
		FOR EACH azione IN problema.Azioni(nodo.Stato) DO
			figlio = nodo.Figlio (problema, nodo, azione)
			IF figlio.Stato non è né in esplorati né in frontiera
				IF problema.TestObiettivo (figlio.stato)
					RETURN Soluzione(figlio)
			frontiera = Inserisci(figlio, frontiera)
END
```
### DF Ricorsivo
``` pseudo-code
FUNCTION Ricerca-DF-A (problema)
	RETURNS Ricerca-DF-ricorsiva(nodo,problema)
END

FUNCTION Ricerca-DF-ricorsiva(nodo, problema)
	IF problema.TestObiettivo(nodo.stato)
		RETURN Soluzione(nodo)
	ELSE
		FOR EACH azione IN problema.Azioni(nodo.Stato) DO
			figlio = Nodo-Figlio (problema, nodo, azione)
			risultato = Ricerca-DF-ricorsiva(figlio, problema)
			IF risultato != fallimento
				RETURN risultato
		RETURN fallimento
END
```
### Analisi della complessità
#### Su albero
- <span style="color:#ff82b2"><b>Complessità in tempo:</b></span> $\textcolor{#ff82b2}{O(b^m)}$ (che può essere $>O(b^d)$)
- <span style="color:#ff82b2"><b>Complessità in spazio:</b></span> $\textcolor{#ff82b2}{bm}$
	dove $m$ è la lunghezza max dei cammini nello spazio degli stati e $b$ il fattore di diramazione
- <span style="color:#ff82b2"><b>Non Completa</b></span> e <span style="color:#ff82b2"><b>Non Ottimale</b></span>
#### su grafo
- Si perdono i vantaggi di memoria (si torna da $bm$ a $\\text{tutti i possibili stati}$)
- diventa <span style="color:#ff82b2"><b>Completa</b></span> in spazi degli stati finiti
- <span style="color:#ff82b2"><b>Non ottimale</b></span>
#### Ricorsiva
- Più efficiente in occupazione di memoria perché mantiene solo il cammino corrente ($m$ nodi al caso pessimo)
## Ricerca in profondità limitata (DL)
L'algoritmo è uguale a quello della [[IntroAI/Algorithms#Versione ad albero|ricerca in profondità]] ma si cerca fino a un certo livello $l$ 
### Analisi della complessità
- <span style="color:#ff82b2"><b>Completa</b></span> per problemi in cui si conosce il limite superiore per la profondità della soluzione $\stackrel{quindi}{\textcolor{#ff82b2}{\implies}}$ <span style="color:#ff82b2"><b>Completo</b></span> se $\textcolor{#ff82b2}{d<l}$
	dove $d$ è la profondità del nodo obiettivo più superficiale e $l$ è il limite
- <span style="color:#ff82b2"><b>Non Ottimale</b></span>
- <span style="color:#ff82b2"><b>Complessità in tempo:</b></span> $\textcolor{#ff82b2}{O(b^l)}$
- <span style="color:#ff82b2"><b>Complessità in spazio:</b></span> $\textcolor{#ff82b2}{O(bl)}$
## Ricerca con approfondimento iterativo (ID)
Si fa [[IntroAI/Algorithms#Ricerca in profondità limitata (DL)|DL]] con $l=1,2,3,...$ fino a trovare la soluzione
### Analisi della complessità
Miglior compromesso tra [[IntroAI/Algorithms#Ricerca in ampiezza (BF)|BF]] e [[IntroAI/Algorithms#Ricerca in profondità (DF)|DF]]
- Vantaggi della BF
	<span style="color:#ff82b2"><b>Completo</b></span> e <span style="color:#ff82b2"><b>Ottimale</b></span> se il costo delle operazioni è fisso
- Con tempi analoghi, ma costo memoria come DF
- <span style="color:#ff82b2"><b>Complessità in tempo:</b></span> $\textcolor{#ff82b2}{O(b^d)}$
- <span style="color:#ff82b2"><b>Complessità in spazio:</b></span> $\textcolor{#ff82b2}{O(bd)}$
### Direzione della ricerca
#### In avanti (o guidata dai dati)
Si esplora lo spazio della ricerca dallo stato iniziale allo stato obiettivo.
Si preferisce quando l'obiettivo è chiaramente definito o si possono formulare una serie limitata di ipotesi
#### all'indietro (o guidata dall'obiettivo)
Si esplora lo spazio di ricerca a partire da uno stato goal e riconducendosi a sotto-goal fino a trovare uno stato iniziale.
Si preferisce quando gli obiettivi possibili sono molti
### Ricerca bidirezionale
Si fa DL nelle due direzioni fino ad incontrarsi
#### analisi
- <span style="color:#ff82b2"><b>Complessità in tempo:</b></span> $\textcolor{#ff82b2}{O(b^{d/2})}$
- <span style="color:#ff82b2"><b>Complessità in spazio:</b></span> $\textcolor{#ff82b2}{O(b^{d/2})}$
Non è sempre applicabile, ad es. se i predecessori non sono definiti o ci sono troppi stati obiettivo
## Ricerca di costo uniforme (UC)
### UC su albero
``` pseudo-code
FUNCTION Ricerca-UC-A (problema)
	nodo = un nodo con stato il problema.stato-iniziale e costo-di-cammino=0 
	frontiera = una coda con priorità con nodo come unico elemento
	LOOP DO
		IF Vuota?(frontiera)
			RETURN fallimento 
		nodo = POP(frontiera)
		IF problema.TestObiettivo(nodo.Stato)
			RETURN Soluzione(nodo)
		FOR EACH azione in problema.Azioni(nodo.Stato) DO
			figlio = Nodo-Figlio(problema, nodo, azione)
		frontiera = Inserisci(figlio, frontiera)
END
```
### UC su grafo
``` pseudo-code
FUNCTION Ricerca-UC-G (problema)
	nodo = un nodo con stato il problema.stato-iniziale e costo-di-cammino 0
	rontiera = una coda con priorità con nodo come unico elemento
	esplorati = insieme vuoto
	LOOP DO
		IF Vuota?(frontiera)
			RETURN fallimento
		nodo = POP(frontiera);
		IF problema.TestObiettivo(nodo.Stato)
			RETURN Soluzione(nodo)
		aggiungi nodo.Stato a esplorati
		FOR EACH azione in problema.Azioni(nodo.Stato) DO
			figlio = Nodo-Figlio(problema, nodo, azione)
			IF figlio.Stato non è in esplorati e non è in frontiera
				frontiera = Inserisci(figlio, frontiera)
			ELSE IF figlio.Stato è in frontiera con Costo-cammino più alto
				sostituisci quel nodo frontiera con figlio
END
```
### Analisi della complessità
- <span style="color:#ff82b2"><b>Ottimale</b></span> e <span style="color:#ff82b2"><b>Completo</b></span> se il costo degli archi è maggiore di un $\textcolor{#ff82b2}{\varepsilon>0}$
- <span style="color:#ff82b2"><b>Complessità:</b></span> $\textcolor{#ff82b2}{O(b^{1+\lfloor C^*/\varepsilon\rfloor})}$
	dove $C^*$ è il costo della soluzione ottima e $\lfloor C^*/\varepsilon\rfloor$ è il numero di mosse nel caso peggiore (arrotondato per difetto)
	Quando ogni azione ha lo stesso costo $\textcolor{#ff82b2}{O({1+d})}$ (simile a [[IntroAI/Algorithms#Ricerca in ampiezza (BF)|BF]]) 
## Confronto delle strategie (versioni su albero)
|    Criterio |    BF    |                    UC                     |    DF    |    DL    |    ID    | Bidirezionale |
| -----------:|:--------:|:-----------------------------------------:|:--------:|:--------:|:--------:|:-------------:|
| Completezza |    si    |                  si (^)                   |    no    |  si(+)   |    si    |    si (£)     |
|       Tempo | $O(b^d)$ | $O(b^{1+\lfloor C^*/\varepsilon\rfloor})$ | $O(b^m)$ | $O(b^l)$ | $O(b^d)$ | $O(b^{d/2})$  |
|      Spazio |  $O(d)$  | $O(b^{1+\lfloor C^*/\varepsilon\rfloor})$ | $O(bm)$  | $O(bl)$  | $O(bd)$  | $O(b^{d/2})$  |
|  Ottimalità | si (\*)  |                   si(^)                   |    no    |    no    | si (\*)  |    si (£)     |
# Ricerca Euristica
## Greedy Best-First
``` pseudo-code
FUNZIONE Greedy-Best-First(problema, euristica)
    nodo-iniziale = nodo con stato = problema.stato-iniziale, costo-cammino = 0, costo-totale = euristica(problema.stato-iniziale)
    frontiera = coda con nodo-iniziale come unico elemento
    esplorati = insieme vuoto
    LOOP DO
        IF Vuota?(frontiera)
            RETURN fallimento
        nodo = POP(frontiera)
        IF problema.TestObiettivo(nodo.Stato)
            RETURN Soluzione(nodo)
        aggiungi nodo.Stato a esplorati
        FOR EACH azione IN problema.Azioni(nodo.Stato) DO
            figlio = NodoFiglio(problema, nodo, azione)
            IF figlio.Stato non è in esplorati e non è in frontiera
                figlio.costo-totale = euristica(figlio.Stato)
                frontiera = Inserisci(figlio, frontiera)
            ELSE IF figlio è nella frontiera con costo-totale maggiore
                sostituisci nodo nella frontiera con figlio
    END
END
```
### Analisi della complessità
- <span style="color:#ff82b2"><b>Complessità:</b></span> Dipende dalla qualità dell'euristica utilizzata e dalla struttura del grafo di ricerca. In generale, l'algoritmo ha una complessità temporale e spaziale di $O(b^m)$, dove b è il fattore di branching massimo e m è la profondità massima della soluzione.
- <span style="color:#ff82b2"><b>Ottimalità:</b></span> Dipende dall'euristica utilizzata. Se l'euristica è ammissibile, cioè se non sovrastima mai il costo effettivo per raggiungere l'obiettivo, allora l'algoritmo è garantito di trovare la soluzione ottima se esiste una soluzione.
- <span style="color:#ff82b2"><b>Completezza:</b></span> dipende anche dall'euristica utilizzata. Se l'euristica è consistente, cioè se soddisfa la proprietà di monotonicità, allora l'algoritmo è garantito di trovare la soluzione ottima se esiste una soluzione. Tuttavia, se l'euristica non è consistente, l'algoritmo potrebbe non terminare o trovare una soluzione non ottima.
## Algoritmo A e A*
La differenza tra i due sta nella funzione $h(n)$ (`euristica`), se questa è ammissibile (non sovrastima) si tratta di `A*` altrimenti `A`.
``` pseudo-code
FUNZIONE Ricerca-A* (problema)
  nodo-iniziale = nodo con stato=problema.stato-iniziale, costo-cammino=0, valore-f=h(problema.stato-iniziale)
  frontiera = coda di priorità con nodo-iniziale come unico elemento, ordinata in base a f(n)
  esplorati = insieme vuoto
  LOOP DO
    SE frontiera è vuota
      RETURN fallimento
    nodo-attuale = POP(frontiera)
    SE problema.Test-Obiettivo(nodo-attuale.stato)
      RETURN Soluzione(nodo-attuale)
    AGGIUNGI nodo-attuale.stato a esplorati
    PER OGNI azione IN problema.Azioni(nodo-attuale.stato) FARE
      figlio = Nodo-Figlio(problema, nodo-attuale, azione)
      SE figlio.stato non è in esplorati e non è in frontiera
        valore-g = nodo-attuale.costo-cammino + costo(nodo-attuale.stato, azione, figlio.stato)
        valore-h = h(figlio.stato)
        valore-f = valore-g + valore-h
        frontiera = Inserisci(figlio, frontiera) con priorità valore-f
      ALTRIMENTI SE figlio.stato è in frontiera e il suo valore-f è maggiore di quello calcolato
        aggiorna la priorità di figlio in frontiera con il nuovo valore-f
  END LOOP
END
```
### Analisi della complessità
#### A
- <span style="color:#ff82b2"><b>Ottimalità</b></span> e <span style="color:#ff82b2"><b>Completezza</b></span> non garantite
- La <span style="color:#ff82b2"><b>Complessità</b></span> dipende dall'euristica utilizzata. In particolare, se l'euristica non è consistente, l'algoritmo potrebbe espandere molti più nodi rispetto all'algoritmo A* con la stessa euristica. In generale, l'algoritmo A con un'euristica non ammissibile è utilizzato solo in casi in cui la complessità computazionale dell'euristica ammissibile è troppo elevata.
#### A*
- <span style="color:#ff82b2"><b>Complessità:</b></span> $\textcolor{#ff82b2}{O(b^m)}$
	L'algoritmo A* ha una complessità temporale dipendente dalla qualità dell'euristica utilizzata, ma in generale è esponenziale nella complessità della soluzione ottima. In particolare, se l'euristica è ammissibile e consistente, allora l'algoritmo A* garantisce di trovare la soluzione ottima con un tempo di esecuzione che è proporzionale al numero di nodi generati dallo spazio degli stati.
- <span style="color:#ff82b2"><b>Ottimalità:</b></span> Ottimo se h(n) è ammissibile
	L'algoritmo A* garantisce di trovare la soluzione ottima se l'euristica utilizzata è ammissibile, cioè se non sovrastima il costo della soluzione minima.
- <span style="color:#ff82b2"><b>Completezza:</b></span> Sì, se il costo è limitato superiormente e h(n) è ammissibile
	L'algoritmo A* garantisce di trovare la soluzione ottima se lo spazio degli stati è finito e se l'euristica utilizzata è ammissibile. Se lo spazio degli stati è infinito, l'algoritmo può non terminare. Tuttavia, se l'euristica è consistente, allora A* è completo anche in spazi degli stati infiniti.
## Beam Search
É come un Best First ma ad ogni passo `k` mantiene solo i nodi più promettenti (`k` l'ampiezza del raggio (beam))
``` pseudo-code
FUNCTION Ricerca-Beam(problema, k)
    nodo = nodo con stato problema.stato-iniziale e costo-cammino = 0
    IF problema.TestObiettivo(nodo.Stato)
        RETURN Soluzione(nodo)
    frontiera = k nodi generati da nodo
    LOOP DO 
        IF Vuota?(frontiera)
            RETURN fallimento
        nuovi-nodi = {}
        FOR EACH nodo IN frontiera DO
            FOR EACH azione IN problema.Azioni(nodo.Stato) DO
                figlio = Nodo-Figlio(problema, nodo, azione)
                IF Problema.TestObiettivo(figlio.Stato)
                    RETURN Soluzione(figlio)
                aggiungi figlio a nuovi-nodi
            frontiera = k nodi migliori di nuovi-nodi
END
```
`k` rappresenta la larghezza della beam, ovvero il numero di nodi migliori da mantenere nella frontiera ad ogni passo.
### Analisi della complessità
- <span style="color:#ff82b2"><b>Complessità:</b></span> dipende dalla larghezza della fascia `k` che viene esplorata, e può essere esponenziale nel caso peggiore, come la ricerca in ampiezza.
- <span style="color:#ff82b2"><b>Ottimalità:</b></span> dipende dall'euristica utilizzata. In generale, non è ottimale, a meno che l'euristica sia adatta a guidare l'algoritmo verso la soluzione ottimale.
- <span style="color:#ff82b2"><b>Completezza:</b></span> dipende dalla larghezza della fascia `k` e dalla struttura del problema. In generale, non è completo perché può terminare prematuramente quando la soluzione non è presente nella fascia `k`.
## A* con approfondimento iterativo (IDA*)
Combina [[IntroAI/Algorithms#A*|A*]] con [[IntroAI/Algorithms#Ricerca con approfondimento iterativo (ID)|ID]], ad ogni passo ricerca in profondità con un limite dato dalla funzione $f$ (e non dalla profondità). Il `limite` viene incrementato ad ogni iterazione e il punto critico è di quanto viene incrementato `limite`.
``` pseudo-code
FUNCTION IDA*-Search(problema)
    limite = f(problema.stato-iniziale)
    LOOP DO
        result = Ricerca-Limite(problema, limite, 0)
        IF result = soluzione THEN
            RETURN soluzione
        ELSE IF result = infinito THEN
            RETURN fallimento
        limite = result
    END LOOP
END

FUNCTION Ricerca-Limite(problema, limite, costo-cammino) 
    nodo = nodo con stato problema.stato-attuale e f(nodo) <= limite
    IF problema.TestObiettivo(nodo.stato) THEN
        RETURN Soluzione(nodo)
    minimo = infinito
    FOR EACH azione IN problema.Azioni(nodo.stato) DO
        figlio = Nodo-Figlio(problema, nodo, azione)
        valutazione = Ricerca-Limite(problema, limite, costo-cammino + costo(azione))
        IF valutazione = soluzione THEN
            RETURN soluzione
        ELSE IF valutazione < minimo THEN
            minimo = valutazione
    RETURN minimo
END
```
### Analisi della complessità
- <span style="color:#ff82b2"><b>Complessità:</b></span> è simile a quella dell'algoritmo A*: esponenziale nel caso peggiore, ma migliore della ricerca esaustiva.
	- <span style="color:#ff82b2"><i>Complessità in spazio:</i></span> $\textcolor{#ff82b2}{O(bd)}$
- <span style="color:#ff82b2"><b>Ottimalità:</b></span> è ottimale (l'euristica è sempre ammissibile)
- <span style="color:#ff82b2"><b>Completezza:</b></span> dipende dal fatto che lo spazio degli stati sia finito o infinito. Se è finito, allora IDA* è completo. Se invece lo spazio degli stati è infinito, IDA* può non terminare o non trovare la soluzione ottima. Tuttavia, è comunque completo in un sottoinsieme di spazi degli stati infiniti.
## Ricerca best-first ricorsiva (RBFS)
É simile a [[IntroAI/Algorithms#DF Ricorsivo|DF Ricorsivo]], tiene tracia ad ogni livello del miglior percorso alternativo. Invece di fare backtracking in caso di fallimento, interrompe l'esplorazione quando trova un nodo promettente (secondo $f$).
``` pseudo-code
FUNCTION Ricerca-Best-First-Ricorsiva(problema)
	RETURN RBFS(problema, CreaNodo(problema.stato-iniziale), infinity)
END

FUNCTION RBFS (problema, nodo, limite)
	IF problema.TestObiettivo(nodo.Stato)
		RETURN Soluzione(nodo)
	successori = []
	FOR EACH azione IN problema.Azioni(nodo.Stato) DO
		aggiungi Nodo-Figlio(problema, nodo, azione) a successori
	IF successori è vuoto
		RETURN (fallimento, infty)
	FOR EACH s in successori DO
		s.f = max(s.g + s.h, nodo.f)
	LOOP DO
		migliore = nodo con f minimo tra tutti i successori
		IF migliore.f > limite
			RETURN (fallimento, migliore.f)
		alternativa = secondo nodo con f minimo tra tutti i successori
		(risultato, migliore.f) = RBFS(problema, migliore, min(limite, alternativa))
		IF risultato != fallimento
			RETURN risultato
	END LOOP
END
```
### Analisi della complessità
- <span style="color:#ff82b2"><b>Complessità:</b></span> dipende dalla qualità della funzione euristica utilizzata, ma in generale ha una complessità esponenziale. Tuttavia, in pratica, l'algoritmo può convergere molto rapidamente rispetto ad altre strategie di ricerca informate, specialmente se la soluzione è raggiungibile a partire da uno stato iniziale relativamente vicino.
- <span style="color:#ff82b2"><b>Ottimalità:</b></span> garantisce la ricerca dell'ottimo globale in spazi di ricerca con alberi con pesi positivi. Tuttavia, in caso di cicli di pesi negativi, RBFS potrebbe non trovare l'ottimo globale.
- <span style="color:#ff82b2"><b>Completezza:</b></span> non è completo in spazi di ricerca infiniti. Tuttavia, se lo spazio di ricerca è finito o limitato dalla profondità massima, RBFS garantisce di trovare una soluzione se esiste.
## A* con memoria limitata (MA*) in versione semplice (SMA*)
Procede come [[IntroAI/Algorithms#A*|A*]] fino ad esaurimento della memoria disponibile, poi "dimentica" il nodo peggiore, doppo aver aggiornato il valore del padre. A parità di $f$ si sceglie il nodo migliore più recente e si dimentica il nodo peggiore più vecchio
### Analisi della complessità
- <span style="color:#ff82b2"><b>Complessità:</b></span> dipende dall'euristica utilizzata. In generale, si può dire che SMA* è meno efficiente di A* ma più efficiente di IDA*.
- <span style="color:#ff82b2"><b>Ottimalità:</b></span>  ottimale se il cammino soluzione sta in memoria
- <span style="color:#ff82b2"><b>Completezza:</b></span> completo, ovvero troverà sempre una soluzione se esiste, a meno che il limite della memoria non sia raggiunto.