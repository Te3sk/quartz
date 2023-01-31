---
title: RSA
tags: #critto
---
##### 22/11/07 #date 
# Costruzione delle chiavi
- Si scelgono $p$ e $q$ come due <span style="color:#ff82b2"><b>numeri primi molto grandi</b></span>
- Si calcola $\textcolor{#ff82b2}{n=p\times q}$ e la funzione di Eulero $\textcolor{#ff82b2}{\phi(n)=(p-1)*(q-1)}$
- Si sceglie un intero $\textcolor{#ff82b2}{e}<\phi(n)$ tale che $\textcolor{#ff82b2}{MCD(e,\phi(n))=1}$
- Si calcola l'inverso in modulo di $e$ $\textcolor{#ff82b2}{d=e^{-1} \:\:mod\:\:n}$
| Chiave Pubblica | Chiave Privata |
|:---------------:|:--------------:|
| $k_{pub}=<e,n>$ | $k_{priv}=<d>$ |
- Si rende pubblica la chiave $k[pub]$ e si mantiene segreta la chiave $k[priv]$
# Cifratura e decifrazione
Ogni messaggio è codificato come <span style="color:#ff82b2"><i>sequenza binaria</i></span>, che viene trattata come un <span style="color:#ff82b2"><i>intero</i></span> $\textcolor{#ff82b2}{m}$. Perché il cifrario funzioni deve risultare $\textcolor{#ff82b2}{m<n}$, se $m\geq n$, si <span style="color:#ff82b2"><i>divide</i></span> $m$ <span style="color:#ff82b2"><i>in blocchi</i></span> di $\lfloor log_2 n\rfloor$ bit, cifrati indipendentemente.
<span style="color:#ff82b2"><b>Cifratura:</b></span> $c=m^e \:\:mod\:\:n$ ($e,n$ sono contenuti in $k[pub]$)
<span style="color:#ff82b2"><b>Decifrazione:</b></span> $m=c^d \:\:mod\:\: n$ ($d$ è contenuto in $k[priv]$)
> [!exp]
> $p=5$    $q = 11$     $n=55$
> $\phi(n)=(p-1)*(q-1)=40$
> $e=7$           $MCD(7,40)=1$
> $$
> \begin{array}{ll}
> 	d=7^{-1} \:\:mod\:\:40\\
> 	& EE(7,40)\to <1,-17,...>\\
> 	& EE(40,7)\to <1,3,-2-3\cdot\lfloor\frac{40}{7}\rfloor> = <1,3,-17>&&&&&&\\
> 	& EE(7,5)\to <1,-2,1+2\cdot\lfloor\frac{7}{5}\rfloor> = <1,-2,3>\\ 
> 	& EE(5,2)\to <1,1,0-1\cdot\lfloor\frac{5}{2}\rfloor> = <1,1,-2>\\
> 	& EE(2,1)\to <1,0,1-0\cdot\lfloor\frac{2}{1}\rfloor> = <1,0,1>\\
> 	& EE(1,0)\to <1,1,0>\\
> 	\textcolor{#ff82b2}{d=-17 \:\:mod\:\:40 = 23}\\
> \end{array}
> $$
> $m<55$        $c=m^7 \:\:mod\:\:55$              $m=c^{23} \:\:mod\:\:55$

# Dimostrazione di correttezza
> [!the]
> $\textcolor{#ff82b2}{\:\:\forall\:\:m<n \to m^{ed} \:\:mod\:\:n=m}$
> $D(C(m, k_{pub}), k_{priv})=m$
> $c^d \:\:mod\:\:n \textcolor{#ff82b2}{=}(m^e \:\:mod\:\:n)^d \:\:mod\:\:n \textcolor{#ff82b2}{=}m^{ed} \:\:mod\:\:n \textcolor{#ff82b2}{=}m$

**Dim:**
1. $\textcolor{#ff82b2}{m}$ *ed* $\textcolor{#ff82b2}{n}$ *sono coprimi,* $\textcolor{#ff82b2}{MCD(m,n)=1}$
Dato 
	**a.** il *th. di eulero* ($m^{\phi(n)}\equiv 1 \:\:mod\:\:n$)
	**b.** la *definizione di inverso* ($ed\equiv 1 \:\:mod\:\:\phi(n)\to ed=1+r\phi(n)$, con $r\in \mathbf{N}$)
$$
m^{ed} \:\:mod\:\:n \stackrel{\textcolor{#ff82b2}{b.}}{\textcolor{#ff82b2}{=}}
m^{1+r\:\phi(n) \:\:mod\:\:n}\textcolor{#ff82b2}{=}
m\cdot(m^{\phi(n)})^r \:\:mod\:\:n \stackrel{\textcolor{#ff82b2}{a.}}{\textcolor{#ff82b2}{=}}
m\cdot 1^r \:\:mod\:\:n \textcolor{#ff82b2}{=}
m \:\:mod\:\:n \stackrel{m<n}{\textcolor{#ff82b2}{=}}m
$$
2. $\textcolor{#ff82b2}{MCD(m, n)\neq 1}$
$p|m$ e $qXm$
$\:\:\forall\:\:r\in\mathbf{N}\Rightarrow m^r\equiv 0 \:\:mod\:\:p$
$\Rightarrow m^r-m\equiv 0 \:\:mod\:\:p$
Scelgo $r=ed\Rightarrow m^{ed}-m\equiv 0 \:\:mod\:\:p$
$\textcolor{#ff82b2}{\boldsymbol{MCD(q,m)=1}}\Rightarrow$
$$
\begin{array}{c}
\Rightarrow
m^{ed} \:\:mod\:\:q \stackrel{\textcolor{#ff82b2}{b.}}{\textcolor{#ff82b2}{=}}
m^{1+r\phi(n)} \:\:mod\:\:q \textcolor{#ff82b2}{=}
m\cdot(m^{\phi(n)})^r \:\:mod\:\:q {\textcolor{#ff82b2}{=}}
m\cdot(m^{(p-1)(q-1)})^r \:\:mod\:\: q \textcolor{#ff82b2}{=}\\
m\cdot(m^{q-1})^{r\cdot(p-1) \:\:mod\:\:q}\stackrel{\textcolor{#ff82b2}{a.}}{\textcolor{#ff82b2}{=}}
m\cdot(1)^{r(p-1)} \:\:mod\:\:q \textcolor{#ff82b2}{=}
m \:\:mod\:\:q \textcolor{#ff82b2}{\Rightarrow}
\boldsymbol{\textcolor{#ff82b2}{m^{ed}-m\equiv 0 \:\:mod\:\:q}}\\
\end{array}
$$
$$
\textcolor{#ff82b2}{m^{ed}-m \equiv 0 \:\:mod\:\:} \stackrel{(=n)}{\textcolor{#ff82b2}{p*q}} \Leftarrow
\begin{cases}
m^{ed}-m\equiv 0 \:\:mod\:\:p\\
m^{ed}-m \equiv 0 \:\:mod\:\:q\\
\end{cases}
$$
# Efficienza
## Generazione della chiave
I numeri primi $p$ e $q$ devono essere molto grandi: con i computer odierni almeno un migliaio di cifre binari. La generazione di $p$ e $q$ può avvenire servendosi dell'<span style="color:#ff82b2"><i>algoritmo Miller-Robin</i></span>, richiede quindi <span style="color:#ff82b2"><i>tempo polinomiale</i></span> nella dimensione di questi numeri.
I valori $\textcolor{#ff82b2}{n}$ e $\textcolor{#ff82b2}{\Phi(n)}$ si calcolano come prodotti, quindi in <span style="color:#ff82b2"><i>tempo polinomiale</i></span> nelle loro dimensioni.
L'intero $\textcolor{#ff82b2}{d}$, inverso di $e \text{ mod }\:\,\Phi(n)$, può essere anch'esso generato in tempo polinomiale.
Le chiavi possono quinidi essere generate in tempo efficiente.
## Cifratura e Decifrazione
Sono altrettanto <span style="color:#ff82b2"><i>efficienti se si calcolano le potenze in modulo per esponenziazioni successive</i></span>. Non vi è alcuna controindicazione al fatto che due utenti scelgano lo stesso valore di $e$ purché scelgano diversi valori di $n$, quindi diverse chiavi pubbliche.
## Dimensione dei blocchi
Abbiamo richiesto che <span style="color:#ff82b2"><i>ogni blocco</i></span> $\textcolor{#ff82b2}{m}$, interpretato come un intero, <span style="color:#ff82b2"><i>sia minore di</i></span> $\textcolor{#ff82b2}{n}$: se infatti fosse $m\geq n$, i due messaggi $m$ e $(m \text{ mod }\:\,n)$ genererebbero lo stesso crittogramma. Dato che il valore di $n$ è relativo a un singolo destinatario, si deve stabilire uno standard per la decomposizione in blocchi nell'intero sistema. La via più semplice è quella di usare una <span style="color:#ff82b2"><i>dimensione dei blocchi uguale per tutti</i></span>.
In alternativa si può <span style="color:#ff82b2"><i>decomporre</i></span> il messaggio in <span style="color:#ff82b2"><i>blocchi di</i></span> $\textcolor{#ff82b2}{l=\lfloor log_2 n\rfloor}$ <span style="color:#ff82b2"><i>bit</i></span>, dove $n$ (quindi anche $l$) è relativo al destinatario del messaggio. Notando che $n$ non è una potenza di 2, questo meccanismo garantisce $m\leq 2^l\leq n$.
# Attacchi RSA
## Sicurezza
Legata alla difficoltà di fattorizzare un numero arbitrario molto grande
$$
\begin{array}{c}
fattorizzare \Rightarrow forzare \:\: RSA\:\: ✔\\
fattorizzare \Leftarrow forzare\:\:RSA\:\: ❓\\
\end{array}
$$
Il calcolo della radice in modulo $$m=\:\:^e\sqrt{c} \:\:mod\:\:\textcolor{#2e80f2}{n}$$
è difficile almento quanto fattorizzare ($\textcolor{#2e80f2}{n}$ composto). Calcolare $\textcolor{#2e80f2}{\phi(n)}$ direttamente da $\textcolor{#2e80f2}{n}$ è equivalente a fattorizzare $n$. Ricavare $\textcolor{#2e80f2}{d}$ da $\textcolor{#2e80f2}{(n, e)}$ sembra costoso quanto fattorizzare $n$.
Il calcolo di $\textcolor{#2e80f2}{\phi(n)}$ direttamente da $\textcolor{#ff82b2}{n}$ <span style="color:#ff82b2"><i>e la fattorizzazione di</i></span> $\textcolor{#ff82b2}{n}$ <span style="color:#ff82b2"><i>sono due problemi computazionali equivalenti</i></span> (ciascuno si trasforma nell'altro in tempo polinomiale).
La fattorizzazione $n\to p$ e $q \to \textcolor{#2e80f2}{\phi(n)=(p-1)(q-1)}$
$$\begin{array}{ll}
n,\phi(n)\to\\
& \textcolor{#2e80f2}{\phi(n)=(p-1)(q-1)=pq-(p+q)+1=n-(p+q)+1}\\
& \textcolor{#2e80f2}{(p+q)=n-\phi(n)+1}\\
& \textcolor{#2e80f2}{(p-q)^2=(p+q)^2-4\:pq=(p+q)^2-4\:n}
\end{array}$$
### Come possiamo fattorizzare un numero intero n?
La [[Glossary#Fattorizzazione| fattorizzazione]] è un <span style="color:#ff82b2"><i>problema difficile, ma non più come un tempo...</i></span>
- Da un lato la potenza di calcolo aumentata, dall'altro gli algoritmi di fattorizzazione vengono raffinati
- Esistono agloritmi relativamente veloci di complessità subesponenziale
	- <span style="color:#ff82b2"><i>General Number Field Sieve (GNFS)</i></span> richiede $O(2^{\sqrt{b\:log\:b}})$ operazioni per fattorizzare un intero $\textcolor{#2e80f2}{n}$, di $\textcolor{#2e80f2}{b=[log_2 n]+1}$ bit
	- Un attacco brute force ne richiede $O(n)$, i.e. $O(2^b)$
- Con la potenza di calcolo attuale, usando <span style="color:#ff82b2"><i>l'algoritmo GNFS</i></span> è possibile fattorizzare semiprimi fino a circa <span style="color:#ff82b2"><i>768 bit</i></span>
- Per interi con una struttura particolari, esistono algoritmi di fattorizzazione particolarmente efficienti
- La <span style="color:#ff82b2"><i>fattorizzazione e il logaritmo discreto non sono problemi NP-Hard</i></span>, e si possono risolvere in <span style="color:#ff82b2"><i>tempo polinomiale su macchine quantistiche</i></span>
## Scelta dei parametri
Si scelgono $p$ e $q$ di almeno 1024 bit
| TDEA,AES (bit della chiave) | RSA e DH (bit del modulo) |
|:---------------------------:|:-------------------------:|
|             80              |           1024            |
|             112             |           2048            |
|             128             |           3072            |
|             192             |           7680            |
|             256             | 15360                          |

### Vincoli su p e q:
- Scegliere $p$ e $q$ <span style="color:#ff82b2"><i>molto grandi</i></span> (di almeno 1024 bit) per resistere agli attacchi brute force
- Sia $\textcolor{#ff82b2}{p-1}$ che $\textcolor{#ff82b2}{q-1}$ devono contenere un <span style="color:#ff82b2"><i>fattore primo grande</i></span> (altrimenti $\textcolor{#ff82b2}{n}$ si fattorizza velocemente)
- $\textcolor{#ff82b2}{MCD(p-1,q-1)}$ deve essere <span style="color:#ff82b2"><i>piccolo</i></span>, conviene scegliere $\textcolor{#ff82b2}{p}$ e $\textcolor{#ff82b2}{q}$ tale che $\textcolor{#ff82b2}{\frac{p-1}{2}}$ e $\textcolor{#ff82b2}{\frac{q-1}{2}}$ siano <span style="color:#ff82b2"><i>co-primi</i></span>.
> [!dan]
> Non usare uno dei primi per altri moduli
> $$
> \begin{array}{rl}
> n_1=p*q_1\\
> n_2=p*q_2\\
> & \to p=gdc(n_1,n_2)
> \end{array}
> $$

Studio del 2012 condotto su $4,7\cdot 10^6$ moduli RSA a 1024 bit: il $3\%_0$ dei moduli condivide un numero primo.
<span style="color:#ff82b2"><b>Scegliere p e q non troppo vicini tra loro</b></span>
$\textcolor{#2e80f2}{n\sim p^2 \sim q^2}$ e quindi anche $\textcolor{#2e80f2}{\sqrt{n}}$ sarà vicino ai primi, bastera quindi un attacco <span style="color:#ff82b2"><i>bruteforce che cerca i fattori vicino alla</i></span> $\textcolor{#ff82b2}{\sqrt{n}}$
$$
\begin{array}{l}
\large{(\frac{p+q}{2})}^2 -n=(\frac{p-q}{2})^2\\
1)\:\: \frac{(p+q)}{2}>\sqrt{n}\\
2)\:\: \frac{(p+q)^2}{2}-n\:\:\text{ è un quadrato perfetto} &&&&&&&&&&&&&&&&&&&&&&&
\end{array}
$$
Si scandiscono gli <span style="color:#ff82b2"><i>interi maggiori di</i></span> $\textcolor{#ff82b2}{\sqrt{n}}$ fino a trovare $\textcolor{#ff82b2}{z}$ tale che
$$\textcolor{#ff82b2}{z^2-n=w^2}$$
si suppone
$$\textcolor{#ff82b2}{\frac{z=(p+q)}{2}\:\:\:\:w=\frac{(p-q)}{2}}$$
da cui
$$\textcolor{#ff82b2}{p=z+w\:\:\:\:\:q=z-w}$$
la vicinanza tra $p$ e $q$ assicura che non dobbiamo allontanarci troppo da $\sqrt{n}$ per trovare $p$ e $q$
### Attacchi con esponenti bassi
Esponenti $\textcolor{#ff82b2}{e}$ e $\textcolor{#ff82b2}{d}$ <span style="color:#ff82b2"><i>bassi</i></span> sono attraenti perché <span style="color:#ff82b2"><i></i></span>. Ovviamente $\textcolor{#ff82b2}{d}$ dovrebbe essere scelto <span style="color:#ff82b2"><i>sufficientemente grande</i></span>, per evitare attacchi bruteforce.
> [!dan]
> Se $\textcolor{#ff82b2}{m}$ e $\textcolor{#ff82b2}{q}$ sono così piccoli che $\textcolor{#ff82b2}{m^e<n}$, allora risulta <span style="color:#ff82b2"><b>facile</b></span> trovare la <span style="color:#ff82b2"><i>radice e-esima di</i></span> $\textcolor{#ff82b2}{c}$, poiché $\textcolor{#ff82b2}{c=m^e}$.
> <span style="color:#ff82b2"><b>Non interviene la riduzione in modulo!</b></span>

Almeno $e$ utenti abbiano scelto lo stesso valore di $\textcolor{#ff82b2}{e}$, e ricevano lo stesso messaggio $\textcolor{#ff82b2}{m}$ (senza padding) attraverso i crittogrammi
$$
\begin{array}{l}
\textcolor{#ff82b2}{c_1=m^e \:\:mod\:\:n_1}\\
\textcolor{#ff82b2}{\vdots}\\
\textcolor{#ff82b2}{c_e=m^e \:\:mod\:\:n_e}
\end{array}
$$
Sia $\textcolor{#ff82b2}{m<n_i} \:\:\forall\:\: 1\leq i\leq e$, e siano $\textcolor{#ff82b2}{n_1,n_2,...,n_e}$ *primi tra loro*.
- Per il <span style="color:#ff82b2"><i>Teorema Cinese del Resto</i></span>, esiste (e si può calcolare in tempo polinomiale) un unico $\textcolor{#ff82b2}{m'<n=n_1 n_2 ... n_e}$ che soddisfa l'equazione $\textcolor{#ff82b2}{m'\equiv m^e \:\:mod\:\:n}$ 
- Dato che $m^e<n$, la congruenza può essere riscritta come $\textcolor{#ff82b2}{m'=m^e}$
- Calcolato $\textcolor{#ff82b2}{m'}$, si può ricavare $\textcolor{#ff82b2}{m}$ mediante l'estrazione della radice $e$-esima che in questo caso si esegue facilmente perché non interviene l'operazione in modulo
#### Common Modulus Attack
> [!def] Identità di Bézout
> Siano $a$ e $b$ due interi, e sia $\textcolor{#ff82b2}{d=MCD(a,b)}$. Allora, esistono, e si possono calcolare in tempo polinomiale, due interi $\textcolor{#ff82b2}{x}$ e $\textcolor{#ff82b2}{y}$ tali che
> $$\textcolor{#ff82b2}{a\:x+b\:y=d}$$

Supponiamo di avere due chiavi $\textcolor{#ff82b2}{(n, e_1), (n,e_2)}$, con $\textcolor{#ff82b2}{GDC(e_1,e_2) = 1}$. Per l'identità di Bézout, esistono due interi $r$ e $s$ tali che
$$\textcolor{#2e80f2}{r\:e_1+s\:e_2=1}$$
Sia $\textcolor{#ff82b2}{r<0}$, e suppooniamo che il crittoanalista intercetti due crittogrammi $\textcolor{#ff82b2}{c_1,c_2}$ relativi allo stesso messaggio in chiaro $\textcolor{#ff82b2}{m}$
$$
\begin{array}{lcccr}
\textcolor{#ff82b2}{c_1=m^{e_1} \:\:mod\:\:n} &&&
\textcolor{#ff82b2}{c_2=m^{e_2} \:\:mod\:\:n}
\end{array}
$$
Risulta
$$\large{m=m^{e_1r+e_2s}=(c^r_1\times c^s_2) \:\:mod\:\:n=((c_1^{-1})^{-r}\times c_2^s) \:\:mod\:\:s}$$
### Attacchi a tempo (DH, RSA)
Si basano sul tempo di esecuzione dell'algoritmo di decifrazione.
L'<span style="color:#ff82b2"><b>idea</b></span> è quella di determinare $d$ <span style="color:#ff82b2"><i>analizzando il tempo impiegato per decifrare</i></span>. Quando viene eseguita l'esponenziazione modulare, si esegue una moltiplicazioni ad ogni iterazione, più <span style="color:#ff82b2"><i>un'ulteriore moltiplicazione modulare per ciascun bit uguale a 1 in</i></span> $\textcolor{#ff82b2}{d}$. Il <span style="color:#ff82b2"><b>rimedio</b></span> a questo è di aggiungere rutardo casuale per confondere il crittoanalista.