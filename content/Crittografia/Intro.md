---
title: Introduzione
alias: critto-intro
tags: #critto
---
##### 2022/09/19 #date
[[Glossary#Crittografia|Significato della crittografia]]
# Due gruppi di persone
- Persone che applicano <span style="color:#ff82b2"><b>metodi di cifratura</b></span> alle loro conversazioni per renderle incomprensibili ai non autorizzati
- Persone che sviluppano <span style="color:#ff82b2"><b>metodi di crittoanalisi</b></span> per riportare alla luce informazioni contenute in quelle conversazioni
## Terminologia
- <span style="color:#ff82b2"><b>Crittografia</b></span> = metodi di cifratura
- <span style="color:#ff82b2"><b>Crittanalisi</b></span> = metodi di interpretazioni
- Crittografria + Crittoanalisi = <span style="color:#ff82b2"><b>Crittologia</b></span> = studio della comunicazione su canali non sicuri e relativi problemi
## Cifratura
- <span style="color:#ff82b2"><i>MSG</i></span> = insieme dei messaggi (testi in chiaro)
- <span style="color:#ff82b2"><i>CRITTO</i></span> = insieme dei crittogrammi (testi cifrati)
> [!def] Cifratura del messaggio
> Operazione con cui si trasforma un messaggio in chiaro **m** in un crittogramma **c** applicando una funzione $C:\:MSG\rightarrow CRITTO$
## Decifratura
> [!def] Decifrazione del crittogramma
> Operazione che permette di ricavare il messaggio in chiaro **m** a partire dal crittogramma **c** applicando una funzione $D:\:CRITTO\rightarrow MSG$

Le funzioni $\textcolor{#ff82b2}{C}$ e $\textcolor{#ff82b2}{D}$ sono una l'inversa dell'altra $$D(c)=D(C(m))=m$$
e la funzione $\textcolor{#ff82b2}{C}$ è <span style="color:#ff82b2"><i>iniettiva</i></span>: a messaggi diversi devono corrispondere crittogrammi diversi
## Livello di segretezza
La classificazione dei metodi crittografici (cifrari) in base al livello di segretezza. Ci sono principalmente due categorie:
- <span style="color:#ff82b2"><b>Cifrari per uso ristretto</b></span>
- <span style="color:#ff82b2"><b>Cifrari per uso generale</b></span>
### Cifrari per uso ristretto
Le funzioni di cifratura $\textcolor{#ff82b2}{C}$ e di decifrazione $\textcolor{#ff82b2}{D}$ sono tenute segrete in ogni loro aspetto. Sono impiegati principalmente per comunicazioni diplomatiche o militari. Non sono adatti per una crittografia di "massa".
### Cifrari per uso generale
Ogni codice segreto non può essere mantenuto tale troppo allungo. In un cifrario utilizzato da molti utenti, la parte segreta del metodo deve essere limitata a un'informazione aggiuntiva, <span style="color:#ff82b2"><b>la chiave</b></span>, nota solo alla coppia di utenti che stanno comunicando (il [[Crittografia/Antichi esempi#Cifrario di Cesare|Cifrario di Cesare]] non aveva chiave). Le regole devono essere <span style="color:#ff82b2"><i>pubbliche</i></span> e solo la chiave deve essere <span style="color:#ff82b2"><i>pubbliche</i></span>.
Le funzioni $\textcolor{#ff82b2}{C}$ e $\textcolor{#ff82b2}{D}$ sono pubblicamente note, si usa una chiave segreta $\textcolor{#ff82b2}{k}$
- diversa per ogni coppia di utenti
- inserita come parametro nelle funzioni di cifratura e decifrazione
$$c=C(m, k)\:\:\:\:\:\:\:\:\:m=D(c, k)$$
Se non si conosce $\textcolor{#ff82b2}{k}$, la conoscenza di $\textcolor{#ff82b2}{C}$ e $\textcolor{#ff82b2}{D}$ e del crittogramma $\textcolor{#ff82b2}{c}$ NON deve permettere di estrarre informazioni sul messaggio in chiaro
- Tenere segreta la chiave è più semplice che tenere segreto l'intero processo di cifratura e decifrazione
- Tutti possono impiegare le funzioni pubbliche $\textcolor{#ff82b2}{C}$ e $\textcolor{#ff82b2}{D}$ scegliendo chiavi diverse
- Se un crittoanalista entra in possesso di una chiave occorre solo generarne una nuova, le funzioni $\textcolor{#ff82b2}{C}$ e $\textcolor{#ff82b2}{D}$ rimangono inalterate
- Molti cifrari in uso (3DES, RC5, IDEA, AES) sono a chiave segreta
### Le chiavi segrete
Se la segretezza dipende unicamente dalla chiave il numero delle chiavi deve essere così grande da essere praticamente <span style="color:#ff82b2"><i>immune ad ogni tentativo di</i></span> [[Glossary#Brute force attack|brute force]], inoltre la chiave segreta deve essere scelta in modo casuale

> [!obs] 
> La segretezza può essere violata con altre tecniche di crittoanalisi. Esistono cifrari più sicuri di altri, pur basandosi su uno spazio di chiavi molto più piccolo:
> 1. un cifrario complicato non è necessariamente un cifrario sicuro
> 2. mai sottovalutare la bravura del crittoanalista

## Crittoanalista
Può avere un <span style="color:#ff82b2"><b>comportamento passivo</b></span>, limitandosi ad ascoltare la comunicazione, o un <span style="color:#ff82b2"><b>comportamento attivo</b></span>, agendo sul canale disturbando la comuicazione o modificando il contenuto dei messaggi.
### Attacchi a un sistema crittografico
Hanno l'obiettivo di <span style="color:#ff82b2"><i>forzare</i></span> il sistema. Il metodo e il livello di pericolosità dipendono dalle informazioni in possesso al crittoanalista
- ##### Chiper Text Attack (solo testo cifrato)
Il crittoanalista rileva sul canale una serie di crittogrammi $c_1,...,c_r$
- ##### Known Plain-Text Attack (testo in chiaro noto)
Il crittoanalista conosce una serie di coppie $(m_1, c_1),...,(m_r, c_r)$
- ##### Attacco chosen plain-text
Un crittoanalsista può scegliere un nuemero qualsiasi di messaggi in chiaro $m_1, ...,m_h$, <span style="color:#ff82b2"><b>cifrarli</b></span> con la funzione pubblica $\textcolor{#ff82b2}{C}$ e la chiave pubblica $k_{pub}$ del destinatario, ottenendo i crittogrammi $c_1,...,c_h$, quindi può confrontare qualsiasi messaggio cifrato $c^*$ che viaggia verso il destinatario con i crittogrammi in suo possesso.
#### Attacchi "Man in-the-middle"
Il crittoanalista si installa sul canale di comunicazione:
- interrompe le comunicazioni dirette tra i due utenti
- le sostituisce con messaggi prorpi
- convince ciascun utente che tali messaggi provengano legittimamente dall'altro
### Attacchi
L'attacco al sistema può essere portato a termine con pieno successo (scoprendo la funzione **D**) o con successo più limitato (scoprendo solo qualche informazione su un particolare messaggio, che può essere sufficiente a comprendere il significato del messaggio)
## Cifrari Inattaccabili (perfetti)
[[Crittografia/Cifrari Perfetti|Cifrari Perfetti]]
Cifrari perfetti esistono, ma richiedono operazioni estremamente complesse e sono utilizzati solo in condizioni estreme.
Claude Shannon, nel 1945, teorizza un cifrario nel quale il messaggio in chiaro e il crittogramma risultano del tutto scorrelati tra loro, nessuna informazione sul testo in chiaro può filrtare dal crittogramma, quindi le conoscenze del crittoanalista non cambiano dopo aver osservato un crittogramma sul canale.
### One time pad
Il cifrario [[Crittografia/Cifrari Perfetti#One-Time Pad|One-Time Pad]] è assolutamente sicuro ma richiede una chiave segreta per ogni messaggio, che sia perfettamente casuale e lunga come il messaggio da scambiare. Il problema sta infatti nello scambio della chiave.
## Situazione odierna
I cifrari diffusi, non sono perfetti, ma sono <span style="color:#ff82b2"><b>dichiarati sicuri</b></span> perché sono rimasti inviolati dagli attacchi degli esperti e per risolverli è necessario risolvere problemi matematici estremamente difficili.
### Cifrari dichiarati sicuri
In questo caso, il prezzo da pagare per forzare il cifrario è troppo alto perché valga la pena sostenerlo. L'operazione richiede di impiegari grossi calcolatori per tempi estremamente lunghi, è quindi praticamente impossibile forzare questi cifrari.
Tuttavia non è ancora chiaro se la risoluzione di questi problemi richieda <span style="color:#ff82b2"><b>necessariamente</b></span> tempi lunghi, o questi tempi sono dovuti alla nostra <span style="color:#ff82b2"><b>incapacità</b></span> di individuare metodi di risoluzione matematica più efficienti.
### Cifrari di oggi - <span style="color:#ff82b2"><b>Advanced Encryption Standard</b></span> (<span style="color:#ff82b2"><i>AES</i></span>)
[[Crittografia/AES (Advanced Encryption Standard)|Cifrario AES (Advanced Encryption Standard)]]
É lo standard per le comunicazioni riservate ma "non classificate", è pubblicamente noto e realizzabile in hardwere su computer di ogni tipo. Le chiavi sono brevi (qualche decina di caratteri, 128 o 256 bit). Il cifrario è simmetrico (a blocchi), la stessa chiave è usata per cifrare e per decifrare, il messaggio è diviso in blocchi come la chiave e la chiave è usata per trasformare un blocco del messaggio in un blocco del crittogramma. 
Le chiavi non sono stabilite direttamente dagli interlocutori, ma dai dispositivi che usano per comunicare, su internet si costruisce una nuova chiave per ogni sessione. È importante che lo scambio della chiave avvenga con facilità e sicurezza, un'intercettazione nello scambio pregiudica il sistema.
### Distribuzione delle chiavi
#### Protocollo Diffie-Hellman
Nel '76 elaborato da Merkle, Hellman e Diffie un algoritmo per generare e scambiare una chiave su un canale sicuro, senza la necessità che le due parte si siano scambiate qualsiasi informazione in precedenza. Questo protocollo è ancora largamente usato nei protocolli crittografici su internet.
Diffie e Hellman propongono inoltre alla comunità la definizione di <span style="color:#ff82b2"><b>crittografia a chiave pubblica</b></span>, ma senza averne un'implementazione pratica. Questo rivoluziona il modo di concepire le comunicazioni segrete
### Cifrari simmetrici
Nei cifrari simmetrici, la chiave di cifratura è uguale a quella di decifrazione (o l'una può essere facilmente calcolata dall'altra) ed è <span style="color:#ff82b2"><b>nota solo ai due partner</b></span>, che la scelgono di comune accordo e la mantengono <span style="color:#ff82b2"><b>segreta</b></span>.
### Cifrari a chiave pubblica
<span style="color:#ff82b2"><b>Obiettivo:</b></span> permettere a tutti di inviare messaggi cifrati ma abilitare solo il ricevente a decifrarli.
Le operazioni di cifratura e decifrazione sono pubbliche e utilizzano <span style="color:#ff82b2"><b>due chiavi diverse</b></span>:
- $k_{pub}$ per <span style="color:#ff82b2"><i>cifrare</i></span>, è pubblica e nota a tutti
- $k_{priv}$ per <span style="color:#ff82b2"><i>decifrare</i></span>, è privata e nota solo al ricevente
La <span style="color:#ff82b2"><i>cifratura</i></span> di un messaggio $\textcolor{#ff82b2}{m}$ da inviare è eseguita da qualunque mittente come $$c = C(m, k_{pub})$$
dove la chiave $k_{pub}$ e la funzione di cifratura $C$ sono note a tutte.
La <span style="color:#ff82b2"><i>decifrazione</i></span> è eseguita dal ricevente come $$m=D(c, k_{priv})$$
dove la funzione di decifrazione $D$ è nota a tutti, ma $k_{priv}$ non è disponibile agli altri.
La cifratura è quindi accessibile a tutti perché tutti conoscono la chiave pubblica, mentre la decifrzione è accessibile solo a chi possiede la chiave privata.
La funzione di cifratura deve essere una funzion <span style="color:#ff82b2"><b>one-way-trap-door</b></span>: calcolare $c=C(m,k_{pub})$ è computazionalmente facile. Invece decifrare $c$ è computazionalmente difficile, a meno che non si conosca un meccanismo segreto, rappresentato da $k_{priv}$ (<span style="color:#ff82b2"><i>trap-door</i></span>).
#### Comunicazione molti a uno
Tutti gli utenti possono inviare in modo sicuro messaggi ad uno stesso destinatario, cifrandoli con la funzione $\textcolor{#ff82b2}{C}$ e la chiave $k_{pub}$ che sono pubbliche. Solo il destinatario può decifrare i messaggi. Un crittoanalista non può ricavare informazioni sui messaggi pur conoscendo $\textcolor{#ff82b2}{C}$, $\textcolor{#ff82b2}{D}$, e $k_{pub}$.
![ComunicazioneMoltiAUno](Crittografia/assets/ComMoltiAUno.png)
|                                                                                                                                                                        <span style="color:#ff82b2"><b>Vantaggi</b></span> | <span style="color:#ff82b2"><b>Svantaggi</b></span>                                                                                                                                                                                          |
| -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Se gli utenti di un sistema sono $\textcolor{#ff82b2}{n}$, il numero complessivo di chiavi (pubbliche e private) è $\textcolor{#ff82b2}{2n}$ invece che $\textcolor{#ff82b2}{\frac{n(n-1)}{2}}$. Inoltre non è richeisto alcuno scambio segreto di chiavi. | Questi sistemi sono molto più <span style="color:#ff82b2"><b>lenti</b></span> di quelli basati su cifrari simmetrici e sono esposti ad attacchi di tipo *[[Crittografia/Intro#Attacchi a un sistema crittografico\|chosen plain-text]]*. |
### Cifrari ibridi
Si usa un <span style="color:#ff82b2"><i>cifrario a chiave segrete</i></span> (AES) per le *comunicazioni di massa*, e un <span style="color:#ff82b2"><i>cifrario a chiave pubblica</i></span> per <span style="color:#ff82b2"><i>scambiare le chiavi segrete</i></span> relative al primo senza incontri fisici tra utenti. La trasmissione dei messaggi lunghi avviene ad alta velocità, mentre è lento lo scambio delle chiavi segrete, le quali sono composte al massimo da qualche decina di byte.
L'[[Crittografia/Intro#Attacchi a un sistema crittografico|attacco chosen plain-text]] è risolto se l'informazione cifrata con la chiave pubblica (chiave segreta dell'AES) è scelta in modo da risultare <span style="color:#ff82b2"><i>imprevedibile al crittoanalista</i></span>.
### Applicazioni su rete
Oltre alla segretezza delle comunicazioni, i sistemi crittografici attuali devono garantire:
1. L'<span style="color:#ff82b2"><b>identificazione</b></span> dell'utente
2. L'<span style="color:#ff82b2"><b>autenticazione</b></span> di un messaggio
3. La <span style="color:#ff82b2"><b>firma digitale</b></span>
[[Crittografia/Firma Digitale#Identificazione, autenticazione, Firma digitale|Identificazione, autenticazione, Firma digitale]]
#### Identificazione
Il sistema deve accertare l'identità di chi richiede di accedere ai suoi servizi
#### Autenticazione
Il ricevente deve accertare che il messaggio ricevuto è stato effettivamente spedito dal suo interlocutore, <span style="color:#ff82b2"><i>un intruso non deve potersi spacciare per un altro utente</i></span>,
il ricevente deve poter stabilire che il messaggio non è stato modificato o sostituito durante la trasmissione
#### Firma digitale
Una volta apposta la <span style="color:#ff82b2"><i>firma</i></span>, il mittente non può ricusare la paternità di un messaggio spedito. Il ricevente può dimostrare a terzi che il messaggio di ricevuto è di chi si aspetta.
### Altre applicazioni
- Trasmissione protetta di dati sulla rete (protocollo [[Crittografia/SSL Protocol|SSL]])
- terminali per le carte bancarie
- moneta elettronica (cryptovalute)
- dimostrazioni a conoscenza zero
- applicazioni crittografiche dei fenomeni di meccanica quantistica