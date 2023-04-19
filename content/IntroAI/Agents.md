---
title: Agenti
tags: #introai
---
L'<span style="color:#ff82b2"><b>intelligenza artificiale</b></span> non è una collezione di tecniche per risolvere problemi specifici. Ma vertice e fronte del progresso dei metodi / sistemi informatici per fornire metodologie sistematiche per dotare le macchine di comportamenti <span style="color:#ff82b2"><i>intelligenti / razionali</i></span>.
# Agenti intelligenti
L'approccio moderno all'IA consiste nella costruzione di <span style="color:#ff82b2"><b>agenti intelligenti</b></span>, questo approccio ci offre un quadro di rifermineto ed una prospettiva diversa dall'analisi dei sistemi software. É comoda per trattare sistemi razionali (<span style="color:#ff82b2"><i>uniformità</i></span>) e vedremo schemi di agenti contenitori di funzionalità.
Il primo obiettivo è quello di trovare agenti per la risoluzione di problemi vista come una <span style="color:#ff82b2"><i>ricerca</i></span> in uno <span style="color:#ff82b2"><i>spazio di stati</i></span> (*problem solving*).
![Ciclo percezione-azione](IntroAI/assets/pictures/ciclo_percezione-azione.png)
## Caratteristiche degli agenti
- Sono <span style="color:#ff82b2"><b>situati:</b></span> ricevono percezioni da un ambiente ed agiscono su questo mediante azioni (<span style="color:#ff82b2"><i>attuatori</i></span>)
- Hanno <span style="color:#ff82b2"><b>abilità sociale:</b></span> sono capaci di comunicare, collaborare e difendersi da altri agenti
- Hanno <span style="color:#ff82b2"><b>credenze, obiettivi, intenzioni,</b></span>...
- Sono <span style="color:#ff82b2"><b>embodied:</b></span> hanno un corpo, fino a considerare i meccanismi delle emozioni
## Percezioni ed azioni
> [!quote] Percezione
> 
> Input da <span style="color:#ff82b2"><b>sensori</b></span>

> [!quote] Sequenza percettiva
> 
> Storia completa delle percezioni

La scelta dell'azione è funzione unicamente della sequenza percettiva, ma non da qualcosa che non abbia percepito.
> [!quote] Funzione agente
> 
> Viene implementata da un <span style="color:#ff82b2"><i>programma agente</i></span> e definisce l'azione da compiere per ogni sequenza percettiva. Descrive completamente l'agente
> $$ \text{Sequenza Percettiva}{\xrightarrow{f}} \text{Azione}$$

### [Esempi di architetture astratte di agenti](https://drive.google.com/file/d/1CMqZhumTydbhdq_x7ok43n5fti4T19kx/view?usp=sharing)

# Agenti razionali
Interagisce con il suo ambiente in maniera <span style="color:#ff82b2"><i>efficace</i></span> (fa la cosa "*giusta*"). Serve  un criterio di <span style="color:#ff82b2"><i>valutazione</i></span> oggettivo dell'effetto delle azioni dell'agente.
## Valutazione della prestazione
### Misura di prestazione
- esterna (come vogliamo che il mondo si evolva?)
- scelta del progettista a seconda del problema considerando l'effetto desiderato sull'ambiente
- valutazione su ambienti diversi
## Definizione
La razionalità è relativa a (dipende da):
- le misure di prestazione
- le conoscenze progresse dell'ambiente
- le percezioni presenti e passate
- le capacità dell'agente
> [!quote] Agente razionale
> 
> per ogni sequenza di percezioni compie l’azione che <span style="color:#ff82b2"><b>massimizza il valore atteso della misura delle prestazioni</b></span>, considerando le sue percezioni passate e la sua conoscenza pregressa.
### Razionalità non onniscienza
Non si pretendono perfezione e conoscienza del futuro, ma massimizzare il risultato atteso. Ma potrebbero essere necessarie azioni di acquisizione di informazione o esplorative.
### Razionalità non onnipotenza
Le capacità dell'agente possono essere limitate
### Razionalità e apprendimento
Raramente tutta la conoscenza sull'ambiente può essere fornita a priori (dal programmatore). L'agente razionale deve essere in grado di modificare il proprio comportamente con l'<span style="color:#ff82b2"><i>esperienza</i></span> (le percezioni passate). Può migliorare esplorando, apprendendo, aumentando autonomia per operare in ambienti differenti o mutevoli.
## Agenti autonomi
> [!quote] Agente autonomo
> 
> Un agente è <span style="color:#ff82b2"><i>autonomo</i></span> nella misura in cui il suo comportamento dipende dalla sua capacità di ottenere esperienza, e non dall'aiuto del progettista.

Un agente il cui comportamento fosse determinato solo dalla sua conoscenza *built-in* (pregressa) sarebbe non autonomo e poco flessibile.

# Un agente opera in un [[IntroAI/Environments|Ambiente]]

# Struttura di un Agente
$$
\begin{array}{c}
	\textcolor{#ff82b2}{\text{Agente}} = \text{Architettura} + \text{Programma} \\\\
	\begin{array}{l}
		Ag: P & \to & Az\\
		percezioni && azioni&&&&&&&&&&&&&&&&&&&&&&&&&&&&
	\end{array}
\end{array}
$$
Il <span style="color:#ff82b2"><i>programma dell'agente</i></span> implementa la funzione $Ag$.
## Programma agente
```pseudo-code
function Skeleton-Agent (percept) returns action
	static: memory      //the agent's memory of the world
	memory ← UpdateMemory(memory, percept)
	action ← Choose-Best-Action(memory)
	memory ← UpdateMemory(memory, action)
return action 
```
Nel meta algoritmo ho una memoria, ogni volta che ho una *percezione* aggionro la memoria, quando devo scegliere un'azione scelgo la migliore della memoria.
## Agente basato su tabella
La scelta dell'azione è un accesso a una *tabella* che associa un'azione ad ogni possibile sequenza di percezioni.
<span style="color:#ff82b2"><b>Problemi:</b></span>
- Dimensione: per giocare a scacchi serve una tabella con un numero di righe $>> 10^{80} \Rightarrow$ ingestibile
- Difficile da costruire
- Nessuna autonomia
- Di difficile aggiornamento, apprendimento complesso
Noi vogliamo realizzare agenti razionali con programma compatto.
## Agenti reattivi semplici
![schema Agente reattivo semplice](IntroAI/assets/pictures/agente_reattivo.png)
```pseudo-code
function Agente-Reattivo-Semplice (percezione) returns azione
	persistent: regole    // insieme di regole
	condizione azione     // if - then
		stato ← Interpreta-Input (percezione)
		regola ← Regola-Corrispondente (stato, regole)
		azione ← regola.Azione
return azione
```
Non c'è storia da memorizzare, si reagisce solo alle situazioni attuali. Assume che l'ambiente sia completamente osservabile.
## Agenti basati su modello
![Schema Agente basato su Modello](IntroAI/assets/pictures/agente_basato_su_modello.png)
```pseudo-code
function Agente-Basato-Su-Modello (percezione) returns azione
	persistent: stato,    // descrizione stato corrente
				modello,  // conoscenza del mondo
				regole,   // insieme di regole condizione-azione
				azione    // azione più recente
	stato ← Aggiorna-Stato (stato, azione, percezione, modello)
	regola ← Regola-Corrispondente (stato, regole)
	azione ← regola.azione
return azione
```
Rispetto all'agente reattivo semplice, abbiamo in più <span style="color:#ff82b2"><i>conoscienza sulle azioni del mondo</i></span>. Lo stato è un'aggiornamento di stato, azione, percezione e modello.
## Agenti con obiettivo
![Schema agente con obiettivo](IntroAI/assets/pictures/agente_con_obiettivo.png)
Sono guidati da un <span style="color:#ff82b2"><i>obiettivo nella scelta dell'azione</i></span>, è fornito un goal esplicito (es. città da raggiungere). A volte l'azione migliore dipende da qual è l'biettivo da raggiungere, devono pianificare una <span style="color:#ff82b2"><i>sequenza di azioni</i></span> per raggiungerlo. Sono meno efficienti ma <span style="color:#ff82b2"><i>più flessibili</i></span> di un agente reattivo, dato che l'obiettivo può cambiare non essendo codificato nelle regole.
## Agenti con valutazione di utilità
![schema agenti con valutazione di utilità](IntroAI/assets/pictures/agente_con_valutazione_di_utilità.png)
Possono esserci <span style="color:#ff82b2"><i>obiettivi alternativi</i></span> (o più modi per raggiungerlo): l'agente deve decidere verso quali di questi muoversi, è necessaria una <span style="color:#ff82b2"><b>funzione utilità</b></span> che associa ad uno stato obiettivo un numero reale. Ci sono alcuni obiettivi più facilmente raggiungibili di altri, la funzione utilità tiene conto anche della <span style="color:#ff82b2"><i>probabilità di successo</i></span>.
## Agenti che apprendono
![schema agenti che apprendono](IntroAI/assets/pictures/agente_che_apprende.png)
1. <span style="color:#ff82b2"><b>Componente di apprendimento:</b></span> produce cambiamenti al programma agente, migliora le prestazioni adattando i suoi componenti, apprendendo dall'ambiente
2. <span style="color:#ff82b2"><b>Elemento esecutivo (<i>performance element</i>):</b></span> Il programma agente (visto sinora per decidere le azioni)
3. <span style="color:#ff82b2"><b>Elemento critico:</b></span> osserva e da feedback sul comportamento
4. <span style="color:#ff82b2"><b>Generatore di problemi:</b></span> suggerisce nupve situazioni da esplorare
# Tipi di rappresentazione
![tipi di rappresentazione](IntroAI/assets/pictures/tipi_di_rappresentazione.png)
- rappresentazione atomica: solo il passaggio da uno stato all'altro.
- rappresentazione fattorizzata: nello stato ci sono più variabili e attributi che assumono valore diverso
- strutturata: aggiunge relazione fra gli stati