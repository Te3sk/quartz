---
title: AES (Advanced Encryption Standard)
tags: #critto
---
##### 2022/10/24 #date 
**AES** è lo standard per le comunicazioni di massa dal 2001. È un cifrario a blocchi di 128 bit, con chiavi di 128, 192 o 256 bit
|           | chiave (bit) | dim. blocco | Numero di Fasi |
| --------- | ------------ | ----------- | -------------- |
| AES (128) | 128          | 128         | 10             |
| AES (192) | 192          | 128         | 12             |
| AES (256) | 256          | 128         | 14               |
Ogni fase opera su un blocco di 128 bit, logicamente organizzato come matrice bidimensionale $B$ di 16 byte
## Storia
- *Gennaio 1997* - NIST annuncia una competizione per selezionare il nuovo standard per la cifratura simmetrica. Furono proposti 15 algoritmi da team di tutto il mondo. Ognuno di questi venne analizzato da NIST ed, essendo pubblico, anche dagli altri team (più motivati ad attaccare i cifrari degli altri).
- *1998 / 1999* - Ci furono 2 workshop per discutere ed analizzare le proposte, dopo il secondo il NIST seleziona 5 finalisti per il "secondo round" della competizione
- *Aprile 2000* - Terzo workshop dove ogni team finalista presenta il proprio cifrario
- *Ottobre 2000* - NIST annuncia la vincita di **Rijndael**
- *Novembre 2001* - AES diventa il nuovo standard ufficiale e si conclude il processo di selezione
> [!obs] 
> Il NIST ha affermato che ciascuno dei 5 finalisti sarebbe stato un eccellente standrd, in nessuno sono state trovate vulnerabilità. La selezione del vincitore è stata basata su proprietà come efficienza, performance in hardware, flessibilità, etc...

## Struttura
É un *cifrario a blocchi*, che può essere efficientemente realizzato su una vasta gamma di processori e su smartcard, prevede che i blocchi in cui è diviso e la chiave segreta siano formati da 128 bit e chiavi di 128, 192 o 256 bit.
Come nel DES il processo di cifratura e decifrazione opera per fasi(10, 12 o 14 a seconda della lunghezza della chiave). Ogni fase impiega una propria *chiave locale* creta a partire dalla chiave segreta del cifrario con un opportuno processo di espansione e selezione. Un'ulteriore *chiave iniziale*, ricavata nello stesso modo, è impiegata all'inizio delle operazioni.
![[Crittografia/assets/AES_matriceB.jpg]]
La matrice $B$ del cifrario, dove i 16 byte $b_{i,j}$ che costituiscono gli elementi della matrice $B$ sono trasformati nel corso di ogni fase.
Spiegeremo le singole operazioni del cifrario nella versione con la chiave a 128 bit.
- ##### Trasformazione iniziale
	Il messaggio $m$ è caricato nella matrice $B$. Ogni byte di $B$ è posto in XOR (bit a bit) con il corrispondente della chiave iniziale
- ##### Fase ripetuta x10
	- **Op 1 -** Ogni byte della matrice $B$ è trasformato mediante una $S$-box. 
		La $S$-box differisce da [[DES (Data Encryption Standard)#S-Box|quella del DES]] per i criteri algebrici molto raffinati, criteri che assicurano la non linearità della trasformazione e la sua sicurezza. 
	- **Op 2 -** La matrice così ottenuta è permutata mediante shift ciclici sulle righe
		La prima riga (riga 0) della matrice $B$ resta inalterata mentre i byte contenuti nelle righe 1,2,3 sono soggetti a uno shift ciclico verso sinistra rispettivamente di 1,2 o 3 posizioni. 
	- **Op 3 -** Le colonne risultanti vengono trasformate con un'operazione algebrica
		Ogni colonna della matrice, trattata come vettore di 4 elementi, viene moltiplicata per una matrice prefissata $M$ di $4\times 4$ byte, dove la moltiplicazione tra byte è eseguita modulo $2^8$ e l'addizione è eseguita come XOR. Notiamo che la matrice $M$ è scelta in modo che ogni byte di una colonna di $B$ influenzi tutti i byte della colonna stessa.
	- **Op 4 -** Ogni byte della matrice risultante è posto in XOR con un byte della chiave locale per quella fase
		Ogni byte $b_{i,j}$ della matrice $B$ è trasformato come $\textcolor{#ff82b2}{b_{i,j}\leftarrow b_{i,j}\oplus k_{i,j}}$, dove $k_{i,j}$ è il corrispondente byte della chiave locale
Alcuni attacchi hanno mostrato che il cifrario presenta una **possibile debolezza** se le fasi sono meno di dieci: ciò potrebbe indurre a credere che 10 fasi possano essere insufficienti in futuro, ma è bene osservare che la versione originale di Rijndael prevede che il loro numero possa essere incrementato.
## Gestore delle chiavi
La chiave iniziale è caricata, per colonne, in una matrice $W$ di byte $4\times 4$. *La matrice è ampliata aggiungendo 40 colonne, generate ricorsivamente a partire dalle prime 4:* $\textcolor{#2e80f2}{W[0], W[1], W[2], W[3]}$.
![[Crittografia/assets/AES_matrix.png]]
$\textcolor{#2e80f2}{W[i]=W[i-4]\oplus W[1]}$, se $\textcolor{#2e80f2}{i}$ non è un multiplo di 4
$\textcolor{#2e80f2}{W[i]=W[i-4]\oplus \textcolor{#f77ead}{\textbf{T}}(W[i-1])}$, se $\textcolor{#2e80f2}{i}$ è un multiplo di 4, dove $\textcolor{#f77ead}{\textbf{T}}$ è la trasformazione non lineare (*s-box*)
La chiave $\textcolor{#f77ead}{k_i}$ per la $\textcolor{#f77ead}{i}$-esima fase è data dalle 4 colonne
$$
\textcolor{#f77ead}{W[4i], W[4i+1], W[4i+2], W[4i+3]}
$$
#### Funzione T
$$
\textcolor{#f77ead}{T(B_0B_1B_2B_3)}
$$
1. shift ciclico verso sinistra dei 4 byte di $\textcolor{#f77ead}{W[i]}$
2. si applica la s-box su ciascun byte
3. si calcola lo XOR del byte più a sinistra ($B_1$) con una costante di round. Il valore di $RC$ è differente per ogni round
$$
\begin{array}{l}
RC_1=1\\
RC_j=2RC_{j-1} & 2\leq j\leq 10 &(\text{moltiplicazione su } GF(2^8)\:) &&&&&&&&&&&&&&
\end{array}
$$
![[Crittografia/assets/AES_gestoreChiavi.png]]
## Trasformazione di fase
1. **Substitution byte :** ogni byte del blocco $B$ è trasformato mediante una *s-box*, una look-up table che contiene una *permutazione di tutti i 256 interi a 8 bit*
![[Crittografia/assets/AES_trasformazioniDiFase1.png]]
2. **Shift Rows :** i byte di ogni riga vengono shiftati verso sinitra di 0,1,2 e 3 posizioni, rispettivamente -> *i 4 byte di ogni colonna si disperdono su 4 colonne diverse*
![[Crittografia/assets/AES_trasformazioniDiFase2.png]]
3. **Mix Columns :** $a_{i,j}$ si trasforma *linearmente* in un *nuovo byte* $b_{i,j}$ che *dipende da tutti i byte della colonna* $\textcolor{#f77ead}{j}$
![[Crittografia/assets/AES_trasformazioniDiFase3.png]]
4. **Add Round Key :** Ogni byte della matrice è posto in *XOR bit a bit*, con un byte della *chiave locale di fase*
![[Crittografia/assets/AES_trasformazioniDiFase4.png]]
## Substituition byte
Ogni byte del blocco $B$ è trasformato mediante una s-box. La *s-box* è una matrice di $\textcolor{#f77ead}{16 \times 16}$ *byte*, che contiene una *permutazione di tutti i 256 interi a 8 bit* (da 0 a 255)
#### Costituizione algebrica
Ogni byte $b_{i,j}$ viene prima sostituito con il suo *inverso moltiplicativo in GF($\textcolor{#f77ead}{2^8}$)[->non linearità]*, e poi moltiplicato per una matrice di $8\times 8$ bit e sommato con un vettore colonna.
#### S-Box
$$
S\:BOX:\{0,1\}^8\to\{0,1\}^8
$$
È una *one-to-one function*, specificata con una tabella $16\times 16$, di 256 elementi (permutazione degli interi da 0 a 255) $b_{ij}=b_{ij_0}b_{ij_1}...b_{ij_7}$.
- Non ha punti fissi: $\forall\:x\in\{0,1\}^8,\:\:s-box(x)\ne x$
- Non ha punti inversi: $\forall\:x\in\{0,1\}^8,\:\:s-box(x)\ne\bar{x}$
## Shift Rows
I byte di ogni riga vengono shiftati ciclicamente verso sinistra di 0,1,2 e 3 posizioni, rispettivamente. In questo modo i *4 byte di ogni colonna si disperdono su 4 colonne diverse*.
![[Crittografia/assets/AES_shiftRows.png]]
## Mix Columns
Ogni *colonna* del blocco, trattata come un vettore di 4 elementi, viene *moltiplicata per una matrice* $\textcolor{#f77ead}{M}$ prefissata di $4\times 4$ bit. La *moltiplicazione è eseguita* $\textcolor{#f77ead}{mod \:\:2^8}$*, e la somma modulo 2* (operazioni del campo $GF(2^8)$). 
La matrice $M$ è scelta in modo che ciascun byte della colonna venga mappato in un nuovo byte che è funzione di tutti i 4 byte presenti nella colonna.
$$
\begin{array}{l}
\textcolor{#2e80f2}{a_{i,j}\text{ si trasforma in un valore }}
\textcolor{#f77ead}{b_{i,j}} 
\textcolor{#2e80f2}{\text{ che dipende da tutti i byte } a_{0,j},a_{1,j},a_{2,j},a_{3,j} \text{ della colonna}} &&&
\end{array} 
$$
Shift Rows e Mix Columns garantiscono **diffusione totale dopo sole due fasi:** ogni bit di output dipende da tutti i bit di inpu.
## Add Round Key
Ogni byte della matrice è posto in XOR bit a bit, con un byte della chiave locale di fase
## Sicurezza
Ad oggi *nessun attacco è stato in grado di compromettere AES* anche nella sua versione più semplice con chiave di 128 bit. **Tutti i bit della chiave sono bit di sicurezza**. Esistono attacchi più efficienti di un attacco esauriente sulle chiavi per le versioni di AES con 6 fasi, ma nessun attacco esauriente sulle chiavi per le versioni di AES con 6 fasi, ma nessun attacco è più efficiente se le fasi sono almeno 7.
Si conoscono *attacchi side-channel* che non sfruttano le caratteristiche del cifrario ma le possibili debolezze della piattaforma su cui esso è implementato.
### CBC: Cipher Block Chaining
**Soluzione:**
- si compongono i blocchi tra loro
- il cifrario risultante è ancora strutturato a blocchi, ma *blocchi uguali nel messggio vengono cifrati in modo diverso*, eliminandone così la periodicità
**Cifratura:** $c_i = C(m\oplus c_{i-1},k)$
**Decifrazione:** $m_i=c_{i-1}\oplus D(c_i,k)$
