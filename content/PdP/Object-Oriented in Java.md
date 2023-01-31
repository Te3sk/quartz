# Object-Oriented in Java
```toc
```
## Introduzione a Java
É un linguaggio di programmazione nato agli inizi degli anni '90 da un gruppo di lavoro della *Sun Microsystem*, i suoi obiettivi erano:
- *semplificare la programmazione* rispetto a C/C++
- *indipendenza della piattaforma*, lo stesso codice compilato eseguibile su qualunque HD supportato, senza ricompilare
- *modularità* per favorire lo sviluppo collaborativo
- *robustezza* e *sicurezza* con controllli statici e dinamici per prevenire e controllare comportamenti anomali dei programmi
## Approccio
Si è scelto un'approccio di **compilazione + interpretazione**, con un *bytecode* intermedio utilizzabile in *ogni architettura* per cui sia disponibile un interprete, senza ricompilare. Questo consetne controlli *statici* e *dinamici*.
## Struttura di un programma Java
Un programma Java è un **insieme di classi**, idealmente, ognuna di queste va in *un file diverso* e dovrà avere un *metodo* `main` da cui parte l'esecuzione del programma.
### Nucleo imperativo
Il corpo dei metodi è *codice imperativo* con una *sintassi molto simile a C*
- **Espressioni:** come in C, ma con aggiunta di `true` e `false`
```java
(3+4.0)*t/2.0
(x>0)&&(x<=100)
(x==0)||(x!=0)||true
```
- **Comandi:** assegnamenti, blocchi, condizionali e cicli come in C
```java
x = 6
if(x>0){...} else {...}
while (x>0) {...} //do {...} while(...)
for(int i = 0; i < 10; i++){...}
return x
```
- **Dichiarazioni di variabili:**
	- Le *variabili locali* devono essere *dichiarate* e *inizializzate* prima di poterle usare
	- *non è possibile ridichiarare* in un blocco una variabile già dichiarata in un blocco esterno (*no shadowing*)
	- *non esistono variabili globali*
```java 
...//nel corpo di un metodo
int x;
int y=0;
int z;
if (x>0) {       //ERRORE: x non è inizializzata
	z=10;
	int k = z;   //OK: z inizializzata un attimo prima
	char y = 'c' //ERRORE: non si può ridichiarare y
}
```
- **Array:**
```java
public void popola (int[] numeri) {
	for (int i = 0; i < numeri.length; i++) {
		numeri[i]=i;
	}
}
public int somma(int[] numeri) {
	int somma = 0;
	//quando l'indice non serve si può usare un for each
	//for(int n: numeri) somma += n;
	for (int i = 0; i < numeri.length; i++) {
		somma+=numeri[i];
	}
	return somma;
}
```
- **Puntatori:** Non ci sono (ma ci saranno riferimenti agli oggetti). Gli array possono quindi essere acceduti solo tramite l'indice. A *run-time*, prima di accedere ad un array, la KVM controlla che l'indice sia compreso tra 0 e lenght-1, altrimenti solleva un eccezione di tipo `ArrayIndexOutOfBounds`
- **Funzioni:** Si usano i *metodi*, che possono essere ricorsivi, anche mutualmente
```java
public int fattoriale(int n) {
	int res=1;
	if (n==0)
		return res;
	else
		return n * fattoriale(n-1);
}
```
- **Funzioni anonime:** Si possono definire funzioni anonime (*lambda-espressioni*) con la seguente sintassi
```java
x -> espressione
x -> {blocco}
(x, y,...) -> espressione
(x, y,...) -> {blocco}
```
Non essendo però Java un linguaggio funzionale, il funzionamento di queste espressioni è un po' particolare
## Classi
Una classe consiste di **variabili** e **metodi**, e può prevedere uno o più *costruttori* per inizializzare le variabili.
> [!def] Principio di astrazione
> “Each significant piece of functionality in a program should be implemented in just one place in the source code. Where similar functions are carried out by distinct pieces of code, it is generally beneficial to combine them into one by abstracting out the varying parts”

Ogni *tipologia di "entità"* identificabile nel programma come caratterizzata da un *comportamento autonomo* (o un *insieme di comportamenti correlati*) dovrebbe corrispondere a una diversa classe.
Vediamo un **esempio** di programma che fa operazioni su un conto corrente. Consiste di due classi: *banca* che modella la banca e contiene il `main` del programma, e *ContoCorrente* che descrive gli oggetti che rappresentano conti correnti
```java
public class Banca {
	public static void main (String[] args) {
		
		// crea un conto inizializzato con 1000 euro
		ContoCorrente conto1 = new ContoCorrente (1000);
		
		// crea un conto inizializzato con 200 euro
		ContoCorrente conto2 = new ContoCorrente (200);
		
		// si assicura che ci siano 700 euro nel conto1...
		if (conto1.saldo >= 700) {
			//...preleva 700 euro dal primo conto...
			conto1.preleva(700);
			//...e li versa nel secondo
			conto2.versa(700);
		}
		
		System.out.println("Saldo primio conto: " + conto1.saldo);
		System.out.println("Saldo secondo conto: " + conto2.saldo);
	}
}

public class ContoCorrente {
	//variabile che rappresenta lo stato (visibile in tutta la classe)
	public double saldo;
	
	//costruttore (stesso nome della classe e senza tipo di ritorno)
	public ContoCorrente (double saldoIniziale) {
		saldo = saldoIniziale;
	}
	
	// medodi
	public void versa (double somma) {
		saldo += somma;
		System.out.println("Versati: " + somma + " euro");
	}
	public void preleva (double somma) {
		saldo -= somma;
		System.out.println("Prelevati: " + somma + " euro");
	}
}
```
> [!obs]
> Abbiamo usato il metodo `main` di Banca *senza creare oggetti di questa classe*. É un *metodo statico* (per questo c'è il modificatore `static`) ossia può essere richiamato anche senza istanziare la classe.
> La classe Banca *può accedere* a tutti i membri di ContoCorrente (sia alle variabili che ai metodi) perché li abbiamo definiti **pubblici**, potremmo evitarlo usando invece il modificatore `private`.
### Interfaccia pubblica di una classe
Le modifiche che apportiamo ai metodi di una classe *non influenzano l'interazione* con una seconda classe, la quale potrà continuare a chiamare i metodi come faceva prima, perché ne modifichiamo solo il contenuto (l'implementazione). Se non modifichiamo **l'interfaccia pubblica** di una classe (nomi dei metodi e variabili pubbliche, parametri di tali metodi e tipi dei valori di ritorno) possiamo modificare e ricompilare la classe senza compromettere il resto del programma (**composizionalità**).
### Incapsulamento
I modificatori `public` e `private` consentono di realizzare meccanismi di **incapsulamento:** la *rappresentazione dell'oggetto* (le variabili) rimane nascosta (**privata**), l'accesso all'esterno è consentito solo tramite (e sotto il controllo di) un certo numero di **metodi pubblici**, dall'esterno si potrebbe *non conoscere la rappresentazione*. É possibile anche definire **metodi privati** (divantano metodi ausiliari utilizzabili solo all'interno della classe stessa)
## Interfacce
L'interfacca pubblica di una classe può essere esplicitata usando uno specifico costrutto del linguaggio Java, **le interfacce**. Un'interfaccia in Java contiene una specifica *astratta della classe*: indica quali membri pubblici la classe deve contenere, e rende le classi una sorta di *Abstract Data Type(ADT)*.
**Esempio:** 
```java
public class ContoCorrente implements BankAccount { //implements!
	private double saldo;
	public ContoCorrente(double saldoIniziale) {...}
	public void versa (double somma) {...}
	public double getSaldo () {return saldo;}
	public boolean preleva (double somma) {
		if (saldo>=somma) {...}
		else return false;
	}
}
```
Aver aggiunto l'interfaccia non cambia nulla nell'esecuzione del programma. Ora però è **esplicito** che il conto corrente è un *tipo di dato astratto* e la sua *segnatura* è nell'interfaccia. Indicando `implements BankAccount` la classe ContoCorrente è *obbligata* a implementare tutti i metodi dell'interfaccia.
### Implementazioni alternative e Nominal Subtyping
Con più implementazioni alternatice dell'interfaccia `BankAccount`, si può scegliere di volta in volta quale usare. `BankAccount` si può usare come *tipo* e `implementss` implica una relazione di *sottotipo*. Questo è il caso di **Nominal Subtyping:** La relazione di sottotipo nasce dal fatto che le due classi *nominano esplicitamente* l'interfaccia `BankAccount`. Il compilatore controlla anche la *proprietà strutturale* (tutti i membri dell'interfaccia implementati nella classe) ma quello che conta è che ci sia scritto `implements BankAccount` nel codice!
```java

```