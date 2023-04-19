---
title: Machine Learning
alias:
- ML
- DT
- Linear
tags: 
- #introai
- #AItask 
- #AImodel
---
# Introduzione
> [!quote]  
> 
> "*The problem of <span style="color:#ff82b2"><b>learning</b></span> is arguably at the very core of the problem of <span style="color:#ff82b2"><b>intelligence</b></span>, both biological and artificial*"
> 	- Poggio, Shelton, AI magazine

L'apprendimento è una delle sfinde maggiori e una strategia per rendere i sistemi **intelligenti**.
Il Machine Learning è emerso in un'area di ricerca che combina lo scopo di <span style="color:#ff82b2"><i>creare computer che possono imparare</i></span> (<span style="color:#ff82b2"><i>IA</i></span>) e quello di <span style="color:#ff82b2"><i>creare nuovi strumenti statistici potenti e adattivi</i></span> con fondamenti rigorosi nell'informatica. Gli scopi sono:
|                            AI Methodology |                            Statistical Learning                             | Innovative application areas                                            |
| -----------------------------------------:|:---------------------------------------------------------------------------:|:----------------------------------------------------------------------- |
| costruire sistemi intelligenti e adattivi | costruire sistemi potenti e predittivi per un'intelligente analisi dei dati | usare modelli come strumenti per problemi complessi e interdisciplinari |

## Quando si usano i modelli di apprendimento predittivit?
- Se si ha una <span style="color:#ff82b2"><b>conoscenza scarsa, assente o difficile da formalizzare</b></span>
- Se si hanno <span style="color:#ff82b2"><b>dati incerti, varianti o incompleti</b></span> che ostacolano la formalizzazione del risultato
- Se si hanno <span style="color:#ff82b2"><b>ambienti dinamici</b></span> che non si conoscono in anticipo
Per raggiungere questi risultati sserve una sorgente di esperienza e tolleranza sulla precisione dei risultati.
![Schema dei modelli](IntroAI/assets/pictures/ML_primoSchema.jpg)
Le componenti *task*, *learning algorithm* e *validation* guidano la costruzione del modello cambiando i parametri del sistema con i dati del caso specifico.
> [!example] Riconoscimento della scrittura a mano
> ![Numeri scritti a mano](IntroAI/assets/pictures/ML_numbers.jpg)
> - $\textcolor{#ff82b2}{\fbox{INPUT:}}$ Gruppo di immagini con cifre scritte a mano
> - $\textcolor{#ff82b2}{\fbox{Problema:}}$ Costruire un modello che riceve in input una di queste immagini e "preveda" le cifre
> La difficoltà sta nel <span style="color:#ff82b2"><i>formalizzare</i></span> l'esatta soluzione: probabilmente avremo dati disturbati e/o ambigui.
> Avere collezioni di dati per l'allenamento è relativamente semplice

## Supervised Learning
#AItask
> [!quote] <span style="color:#ff82b2"><b>Supervised Learing</b></span>
> 
> - $\textcolor{#ff82b2}{\fbox{INPUT}:}$ Esempi della forma $\textcolor{#ff82b2}{<input,output>=(x,d)}$, detti <span style="color:#ff82b2"><b>labeled examples</b></span> per una funzione $\textcolor{#ff82b2}{f}$ sconosciuta
> 	- <span style="color:#ff82b2"><b>Target Value:</b></span> il valore di $\textcolor{#ff82b2}{d}$ desiderato, dato dall'insegnante in accordo con $\textcolor{#ff82b2}{f(x)}$ e $\textcolor{#ff82b2}{(x,d)}$
> - $\textcolor{#ff82b2}{\fbox{OUTPUT}:}$ Una buona approssimazione di $\textcolor{#ff82b2}{f}$

Il <span style="color:#ff82b2"><b>target</b></span> $\textcolor{#ff82b2}{d}$ (anche $\textcolor{#ff82b2}{t}$ o $\textcolor{#ff82b2}{y}$) è un'<span style="color:#ff82b2"><b>etichetta</b></span> categorica o numerica
- <span style="color:#2e80f2"><b>Classificazione:</b></span> $\textcolor{#ff82b2}{f(x)}$ restituisce la classe (assunta) corretta per $\textcolor{#ff82b2}{x}$. $\textcolor{#ff82b2}{f(x)}$ è una funzione a valori discreti $\textcolor{#ff82b2}{\in \{1,2,...,k\}}$
- <span style="color:#2e80f2"><b>Regressione:</b></span> valori in output reali e continui
> [!example]
> 
> | Handwrite reconition                                                                           | Diagnosi di malattie                                                                                                                                 | Face Reconition                                                                                    | Spam detection                                                                  |
> | ---------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
> | $\textcolor{#ff82b2}{x}$: dati da immagini, $\textcolor{#ff82b2}{f(x)}$: lettere dell'alfabeto | $\textcolor{#ff82b2}{x}$ proprietà deoi pazienti, $\textcolor{#ff82b2}{f(x)}$: diagnosi, $\textcolor{#ff82b2}{<x, f(x)>}$: database di record medici | $\textcolor{#ff82b2}{x}$: bitmap di foto di facce, $\textcolor{#ff82b2}{f(x)}$: Nome della persona | $\textcolor{#ff82b2}{x}$: emails, $\textcolor{#ff82b2}{f(x)}$: `true` o `false` |

## Unsupervised Learning
#AItask 
L'apprendimento può essere anche non supervisionato
- <span style="color:#2e80f2"><b>Training Set:</b></span> insieme di dati non etichettati $\textcolor{#ff82b2}{x}$
- Cerco di trovare <span style="color:#ff82b2"><b>raggruppamenti naturali di dati</b></span>
# Modelli
#AImodel
> [!quote] <span style="color:#ff82b2"><b>Modello</b></span>
> 
> Un modello definisce la classe di funzioni che la macchina di apprendimento può implementare

Lo <span style="color:#ff82b2"><b>scopo</b></span> è quello di definire relazioni tra i dati:
- <span style="color:#2e80f2"><b>Training example(</b></span>superv.<span style="color:#2e80f2"><b>):</b></span> un esempio della forma $\textcolor{#ff82b2}{(x, f(x) + noise)}$ dove $\textcolor{#ff82b2}{x}$ di solito è un <span style="color:#ff82b2"><i>vettore di caratteristiche</i></span> e $\textcolor{#ff82b2}{f(x)+noise}$ è detto <span style="color:#ff82b2"><i>target value</i></span>
- <span style="color:#2e80f2"><b>Target function:</b></span> la vera funzione $\textcolor{#ff82b2}{f}$
- <span style="color:#2e80f2"><b>Hypotesis:</b></span> Una funzione $\textcolor{#ff82b2}{h}$ proposta che si crede essere <span style="color:#ff82b2"><i>simile a</i></span> $\textcolor{#ff82b2}{f}$. Un'espressione in un dato linguaggio che descriva la relazione tra i dati
- <span style="color:#2e80f2"><b>Hypotesis space (</b></span>$\textcolor{#ff82b2}{H}$<span style="color:#2e80f2"><b>):</b></span> lo spazio di tutte le ipotesi che possono essere l'output dell'algoritmo di apprendimento
### Linguaggi per H
Le <span style="color:#ff82b2"><b>ipotesi</b></span> possono essere <span style="color:#ff82b2"><b>espresse</b></span> in:
1. <span style="color:#ff82b2"><i>logica del prim'ordine</i></span>
2. <span style="color:#ff82b2"><i>equazioni numeriche</i></span>
3. <span style="color:#ff82b2"><i>probabilità</i></span>
### Learning Algorithms
> [!quote] <span style="color:#ff82b2"><b>Learning Algorithms</b></span>
> 
> Basati su dati, task e modelli, apprendono come una ricerca (euristica) tra lo spazio delle ipotesi $\textcolor{#ff82b2}{H}$ dell'ipotesi migliore

Si tratta quindi di trovare la migliroe approssimazione della funzione target (sconosciuta). Tipicamente si cerca $\textcolor{#ff82b2}{h}$ con il minimo errore.
$\textcolor{#ff82b2}{H}$ può non coincidere con il set di possibili funzioni e la ricerca può non essere esaustiva: abbiamo bisogno di fare **Assunzioni** $\textcolor{#ff82b2}{\to}$ <span style="color:#ff82b2"><b>inductive bias</b></span>.

# Generalizzazione
> [!quote] <span style="color:#ff82b2"><b>Generalization</b></span>
> 
> **Buona generalizzazione di errore:** misura quanto accuratamente il modello prevede nuovi campioni di dati

La generalizzazione è un punto cruciale del Machine Learing. L'apprendimento e la ricerca di una buona funzione in uno spazio di funzioni usando dati nuovi (di solito si punta a minimizzare l'errore/la perdita)
| $\textcolor{#ff82b2}{\stackrel{\Large{training}}{fitting}}$  Fase di apprendimento                | Fase predittiva $\textcolor{#ff82b2}{test}$ |
| -------------------------------------------------------------------------------------------------: | ------------------------------------------- |
| Costruzione del modello dai dati nati ($\stackrel{\Large\text{training}}{\text{and bias}}$ datas) | Applicazione di <span style="color:#ff82b2"><b>nuovi esempi</b></span>, valutazione delle ipotesi predittive $\stackrel{quindi}{\to}$ della capability della generalizzazione                                            |

$$
{\stackrel{\Large prformance}{\text{in ML}}} \textcolor{#2e80f2}{\Large=} {\stackrel{\Large predictive}{accuracy}}
$$
Si ha quindi un miglioramento nella task $\textcolor{#ff82b2}{T}$, rispettando la misura di prestazione $\textcolor{#ff82b2}{P}$ e basato sull'esperienza $\textcolor{#ff82b2}{E}$.
# Concept learing
> [!quote] <span style="color:#ff82b2"><b>Concept Learning</b></span>
> 
> Inferire una funzione booleana da esempi di training positivi e negativi

$$
\begin{array}{c}
	c: X\to \{t,f\}\lor\{yes, no\}\lor\{+,-\}\lor\{0,1\}\\
	\text{con }X: \text{spazop delle istanze}
\end{array}
$$
- <span style="color:#2e80f2"><b>Example:</b></span> $\textcolor{#ff82b2}{x,c(x)>\in D}$
- $\textcolor{#ff82b2}{h:X\to\{0,1\}}$ soddisfa $\textcolor{#ff82b2}{x}$ se $\textcolor{#ff82b2}{h(x)=1}$
- $\textcolor{#ff82b2}{D}$, se $\textcolor{#ff82b2}{h(x)=c(x)}$ per ogni esempio di training $\textcolor{#ff82b2}{<x, c(x)\in D}$
## Funzioni booleane ben e mal poste
> [!example] Example: Learning Boolean functions
> 
> ![schema e tabella di verità](IntroAI/assets/pictures/ML_esempio_conceptLearning.jpg)
> É un problema <span style="color:#ff82b2"><b>mal posto:</b></span> potremmo violare la soluzione o la sua esistenza, unicità o stabilità.
> Abbiamo $\textcolor{#ff82b2}{2^{2^4}=2^16=65536}$ possibili funzioni su 4 valori in input. Non possiamo sapere quale sia quella corretta finché non le vediamo tutte

In generale $\textcolor{#ff82b2}{|H|=2^{\#istanze}=2^{2^n}}$, dove $\textcolor{#ff82b2}{n}=\text{dimensione dell'input}$. Cerchiamo di lavore con uno <span style="color:#ff82b2"><i>spazio delle ipotesi ristretto</i></span>, cominciamo scegliendo un $\textcolor{#ff82b2}{H}$ considerabilmente minore dello spazio di tutte le funzioni possibili
## Regole delle congiunzioni
<span style="color:#2e80f2"><b>Literali possibili</b></span> (in AND): $\textcolor{#ff82b2}{|H|=2^n}$, se consideriamo anche l'operatore NOT: $\textcolor{#ff82b2}{3^n+1}$
## Rappresentare le ipotesi
Nella <span style="color:#ff82b2"><b>rappresentazione delle ipotesi</b></span> consideriamo $\textcolor{#ff82b2}{h}$ una congiunzione di costrutti sugli attributi, ognino di questi può essere un valore
|    Specifico | Trascurabile | Non Accettato |
| ------------:| :------------: |:------------- |
| Water = warm | Water = ?    | Water = $\emptyset$              |
> [!example]
> $$
> \begin{array}{c}
> 
> {\LARGE{<}}
> \begin{array}{cccccc}
> sky & temp & Humid & Wind & Water & Farecast\\
> sunny & ? & ? & strong & ? & same
> \end{array}
> {\LARGE >}
> \Rightarrow
> \\
> \Rightarrow
> sky= \text{sunny }\wedge\text{ wind}=\text{strong }\wedge\text{ forecast}=\text{same}
> \end{array}
> $$


|                                                                                                <span style="color:#ff82b2"><b>INPUT</b></span> | <span style="color:#ff82b2"><b>OUTPUT</b></span> |
| ----------------------------------------------------------------------------------------------------------------------------------------------:|:------------------------------------------------ |
| <span style="color:#ff82b2"><b>Istanza</b></span> $\textcolor{#ff82b2}{X:}$ Possibili giorni descritti dagli attributi $sky,temp,humidity,...$ |                                                  |
|                                                                                                                                                |                                                  |


- <span style="color:#ff82b2"><b>Istanza</b></span> $\textcolor{#ff82b2}{X:}$ Possibili giorni descritti dagli attributi $sky,temp,humidity,...$
- <span style="color:#ff82b2"><b>Target Function:</b></span> $c:EnjoySportX\to\{0,1\}$
- <span style="color:#ff82b2"><b>Ipotesi</b></span> $\textcolor{#ff82b2}{H:}$ congiunzione di un insieme finito di letterali $<\text{sunny ? ? strong ? same}>$
- <span style="color:#ff82b2"><b>Esempi di training</b></span> $\textcolor{#ff82b2}{D:}$ esempi positivi e negativi della funzione target $<x_1,c(x_1>, ..., <x_n, c(c_n)>$
