---
title: Cifrari Simmetrici per la comunicazione di massa
tags: #critto
---
##### 2022/10/18 #date 
## Intro
Offrono **sicurezza computazionale**, nascondono l'informazione se il crittoanalista ha accesso a risorse computazionali limitate (**polinomiale**) e se [[Crittografia/Teoria della Complessità#Classe P e NP|P != NP]].
## Advanced Encryption Standard ([[Crittografia/AES (Advanced Encryption Standard)|AES]])
È lo standard per le comunicazione riservate ma *non classificate*. Pubblicamente noto e realizzabile in hardware su computer di ogni tipo. Le chiavi sono *brevi* ($\approx$ 10 caratteri, 128 o 256 bit) ed il cifrario è **simmetrico**, la stessa chiave è usata per cifrare e decifrare.
Il messaggio è *diviso in **blocchi** lunghi come la chiave*, e la chiave è usata per decifrare il singolo blocco.
## Principio di Shannon
La sicurezza dei cifrari a chiave segreta o simmetrici è basata su 2 principi:
- **Diffusione:** *alterare la struttura del testo in chiaro "spargendone" i caratteri su tutto il testo cifrato*, il testo in chiaro si deve quindi distribuire su tutto il crittogramma, ogni carattere del crittogramma dipende da tutti i caratteri del blocco del messaggio.
- **Confusione:** *combinare in modo complesso il messaggio e la chiave per non permettere al crittoanalista di separare queste due sequenze mediante un'analisi del crittogramma*. Il testo in chiaro e la chiave si devono quindi combinare nel crittogramma in modo complesso per impedire al cirttoanalista di separare le due informazioni durante l'analisi del crittogramma. La chiave deve essere ben distribuita sul testo cifrato e ogni bit del crittogramma deve dipendere da tutti i bit della chiave.
Queste due proprietà sono state pensate per resistere agli attacchi di crittoanalisi statistica.
## Data Encryption Standard ([[Crittografia/DES (Data Encryption Standard)|DES]])
È stato il primo cifrario *ufficialmente certificato* (*NIST*) per la protezione delle comunicazioni non classificate. Prima, quando la crittografia era usata principalmente dalle aziende, solo chi progettava rilasciava il sistema crittografico ne garantiva la sicurezza.
## Possibili upgrade del [[Crittografia/DES (Data Encryption Standard)|DES]]
Le variazioni del DES hanno sostanzialmente lo scopo di *preservare la semplicità* del cifrario ed allo stesso tempo *espandere lo spazio delle chiavi*
### Scelta indipendente delle chiavi
Le 16 sottochiavi di 48 bit utilizzate nelle successive fasi vengono scelte indipendentemente l'una dall'altra, per un totale di 786 bit anziché 56, il che rende impraticabile un attacco esauriente sulle chiavi.
### Cifratura multipla
L'**idea** è quella di concatenare più copie del DES che usano chiavi diverse. Abbiamo visto che in generale questo non garantisce un aumento della sicurezza del cifrario complessivo rispetto ai componenti. Nel caso del DES però, la concatenazione di più copie del cifrario non è equivalente a un'applicazione singola del cifrario
	Date 2 chiavi arbitrari $k_1$ e $k_2$ $$C_{DES}(C{DES}(m,k_1),k_2)\textcolor{#ff82b2}{\ne}X_{DES}(m,k_3)$$
	Per qualsiasi messaggio $m$ e qualsiasi chiave $k_3$
	Due chiavi di 56 bit -> una chiave di *57* bit
#### Attacchi "meet in the middle"
$$
c=C_{DES}(C_{DES}(m,k_1),k_2) \:\:\:\:\:\:\:\: D_{DES}(c,k_2)=C_{DES}(m,k_1)
$$
Data una coppia $<m,c>$
1. Per ogni $k_1$, si calcola e si salva $C_{DES}(m,k_1)$
2. Per ogni $k_2$, si calcola $D_{DES}(c,k_2)$ e si cerca nella tabella

**COSTO:** $\textcolor{#f77ead}{O(2^b+2^b)op. =O(2^{b+1})op.}$ dove $\textcolor{#f77ead}{b}$ sono i bit della chiave
##### 2022/10/24 #date
## Alternative al [[Crittografia/DES (Data Encryption Standard)|DES]]
### Triple Data Encryption Algorithm (TDEA)
La sicurezza di questi cifrari è pari ad una chiave di $112$ bit, sono stati certificati dal *NIST* fino al 2005, ma dal 2001 il nuovo standard per la cifratura simmetrica è diventato l'*AES*. Prima di questo occorreva trovare un buon compromesso tra sicurezza e semplicità d'impiego. La proposta più importante fu il **TDEA**(*Triple Data Encryption Algorithm*) con 2(*2TDEA*) o 3(*3TDEA*) chiavi.
#### 3TDEA
$$\textcolor{#ff82b2}{c = C_{DES}(D_{DES}(C_{DES}(m, k_1), k_2), k_3)} \rightarrow \text{chiavi a } 168 \text{ bit}$$
3 chiavi a 56 bit -> sicurezza pari ad una chiave a 112 bit (non 168)
##### Attacchi Meet In The Middle
$D_{DES}(c, k_3)=D_{DES}(C_{DES}(m, k_1), k_2)$
$C_{DES}(D_{DES}(c, k_3), k_2) = C_{DES}(m, k_1)$
Data una coppia $<m, c>$
1. per ogni $k_1$, si calcola e si salva $C_{DES}(m, k_1)$ in una tabella
2. per ogni *coppia* di chiavi $\textcolor{#f77ead}{k_2}, k_3$ si calcola $\textcolor{#f77ead}{C_{DES}(}D_{DES}(c, k_3)\textcolor{#f77ead}{, k_2)}$ e si cerca nella tabella
**costo:** $O(2^b+2^b\cdot 2^b) op = O(2^b)op$, dove $b$ è il numero di bit della chiave ($2^{112}$ coppie di chiavi)
#### 2TDEA
$$\textcolor{#ff82b2}{c = C_{DES}(D_{DES}(C_{DES}(m, k_1), k_2), k_1)} \rightarrow \text{chiavi a } 112 \text{ bit}$$
dove $k_1$ e $k_2$ sono chiavi indipendenti di 56 bit segreti, la *chiave complessiva risulta quindi di 112* bit più 16 di parità.
La decifrazione era invece definita come:
$$
\textcolor{#2e80f2}{m = D_{DES}(C_{DES}(D_{DES}(c,k_1),k_2),k_1)}
$$
##### Attacchi Meet In The Middle
$D_{DES}(c, k_1)=D_{DES}(C_{DES}(m, k_1), k_2)$
$C_{DES}(D_{DES}(c, k_1), k_2)=C_{DES}(m, k_1)$
Hanno lo stesso **costo** di un attacco esauriente sulle chiavi
### RC5 (Ron's Code 5)
Fu il successoree di altri quattro cifrari di cui RC2 e RC4, segue le linee di base del DES, ma lascia maggiote libertà agli utenti. É un cifrario a *blocchi di 64 bit*, con chiave $\textcolor{#2e80f2}{c}\times 32$ bit, organizzato in $\textcolor{#2e80f2}{r}$ fasi: i parametri $\textcolor{#2e80f2}{c}$ e $\textcolor{#2e80f2}{r}$ possono essere **scelti dall'utente**.
Dalla chiave vengono generate $\textcolor{#ff82b2}{2r+2}$ *sottochiavi* $k[0], \dots, k[2r+1]$ di 32 bit ciascuna: $k[0]$ e $k[1]$ sono usate in una relaborazione iniziale, $k[i]$ e $k[2i+1]$ nella fase $i$-esima. 
Ogni blocco del messaggio viene decomposto in 2 sottoblocchi $X$ e $Y$ di 32 bit, si inizia ponendo
- $\textcolor{#ff82b2}{X}\leftarrow(X+[0])\text{ mod }\:\,2^{32}$
- $\textcolor{#ff82b2}{Y}\leftarrow(Y+k[1])\text{ mod }\:\,2^{32}$
I sottoblocchi subiscono poi una serie di informazioni nel corso delle $r$ fasi, le operazione usate nel cifrario sono lo *shift ciclico*, lo *XOR* e *l'addizione* modulo $2^{32}$. É molto veloce e resiste con successo a tutti gli attacchi standard purché i valori dei parametri $c$ ed $r$ siano scelti opportunamente.
### IDEA (International Data Encrypting Algorithm)
Si divide il messaggio in *blocchi da 64 bit*, la *chiave è di 128 bit*, e da essa vengono estratte *52 sottochiavi di 16 bit* mediante una serie di shift ciclici di 25 posizioni (così si estraggono sempre chiavi diverse).
La **cifratura** si siviluppa in 8 fasi uguali tra loro. Le operazione usate nell'interno del cirario sono lo *shift ciclico*, lo *XOR*, *l'addizione* $\text{ mod }\:\,2^{16}$ e la *moltiplicazione* $\text{ mod }\:\,(2^{16}+1)$ (che è un numero primo): queste ultime due garantiscono l'interdipendenza tra bit di ingresso e di uscita.
## Cifrari a composizione di blocchi
I cifrari simmetrici prevedono un funzionamento a blocchi, la trasmissione risulta quindi in un certo senso *sincopata*, cioè più creare *problemi tecnici alla trasmissione* su reti ad alta velocità, ma soprattutto espone la comunicazione ad *attacchi fin qui non considerati*: infatti blocchi uguali nel messaggio producono blocchi cifrati uguali inducendo una periodicità nel crittogramma che fornisce utili informazioni per la crittoanalisi. I metodi proposti per ovviare al problema intervengono componendo i blocchi tra loro, il più diffuso è stato il [[Crittografia/CBC (Cipher Block Chaining)|Cipher Blcok Chaining(CBC)]]