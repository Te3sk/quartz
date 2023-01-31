---
title: Firma Digitale
alias:
- protocolli
- autenticazione
- identificazione
- Certification Authority
tags: #critto
---
##### 2022/11/24 #date 
# Identificazione, autenticazione, Firma digitale
> [!def] Identificazione
> Un sistema di elaborazione, isolato o in rete, deve essere in grado di <span style="color:#ff82b2"><i>accertare l'identità di un utente</i></span> che richiede di accedere ai suoi servizi

> [!def] Autenticazione
> Il destinatario di un messaggio deve essere in grado di accertare:
> - <span style="color:#ff82b2"><i>l'identità del mittente</i></span>
> - <span style="color:#ff82b2"><i>l'integrità del crittogramma ricevuto</i></span>

> [!def] Firma digitale
> 1. `MITT` non deve poter negare di aver inviato un messaggio
> 2. `DEST` deve essere in grado di autenticare il messaggio
> 3. `DEST` non deve poter sostenere che $m'\ne m$ è il messaggio inviato da `MITT`
> Tutto ciò deve essere verificato da una terza parte

### Relazioni tra le funzionalità
Non sono indipendenti, ma ciascuna estende le precedenti: l'autenticazione di un messaggio garantisce l'identificazione del mittente e l'apposizione della firma del garantisce l'autenticazione del messaggio.
<span style="color:#ff82b2"><i>Ogni funzionalità è utilizzata per contrastare gli attacchi attivi</i></span>. Esistono relazioni algoritmiche basate sui cifrari asimmetrici e simmetrici.
## Funzioni hash
> [!def] 
> Una <span style="color:#ff82b2"><i>funzione hash</i></span> $\textcolor{#ff82b2}{H: X \rightarrow Y}$ è una funzione tale che
> $$n = |X| >> m=|Y|$$
> $\exists\:\:X_1,X_2,...,X_m\subseteq X$ <span style="color:#ff82b2"><b>disgiunti</b></span> tali che
> $$
> \begin{array}{l}
> 	X=X_1\cup X_2\cup ... \cup X_m \\
> 	\forall\:\: i, \:\:\forall\:\: x \in X_i, H(x)=y
> \end{array}
> $$

Nella costruzione di algoritmi, queste funzioni sono impiegate per operare su elementi $x \in X$ attraverso la loro <span style="color:#ff82b2"><i>immagine</i></span> (o <span style="color:#ff82b2"><i>fingerprint</i></span>) $\textcolor{#ff82b2}{y=f(x)\in Y}$, essenzialmente per due scopi:
$$
\begin{array}{lcr}
	\boxed{
		\begin{array}{c}
			\text{La \textcolor{#ff82b2}{rappresentazione di }} \textcolor{#ff82b2}{y}\text{ richiede } \\\textcolor{#ff82b2}{log_2\:m}\text{ bit, è quindi più breve di }\\\text{quella di }x\text{ che ne richiede }log_2\:n
		\end{array}
	}&&
	\boxed{
		\begin{array}{c}
			\textcolor{#ff82b2}{Y} \text{ può avere una \textcolor{#ff82b2}{"struttura" assente}}\\
			\textcolor{#ff82b2}{\text{in } X:} \text{per esempio gli elementi}\\y\text{ corrispondono alle posizioni di}\\\text{un vettore di }m\text{ celle in cui memorizzare}\\\text{informazioni associate a }m\text{ diversi elementi di }X.
		\end{array}
	}
\end{array}
$$

Una buona funzione Hash deve assicurarsi che:
- I sottoinsiemi $X_1, ..., X_m$ abbiano <span style="color:#ff82b2"><b>circa la stessa cardinalità</b></span>, due elementi estratti a caso da $X$ hanno probabilità circa $\frac{1}{m}$ di avere la stessa immagine in $Y$
- Elementi di $X$ molto "simili" tra loro <span style="color:#ff82b2"><b>appartengano a due sottoinsiemi diversi</b></span>, se $X$ è un insieme di interi, due elementi con valori prossimi devono avere immagini diverse
- <span style="color:#ff82b2"><b>Gestione delle collisioni:</b></span> l'algoritmo che impiega la funzione hash dovrà affrontare la situazione in cui più elementi di $X$ hanno la stessa immagine in $Y$
### Funzioni hash one-way
Se la funzione è applicata in crittografia, deve soddisfarre le seguenti proprietà:
1. $\forall\:\: x \in X$ è <span style="color:#ff82b2"><i>computazionalmente facile</i></span> calcolare $\textcolor{#ff82b2}{y=H(x)}$
2. Proprietà *one-way*: per la maggior parte degli $y\in Y$ è *computazionalemnte difficil* determinare $x\in X$ tale che $\textcolor{#ff82b2}{H(x)=y}$
3. Proprietà *collision-resistant*: è computazionalmente difficile determinare una coppia di elementi $x_1, x_2 \in X$ tali che $\textcolor{#ff82b2}{H(x_1)=H(x_2)}$
|                                          Requirement | Description                                                                                                       |
| ----------------------------------------------------:|:----------------------------------------------------------------------------------------------------------------- |
|                                  Variable input size | $H$ can be applied to a block of data of any size                                                                 |
|                                    Fixed output size | $H$ produces a fixed-length output                                                                                |
|                                           Efficiency | $H(x)$ is relatively easy to compute for any given $x$, making both hardware and software implementation pratical |
|                Preimage resistant (one-way property) | For any given hash value $h$, it's computationally infeasible to find $y$ such that $H(y)=h$                      |
| Second preimage resistant (weak collision resistant) | For any given block $x$, it's computationally infeasible to find $y\ne x$ with $H(y)=H(x)$                        |
|     Collision resistant (strong collision resistant) | It's computationally infeasible to find any pair $(x, y)$ such that $H(x)=H(y)$                                   |
|                                     Pseudorandomness | Output of $H$ meetss standard tests for pseudorandomness                                                          |

Ovviamente tutte le funzioni hash dovrebbero soddisfarre la prima proprietà, la seconda (<span style="color:#ff82b2"><i>one-way</i></span>) e la terza (<span style="color:#ff82b2"><i>claw-free</i></span>), necessarie per impieghi crittografici, dovrebbero essere garantite attraverso una dimostrazione formale di intrattabilità.
### Funzioni hash usate in crittografia
#### MD5 (Message Digest, versione 5)
Si tratta di una famiglia di algoritmi: quello originale non fu mai pubblicato; si pubblicarono MD2 e MD4. In questi due furono trovate debolezze e venne proposto MD5, che riceve in input una sequenza $S$ di 512 bit e produce un'*immagine di 128 bit*: la sequenza è *digerita* riducendone la lunghezza ad un quarto.
É stato in seguito dimostrato che MD5 non resiste alle collisioni, e nel 2004 si sono individuate debolezze serie. Oggi la sua sicurezza è considerata *severamente compromessa*
#### RIPEMD-160
É la versione "matura" delle funzioni MD. Nata nel '95 da un progetto dell'UE, *produce immagini di 160 bit* ed è esente dai difetti di MD5
#### SHA (Secure Hash Algorithm)
Progettata da NIST e NSA nel '93, si adotta quando la proprietà di resistenza alle collisioni è cruciale per la sicurezza del sistema. Opera su sequenze lunghe fino a $2^{64}$ bit e produce *immagini di 160 bit*. É una funzione *crittograficamente sicura*: soddisfa i requisiti delle funzioni hash one-way, e genera immagini molto diverse per sequenze molto simili.
La prima prese il nome di <span style="color:#ff82b2"><i>SHA0</i></span>, viene sostituita con la nuova versione <span style="color:#ff82b2"><b>SHA1</b></span> che, come la precedente opera su <span style="color:#ff82b2"><i>sequenze di</i></span> $\textcolor{#ff82b2}{2^{64}-1}$ <span style="color:#ff82b2"><i>bit</i></span> e produce <span style="color:#ff82b2"><i>immagini di 160 bit</i></span>. Opera al suo interno su blocchi di 160 bit contenuti in un *buffer* di 5 *registri* $A, B, C, D, E$ di 32 bit ciascuno. Il messaggio $m$ di cui si vuole calcolare l'immagine è concatenato con una sequenza di <span style="color:#ff82b2"><i>padding</i></span> che ne rende la lunghezza multipla di 512 bit. Alla fine del procedimento, quando il messaggio è stato letto, i registri contengono l'hash $SHA1(m)$
## Identificazione su canali sicuri
> [!exp]
> Accesso di un utente alla propria casella di posta elettronica, o a file personali memorizzati su un computer ad accesso riservato ai membri della sua organizzazione

L'utente inizia il collegamento inviando in chiaro *login* e *password*, se il canale è protetto in lettura e scrittura, un attacco può essere sferrato solo da un utente locale al sistema. Il meccanismo di identificazione prevede una *cifratura delle password*, realizzata con *funzioni hash one-way*.
### Cifratura password nei sistemi UNIX
Quando un *utente* $\textcolor{#ff82b2}{U}$ fornisce per la prima volta una *password* $\textcolor{#ff82b2}{P}$, il sistema associa a $U$ due sequenze binarie (che memorizza nel file delle password al posto di $P$):
- $\textcolor{#ff82b2}{S}$ (*seme*): prodotto da un generatore pseudocasuale
- $\textcolor{#ff82b2}{Q=H}$ (*PS*): $H$: funzione hash one-way
Ad ogni successiva connessione di $U$, il sistema
1. recupera $S$ dal file delle password
2. concatena $S$ con la password fornita da $U$
3. calcola l'immagine one-way della nuova sequenza: $H(PS)$
4. se $H(PS)=Q$ l'identificazione ha successo
Un accesso illecito al file delle password non fornisce informazioni interessanti: è computazionalmente difficile ricavare la password originale dalla sua immagine one-way
### Protezione del canale
*Se il canale è insicuro, la password può essere intercettata durante la sua trasmissione in chiaro*. Il sistema non dovrebbe mai maneggiare direttamente la password, ma una sua immagine inattaccabile
#### Canale insicuro: identificazione
$\textcolor{#ff82b2}{< e, n >, < d > =}$ chiavi pubblica e private di un *utente* $\textcolor{#ff82b2}{U}$ che richiede l'accesso ai servizi offerti dal *sistema* $\textcolor{#ff82b2}{S}$
1. $S$ genera un numero casuale $r<n$ e lo invia in chiaro a $U$
2. $U$ calcola $\textcolor{#ff82b2}{f=r^d \:\:mod\:\:n}$ (*firma di* $\textcolor{#ff82b2}{U}$ *su* $\textcolor{#ff82b2}{r}$) con la sua chiave privata e lo spedice a $S$
3. Si verifica la correttezza del valore ricevuto calcolando e si verifcando se $\textcolor{#ff82b2}{f^e \:\:mod\:\:n=r}$. Se ciò avviene, l'identificazione ha successo
Le operazioni di cifratura e decifrazione sono invertite all'impiego standard nell'RSA. Questo è possibile perché le due operazioni sono commutative
$$
\textcolor{#ff82b2}{(x^e \:\:mod\:\:n)^d \:\:mod\:\:n=(x^d \:\:mod\:\:n)^e \:\:mod\:\:n \:\:\:(=x)}
$$
$f$ può essere generata solo da $U$ che possiede $< d >$. Se il passo 3 va a buon fine, il sistema ha la garanzia che l'utente ha richiesto l'identificazione sia effettivamente $U$, anche se il canale è insicuro.
**Problema:** $S$ chiede a $U$ di applicare la sua chiave provata ad una sequenza $r$ che $S$ stesso ha generato, potrebbe essere stata scelta di proposito per ricavare qualche informazione sulla chiave privata di $U$
**Protocollo alternativo a "conoscenza zero":** impedisce cha da una comunicazione si possa estrarre più di quanto sia nelle intenzioni del comunicatore
#### Canale sicuro: Autenticazione
Il **destinatario** deve *autenticare* il messaggio accertando l'identità del mittente e l'integrità del messaggio $m$.
$$
\textcolor{#ff82b2}{\text{\emph{Il mittente ed il {destinatario} concordano una chiave segreta } } k}
$$
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  Mittente | Destinatario |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------ |
| $\rightarrow$ allega il messaggio con un **MAC**(*Message Authentication Code*) allo scopo di garantire la provenienza e l'integrità del messaggio $\textcolor{#ff82b2}{A(m, k)}$ $$$$ $\rightarrow$ spedisce la coppia $\textcolor{#ff82b2}{< m, A(m,k) >}$ in chiaro $$$$ $\rightarrow$ oppure, cifra $m$ e spedisce $\textcolor{#ff82b2}{< C(m, k'), A(m, k) >}$, dove $\textcolor{#ff82b2}{C}$ è la funzione di cifratura e $\textcolor{#ff82b2}{k'}$ è la chiave pubblica o segreta del cifrario scelto | $\rightarrow$ entra in possesso di $m$ (dopo averlo eventualmente decifrato)$$$$ $\rightarrow$ essendo a conoscenza di $A$ e $k$, calcola $\textcolor{#ff82b2}{A(m, k)}$ $$$$ $\rightarrow$ confronta il valore ottenuto con quello inviato dal mittente per verificare che il MAC ricevuto corrisponda al messaggio ca cui risulta allegato, se la verifica ha successio il messaggio è autentico, altrimenti il destinatario scarta il messaggio             |
### MAC 
Il **MAC** è un'immagine breve del messaggio, che può essere stata generata solo da un mittente conosciuto dal destinatario, previ opportuni accordi. Ne sono state proposte varie realizzazioni, basate su cifrari asimmetrici, simmetrici e sulle *funzioni hash one-way*
#### MAC con funzioni hash one-way
$$
\textcolor{#ff82b2}{
	A(m, k)=H(mk)\text{, con } H \text{ funzione hash one-way}
}
$$
Risulta computazionalmente difficile per un crittoanalista scoprire la chiave segreta $k$, $H$ è nota a tutti, e $m$ può viaggiare in chiaro o essere scoperto per altra via, *ma $\textcolor{#ff82b2}{k}$ viaggia all'interno del MAC*, quindi per recuperare $k$ si dovrebbe invertire $H$.
Il crittoanalista non può sostituire facilmente il messaggio $m$ con un altro messaggio $m'$, dovrebbe allegare alla comunicazione di $m'$ il MAC $A(m',k)$ che può produrre solo conoscendo $k$.
#### CBC + MAC
Usando il cifrario a blocchi in modalità CBC, si può usare il blocco finale del crittogramma come MAC, il lblocco finale è infatti funzione all'interno del messaggio.
##### 2022/11/28 #date 
## Firma digitale
| **Firma manuale**                                                                                                                                     | **Firma digitale**                                                                                                                                                                                                         |
| ----------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| *É autentica e non falsificabile*, prova che chi l'ha prodotta è chi ha sottoscritto il documento                                                     | Non può consistere semplicemente di una digitalizzazione del documento originale manualmente, un crittoanalista potrebbe "tagliare" dal documento digitale la parte contenente la firma e "copiarla" su un altro documento |
| *Non è riutilizzabile*, è legata al documento su cui è stata apposta                                                                                  | Deve avere una forma che dipenda dal documento su cui viene apposta, per essere inscindibile da questo (è una funzione matematica del documento)                                                                           |
| *Il documento firmato non è alterabile*, chi ha prodotto la firma è sicuro che questa si riferirà solo al documento scritto nella sua forma originale | Per progettare firme digitali si possono usare sia cifrari simmetrici che asimmetrici                                                                                                                                                                                                                           |
| *Non può essere ripudiata da chi l'ha apposta*, costituisce prova legale di un accordo o dichiarazione                                                |                                                                                                                                                                                                                            |

### Protocollo 1 (Diffie e Hellman) - messaggio in chiaro e firmato
$\textcolor{#ff82b2}{U}$: utente, $\textcolor{#ff82b2}{k_U[priv]}$ e $\textcolor{#ff82b2}{k_U[pub]}$: chiavi di $U$, $\textcolor{#ff82b2}{C}$ e $\textcolor{#ff82b2}{D}$: funzioni di cifratura e decifrazione di un cifrario asimmetrico
#### Firma
$U$ genera la *firma* $\textcolor{#ff82b2}{f=D(m, k_U[priv])}$ per $m$, poi spedisce all'utente $V$ la tripla $\textcolor{#ff82b2}{< U, m, f >}$
#### Verifica
$V$ riceve $\textcolor{#ff82b2}{< U, m, f >}$ e verifica l'autenticità della firma $f$ calcolando e controllando che
$$
\textcolor{#ff82b2}{C(f, k_U[pub])=m}
$$
L'indicazione del mittente $U$ consente a $V$ di selezionare la chiave pubblica $k_U[pub]$ da utilizzare nel calcolo. I processi di firma e verifica impiegano le funzioni $C$ e $D$ in ordine diverso a quello standard, $C$ e $D$ devono essere commutative
$$
\textcolor{#ff82b2}{C(D(m))=D(C(m))=m}
$$
#### Requisiti
**Il protocollo soddisfa i requisiti della firma manuale:**
- $f$ *è autentica e non falsificabile*
	- $k_U[priv]$ è nota solo a $U$
	- per falsificare la firma occorre conoscere $k_U[priv]$, ma $D$ è *one-way*
- Il documento firmato $< U, m, f >$ *non può essere alterato* se non da $U$, pena la consistenza tra $m$ e $f$
- Poiché solo $U$ può aver prodotto $f$, $\textcolor{#ff82b2}{U}$ *non può ripudiare la firma*
- La firma $f$ *non è riutilizzabile* su un'altro documento $m'\ne m$, poiché è immagine di $m$ (la funzione che crea la firma prende il documento $m$ come parametro)
É stato definito per un particolare utente $U$, ma non per un particolare destinatario. Chiunque può convincersi dell'autenticità della firma facendo uso solo della chiave pubblica di $U$. É solo uno schema di principio: comporta lo scambio di un messaggio di lunghezza doppia dell'originale poiché la dimensione della firma è paragonabile alla dimensione del messaggio. Il messaggio non può essere cifrato poiché è ricavabile pubblicamente dalla firma attraverso la verificha di questa.
### Protocollo 2 - messaggio cifrato e firmato
#### Firma e Cifratura
- $U$ genera la firma $\textcolor{#ff82b2}{f=D(m,k_U[priv])}$ per $m$
- calcola il *crittogramma firmato* $\textcolor{#ff82b2}{c=C(f,k_V[pub])}$ con la chiave pubblica del destinatario $V$ (*si incapsula la firma nel documento cifrato*)
- spedisce la coppia $\textcolor{#ff82b2}{< U, c >}$ a $V$
#### Decifrazione e Verifica
- $V$ riceve la coppia $< U, c >$ e decifra il crittogramma $$\textcolor{#ff82b2}{D(c, k_V[priv])=f}$$
- Cifra tale valore con la chiave pubblica di $U$ ottenendo $$\textcolor{#ff82b2}{C(f, k_U[pub])=C(D(m,k_U[priv]), k_U[pub])=m}$$
- $V$ ricostruisce $m$, e se $m$ è significativo attesta l'identità di $U$. Un utente diverso da $U$ ha probabilità praticamente nulla di generare un crittogramma di significato accettabile se "cifrato" con la chiave pubblica di $U$
#### Algoritmo
| Cifrario RSA, con                                                                                                   | Utente $U$                                                                                                                                                                                                                                                                 | utente $V$ |
| ------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| $$\begin{array}{l}< d_U >, < e_U, n_U > \text{ chiavi di }U\\< d_V >, < e_V, n_V > \text{ chiavi di }V\end{array}$$ | $\rightarrow$ genera la firma del messaggio$$\textcolor{#ff82b2}{f=m^{d_U} \:\:mod\:\:n_U}$$$\rightarrow$ cifra $f$ con la chiave pubblica di $V$ $$\textcolor{#ff82b2}{c=f^{e_V} \:\:mod\:\:n_V}$$ $\rightarrow$ spedisce a $V$ la coppia $\textcolor{#ff82b2}{< U, c >}$ | $\rightarrow$ riceve la coppia $< U, c >$ e decifra $c$$$\textcolor{#ff82b2}{c^{d_V}\:\:mod\:\:n_V=f}$$$\rightarrow$ "decifra" $f$ con la chiave pubblica di $U$$$\textcolor{#ff82b2}{f^{e_U}\:\:mod\:\:n_U=m}$$$\rightarrow$ Se $m$ è significativo, conclude che è autentico 

Per la correttezza del procedimento è necessario che $\textcolor{#ff82b2}{n_U\leq n_V}$ perché risulti $\textcolor{#ff82b2}{f<n_V}$ e $f$ possa essere cifrata correttamente e spedita a $V$, questo impedirebbe a $V$ di inviare messaggi firmati e cifrati a $U$. Ogni utente stabilisce chiavi distinte per la firma e per la cifratura.
#### Attacco
Supponiamo che un utente $U$ invii una risposta automatica (ack) al mittente ogni volta che riceve un messaggio $m$, il segnale di ack sia il crittogramma della firma di $U$ su $m$.
Un crittoanalista attivo $X$ può decifrare i messaggi inviati a $U$:
- $X$ intercetta il crittogramma $c$ firmato inviato da $V$ a $U$, lo rimuove dal canale e lo rispedisce a $U$, facendogli credere che $c$ sia stato inviato da lui
- $U$ spedisce automaticamente a $X$ un ack
- $X$ usa l'ack ricevuto per risalire al messaggio originale $m$ applicando le funzioni del cifrario con le chiavi pubbliche di $V$ e di $U$
| Utenti                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Crittoanalista |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------- |
| $1\rightarrow$ $V$ invia il crittogramma $\textcolor{#ff82b2}{c}$ a $U$:$$\textcolor{#ff82b2}{\begin{array}{l}c=C(f,k_U[pub])\\f=D(m,k_V[priv])\end{array}}$$$2\rightarrow$ Il crittoanalista $X$ intercetta $\textcolor{#ff82b2}{c}$, lo rimuove dal canale e lo rispedisce a $U$ ($U$ crede che $c$ arrivi da $V$)$$$$$3\rightarrow$ $U$ decifra $\textcolor{#ff82b2}{c}$ $$\textcolor{#ff82b2}{f=D(c,k_U[priv])}$$e verifica la firma con la chiave pubblica di $X$ ottenendo un messaggio $$\textcolor{#ff82b2}{m'=C(f,k_X[pub])}$$$4\rightarrow$ $\textcolor{#ff82b2}{m'\ne m}$ è un messaggio privo di senso, ma il sistema manda l'ack $c'$ a $X$$$\textcolor{#ff82b2}{\begin{array}{l}f'=D(m',k_U[priv])\\c'=C(f',k_X[pub])\end{array}}$$ | $X$ usa $c'$ per risalire al messaggio $m$:$$$$$1\rightarrow$ decifra $c'$ e trova $f'$$$D(\textcolor{#ff82b2}{c'}, k_X[priv])=D(\textcolor{#ff82b2}{C(f', k_X[pub])}, k_X[priv])=f'$$$2\rightarrow$ verifica $f'$ e trova $m'$$$C(\textcolor{#ff82b2}{f'}, k_U[pub])=D(\textcolor{#ff82b2}{C(m', k_U[priv])}, k_U[pub])=m'$$$3\rightarrow$ da $m'$ ricava $f$ $$D(\textcolor{#ff82b2}{m'}, k_X[priv])=D(\textcolor{#ff82b2}{C(f, k_X[pub])}, k_X[priv])=f$$$4\rightarrow$ verifica $f$ con la chiave pubblica di $V$ e trova $m$ $$C(\textcolor{#ff82b2}{f}, k_V[pub]=C(\textcolor{#ff82b2}{D(m, k_V[priv])}, k_V[pub])=m$$               |
Per evitare questo attacco occorre che $U$ blocchi l'ack automatico. L'ack deve essere inviato solo dopo un esame preventivo di $m$ e scartando i messaggi che non si desidera firmare.
Per avere un **Protocollo resistente agli attacchi**, si evita la firma diretta del messaggio, si appone la firma digitale su un'immagine del messaggio ottenuta con una funzione hash one-way e pubblica (tipo MD5, SHA). La firma non è più soggetta a giochi algebrici.
### Protocollo 3 - messaggio cifrato e firmato in hash
Questo protocollo richiede lo scambio di una maggiore quantità di dati, ma l'incremento è trascurabile. La firma può essere calcolata più velocemente
#### Firma e Cifratura
Il mittente $U$ calcola $\textcolor{#ff82b2}{H(m)}$ e genera la firma $$
\textcolor{#ff82b2}{f=D(H(m), k_U[priv])}
$$
calcola separatamente il crittogramma $$
\textcolor{#ff82b2}{c=C(m, k_V[pub])}
$$
e spedisce a $V$ la tripla $\textcolor{#ff82b2}{< U, c, f >}$
#### Decifrazione e Verifica
$V$ riceve la tripla $\textcolor{#ff82b2}{< U, c, f >}$ e decifra il crittogramma $c$: $$
\textcolor{#ff82b2}{m=D(c, k_V[priv])}
$$
calcola separatamente $\textcolor{#ff82b2}{H(m)}$ e $\textcolor{#ff82b2}{C(f, k_U[pub])}$, se $\textcolor{#ff82b2}{H(m)=C(f, k_U[pub])}$, conclude che il messaggio è autentico.
## Attacchi man in-the-middle
Questo tipo di attacchi sono dovuti alla *debolezza dei protocolli*: le chiavi di cifratura sono pubbliche e non richiedono un incontro diretto tra gli utenti per il loro scambio. Un crittoanalista attivo può intromettersi proprio in questa fase iniziale del protocollo, pregiudicando il suo corretto svolgimento. In questo caso, il crittoanalista si intromette nella comunicazione tra i due utenti, intercettando e bloccando le loro comunicazioni, e fingendosi l'uno agli occhi dell'altro.
### Attacchi man in-the-middle sulle chiavi pubbliche
Il crittoanalista $X$ devia le comunicazioni che provengono da $U$ e $V$ e le dirige verso se stesso. Quindi quando $U$ chiede a $V$ la sua chiave pubblica, $X$ *intercetta la risposta* contenente $k_V[pub]$ e la *sostituisce con la sua chiave pubblica* $k_X[pub]$. Si pone poi *in ascolto* dei crittogrammi spediti da $U$ a $V$, rimuove dal canale ciascuno di questi, lo decritta, lo cifra e lo rispedisce.
$U$ e $V$ *non si accorgeranno della presenza* di $X$ se il processo di intercettazione e rispedizione è sufficientemente veloce da rendere il relativo ritardo apparentemente attribuibile alla rete.
## Certification Authority (CA)
Un algoritmo crittografico è tanto robusto quanto la sicurezza delle sue chiavi, lo scambio/generazione della chiave è un passo cruciale e gli attacchi man in-the-middle sono i principali problemi di sicurezza da affrontare. Nacono così le **Certification Authority:** infrastrutture che garantiscono la validità delle chiavi pubbliche e ne regolano l'uso, gestendo la distribuzione sicura delle chiavi alle due entità che vogliono comunicare.
La CA autentica l'associazione $< utente,\:\:chiave\:\:pubblica >$ emettendo un **certificato digitale**, il quale consiste della chiave pubblica e di una *lista di informazioni relative al suo proprietario*, opportunamente firmate dalla CA. Per svolgere correttamente il suo ruolo, la CA *mantiene un archivio di chiavi pubbliche sicuro*, accessibile a tutti e protetto da attacchi in scrittura non autorizzati. La chiave pubblica della CA è nota agli utenti che la mantengono protetta da attacchi esterni e la utilizzano per verificare la firma della CA stessa sui certificati.

Se $U$ vuole comunicare con $V$ può richiedere $k_V[pub]$ alla CA, che risponde inviando a $U$ il certificato digitale $cert_V$ di $V$. Poiché $U$ conosce $k_{CA}[pub]$, può controllare l'autenticità del certificato verificandone il periodo di validità e la firma prodotta dalla CA su di esso. Se tutti i controllo vanno a buon fine, $K_V[pub]$ nel certificato è corretta e $U$ avvia la comunicazione con $V$. Un crittoanalista potrebbe intromettersi solo falsificando la certificazione, ma si assume che la CA sia fidata e il suo archivio di chiavi sia inattaccabile.

Esistono in ogni stato diverse CA organizzate ad albero, la verifica di un certificato è in questo caso più complicata e si svolge attraverso una catena di verifiche che vanno dalla CA di $U$ alla CA di $V$: $V$ invia a $U$ una sequenza di certificati ordinati in accordo alla CA che li ha firmati, da quella che ha certificato $V$ a quella che è alla radice dell'albero delle CA.
Ogni utente mantiene localmente al sistema una coppia del proprio certificato $cert_U$ e una copia di $K_{CA}[pub]$, interagisce con la CA una sola  volta, e poi la gestione delle chiavi pubbliche diventa decentralizzata.

#### Contenuto di un Certificato digitale
- una indicazione del suo *formato* (numero di versione)
- il *nome della CA* che l'ha rilasciato
- un *numero seriale* che lo individua unicamente all'interno della CA emittente
- la *specifica dell'algoritmo* usato dalla CA per creare la firma elettronica
- il *periodo di validità* del certificato
- il *nome dell'utente* a cui questo certificato si riferisce e una serie di informazioni a lui legate
- un'indicazione del *protocollo a chiave pubblica adottato* da questo utente per la ficratura e la firma (nome dell'algoritmo, suoi parametri, e chaive pubblica dell'utente)
- *firma della CA* eseguita su tutte le informazioni precedenti
### Protocollo 4 - messaggio cifrato, firmato in hash e certificato
#### Firma, cifratura e certificazione
Il mittente $U$ si procura il certificato $\textcolor{#ff82b2}{cert_V}$ di $V$, calcola $\textcolor{#ff82b2}{H(m)}$ e genera la firma $$
\textcolor{#ff82b2}{f=D(H(m), k_U[priv])}
$$
calcola il crittogramma $\textcolor{#ff82b2}{c=C(m, k_V[pub])}$ e spedisce a $V$ la tripla $\textcolor{#ff82b2}{< cert_U, c, f >}$.  $\textcolor{#ff82b2}{cert_U}$ contiene la chiave $\textcolor{#ff82b2}{k_U[pub]}$ e la specificazione della funzione $H$ utilizzata.
#### Verifica del certificato
Il destinatario $V$ riceve la tripla $\textcolor{#ff82b2}{< cert_U, c, f >}$ e verifica l'autenticità di $cert_U$ (e dunque di  $k_U[pub]$) utilizzando la propria copia di $K_CA[pub]$
#### Decifrazione e verifica della firma
$V$ decifra il crittogramma $c$ mediante la sua chiave privata e ottiene $$
\textcolor{#ff82b2}{m=D(c, k_V[priv])}
$$
$V$ verifica l'autenticità della firma apposta da $U$ su $m$ controllando che risulti $\textcolor{#ff82b2}{C(f, k_U[pub])=H(m)}$
#### Conclusioni
Il **punto debole** di questo meccanismo è rappresentato dai *certificati revocati* (non più validi). Le CA mettono a disposizione degli utenti un archivio pubblico contenente i certificati non più validi, la frequenza di controllo di questi certificati e la modalità della loro comunicazione agli utenti sono cruciali per la sicurezza.
Attacchi man in-the-middle possono essere evitati se si stabilisce unincontro diretto tra utente e CA nel momento in cui si istanzia il sistema asimmetrico dell'utente.
## Elliptic Curve Digital Signature Algorithm (ECDSA)
I **parametri globali** sono:
- Una curva prima $E_p(a, b)$
- Un punto base $B$ della curva, di ordine elevato (e primo)
#### Generazione delle chiavi
Ogni utente (firmatario) genera una coppia di chiavi:
- selezione un intero casuale $\textcolor{#ff82b2}{d\in[1,n-1]}$
- calcola $\textcolor{#ff82b2}{Q=d\:B}$
- $\textcolor{#ff82b2}{K[pub]=Q, \:\:K[priv]=d}$
#### Generazione della firma
Il firmatario:
1. Selezione un intero casuale $\textcolor{#ff82b2}{k\in[1, n-1]}$
2. Calcola il punto $\textcolor{#ff82b2}{P=(x, y)=k\:B}$ e pone $\textcolor{#ff82b2}{r=x \:\:mod\:\:n}$. Se $\textcolor{#ff82b2}{r=0}$, riparte dal *punto 1*
3. Calcola $\textcolor{#ff82b2}{t=k^{-1}\:\:mod\:\:n}$
4. Calcola $\textcolor{#ff82b2}{e=H(m))}$, dove $H$ è una funzione hash crittografica (SHA-1)
5. Calcola $\textcolor{#ff82b2}{s=k^{-1}(e+d\:r)\:\:mod\:\:n}$, dove $\textcolor{#ff82b2}{d}$ è la sua chiave privata. Se $\textcolor{#ff82b2}{s=0}$, riparte dal *punto 1*
6. La **firma** del messaggio è la coppia $\textcolor{#ff82b2}{\mathbf{(r,s)}}$
#### Verifica della firma
Il ricevente riceve il messaggio $\textcolor{#ff82b2}{m}$ e la firma $\textcolor{#ff82b2}{(r,s)}$
1. Verifica che $\textcolor{#ff82b2}{r}$ e $\textcolor{#ff82b2}{s}$ siano interi compresi tra $\textcolor{#ff82b2}{[1, n-1]}$
2. Calcola $\textcolor{#ff82b2}{e=H(m)}$
3. Calcola $\textcolor{#ff82b2}{w=s^{-1}\:\:mod\:\:n}$
4. Calcola $\textcolor{#ff82b2}{u_1=e\:w}$ e $\textcolor{#ff82b2}{u_2=r\:w}$
5. Calcola il punto $\textcolor{#ff82b2}{X=(x_1, y_1=u_1B+u_2Q)}$
6. Se $\textcolor{#ff82b2}{\mathbf{X=O}}$, *rifiuta la firma*, altrimenti calcola $\textcolor{#ff82b2}{v=x_1 \:\:mod\:\:n}$
7. *Accetta la firma* se e solo se $\textcolor{#ff82b2}{v=r}$
### Correttezza
Se il messaggio ricevuto è quello firmato dal legittimo mittente, allora
$$
\textcolor{#ff82b2}{s=k^{-1}(e+d\:\:r)\:\:mod\:\:n}
$$
Quindi $$
\begin{array}{l}
	\textcolor{#ff82b2}{k}&=s^{-1}(e+d\:r)\:\:mod\:\:n=(s^{-1}e+s^{-1}d\:r)\:\:mod\:\:n=\\
	&=(w\:e+w\:d\:r)\:\:mod\:\:n= \textcolor{#ff82b2}{(u_1+u_2\:d)\:\:mod\:\:n}
\end{array}
$$
Osserviamo che
$$
\textcolor{#ff82b2}{u_1B+u_2Q}=u_1B+u_2B=(u_1+u_2\:d)B= \textcolor{#ff82b2}{k\:B}
$$
Dunque risulta
$$
\textcolor{#ff82b2}{k\:B=u_1\:B+u_2\:Q}
$$
Al passo 6 della verifica, si pone $\textcolor{#ff82b2}{v=x_1 \:\:mod\:\:n}$, dove $\textcolor{#ff82b2}{x_1}$ è l'*ascissa del punto* $\textcolor{#ff82b2}{X=(x_1, y_1)=u_1\:B+u_2\:Q}$.
Al passo 2 della generazione della firma si pone invece $\textcolor{#ff82b2}{r=x \:\:mod\:\:n}$, dove $\textcolor{#ff82b2}{x}$ è l'*ascissa del punto* $\textcolor{#ff82b2}{k\:\:B}$.
La correttezza segue allora dal fatto che
$$
\textcolor{#ff82b2}{\mathbf{
	k\:B=u_1\:B+u_2\:Q
}}
$$
### Discussione
La verifica della firma è fatta sul valore $\textcolor{#ff82b2}{r}$ (ascissa del punto $\textcolor{#ff82b2}{k\:B}$), che *non dipende dal messaggio* firmato. Il valore $\textcolor{#ff82b2}{s}$ della firma invece dipende da $\textcolor{#ff82b2}{k}$, dall'*hash del messaggio* e dalla *chiave privata* dell'utente che firma. $\textcolor{#ff82b2}{s}$ è utilizzato per 'ricostruire' $r$ durante la verifica della firma, data la difficoltà del problema del logaritmo discreto su curve ellittiche, è computazionalmente improponibile recuperare $\textcolor{#ff82b2}{k}$ da $\textcolor{#ff82b2}{r}$, o $\textcolor{#ff82b2}{d}$ da $\textcolor{#ff82b2}{Q}$.
L'intero $\textcolor{#ff82b2}{k}$ deve essere *generato casualmente* ed essere *unico per ciascuna firma*. In caso contrario, è possibile recuperare la chiave privata $\textcolor{#ff82b2}{d}$, se abbiamo $\textcolor{#2e80f2}{m}$ e $\textcolor{#2e80f2}{m'}$ messaggi noti firmati usando lo stesso $\textcolor{#2e80f2}{k}$ (non noto), dato che il primo valore della firma, $\textcolor{#2e80f2}{r}$, dipende solo da $\textcolor{#2e80f2}{k}$, risulta:
- firma su $m$: $\textcolor{#ff82b2}{(r, s)}$
- firma su $m'$: $\textcolor{#ff82b2}{(r, s')}$
- siano $\textcolor{#2e80f2}{e=H(m)}$, e $\textcolor{#2e80f2}{e'=H(m')}$, risulta $$
\textcolor{#2e80f2}{\begin{array}{l}
	s=k^{-1}(e+d\:r)\\
	s'=k^{-1}(e'+d\:r)
\end{array}}
$$
Da $\textcolor{#ff82b2}{(s-s')=k^{-1}(e-e')}$, il crittoanalista può trovare $$
\textcolor{#2e80f2}{k= \frac{e-e'}{s-s'}}
$$
Dato che $\textcolor{#ff82b2}{k=k^{-1}(e+d\:r)}$, il crittoanalista può infine trovare la chiave privata $$
\textcolor{#2e80f2}{d=\frac{s\:k-e}{r}}
$$
(tutte le operazioni sono eseguite in modulo $n$) Questa implementazione errata è stata sfruttata, per esempio, per estrarre la firma digitale usata nella console PS3.