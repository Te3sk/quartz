# Quantum Computing
```toc
```
##### 2022/12/06 #date 
L'**informatica quantistica** riguarda la *relazione tra fisica e informatica*. I fenomeni fisici si applicano all'informazione ad al calcolo, quindi *un processo computazionale è visto come un processo fisico* eseguito da una macchina le quali operazioni obbediscono a certe leggi fisiche.
La teoria classica della computazione è basata sull'*Universal Turing Machine* (un'astrazione matematica e non un dispositivo fisico), che funziona secondo un insime di ruoli e principi enunciati da Alan Turing e rielaborati da Von Neumann.
> [!the] **Church-Turing Thesis**
> *Every 'function which would naturally be regarded as computable' can be computed by Universal Turing Machine*
> $$
> \begin{array}{l}
> \small{\text{\emph{Ogni 'funzione che sarebbe naturalmente considerata}}}\\
> \small{\text{\emph{come calcolabiile', può essere calcolata dalla Macchina}}} \\
> \small{\text{\emph{Universale di Turing}}}&&&&&&&&&&&&
> \end{array}
> $$

L'assunzione alla base di questo principio è che una Macchina di Turing idealizza un *dispositivo di computazione meccanico*, con infinite potenzialità di memoria, che *obbedisce alle leggi della fisica classica*.
Le macchine basate sul modello di Turing hanno problemi a simulare sistemi quantomeccanici. In particolare, sembrano essere molto lenti ed inefficienti.
Nel 1985, *David Deutsch*, propone un nuovo tipo di sistema di computazione, il '***quantum computer***', il quale può fare tutto ciò che può fare un computer convenzionale, ma è anche capace di *simulare efficientemente processi quantomeccanici*.
## Preview of Quantum Physics
'**Quantum Physics**' è un **modello matematico** inizialmente usato per descrivere fenomeni fisici a livelli microscopici (atomico/subatomico). La teoria dei quanti spiega il loro comportamento da ci da un'immagine più completa dell'universo.
### Fenomeni di fisica quantistica che possono intervenire processando informazioni
#### Superposition
É la proprietà di un sistema quantico di essere in *diversi stati allo stesso tempo*. Un sistema quantistico può trovarsi in più di uno stato con ampiezze diverse da zero (?), quindi diciamo che il nostro sistema è in *sovrapposizione di stati*. Quando ci si evolve da una superposizione, le transizioni conseguenti possono influenzarsi in modo *costruttivo o distruttivo*.
#### Decoherence
Il tentativo di *osservare* o *misurare* lo stato del sistema causa il suo *collasso* verso un unico stato. La probabilità di un sistema di essere osservato in uno specifico stato è il quadrato del valore della sua ampiezza di stato. Dopo la misurazione, il sistema non è più in superposizione e l'informazione tenuta in superposizione è *persa*. La manipolazione sperimentale dei sistemi quantici è estremamente difficile dato che ogni minimo perturbazione può determinarne la **decoherence**.
I **qbits** interagiscono con il loro ambiente in una certa misura, anche se il substrato fisico usato per conservato è stato progettato per mantenerli isolati.
#### No-Cloning
É impossibile creare una coppia indipendente e identica di un arbitrario *quantum state* sconosciuto
#### Entanglement
Possibilità che due o più elementi si trovino in stati quantistici completamente collegati così che, anche se trasportati a grande distanza l'uno dall'altro, *mantengano la loro correlazione*.
## Computer Quantici: bit & qbit
Il **qbit** (**quantum bit**) è l'equivalente del *bit* usato nei computer quantici
Il **bit** è il concetto fondamentale della computazione classica. Possiamo pensare al bit ocmpe un termine astratto avente uno *stato* che può essere *0* o *1*
Il **qbit** è l'elemento più semplice dei sistemi quantici, come il bit, ha uno [[Glossary#Dirac Notation|stato]], ma due stati speciali per il qbit sono $\textcolor{#ff82b2}{|0\rangle}$ e $\textcolor{#ff82b2}{|1\rangle}$, che corrispondono allo stato 0 e 1 dei bit classici.
$$
\begin{array}{llll}
|0\rangle=\begin{pmatrix}1\\0\end{pmatrix} &&&
|1\rangle=\begin{pmatrix}0\\1\end{pmatrix}&&&&&&&&&&
\end{array}
$$
La differenza tra bits e qbits è che i secondi **possono essere in altri stati oltre** $\textcolor{#ff82b2}{|0\rangle}$ **e** $\textcolor{#ff82b2}{|1\rangle}$**:** un qbit può essere in una **superposition** dei due stati simultaneamente
$$
\begin{array}{c}
|\Psi\rangle=\alpha\,|0\rangle+\beta\,|1\rangle\\
\small\text{\textcolor{#ff82b2}{può essere in uno stato dove è sia 1 che 0 allo stesso tempo}}
\end{array}
$$
La rappresentazione delle informazioni è binaria, ma ogni qbit può contenere il doppio delle informazioni rispetto a un bit.
Lo stato di un singolo qbit può essere descritto dalla *combinazione lineare* degli stati $\textcolor{#ff82b2}{|0\rangle}$ e $\textcolor{#ff82b2}{|1\rangle}$
$$
\begin{array}{c}
	\begin{array}{lr}
		|\Psi\rangle=\alpha\:|0\rangle+\beta\:|1\rangle=\alpha\begin{pmatrix}1\\0\end{pmatrix}+\beta\begin{pmatrix}0\\1\end{pmatrix}=\begin{pmatrix}\alpha\\\beta\end{pmatrix}
		&&& \alpha,\beta\in\mathbf{C}\\
	\end{array}\\
	|\alpha|^2+|\beta|^2=1
\end{array}
$$
Gli stati speciali $|0\rangle$ e $|1\rangle$ sono conosciuti come **computational basis states**, e formano una *base ortonormale* per lo spazio vettoriale ($\mathbf{C}^2$).
Possiamo *esaminare un bit* per determinare quando sarà nello stato 0 o 1, ma *non possiamo* fare lo stesso con *un qbit*. Possiamo solo acquisire informazioni molto più ristretto sullo stato quantico, *misurando un qbit* possiamo solo dare risultati 'classici'
- o il risultato è $0$, con probabilità $|\alpha|^2$
- o il risultato è $1$, con probabilità $|\beta|^2$
Dopo la misurazione, ogni informazione è persa. Lo stato corrispondente del qbit dopo la misurazione è o $|0\rangle$ o $|1\rangle$.
| Deterministic bit                                                                                                                                                                                           | Probabilistic Classical Bit                                                                                                                                                                                                                                  | Quantum Bit                                                                                                                                                   |
| :-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| Lo stato di un bit deterministico classico, può essere descritto con un singolo valore binario, che è uguale o a 0 o a 1. Lo stato può essere rappresentato come *uno dei due punti*, etichettati '0' e '1' | Lo stato di un *bit probabilistico* è descritto dalla probabilità $p_0$ e $p_1$ (reali e non-negativi), che soddisfano $\textcolor{#ff82b2}{p_0+p_1=1}$. Lo stato di un bit può essere disegnato come *un punto su una linea* tra le due posizioni di 0 e 1. | Un qbit $\|\psi\rangle$ può essere rappresentato in uno spazio tridimensionale, come *un punto sulla superfice di una sfera* conosciuta come 'sfera di Bloch' |
| ![[Crittografia/assets/deterministic_bit.png]]                                                                                                                                                                                  | ![[Crittografia/assets/probabilistic_bit.png]]                                                                                                                                                                                                                                   | ![[Crittografia/assets/qbit.png]]                                                                                                                                                              |
### Quante informazioni si possono rappresentare con un qbit?
$\alpha$ è un numero complesso, possiamo immaginare di immagazzinare molti bit classici nell'espansione binaria della componente reale di $\alpha$. Se ci fosse un modo sperimentale per misuare esattamente il valore di $\alpha$, allora si potrebbe estrarre quell'informazione classica. Tuttavia $\alpha$ e $\beta$ sono una specie di *informazione nascosta*:
- la misurazione di un qbit darà solo un bit di informazione (0 o 1), sullo stato del qbit
- non c'è modo di capire se $\alpha$ e $\beta$ iniziano sconosciuti, dopo la misurazione sono persi
Nessuno sa perché si verifica questo tipo di collasso
## La potenza della computazione quantica
Il sistema può essere posto in una combinazione di un alto numero di stati: un qbit può essere in *superposition* di 2 stati e *lo stato di $\textcolor{#ff82b2}{n}$ qbits può essere in una superposizione di* $\textcolor{#ff82b2}{2^n}$ *stati*. Eseguire un'operazione su $n$ qbits è come eseguirla su $2^n" bit di informazione 'classici' nello stesso momento. L'**idea** è quella di trovare un algoritmo che faccia convergere tutti gli stati del qbuts in uno che sia la soluzione del probllema che si sta cercando di risolvere.
### Quantum algorithms
Partiamo da uno stato iniziale ben noto, il sistema evolve in modo quantistico: i qbits sono connessi tra loro attraverso un *circuito logico elementare*, e sono manipolati da un *semplice inseiem di regole* (in pratica rotazioni del vettore che rappresentano lo stato quantico)
$$
\begin{array}{c}
\textcolor{#01bd00}{\text{superposizione di stati}}\\
\downarrow\\
\textcolor{#ff82b2}{\text{superposizione di calcoli}}\\
\downarrow\\
\textcolor{#2e80f2}{\text{superposizione di risultati}}
\end{array}
$$
Quando le macchine misurano lo stato finale, *la superposizione di risultati collassa **sul risultato con probabilità più alta***. Questa *misurazione*, in teoria, *restituisce con alta probabilità la soluzione del problema*.
1. L'osservazione del sistema durante questa manipolazioni comporta una *penalità severa*: se guardiamo troppo presto la computazione fallisce, siamo autorizzati a vedere solo lo stato finale della macchina
2. L'interazione con l'esterno avviene tramite la sequenza di bit classici, i *qbit collassano* in quell'istante *ad un solo stato*
### Perché studiare la computazione quantistica
L'idea della computazione quantistica è nata per *simulare in modo efficiente i processi quantomeccanici*. Il modello ha attratto gli informatici in quanto avrebbe potuto aiutare a risolvere problemi di elevata complessità computazionale.
- *La computazione quantistica non viola la tesi di Church-Turing*, secondo la quale non esiste un modello di computazione sostanzialmente più potente thella macchina di Turing, in termini di problemi risolvibili con esso
- É documentato che *neanche i computer quantistici possano risolvere problemi NP-completi in modo efficiente*
## Applicazioni
Le aree in cui gli algoritmi quantici sono applicabili sono:
- simulazioni di sistemi quantici
- crittografia
- ricerca e ottimizzazione
- machine learning
- risolvere grandi sistemi di equazioni lineari
Sono usati anche nelle *applicazioni commerciali* di farmacia e chimica, previsioni meteo e cambiamento climatico, finanza, etc...
Le aspettative sono molto alte, le compagnie tech ed i governi stanno scommettendo sulla computazione quantica.
### Shor's factoring algorithm
Nel '94 Peter Shor inventa un algoritmo quantico che può fattorizzare i numeri in tempo polinomiale. Questo rimane una delle più importanti applicazioni dei computer quantici. *L'odierna crittografia a chiave pubblica diventa attaccabile*
### Grover's quantum search
Nel '96 Lov Grover pubblica il *quantum search algorithm*, che fino ad oggi rimane uno degli algoritmi quantici più importanti. L'algoritmo riguarda la ricerca in un *database non strutturato* con $\textcolor{#ff82b2}{N}$ ingressi. Se stiamo cercando un ingresso contrassegnato univocamente, avremo una complessità di $N$ al caso pessimo e $\frac{N}{2}$ al caso medio. L'algoritmo di Grover ha invece una *complessità* di $\textcolor{#ff82b2}{O(\sqrt{N})}$. Nessun algoritmo di ricerca è più efficiente.
Da un punto di vista pratico, questa accellerazione quadratica fa una grossa differenza. Supponiamo di avere $N=10^{20}$
- *processore quantico:* facendo $10^8$ operazioni al secondo e potendo trovare una soluzione con $10^{10}$ chiamate, finiremmo in $10^2$ secondi (circa *2 minuti*)
- con un *processore classico* della stessa velocità avremmo bisogno di circa $10^{12}$ secondi (circa *3200 anni*).
L'impatto di ciò sulla crittografia, è che c'è la necessità di incrementare la lunghezza delle chiavi crittografiche per sopravvivere agli attacchi di forza bruta.
## Quantum supremacy
La *quantum supremacy* (o *quantum advantage*) è l'obiettivo di dimostrare che un **dispositivo quantico programmabile** può risolvere problemi che i computer classici non possono risolvere in un qualsiasi quantità di tempo fattibile (indipendentemente dall'utilità del problema).
Le implicazioni della *quantum supremacy* sono il compito *ingegneristico di costruire* un computer quantico potente e il compito teorico di *trovare un problema* che può essere risolto da un computer quantico e che abbia un'*accelerazione superpolinomiale* sul miglior algoritmo classico possibile.
## Realizzazione fisica del qbit
Un qbit può essere realizzato
- come due polarizzazioni diverse di un fotone
- come l'allineamento di uno spin nucleare in un campo magnetico uniforme
- come i due stati di un elettrone orbita attorno a un singolo atomo nel modello atomico
	L'elettrone può esistere in due stati, $\textcolor{#2e80f2}{ground}$ e $\textcolor{#ff82b2}{excited}$, che chiameremo rispettivamente $\textcolor{#2e80f2}{|0\rangle}$ e $\textcolor{#ff82b2}{|1\rangle}$. Illuminando l'atomo, con la giusta energia e per il giusto tempo, *è possibile spostare l'elettrone da uno stato all'altro*. Se si riduce il tempo di illuminazione, un elettrone può essere spostato **a metà** *tra uno stato e l'altro*.
Tratteremo i qbit come un **oggetto matematico astratto** e costruiremo una teoria generale di calcolo che non dipenderà da un sistema specifico.