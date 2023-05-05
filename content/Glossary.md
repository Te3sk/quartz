---
title: Glossary
alias:
- glossario
- dizionario
- meaning
tags: #critto
---
# Interazione uomo-macchina
## Design
Da Treccani.it: design s. ingl. [propr. «disegno, progetto», dal fr. dessein, che a sua volta è dall’ital. disegno], usato in ital. al masch. – Nella produzione industriale, progettazione (detta più precisamente industrial design) che mira a conciliare i requisiti tecnici, funzionali ed economici degli oggetti prodotti in serie, così che la forma che ne risulta è la sintesi di tale attività progettuale; quando la forma dell’oggetto viene elaborata indipendentemente dalla progettazione vera e propria, si parla più propriam. di styling design.
## Pensiero Computazionale
il pensiero computazionale è un processo di formulazione di problemi e di soluzioni in una forma che sia eseguibile da un agente che processi informazioni.

# Introduzione alle intelligenze artificiali
## VLSI
_**Very large scale integration**_ (in acronimo **VLSI**) è una denominazione generica che indica una elevata integrazione di transistor all'interno di un singolo chip.
## Distanze Manhattan
$$\textcolor{#ff82b2}{h((x,y))=MD((x,y),(x_g,y_h))=|x-x_g|+|y-y_g|}$$
## Backtracking
Il backtracking (in italiano, si può definire "monitoraggio a ritroso") è una **tecnica per trovare soluzioni a problemi in cui devono essere soddisfatti dei vincoli**. Questa tecnica enumera tutte le possibili soluzioni e scarta quelle che non soddisfano i vincoli.
## Training Examples
un esempio della forma $\textcolor{#ff82b2}{(x, f(x) + noise)}$ dove $\textcolor{#ff82b2}{x}$ di solito è un <span style="color:#ff82b2"><i>vettore di caratteristiche</i></span> e $\textcolor{#ff82b2}{f(x)+noise}$ è detto <span style="color:#ff82b2"><i>target value</i></span>
## Target function
la vera funzione $\textcolor{#ff82b2}{f}$
## Ipotesi
Una funzione $\textcolor{#ff82b2}{h}$ proposta che si crede essere <span style="color:#ff82b2"><i>simile a</i></span> $\textcolor{#ff82b2}{f}$. Un'espressione in un dato linguaggio che descriva la relazione tra i dati
## Spazio delle ipotesi
$\textcolor{#ff82b2}{H}$: lo spazio di tutte le ipotesi che possono essere l'output dell'algoritmo di apprendimento
## Fitting
cercare di trovare la curva che minimizza il discostamento verticale (cioè rispetto all'asse y) di un punto dalla curva

# Crittografia
## Crittografia
<span style="color:#ff82b2"><b>Crittografia</b></span> significa <span style="color:#ff82b2"><i>scrittura nascosta</i></span>, ed è lo studio di tecniche matematiche sofisticate per mascherare i messaggi (crittografia) o tentare di svelarla (crittoanalisi).
## Brute force attack
Anche detto <span style="color:#ff82b2"><b>attacco di forza bruta</b></span> o <span style="color:#ff82b2"><b>Attacco esauriente</b></span>: il crittoanalista potrebbe sferrare un <span style="color:#ff82b2"><b>attacco esauriente</b></span> verificando la significatività delle sequenze $D(c, k),\:\forall$ chiave $k$. Se il numero di chiavi possibili $|key|=10^{20}$ e un calcolatore impiegasse $10^{-6}$ secondi per calcolare $D(c, k)$ e verificarne la significatività
occorrerebbe in media più di un <span style="color:#ff82b2"><b>milione di anni</b></span> per scoprire il messaggi provando tutte le chiavi possibili.
## Alfabeto
Insieme finito di <span style="color:#ff82b2"><b>caratteri</b></span> o <span style="color:#ff82b2"><b>simboli</b></span>
Un oggetto è rappresentato da una sequenza ordinata di caratteri dell'alfabeto.
- A oggetti diversi corrispondono sequenze diverse
- Il numero di oggetti che si possono rappresentare non ha limiti
## Insiemi Numerabili
Un insieme i cui elementi possono essere enumerati, cioè descritti da una sequenza del tipo $$a_1,a_2,a_3,...,a_n,...$$
## Algoritmo
Sequenza finita di operazioni, completamente e univocamente determinate
## Problemi indecidibili
Ci sono problemi (e.g. problema dell'arresto) che non possono essere risolti da nessun calcolatore, indipendentemente dal tempo a disposizione
## Problemi intrattabili
Ci sono problemi decidibili che possono richiede tempi di risoluzione esponenziali nella dimensione dell'istanza (e.g. torri di Hanoi, generazione delle sequenze binarie e delle permutazioni)
## Problemi Trattabili
Ci sono problemi che possono essere risolti con algoritmi di costo polinomiale (e.g. ordinamento, problemi sui grafi, ...)
## Problemi presumibilmente intrattabili
Ci sono problemi il cui stato non è noto (e.g. clique, cammino hamiltoniano). Per questi abbiamo a disposizione solo algoritmi di costo esponenziale e nessuno ha dimostrato che non possono esistere algoritmi di costo polinomiale
## Sistemi di Calcolo
Un sistema di calcolo è un insieme di regole e procedure che permettono di eseguire calcoli numerici o logici utilizzando una particolare tecnologia o metodologia. Ogni sistema di calcolo ha le proprie regole e procedure per eseguire calcoli e risolvere problemi.
## Casualità
Una sequenza binaria è detta <span style="color:#ff82b2"><b>casuale</b></span> se <span style="color:#ff82b2"><i>NON</i></span> ammette un algoritmo di generazione la cui rappresentazione binaria sia più corta della sequenza stessa
## Coprimi
coppia di numeri interi che non ammettono divisori comuni diversi da 1 o −1
## Funzioni one-Way
Chiamiamo $f$ <span style="color:#ff82b2"><b>One-Way</b></span> se:
 - dato $x$ calcolare $y=f(x)$ è <span style="color:#ff82b2"><i>facile</i></span> (polinomiale)
 - dato $y$ calcolare $x t.c. f(x)=y$ è <span style="color:#ff82b2"><i>difficile</i></span>
## Cifrario Lineare
Un cifrario è detto <span style="color:#ff82b2"><i>Lineare</i></span> se $\textcolor{#f77ead}{C(q\oplus b,k)\ne(C(a,k)\oplus C(b,k)}$
## Generatori
$a\in Z_n^*$ è un <span style="color:#ff82b2"><b>generatore di</b></span> $\textcolor{#ff82b2}{Z_n^*}$, ma in un ordine difficile da prevedere
## Fattorizzazione
per fattorizzazione si intende la scomposizione di un numero intero in un insieme divisori non banali che, moltiplicati fra loro, diano il numero di partenza
#### Logaritmi discreti
Sono l'equivalente dei logaritmi ordinari per l'[[Crittografia/Algebra Modulare|algebra modulare]].
Si consideri un gruppo ciclico $\textcolor{#ff82b2}{G}$ di ordine finito $\textcolor{#ff82b2}{n}$ (la legge è scritta moltiplicativamente) ed un generatore $\textcolor{#ff82b2}{b}$ di $\textcolor{#ff82b2}{G}$ . Quindi ogni elemento $\textcolor{#ff82b2}{}$$\textcolor{#ff82b2}{x}$ di $\textcolor{#ff82b2}{G}$ può essere scritto nella forma $\textcolor{#ff82b2}{x = b\cdot k}$ per alcuni interi $\textcolor{#ff82b2}{k}$; inoltre, due qualsiasi di questi numeri interi sono necessariamente congruenti modulo $\textcolor{#ff82b2}{n}$. Il logaritmo discreto può essere definito come il più piccolo intero naturale $\textcolor{#ff82b2}{k}$ che soddisfa questa proprietà (ce n'è solo uno tale che $\textcolor{#ff82b2}{0\leq k\leq n}$). Si tratta quindi di una mappatura reciproca dell'elevamento a potenza $\textcolor{#ff82b2}{k}$ ↦ b $\textcolor{#ff82b2}{k}$.
## q-grammi
Gruppi di $q$ lettere consecutive
## Gruppo abeliano additivo
Un insieme di elementi che soddisfano tre proprietà: la proprietà dell'<span style="color:#ff82b2"><b>identità</b></span>, l'<span style="color:#ff82b2"><b>inversa additiva</b></span> e l'<span style="color:#ff82b2"><b>associatività dell'addizione</b></span>. 
- <span style="color:#ff82b2"><b>Identità:</b></span> un elemento neutro, chiamato "0" che quando è aggiunto ad un qualsiasi elemento del gruppo non cambia il valore di questo elemento.
- <span style="color:#ff82b2"><b>Inversa additiva</b></span> di un elemento $x$<span style="color:#ff82b2"><b>Inversa additiva</b></span> è un altro elemento, chiamato $-x$, che quando sommato a $x$, dà come risultato l'elemento neutro.
- <span style="color:#ff82b2"><b>Associatività dell'addizione:</b></span> per qualsiasi scelta di tre elementi del gruppo, la somma $(a + b) + c$ è uguale a $a + (b + c)$
## Forward Secrecy
La <span style="color:#ff82b2"><b>forward secrecy</b></span> è una proprietà dei protocolli di negoziazione delle chiavi che assicura che <span style="color:#ff82b2"><i>se una chiave di cifratura a lungo termine viene compromessa, le chiavi di sessione generate a partire da essa rimangono riservate</i></span>
## Dirac Notation
$|\:\:\rangle$ è detta <span style="color:#ff82b2"><i>Dirac notation</i></span>, ed è la notazione standard per indicare gli stati in meccanica quantistica