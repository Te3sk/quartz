---
title: Cifrari perfetti
tags: #critto
---
##### 2022/10/17 #date 
# Cifrari Perfetti
Detti anche <span style="color:#ff82b2"><b>incondizionatamente sicuri</b></span>, si contrappongono ai cifrari <span style="color:#ff82b2"><i>computazionalmente sicuri</i></span>, che si basano sul limite delle risorse del crittoanalista.
Nei cifrari incondizionatamente sicuri, l'informazione è protetta con certezza assoluta, si possono decifrare solo se si ha la chiave. Il *[[Glossary#Brute force attack|brute force]]* <span style="color:#ff82b2"><i>non ha senso</i></span>.
Il cifrario è <span style="color:#ff82b2"><b>simmetrico</b></span>, quindi con la stessa chiave crittiamo e decifriamo.
I dati che abbiamo sono:
- <span style="color:#ff82b2"><b>MSG</b></span> $\textcolor{#ff82b2}{\rightarrow}$ Spazio dei messaggi
- <span style="color:#ff82b2"><b>CRITTO</b></span> $\textcolor{#ff82b2}{\rightarrow}$ Spazio dei crittogrammi
- <span style="color:#ff82b2"><b>KEY</b></span> $\textcolor{#ff82b2}{\rightarrow}$ Spazio delle chiavi
- <span style="color:#ff82b2"><b>M</b></span> $\textcolor{#ff82b2}{\rightarrow}$ Variabile aleatoria che descrive il comportamento del mittente ($\textcolor{#f77ead}{M\in MSG}$)
- <span style="color:#ff82b2"><b>C</b></span> $\textcolor{#ff82b2}{\rightarrow}$ Variabile aleatoria che descrive il comportamento del processo di comunicazione ($\textcolor{#f77ead}{C\in CRITTO}$)
- $\textcolor{#ff82b2}{P(M=m)\rightarrow}$ Probabilità che il mittente voglia inviare il messaggio $\textcolor{#f77ead}{m}$
- $\textcolor{#ff82b2}{P(C=c)\rightarrow}$ Probabilità che sul canale stia transitando il crittogramma $\textcolor{#f77ead}{c}$
- $\textcolor{#ff82b2}{P(M=m|C=c)\rightarrow}$ Probabilità condizionata che il messaggio inviato sia $\textcolor{#f77ead}{m}$ posto che il crittogramma è $\textcolor{#f77ead}{c}$
> [!quote] Cifrario perfetto
> 
> Un cifrario è detto perfetto se $\forall m\in MSG, \forall c \in CRITTO \implies$
> $\implies P(M=m|C=c)=P(M=m)$

Dal crittogramma non filtra alcuna informazione sul messaggio. La conoscenza del crittoanalista non cambia dopo aver osservato il crittogramma $c$.

> [!example] Example 
> 
> $\exists \bar{m}\in\text{ MSG }t.c. P(M=\bar{m})>0 \wedge P(M=\bar{m} | C= \bar{c}) = 0$
> se sul canale transita $\bar{c}$ sicuramente il messaggio è $\bar{m}$

> [!example] Example 
> 
> $\exists m'\in MSG, t.c. P(M=m')=p>0$
> $\exists c'\text{ t.c. }P(M=m'|C=c')= 1$
> Se osservo $c'$, so con certezza che il messaggio inviato è $m'$

## Teorema di Shannon
> [!abstract] Theorem
>  In un cifrario perfetto, il numero delle chiavi deve essere maggiore o uguale al numero dei messaggi possibili

<span style="color:#ff82b2"><b>Dim:</b></span>
I messaggi possibili sono i messaggi $m$ t.c. $P(M=m)>0$. Supponiamo per assurdo di avere un cifrario con un numero di chiavi inferiore al numero di messaggi possibili.
$$
\begin{array}{ll}
	\textcolor{#ff82b2}{N_c}=\underline{\#\text{ chiavi}} & \textcolor{#ff82b2}{N_m}=\underline{\#\text{ messaggi protetti}}\implies N_c<N_m &&&&&&&&&&&&&&&&&&&&&&&
\end{array}
$$
Quindi il numero di messaggi che possono corrispondere a $\textcolor{#f77ead}{N_c}$ è $\textcolor{#f77ead}{\leq N_k}$ (numero di chiavi). Il numero 
$s=|\{m\in MSG\,|\,\exists\,k\in key\text{ t.c. }D(c,k)=m\}|=$ al numero di messaggi che possono corrispondere a $\textcolor{#f77ead}{c}$, basati decifrando $c$ con tutte le chiavi
$$
\begin{array}{l}
	S\leq N_k<N_m\implies S<N_m \implies\\
	\implies \exists m'\in \text{MSG possibili, t.c.}\\
	P(M=m')>0 \wedge P(M=m'|C=c)=0 &&&&&&&&&&&&&&&&&&&&&&&&&&&&&
\end{array}
$$
Questo vuol dire che il cifrario non è perfetto.
## One-Time Pad
- [[Glossary#Alfabeto|Alfabeto]] $\{0,1\}$
- $MSG, CRITTO, KEY=\{0,1\}$ e $n>0$

La <span style="color:#ff82b2"><b>cifratura</b></span> e la <span style="color:#ff82b2"><b>decifrazione</b></span>, si esegue con lo <span style="color:#ff82b2"><b>XOR bit a bit</b></span> 
|     | 00  | 01  | 10  | 11  |
| --- | --- | --- | --- | --- |
| <span style="color:#ff82b2"><i>XOR</i></span> | 0   | 1   | 1   | 0    |

$m=m_1m_2m_3...m_i...m_n$
$k=k_1k_2k_3...k_i...k_nk_{n+1}...$
$$
\begin{array}{rl}
	\boxed{
		\begin{array}{r}
			\textcolor{#ff82b2}{\text{Cifratura}}\\
			\begin{array}{r}
				c_1c_2c_3...c_i...c_n \\
				c_i=m_i \oplus k_i \\
				c=m\oplus k_{\text{bit a bit}}
			\end{array}
		\end{array}
	} & \boxed{
		\begin{array}{}
			\textcolor{#ff82b2}{\text{Decifrazione}} \\
			\begin{array}{l}
				\\
				m_i=C_i\oplus k_i \text{ con }1\leq i \leq n \\
				\\
			\end{array}
		\end{array}
	}\\
\end{array}
$$
<span style="color:#ff82b2"><b>Dimostrazione di correttezza</b></span>
$\forall i, 1 \leq i \leq n$
$c_i\oplus :k_i = (m_i\oplus k_i)\oplus k_i=m_i\oplus k_i\oplus k_i=m_i \oplus \textcolor{#f77ead}{0} = m_i$
> [!tip] Observation
> La chiave non deve essere riutilizzata, altrimenti
> $$
> \begin{array}{lccl}
> c = m\oplus k && c\oplus c'= & m\oplus k\oplus m' \\
> c'=m\oplus k &&& \oplus k=m\oplus m' \\
> &&&\oplus k\oplus k= m\oplus m'
> \end{array}
> $$

### Dimostrazione di perfezione di One-Time Pad
pboldIpotesi:
1. Tutti i messaggi hanno lunghezza $n$
2. Tutte le sequenze di $n$ bit sono messaggi possibili, anche le sequenze non significative hanno probabilità non nulla, molto bassa, di essere inviati
3. Chiave scelta perfettamente a caso per ogni messaggio e non riutilizzata. Ogni chaive deve essere scelta con prob. $\frac{1}{2^n}$ 
> [!abstract] Theorem
> Sotto le ipotesi precedenti, One-Time Pad è un cifrario <span style="color:#ff82b2"><b>perfetto</b></span> e <span style="color:#ff82b2"><b>minimale</b></span>, quindi impiega un numero minimo di chiavi

<span style="color:#ff82b2"><b>Dimostrazione:</b></span>
dobbiamo dimostrare che $\forall\:m\in MSG, \forall\:c\in CRITTO$, $\textcolor{#f77ead}{P(M=m|C=c)=P(M=m)}$
$$
P(M=m|C=c)=\frac{P(M=m \:\:e \:\:C=c)}{P(C=c)}
$$
Per le proprietà dello XOR, fissato $m$, chiavi diverse creano crittogrammi diversi
$\exists$ chiave che ha forma $m$ (fissato) in $c$ (fissato) t.c. $c=m \oplus k\implies \textcolor{#f77ead}{k = m \oplus c}$.
La probabilità di scegliere l'unica chiave che trasforma un $m$ fissato in $c$ è $\frac{1}{2^n}\implies P(C=c)=\frac{1}{2^n}$. Ciò vuol dire che on dipende dal messaggio, ma solo dalla chiave.
$$
P(M=m|C=c)=\frac{P(M=m \:\:e \:\:C=c)}{P(C=c)}=\frac{P(M=m)P(C=c)}{P(C=c)}
$$
#### Possibile riduzione del numero di bit casuali della chiave
Proviamo a rimuovere l'ipotesi 2, quindi solo $\alpha^n\:(\alpha=1.1)$ sequenze binarie di lunghezza $n$ sono significative (nel caso della lingua inglese)

$$
\begin{array}{l}
	|\text{ MSG possbili }|=\alpha^n\\
	|\text{ KEY }|=N_k\geq\alpha^n\\
	2^t\geq\alpha^n &\text{dove }t=\text{\#bit casuali della chiave}\\
	t\geq log_2\,\alpha^n=n\:log_2\,\alpha=0,12\,n
\end{array}
$$
