# Object-Oriented in OCaml
```toc
```
## Oggetti, classi e tipi oggetto
Un *oggetto* in OCaml è un *valore* costituito da *campi* e *metodi*. Sebbene esistano costrutti linguistici per la definizione di classi, gli oggetti possono essere creati direttamente senza prima specificarne una classe (come in JS). Il **tipo oggetto** è dato dai *metodi* che esso contiene (i campi non influiscono sul tipo).
> [!exp]
> ``` Ocaml
> (*oggetto che realizza uno stack*)
> let s = object
> 	(*campo mutabile che contiene la rappresentazione dello stack*)
> 	val mutable v = [0;2] (*Assumiamo per ora inizializzato non vuoto*)
> 	(*metodo pop*)
> 	method pop = (*nei metodi senza parametri non serve aggiungere ()*)
> 		match v with
> 		|hd :: tl ->
> 			v <- tl; 
> 		(*i campi dell'oggetto sono visibili nei metodi (non serve this)*)
> 			Some hd
> 		|[] -> None
> 	(*metodo push*)
> 	method push hd =
> 		v <- hd :: v
> end;;
> ```
> Uso dell'oggetto `s` (l'invocazione del metodo si fa con `#-notation` invece che con dot-notation)
> ```ocaml
> s#pop ;;
> - : int option = Some 0
> s#pop ;;
> - : int option = Some 2
> s#pop ;;
> - : int option = None
> s#push 9;;
> - : unit = ()
> s#pop ;;
> - : int option = Some 9
> ```
#### digressione: type weakening
> Che succede se nell'oggetto `s` inizializziamo `v` come lista vuota? Che tipo viene inferito all'oggetto

```ocaml
let s = object
	
	val mutable v = []  (*lista vuota*)
	
	method pop = ...
	method push hd = ...
end;
```
**Tipo inferito:** `val s: <pop : '_weak option; push : '_weak -> unit > = <obj>`
Il tipo inferito contiene variabili di tipo, ma non è veramente polimorfo. Benché sia mutabile, la variabile `v` non potrà avere tipi diversi in momenti diversi dall'esecuzione. L'oggetto `s` dovrebbe avere tipo `<pop : t option; push : t -> unit>`, per qualche *tipo concreto* `t`. Questo tipo concreto non è però noto al momento della dichiarazione della variabile mutabile, quindi il type checker *indebolisce temporaneamente* il tipo inferito includendo delle variabili di tipo. Appena possibile (al primo utilizzo) il tipo di `s` sarà *ricalcolato* andando ad *istanziare definitivamente* la variabile provvisoria con un tipo concreto.
> [!exp]
> Uso dell'oggetto con type weakening
> ```ocaml
> s;;
> val s : <pop : '_weak option; push : '_weak -> unit > = < obj >
> s#pop ;;
> - : '_weak option = None
> s#push 5 ;;
> - : int option = None
> s ;;
> val s : < pop : int option; push int -> unit > = < obj >
> s#pop ;;
> - : int option = Some 5
> s#push "chiao"
> 	Error: This expressionhas type string but an expression was 
> 	expected of type int
> ```

## Costruzione di oggetti tramite funzioni
Gli oggetti possono essere costruiti tramite funzioni
```ocaml
(* funzione che costruisce oggetti inizializzati con init *)
let stack init = object
	val mutable v = init
	
	method pop =
		match v with
		| hd :: tl ->
			v <- tl;
			Some hd
		| [] -> None
		
	method push hd =
		v <- hd :: v
end;;

(* la funzione stack è (veramente) polimorfa *)

let stack [3;2;1] ;;
	val s : < pop : int option; push ; int -> unit > = < obj >
s#pop ;;
	- : int option = Some 3

let s = stack [] ;;  (* con [] ancora type weakening... *)
	val s : < pop : '_weak option; push : '_weak -> unit > = < obj >

(* ... ma basta un type annotation del parametro attuale per
forzare l'istanziazione al tipo che preferiamo *)
let s = stack ([]:int list);;
	val s : < pop : int option; push : int -> unit > = < obj >
```
## Polimorfismo di Oggetti
Quando si definisce una funzione che prende un *oggetto come parametro*, il *tipo dell'oggetto* viene *inferito dai metodi* che sono invocati a partire dall'oggetto. Indipendende dal fatto che l'oggetto sia già stato definito o meno.
Il seguente oggetto `quadrato` può essere passato a tutte le funzioni viste
```ocaml
let quadrato = object
	val w = ref 30
	method width = !w
	method color = "red"
	method resize n = w := n
end;;

area : < width : int; ...> -> int
minimize : < resize : int -> 'a; ...> -> 'a
limit : <resize : int -> unit; width : int; ...> -> unit
```
Si applica la regola di **subsumption**
$$
\frac{
	\Gamma\:\:\vdash\:\:e:S\:\:\:\:\:\:S<:T
}{
	\Gamma\:\:\vdash\:\:e:T
}
$$
esattamente come nel caso dei record, abbiamo:
```
< color: string;
  resize : int -> unit;
  width : int >

< width : int >
```
Grazie a cui, nel nostro esempio, possiamo derivare
$$
\Gamma\:\:\vdash\:\:quadrato:\:\:<\text{ width : int }>
$$
e concludere che l'oggetto `quadrato` può essere passato alla funzione `area`. Lo stesso vale per le funzioni `minimize` e `limit`.
### Structural subtyping
La notazione con i *puntini*
```
< width : int, ...>
```
usata dall'interprete di OCaml nell'inferire il tipo del parametro formale di `area` *enfatizza lo structural subtyping*. La funzione accetta un **qualunque sotto-tipo** di `< width : int >`, ossia qualunque oggetto che contiene *almeno* il metodo `width`. I puntini **...** possono essere considerati come una *variabile di tipo* istanziabile con una lista di metodi da aggiungere a `width`.
### Coercion di tipi oggetto
Al di là del passaggio dei parametri a una funzione, ci sono numerose altre situazioni dove il subtyping degli oggetti si rende utile. Supponiamo di definire i seguenti tipi di oggetto (tramite `type`)
```ocaml
type shape = < area : float >
type square = < area : float; width : int >
```
É chiaro che `square` sia un sottotipo di `shape` (contiene dei metodi in più), quindi è lecito pensare che ovunque si possa usare un oggetto di tipo `shape` si possa usare al suo posto un oggetto di tipo `square` ([[|principio di sostituzione]]).
Facciamo una prova... definiamo funzioni costruttore per i due tipi:
```ocaml
(* costruttore di oggetti di tipo shape *)
let shape (a:float): shape = object
	method area = a
end;;

(* costruttore di oggetti di tipo square *)
let square (w:int): square = object
	method area = (float_of_int) (w * w)
	method width = w
end ;;
```
Proviamo ad aggiungere uno `square` ad una lista di `shape`:
```ocaml
let lis1 = [shape 10.0; shape 20.0];;
	val lis1 : shape list = [< obj >; < obj >]

let lis2 = square 5 :: lis1::
	Error: This expression has type shape list but an expression was
	expected of type square list Type shape = < area : float > is not
	compatible with type square = < area : float; width : int > The
	first object type has no method width
```
Serve una **type coerction** (conversione di tipo) **esplicita**, tramite l'operatore `:>`
```ocaml
let lis2 = (square 5 :> shape) :: lis1 ;;
	val l2 : shape list = [< obj >, < obj >, < obj >]
```
La type coercion `e :> t` forza il type checker a trattere l'espressione `e` come se fosse di tipo `t`, il quale deve essere un tipo più generale (un *supertipo*, con metodi in meno) del tipo vero di `e`.
### Polimorfismo di oggetti VS principio di sostituzione
I due concetti di **polimorfismo di oggetti** e di **principio di sostituzione**, sebbene tra loro collegati, vengono *trattati in OCaml in due modi diversi* (per il ssecondo è richiesta la type coercion esplicita). Questo sempre perché il type checker di Ocaml, come già visto con i tipi primitivi, *non effettua conversioni di tipo implicite*.
### OCaml VS Java
Vedremo che in *Java* sarà possibile fare delle *type coercions da un supertipo a un sottotipo* (es. trasformare uno `shape` in uno `square`). Questo però sarà reso possibile tramite *controlli dinamici di tipo* svolti a runtime dall'interprete della JVM (resi più facili dal *nominal subtyping*)
## Classi (e costruttori linguistici per l'ereditarietà)
Abbiamo visto che OCaml consente di lavorare direttamente con gli oggetti (in stile [[Object Oriented Intro#Object-Based|object-based]]). Uno dei meccanismi di forza della programmazione OO sono queli legati all'*ereditarietà*, abbiamo visto in JS che realizzare meccanismi di ereditarietà lavorando direttamente con gli oggetti richiederebbe di usare tecniche tipo i *prototipi*, il cui *funzionamento è complicato*. Per questo *OCaml* introduce anche dei costrutti di tipo **classe**
### Classi
Intuitivamente, una classe è la "ricetta" che descrive come creare oggetti di un certo tipo, una classe si definisce con `class` e si istanzia con `new`
```ocaml
class istack = object (* classe per stack di interi *)
	val mutable v = [0;2] (* inizializzato non vuoto *)
	method pop =
		match v with
		| hd :: tl ->
			v <- tl;
			Some hd
		| [] -> None
	method push hd =
		v <- hd :: v
end;;


let s = new istack ;;
	val s : istack = < obj >
s#pop ;;
	- : int option = Some 0
```
### Classi parametriche e polimorfe
Una classe può prevedre *parametri*
- *di costruzione* (es. `init`) che vanno passati al momento dell'istanziazione
- *di tipo* (es. `'a`) che la rendono polimorfa
```ocaml
class ['a] stack init = object (* classe polimorfa per stack *)
	val mutable v : 'a list = init (* init è parametro costruttore *)
	method pop = 
		match v with
		| hd :: tl ->
			v <- tl;
			Some hd
		|[] -> None
	method push hd = v <- hd :: v
end;;

let s = new stack ["pippo"] ;;
	val s : string stack = < obj >
s#pop ;;
	- : string option = Some "pippo"
```
### Classi e tipi di oggetto
La definizione di una classe introduce anche un tipo con lo stesso nome, si tratta però solo di un *alias* del tipo-oggetto che si otterrebbe costruendo gli oggetti direttamente
### Inheritance (ereditarietà)
L'*ereditarietà* è una funzionalità realizzata tramite opportuni *costrutti linguistici* che consente di definire una classe (o, più in generale, una tipologia di oggetti) sulla base di un'alatra esistente. I linguaggi [[Object Oriented Intro#Class-Based|class-based]], consentono di definire una classe come *estensione* di un'altra. La nuova classe *eredita* tutti i membri (valori e metodi) della precedente, con la possibilità di *aggiungerne* altri (o ridefinirne alcuni, *overriding*).
> [!exp]
> ```ocaml
> (* classe per stack di int che raddoppia i valori inseriti * )
> class double_stack init = object
> 	(* super è l'oggetto da estendere in fase di istanziazione * )
> 	inherit [int] stack init as super
> 	
> 	method push hd =      ( * ridefinisce un metodo * )
> 		super#push ( hd * 2 )
> end;;
> ```

## Recap
OCaml *combina* costrutti linguistici tipicamente [[Object Oriented Intro#Object-Based|object-based]] con costrutti tipicamente [[Object Oriented Intro#Class-Based|class-based]]. I costruttori *object-based* favoriscono la definizione di *object-types* e lo *structural subtyping*, la *non modificabilità della struttura degli oggetti* consente di effettuare *controllo di tipo* a tempo di compilazione. I costrutti *class-based* favoriscono una naturale definizione di meccanismi di *ereditarietà*.