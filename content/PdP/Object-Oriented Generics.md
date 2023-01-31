# Object-Oriented Generics
```toc
```
##### 2022/11/23 #date 
Consideriamo i due codici
```java
class CircleQueue {   //coda di cerchi
	
	//rappresentazione come array
	private Circle[] circles;
	
	//costruttore
	public CircleQueue(int size) {...}
	
	//metodi
	public boolean isFull() {...}
	public boolean isEmpty() {...}
	public void enqueue (Circle c) {...}
	public Circle dequeue() {...}
}
```

```java
class PointQueue {   //coda di punti
	
	//rappresentazione come array
	private Point[] circles;
	
	//costruttore
	public PointQueue(int size) {...}
	
	//metodi
	public boolean isFull() {...}
	public boolean isEmpty() {...}
	public void enqueue (Point c) {...}
	public Point dequeue() {...}
}
```
> [!def] Astrazione
> Where similar functions are carried out by distinct pieces of code, it is generally beneficial to combine them into one by abstracting out the varying parts

Usiamo una terza coda
```java
class ObjectQueue {   //coda di oggetti
	
	//rappresentazione come array
	private Object[] circles;
	
	//costruttore
	public ObjectQueue(int size) {...}
	
	//metodi
	public boolean isFull() {...}
	public boolean isEmpty() {...}
	public void enqueue (Circle c) {...}
	public Object dequeue() {...}
}


ObjectQueue cq = new ObjectQueue(10);
cq.enqueue(new Circle(new Point(0,0), 10));
cq.enqueue(new Circle(new Point(1,1), 5));
```
#### Altre Operazione
Estrarre dalla coda un oggetto instanza di Circle è leggermente più complicato
```java
Circle c = (Circle)cq.dequeue();
```
> [!dan]
> ```java
> Circle c = cq.dequeue();
> ```
> *compilation error:* non possiamo assegnare un'espressione di tipo `Object` a una variabile di tipo `Circle` senza un'operazione di casting esplicito

## Java Generics
Sono un meccanismo di astrazione linguistica che consente la *definizione di classi e metodi parametrici rispetto al tipo che utilizzano*. Un'astrazione che permette di definire algoritmi che si applicano in contesti diversi, ma in cui l'interfaccia e il funzionamento generale e l'implementaizone possono essere definiti in modo tale da applicarsi indipendentemente dal contesto dell'appliczione.
#### Variabili di tipo
```java
//<T> nella prima riga è la DICHIARAZIONE
class NewSet <T> implements Set <T> {
	// non-null, contains no duplicates
	// ...
	List<T> theRep;      //utilizzo
	T lastItemInsered;   //utilizzo
	...
}
```
#### Dichiarare generici
```java
class Name <TypeVar1, ..., TypeVarN> {...}

interface Name <TypeVar1, ..., TypeVarN> {...}
```
**Convenzioni standard:**
- `T` per `Type`
- `E` per `Element`
- `K` per `Key`
- `V` per `Value`
- ...
Instanziare una classe generica significa fornire un valore di tipo `Name<Type1, ..., TypeN>`
#### Lista generica
```java
class GenList<T> implements Collection<T> {
	int size = 0;
	Node<T> head = null; //reference to first Node;
						 //null if list is empty
	boolean contains (T x) {
		Node<T> n = head;
		while (n != null) {
			if (x.equals(n.data)) return true;
			n = n.next;
		}
		return false;
	}
}
```
### Perche i generics in Java sono covarianti?
Se Java ammettesse la covarianza sui tipi generici, il seguente frammento sarebbe corretto:
