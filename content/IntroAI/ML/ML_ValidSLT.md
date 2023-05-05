---
title: ML - Validation SLT
alias:
- 
tags: introai
---
# Generalizzazione
Richiamo al concetto di [[IntroAI/ML/ML_introduction#Generalizzazione|generalizzazione]] visto nell'[[IntroAI/ML/ML_introduction|introduzione]]
É un punto centrale, come obiettivo del ML, una teoriache supporta in che condizioni ciò si verifica. Guida la <span style="color:#ff82b2"><i>scelta del "best model"</i></span> e va <span style="color:#ff82b2"><i>verificato nelle applicazioni</i></span>.
> [!quote] Ipotesi di apprendimento induttivo
> 
> Tutti gli $\textcolor{#ff82b2}{h}$ che <span style="color:#ff82b2"><i>approssimano bene</i></span> $\textcolor{#ff82b2}{f}$ sugli esempi di training, approssima bene $f$ <span style="color:#ff82b2"><i>anche sulle nuove istanze (non viste)</i></span> $\textcolor{#ff82b2}{x}$.

| Fase di <span style="color:#ff82b2"><b>Learning</b></span> (<span style="color:#ff82b2"><i>allenamento, fitting</i></span>) | Fase <span style="color:#ff82b2"><b>Predittiva</b></span> (<span style="color:#ff82b2"><i>test</i></span>) |
| ---------------------------------------------------------------------------------------------------------------------------:| ---------------------------------------------------------------------------------------------------------- |
|                  Si costruisce il modello dai dati noti (<span style="color:#ff82b2"><i>dati di training e bias</i></span>) | Si applica ai nuovi esempi. Prendiamo un input $\textcolor{#ff82b2}{x'}$ e <span style="color:#ff82b2"><i>calcoliamo la risposta</i></span> con il modello. Lo compariamo con il <span style="color:#ff82b2"><i>target</i></span> $\textcolor{#ff82b2}{d'}$ che il modello non ha mai visto. Qui <span style="color:#ff82b2"><i>valutiamo le ipotesi predittive</i></span> $\stackrel{quindi}{\textcolor{#ff82b2}{\implies}}$ <span style="color:#ff82b2"><b>generalization capability</b></span>                                                                                                           |
$$
\begin{array}{c}
	{\stackrel{\Large prformance}{\text{in ML}}} \textcolor{#ff82b2}{\Large=} {\stackrel{\Large predictive}{accuracy}}
	\\
	\text{Stimata dagli errori calcolati sul \textcolor{#ff82b2}{Test Set}}
\end{array}
$$
[[IntroAI/ML/ML_DecisionTree#Definition|Overfitting]]

## Premessa: cosa si misura? - recap
Per valutare di solito misuriamo:
- Per la [[IntroAI/ML/ML_LinearModels#Classificazione|Classificazione]]:
	- <span style="color:#ff82b2"><b>MSE</b></span> per la perdita
	- <span style="color:#ff82b2"><b>Accuratezza</b></span> o <span style="color:#ff82b2"><b>tasso di errore medio</b></span> per esito
- Per la [[IntroAI/ML/ML_LinearModels#Regressione|Regressione]]
	- <span style="color:#ff82b2"><b>MSE</b></span>
	- <span style="color:#ff82b2"><b>Root MSE</b></span> ($\textcolor{#ff82b2}{S}$)
	- <span style="color:#ff82b2"><b>Errore assoluto medio</b></span>
	- <span style="color:#ff82b2"><b>Errore assoluto massimo</b></span>
	- Anche <span style="color:#ff82b2"><i>misure statistiche</i></span> come $\textcolor{#ff82b2}{R}$
Un alto errore $\stackrel{comporta}{\textcolor{#ff82b2}{\iff}}$ bassa accuratezza, ad esempio
	$\textcolor{#9172dd}{\rightarrow}$ fitting povero con un alto errore nell'allenamento
	$\textcolor{#9172dd}{\rightarrow}$ generalizzazione povera con alto errore nei test
# Validazione: 2 scopi
## Selezione del modello
<span style="color:#ff82b2"><i>Stima della prestazione</i></span> (errore di generalizzazione) o <span style="color:#ff82b2"><i>modello di apprendimento diverso</i></span> in ordine per scegliere il migliore. Questo include anche la ricerca del <span style="color:#ff82b2"><i>miglior hyperparameters</i></span> del modello.
<span style="color:#ff82b2"><b>Ritorna un modello</b></span>
## Valutazione del modello
Avendo scelto un modello finale, dobbiamo <span style="color:#ff82b2"><i>stimare</i></span>/<span style="color:#ff82b2"><i>valutare</i></span> la sua <span style="color:#ff82b2"><i>predizione di errore</i></span>/<span style="color:#ff82b2"><i>rischio</i></span> sui nuovi dati di test, misuriamo la qualità/performance degli modelli scelti per ultimi.
<span style="color:#ff82b2"><b>Ritorna una stima</b></span>
# Resistenza
> [!quote] Important
> 
> - Se la dimensione dell'insieme è sufficiente $\textcolor{#9172dd}{(es\text{ }15\%\text{ }TR,25\%\text{ }VL,25\%\text{ }TS\text{, insiemi disgiunti})}$
> [image pag. 11 of 3.6]
> - <span style="color:#ff82b2"><b>TR:</b></span> *Insieme di training* usato per il fit - fase di <span style="color:#ff82b2"><b>training</b></span>
> - <span style="color:#ff82b2"><b>VL:</b></span> *Insieme di Validazione* (o *di selezione*) usato per selezionare il miglior modello tra molti e/o configurazioni di hyperparametri - <span style="color:#ff82b2"><b>Selezione del modello</b></span>
> - $\textcolor{#ff82b2}{TR+VL}$ a volte sono uniti e chiamati <span style="color:#ff82b2"><i>insieme di sviluppo/design</i></span> $\stackrel{quindi}{\textcolor{#ff82b2}{\implies}}$ è usato per costruire il modello finale
> - <span style="color:#ff82b2"><b>TS:</b></span> *Insieme di Test* usato per stiamare l'errore di generalizzazione del modello finale - <span style="color:#ff82b2"><b>valutazione del modello</b></span>
> - 

La stima fatta per la selezione del modello, non è una buona stima per la valutazione del test di fase/rischio. <span style="color:#ff82b2"><b>I risultati dell'insieme di test</b></span> non possono essere usati per la selezione del modello.
## Selezione del test o del modello?
Che succede se <span style="color:#ff82b2"><i>l'insieme di test è usato in un ciclo di design ripetuto</i></span>?
Facciamo una <span style="color:#ff82b2"><i>selezione del modello</i></span> e una <span style="color:#ff82b2"><i>valfutazione non affidabile</i></span> (stimiamo l'errore di generalizzazione attesa) e *non siamo in grado di farlo negli esempi futuri*. In questo caso <span style="color:#ff82b2"><i>l'insieme di test usato, fornisce una valutazione troppo ottimistica</i></span> del vero errore di test.
#### Gold rule
> [!quote] Gold Rule
> 
> Mantenere la separazione tra gli obiettivi e usare insiemi separati
> $$
> \begin{array}{c}
>	\text{\textcolor{#ff82b2}{TR} (insieme di training) per l'allenamento}
>	\\
>	\text{\textcolor{#ff82b2}{VL} (insieme di validazione) per la selezione del modello}
>	\\
>	\text{\textcolor{#ff82b2}{TS} (insieme dei test) per la stima del rischio}
> \end{array}
> $$

## Schema di TR/VL/TS
[image pag 13 of 3.6]
## meta-algoritmo semplice
- Separare gli insiemi <span style="color:#ff82b2"><i>TR</i></span>, <span style="color:#ff82b2"><i>VL</i></span>, <span style="color:#ff82b2"><i>TS</i></span>
- Cercare il <span style="color:#ff82b2"><i>miglior</i></span> $\textcolor{#ff82b2}{h_{w,\textcolor{#2e80f2}{\lambda}}()}$ <span style="color:#ff82b2"><i>cambiando gli hyperparametri</i></span> $\textcolor{#ff82b2}{\lambda}$ del modello
- `For Each` valore diverso di $\textcolor{#ff82b2}{\lambda}$ (ricerca a griglia)
	- Cercare il <span style="color:#ff82b2"><i>miglior</i></span> $\textcolor{#ff82b2}{h_{w,\textcolor{#2e80f2}{\lambda}}()}$ che minimizzi l'errore/la perdita empirica (<span style="color:#ff82b2"><i>fitting dell'insieme TR</i></span>), trovando il <span style="color:#ff82b2"><i>miglior parametro</i></span> $\textcolor{#ff82b2}{w}$. Per <span style="color:#ff82b2"><i>miglior</i></span> si intiende il <span style="color:#ff82b2"><i>minimo errore sull'insieme TR</i></span>
- Selezionare il <span style="color:#ff82b2"><i>miglior</i></span> $\textcolor{#ff82b2}{h_{w,\textcolor{#2e80f2}{\lambda}}()}$, dove <span style="color:#ff82b2"><i>miglior</i></span> vuol dire <span style="color:#ff82b2"><i>minimo errore sull'insieme VL</i></span>
- (<span style="color:#ff82b2"><b>facoltativo</b></span>) Ora è possibile fare il fit di $h_{w,\lambda}(x)$ sull'insieme TR+VL con il miglior modellfo $\lambda$
- <span style="color:#ff82b2"><i>Valutare</i></span> la funzione $h_{w,\lambda}(x)$ <span style="color:#ff82b2"><i>sull'insieme TS</i></span>
Il `for each` è un <span style="color:#ff82b2"><b>doppio ciclo</b></span>: Cercare il migliore può essere un `for` su una griglia di valori in un ciclo esterno
	`for each` $\lambda$ si allena un modello $h_{w,\textcolor{#2e80f2}{\lambda}}(x)$ (nel ciclo interno) sull'insieme VL. Poi si tiene il miglior valore di $\textcolor{#2e80f2}{\lambda}$ $\stackrel{quindi}{\textcolor{#ff82b2}{\implies}}$ il modello con il minimo errore o la massima accuratezza sull'insieme VL
### Ricerca su una griglia
> [!quote] Ricerca su una griglia
> 
> Trovare un <span style="color:#ff82b2"><i>valore di un hyperparametro</i></span> $\stackrel{quindi}{\textcolor{#ff82b2}{\implies}}$ un parametro che non è appreso direttamente e che non viene modificato dall'allenamento

La ricerca del miglior hyperparametro può essere un `for` su una griglia di valori candidati. `for each` modello allenato $h_{w,\textcolor{#2e80f2}{\lambda}}$ si calcola il risultato (l'accuratezza) sull'insieme VL, poi si tiene quello con il minimo errore o la massima accuratezza.
> [!example]
> 
> [image pag. 15 of 3.6]
> $\textcolor{#9172dd}{Res1}$ è calcolato nell'insieme VL dal modello con grado polinomiale pari a $1$ e $\lambda=0.1$ allenata sull'insieme TR.
> Il migliore è $\textcolor{#9172dd}{Res3}\to$ grado$=4$ e $\lambda=0.1$ è il vincitore

Possiamo automatizzare questo processo, la parallelizzazione è facile (indipendenza dei tentativi).
## Controesempio
- $20-30$ esempi
- $1000$ variabili di input casuali
- target casuale $0/1$
Scelgo <span style="color:#ff82b2"><i>un modello</i></span> con <span style="color:#ff82b2"><i>una sola variabile/feature che indovina</i></span> "*per caso*" al $99\%$ sull'insieme di dati, poi faccio lo stesso su qualsiasi split degli insiemi TR, VS, TS.
$$
\begin{array}{c}
	\textcolor{#ff82b2}{\text{Risultato perfetto (modello con accuratezza }99\%)?\text{ Cosa non va?}}
	\\
	99\%\text{ non è una buona stima dell'errore di test, quella \textcolor{#ff82b2}{corretta è }} \textcolor{#ff82b2}{50\%}
\end{array}
$$
1. L'errore stimato sugli insiemi TR o VS per la selezione del modello <span style="color:#ff82b2"><i>non è utile per la stima del rischio</i></span>. I dati di TR o VL non vanno usati per scopi di test
2. Usare <span style="color:#ff82b2"><i>tutto il data set</i></span> per *feature/model selection* <span style="color:#ff82b2"><i>lede la correttezza della stima</i></span>
	Il TS è stato usato implicitamente all'inizio. Il test deve essere separato pirma di qualsiasi model selection o design del modello, inclusa la selezione delle features
Un <span style="color:#ff82b2"><i>TS esterno</i></span> fornisce invece la <span style="color:#ff82b2"><i>stima corretta del</i></span> $\textcolor{#ff82b2}{50\%}$ (<span style="color:#ff82b2"><b>random coin result</b></span>). É la correttezza della stima che è in giudizio, non la possibilità di risolvere task.
### Tabella per il controesempio
|          | 1   | 2   | ... | $\textcolor{#ff82b2}{26}$ | 27     | 28     | ... | 100 | <span style="color:#ff82b2"><b>target</b></span> |
| -------- | --- | --- | --- | ------------------------- | ------ | ------ | --- | --- | ------------------------------------------------ |
| 1        | ... | ... | ... | 1                         | 1      | 1      | ... | ... | 1                                                |
| 1        |     |     |     | 0                         | 0      | 0      |     |     | 0                                                |
| 3        |     |     |     | 1                         | 1      | 1      |     |     | 1                                                |
| 4        |     |     |     | 0                         | 0      | 0      |     |     | 0                                                |
| ...      |     |     |     | 0                         | 0      | 0      |     |     | 0                                                |
| ...      |     |     |     | 1                         | 1      | 1      |     |     | 1                                                |
| ...      |     |     |     | 0                         | 0      | 0      |     |     | 0                                                |
| 20       |     |     |     | 1                         | 1      | 1      |     |     | 1                                                |
|          |     |     |     |                           |        |        |     |     |                                                  |
| TS1      |     |     |     | 1                         | 0      | 0      |     |     | 1                                                |
| TS2      |     |     |     | 0                         | 1      | 0      |     |     | 0                                                |
| TS3      |     |     |     | 1                         | 1      | 1      |     |     | 1                                                |
|          |     |     |     |                           |        |        |     |     |                                                  |
| Accuracy |     |     |     | $100\%$                   | $33\%$ | $66\%$ |     |     |                                                  |

## Resistenza e convalida incrociata K-FOLD
Fare resistenza su CV può fare un uso insufficiente dei dati
> [!quote] Convalida incrociata K-fold
> 
> [image pag. 20 of 3.6]
> Si splitta l'insieme dei dati $D$ in $k$ sottoinsiemi <span style="color:#ff82b2"><i>mutualmente esclusivi</i></span> $\textcolor{#ff82b2}{D_1,D_2,...,D_k}$. Alleniamo l'algoritmo di apprendimento su $\textcolor{#ff82b2}{\large\frac{D}{D_i}}$ e lo testiamo su $\textcolor{#ff82b2}{D_i}$. In fine facciamo la <span style="color:#ff82b2"><i>media</i></span> di tutti i risultati di $\textcolor{#ff82b2}{D_i}$ (diagonali).
> 
> 
> Questa tecnica può essere usata sia per il VS che per il TS, <span style="color:#ff82b2"><i>usa tutti i dati per fare allenamento, valutazione o testing</i></span>.

I <span style="color:#ff82b2"><b>problemi </b></span> di questa tecninca sono:
- Troppi fold
- Spesso molto costoso dal punto di vista computazionale
- Combinabile con il VS, doppia k-fold
### Esempio di Selezione e Valutazione del modello con K-Fold
Separiamo i dati in <span style="color:#ff82b2"><b>TR</b></span> e <span style="color:#ff82b2"><b>TS</b></span> (semplice hold-out o k-fold).
<span style="color:#ff82b2"><b>Selezione del Modello</b></span>
	Usa K-Fold CV (interno) sul <span style="color:#ff82b2"><b>TR</b></span>, ottenendo nuovi <span style="color:#ff82b2"><b>TR</b></span> e <span style="color:#ff82b2"><b>VL</b></span> in ogni separazione, per troavre i <span style="color:#ff82b2"><i>migliori hyperparametri</i></span> del modello, applicando una <span style="color:#ff82b2"><b>grid search</b></span> con molti valori possibile dell'hyperparametro.
	$\stackrel{quindi}{\textcolor{#ff82b2}{\implies}}$ per esempio un K-fold-CV per $\textcolor{#ff82b2}{\lambda=0.1}$ un k-fold-CV per $\textcolor{#ff82b2}{\lambda=0.01}$, ... poi prendiamo il <span style="color:#ff82b2"><i>miglior</i></span> $\textcolor{#ff82b2}{\lambda}$
Alleniamo con tutto il TR il modello finale
<span style="color:#ff82b2"><b>Valutazione del modello</b></span>
	Valuta il modello su un <span style="color:#ff82b2"><i>TS esterno</i></span>
# Tipico comportamento dell'apprendimento
[image pag. 24 of 3.6]
Sia l'errore di training che quello del test sono alti, in questo caso abbiamo <span style="color:#ff82b2"><b>underfitting</b></span>: il modello è troppo semplice riferito alla funzione target sia per i dati noti che per quelli nuovi.
[image pag. 26 of 3.6]
L'errore di training è basso, mentre quello di test è crescente, in questo caso abbiamo <span style="color:#ff82b2"><b>overfitting</b></span>: il modello è troppo complesso, si adatta al disturbo dei dati. L'errore di training è molto basso, quello di test può essere alto.
[image pag. 28 of 3.6]
## Dimensione dell'insieme dei dati 
| $l=15$                 | $l=100$                |
| ---------------------- | ---------------------- |
| [image pag. 29 of 3.6] | [image pag- 30 of 3.6] |

# Verso SLT
Mettendo tutto insieme:
- La <span style="color:#ff82b2"><i>capacità di generalizzazione</i></span> (misurata come l'errore di rischio o di test) del modello, rispettando l'errore di training e le zone di overfitting e underfitting
	1. Il ruolo della complessità del modello
	2. Il ruolo del numero di dati
<span style="color:#ff82b2"><b>Statistical Learning Theory</b></span> (<span style="color:#ff82b2"><b>SLT</b></span>): una teoria generale relativa a questi argomenti
## Impostazione formale della Statistical Learning Theory (SLT) - Semplificata
> [!quote] Definition
> 
> - Approssimare la funzione sconosciuta $\textcolor{#ff82b2}{f(x), d\text{ (or }y\text{ or }t)}$ è il target $\textcolor{#ff82b2}{(d=true\text{ }f+noise)}$
> - <span style="color:#ff82b2"><i>Minimizzare la funzione di rischio</i></span> $\textcolor{#ff82b2}{R=\int L(d, h(x))dP(x,d)}$
> - Dati
> 	- Un valore dal teacher ($\textcolor{#ff82b2}{d}$) e la probabilità di distribuzione $\textcolor{#ff82b2}{P(x,d)}$
> 	- Una funzione di perdita (o di costo) ad esempio $\textcolor{#9172dd}{L(h(x),d)=(d-h(x))^2}$
> Cerca $\textcolor{#ff82b2}{h}$ in $\textcolor{#ff82b2}{H}$: $\textcolor{#ff82b2}{Min\text{ }R}$, ma abbiamo solo l'insieme dei dati finito ($\textcolor{#ff82b2}{TR=(x_p,d_p)}, \text{ }p=1,...,l$).
> - Per cercare $\textcolor{#ff82b2}{h}$: minimizzare il <span style="color:#ff82b2"><i>rischio empirico</i></span> (<span style="color:#ff82b2"><i>errore di training</i></span> $\textcolor{#ff82b2}{E}$), trovando il miglio valore per i parametri liberi del modello
> $$
> \textcolor{#ff82b2}{
> 	R_{emp}={\large\frac{1}{l}}\sum_{p=1}^{l}(d_p-h(x_p))^2
> }
> $$
> - <span style="color:#ff82b2"><b>Minimizzazione del rischio empirico (ERM) - Principio induttivo</b></span>

## Vapnik-Chervonenkis-dim e SLT - una teoria generale
> [!quote] VC-dim
> 
> Data la $\textcolor{#ff82b2}{VC-dim}$, una misura della <span style="color:#ff82b2"><i>complessità di </i></span> $\textcolor{#ff82b2}{H}$ (flessibilità per adattare i dati)

> [!quote] VC-Bounds
> 
> $\textcolor{#ff82b2}{VC-bounds}$ nella forma: mantiene con probabilità $\textcolor{#ff82b2}{1-\delta}$ che garantisce il rischio
> $$
> 	R\leq R_{emp}+\varepsilon({\large\frac{1}{l}},VC,{\large\frac{1}{\delta}})
> $$
> $\varepsilon$ è una <span style="color:#ff82b2"><i>funzione</i></span> che <span style="color:#ff82b2"><i>cresce con</i></span> $\textcolor{#ff82b2}{VC}$, che decresce con $l$ e $\delta$. Sappiamo che $\textcolor{#ff82b2}{R_{emp}}$ <span style="color:#ff82b2"><i>decresce usando modelli complessi</i></span> (con $VC-dim$ alto).
> $\textcolor{#ff82b2}{\delta}$ è la <span style="color:#ff82b2"><i>fiducia</i></span>, regola la probabilità che il limite valga.
> Ora possiamo vedere come questo può "spiegare" l'<span style="color:#ff82b2"><i>underfitting</i></span> e l'<span style="color:#ff82b2"><i>overfitting</i></span> e gli aspetti che li controllano

<span style="color:#ff82b2"><b>Intuizione:</b></span>
- $\textcolor{#ff82b2}{l}$ <span style="color:#ff82b2"><i>alto</i></span> $\to$ Fiducia $VC$ bassa e limite vicino a $R$
- <span style="color:#ff82b2"><i>modello molto semplice</i></span> ($VC-dim$ basso) può non essere sufficiente a causa di $\textcolor{#ff82b2}{R_{emp}}$ <span style="color:#ff82b2"><i>alto</i></span> (<span style="color:#ff82b2"><b>underfitting</b></span>)
- $\textcolor{#ff82b2}{VC-dim}$ <span style="color:#ff82b2"><i>alto</i></span> ($\textcolor{#ff82b2}{l}$ <span style="color:#ff82b2"><i>fissato</i></span>) $\to$ $R_{emp}$ basso ma $VC-conf$ e quindi $R$ può crescere (<span style="color:#ff82b2"><b>overfitting</b></span>)
> [!quote] Minimizzazione del rischio strutturale
> 
> [image pag.34 of 3.6]
> <span style="color:#ff82b2"><b>flessibilità</b></span> - concetto di controllo della complessità del modello: compromesso tra <span style="color:#ff82b2"><i>accuratezza del TR</i></span> (fitting) e la <span style="color:#ff82b2"><i>complessità del modello</i></span> ($VC-dim$)

## Controllo della complessità - discussione
<span style="color:#ff82b2"><b>Statistical Learning Theory (SLT):</b></span>
	Permette <span style="color:#ff82b2"><i>l'inquadramento formale del problema della generalizzazione</i></span> e dell'<span style="color:#ff82b2"><i>underfitting</i></span>/<span style="color:#ff82b2"><i>overfitting</i></span>, fornendo limitaizioni superiori analitiche e quantitative al rischio $R$ di predizione su tutti i dati, indipendentemente dal tipo di learning algorithm o dai dettagli del modello.
	Il ML è ben formato:
	- il <span style="color:#ff82b2"><i>rischio del learning</i></span> (e l'errore di generalizzazione) può essere <span style="color:#ff82b2"><i>analiticamente limitato</i></span> e solo pochi concetti fondamentali
	- Si può trovare una <span style="color:#ff82b2"><i>buona approssimazione della funzione target</i></span> $\textcolor{#ff82b2}{f}$ da esempi, a condizione di avere un <span style="color:#ff82b2"><i>buon numero di dati ed un'adeguata complessità del modello</i></span> (misurabile formalmente con la VC-dim)
	La SLT porta a <span style="color:#ff82b2"><i>nuovi modelli</i></span> (<span style="color:#ff82b2">SVM</span>) e altri modelli che direttamente considerino  il controllo della complessità nella costruzione del modello. Inoltre fonda uno dei principi induttivi sul <span style="color:#ff82b2"><i>controllo della complessità</i></span>
<span style="color:#ff82b2"><i>Quali</i></span> (altri) <span style="color:#ff82b2"><i>principi ci sono per fondare il controllo della complessità e <u>come operare in pratica</u>?</i></span>
- Come misurare la complessità? (flessibilità per il fitting)
- Come trovare il bilanciamento ottimo tra fitting e complessità
### Esempi sul controllo della complessità
[[IntroAI/ML/ML_LinearModels|Modelli lineari]]
- La complessità sembra correlata al numero di parametri liberi $w$ e alla dimensione dell'input / dell'espansione di base
- Si usano i parametri-$\lambda$ per la versione regolarizzata, usando le tecniche di selezione / validazione del modello per trovare i valori propri di $\lambda$
[[IntroAI/ML/Machine Learning|DT|Decision Tree]]
- numero di nodi
# Conclusioni
#### Flessibilità dei modelli di ML
Usare il potere del ML senza controllo è un modo di produrre <span style="color:#ff82b2"><i>risultati illusori</i></span>. Si controlla il <span style="color:#ff82b2"><i>compromesso</i></span> tra il fitting e la complessità di un modello. É fondamentale il <span style="color:#ff82b2"><i>ruolo degli approcci di validazione</i></span>, per la selezione e la stima del modello.
#### Il ML è ben fondato teoricamente
Vengono fatte domande fondamentali tramite SLT ed altre.