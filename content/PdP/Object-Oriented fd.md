---
tags: PDP
alias: []
links:
data: 2021-11-22-09-20
notes:
---

# Object Oriented fd
```toc
```
## Composizione

- Stato
 - Rappresentato da un gruppo di attributi proprietà e variabili
 - Incapsulamento Stato di un oggetto non accessibili dagli altri
 - Non ci deve essere modo di leggere/modificare le variabili di *A* da un oggetto *B*
- Funzionalità
 - Rappresentate da Metodi e Funzioni
 - Sono messe a disposizione di altri oggetti
 - Descrivono il COMPORTAMENTO dell’oggetto
- Cooperazione
 - Scambio di messaggi
 - Deve esserci un sistema runtime che gestisce il CICLO DI VITA di un oggetto

## Concetti

- Incapsulamento
- Astrazione
- Interfaccia
- Ereditarietà
- Principio di sostituzione
 - Un oggetto A può essere usato al posto di un ogetto B In maniera trasparente e controllata
- Poliorfismo

## Object Based

Definizione diretta di un oggetto
*Javascript, lua, etc*

> Record con metodi

Oggetto javascript con stato e funzionalità

javascript
let mario = {
 nome: "Mario",
 eta: 35,
 
 compleanno: function(){
  this.eta ++;
 }
}

Molto dinamico e flessibile E’ difficile predire di che tipo sarà l’oggetto dopo la creazione

## Class Based

Una classe definisce la struttura che determina la composizione di tutti gli oggetti istanze di quella classe
*Java, C#, etc*

javascript
class Persona {
 constructor(n, e){
  this.nome = n;
  this.eta = e;
 }
 
 compleanno(){
  this.eta++
 }
}

Maggiore disciplina Controllo di tipo statico

> il tipo di un ogetto sarà legato alla classe da cui deriva

## Ereditarietà
Realizzato tramite costrutti linguistici
Definizione di una classe sulla base dell’esistenza di un’altra

### Object base
Sistema a prototipi

> Ogni oggetto mantiene lista di prototipi

Ogni livello in più di ereditarietà richiede di passare una mole maggiore di prototipi
aumento di errori e attenzioni
### Class Base

> Estendiamo una calsse introducento, ereditando, sovrascrivendo nuove funzionalità

Dall’**istanza di classe figlio si accede** a tutti i metodi (se non sovrascritti) della calsse padre

> Un oggetto figlio può stare in tutti i posti dove può stare il padre

Particolarità

Da quante classi padre posso ereditare?

Con più classi padre posso avere confitti di metodi con la stessa firma presenti in più padri

> L’ereditarietà singola risolve!
 
## Sottotipo
### Strutturale

Usata spesso in Object Base Languages

> Un oggetto B è sottotipo di A se ==contiene almeno tutti i membri pubblici di A==

Più FLESSIBILE Fornisce polimorfismo

### Nominale

Usata spesso in Class Base Languages

Il tipo di un oggetto è il nome della classe di cui è instanza

> Un oggetto B è sottotipo di A se è ==sintatticamente definito come estensione== della classe A, la proprietà è transitiva: ==C ← B ← A== allora ==C è sottotipo di A==

Più RIGIDO Permette migliori controlli dell’interprete