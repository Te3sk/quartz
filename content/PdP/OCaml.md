# OCaml
```toc
```
##### 2022/10/18 #date 
## Tipi
### Tipi di variabili
```ocaml
let id x = x;;
```
A questo programma viene assegnato il tipo $a\to 'a$
L'istanziazione non viene fatta implicitamente, ma viene fatta da OCaml quando gli si passa un argomento
```ocaml
let id int.x = x;;
```
se si vuole avere l'informazione di tipo si può specificare il tipo PRESUNTO
### Tipo di Funzioni
```ocaml
let scambia (x,y)=(y,x);;
let scambia = fun (x,y)->(y,x);;
```
La prima riga è _zucchero sintattico_ per scrivere la seconda. Il suo tipo sarà quindi __funzione__ ($'a*'b\to'b*'a$).

OCaml cerca sempre di assegnare il __tipo più generico__ possibile. Se quindi avessi modificato
```ocaml
let scambia (x,y) = (y+5, x+5);;
```
avrebbe capito che c'era una tupla e avrebbe creato l'albero relativo alle operazioni ($x+5$, $y+5$). Essendo che $+:int\to int \to int$ allora si registra che $x$ deve avere tipo $int$.
Se ci fosse stato un `if then else(x, ...)`, $x$ avrebbe dovuto avere tipo `bool`
### Tipo di IF THEN ELSE
```ocaml
let maggiore x y = 
    if x>y then x else y;;
```
Questa è una **funzione** di tipo $'a\to 'b\to ?$. Per trovare il tipo devo guardare le regole dell'`if then else`, quindi determinare qual'è il tipo di `x`, di `y` e che `x>y` ha tipo `bool`. Il primo vincolo che noto è che per poter fare `x>y`, `x` e `y` devono avere lo stesso tipo, quindi posso correggere $'a\to'a\to?$, di conseguenza, dato che si restituisce o $x$ o $y$, che hanno lo stesso tipo $$'a\to'a\to'a$$
L'unico tipo certo che inizialmente so, è che `x>y` è di tipo `bool`. OCaml definisce l'operazione `>` come $'a\to'a\to bool$, quindi non indica che tipo passargli e lascia spazio a un errore a run time.
```ocaml
maggiore 5 6;;
maggiore "ciao" "bello";;
maggiore true false;;
maggiore (fun x->x+1) (fun x->x+5);;
```
Il sistema di tipo non previene l'errore se gli passo variabili di un tipo che non va bene, come le _funzioni_
### Variabili di Tipo
In oCaml le variabili di tipo, ci permettono di scrivere espressioni, ad es.
```
bool * (a -> int);;
```
in OCaml va interpretato come se davanti ci fosse un $\forall a$`bool * (a -> int);;`, come se mi devinissi il mio nuovo tipo `a`. Quando Ocaml fa la _type inference_ costruisce l'albero di prova in modo bidirezzionale, OCaml scorre l'albero e cerca il tipo che possa risolvere l'espressioni, se incontra delle variabili sulle qualli non ha informazioni, ci mette delle variabili di tipo, che poi sostituirà, se potrà, continuando a scorrere l'albero.
### Liste
lista $\triangleq$ Sequenza finita e **immutabile** di espressioni dello **stesso tipo**.
```ocaml
[1;2;3];;
[true;true;false];;
["ciao"; "bello"];;
[]
```
La **Lista vuota** ha senso per qualsiasi tipo, è indicata con il tipo generico `'a`
Avendo aggiunto il tipo `list T`f al linguaggio, posso creare liste come
```ocaml
[[2;3];[];[4;5]];;
[[];[fun x->x]];;
```
Se i 2 elementi non hanno lo stess tipo, devo fare **unificazione**, ovvero assegnare ad entrambi lo stesso tipo, il più generico che va bene per entrambi è `'a->'a list`
#### Operatore delle liste
`::`
```ocaml
5::[2;3];;
5::(2::(3::[]));;
```
Le liste si ottengono dalla lista vuota facendo applicazione finite di `const`. Le parentesi associano automaticamente a destra.