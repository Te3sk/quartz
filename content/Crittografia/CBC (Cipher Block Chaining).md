---
title: CBC (Cipher Block Chaining)
tags: #critto
---
Il CBC segue il principio della diffusione di Shannon inducendo una *dipendenza di posizione* tra il blocco in elaborazione e quelli precedenti. Il cifrario risultante è ancora strutturato in blocchi, ma blocchi uguali nel messaggio vengono cifrati in modo diverso, eliminando così la periodicità.
## Struttura
Il metodo si impiega in cifratura e in decifrazione utilizzando le funzioni $C$ e $D$ del cifrario d'origine. Supponiamo che il cifrario d'origine operi su blocchi di $\textcolor{#ff82b2}{b}$ bit con chiave $\textcolor{#ff82b2}{k}$. Il messaggio $\textcolor{#ff82b2}{m}$ è *diviso in blocchi*, $m=m_1m_2\dots m_s$: se
- $\textcolor{#ff82b2}{m_s \text{ contiene }r<b\text{ bit}}$ lo si *completa* aggiungendovi la sequenza binaria 10000... di lunghezza $b-r$
- $\textcolor{#ff82b2}{m_s \text{ contiene }r=b\text{ bit}}$ si aggiunge al messaggio un intero *nuovo blocco* $m_{s+1}=10000\dots$ di lunghezza $b$.
In entrambi i casi la nuoav sequenza funge da terminatore del messaggio. Sia ora $\textcolor{#ff82b2}{c_0}$ una sequenza di $b$ bit scelta in modo casuale e diversa per ogni messaggio ($c_0$ può essere pubblica).
### Cifratura e decifrazione
Ogni blocco $m_i$ viene **cifrato** come
$$
\textcolor{#ff82b2}{c_i=C(m_i\oplus c_{i-1}, k)}
$$
La **decifrazione** del blocco $c_i$ è eseguita come
$$
\textcolor{#ff82b2}{m_i=c_{i-1}\oplus D(c_i, k)}
$$
Si osservi come il processo di *cifratura sia strettamente sequenziale* (il calcolo di ogni $c_i$ impiega il risultato $c_{i-1}$ del passo precedente), mentre il processo di *decifratura* può essere eseguito in *parallelo* (se tutti i blocchi cifrati sono disponibili).
L'introduzione della sequenza $c_0$ e la sua modifica per ogni nuovo messaggio sono necessarie per evitare che messaggi uguali generino crittogrammi uguali, o che prefissi di messaggi uguali generino prefissi di crittogrammi uguali.
### Vantaggi
- I processi di cifratura e decifrazione non sono apprezabilmente più lenti di quelli del cifrario originale
- Più messaggi possono essere cifrati con la stessa chiave
- *Resistenza agli errori* nel crittogramma, dato che la sostituzione di un bit nel blocco $c_i$ induce un errore nella decifrazione dell'intero blocco $m_{i+1}$, ma qui la propagazione dell'errore si arresta
Il metodo è però esposto a errori di inserimento o cancellazione di bit nel crittogramma e, per essere protetto da questo, deve essere combinato con altri meccanismi