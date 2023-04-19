[[IntroAI/assets/slides/2-2023-agents.pdf|slide lezione]]
Prime slide
# Agente razionale
Interagisce con il suo ambiente in maniera <span style="color:#ff82b2"><i>efficace</i></span> (fa la cosa "*giusta*"). Serve  un criterio di <span style="color:#ff82b2"><i>valutazione</i></span> oggettivo dell'effetto delle azioni dell'agente.
# Valutazione delle prestazioni
## misura di prestazione
- esterna (come vogliamo che il mondo si evolva?)
- scelta del progettista a seconda del problema considerando l'effetto desiderato sull'ambiente
- valutazione su ambienti diversi
## Agente razionale: definizione
La razionalità è relativa a (dipende da):
- le misure di prestazione
- le conoscenze progresse dell'ambiente
- le percezioni presenti e passate
- le capacità dell'agente
> [!quote] Agente razionale
> per ogni sequenza di percezioni compie l’azione che <span style="color:#ff82b2"><b>massimizza il valore atteso della misura delle prestazioni</b></span>, considerando le sue percezioni passate e la sua conoscenza pregressa.

## Razionalità non onniscienza
Non si pretendono perfezione e conoscienza del futuro, ma massimizzare il risultato atteso. Ma potrebbero essere necessarie azioni di acquisizione di informazione o esplorative.
## Razionalità non onnipotenza
Le capacità dell'agente possono essere limitate
## Razionalità e apprendimento
Raramente tutta la conoscenza sull'ambiente può essere fornita a priori (dal programmatore).

# Agente singolo / multiagente
possiamo immaginare una situazione con un monoagente o multiagente (ad es. *competitivo* (scacchi), nel quale non necessariamente un algoritmo randomizzato è negativo | o *cooperativo*). La scelta di questo è spesso di *designe*.
# Predicibilità
Nel caso *deterministico* ( ad es. scacchi) quando si fa una mossa non ci sono "sorprese". Nel caso *stocastico* o di un tiro in porta ci sono una serie di elementi da considerare. Si può modellare questa incertezza nell'implementazione. Nel caso *non deterministico* si pianifica una situazione condizionale, si portano avanti più possibilità che si adattano a più situazione
# Espiodico / sequenziale
in un ambiente *sequenziale* ogni decisione influenza le successive
# Noto / ignoto
diverso da *osservabile*: se ho delle carte coperte, è noto perché conosco le regole, ma non osservabile perché non conosco le carte.
Gli ambienti reali sono i peggio perché parzialmente osservabili, stocastici, sequenziali, dinamici, continui, multi agente, ignoti.
# Simultaore di ambienti
utili per gli esperimenti sulle classi di ambienti. Non risolveremo un'istanza, ma diversi tipi (non una partita, ma più tipi di partite) in condizioni diverse. *capire* significa saper *generalizzare*, altrimenti la risposta è stereotipata alla specifica istanza.
$$\text{MI DISSOCPADRE PIO}$$
# Programma agente
Nel meta algoritmo ho una memoria, ogni volta che ho una *percezione* aggionro la memoria, quando devo scegliere un'azione scelgo la migliore della memoria.
## basato su tabella
Si può immaginare la scelta dell'azione come l'accesso a una tabella che associa un'azione ad ogni possibile sequenza di percezioni. Presenta dei problemi nella *dimensione*, nella *difficoltà di costruzione*, nell'*assenza di autonomia* e nella *difficoltà di aggiornamento*.
Invece noi vogliamo realizzare agenti razionali con programma *compatto*.
$$
\LARGE\text{BRUNO SEI UN GRANDE}
$$
# agenti reattivi semplici
quà non c'è storia da memorizzare, si reagisce solo alle situazioni attuali. Assume che l'ambiente sia completamente osservabile.
# agenti basati su modello
in più al precedente abbiamo conoscienza sulle azioni del mondo. Lo stato è un'aggionramento di stato, azione, percezione e modello.
# agenti con obiettivo
in più al precedente è possibile *modificare l'obiettivo*. Nel precedente si costruisce il programma codificando l'obiettivo, in questo caso si può decidere dinamicamente
# agenti con valutazione di utilità
quà si considera anche "l'utilità del gol", quindi si valuta anche la modalità di raggiungimento dello scopo o si prendono in considerazione scopi alternativi. si ha una *funzione utilità* che associa ad uno stato obiettivo un numero reale. la funzione può tenere ocnto anche della prob di successo (strada molto rapida ma spesso c'è traffico).
# agenti che apprendono
il sistema apprende e quindi si adatta, almeno in parte, da solo. Il programma agente non è più fissato, ma può cambiare adattandosi ai cambiamenti e modificando i suoi componenti.
# tipi di rappresentazione
- rappresentazione atomica: solo il passaggio da uno stato all'altro.
- rappresentazione fattorizzata: nello stato ci sono più variabili e attributi che assumono valore diverso
- strutturata: aggiunge relazione fra gli stati


---
# problem solving
[[IntroAI/assets/slides/3-2023-problem_solving.pdf|slide]]
---
# agenti risolutori di problemi
pianifichiamo l'azione e lasciamo che l'agente lavori. 
# processo di risoluzione
passi:
1. 
2. qui c'è un designe che va fatto (ancora umano)
3. "trovare il piano ottimale", la soluzione è algoritmica
4. 
# che tipo di assunzioni?
è tutta un'astrazione
# Formulazione del problema
1. 
2. 
3. dato uno stato attuale e un'azione si definisce uno stato successivo

definire implicitamente lo spazio degli stati è una ticciata perché l'agente non conosce già tutto in anticipo, per alcuni problemi servirebbe uno spazio enorme. Dobbiamo quindi trovare la soluzione ottima in uno spazio definito implicitamente.
4. 
5. 
# algoritmi di ricerca
il costo totale è il costo della ricerca + il costo del cammino soluzione. noi valuteremo gli algoritmi sul costo della ricerca, poi ottimizzeremo sul secondo. uno è il costo per pianificare e l'altro per fare il viaggio (dobbiamo ottimizare questo). se il costo della ricerca è troppo più alto può anche essere impossibile trovare una soluzione.

