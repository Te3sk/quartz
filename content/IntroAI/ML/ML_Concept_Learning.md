---
title: Concept Learning
alias:
- 
tags: #introAI #ML
---
# Definizione
> [!quote] Concept Learning
> 
> Inferire una funzione booleana da esempy di training positivi e negativi
> $$c:X\to\{t,f\}\vee\{yes,no\}\vee\{+,-\}\vee\{0,1\}$$
> dove $X$ è lo spazio delle ipotesi.
> - Un <span style="color:#ff82b2"><i>esempio</i></span> di training ha la forma $<x,c(x)>\in D$
> - $\textcolor{#ff82b2}{h:X\to\{0,1\}}$ soddisfa $\textcolor{#ff82b2}{x}$ se $\textcolor{#ff82b2}{h(x)=1}$
> - Un'ipotesi $\textcolor{#ff82b2}{h}$ è <span style="color:#ff82b2"><i>consistente</i></span> con
> 	- un esempio se $\textcolor{#ff82b2}{h(x)=c(x)\in D}$
> 	- $\textcolor{#ff82b2}{D}$ se $\textcolor{#ff82b2}{h(x)=c(x)}$ per ogni esempio di training $\in D$

# Apprendimento delle funzioni booleane

> [!example] Example: Learning Boolean functions
> 
> ![schema e tabella di verità](IntroAI/assets/pictures/ML_esempio_conceptLearning.jpg)
> É un problema <span style="color:#ff82b2"><b>mal posto:</b></span> potremmo violare la soluzione o la sua esistenza, unicità o stabilità.
> Abbiamo $\textcolor{#ff82b2}{2^{2^4}=2^{16}=65536}$ possibili funzioni su 4 valori in input. Non possiamo sapere quale sia quella corretta finché non le vediamo tutte

In generale ci sono $\textcolor{#ff82b2}{|H|=2^{\#istanze}=2^{2^n}}$, dove $\textcolor{#ff82b2}{n}=\text{dimensione dell'input}$. Cerchiamo di lavore con uno <span style="color:#ff82b2"><i>spazio delle ipotesi ristretto</i></span>: cominciamo scegliendo un $\textcolor{#ff82b2}{H}$ considerabilmente minore dello spazio di tutte le funzioni possibili.

# Regole delle congiunzioni
<span style="color:#2e80f2"><b>Literali possibili</b></span> (in AND): $\textcolor{#ff82b2}{|H|=2^n}$, se consideriamo anche l'operatore NOT: $\textcolor{#ff82b2}{3^n+1}$

# Rappresentare le ipotesi
Nella <span style="color:#ff82b2"><b>rappresentazione delle ipotesi</b></span> consideriamo $\textcolor{#ff82b2}{h}$ una congiunzione di costrutti sugli attributi, ognino di questi può essere un valore
|    Specifico | Trascurabile | Non Accettato |
| ------------:| :------------: |:------------- |
| Water = warm | Water = ?    | Water = $\emptyset$              |
> [!example]
> $$
> \begin{array}{c}
> 
> {\LARGE{<}}
> \begin{array}{cccccc}
> sky & temp & Humid & Wind & Water & Farecast\\
> sunny & ? & ? & strong & ? & same
> \end{array}
> {\LARGE >}
> \Rightarrow
> \\
> \Rightarrow
> sky= \text{sunny }\wedge\text{ wind}=\text{strong }\wedge\text{ forecast}=\text{same}
> \end{array}
> $$
> |                                                                                                                                                  <span style="color:#ff82b2"><b>INPUT</b></span> | <span style="color:#ff82b2"><b>OUTPUT</b></span>                                                                 |
> | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:---------------------------------------------------------------------------------------------------------------- |
> |                                                   <span style="color:#ff82b2"><b>Istanza</b></span> $\textcolor{#ff82b2}{X:}$ Possibili giorni descritti dagli attributi $sky,temp,humidity,...$ | Un'ipotesi $\textcolor{#ff82b2}{h}\in\textcolor{#ff82b2}{H}$ t.c. $\textcolor{#ff82b2}{h(x)=x(x)}\forall x\in X$ |
> |                                                                                                          <span style="color:#ff82b2"><b>Target function:</b></span> $c:EnjoySport$ $X\to\{0,1\}$ |                                                                                                                  |
> | <span style="color:#ff82b2"><b>Ipotesi</b></span> $\textcolor{#ff82b2}{H}$<span style="color:#ff82b2"><b>:</b></span> congiunzione di un insieme finito di letterali $<Sunny,?,?,Strong,?,Same>$ |                                                                                                                  |
> | <span style="color:#ff82b2"><b>Esempi di training:</b></span> $D$: esempi positivi e negativi della funzione target $<x_1,c(x_1)>,...,<x_n,c(x_n)>$                                                                                                                                                                                                 |                                                                                                                  |
> $Sky$: 3 valori, $Temp, Humidity,wind, water, forecast$: 2 valori
> - $\underline{\#\text{istanze distinte}}=3\cdot2\cdot2\cdot2\cdot2\cdot2=96$
> - $\underline{\#\text{concetti distinti}}=2^{\underline{\#\text{istanze}}}=2^{96}$
> - $\underline{\#\text{ipotesi sintatticamente distinte(congiunzioni)}}=5\cdot4\cdot4\cdot4\cdot4\cdot4=5120$
> - $\underline{\#\text{ipotesi semanticamente distinte}}=1+4\cdot3\cdot3\cdot3\cdot3\cdot3=973$

In generale, <span style="color:#ff82b2"><i>strutturare lo spazio di ricerca</i></span> può sempre essere utile queando abbiamo grandi spazi di ricerca (anche infiniti). 
# Ordinamento da generale a specifico
Se consideriamo ad esempio $\textcolor{#9172dd}{2}$ ipotesi:
$$\textcolor{#9172dd}{h_1}=<Sunny,?,?,Strong,?,?> \textcolor{#9172dd}{\to h_2}=<Sunny,?,?,?,?,?>$$
e un'insieme di istanze coperte dalle ipotesi
$$
\textcolor{#9172dd}{h_2}\text{ impone meno vincoli di }\textcolor{#9172dd}{h_1}\text{, quindi classifica più istanze come positive}
$$
> [!quote]  Generalize
> 
> Dati $\textcolor{#ff82b2}{h_1}$ e $\textcolor{#ff82b2}{h_2}$ funzioni a calori booleani definite su $X$, $\textcolor{#ff82b2}{h_j}$ è <span style="color:#ff82b2"><b>più generale</b> o <b>uguale</b></span> a $\textcolor{#ff82b2}{h_k}(h_j\geq h_k)$ sse
> $$
> \forall x \in X:(h_k(x)=1)\implies(h_j(x)=1)
> $$

La relazione $\textcolor{#ff82b2}{\geq}$ impone un ordine parziale (<span style="color:#ff82b2"><i>p.o.</i></span>) sullo spazio delle ipotesi $\textcolor{#ff82b2}{H}$. Possiamo trovare vantaggio <span style="color:#ff82b2"><i>organizzando la ricerca in</i></span> $\textcolor{#ff82b2}{H}$.

# Algoritmo Find-S
> [!quote]  Definizione
> 1. Inizializza $\textcolor{#ff82b2}{h}$ all'ipotesi più specifica di $H$
> 2. Per ogni istanza di training $\textcolor{#ff82b2}{x}$
> 	```
> 	FOR EACH x in h
> 		IF the a_i in h is satisfied by x
> 			THEN do nothing
> 			ELSE replace a_i in the next more general constraing that is staisfied by x
> 	```
> 3. Output dell'ipotesi $\textcolor{#ff82b2}{h}$

| Proprietà                                                                                                                                                                  | Limitazioni                                                                                                                                                                                                                                        |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Lo <span style="color:#ff82b2"><i>spazio delle ipotesi</i></span> è descritto da congiunzioni di attributi                                                                 | Non dice se chi apprende <span style="color:#ff82b2"><i>converge al concetto target</i></span>, nel senso che non può determinare se ha trovato l'<span style="color:#ff82b2"><i>l'unica ipotesi consistente</i></span> con gli esempi di training |
| `Find-S` restituirà l'<span style="color:#ff82b2"><i>ipotesi più specifica</i></span> che mantenga $H$ consistente con gli esempi di training positivi                     | Non dice quando i <span style="color:#ff82b2"><i>dati di training</i></span> sono <span style="color:#ff82b2"><i>inconsistenti</i></span> e ignora gli esempi negativi                                                                             |
| L'ipotesi $\textcolor{#ff82b2}{h}$ restituita sarà <span style="color:#ff82b2"><i>consistente</i></span> con gli esempi negativi (dato che $\textcolor{#ff82b2}{c\geq h}$) | $\fbox{\textcolor{#ff82b2}{no noise tolleration}}$                                                                                                                                                                                                                                                   |

# Version Space
> [!quote] Version space
> 
> Il <span style="color:#ff82b2"><b>Version Space</b></span>, $\textcolor{#ff82b2}{VS_{H,D}}$, rispettando lo spazio delle ipotesi $H$ e il training set $D$, è il <span style="color:#ff82b2"><i>sottoinsieme delle ipotesi </i></span> $\textcolor{#ff82b2}{H}$ <span style="color:#ff82b2"><i>consistenti con tutti gli esempi di training</i></span>
> $$\textcolor{#ff82b2}{VS_{H,D}=\{h\in H | consistent(h,D)\}}$$

L'idea è quella che si restituisca una <span style="color:#ff82b2"><i>descrizione</i></span> dell'insieme di tutti gli $\textcolor{#ff82b2}{h}$ consistenti con $D$, così possiamo <span style="color:#ff82b2"><b>enumerarli</b></span>.
# Algoritmo List-Then Eliminate
> [!quote] 
> 
> 1. <span style="color:#ff82b2"><b>Version space</b></span> $\leftarrow$ una lista contenente tutte le ipotesi in $H$
> 2. `for each` esempio di training $\textcolor{#ff82b2}{<x, c(x)>}$ rimuovi dallo spazio del version space tutte le ipotesi inconsistenti con l'esempio di training $$\textcolor{#ff82b2}{h(x)\ne c(x)}$$
> 3. Restituisci la lista delle ipotesi nel version space

# Rappresentare il Version Space
> [!quote] 
> 
> - Il <span style="color:#ff82b2"><b>General Boundary</b></span> (*confine generale*) ($\textcolor{#ff82b2}{G}$) del version space è l'insieme dei massimi membri generali di $H$ consistente con D
> - Il <span style="color:#ff82b2"><b>Specific Boundary</b></span> (*confine specifico*) ($\textcolor{#ff82b2}{S}$) del Version Space è l'insieme dei massimi membri specifici di $H$ consistente con $D$
> <span style="color:#ff82b2"><b><u>TEOREMA:</u></b></span> Tutti i membri del Version Space sono compresi fra questi due boundaries $$\textcolor{#ff82b2}{VS_{H,D}=\{h\in H | (\exists s \in S)(\exists g\in G)(g\geq h\geq s)\}}$$

Possiamo fare una <span style="color:#ff82b2"><i>ricerca completa</i></span> di ipotesi consistenti usando in due boundaries $\textcolor{#ff82b2}{S}$ e $\textcolor{#ff82b2}{G}$ per il [[IntroAI/ML/ML_Concept_Learning#Version Space|VS]], quindi non ristretto a $\textcolor{#ff82b2}{S}$ come per [[IntroAI/ML/ML_Concept_Learning#Algoritmo Find-S|find-S]].
# Candidate Elimination Algorithm
|                                                                                                                                                                                                                                                                                                                                                     Esempio Positivo | Esempio Negativo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
|                                                                                                                                                                                                                                                                Si rimuovuono da $\textcolor{#ff82b2}{G}$ tutte le ipotesi inconsistenti con $\textcolor{#ff82b2}{d}$ | Rimuovi da $\textcolor{#ff82b2}{S}$ tutte le ipotesi inconsistenti con $\textcolor{#ff82b2}{d}$                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| `For each` ipotesi $\textcolor{#ff82b2}{s}\in\textcolor{#ff82b2}{S}$ non consistente con $\textcolor{#ff82b2}{d}$: Generalizza $\textcolor{#ff82b2}{s}\large\rightarrow$ Aggiungi $\textcolor{#ff82b2}{s}$ alla minima generalizzazione $\textcolor{#ff82b2}{h}$ t.c. $(h \text{ consistente con } d) \vee (\text{ dei membri di } S \text sono più generali di ) h$ | `For each` ipotesi $\textcolor{#ff82b2}{g}\in \textcolor{#ff82b2}{G}$ non consistente con $\textcolor{#ff82b2}{d}$: Specializza $\textcolor{#ff82b2}{G}\large\rightarrow$ Rimuovi $\textcolor{#ff82b2}{g}$ da $\textcolor{#ff82b2}{G}\large\rightarrow$ Aggiungi a $\textcolor{#ff82b2}{G}$ tutte le minime specializzazioni $\textcolor{#ff82b2}{h}$ t.c. $(\textcolor{#ff82b2}{h}\text{ consistente con }\textcolor{#ff82b2}{d})\vee(\text{Alcuni membri di }\textcolor{#ff82b2}{S}\text{ sono più generali di }\textcolor{#ff82b2}{h})$ |
|                                                                                                                                                                                                                                                                                                   Rimuovi da $\textcolor{#ff82b2}{G}$ tutte le ipotesi più generali | Rimuovi da $\textcolor{#ff82b2}{G}$ tutte le ipotesi meno generali                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
Il nostro spazio delle ipotesi non può rappresentare un concetto target disgiunto semplice (ad es. $Sky=Sunny\vee Sky=Cloudy$)
## Bias
Assumiamo che lo spazio delle ipotesi $\textcolor{#ff82b2}{H}$ contenga il concetto target $\textcolor{#ff82b2}{c}\implies \textcolor{#ff82b2}{c}$ può essere descritto da una <span style="color:#ff82b2"><i>congiunzione</i></span> (con un AND) <span style="color:#ff82b2"><i>di letterali</i></span>.
## Unbiased Learner
Si sceglie un $\textcolor{#ff82b2}{H}$ che esprima tutti i concetti apprendibili, quindi $\textcolor{#ff82b2}{H}$ è l'insieme dei <span style="color:#ff82b2"><i>possibili sottoinsiemi di</i></span> $\textcolor{#ff82b2}{X}$, il power set $\textcolor{#ff82b2}{P(X)}$.
> [!example]
> 
> $$
> \begin{array}{c}
> |X| = 96, &
> |P(X)|=2^{96}\approx 10^{28} & \text{concetti distinti}
> \end{array}
> $$

Assumiamo gli esempi positivi $\textcolor{#ff82b2}{(x_1,x_2,x_3)}$ e quelli negativi $\textcolor{#ff82b2}{(x_4,x_5)}$
$$
\begin{array}{lcr}
	\textcolor{#ff82b2}{S:}\{(x_1\vee x_2\vee x_3\}
	&&
	\textcolor{#ff82b2}{G:}\{\neg(x_4\vee x_5)\}
\end{array}
$$
Gli unici esempi <span style="color:#ff82b2"><i>non ambigui</i></span> sono gli esempi di training stessi (rappresentati da $\textcolor{#ff82b2}{H}$), quindi per apprendere il target concept.
> [!summaries] Theorem
> 
> Un <span style="color:#ff82b2"><b>unbiased learner</b></span> non può generalizzare
> <span style="color:#ff82b2"><b><u>DIM:</u></b></span>
> 	Ogni istanza non osservata viene classificata come positiva precisamente dalla metà delle ipotesi nel VS e negativa dall'altra metà (<span style="color:#ff82b2"><i>rejection</i></span>)
> 	$$
>		\begin{array}{c}
> 			\forall\text{ }\textcolor{#ff82b2}{h}\text{ consistente con } \textcolor{#ff82b2}{x_i}\text{ }(test),\exists\text{ }\textcolor{#ff82b2}{h}\text{ identico a } \textcolor{#ff82b2}{h}\text{ eccetto } \textcolor{#ff82b2}{h'(x_i)<>h(x_i)}
> 			\\ \\
> 			\textcolor{#ff82b2}{h}\in VS\implies \textcolor{#ff82b2}{h'}\in VS
> 			\\
> 			\small{(\text{sono identici in } D)}
>		\end{array}
>	 $$

### Futilità di un Free-Biased Learner
Un learner che non fa assunzioni sull'identità del concetto target, <span style="color:#ff82b2"><i>non</i></span> ha basi razionali per <span style="color:#ff82b2"><i>classificare le istanze non viste</i></span>. Bias non fa assunzioni per efficienza, ma ne ha <span style="color:#ff82b2"><i>bisogno per generalizzarla</i></span>, ma non ci dice qual'è la migliore soluzione per generalizzare. Il <span style="color:#ff82b2"><i>problema</i></span> è caratterizzare il bias per approcci di apprendimento diversi.
> [!example]
> 
> ($\textcolor{#ff82b2}{TR}=$Training Set, $\textcolor{#ff82b2}{TS}=$Test Set)
> $$
> \begin{array}{cccl}
> & \textcolor{#ff82b2}{X} & \textcolor{#ff82b2}{c(x)} & \textcolor{#ff82b2}{H} = \{x, \lnot x, 0, 1\}
> \\
> \textcolor{#ff82b2}{TR} & 0 & 0 & VS=\{0,1\}
> \\
> \textcolor{#ff82b2}{TS} & 1 & \textcolor{#2e80f2}{?} & \implies \text{Può essere 1 o 0 finché non si usa X come TR}
> \end{array}
> $$

### Inductive Bias
> [!quote] Inductive Bias
> 
> I <span style="color:#ff82b2"><b>bias induttivi</b></span> di $\textcolor{#ff82b2}{L}$ sono tutti i minimi insiemi di asserzioni $\textcolor{#ff82b2}{B}$ tali che per ogni concetto target $\textcolor{#ff82b2}{c}$ e corrispondente dato di training $\textcolor{#ff82b2}{D_c}$
> $$
> \begin{array}{cr}
> 	(\forall \textcolor{#ff82b2}{x_i}\in \textcolor{#ff82b2}{X})[\textcolor{#ff82b2}{B}\vee \textcolor{#ff82b2}{D_c}\vee \textcolor{#ff82b2}{x_i}]\vdash \textcolor{#ff82b2}{L}(\textcolor{#ff82b2}{x_i}, \textcolor{#ff82b2}{D_c})
> 	\\ &
> 	{\begin{array}{|r|}
> 		{A \textcolor{#ff82b2}{\vdash}B}\text{ significa che }A \\ \text{ \textcolor{#ff82b2}{comporta logicamente}} B
> 	\end{array}}
> \end{array}
> $$

Consideriamo:
- L'algoritmo di concept learning $\textcolor{#ff82b2}{L}$
- L'istanza $\textcolor{#ff82b2}{X}$ e il concetto target $\textcolor{#ff82b2}{c}$
- Gli esempi di training $\textcolor{#ff82b2}{D_c=\{<x,c(x)>\}}$
$\textcolor{#ff82b2}{L(x_i,D_c)}$ denota la classificazione assegnata all'istanza $\textcolor{#ff82b2}{x_i}$ da $\textcolor{#ff82b2}{L}$ dopo il training su $\textcolor{#ff82b2}{D_c}$
### 3 Learner con biased diversi
|                                                                                                                                                                                                                       <span style="color:#ff82b2"><b>Rote Learner (look up table)</b></span> | <span style="color:#ff82b2"><b>Version Space Candidate Elimination Algorithm</b></span> | <span style="color:#ff82b2"><b>Find-S</b></span>                                                                                                                                                                                                                                     |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:---------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Memorizza gli esempi, classifica $\textcolor{#ff82b2}{x}$ sse è associato a un esempio osservato precedentemente $$\begin{array}{rcl}\begin{array}{r}osservato\\no inductive\\bias\end{array}&\Large\textcolor{#ff82b2}{\implies}&\begin{array}{l}no\\generalization\end{array}\end{array}$$ |  Bias: lo spazio delle ipotesi contiene il target concept (congiunzioni di attributi)   | Bias: lo spazio delle ipotesi contiene il concetto target e tutte le istanze negative $\textcolor{#ff82b2}{\implies}$ Abbiamo un linguaggio bias fatto di AND sui letterali più il <span style="color:#ff82b2"><i>search bias</i></span> sulle preferenze dell'ipotesi più specifica |

Per superare le restrizioni congiuntive sui <span style="color:#ff82b2"><i>modelli flessibili</i></span> usiamo <span style="color:#ff82b2"><i>diversi approcci di ML:</i></span>
- [[IntroAI/ML/ML_DecisionTree|Decision Tree]]
- <span style="color:#ff82b2"><i>Algoritmi Genetici</i></span> (TO LINK)
	Si codifica ogni insieme di regole come una stringa di bit e si usa la ricerca genetica per esplorare questo $\textcolor{#ff82b2}{H}$
- <span style="color:#ff82b2"><i>Programmazione logica induttiva</i></span> (TO LINK)
	Le regole della logica del prim'ordine contengono le variabili e i programmi sono inferiti automaticamente dagli esempi
