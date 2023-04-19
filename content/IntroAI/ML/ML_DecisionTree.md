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
			ELSE aggiungi il sottoalbero generato da IB3(X_i, T, Attrs - {A})
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
$\textcolor{#ff82b2}{S_V}$ è un sottoinsieme di esempi di $\textcolor{#ff82b2}{S}$ per i quali $\textcolor{#ff82b2}{A}$ ha valore $\textcolor{#ff82b2}{v}$. Maggiore è il guadagno, maggiore è l'efficienza dell'attributo nella classificazione dei dati di training
#### Perché si usa
L'entropia misura l'omogeneità (anzi l'impurità) della classe del sottoinsieme di esempi, quindi si <span style="color:#ff82b2"><i>seleziona un</i></span> $\textcolor{#ff82b2}{A}$ che <span style="color:#ff82b2"><i>massimizzi</i></span> $\textcolor{#ff82b2}{Gain(S, A)}$. Dopo la suddivisione ci sono valuri più bassi (target più omogenei) in ogni sottoinsieme (<span style="color:#ff82b2"><b>maggior guadagno</b></span>).
Lo scopo è di <span style="color:#ff82b2"><i>separare gli esempi</i></span> sulla base del target, trovare gli attributi che discriminano gli esempi che appartengono a diversi target $\textcolor{#9172dd}{\text{( ad es.}} \text{ tutti i positivi a sinistra, tutti i negativi a destra}\textcolor{#9172dd}{)}$.
> [!example]
> 
> | ![albero1](IntroAI/assets/pictures/ML_informationGain_exp1.png)                                                                              | ![albero1](IntroAI/assets/pictures/ML_informationGain_exp1.png) |
| -------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- |
> | $$\begin{array}{l}\textcolor{#ff82b2}{Gain(S,Humidity)=}&\\\begin{array}{}\textcolor{#ff82b2}{=9.40-}\textcolor{#ff82b2}{(\frac{7}{14}).985-(\frac{7}{14}).592= \textcolor{#83ed0e}{\boxed{\textcolor{#ff82b2}{.151}}}}\end{array}\end{array}$$ | $$\begin{array}{l}\textcolor{#ff82b2}{Gain(S,Wind)=}\\\begin{array}{}\textcolor{#ff82b2}{.940-(\frac{8}{14}).811 - (\frac{6}{14})1.0=0.48}\end{array}\end{array}$$                                                               |

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