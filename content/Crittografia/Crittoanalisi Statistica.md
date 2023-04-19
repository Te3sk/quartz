---
title: Crittoanalisi Statistica
tags: #critto
---
##### 2022/10/11 #date 
La sicurezza di un cifrario è legata alla dimensione dello spazio delle chiavi, ma spesso la vulnerabilità non è nell'algoritmo, ma nelle sue applicazioni:
* <span style="color:#ff82b2"><i>chiavi usate male:</i></span> troppo corte, generate male, prevedibili, riutilizzate
* si conosce il <span style="color:#ff82b2"><i>formato</i></span> del messaggio
Nella crittoanalisi statistica:
* il cifrario non è forzato da un punto di vista algoritmico, ma statistico (<span style="color:#ff82b2"><b>analisi delle frequenze</b></span>)
* I cifrari storici sono stati violati con un attacco statistico di tipo <span style="color:#ff82b2"><b>cipher text</b></span>, il crittoanalista ha a disposizione solo il crittogramma
* L'impiego del metodo si fa risalire in Europa alla metà del XIX secolo, quando si scoprì come violare il [[Crittografia/Cifrari a Sostituzione#Cifrario di De Vigenère (1586)|Cifrario di De Vigenère]], considerato assolutamente sicuro da 300 anni.
Si assume che il crittoanalista conosca il metodo impiegato per la cifratura, che il messaggio sia scritto in un linguaggio naturale noto al crittoanalista e che il messaggio sia sufficientemente lungo da poter rivelare alcuni dati statistici sui caratteri che compongono il crittogramma. Si possono insoltre sfruttare gli <span style="color:#ff82b2"><i>istogrammi delle frequenze</i></span> e gli <span style="color:#ff82b2"><i>istogrammi delle frequenze dei</i></span> *[[Glossary#q-grammi|q-grammi]]*.
## Cifrari a sostituzione
### Sostituzione monoalfabetica
- [[Crittografia/Cifrari Storici#Cifrario di Cesare|Cifrario di Cesare:]] è sufficiente scoprire la corrispondenza di una sola coppia $\langle y, x\rangle$ 
- <span style="color:#ff82b2"><i>Cifrario affine:</i></span> è sufficiente individuare due coppie di lettere corrispondenti con cui impostare un sistema di congruenze nelle due incognite $a$ e $b$ che formano la chiave segreta
- <span style="color:#ff82b2"><i>Cifrario completo:</i></span> Si prova ad associare le lettere in base alle frequenze e si provano le associazioni possibili.
### Sostituzione polialfabetica
Ciascuna lettera $y$ del crittogramma dipende da una coppia di lettere $\langle x,z\rangle$ provenienti dal messagio e dalla chiave, questo altera le frequenze delle lettere del crittogramma. Il <span style="color:#ff82b2"><i>punto di debolezza</i></span> è che <span style="color:#ff82b2"><i>la chiave è unica</i></span> e ripetuta più volte. Il messaggio quasi sempre contiene gruppi di lettere adiacenti ripetuti più volte, se due apparizioni di una stessa sottosequenza sono allineate nel messaggio con la stessa porzione della chiave, vengono trasformate nel crittogramma in due sequenze identiche. 
Si tratta quindi di cercare coppie di posizioni $p_1, p_2$ nel crittogramma in cui iniziano sottosequenze identiche, perché la distanza $d=p_2-p_1$ è molto probabilmente la <span style="color:#ff82b2"><i>lunghezza</i></span> $\textcolor{#ff82b2}{h}$ della chiave (o un suo multiplo); se si trovano distanze $d, d', d'', ...$ diverse, $h$ è probabilmente un loro divisore comune.
Una volta trovato il valore di $h$ si procede sulle sottosequenze come in un cifrario a sostituzione monoalfabetica.
## Cifrari a trasposizione
Qui non ha senzo sfruttare gli istogrammi delle frequenze, visto le lettere del messaggio non vengono cambiate ma solo spostate. Possiamo però dividere il crittogramma in sottosequenze lunghe quanto la chiave (facendo qualche tentativo, ricordando che la chiave sarà un divisore della lunghezza del crittogramma) e cercare i gruppi di $q$ lettere che formano i $\textcolor{#ff82b2}{q}$<span style="color:#ff82b2"><i>-grammi</i></span>.
## Scoprire il cifrario
- Nei [[Crittografia/Cifrari a Trasposizione|Cifrari a trasposizione]] l'istogramma delle frequenze del crittogramma coincide approssimativamente con quello proprio del linguaggio
- Nei [[Crittografia/Cifrari a Sostituzione#Cifrari Monoalfabetici|cifrari a sostituzione monoalfabetica]] i due istogrammi conincidono a meno di una permutazione delle lettere
- Nei [[Crittografia/Cifrari a Sostituzione#Cifrari Polialfabetici|cifrari a sostituzione polialfabetica]] l'istogramma delle frequenze risulta più "appiattito"