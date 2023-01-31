---
title: ElGamal
tags: #critto
---
##### 2022/11/08 #date 
La sicurezza si basa sulla difficoltà di calcolare i [[Glossary#Logaritmi discreti| logaritmi discreti]]
## Creazione della coppia di chiavi
1. Si sceglie un *numero primo* $\textcolor{#ff82b2}{p}$ e un generatore $\textcolor{#ff82b2}{g}$ di $\textcolor{#ff82b2}{Z_p^*}$
2. Si sceglie a caso un *intero* $\textcolor{#ff82b2}{x\in [2,p-2]}$
3. Si calcola $\textcolor{#ff82b2}{y=g^x \:\:mod\:\:p}$
**Chiave pubblica:** $\textcolor{#2e80f2}{k_{pub}=(p,g,y)}$
**Chiave privata:** $\textcolor{#2e80f2}{k_{priv}=(x)}$
## Cifratura e decifrazione
**Cifratura:**
- Il messaggio $\textcolor{#ff82b2}{m}$ è codificato come una sequenza binaria, trattata come un numero intero
- $\textcolor{#ff82b2}{m<p}$ (se $\textcolor{#ff82b2}{m\geq p}\to$ cifratura a blocchi, di $\textcolor{#ff82b2}{log_2 \:p}$ bit cascuno)
- Si sceglie a caso un intero $\textcolor{#ff82b2}{r \in [2,p-2]}$
- Si calcola la coppia dei logaritmi $\textcolor{#ff82b2}{c=g^r \:\:mod\:\:p}$ e $\textcolor{#ff82b2}{d=m\cdot y^r \:\:mod\:\:p}$
**Decifrazione:**
$$\textcolor{#ff82b2}{m=d\cdot c^{-x} \:\:mod\:\:p}$$
$$
d\cdot c^{-x} \textcolor{#ff82b2}{=}
m\cdot y^r \cdot c^{-x} \:\:mod\:\:p \textcolor{#ff82b2}{=}
m\cdot y^r\cdot(g^r)^{-x} \:\:mod\:\:p \textcolor{#ff82b2}{=}
m\cdot(g^x)^r\cdot g^{-r\cdot x} \:\:mod\:\:p \textcolor{#ff82b2}{=}
m \:\:mod\:\:p \textcolor{#ff82b2}{=} m
$$
> [!obs]
> - La difficoltà di calcolare il logaritmo discreto rende la chiave privata $\textcolor{#ff82b2}{x}$ sicura, anche se $\textcolor{#ff82b2}{y}$ e $\textcolor{#ff82b2}{g}$ sono pubblici
> - $\textcolor{#ff82b2}{r}$ è un intero casuale
> - $\textcolor{#ff82b2}{y^r \:\:mod\:\:p}$ è un intero casuale
> - $\textcolor{#ff82b2}{d=m\cdot y^r \:\:mod\:\:p}$ è casuale e non fornisce alcuna informazione su $\textcolor{#ff82b2}{m}$ al crittoanalista
> - Per estrarre $\textcolor{#ff82b2}{r}$ da $\textcolor{#ff82b2}{c}$ occorre risolvere il problema del logaritmo discreto, se $\textcolor{#ff82b2}{r}$ è noto si può decifrare: $\textcolor{#ff82b2}{m=d\cdot y^{-r} \:\:mod\:\:p}$
> - Occorre un nuovo numero casuale $\textcolor{#ff82b2}{r}$ per ogni nuovo messaggio

## Attacchi man-in-the-middle
Le chiavi di cifratura sono pubbliche e non richiedono un incontro diretto tra gli utenti per il loro scambio. Un crittoanalista attivo può intromettersi proprio in questa fase iniziale del protocollo, pregiudicano il suo corretto svolgimento. Nell'attacco attivo *man-in-the-middle*, il crittoanalista si intromette nella comunicazione tra i due utenti facendo credere ad ognuno di essere l'altro, intercettando e bloccando le loro comunicazioni.
### Attacchi man-in-the-middle sulle chiavi pubbliche
Il crittoanalista può intercettare lo scambio della chiave pubblica e sostituirla con la propria, così da poter cambiare e reinviare i messaggi a suo piacimento. Gli utenti non si accorgeranno della sua presenza se il processo di intercettazione e rispedizione è sufficientemente veloce da rendere il relativo ritardo apparentemente attribuibile alla rete.
## Protocollo Diffie-Hellman - Cifrario ibrido
Il **mittente**
- genera una $\textcolor{#ff82b2}{k}_{sessione}$ random
- cifra $\textcolor{#ff82b2}{k}$ con la chiave pubblica del ricevente
- cifra il messaggio con AES e $\textcolor{#ff82b2}{k}_{sessione}$
$$
<C_{RSA}(k_{sessione}, k_{pub}), C_{AES}(m, k_{sessione})
$$
Il **ricevente** decifra il *primo crittogramma* con $k_{priv}$, e ottiene $k_{sessione}$, e decifra il *secondo crittogramma* usando $k_{sessione}$
Mittente e ricevente si accordano pubblicamente su un numero primo $\textcolor{#ff82b2}{p}$ molto grande, e su u n generatore $\textcolor{#ff82b2}{g}$ per $\textcolor{#ff82b2}{\mathbf{Z}_p^*}$
## Schema del canale
| Mittente                                                                                                           |                                                   Canale (nascondiglio crittoanalista)                                                    |                                                                                                                                         Ricevente |
| ------------------------------------------------------------------------------------------------------------------ |:-----------------------------------------------------------------------------------------------------------------------------------------:| -------------------------------------------------------------------------------------------------------------------------------------------------:|
| Sceglie a caso $2\leq x \leq p-2$ e calcola $A=g^x \:\:mod\:\:p$, infine tenta di spedire $\textcolor{#ff82b2}{A}$ |                               $E=g^z \:\:mod\:\:p$ con $2\leq z \leq p-2$. *Sostituisce* $A$ e $B$ con $E$                                | sceglie a caso un $\textcolor{#ff82b2}{y}$ t.c. $2\leq y\leq p-2$ e calcola $\textcolor{#ff82b2}{B}=g^y \:\:mod\:\:p, infine tenta di spedire $B$ |
| Calcola $\textcolor{#ff82b2}{K_A}=E^x \:\:mod\:\:p=g^{xz} \:\:mod\:\:p                                             | Calcola $\textcolor{#ff82b2}{K_A}=A^z \:\:mod\:\:p = g^{xz}\:\:mod\:\:p$ e $\textcolor{#ff82b2}{K_B}=B^z \:\:mod\:\:p=g^{yz}\:\:mod\:\:p$ | Calcola $\textcolor{#ff82b2}{K_B}=E^y \:\:mod\:\:p=g^{yz} \:\:mod\:\:p$                                                                                                                                                  |
 
