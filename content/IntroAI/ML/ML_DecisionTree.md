---
title: ML: Decision Tree
alias:
- 
tags: 
- #introai
- #ML
---
# Potere espressivo
L'<span style="color:#ff82b2"><b>albero di decision</b></span> rappresenta un disgiunzione di congiunzioni di costrutti sul valore degli attributi.
![esempio](IntroAI/assets/pictures/ML_DT_start.png)
Lo spazio delle ipotesi $\textcolor{#ff82b2}{H}$ è capace di esprimere qualunque funzione finita a valori discreti
## Top-Down
<span style="color:#ff82b2"><b>ID3</b></span> è un algoritmo base per l'apprendimento dei Decision Tree. Dato un insieme di esempi di training, l'algoritmo esegue la <span style="color:#ff82b2"><i>ricerca nello spazio</i></span> del DT. La costruzione dell'albero è <span style="color:#ff82b2"><b>top-down</b></span>, l'algoritmo esegue una ricerca <span style="color:#ff82b2"><b>greedy</b></span>:
1. Seleziona il miglior attributo
2. Viene poi creato un nodo discendente per ogni possibile valore dell'attributo e gli esempi sono partizionati in base a quel valore
3. Il processo si ripete per ogni nodo successore finché tutti gli esempi sono classificati bene o finché non rimangono più attributi
```
ID3 (X, T, Attrs)
	create Root node
	IF tutti gli X sono + RETURN Root con classe +
	IF tutti gli X sono - RETURN Root con classe -
	IF Attrs è vuoto RETURN Root con la classe più comune di T in X
	ELSE
		A <-- miglio attributo, attributo di decisione per Root <-- A
		FOR EACH possibile valore v_i di A
			aggiungi un nuovo ramo sotto Root, per testare a = v_i
			X_i <-- sottoinsieme di X con A = k
			IF X_i è vuoto THEN aggiungi una nuova fogli acon la classe più comune di T in X
			ELSE aggiungi il sottoalbero generato da ID3(X_i, T, Attrs - {A})
	RETURN Root
```
dove `X` sono gli esempi di training, `T` il target attribute e `Attrs` gli altri attributi (inizialmente tutti).
### Selezione del miglio attributo
#### Entropia
Usiamo la nozione di <span style="color:#ff82b2"><b>entropia</b></span>, che misura l'impurità di un insieme di esempi e dipende dalla distribuzione della variabile random $\textcolor{#ff82b2}{p}$.
> [!quote] Entropia
> 
> $$
> \begin{array}{c}
> 	\textcolor{#ff82b2}{
> 		Entropy(S)=p_+log_2p_+-p_-log_2p_-
> 	}
> 	\\
> 	\textcolor{#9172dd}{\to} Entropy([14+,0-])=-14/14 log_2(14/14)-0log_2(0)=0 
> \end{array}
> $$

$\textcolor{#ff82b2}{S}$ è l'insieme di esempi di training, $\textcolor{#ff82b2}{p_+}$ e $\textcolor{#ff82b2}{p_-}$ sono la porzione di esempi positivi e negativi in $\textcolor{#ff82b2}{S}$.
Il guadagno di informazioni è la riduzione attesa di entropia causata dalla partizione degli esempi su un attributo. Ci sono $\textcolor{#ff82b2}{Values(A)}$ valori possibili per $\textcolor{#ff82b2}{A}$.
$$
\textcolor{#fc0202}{
	\boxed{
		\textcolor{#ff82b2}{
			Gain(S,A)=Entropy(S)-\sum_{v\in Values(S)} {\large\frac{|S_V|}{|S|}} Entropy(S_V)
		}
	}
}
$$
## Information Gain
$\textcolor{#ff82b2}{S_V}$ è un sottoinsieme di esempi di $\textcolor{#ff82b2}{S}$ per i quali $\textcolor{#ff82b2}{A}$ ha valore $\textcolor{#ff82b2}{v}$. Maggiore è il guadagno, maggiore è l'efficienza dell'attri  buto nella classificazione dei dati di training
#### Perché si usa
L'entropia misura l'omogeneità (anzi l'impurità) della classe del sottoinsieme di esempi, quindi si <span style="color:#ff82b2"><i>seleziona un</i></span> $\textcolor{#ff82b2}{A}$ che <span style="color:#ff82b2"><i>massimizzi</i></span> $\textcolor{#ff82b2}{Gain(S, A)}$. Dopo la suddivisione ci sono valuri più bassi (target più omogenei) in ogni sottoinsieme (<span style="color:#ff82b2"><b>maggior guadagno</b></span>).
Lo scopo è di <span style="color:#ff82b2"><i>separare gli esempi</i></span> sulla base del target, trovare gli attributi che discriminano gli esempi che appartengono a diversi target $\textcolor{#9172dd}{\text{( ad es.}} \text{ tutti i positivi a sinistra, tutti i negativi a destra}\textcolor{#9172dd}{)}$.
> [!example]
> 
> | ![albero1](IntroAI/assets/pictures/ML_informationGain_exp1.png)                                                                              | ![albero1](IntroAI/assets/pictures/ML_informationGain_exp1.png) |
| -------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- |
> | $$\begin{array}{l}\textcolor{#ff82b2}{Gain(S,Humidity)=}&\\\begin{array}{}\textcolor{#ff82b2}{=9.40-}\textcolor{#ff82b2}{(\frac{7}{14}).985-(\frac{7}{14}).592= \textcolor{#9172dd}{\boxed{\textcolor{#ff82b2}{.151}}}}\end{array}\end{array}$$ | $$\begin{array}{l}\textcolor{#ff82b2}{Gain(S,Wind)=}\\\begin{array}{}\textcolor{#ff82b2}{.940-(\frac{8}{14}).811 - (\frac{6}{14})1.0=0.48}\end{array}\end{array}$$                                                               |

#### Problemi
Il guadagno di informazioni fornisce gli attributi con molti valori possibili. Si consideri <span style="color:#9172dd"><i>ad esempio</i></span> gli attributi $\textcolor{#ff82b2}{Date}$ nell'esempio $\textcolor{#ff82b2}{PlayTennis}$:
	$\textcolor{#9172dd}{\to}$ $\textcolor{#ff82b2}{Date}$ ha il massimo guadagno di informazioni: ogni giorno corrisponde a un diverso sottoinsieme puro: $\textcolor{#ff82b2}{[1+,0-]}$ o $\textcolor{#ff82b2}{[0+,1-]}\implies\textcolor{#ff82b2}{0\text{ entropy}}$
	$\textcolor{#9172dd}{\to}$ Fit (separazione) perfetta dei dati di training
	Non è significativo: <span style="color:#ff82b2"><i>inutile per i dati non visti</i></span>

## Misura alternativa: Gain Ratio
> [!quote] Gain ratio
> 
> $$
> \textcolor{#fc0202}{\boxed{
> 	\begin{array}{}
> 		\textcolor{#ff82b2}{GainRatio(S,A)=
> 		{\Large\frac{
> 			Gain(S, A)
> 		}{
> 			SplitInformation(S,A)
> 		}}}
> 		\\ \\
> 		\textcolor{#ff82b2}{SplitInformation(S,A)=
> 			-\sum_{i=1}^c{\Large\frac{|S_i|}{|S|}}
> 			log_2{\Large\frac{|S_i|}{|S|}}
> 		}
> 	\end{array}
> }}
> $$
> $\textcolor{#ff82b2}{S_i}$ sono gli insiemi ottenuti dalla partizione sui valori $\textcolor{#ff82b2}{v_i}$ di $\textcolor{#ff82b2}{A}$ fino a $\textcolor{#ff82b2}{c}$ valori

$\textcolor{#ff82b2}{SplitInformation}$ musira l'<span style="color:#ff82b2"><b>entropia</b></span> di $\textcolor{#ff82b2}{S}$ rispetto ai <span style="color:#ff82b2"><i>valori di</i></span> $\textcolor{#ff82b2}{A}$. Più la dispersione dei dati è uniforme, maggiore è il suo valore,.
$\textcolor{#ff82b2}{GainRatio}$ penalizza gli attributi che separano gli esempi in molte classi minori come $\textcolor{#ff82b2}{Date}$
### Regolarizzazione
   <span style="color:#ff82b2"><b><u>Problema</u></b></span> $\textcolor{#ff82b2}{SplitInformaition(S,A)}$ può essere vuoto ($\emptyset$) o molto piccolo quadno $\textcolor{#ff82b2}{|S_i|\approx|S|}$ per qualche valore $\textcolor{#ff82b2}{i\text{ }[log 1 = 0]}$.
Per mitigare questo effetto viene usata la seguente <span style="color:#ff82b2"><b>euristica</b></span>:
1. Calcolare $\textcolor{#ff82b2}{GainRatio}$ per ogni attributo
2. Applicare $\textcolor{#ff82b2}{GainRatio}$ solo per gli attributi con $\textcolor{#ff82b2}{Gain}$ sopra la media
# Ricerca nello spazio delle ipotesi in DT Learning
La ricerca in *HP Space*, ricerca (con <span style="color:#ff82b2"><i>Hill-climbing</i></span>) nello spazio dei possibili DT dal più semplice fino al più complesso, riferito all'algoritmo precedente (<span style="color:#ff82b2"><i>Cand.Elimin</i></span>):
- Lo spazio delle ipotesi è completo (rappresenta tutte le funzioni o valori discreti)
- La ricerca mantiene una singola ipotesi alla volta
- Non torna indietro e non garantisce l'ottimalità
- Usa tutti gli esempi disponibili
- Può terminare prima
- Accetta classi disturbate
## Bias induttivo in DT learing
$$
\textcolor{#ff82b2}{\text{"Shortest trees are prefered over longer trees"}}
$$
Grazie alla ricerca dal semplice al complesso, incrementale. Inoltre questo è il *bias* esibito da un semplice *breadh first* che genera tutto il DT e seleziona quello consistente più corto.
$$
\textcolor{#ff82b2}{\text{"Preferes trees that place high information gain attributes close to the root"}}
$$
I DT sono limitati dal rappresentare tutte le informazioni possibili, la <span style="color:#ff82b2"><i>restrizione</i></span> non è sull'ipotesu ma sulla <span style="color:#ff82b2"><i>strategia di ricerca</i></span>.
### 2 tipi di Bias
| Preferenza o <span style="color:#ff82b2"><b>search bias</b></span> (dato dalla strategia di ricerca) | Restrizione o <span style="color:#ff82b2"><b>lenguage bases</b></span> |
| ----------------------------------------------------------------------------------------------------: | :---------------------------------------------------------------------- |
| ID3 cerca uno spazio delle ipotesi completo, la strategia di ricerca è incompleta                    | Si fa la ricerca del tipo <span style="color:#ff82b2"><b>candidate-elimination</b></span> che cerca uno <span style="color:#ff82b2"><i>spazio delle ipotesi incompleto</i></span>. La strategia è completa                                                                      |

Nell'apprendimento di un modello lineare si usa una <span style="color:#ff82b2"><i>combinazione di bias</i></span>.

$$
\begin{array}{r}
	\text{Il search bias è}\\
	\text{preferibilmente al}\\
	\text{lenguage bias}&
\end{array}
\cases{
	\begin{array}{ll}
		&\text{Nel ML si usano tipicamente}\\
		&\text{\textcolor{#ff82b2}{approcci flessibili} (capability universale)}\\
		&\text{dei modelli), senza escludere a priori}\\
		&\text{la funzione target sconosciuta}
	\end{array}
	\\ \\
	\begin{array}{lr}
		&\text{Flessibilità}\textcolor{#ff82b2}{\to}\text{tener cura dei propri problemi}\\
		&\text{di overfitting}
	\end{array}
}
$$
### Ockham's Razor Prefer shorter hypoteses
- <span style="color:#ff82b2"><i>Controllo della complessità del modello</i></span> dalla regoralizzazioene dei sistemi lineari
- Nel ML è importante <span style="color:#ff82b2"><b>razionalizzare</b></span> il modello alla fine
# Problemi nell'apprendimento dei DT
<span style="color:#ff82b2"><b>Overfitting:</b></span>
- interrompere presto
- ridurre gli errori di pruning
- regole post-pruning
<span style="color:#ff82b2"><b>Extensions:</b></span>
- misure alternative per selezionare gli attributi
- attributi continuamente valutati
- gestione degli esempi con valorre degli attributi mancanti
- gestione degli attributi con costi differenti
- miglirare l'efficienza di computazione
## Overfitting
### Definition
Costruire alberi che <span style="color:#ff82b2"><i>"</i></span>si adattano troppo<span style="color:#ff82b2"><i>"</i></span> agli esempi di training può portare a <span style="color:#ff82b2"><b>overfitting</b></span>. Consideriamo l'errore dell'ipotesi $\textcolor{#ff82b2}{h}$ su i dati di training ($\textcolor{#ff82b2}{error_D(h)}$) e sull'intera distribuzione $\textcolor{#ff82b2}{X}$ di dati ($\textcolor{#ff82b2}{error_X(h)}$).
> [!quote] Overfitting
> 
> L'ipotesi $\textcolor{#ff82b2}{h}$ fa <span style="color:#ff82b2"><b>overfitting</b></span> sui dati di training se esiste un'ipotesi $\textcolor{#ff82b2}{h'\in H}$ t.c.
> $$
> \begin{array}{c}
> 	\textcolor{#ff82b2}{error_D(h)<error_D(h')}\\
> 	\textcolor{#fc0202}{AND}\\
> 	\textcolor{#ff82b2}{error_X(h)<error_X(h')}\\
> \end{array}
> $$
> Quindi $\textcolor{#ff82b2}{h'}$ si comporta peggio sui dati di training, ma meglio sui dati sconosciuti

Gli approcci flessibili possono facilmente andare in contro a overfitting se non usati con particolare cura

> image pag.28 of 3.4

> [!example]
> 
> [image pag.29 of 3.4]
> Immaginiamo che questo esempio disturbato causi la divisione del secondo nodo foglia: il DT cresce come la sua complessità

### Evitare l'overfit nei DT
Ci sono 2 strategie:
1. Fermare presto la crescita dell'albero, prima della classificazione perfetta
2. Permettere che l'albero faccia overfit sui dati, poi fare *post-prune*
Valutazione degli effetti:
|                                                                           Training & validation set | Altri approcci                                                                                                            |
| ---------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------------------------------------------- |
| Divide il training set in due (training & validation) e usa il secondo per valutare i punti 1. e 2. | Si usa un test statisstico per stimare gli effetti di espansione / pruning, oppure *minimum description lenght principle* |

### Pruning a errore ridotto
- <span style="color:#ff82b2"><i>Ogni nodo è candidato</i></span> per il pruning
- Il pruning consiste nel <span style="color:#ff82b2"><b>rimuovere sottoalberi</b></span> radicati in un nodo: il nodo diventa una foglia ed è assegnato al classificatore più comune
- Il nodo viene <span style="color:#ff82b2"><i>rimosso</i></span> solo <span style="color:#ff82b2"><i>se l'albero risultante non performa peggio</i></span> sul validation set
- I nodi vengono <span style="color:#ff82b2"><i>potati iterativamente</i></span>: a ogni iterazione il nodo la cui rimozione incrementa di più l'accuratezza sul validation set viene potato
- Il pruning si ferma quando nessun pruning incrementa l'accuratezza
<span style="color:#ff82b2"><i>Effetti del pruning a errore ridotto:</i></span>
> image pag.32 of 3.4

#### Regole post-pruning
1. Creare il DT dal training set
2. Convertire il DT in un insieme di regole equivalenti
	- Ogni <span style="color:#ff82b2"><i>path</i></span> corrisponde a una <span style="color:#ff82b2"><i>regola</i></span>
	- Ogni <span style="color:#ff82b2"><i>nodo</i></span> lungo il path corrisponde a una <span style="color:#ff82b2"><i>pre-condizione</i></span>
	- Ogni <span style="color:#ff82b2"><i>classificazione di foglia</i></span> corrisponde a una <span style="color:#ff82b2"><i>post-condizione</i></span>
$$
\textcolor{#9172dd}{(Outlook=Sunny)\wedge(Humidity=High)\implies(PlayTennis=No)}
$$
3. Potare (generalizzare) ogni regola rimuovendo le precondizioni che non miglirano l'accuratezza
4. Ordinare le regole nell'<span style="color:#ff82b2"><i>ordine dell'accuratezza stimata</i></span>, e considerarle <span style="color:#ff82b2"><i>in sequenza</i></span> quando si classificano nuove istanze
#### Perché si convertono in regole
Dove un attributo continuo $\textcolor{#ff82b2}{A}$, crea dinamicamente un <span style="color:#ff82b2"><i>nuovo attributo</i></span> $\textcolor{#ff82b2}{A_c}$
$$
\textcolor{#ff82b2}{A_c=true\text{ if }A<c, false\text{ otherwise}}
$$
Come si determina il <span style="color:#ff82b2"><i>valore soglia</i></span> $\textcolor{#ff82b2}{c}$? (<span style="color:#9172dd"><b>Esempio:</b></span> $\textcolor{#ff82b2}{Temperature}$ nell'esempio $\textcolor{#ff82b2}{PlayTennis}$)
- Si ordinano gli esempi secondo $\textcolor{#ff82b2}{Temperature}$
$$
\textcolor{#9172dd}{
	\begin{array}{rcc|c|c|c|c|c|c|c|c}
		\textcolor{#ffffff}{Temperature} 
			&& \textcolor{#ffffff}{40} 
			& \textcolor{#ffffff}{48}
			&& \textcolor{#ffffff}{60}
			& \textcolor{#ffffff}{72}
			& \textcolor{#ffffff}{80}
			&& \textcolor{#ffffff}{90}
		\\
		\textcolor{#ffffff}{PlayTennis}
			&& \textcolor{#ffffff}{No}
			& \textcolor{#ffffff}{No}
			&& \textcolor{#ffffff}{Yes}
			& \textcolor{#ffffff}{Yes}
			& \textcolor{#ffffff}{Yes}
			&& \textcolor{#ffffff}{No}
	\end{array}
}
$$
- Si determinano le soglie dei candidati <span style="color:#ff82b2"><i>facendo la media</i></span> dei valori consegutivi che sono in classifica di cambiamento
$$
\begin{array}{ccc}
	{\Large\frac{48+60}{2}}=54
	&&
	{\Large\frac{80+90}{2}=85}
\end{array}
$$
- Si valuta <span style="color:#ff82b2"><i>la soglia dei candidati</i></span> (attributi) secondo il guadagno di informazioni <span style="color:#9172dd"><i>(</i></span> Miglior $\textcolor{#ff82b2}{temperature>54}$ <span style="color:#9172dd"><i>)</i></span>. Ora il nuovo attributo compete con gli altri
## Gestire i dati di training incompleti (imputation)
1. (<span style="color:#ff82b2"><i>più comune</i></span>) Assegnare il valore più in comune tra tutti i dati di training negli esempi al nodo o nella stessa classe
2. Assegnare una probabilità $\textcolor{#ff82b2}{p_i}$ a ogni valore $\textcolor{#ff82b2}{v_i}$, basata sulla frequenza, e assegnare i valori agli attributi mancanti, secondo questa distribuzione di pobabilità (si aggiungono più esempi pesati con la probabilità) $\stackrel{quindi}{\to}$ Si assegna una <span style="color:#ff82b2"><i>frazione</i></span> $\textcolor{#ff82b2}{p_i}$ dell'esempio ad ogni albero discendente
3. Classificare il nuovo esempio allo stesso modo (pesandolo): Viene scelta la <span style="color:#ff82b2"><i>classificazione più probabile</i></span>
## Gestire attributi con costi differenti
Gli attributi dell'istanza possono avere un <span style="color:#ff82b2"><i>costo associato</i></span>: prefiamo i DT chge usano attributi a <span style="color:#ff82b2"><b>basso costo</b></span>. ID3 può essere modificato per <span style="color:#ff82b2"><i>tenere conto del costo</i></span>:
1. Tan & Schlimmer (1990): $\textcolor{#ff82b2}{{\Large\frac{Gain^2(S,A)}{Cost(A)}}}$
2. Numez (1988): $\textcolor{#ff82b2}{{\Large\frac{2^{Gain(S,A)-1}}{(Cost(A)+1)^w}}}\text{ con }\textcolor{#ff82b2}{w\in[0,1]}$
> image pag.40 of 3.4
# Conclusione
- I DT sono un approccio popolare per la <span style="color:#ff82b2"><i>classificazione</i></span> in un discreto numero di classi
	- <span style="color:#ff82b2"><i>Spazio delle ipotesi espressivo</i></span> nell'area delle preposizione (approccio <span style="color:#ff82b2"><i>rule-based</i></span>)
	- Robusti sui dati disturbati
	- <span style="color:#ff82b2"><i>facile da capire</i></span> (regole $\textcolor{#ff82b2}{if-then}$ esplicite)
	- Molte estensioni allo schema base
- <span style="color:#ff82b2"><b>Language</b></span> e <span style="color:#ff82b2"><b>search bias</b></span>: ID3 crea uno spazio delle ipotesi completo, con strategia greedy incompleta
- <span style="color:#ff82b2"><b>Approccio flessibile</b></span>: L'overfitting è un problema importante trovato con <span style="color:#ff82b2"><i>early stopping</i></span>, <span style="color:#ff82b2"><i>post-pruning</i></span> e <span style="color:#ff82b2"><i>generalizzazione</i></span> delle regole indotte