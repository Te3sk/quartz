---
title: Cifrari a Chiave pubblica
alias: asimmetrici
tags: #critto
---
##### 2022/10/25 #date 
- #### One-Time Pad (1917)
Il cifrario [[Crittografia/Cifrari Perfetti#One-Time Pad|One-Time Pad]] <span style="color:#ff82b2"><i>non può essere decifrato senza conoscere la chiave</i></span>, è assolutamente sicuro ma richiede una nuova chiave segreta per ogni messaggio che sia perfettamente casuale e lunga come il messaggio.
- #### AES
Il cifrario [[Crittografia/AES (Advanced Encryption Standard)]] è lo standard per le comunicazioni riservate ma "non classificate", è pubblicamente noto e realizzabile in hardware su computer di ogni tipo e richiede chiavi brevi (128 o 256 bit $\approx$ qualche decina di caratteri).
## Il problema dello scambio della chiave
<span style="color:#ff82b2"><i>La chiave serve per comunicare in sicurezza, ma deve essere stabilita comunicando in sicurezza senza poter ancora usare il cifrario</i></span>
### TTP
$N$ utenti vogliono comunicare tra loro su un canale condiviso, il crittoanalista può vedere i messaggi scambiati tra gli utenti ma non li può decifrare.
 - <span style="color:#ff82b2"><i>Soluzione ingenua:</i></span> ogni utente memorizza $N-1$ chiavi diverse, condivise con ciascun altro utente. Ci sarà quindi un quantitativo quadratico di chiavi, da generare e scambiare
 - <span style="color:#ff82b2"><i>Soluzione alternatica:</i></span> ricorrere ad un $\textcolor{#2e80f2}{\text{trusted 3rd party (TTP)}}$, nel quale ogni utente si deve ricordare una sola chiave, ed il sistema TTP gestisce la creazione delle chiavi condivise tra le coppie di utenti
<span style="color:#ff82b2"><b>Problemi:</b></span>
1. TTP deve essere sempre online
2. TTP conosce tutte le chiave: è un unico punto di errore per l'intero sistema
## Crittografia a chiave pubblica
> **Obiettivo:** Permettere a tutti di inviare messaggi cifrati a un individuo, ma abilitare solo lui a decifrarli
 
Nel 1976 Diffie e Hellman propongono alla comunità scientifica anche la definizione di <span style="color:#ff82b2"><i>crittografia a chiave pubblica</i></span> (ma senza averne un'implementazione completa).
Mentre nei cifrari simmetrici la chiave di cifratura è uguale a quella di decifrazione ed è nota solo ai due partner, in quelli a chiave pubblica (<span style="color:#ff82b2"><b>asimmetrici</b></span>), le operazioni di cifratura e decifrazione sono pubbliche e utilizzano due <span style="color:#ff82b2"><i>chiavi diverse</i></span>:
- $\textcolor{#ff82b2}{k_{pub}}$ per cifrare: è <span style="color:#ff82b2"><i>pubblica</i></span>, nota a tutti
- $\textcolor{#ff82b2}{k_{priv}}$ per decifrare: è <span style="color:#ff82b2"><i>privata</i></span>, nota solo al ricevente
Esiste una coppia $\textcolor{#ff82b2}{<k_{pub}, k_{priv}>}$ per ogni utente dal sistema, scelta da questi nella sua veste di possibile destinatario.
### Requisiti
1. <span style="color:#2e80f2"><b>Correttezza della cifratura/decifrazione:</b></span> $\textcolor{#ff82b2}{\forall}$ possibile messaggio $\textcolor{#ff82b2}{m}$ si ha
$$
\textcolor{#ff82b2}{D(C(m, k[pub]), k[priv])=m}
$$
	ovvero il destinatario deve aver la possibilità di interpretare qualsiasi messaggio che gli altri utenti decidano di spedirgli
2. <span style="color:#2e80f2"><b>Efficienza e sicurezza del sistema:</b></span> La sicurezza e l'efficienza del cifrario dipendono dalle funzioni $C$ e $D$, e dalla relazione che esiste tra le chiavi $k[priv]$ e $k[pub]$ di ogni coppia, quindi:
	1. La coppia $\textcolor{#ff82b2}{\langle k[priv],k[pub]\rangle}$ è <span style="color:#ff82b2"><i>facile da generare</i></span>, e deve risultare <span style="color:#ff82b2"><i>impossibile che due utenti scelgano la stessa chiave</i></span>
	2. Dati $\textcolor{#ff82b2}{m}$ e $\textcolor{#ff82b2}{k[pub]}$, per il mittente è <span style="color:#ff82b2"><i>facile da calcolare</i></span> il crittogramma $\textcolor{#ff82b2}{c=C(m,k[pub])}$
	3. Dati $\textcolor{#ff82b2}{c}$ e $\textcolor{#ff82b2}{k[priv]}$, per il destinatario è <span style="color:#ff82b2"><i>facile da calcolare</i></span> il messaggio originale $\textcolor{#ff82b2}{m=D(c,k[priv])}$
	4. Pur conoscendo il crittogramma $c$, la chiave pubblica $k[pub]$ e le funzioni $C$ e $D$, per il crittoanalista è <span style="color:#ff82b2"><i>difficile risalire al messaggio</i></span> $\textcolor{#ff82b2}{m}$
La proprietà 1 garantisce la correttezza del processo complessivo di cifratura e decifrazione, la proprietà 2.1 esclude che due utenti possano avere le stesse chiavi, le proprietà 2.2 e 2.3 assicurano che il processo possa essere di fatto adottato e la proprietà 2.4 garantisce la sicurezza del cifrario
### Funzione di cifratura (One-way trap-door)
La funzione di cifratura $C$ deve essere <span style="color:#ff82b2"><b>one-way</b></span>, quindi <span style="color:#ff82b2"><i>facile da calcolare</i></span> ma <span style="color:#ff82b2"><i>difficile da invertire</i></span>, deve però contenere un meccanismo segreto detto <span style="color:#ff82b2"><b>trap-door</b></span> che ne consenta la facile invertibilità solo a chi conosce il meccanismo ($k[priv]$).
Se non si conosce $k[priv]$, <span style="color:#ff82b2"><i>l'inversione richiede la soluzione di un problema NP-arduo</i></span>, per cui non si conosce un algoritmo polinomiale. Tre di queste funzioni sono particolarmente rilevanti in crittografia:
- #### Fattorizzazione
	Dati due interi $\textcolor{#ff82b2}{p}$ e $\textcolor{#ff82b2}{q}$, calcolare $\textcolor{#ff82b2}{n=p\times q}$ è polinomialmente facile. Invertire la funzione significa <span style="color:#ff82b2"><i>ricostruire</i></span> $\textcolor{#ff82b2}{p}$ e $\textcolor{#ff82b2}{q}$ <span style="color:#ff82b2"><i>a partire da</i></span> $\textcolor{#ff82b2}{n}$, il che è univocamente possibile solo se $p$ e $q$ <span style="color:#ff82b2"><i>sono primi</i></span>. Tale inversione richiede <span style="color:#ff82b2"><b>tempo esponenziale</b></span> per quanto è noto fino ad oggi, anche se non vi è ancora dimostrazione che il problema sia NP-arduo. 
	Se però si conosce uno dei due fattori (la chiave segreta) ricostruire l'altro è ovviamente facile.
- #### Calcolo della radice in modulo
	Dati gli interi $x, y, z$, calcolare la potenza $\textcolor{#ff82b2}{y=x^z \text{ mod }\:\,s}$ richiede <span style="color:#ff82b2"><i>tempo esponenziale</i></span> se si procede per [[Crittografia/Algebra Modulare#Esponenziazioni Successive|esponenziazioni successive]], eseguendo $\Theta(log_2 z)$ operazioni moltiplicazioni (lineare nella lunghezza di rappresentazione di $z$), l'algoritmo risulta complessivamente <span style="color:#ff82b2"><i>cubico</i></span>.
	Se $\textcolor{#ff82b2}{s}$ <span style="color:#ff82b2"><i>non è primo</i></span> e se ne ignora la fattorizzazione, <span style="color:#ff82b2"><i>invertire la funzione</i></span>, quindi calcolare $\textcolor{#ff82b2}{x = \sqrt[z]{y} \text{ mod }\:\,s}$ se sono noti $x, y, z$ richiede tempo esponenziale.
	Se però $\textcolor{#ff82b2}{x}$ <span style="color:#ff82b2"><i>è primo con</i></span> $\textcolor{#ff82b2}{s}$ e si conosce l'<span style="color:#ff82b2"><i>inverso</i></span> $\textcolor{#ff82b2}{v}$ di $z \text{ mod }\:\,\Phi(s)$ (cioè $z\cdot v\equiv1 \text{ mod }\:\,\Phi(s)$) si ha: $\textcolor{#ff82b2}{y^v \text{ mod }\:\,s = x^{zv} \text{ mod }\:\,s = x^{1+k\Phi(s)} \text{ mod }\:\,s=x \text{ mod }\:\,s}$. Si ricostruisce quindi in <span style="color:#ff82b2"><i>tempo polinomiale</i></span> $x$ calcolando $y^v \text{ mod }\:\,s$: in questo caso $\textcolor{#ff82b2}{v}$ <span style="color:#ff82b2"><b>è la chiave segreta</b></span>.
- #### Calcolo del logaritmo discreto
	La funzione potenza del punto precedente si può invertire anche rispetto a $z$ (dati $x,y,s$ trovare $z$ t.c. $y=x^z \text{ mod }\:\,s$). Operando il modulo, <span style="color:#ff82b2"><i>il problema è computazionalmente difficile</i></span> e non sempre ha soluzione. Se $\textcolor{#ff82b2}{s}$ <span style="color:#ff82b2"><i>è primo</i></span> esiste una soluzione per ogni $y$ se e solo se $\textcolor{#ff82b2}{x}$ <span style="color:#ff82b2"><i>è un generatore di</i></span> $\textcolor{#ff82b2}{\mathcal{Z}^*_s}$.
## Vantaggi & Svantaggi chiave pubblica
|                                                                                                                       Vantaggi | Svantaggi                                                                                                              |
| ------------------------------------------------------------------------------------------------------------------------------:| ---------------------------------------------------------------------------------------------------------------------- |
| Se gli utenti di un sistema sono $n$, il numero complessivo di chiavi (pubbliche e private) è $2n$ anziche $\frac{n(n-1)}{2}$. | Questi sistemi sono <span style="color:#ff82b2"><i>molto più lenti</i></span> di quelli basati sui cifrari simmetrici.Il rapporto tra le loro velocità sta tra due e tre ordini di grandezza e le realizzazione hardware sono meno competitive di quelle software |
|                                     <span style="color:#ff82b2"><i>Non è richiesto alcuno scambio segreto di chiavi</i></span> |  Sono esposti ad attacchi di tipo <span style="color:#ff82b2"><i>chosen plain-text</i></span>                                                                                                                       |

### Attacchi chosen plain-text
Un crittoanalista può:
1. scegliere un numero qualsiasi di messaggi in chiaro $$\textcolor{#ff82b2}{m_1,...,m_h}$$
2. *cifrarli* con la funzione pubblica $C$ e la chiave pubblica $\textcolor{#ff82b2}{k_{pub}}$ del destinatario, ottenendo i crittogrammi $$\textcolor{#ff82b2}{c_1,...,c_h}$$
3. confrontare qualsiasi messaggio cifrato $\textcolor{#ff82b2}{c^*}$ che viaggia verso il destinatario con i crittogrammi in suo possesso
	- Se $\textcolor{#ff82b2}{c^*}$ <span style="color:#ff82b2"><i>coincide</i></span> con uno di questi, il messaggio è automaticamente <span style="color:#ff82b2"><i>decifrato</i></span>
	- Se $\textcolor{#ff82b2}{c^*\ne c_i \:\:\forall\:\:i}$, il crittoanalista ha compunque <span style="color:#ff82b2"><i>acquisito un'informazione</i></span> importante, cioè che il messaggio è diverso da quelli che lui ha scelto
L'attacco è particolarmente pericoloso se il crittoanalista sospetta che il destinatario debba ricevere un messaggio particolare ed è in attesa di vedere quando questo accada.
## Il cifrario [[Crittografia/RSA|RSA]]
Si basa sulla moltiplicazione di due numeri primi $p,q$
- calcolare $n=p\times q$ è <span style="color:#ff82b2"><i>facile</i></span>, richiede tempo quadratico nella lunghezza della loro rappresentazione
- ricostruire $p$ e $q$ partendo da $n$ richiede un tempo <span style="color:#ff82b2"><i>(sub)esponenziale</i></span>
	- anche se non vi è dimostrazione del fatto che il problema sia NP hard, l'esistenza di un algoritmo polinomiale è assai improbabile
<span style="color:#ff82b2"><i>trap-door -></i></span> se si conosce uno dei due fattori, ricostruire l'altro è facile.
## [[Crittografia/ElGamal|Cifrario di ElGamal]] (1985)
Si basa su un sistema a chiave pubblica basato sul problema del logaritmo discreto, questo cifrario usa l'[[Crittografia/Algebra Modulare|algebra modulare]].
## [[Crittografia/Cifrari ibridi|Cifrari ibridi]]
Poiché i cifrari simmetrici e asimmetrici presentano pregi e difetti complementari, nei protocolli crittografici esistenti si tende ad adottare un approccio ibrido che combina i pregi ed esclude i difetti delle due famiglie.
Si usa un <span style="color:#ff82b2"><i>cifrario a chiave segreta</i></span> (AES) per le comunicazioni di massa e un <span style="color:#ff82b2"><i>cifrario a chiave pubblica</i></span> per <span style="color:#ff82b2"><b>scambiare le chiavi segrete</b></span> relative al primo, senza incontri fisici tra gli utenti.
La trasmissione dei messaggi lunghi avviene ad alta velocità (con cifrari simmetrici), mentre è lento lo scambio delle chiavi segrete che sono corte (con cifrari a chiave pubblica).
- le chiavi sono composte al massimo da qualche decina di byte
- l'attacco chosen plain-text è risolto se l'informazione cifrata con la chiave pubblica (chiave segreta dell'AES) è scelta in modo da <span style="color:#ff82b2"><i>risultare imprevedibile al crittoanalista</i></span>.
La chiave pubblica deve essere estratta da un certificato digitale valido, per evitare attacchi <span style="color:#ff82b2"><i>man-in-the-middle</i></span>.
### Realizzazioni
#### Merkle
La difficoltà di inversione della funzione di cifratura era basata sulla risoluzione del problema dello zaino (NP-Hard). Il cifrario è stato violato per altra via, successivi cifrari basati sullo stesso problema sono tuttora inviolati
#### Rivest, Shamir e Adelman -> RSA (1978)
Fonda la sua sicurezza sulla difficoltà di fattorizzare grandi nummeri interi (problema che non è comunque NP-Hard). É ad oggi inviolato, ed è stato il primo cifrario asimmetrico di largo impiego
#### ElGamal
Fonda la sua sicurezza sulla difficoltà di calcolare il logaritmo discreto (-> ECC)