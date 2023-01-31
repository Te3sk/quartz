---
title: Cifrari a Sostituzione
tags: #critto
---
##### 2022/10/10 #date 
# Cifrari Monoalfabetici
> [!quote] Cifrario monoalfabetico
> 
> Alla stessa lettera del messaggio corrisponde una stessa lettera nel crittogramma (e.g. cifrario di cesare)

Si possono impiegare funzioni di cifratura e decifrazione più complesse dell'addizione e della sottrazione in modulo. Lo spazio delle chiavi è molto più ampio, ma la sicurezza è comunque molto modesta.
## Il cifrario affine
### Cifratura 
Una lettera in chiaro $X$ viene sostituita con la lettera cifrata $Y$ che occupa nell'alfabeto la posizione
$$
	pos(Y)=(a*(pos(X)+b))\text{ mod }26
$$
$\textcolor{#f77ead}{k=<a,b>:}$ chiave segreta del cifrario

### Decifrazione
$$
	pos(X)=a^{-1}*(pos(Y)-b)\text{ mod }26
$$
$\textcolor{#f77ead}{a^{-1}:}$ l'inverso di $a$ modulo $26$
$$(a\times a^{-1}=1\text{ mod }26)$$
### Vincoli 
L'inverso di un intero $a$ modulo $m$ <span style="color:#ff82b2"><i>esiste ed è unico</i></span> se e solo se $MCD(a,m)=1$
#### Vincolo forte sulla definizione della chiave 
se $MCD(a,36)\neq 1$, la funzione di cifratura non è iniettiva, e la decifrazione diventa impossibile
#### Chiavi segrete:
 - $a$ e $26$ devono essere co-primi, i fattori primi di $26$ sono $2$ e $13$, $a$ può assumere qualsiasi valore dispari tra 1 e 25, ad eccezione di $13$ ($12$ valori possibili)
 - $b$ può essere scelto liberamente tra $0$ e $25$, dunque può assumere $26$ valori
 - La chiave legittime sono tutte le possibili $<a,b>$
 - In totale $12\times 26=312$ chiavi (troppo poche...), in realtà $311$ (la coppia $<1,0>$, lascia inalterato il messaggio)

Se la segretezza dipende unicamente dalla chaive, il numero delle chiavi deve essere così grande da essere praticamente immune da ogni tentativo di provarle tutte. La chiave segreta deve essere sscelta in modo casuale.

### Cifrario Completo
Si può prendere una permutazione arbitraria dell'alfabeto come chiave:
$$
\begin{array}{c}
\text{lettera in chiaro di posizione } i \\
\Downarrow \\
\text{lettera di posizione } i \text{ nella permutazione}
\end{array}
$$
![Cifrario a Sostituzione monoalfabetica Completo](Crittografia/assets/CifMonoCompleto.jpg)

<span style="color:#ff82b2"><i>testo:</i></span> $BOBSTAIATTENTOAEVE$

<span style="color:#ff82b2"><i>messaggio cifrato:</i></span> $DEDFWSRWWBYWESBGB$

- <span style="color:#ff82b2"><b>Chiavi</b></span> di 26 lettere, cotro 2 numeri interi del cifrario
- <span style="color:#ff82b2"><b>Spazio delle chiavi:</b></span> esteso a $26! -1(\sim 4\cdot 10^{26})$ chiavi, vasto e inesplorabile con metodi esaurienti
- <span style="color:#ff82b2"><b>Ma il cifrario non è sicuro:</b></span> si può forzare (senza ricorrere a un attacco esauriente) sfuttando
	- struttura logica dei messaggi in chiaro
	- occorrenza statistica delle lettere
Il sistema è attaccabile facilmente con un'analisi statisstica sulla frequenza dei caratteri in italiano.

![Istogramma delle Frequenze](Crittografia/assets/frequenzaLettere.jpg)

Le cifrature monoalfabetiche sono facili da violare perché conservano le informazioni di frequenza dell'alfabeto originario. <span style="color:#ff82b2"><i>La struttura del testo in chiaro permane nel testo cifrato</i></span>.
##### 2022/10/11 #date 
# Cifrari Polialfabetici
> [!quote] Cifrario Polialfabetico
> 
Alla stessa lettera del messaggio corrisponde una lettera scelta in un insieme di lettere possibili, secondo una regola opportuna, a seconda della posizione o del contesto in cui appare la lettera nel messaggio

Una <span style="color:#ff82b2"><i>stessa lettera che si incontra in punti diversi</i></span> del messaggio in chiaro <span style="color:#ff82b2"><i>ammette un insieme di lettere sostitutive possibili</i></span> scelte con una regola opportuna. L'esempio più antico risale ai tempi di Roma Imperiale (archivio cifrato di Augusto)
## Cifrario di Augusto
Svelato dall'imperatore Claudio
I documenti dell'archivio erano <span style="color:#ff82b2"><i>scritti in numeri</i></span>, non in lettere. Augusto li scriveva in greco, poi metteva in corrispondenza la sequenza di lettere del documento con la sequenza di lettere del primo libro dell'Iliade. Sostituiva ogni lettera del documento con il numero che indicava la <span style="color:#ff82b2"><i>distanza</i></span>, nell'alfabeto greco, di tale lettera con quella in pari posizione nell'Iliade
> [!example] Example 
> 
> lettera in posizione $\textcolor{#f77ead}{i}$ nel documento: $\textcolor{#f77ead}{\alpha}$
> 
> lettera in posizione $\textcolor{#f77ead}{i}$ nell'Iliade: $\textcolor{#f77ead}{\varepsilon}$
> 
> carattere in posizione $\textcolor{#f77ead}{i}$ nel crittogramma: $\textcolor{#f77ead}{4}$ (*distanza tra $\textcolor{#f77ead}{\alpha}$ e $\textcolor{#f77ead}{\varepsilon}$*)

Il cifrario risulta difficile da forzare se la chiave è lunghissima, fu utilizzato nella seconda guerra mondiale prendendo come chiave una pagina prefissata di un libro, e cambiandola di giorno in giorno. Lo <span style="color:#ff82b2"><b>svantaggio</b></span> era che la chiave fosse registrata per scritto.
## Il disco di Leon Battista Alberti (XV secolo)
![Disco di Alberti](Crittografia/assets/discoAlberti.jpg)
- <span style="color:#ff82b2"><b>Alfabeto esterno:</b></span> lettere (alcune) e numeri, per formulare il messaggio
- <span style="color:#ff82b2"><b>Alfabeto interno:</b></span> più ricco, disposto in modo arbitrario (e diverso per ogni coppia di utenti), per costruire il crittogramma.
Il crittogramma si formava posizionando arbitrariamente il disco interno e, se inseriti numeri, questi corrispondevano a un cambio di chiave, dove la cifra inserita indicava la nuova lettera che corrispondeva alla *A* (la prima).
Si cambia chiave ogni volta che si incontra un carattere speciale, inserendoli spesso il cifrario è difficile da attacre. Il continuo cambio di chiave rende inutili gli attacchi basati sulla frequenza dei caratteri.
## Cifrario di De Vigenère (1586)
La chiave è corta e ripetuta ciclicamente. Ogni lettera della chiave indica una traslazione della corrispondente lettera del testo.

![Cifrario di Vigenère](Crittografia/assets/Vigenère.jpg)

Possiamo vederla come una sequenza di cifrari di Cesare con shift diversi.
<span style="color:#ff82b2"><b>Chiave:</b></span> parola segreta $k$
<span style="color:#ff82b2"><b>Cifratura:</b></span>
- si procede carattere per carattere
- si dispongono $m$ e $k$ su due righe adiaventi, allineando le lettere in verticale (se $k$ è più corta di $m$, la chiave si ricopia più volte)
- ogni lettera $X$ del messaggio in chiaro risulta allineata ad una lettera $Y$ della chiave
- la $X$ viene sostituita nel crittogramma con la lettera che si trova nella cella $T$ all'incrocio tra la riga che inizia con $X$ e la colonna che inizia con $Y$
<span style="color:#ff82b2"><b>Decifrazione:</b></span> si esegue cercando nella riga corrispondente al carattere della chiave il corrispondente carattere del crittogramma. Il primo carattere della colonna corrisponde al carattere in chiaro.

$$
\begin{array}{l}
	m=m_0m_1m_2...m_i...m_{h-1}m_hm_{h+1}... & \textcolor{#f77ead}{\text{messaggio}} \\
	k=k_0k_1k_2...k_i...k_{h-1}k_hk_{h+1}... & \textcolor{#f77ead}{\text{chiave}} \\
	c=c_0c_1c_2...c_i...c_{h-1}c_hc_{h+1}... & \textcolor{#f77ead}{\text{crittogramma}} \\
	\textcolor{#f77ead}{\text{\textbf{Cifratura}}\boldsymbol{\rightarrow}}  c_i=(m_i+k_{i\text{ mod }h})\text{ mod }26 \\
	\textcolor{#f77ead}{\text{\textbf{Decifrazione}}\boldsymbol{\rightarrow}}m_i=(c_i-k_{i\text{ mod }h})\text{ mod }26
\end{array}
$$
> [!example] Example 
> 
> $m=\text{il delfino}$
> 
> $k=\text{abra}$
> 
> $$
> \begin{array}{c}
> I&L&&D&E&L&F&I&N&O\\
> A&B&&R&A&A&B&R&A&A\\
> \downarrow&\downarrow&&\downarrow&\downarrow&\downarrow&\downarrow&\downarrow&\downarrow&\downarrow&\\
> I&M&&U&E&L&G&Z&N&O\\
> \end{array}
> $$

<span style="color:#ff82b2"><b>Sicurezza:</b></span> La sicurezza del metodo è influenzata dalla lunghezza della chiave, se la chiave contiene $\textcolor{#f77ead}{h}$ caratteri, le apparizione della stessa lettera saranno distanti un multiplo di $\textcolor{#f77ead}{h}$ nel messaggio si sovrappongono alla stessa lettera della chiave, quindi sono trasformate nella stessa lettera cifrata
### One-Time Pad (1917)
Il cifrario [[Crittografia/Cifrari Perfetti#One-Time Pad|One-Time Pad]] è ottenuto estendendo il metodo di Vigenère impiegando una <span style="color:#ff82b2"><b>chiave lunga come il testo</b></span>, <span style="color:#ff82b2"><b>casuale</b></span> e <span style="color:#ff82b2"><b>non riutilizzabile</b></span>, il cifrario diventa <span style="color:#ff82b2"><b>inattaccabile</b></span>, non può essere decifrato senza conoscere la chiave.