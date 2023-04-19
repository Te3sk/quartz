# Il Principio di Sostituzione di Liskov
```toc
```
##### 2022/11/22 #date 
## Fasi della progettazione e dello sviluppo
Idealmente, la *progettazione e lo svilupppo* di un programma Java dovrebbe attraversare le seguenti fasi:
1. **Definizione** della *gerarchia di classi* e interfacce da realizzare
	> Capire che classi fare
2. **Identificazione** dei *membri pubblici* di ogni classe
	> Capire cosa fa ogni classe
3. **Definizione** delle *specifiche* di ogni metodo pubblico (condizioni sui parametri, sul risultato e comportamento atteso del metodo)
	> Specifiche = condizioni sull'input (parametri) perché il metodo venga invocato e le condizioni sull'output desiderato
4. **Implementazione** di *programmi di testing* delle singole classi sulla base di quanto definito (membri pubblici e specifiche)
	> così ci si assicura che i test si basino sulle proprietà che i metodi devono avere e non sull'implementazioni
5. **Definizione** dei *membri privati* seguendo il *principio di incapsulamento*
	> Definire tutto ciò che è privato facendo attenzione all'incapsulamento
6. **Implementazione** del *codice delle classi*, da verificare con i test già sviluppati
	> Implementazione vera e propria, con esecuzione di test
## Definire le specifiche
> [!def] **Definire le specifiche**
> di una classe significa *esprimere in modo non ambiguo il comportamento atteso* dei suoi metodi pubblici
- Si aggiungono opportuni *commenti* al codice
- Esprimendo delle *condizioni* sui parametri e sulle variabili che siano *verificabili* o *testabili*, ad esempio, con `assert`.
Per ogni metodo, le specifiche indicano:
- **PRECONDIZIONI:** condizioni sui parametri sul valore delle variabili che devono essere *soddisfatte*
- **POSTCONDIZIONI:** condizioni sul risultato e sul valore delle variabili che devono essere *soddisfatte al termine dell'esecuzione* del metodo, *assumendo* che all'inizio fossero *vere le precodizioni*
$$
\begin{array}{c}
	\textcolor{#2e80f2}{\underline{\text{Logica di Hoare}}}\\
	\{\varphi\}P\{\psi\}\\
	\text{dove }\varphi\text{ è la PRE e }\psi\text{ è la POST}\\ \\
	\frac{
		\Large{s\vDash \varphi\:\:\:\:\:<s,P>\Rightarrow<s',->}
	}{
		\Large{s'\vDash \psi}
	}
\end{array}
$$
### Verifica delle specifiche
I **test** consentono di *identificare errori* di programmazione (rispetto alle specifiche)
- *non sono esaustivi* (spesso non possono testare tutti i casi possibili)
- esistono strumenti di *generazione automatica di test* (dalle specifiche) che consentono di aumentare la copertura dei casi da provare
Esistono metodi di *analisi statica* e *model checking* che consentonon di verificare (dimostrare) in modo automatico la correttezza del codice rispetto alle specifiche. Necessari in casi di programmi eseguiti in contesti *safety-critical* e *cybersecurity*
## Principio di Sostituzione di Liskov
L'esistenza di una specifica consente di ragionare sul *rapporto tra superclasse e sottoclasse* da un *punto di vista comportamentale*
> [!def] Principio di Sostituzione di Liskov (LSP)
> Un oggetto di un *sottotipo* può essere sostituire un oggetto del *supertipo*, *senza influire sul comportamento* dei programmi che usano il supertipo

La sottoclasse deve soddisfarre le specifiche della superclasse
$$
\begin{array}{c}
\frac{
	\Large{X<Y\:\:\:\:\:\:\{PRE\}c[Y]\{POST\}}
}{
	\Large{\{PRE\}c[X]\{POST\}}
}
\end{array}
$$
$c[X]$ e $c[Y]$ vuol dire che non mi basta *controllare* i due tipi, ma *tutto il codice che li usa*.
##### 2022/11/23 #date 
### Regola dei metodi
> [!def] La regola dei metodi
> Le chiamate dei metodi del sotto-tipo devono comportarsi come le chiamate dei corrispondenti metodi del super-tipo

In generale, *un sotto-tipo può indebolire le precondizioni e rafforzare le post-condizioni* dei metodi che sono riscritti.
Per avere compatibilità tra la specifica di un metodo `m` del super-tipo e la specifica del metodo `m` del sotto-tipo, devono essere soddisfatte
- **Regola della pre-condizione:** $pre_{super}\implies pre_{sub}$
- **Regola della post-condizione:** $(pre_{super}\:\&\&\:post_{sub})\implies post_{super}$
### Regola della proprietà
> [!def] Regola della proprietà
> Il sotto-tipo deve preservare tutte le proprietà che possono essere provate sugli oggetti del super-tipo

*Ogni ragionamento* sulle proprità degli oggetti basato sul super-tipo è *ancora valido* quando gli oggetti appartengono al sotto-tipo. Le proprietà sono *degli oggetti*, *non dei metodi*, e vengono dal *modello del tipo astratto* (la struttura dati che il modello rappresenta).
#### Verificare la regola della proprietà
Per mostrare che un sotto-tipo soddisfa la regola della proprietà dobbiamo *mostrare che preserva le proprietà del super-tipo*.
> [!exp]
> *Proprietà invariante* di IntSet: gli insiemi rappresentati hanno sempre elementi tutti diversi da loro
>```java
> public class FlexIntSet extends IntSet {
> 	//protected int[] a; //EREDITATI
> 	//protected int size;
> 	
> 	public FlexIntSet (int capacity) { super(capacity); }
> 	
> 	public boolean add(int elem) {
> 		if (size >= a.length) {
> 			int[] tmp = new int[size+10];
> 			for (int i = 0; i < size; i++) {
> 				tmp[i] = a[i];
> 			}
> 			a = tmp;
> 		}
> 		if (contains(elem)) return false;
> 		else { a[size] = elem; size++; return true; }
> 	}
> 	//public boolean contains(int elem) //EREDITATI
> }
> ```
> Mostriamo che il sotto-tipo `FlexIntSet` verifica questa proprietà di `IntSet`:
> - Il *costruttore* crea un array vuoto che rappresetna un insieme vuoto (elementi diversi *ok*)
> - Il *metodo* `add` aggiunge elementi solo se non presenti (elementi tutti diversi *ok*)
> - il *metodo* `contains` non modifica l'array (elementi tutti diversi *ok*)

### Invarianti e Incapsulamento
La possibilità di dimostrare proprietà invarianti si basa fortemente sulla **proprietà di incapsulamento** della classe: La *rappresentazione deve essere privata*, altrimenti tra una chiamata di metodo e l'altra un esterno potrebbe modificarla violando l'invariante
> [!exp]
> Se l'array che rappresenta l'insieme fosse pubblico, dall'esterno si potrebbe aggiungere due copie dello stesso elemento (accedendo direttamente all'array)

> [!dan]
> Quando lavoriamo *variabili d'istanza di tipo non primitivo* (oggetti) è comune commettere un *errore di programmazione* che porta ad *esporre la rappresentazione*, anche se privata
