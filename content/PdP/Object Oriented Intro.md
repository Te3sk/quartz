# Object Oriented Intro
```toc
```
##### 2022/11/-- #date 
## Modularità dei programmi
I linguaggi di programmazione supportano in vari modi la possibilità di *modularizzare* i programmi. Ad esempio:
- Sotto il *profilo linguistico*, con *l'astrazione procedurale* (possibilità di scomporre il problema in sottoproblemi da risolvere con specifiche procedure/funzioni)
``` java
int main () {
	..
	int x = sotto_problema();
	..
}

int sotto_problema () {...}
```
- Sotto il *profilo dei tipi di dato*, con i *tipi di dato astratti*
``` ocaml
module type BOOL = sig
	type t
	val yes: t
	val no: t
	val choose: t->'a->'a->'a
```
- Sotto il *profilo delle tecniche di compilazione* ed esecuzione, con la *compilazione separeata* e il linking
## Livelli di astrazione
La possibilità di modularizzare i programmi consente di progettare e sviluppare un programma per *livelli di astrazione*. Ad esempio:
- La *libreria standard* consente di scrivere un programma che opera su stringhe astraendo da come le operazioni su stringhe siano implementate
- Per implementare un programma di *gestione di banche*
	1. Si implementano moduli di gestione dei *singoli conti correnti*
	2. Si passa a implementare i moduli di gestione di una *filiale* usando i conti correnti, ma astraendo dalla loro implementazione
	3. Si passa ad implementare i moduli di gestione della *rete di filiali* usando il modulo della singola filiale, ma astraendo dalla sua implementazione
Tutti i **sistemi informatici complessi** sono organizzati per *livello di astrazione*. Ad esempio:
- Linguaggi di programmazione -> `source code -> bytecode -> assembler`
- Sistemi Operativi -> `applicazione -> sistema operativo -> hardware`
- Protocolli di comunicazione su reti -> `http -> tcp -> ip -> Ethernet`
## Modularità e Ingegneria del Software
Sviluppare un programma complesso in modo modulare consente inoltre di *suddividere il lavoro* tra sviluppatori diversi:
- Si definiscono le **specifiche** (e le interfacce) delle diverse parti
- Ognuno sviluppa la propria parte seguendo le specifiche e assumendo che anche gli altri le seguono
- Esistono metodi di [[SOFTWARE-ENGINEERING|Ingegneria del Software]] per definire le specifiche in modo non ambiguo usando diagrammi standard (UML)
## Il paradigma a oggetti
> **Sistema di software** = insieme di *oggetti cooperanti*

Gli oggetti sono caratterizzati da:
- Uno **stato**
- Un insieme di **funzionalità**
- Gli oggetti cooperano scambiandosi *messaggi*
### Lo stato di un oggetto
Lo **stato** di un oggetto è solitamente *rappresentato* da un gruppo di *attributi/proprietà/variabili*
> [!def] Proprietà di **incapsulamento**
> Idealmente, lo stato di un oggetto non dovrebbe essere accessibile da altri oggetti

Un oggetto $A$ non dovrebbe poter leggere/modificare le variabili che rappresentano lo stato di un altro oggetto $B$ (*Information Hiding*). Anzi, $A$ non dovrebbe nemmeno aver bisogno di sapere come lo stato di $B$ sia rappresentato (che variabili usa)...
### Lo stato del Programma
Idealmente, in un linguaggio basato solo sul paradigma object-oriented, lo *stato del programma* corrisponderebbe all'*insieme degli stati degli oggetti che lo compongono*. In aggiunta a questo, ci saranno le struttre dati di un sistema, necessarie per l'esecuzione del supporto a run-time (es. run-time stack).
### Le funzionalità di un oggetto
Le *funzionalità* di un oggetto sono solitamente *rappresentate* da un gruppo di *metodi/funzioni* che l'oggetto *mette a disposizione degli altri oggetti*.
I metodi descrivono il **comportamento** dell'oggetto, ossia come un oggetto "risponde" ad un messaggio ricevuto da un altro oggetto, anche modificando il proprio stato interagendo con altri oggetti. Solitamente l'*invio* di un messaggio è codificato come la *chiamata di un metodo* e la *risposta* ad un messaggio è codificato come *restituzione del risultato*.
### Caratteristiche di un oggetto
Oltre ad avere uno *stato* e delle *funzionalità*, gli oggetti sono anche caratterizzati da:
- **Identità:** identificatore dell'oggett
- **Ciclo di vita:** creati, riferiti, disattivati
- **Locazione** di memoria
## Altri concetti del paradigma Object-Oriented
- **Incapsulamento:** lo stato di un oggetto non è accessibile agli altri oggetti
- **Astrazione:** ragionare sul comportamento di un oggetto senza conoscerne la rappresentazione interna
- **Ereditarietà:** come un oggetto può fare proprie le funzionalità di un altro oggetto, ad esempio *estendendolo*
- **Principio di sostituzione:** quando un oggetto può essere *usato al posto di un altro* in maniera trasparente e controllata
- **Poliformismo:** come un oggetto può *processare* altri oggetti indipendentemente anche di *"tipi" diversi*
### Strutture linguistiche
Dal punto di vista dei *costrutti logici*, i linguaggi di programmazione supportano concetti Object-Oriented seguendo due approcci principali: **Object-based** (JS, Self, Lua, ...) e **Class-based** (C++, C#, Java,...)
#### Object-Based
Gli oggetti vengono trattati nel linguaggio in modo *simile ai record*. I *campi* (o membri/proprietà/variabili) possono essere associati a funzioni. Una funzione in un oggetto (= *metodo*) può accethere ai campi dell'oggetto stesso tramite il riferimento `this.`
|                                                                                                                                                                                                                                                                           **Pro** | **Contro** |
| -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:| ------ |
| COnsente al programmatore di lavorare con gli oggetti in modo *flessibile*, non è necessario scrivere il codice della classe prima di creare un oggetto e si può creare tante variabile di un oggetto (es. con metodi diversi) senza bisogno di scrivere tante classi diverse | Rende *dificile predirre* con precisione quello che sarà il *tipo* di un oggetto, la struttura dell'oggetto può cambiare a tempo di esecuzione e *ostacola i controlli di tipo statici*       |
#### Class-Based
Un linguaggio *class-based* prevede un concetto di *classe* a cui rispondono determinati costrutti linguistici. Una *classe definisce* il contenuto (variabili + metodi) degli oggetti di un certo *tipo*. Gli oggetti vengono *creati* successivamente come *istanze* di una certa classe.
|                                                                                                                                                                                                                                                                                   Pro | Contro |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:------ |
| Richiede al programmatore una maggiore *disciplina*, deve implementare le classi prima di creare gli oggetti. Consente di fare *controlli di tipo statici* sugli oggetti, il tipo di un oggetto sarà legato alla classe da cui è stato istanziato e prende il nome di *normal typing* | É una *scelta di design* del linguaggio di programmazione, dipende da che tipo di utilizzo ci si aspetta sia fatto del linguaggio di programmazione in questione       |
### Inheritance e Subtyping
La scelta tra *object-based* e *class-based* ha un impatto significativo sui meccanismi di ereditarietà e sottotipatura del linguaggoi
### Inheritance (Ereditarietà)
L'*ereditarietà* è una funzionalità realizzata tramite opportuni *construtti linguistici* che consente di definire una classe (o, più in generale, una tipologia di oggetti) sulla base di un'altra esistente. 
I linguaggi *object-based*, per ogni oggetto mantengono una *lista di prototipi*, che sono tutti gli oggetti da cui esso eredita funzionalità, la gestione dei prototipi nei programmi diventa rapidamente piuttosto complessa.
I linguaggi *class-based*, consentono di definire una classe come *estensione* di un'altra. La nuova classe *eredita* tutti i membri (valori e metodi) della precedente, con la possibilità di *aggiungere* altri (o ridefinirne alcuni, *orverriding*)
### Subtyping
I meccanismi di inheritance (e in generale il fatto di avere oggetti che sono l'uno una "estensione" dell'altro) inducono nozioni di *sottotipo tra oggetti*. Idealmente, un oggetto $B$ che è estensione di un altro oggetto $A$ dovrebbe poter essere usato dovunque si possa usare $A$. Un oggetto che descrive uno studente dovrebbe poter essere usato ovunque sia richiesto un oggetto che descrive genericamente una persona, ad es. il tipo `studente` dovrebbe essere un *sottotipo* di `persona`.
#### Structural Subtyping
I linguaggi *object-based* solitamente usano una nozione di **subtyping strutturale:** un oggetto $B$ è un sottotipo di un oggetto $A$ se *contiene almeno tutti i membri "pubblici"* che sono presenti anche in $A$
#### Nominal Subtyping
I linguaggi *class-based* solitamente usano una nozione di **subtyping nominale:** Il *tipo di un oggetto* corrisponde alla *classe* da cui è stato istanziato, il nome della classe diventa il nome del tipo. Un tipo-classe $B$ è un sottotipo di un tipo-classe $A$ se la classe $B$ è stata definita (*sintatticamente*) come *estensione* della classe $A$ (vale la proprietà transitiva: se $C$ è stottotipo di $B$ e $B$ è sottotipo di $A$, allora $C$ è sottotipo anche di $A$)
|                                                                                                                                                                              **Structural Subtyping** | **Nominal Subtyping** |
| -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:| --------------------- |
| É più *flessibile*, non è necessario dire esplicitamente chi estende chi. Favorisce il *polimorfismo* (la relazione di sottotipo è più debole, quindi ci sono più oggetti l'uno sottotipo dell'altro) | É più *rigoroso* esplicitando i vincoli del programmatore. Mette in relazione di sottotipo solo classi che il *programmatore* ha esplicitamente dichiarato essere in questa relazione (con `extends`). É *più semplice da verificare* per l'interprete (deve solo controllare se una classe è nominata nella lista degli antenati dell'altra, non serve un confronto del contenuto delle due classi)                      |
