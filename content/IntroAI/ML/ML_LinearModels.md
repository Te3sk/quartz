---
title: ML - Modelli Lineari
alias:
- 
tags: 
- #introai
- #ML
---
# Regressione
#AItask 
> [!quote]  Regressione
> 
> Processo di stima di una funzione a valori reali sulla base di un insieme di campioni disturbati
> - coppie note: $(x,f(x)+\text{random noise})$

> [!example]
> 
> ![Grafico](IntroAI/assets/pictures/ML_esempio_regressione.png)
> Vogliamo risolverlo (come trovare $w_1$ e $w_0$) in modo "*sistematico*"

## Unvariate Linear Regression
> [!quote] Unvariate Linear Regression
> 
> Assumiamo di avere un $\textcolor{#ff82b2}{h_w(x)}$ espresso come $\textcolor{#ff82b2}{out=h(x)=w_1x+w_0}$
> $$
> \stackrel{x}{\to}\fbox{h}\to
> $$
> dove $\textcolor{#ff82b2}{w}$ è un coefficiente a valori reali o un parametro libero(<span style="color:#ff82b2"><i>weights</i></span>)

Caso invariato, semplice regressione lineare, partiamo da una variabile in input e una in output. Adattiamo i dati con una <span style="color:#ff82b2"><i>retta</i></span>.
## Task e Modelli
Proviamo quindi a trovare una $\textcolor{#ff82b2}{h}\text{ }{\Large (}\stackrel{\large modello}{lineare}{\Large )}$ che faccia il miglior fitting di dati, avendo un insieme di dati di valori $\textcolor{#ff82b2}{x}$ e $\textcolor{#ff82b2}{g}$. Assumiamo che le variabili $\textcolor{#ff82b2}{x}$ e $\textcolor{#ff82b2}{y}$ siano collegate da $\textcolor{#ff82b2}{y = w_1 x + w_0 + noise}$, dove $\textcolor{#ff82b2}{w}$ è un parametro libero e $\textcolor{#ff82b2}{noise}$ è l'errore nella misurazione dei targets.
Proviamo quindi a <span style="color:#ff82b2"><i>trovare i valori di</i></span> $\textcolor{#ff82b2}{w}$ per costruire un modello che faccia <span style="color:#ff82b2"><i>predire</i></span>/<span style="color:#ff82b2"><i>stimare</i></span> $\textcolor{#ff82b2}{y}$ per i valori di $\textcolor{#ff82b2}{x}$ non noti.
## Apprendimento via LMS
> [!summaries] Premesse
> 
> Dobbiamo trovare i valori del parametro $\textcolor{#ff82b2}{w}$ per minimizzare l'errore nell'output del modello (fare un <span style="color:#ff82b2"><i>buon fitting</i></span>).
> Lo <span style="color:#ff82b2"><i>spazio delle ipotesi</i></span> è infinito, ma abbiamo una buona soluzione dalla matematica classica:
> - possiamo *apprendere* da strumenti basici
> - include diversi concetti rilevanti del ML moderno

$$
\begin{array}{rl}
	\begin{array}{|r|}
		\textcolor{#ff82b2}{\underline{INPUT:}} 
		\text{ Insieme di } \textcolor{#ff82b2}{l}\text{ esempi di}\\
		\text{training } \textcolor{#ff82b2}{(x_p, y_p)}\text{ con} \textcolor{#ff82b2}{p=1,...,l} \\
		\textcolor{#ff82b2}{\underline{OUTPUT:}} \text{ }
		\textcolor{#ff82b2}{h_w(x)}\text{ nella forma } \\\textcolor{#ff82b2}{w_1x+w_0} 
		(\text{quindi trovare }\textcolor{#ff82b2}{w})\text{ che}\\
		 \text{minimizzi la perdita di dati di}\\
		\text{training prevista}
	\end{array}
	&
	\begin{array}{l}
		\\ \\
		\text{Per la perdita usiamo l'errore dei quadrati}\\ \\
		\begin{array}{|l|}
			\text{Trovare } \textcolor{#ff82b2}{w} \text{ che minimizzi la somma}\\
			\text{residua dei quadrati}\\
			\textcolor{#ff82b2}{Loss(h_w)=E(w)=\sum^l_{p=1}(y_p-h_w(x_p))^2=}\\
			\textcolor{#ff82b2}{=\sum^l_{p=1}(y_p-(w_1x-w_0))^2}
		\end{array}
	\end{array}
\end{array}
$$
Dove $\textcolor{#ff82b2}{x_p}$ è il $p$-esimo input, $\textcolor{#ff82b2}{y_p}$ è l'output per $p$, $\textcolor{#ff82b2}{w}$ sono i parametri liberi e $\textcolor{#ff82b2}{l}$ il numero di esempi.
### Perché usarlo
![Grafico](IntroAI/assets/pictures/ML_whyLMS.png)
Linee <span style="color:#ff82b2"><b>blu</b></span> hanno diverse linee <span style="color:#33b85d"><b>verdi</b></span>. Minimizzare le linee <span style="color:#33b85d"><b>verdi</b></span> è un modo per trovare la migliore approssimazione/fitting dei dati. L'**errore quadratico** $\textcolor{#ff82b2}{E(w)}$ quantifica le linee verdi
$$
\textcolor{#ff82b2}{
	E(w)=\sum^p_{p=1}(y_p-h_w(x_p))^2
}
$$
Il metodo <span style="color:#ff82b2"><b>least squares</b></span> è un approccio standard per approssimare la soluzione di un sistema sopra-stimato, quindi un insieme di equazioni dove ci sono più equazioni conosciute che non.
### Cosa risolve?
Minimo locale come punto stazionario $\to$ il gradiente è nullo
$$
\textcolor{#ff82b2}{
\begin{array}{lr}
	{\Large \frac{\partial E(w)}{\partial w_i}}=0
	&&&
	i=1,...,(dim\_input + 1)
\end{array}
}
$$
Per la regressione lineare semplice (2 parametri semplici)
$$
\textcolor{#ff82b2}{
	\begin{array}{lr}
		{\Large \frac{\partial E(w)}{\partial w_o}}=0
		&&
		{\Large\frac{\partial E(w)}{\partial w_1}}=0
	\end{array}
}
$$
Le soluzioni finali non sono rilevanti per il corso
# Calcolo del gradiente
> [!quote] Regola base
> 
> $$
> \begin{array}{c}
> 	\begin{array}{c|c|c|c}
> 		\textcolor{#ff82b2}{{\Large\frac{\partial}{\partial w}}k =0} &
> 		\textcolor{#ff82b2}{{\Large\frac{\partial}{\partial w}}w = 1} &
> 		\textcolor{#ff82b2}{{\Large\frac{\partial}{\partial w}}w^2=2w} &
> 		\textcolor{#ff82b2}{{\Large\frac{\partial (f(w))^2}{\partial w}}=2f(w){\Large\frac{\partial (f(w))}{\partial w}}}
> 	\end{array}
> 	\\ \\
> 	\implies
> 	{\Large\frac{\partial E(w)}{\partial w_i}}=
> 	{\Large\frac{\partial(y-h_w(x))^2}{\partial w_i}}=
> 	2(y-h_w(x)){\Large\frac{\partial (y-h_w(x))}{\partial w_i}}=
> 	2(y-h_w(x)){\Large\frac{\partial (y-(w_1x+w_0))}{\partial w_i}}
> 	\\ \\
> 	\begin{array}{ccc}
> 		\boxed{
> 			\textcolor{#ff82b2}{
> 				\frac{\partial E(w)}{\partial w_0}=-2(y-h_w(x))
> 			}	
> 		}
> 	&&
> 		\boxed{
> 			\textcolor{#ff82b2}{
> 				\frac{\partial E(w)}{\partial w_1}=-2(y-h_w(x))\cdot x
> 			}
> 		}
> 	\end{array}
> \end{array}
> $$

## Gradiente decrescente
### Local Search
La derivazione precedente, suggerisce di costruire un <span style="color:#ff82b2"><i>algoritmo iterativo</i></span> basato su $\textcolor{#ff82b2}{\Large\frac{\partial E(w)}{\partial w_i}}$:
> [!quote] Gradiente = DIREZIONE DI SALITA
> 
> Possiamo muoverci verso il minimo con un gradiente decrescente ($\textcolor{#ff82b2}{\Delta w=\underline{-\text{gradiente di }E(w)}}$)

<span style="color:#ff82b2"><b>Local Search:</b></span> so comincia da un vettore di pesi iniziale, poi si modifica iterativamente per decrementare e minimizzare l'errore delle funzioni
> [!nb]
> 
> $$
> \begin{array}{rl}
> 	\textcolor{#ff82b2}{w_{new}=w+\eta\Delta w} &
> 	\begin{array}{l}
> 		\text{dove} {\stackrel{eta}{\textcolor{#ff82b2}{\large\eta}}}\text{ è una costante {\LARGE(}} \textcolor{#ff82b2}{\stackrel{\Large step}{size}}\text{\LARGE )}\\
> 		\text{chiamata \textcolor{#ff82b2}{learning note}}
> 	\end{array}
> \end{array}
> $$

## Motivazioni intuitive
$$
\begin{array}{c|cc|cc|c}
\textcolor{#ff82b2}{w_{new}=w+\eta\Delta w} &&
\textcolor{#ff82b2}{\Delta w_0=-{\Large\frac{\partial E(w)}{\partial w_0}}= 2(y-h(x))} &&
\textcolor{#ff82b2}{\Delta w_1={\Large\frac{\partial E(w)}{\partial w}}=2(y-h_w(x))\cdot x}
\end{array}
$$
> [!quote] Delta Rule 
> 
> É una regola per correggere gli errori (detta <span style="color:#ff82b2"><b>delta rule</b></span>) che combina $\textcolor{#ff82b2}{w}$ proporzionalmente all'errore ($\textcolor{#ff82b2}{target-output}$):
> - Se $\textcolor{#ff82b2}{target-output=err=0}\to$ nessuna correzione
> - Se $\textcolor{#ff82b2}{output > target}\to \textcolor{#ff82b2}{(g-k)<0}\to$ l'output è troppo alto
> 	- $\textcolor{#ff82b2}{\Delta w_0}$ negativo $\to$ riduce $\textcolor{#ff82b2}{w_0}$
> 	- $if(\text{input }\textcolor{#ff82b2}{x>0)\Delta w_1}$ negativo $\to$ riduci $\textcolor{#ff82b2}{w_1}$ ($else$ incrementalo)

Miglioriamo l'apprendimento dagli errori precedenti.
L'approccio del gradiente decrescente è un approccio di `local search` semplice ed efficace per le soluzioni LMS. Ci permette di cercare in <span style="color:#ff82b2"><i>spazi delle ipotesi infiniti</i></span> e può sempre essere applicato per $\textcolor{#ff82b2}{H}$ <span style="color:#ff82b2"><i>continuo</i></span> e perdite differenziali. Per renderlo più efficiente ci sono miglioramenti possibili.
# Linear models
> [!quote] Riassunto per $l$ patterns $\textcolor{#ff82b2}{(x_p, y_p)}$
> 
> $$
> \boxed{
> 	\begin{array}{cc|cc}
> 	\textcolor{#ff82b2}{
> 		\Delta w_0=-{\Large\frac{\partial E(w)}{\partial w_0}} = 2 \sum_{p=1}^{l}(y_p-h_w(x_p))
> 	}
> 	&&
> 	\textcolor{#ff82b2}{
> 		\Delta w_1 = - {\Large\frac{\partial E(w)}{\partial w_1}}=2\sum_{p=1}^{l}(y_p-h_w(x))\cdot x
> 	}
> 	\end{array}
> }
> $$


$$
\begin{array}{c|c}
	\begin{array}{c}
		\text{Aggiornare }\textcolor{#ff82b2}{w}\text{ dopo (ripetendo)} \\ 
		\text{ogni "epoca" di }\textcolor{#ff82b2}{l}\text{ dati di training}
	\end{array}
	&
	\begin{array}{c}
		\text{Aggiornare }\textcolor{#ff82b2}{w}\text{ dopo ogni}\\
		\text{ pattern }\textcolor{#ff82b2}{p}
	\end{array}
	\\
	\downarrow & \downarrow \\
	\textcolor{#ff82b2}{\text{Batch Algorithm}}
	&
	\textcolor{#ff82b2}{\text{One-Line Algorithm}}
\end{array}
$$
## Multidimensional input
<span style="color:#ff82b2"><b>Caso standard:</b></span> usando <span style="color:#ff82b2"><i>da 2 a 100 variabili</i></span> in input
$$
\textcolor{#ff82b2}{x=[x_1,x_2,...,x_n]}
$$
(il pattern in input è un vettore) ci sono molte possibilità in un largo range di campi applicativi
## Recap sulla notazione dei dati
| Pattern |   $x_1$   |   $x_2$   |   $x_i$   |   $x_n$   |
| -------:|:---------:|:---------:|:---------:|:---------:|
|   Pat 1 | $x_{1,1}$ | $x_{1,2}$ | $x_{1,i}$ | $x_{1,n}$ |
|     ... |    ...    |    ...    |    ...    |    ...    |
| Pat $p$ | $x_{p,1}$ | $x_{p,2}$ | $x_{p,i}$ | $x_{p,n}$ |
|     ... |    ...    |    ...    |    ...    |    ...    |
$\textcolor{#ff82b2}{X}$ è una matrice $\textcolor{#ff82b2}{l\times n}$. 
Spesso dobbiamo omettere gli indici quando il contesto è chairo, ad esempio:
- Ogni riga, un generico $\textcolor{#ff82b2}{x}$, una riga nella tabella <span style="color:#ff82b2"><i>(input)</i></span>
- $\textcolor{#ff82b2}{x_i}$ o $\textcolor{#ff82b2}{x_j}$ (scalari): componenti $\textcolor{#ff82b2}{i}$ o $\textcolor{#ff82b2}{j}$ (dato un pattern $\implies$quindi si ammette $\textcolor{#ff82b2}{p}$)
- $\textcolor{#ff82b2}{x_{p,i}}$ (scalare) come $\textcolor{#ff82b2}{(x_p)}$: componente $\textcolor{#ff82b2}{i}$ del pattern $\textcolor{#ff82b2}{p}$
- Per il target di solito usiamo $\textcolor{#ff82b2}{y_p}$ con $\textcolor{#ff82b2}{p=1,...,l}$
### Notazione con input multidimensionale
Assumiamo di avere <span style="color:#ff82b2"><i>vettori colonna</i></span> per $\textcolor{#ff82b2}{x}$ e $\textcolor{#ff82b2}{y}$, un numero $\textcolor{#ff82b2}{l}$ di dati, una dimensione $\textcolor{#ff82b2}{n}$ dei vettori in input e un target $\textcolor{#ff82b2}{y_p}$
$$
\textcolor{#ff82b2}{w^Tx+w_0=w_0+w_1x_1+w_2x_2+...+w_nx_n=w_0+\sum_{i=1}^{n}w_ix_i}
$$
Spesso conviene includere $\textcolor{#ff82b2}{x_0=1}$ così possiamo scrivere $\textcolor{#ff82b2}{w^Tx=x^Tw}$
## Vantaggi del modello lineare
Se funziona bene è molto semplice, tutte le informazioni dei dati stanno in $\textcolor{#ff82b2}{w}$, è facile da interpretare e sono ammessi dati disturbati. É una <span style="color:#ff82b2"><i>baseline dell'apprendimento</i></span> ed è stato usato/incluso in molti modelli complessi.
## Limitazione
![Grafico](IntroAI/assets/pictures/ML_linearModels_limits.png)
<span style="color:#ff82b2"><i>Muoversi verso una relazione non lineare:</i></span> come un modello parametrico statistico: "lineare" non si riferisce alla linea dritta, ma al <span style="color:#ff82b2"><i>modo</i></span> in cui i <span style="color:#ff82b2"><i>coefficienti di regressione</i></span> $\textcolor{#ff82b2}{w}$ occorrono nell'<span style="color:#ff82b2"><i>equazione di regressione</i></span>.
Possiamo anche usare <span style="color:#ff82b2"><i>input trasformati</i></span> con input e output a relazioni non lineari
$$
\begin{array}{cc}
	{\large\textcolor{#ff82b2}{h_w(x)=\sum_{j=0}^{M}w_jx^j}}
	&
	\begin{array}{c}
		regressione\\
		polinomiale
	\end{array}
\end{array}
$$
> [!example]
> 
>![grafico](IntroAI/assets/pictures/ML_esempio_lineare.png)
>Il risultato del fitting di una <span style="color:#2e80f2"><i>funzione quadratica</i></span>, $\textcolor{#ff82b2}{M=2}$, verso un <span style="color:#dd1024"><i>insieme di dati</i></span>. Nel *linear squares least* la funzione necessita di essere <span style="color:#ff82b2"><i>non lineare</i></span> negli argomenti (variabili in input), ma solo nei parametri $\textcolor{#ff82b2}{w}$ determinati per avere il miglior fit.

## Generalizzazione - LBE
> [!quote] 
> 
> Trasformazione di base - <span style="color:#ff82b2"><i>linear basis expansion</i></span>: 
> $$
> 	h_w(x)=\sum_{k=0}^{N}w_k\phi_k(x)
> $$
> Si argomenta il vettore in input con variabili più che siano trasformazioni di $\textcolor{#ff82b2}{x}$ secondo la funzione <span style="color:#ff82b2"><i>phi</i></span> ($\textcolor{#ff82b2}{\phi_k:\mathbb{R}^n\to\mathbb{R}}$)

Ad esempio:
- Rappresentazione polinomiale di $\textcolor{#ff82b2}{x:\phi(x)=x_j^2}$ o $\textcolor{#ff82b2}{\phi(x)=x_ix_j}$ o ...
- Trasformazioni non lineari di input singoli: $\textcolor{#ff82b2}{\phi(x)=log(x_j)}$, $\textcolor{#ff82b2}{\phi(x)=root(x)}$, ...
- Trasformazioni non lineari di input multipli: $\textcolor{#ff82b2}{\phi(x)=||x||}$
Il modello è <span style="color:#ff82b2"><i>lineare nei parametri</i></span> (in phi, non in $x$): possiamo usare lo stesso algoritmo di apprrendimento di prima
> [!example]
> 
> $\textcolor{#ff82b2}{\phi_j(x)=x^j}\implies\textcolor{#ff82b2}{h(x)=w_0+w_1x + w_2x^2 + \dots + w_nx^M = \sum_{j=0}^M w_jw^j}\implies$ Regressione polinomiale a 1 dimensione
> $\textcolor{#ff82b2}{\phi(x)=\phi([x_1,x_2,x_3])}\implies\textcolor{#ff82b2}{h(x)w_1x_1+w_2x_2+w_3 log(x_2)+w_4 log(x_3)+w_5(x_2x_3)+w_0}$

|                                                          <span style="color:#ff82b2"><b>PRO</b></span> | <span style="color:#ff82b2"><b>CONTRO</b></span> |
| ------------------------------------------------------------------------------------------------------:|:------------------------------------------------ |
| É più più <span style="color:#ff82b2"><i>espressivo</i></span>: può modellare relazioni più complicate | Con una grande base di funzioni, abbiamo bisogno di metodi per controllare la <span style="color:#ff82b2"><i>complessità del modello</i></span>                                                 |

|        <span style="color:#ff82b2"><i>Polinomio del 1° ordine</i></span> | <span style="color:#ff82b2"><i>Polinomio del 3° ordine</i></span> | <span style="color:#ff82b2"><i>Polinomio del 9° ordine</i></span> |
| ------------------------------------------------------------------------:|:-----------------------------------------------------------------:|:----------------------------------------------------------------- |
|          ![grafico](IntroAI/assets/pictures/ML_polinomio1ordine_exp.png) |  ![grafico](IntroAI/assets/pictures/ML_polinomio3ordine_exp.png)  | ![grafico](IntroAI/assets/pictures/ML_polinomio9ordine_exp.png)   |
| Soluzione povera: <span style="color:#ff82b2"><i>underfitting</i></span> |                   Più flessibile e più usabile                    | Molto flessibile ma può essere <span style="color:#ff82b2"><i>eccessiva</i></span> (overfitting)                                                                  |
## Complessità
- Modello molto semplice $\to$ non fa un buon fit dei dati $\to$ <span style="color:#ff82b2"><i>underfitting</i></span>
- Modello molto complesso $\to$ troppa sensibilità alla perturbazione dei dati$\to$ <span style="color:#ff82b2"><i>overfitting</i></span>
Vogliamo trovare la regolarizzazione per bilanciare i due casi verso il <span style="color:#ff82b2"><i>controllo della complessità del modello</i></span>. La complessità non è intesa come costo computazionale ma è una <span style="color:#ff82b2"><i>misura della flessibilità</i></span> del modello per il fit dei dati.
## Regolarizzazione
Può controllare l'<span style="color:#ff82b2"><i>overfitting</i></span> penalizzando le funzioni "complesse" con i pesi $\textcolor{#ff82b2}{w}$ grandi o il numero di parametri liberi, mantenendo la flessibilità dello spazio delle ipotesi.
$$
\begin{array}{c}
	\begin{array}{rcl}
		\begin{array}{r}
			\textcolor{#ff82b2}{"}\text{The simplest explanation} \\
			\text{is more likely the} \\
			\text{correct one}\textcolor{#ff82b2}{"}
		\end{array}
		&
		{\textcolor{#ff82b2}{\LARGE =}}
		&
		\begin{array}{l}
			\textcolor{#ff82b2}{"}\text{Prefer the simplest} \\
			\text{hypotesis that} \\
			\text{fits the datas}\textcolor{#ff82b2}{"}
		\end{array}
	\end{array}
	\\
	\textcolor{#ff82b2}{\text{ Ockham razor (1300)}}
\end{array}
$$
### Regolarizzazione con Ridge Regression
> [!quote] Ride Regression / Regolarizzazione di Tikhonov
> 
> Modelli smussati: possibilità di aggiungere <span style="color:#ff82b2"><i>vincoli</i></span> alla somma dei $\textcolor{#ff82b2}{|w_j|}$ favorendo modelli "sparsi".
> $$
> 	\textcolor{#ff82b2}{Loss(h_w)=\sum_{p=1}^l (y_p-h_w(x_p))^2 +} \textcolor{#2e80f2}{\lambda ||w||^2}
> $$

Ad esempio, con meno termini, a causa dei pesi $\textcolor{#ff82b2}{w_j=0}$ (o quasi), $\textcolor{#2e80f2}{\lambda}$ (costante) è detta <span style="color:#2e80f2"><i>coefficiente di regolarizzazione</i></span>.
L'<span style="color:#ff82b2"><i>effetto</i></span> è la decadenza del peso $\implies$ $\textcolor{#ff82b2}{w_{new}=w+\eta \cdot\Delta w} \textcolor{#2e80f2}{- 2\lambda w}$ (si aggiunge $\textcolor{#2e80f2}{2\lambda w}$ al gradiente del Loss).
> [!example]
> 
> Con gradiente $0$, decrementa il valore di ogni $\textcolor{#ff82b2}{w}$ con una frazione del vecchio $\textcolor{#ff82b2}{w}$

### Considerazioni
#### Applicabilità generale
Ad esempio possiamo controllare la complessità del modello usando solo $\textcolor{#2e80f2}{\lambda}$, senza sapere $\textcolor{#ff82b2}{M}$ o quando non conosciamo il modello.
#### Notare il <span style="color:#ff82b2"><b>BILANCIAMENTO</b></span> (trade-off) tra i due termini
- Vogliamo controllare la complessità del modello per controllare l'<span style="color:#ff82b2"><i>overfitting</i></span> $\to$ introduciamo un secondo termine nella minimizzazione
- Possiamo eccedere perché troppo peso sul secondo termine porta il focus della minimizzazioen solo (o per la maggio parte) sulla <span style="color:#ff82b2"><b>regolarizzazione</b></span> $\to$ l'errore dei dati può crescere troppo $\to$ <span style="color:#ff82b2"><i>underfitting</i></span>
- Il trade-off è rimosso dal valore di $\textcolor{#2e80f2}{\lambda}$
## Limitazioni delle funzioni a base fissa
Avendo funzioni di base lungo ogni dimensione di uno spazio di input a $\textcolor{#ff82b2}{D}$ dimensioni ($\textcolor{#ff82b2}{k}$ per LBE), si richiede un <span style="color:#ff82b2"><i>numero combinatorio di funioni</i></span>
- I dati diventano spari in un grande volume $\to$ La quantità di dati ha bisogno di supportare che il risultato cresce sempre esponenzialmente con la dimensione
- <span style="color:#ff82b2"><i>Phi</i></span> è fixato prima di osservare i dati di training
Quando dovremmo vedere come possiamo uscire con funzioni con <span style="color:#ff82b2"><i>basi basse</i></span>, scegliendole usando i dati di training: <span style="color:#ff82b2"><i>phi</i></span> dipende da $\textcolor{#ff82b2}{w}$ e il modello è non lineare nei parametri.
In altri modelli la computazione del nuovo spazio è fatto implicitamente attraverso il nucleo delle funzioni e controllando la complessità del modello.
## Classificazione dei modelli lineari
Lo stesso modello usato per la regressione, può essere usato per la <span style="color:#ff82b2"><i>classificazione</i></span> (<span style="color:#ff82b2"><b>target concept</b></span>). In questo caso usiamo un <span style="color:#ff82b2"><b>hyperplane</b></span> ($\textcolor{#ff82b2}{wx}$) assumendo valori possibili e negativi. Sfruttiamo questi modelli per <span style="color:#ff82b2"><i>decidere se un</i></span> $\textcolor{#ff82b2}{x}$ <span style="color:#ff82b2"><i>appartiene alla zona</i></span> più o meno dell'<span style="color:#ff82b2"><i>hyperplane</i></span>. Vogliamo quindi imporre $\textcolor{#ff82b2}{x}$ (con l'apprendimento) t.c. abbiamo una buona accuratezza nella classificazione.
![grafico](IntroAI/assets/pictures/ML_hyperplane.png)
$$
\begin{array}{c}
	\textcolor{#ff82b2}{w^Tx}\text{ definisce un \textcolor{#ff82b2}{hyperplane}} 
	\\
	\begin{array}{ccc}
		\textcolor{#ff82b2}{w^Tx=w_1x_1+w_2x_2+w_0=0}
		&
		\begin{array}{r}
			\textcolor{#fc0202}{decision}\\
			\textcolor{#fc0202}{boundary}
		\end{array}
	\end{array}
	\\
	\begin{array}{l}
		\text{Può essere usato per la classificazione:} \\
		\text{\textcolor{#9172dd}{es:}}\\
		\textcolor{#9172dd}{\to}<(1.0,1.0), 1>\\
		\textcolor{#9172dd}{\to}<(0.5,3.0), 1>\\
		\textcolor{#9172dd}{\to}<(2.0,2.0), 0>\\
	\end{array}
	\\
	\text{Dove }\textcolor{#ff82b2}{x}\text{ è il vettoredi input e }\textcolor{#ff82b2}{w}\text{ i parametri liberi}
\end{array}
$$
> [!quote] Classificatore
> 
> ![grafico](IntroAI/assets/pictures/ML_classificatore.png)
> 

$$
\textcolor{#fc0202}{
		\boxed{
		\begin{array}{c}
			\begin{array}{l}
				\textcolor{#ff82b2}{h(x)=
					\cases{
						\begin{array}{c}
							1 & if\text{ }wx+w_0\geq 0 \\
							0 & otherwise
						\end{array}
					}
				}		
			\end{array}
			\\ \\
			\textcolor{#ff82b2}{
				h(x) = sign(wx+w_0)
			}
			\\ \\
			\textcolor{#ff82b2}{
				h(x_p) = sign(x_P^Tw)=sign(\sum_{i=0}^n x_{p,i}w_i)
			}
		\end{array}
	}
}
$$
 ### Classificazione con boundary di decisione lineare
 La classificazione può essere vista come l'allocazione dello spazio di input nell regione di decisione
$$
\textcolor{#fc0202}{
	\boxed{
		\begin{array}{l}
			\\
			&
			\textcolor{#ff82b2}{
				h(x)=\cases{
					\begin{array}{rrr}
						1 & if\text{ }wx+w_0\geq0
						\\
						0 & otherwise
					\end{array}
				}
			}
			&
			\\ \\
			&
			\textcolor{#ff82b2}{
				h(x)=sign(wx+w_0)
			}
			&
			\\ \\
			&
			\begin{array}{c}
				\textcolor{#fc0202}{\text{Linear threshold unit (LTU)}}
			\end{array}
			&
		\end{array}
	}
}
$$
> [!Warning] Nota
> dando il bias $\textcolor{#ff82b2}{w_0}$ nell'LTU dicedndo
> $$\textcolor{#ff82b2}{h(x)=w^Tx+w_0\geq0}$$
> <span style="color:#ff82b2"><b>è equivalente a dire</b></span>
> $$\textcolor{#ff82b2}{h(x)=w^Tx\geq-w_0}$$
> con $\textcolor{#ff82b2}{-w_0}$ <span style="color:#ff82b2"><i>threshold value</i></span>

- Le due forme identificano la stessa zona positiva del classificatore
- La seconda enfatizza il ruolo del bias come un valore threshold per "*attivare*" l'output +1 del calssificatore
### Problema lineare per classificatore lineare
> [!quote] important
> 
> <span style="color:#ff82b2"><b>INPUT:</b></span> un insieme $\textcolor{#ff82b2}{l}$ di esempi di training
> <span style="color:#ff82b2"><b>OUTPUT:</b></span> $\textcolor{#ff82b2}{w}$ che minimizza la somma residua dei quadrati (LS)
> $$
> \textcolor{#ff82b2}{
> 	E(w)=\sum_{p=1}^{l}(y_p-x_p^Tw)^2=||y-Xw||^2
> }
> $$
> dove 
> - $\textcolor{#ff82b2}{x_p}$ è il $p$-esimo vettore di input
> - $\textcolor{#ff82b2}{y_p}$ l'output per $\textcolor{#ff82b2}{p}$
> - $\textcolor{#ff82b2}{w}$ i parametri liberi
> - $\textcolor{#ff82b2}{l}$ il minimo di esempi
> - $\textcolor{#ff82b2}{n}$ la dimensione degli esempi in input

 <span style="color:#ff82b2"><b>Errore Minimo:</b></span> se $\textcolor{#ff82b2}{y_p=1}$ allora  $\textcolor{#ff82b2}{x_p^Tw}\to \textcolor{#ff82b2}{1}$. Se $\textcolor{#ff82b2}{y_p=0}/\textcolor{#ff82b2}{-1}$ allora $\textcolor{#ff82b2}{x_pw^T}\to  \textcolor{#ff82b2}{0}/\textcolor{#ff82b2}{-1}$. Si noti che $\textcolor{#ff82b2}{E(x)}$ **NON** usa la forma $\textcolor{#ff82b2}{h(x)}$.
Abbiamo un <span style="color:#ff82b2"><i>algoritmo iterativo con gradiente decrescente</i></span>
$$
	\textcolor{#fc0202}{
		\boxed{
			\begin{array}{crlc}
				\\ &
				\textcolor{#ff82b2}{\Delta w_i =} & \textcolor{#ff82b2}{{\Large\frac{\partial E(w)}{\partial w_i}}=}
				& \\ 
				&&& \textcolor{#ff82b2}{i}\textcolor{#ffffff}{=0,\dots,1}
				\\ &&
				\textcolor{#ff82b2}{
					=\sum_{p=1}^l(y_px_p^Tw)-x_{p,i}
				}
			\end{array}
		}
	}
$$
![example](IntroAI/assets/pictures/ML_DeltaW_learningRule.png)
$$
\begin{array}{lr}
		\textcolor{#ff82b2}{(w_0=0)}
		& & &
		\textcolor{#ff82b2}{\Delta w_i=\sum_{p=1}^l(y_p-x_p^Tw)-x_{p,i}}
	\end{array}
$$
Se <span style="color:#ff82b2"><b>mal classificato</b></span> (perché il target è $+1$)
- Il delta è positivo e grande per $\textcolor{#ff82b2}{w_1}$ e $\textcolor{#ff82b2}{w_3}$
- Si incrementano proporzionalmente al delta, quindi in questo esempio un valore positivo [<span style="color:#ff82b2"><b>error connection rule</b></span>]
### Limitazioni
In geometria, 2 insiemi di punti in un graico a 2 dimensioni è <span style="color:#ff82b2"><b>linearmente separabile</b></span> quando i 2 insiemi possono essere completamente separati da una singola retta.
![grafico](IntroAI/assets/pictures/ML_SeparableSet.png)
In generale, 2 gruppo sono linearmente separabili in $\textcolor{#ff82b2}{n}$<span style="color:#ff82b2"><i>-dimensioni</i></span> se possono essere separati da un <span style="color:#ff82b2"><b>iperpiano</b> <i>a</i></span> $\textcolor{#ff82b2}{(n-1)}$ <span style="color:#ff82b2"><i>dimensioni</i></span>.
Il <span style="color:#ff82b2"><b>linear decision boundary</b></span> può fornire soluzioni esatte solo per insiemi di punti linearmente separabili.
> [!example]


| decision boundary <span style="color:#ff82b2"><b>non lineare</b></span> (da un'<span style="color:#ff82b2"><i>espansione di base</i></span>) | decision boundary <span style="color:#ff82b2"><b>non lineare</b></span> (da un'<span style="color:#ff82b2"><i>espansione di base</i></span>) <span style="color:#ff82b2"><b>regolarizzato</b></span> |
| -------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ![grafico](IntroAI/assets/pictures/ML_nonLinearDecisionBound_Exp1.png)                                                                       | ![grafico](IntroAI/assets/pictures/ML_nonLinearDecisionBound_Exp2.png)                                                                                                                               |
| Otteniamo un classificatore <span style="color:#ff82b2"><b>non smooth</b></span> che può aver bisogno di regolarizzazione                    | In questo caso un errore di training ($\textcolor{#ff6600}{+}$) è ammesso                                                                                                                                                                                                    |

## Altri modelli di apprendimento per la classificazione
- <span style="color:#ff82b2"><b>Linear Discriminant Analysis</b></span> (anche multi-class)
- <span style="color:#ff82b2"><b>Logistic Regression</b></span>
	$\textcolor{#ff82b2}{P(y/x)}$ comincia dal modellare la densità della classe come fosse una densità nota. Il threshold è soft (continua, differenziabile) comn funzioni logistiche, anziché $\textcolor{#ff82b2}{0/1}$ hard threshold 
- <span style="color:#ff82b2"><b>Neural Networks (NN) e SVM</b></span>
	Modelli flessibili che includono <span style="color:#ff82b2"><i>approssimazioni non lineari</i></span> sia per la regressione che per la classificazione
	- usa molte unità (tipo LTU) con diversi livelli
	- apprendimento della rappresentazione delle features in ogni livello
	- approccio del gradiente decrescente per l'apprendimento