---
title: Oltre la ricerca calssica
alias:
- Oltre la Ricerca classica
- argomento 5
tags: #introai
---
# Risolutori "classici"
Gli agenti <span style="color:#ff82b2"><i>risolutori di problemi "classici"</i></span> assumono:
- ambienti completamente osservabili
- ambienti deterministici
- sono nelle condizioni di produrre offline un piano (sequenza di azioni) che può essere eseguito senza imprevisti per raggiungere l'obiettivo
# Verso ambienti più realistici
La ricerca sistematica, o anche euristica, nello spazio degli stati è troppo costosa, sono necessari <span style="color:#ff82b2"><i>metodi di ricerca locale</i></span>.
Dobbiamo inoltre <span style="color:#ff82b2"><i>riconsiderare le assunzioni sull'ambiente</i></span>:
	Le azioni sono non deterministiche e l'ambiente è parzialmente osservabile. Oppure gli ambienti sono sconosciuti e abbiamo quindi problemi di esplorazione.
## Ricerca locale
### Assunzioni
Gli algoritmi visti fin'ora esplorano gli spazi di ricerca cercando un goal e <span style="color:#ff82b2"><i>restituiscono un cammino soluzione</i></span>. A volte però <span style="color:#ff82b2"><b>lo stato goal è la soluzione</b></span> del problema. Gli algoritmi di ricerca locale sono adatti per problemi in cui:
- <span style="color:#ff82b2"><i>la sequenza di azioni non è importante</i></span>, quello che conta è unicamente lo stato goal
- Tutti gli elementi della soluzione sono nello stato ma <span style="color:#ff82b2"><i>alcuni vincoli sono violati</i></span>.
> [!example]
> 
> Nel problema delle regine nella formulazione a stato completo, ci interessa ottenere la soluzione ma non il path

### Gli algoritmi
Non sono sistematici e tengono traccia solo del nodo corrente spostandosi poi sui nodi adiacenti.
Non tengono traccia dei cammini (non servono in uscita): sono <span style="color:#ff82b2"><i>efficienti in occupazione di memoria</i></span> e possono trovare soluzioni ragionevoli anche in spazi molto grandi e infiniti, come nel caso degli spazi continui.
Sono utili per risolvere <span style="color:#ff82b2"><i>problemi di ottimizzazione</i></span>: trova lo stato migliore secondo una <span style="color:#ff82b2"><i>funzione obiettivo</i></span> e lo stato di <span style="color:#ff82b2"><i>costo minore</i></span> (ma non il path).
> [!example]
> 
> - minimizzare il numero di regine sotto attacco
> - training di un modello di Machine Learning

### Panorama dello spazio degli stati
$f$ è euristica di costo della funzione obiettivo (NON del cammino)
![Grafico della funzione](IntroAI/assets/pictures/panoramaSpazioStati.png)
- Uno <span style="color:#ff82b2"><b>stato</b></span> ha posizione <span style="color:#ff82b2"><i>sulla superfice</i></span> e un'altezza che corrisponde al valore della funzione di valutazione (funzione obiettivo)
- Un <span style="color:#ff82b2"><b>algoritmo</b></span> provoca <span style="color:#ff82b2"><i>movimento sulla superfice</i></span>
- Utile per trovare l'avvallamento più basso (es. minimo costo) o il picco più alto (es. max di un obiettivo)
### Ricerca in salita - Hill Climbing
<span style="color:#ff82b2"><b>Steepest ascent/descent</b></span>
É una <span style="color:#ff82b2"><i>ricerca locale greedy</i></span>. Venono generati i successori e valutati, viene scelto un nodo che migliora la valutazioen dello stato attuale e non si tiene traccia degli altri, l'albero di ricerca non sta quindi in memoria. I tipi sono:
- si cerca il <span style="color:#ff82b2"><i>migliore</i></span>: Hill Climbing a salita rapida/ripida
- se ne cerca <span style="color:#ff82b2"><i>uno a caso</i></span> (tra quelli che salgono): Hill Climbing stocastico (anche dipendendo da pendenza)
- si cerca <span style="color:#ff82b2"><i>il primo</i></span>: Hill Climbing con prima scelta (il primo generato tra tanti possibili)
Se non ci sono stati successore migliori, l'algoritmo <span style="color:#ff82b2"><i>termina con fallimento</i></span>.

É come il [[IntroAI/Problem Solving#Ricerca in Profondità (DF)|DF]] ma ancora più economico, non si mantengono in memoria neanche i nodi fratelli, a parte in casi eccezionali.
#### Steepest Ascent
```pseudo-code
function Hill-climbing (problema)
		returns uno stato che è massimo locale
	nodo-corrente = CreaNodo(problema.Stato-iniziale)
	loop do
		vicino = il successore di nodo-corrente di valore più alto
		if vicino.Valore <= nodo-corrente.Valore then
			return nodo-corrente.Stato // interrompe la ricerca
		else nodo-corrente = vicino 
			// altrimenti, se vicino è migliore continua
```
> [!Warning] Nota
> 
> Si prosegue solo se il vicino (più alto) è migliore dello stato corrente, se tutti i vicini sono pegiori o uguali si ferma

Non c'è frontiera a cui ritornare, <span style="color:#ff82b2"><i>si tiene un solo stato</i></span>.
<span style="color:#ff82b2"><b>Tempo:</b></span> il numero di cicli è variabile in base al punto di partenza
#### Problema delle 8 regine
![schema scacchiera](IntroAI/assets/pictures/8regineHillClimbing.png)
Vogliamo minimizzare le coppie di regine che si attaccano, nella figura ci sono già i valori dei successori (di $h$ se ci si spostasse in quel punto). Tra tutti i possibili successori migliori (in questo caso $\textcolor{#ff82b2}{12}$), se ne sceglie <span style="color:#ff82b2"><i>uno a caso</i></span>. Non è detto che vada sempre verso lo zero, se si finisce in un <span style="color:#ff82b2"><i>minimo locale, non si può migliorare</i></span>.
Con questo algoritmo, il problema si blocca l'$86\%$ delle volte, in media però in 4 passi si trova la soluzione o in 3 passi si accorge di essersi bloccato.
- <span style="color:#ff82b2"><i>Costo</i></span> $\textcolor{#ff82b2}{h}$ (stima euristica del costo $f$): numero di coppie di regine che si attaccano a vicenda ($\textcolor{#ff82b2}{17}$ nell'esempio)
- Si cerca il minimo
- I numeri sono i valori dei successori ($\textcolor{#ff82b2}{7\times8}$, 7 posizioni per ogni regina, su ogni colonna).
- Tra i migliori di pari valore ($\textcolor{#ff82b2}{12}$) si sceglie a caso
- <span style="color:#ff82b2"><i>Minimo globale</i></span>$=0$
#### Miglioramenti
1. Consentire (un numero limitato di) <span style="color:#ff82b2"><i>mosse laterali</i></span>:
		ci si ferma per $<$ nell'algoritmo invece che per $\leq$, quindi continua anche a parità di $h$.
2. <span style="color:#ff82b2"><b>Hill Climbing stocastico</b></span>: si sceglie a caso tra le mosse in salita (magari tenendi conto della pendenza)
		converge più lentamente ma a volte trova soluzioni migliori. Si allontana più agevolmente dalle situazioni di massimo locale.
3. <span style="color:#ff82b2"><b>Hill Climbing con prima scelta</b></span>
		Può generare le mosse a caso, uno alla volta, fino a trovarne una migliore dello stato corrente (si prende solo il primo che migliora).
		É come la stocastica ma è utile quando i successori sono molti, evitando una scelta tra tutti. Migliora l'efficienza.
4. <span style="color:#ff82b2"><b>Hill Climbing con riavvio casuale</b></span>
		Ripartire da un punto a caso se ci si ferma in un massimo locale: se la probabilità di successo è $p$, saranno necessarie in media $\Large\frac{1}{p}$ ripartenze per trovare la soluzione (es. nelle 8 regine $p=0.14\rightarrow 7$ ripartenze).
		Hill Climbing con random-restart è tendenzialmente completo.
		La riuscita dipende dalla forma del panorama degli stati: molti minimi locali abbassano $p$ e trovare il buon punto di ripartenza è difficile.
### Tempra Simulata
Questo algoritmo <span style="color:#ff82b2"><i>combina Hill-Climbing con una scelta stocastica</i></span> (non del tutto casuale perché sarebbe poco efficiente).
Viene fatta un'analogia con il processo di tempra dei metalli in metallurgia:
	I metalli vengono portati a temperature molto elevate (alta energia/stocasticità iniziale) e raffreddati gradualmente consentendo di cristallizzare in uno stato a (più) bassa energia
All'inizio quindi ammettiamo molte mosse stocastiche e, andando avanti, sempre meno.
#### Tempra Simulata per ascesa
Ad ogni passo <span style="color:#ff82b2"><i>si sceglie un successore</i></span> $\textcolor{#ff82b2}{n'}$ <span style="color:#ff82b2"><b>a caso</b></span>:
- se migliora, lo stato corrente viene espanso
- se peggiora/rimane uguale ($\Delta E=f(n')-f(n)\textcolor{#ff82b2}{\leq0}$), quel nodo viene scelto con probabilità $\textcolor{#ff82b2}{p=e^{\large\frac{\Delta E}{T}}}$
		Si genera un numero casuale tra 0 e 1: se questo è $<p$ il successore viene scelto, altrimenti no
- Quindi $\textcolor{#ff82b2}{p}$ <span style="color:#ff82b2"><i>è inversamente proporzionale al peggioramento</i></span>, infatti se la mossa peggiora molto, $\Delta E$ diventa un negativo alto e la $p$ si abbassa
- <span style="color:#ff82b2"><b>Fattore Temperatura</b></span>($\textcolor{#ff82b2}{\mathbf{T}}$): decresce con il progredire dell'algoritmo secondo un piano definito. Al progredire dell'algoritmo, quindi quando $T$ diventa bassa ($e^{-\infty}\approx0$), si ferma la probabilità di prendere mosse peggiorative.
Si può tornare indietro ma con una probabilità guidata.
Si può riformulare lo stesso algoritmo per cercare un minimo invece che un massimo.
#### Tempra simulata: analisi
La probabilità $p$ di una mossa in discesa diminuisce col tempo e l'algoritmo si comporta sempre di più come [[IntroAI/OltreLaRicercaClassica#Ricerca in salita - Hill Climbing|Hill Climbing]]. Se $T$ viene decrementato abbastanza lentamente con probabilità tendente ad 1, si raggiunge la soluzione ottimale
#### Tempra simulata: Parametri
Il valore iniziale ed il decremento di $\textcolor{#ff82b2}{T}$ <span style="color:#ff82b2"><i>sono parametri</i></span>. I valori per $T$ sono <span style="color:#ff82b2"><i>determinati sperimentalmente</i></span>: il valore iniziale di $T$ è tale che per valori medi di $\Delta E, p=e^{\large\frac{\Delta E}{T}}$ sia circa $0.5$.
### Ricerca local beam
É la versione locale della [[IntroAI/InfoSearch#Beam search|beam search]]: si tengono <span style="color:#ff82b2"><i>in memoria</i></span> $\textcolor{#ff82b2}{k}$ <span style="color:#ff82b2"><i>stati</i></span> anziché uno solo. 
Ad ogni passo si generano i successori di tutti i $k$ stati: se si trova un goal ci si ferma, altrimenti si prosegue con i $k$ migliori tra questi
> [!Warning] Nota
> 
> - diverso da $K-restart$ (che riparte da zero)
> - diverso da beam search

### Beam Search Stocastica
Si introduce un elemento di casualità, come in un processo di <span style="color:#ff82b2"><i>selezione naturale</i></span>, per diversificare la nuova generazione.
Nella variante stocastica della local beam, si scelgono $\textcolor{#ff82b2}{k}$ <span style="color:#ff82b2"><i>successori</i></span>, ma con probabilità maggiore per i migliori.
Non si scelgono i successori migliori, ma quelli con maggiore probabilità di portare ai migliori.
<span style="color:#ff82b2"><b>Terminologia:</b></span>
- <span style="color:#ff82b2"><i>organismo</i></span> - stato
- <span style="color:#ff82b2"><i>progenie</i></span> - successori
- <span style="color:#ff82b2"><i>fitness</i></span> - valore della $f$ - idoneità
Introduciamo questa terminologia perché vogliamo parlare di processi che simulano la selezione naturale.
### Algoritmi genetici / evolutivi: funzionamento
La <span style="color:#ff82b2"><b>popolazione iniziale</b></span> è formata da $\textcolor{#ff82b2}{k}$ stati/<span style="color:#ff82b2"><i>individui</i></span> generati casualmente, ognuno dei quali è <span style="color:#ff82b2"><i>rappresentato come una stringa</i></span>.
> [!example]
> 
> posizione nelle colonne ("24748552") stato delle 8 regine o con 24 bit

Gli individui sono <span style="color:#ff82b2"><i>valutati da una funzione di fitness</i></span>.
Si <span style="color:#ff82b2"><i>selezionano gli individui per gli "accoppiamenti"</i></span> con una probabilità proporzionale alla fitness. Le coppie danno vita alla <span style="color:#ff82b2"><i>generazione successiva</i></span> combinando materiale genetico o con un meccanismo aggiuntivo di mutazione genetica.
La popolazione ottenuta dovrebbe essere migliore, non è sempre detto, ma di base i genitori migliori generano una progenie migliore. La cosa si ripete fino ad ottenere stati abbastanza buoni (stati obiettivo) o finché non miglioriamo più.
> [!example]
> 
> ![schema esempio delle regine](IntroAI/assets/pictures/exAlgoGenetici.png)
> Per ogni coppia (scelta con <span style="color:#2e80f2"><i>probabilità</i></span> proporzionale alla <span style="color:#ff82b2"><i>fitness</i></span>) viene scelto un punto di crossing over e vengono generati due figli scambiandosi pezzi (del DNA).
> Viene infine effettuata una mutazione casuale che dà luogo alla prossima generazione.
> La fitness progressivamente tenderà a favorire generazioni migliori.
> Emula i meccanismi genetici ma anche l'evoluzione della specie.

#### Nascita di un Figlio
> [!example]
> 
> ![Nascita di un figlio nell'esempio delle regine](IntroAI/assets/pictures/nascitaFiglio.png)
> Le parti chiare sono passate al figlio, quelle grige si perderanno. 

Se i genitori sono molto diversi anche i nuovi stati lo saranno, se sono molto simili, l'evoluzione tende a rallentare. All'inizio gli spostamenti saranno maggiori, si raffinano andando avanti.
#### Algoritmi genetici
Il <span style="color:#ff82b2"><b>Natural Computing</b></span> prende ispirazione dai meccanismi naturali: combinano la tendenza a salire della [[IntroAI/OltreLaRicercaClassica#Beam Search Stocastica|beam search stocastica]] e l'interscambio di informazioni (indiretto) tra thread paralleli di ricerca (blocchi utili che si combinano).
Funziona meglio se il problema (e le sue soluzioni) ha componenti significative rappresentate in <span style="color:#ff82b2"><b>sottostringhe</b></span>, questa necessità è anche il suo <span style="color:#ff82b2"><b>punto critico</b></span>.
> [!example]
> 
> Le formiche inizialmente vanno in giro a caso, quando una raggiunge l'obiettivo rialscia dei feromoni per comunicare con le altre. La formica che ha raggiunto l'obiettivo facendo il percorso più breve, rilascia la scia di feromoni più intensa, quindi seguendo quella, tutte le formiche faranno il percorso più breve.

#### Spazi continui
Fino ad ora ci siamo mossi su spazi discreti, ma molti problemi reali hanno spazi di ricerche continui (fondamentale per il Machine Learning).
Lo <span style="color:#ff82b2"><b>stato</b></span> è descritto da <span style="color:#ff82b2"><i>variabili continue</i></span> $x_1,\dots,x_n$, rappresentabili con un <span style="color:#ff82b2"><i>vettore</i></span> $\textcolor{#ff82b2}{x}$. In questo caso il <span style="color:#ff82b2"><i>fattore di ramificazione è infinito</i></span>, ma abbiamo molti strumenti matematici che portano ad approcci efficienti.
##### Gradient
Se la $f$ è continua e differenziabile (es. quadratica rispetto al vettore $x$), il minimo o il massimo si può cercare usando il <span style="color:#ff82b2"><b>gradiente</b></span>, che restituisce la direzione di massima pendenza del punto (per noi in che direzione andare/di quanto).
Data una $f$ obiettivo su 3D:
$$
\nabla f= (\large\frac{\partial f}{\partial x_1},\frac{\partial f}{\partial x_2},\frac{\partial f}{\partial x_3})
$$
*Hill Climbing Iterativo:* $\large x_{new}=x\pm\eta\nabla f(x)$
	In questa equazione si usa $+$ per salire (maximization) e $-$ per scendere (minimization), la $\eta$ rappresenta la step size, è una costante tipicamente minore di 1.
	L'intera formula quantifica direzione e spostamento, senza cercarlo tra gli infiniti possibili successori.
![Esempio di grafico del gradiente](IntroAI/assets/pictures/graficoGradiente.png)
# Oltre la ricerca classica - Cenni 
## Ambienti più realistici
Gli <span style="color:#ff82b2"><i>agenti risolutori di problemi "classici"</i></span> assumono:
- Ambienti completamente osservabili
- Azioni/ambienti deterministici
- Il piano generato è una sequenza di azioni che può essere generato offline e eseguito senza imprevisti
- Le percezioni non servono se non nello stato iniziale
## Soluzioni più complesse
In un ambiente parzialmente osservabile e non deterministico <span style="color:#ff82b2"><i>le percezioni sono importanti</i></span>: restringono gli stati possibilie e informano sull'effetto dell'azione.
Più che un piano l'agente può elaborare una "<span style="color:#ff82b2"><b>strategia</b></span>", che tiene di conto delle diverse eventualità: un <span style="color:#ff82b2"><b>piano con contingenza</b></span>.
> succede una cosa, ma se succede questa faccio quest'altra
## Azioni non deterministiche - aspirapolvere imprevedibile
<span style="color:#ff82b2"><b>Comportamento:</b></span> Se aspira in una stanza sporca, la pulisce ma talvolta pulisce anche una stanza adiacente. Se aspira in una stanza pulita, a volte rilascia sporco.
<span style="color:#ff82b2"><b>Varianzioni necessarie al modello:</b></span> Il modello di transizione restituisce un <span style="color:#ff82b2"><i>insieme di stati</i></span>, l'agente non sa in quale si troverà. Il piano <span style="color:#ff82b2"><i>di contingenza</i></span> sarà un piano condizionale e magari con cicli.

![Esempio dell'aspirapolvere](IntroAI/assets/pictures/aspirapolvereNonDet.png)
<span style="color:#ff82b2"><b>Esempio:</b></span> $Risultati(Aspira, 1) = \{5,7\}$
<span style="color:#ff82b2"><b>Piano possibile:</b></span> $[Aspira, \text{\textbf{if} stato}=5\text{\textbf{then} [Destra, Aspira] \textbf{else} [ ] }]$
<span style="color:#ff82b2"><b>Soluzione:</b></span> da una sequenza di azioni a piano (albero)
### Come si pianifica: alberi di ricerca AND-OR
In un <span style="color:#ff82b2"><b>albero di ricerca AND-OR</b></span>
- I <span style="color:#ff82b2"><i>nodi OR</i></span> rappresentano le scelte dell'agente (1 sola azione)
- I <span style="color:#ff82b2"><i>nodi AND</i></span> le diverse contingenze (scelte dell'ambiente, più stati possibili). Sono **tutte da considerare**
Una <span style="color:#ff82b2"><b>soluzione</b></span> a un problema di ricerca AND-OR è un <span style="color:#ff82b2"><b>albero</b></span> che:
- ha un <span style="color:#ff82b2"><i>nodo obiettivo</i></span> in ogni foglia
- specifica un'<span style="color:#ff82b2"><i>unica azione</i></span> nei nodi OR
- include tutti gli <span style="color:#ff82b2"><i>archi uscenti da nodi AND</i></span> (tutte le contingenze)
![Esempio di un albero AND-OR](IntroAI/assets/pictures/esempioAndOrTree.png)
$$
\text{Piano: [Aspira, \textbf{if} Stato=5 \textbf{then} [Destra, Aspira] \textbf{else} [ ] ]}
$$
