# Esercitazione 1
[[Crittografia/PDF/EserciziComplessitaRandomizzazione.pdf|Testi esercizi Complessità e randomizzazione]]
[[Crittografia/PDF/EserciziCifrariStorici.pdf|Testi esercizi Cifrari Storici]]
##### 2022/10/17 #date 
```toc
```
## Complessità e randomizzazione
### Esercizio 1

## Cifrari Storici
### Esercizio 2
Testo in chiaro scelto -> *hahaha*
Testo cifrato -> *nonono*
Nel cifrario affine mi basta conoscere due coppie di lettere inChiaro-Cifrate per impostare un sistema due equazione due incognite. Nel cifrario affine $pos(Y)=(a\cdot pos(X)+b)mod \:\:26$
$$
\begin{array}{llcrcl}
	&&&m&&c\\
	&&&H&\to&N\\
	&&&A&\to&O\\
	pos(N)=&(a\cdot pos(N)+b)\:\:mod \:\:26 \\
	pos(O)=&(a\cdot pos(O)+b)\:\:mod \:\:26 \\
	&13 = (a\cdot 7 +b) \:\:mod \:\:26&&\implies \textcolor{#f77ead}{b=14}\\
	&14 = b \:\: mod \:\:26\\
	&13=(7a+14) \:\:mod \:\:26\\
	&7a=(13-14) \:\: mod \:\:26\\
	&\textcolor{#f77ead}{\text{RIPRENDI DA SLIDE}}
\end{array}
$$
### Esercizio 3
trovare $\#\text{chiavi (coppie <a,b>)}$ nel cifrario con 27 caratteri nell'alfabeto.
Dobbiamo valutare i vincoli:
- $\textcolor{#f77ead}{b}:$ può assumere tutti i valori in $[0,26]$->$27$ valori
- $\textcolor{#f77ead}{a}:$ deve essere *coprimo* con $27=3^3$->può assumere i valori tra 1 e 26 che non siano multipli di 3 $$\emptyset(27)=\emptyset(3^3)=18$$ dove $\emptyset()$ è la **funzione di Eulero**
$\#chiavi=18\cdot 27-1=485$ (tolgo la coppia $(1,0)$)
Se si lavora in modulo 29 i conti sono gli stessi ma sostituendo il nuovo valore $\#chiavi=28\cdot 29-1= \textcolor{#f77ead}{riprendi}$
### Esercizio 4
Composizione di cifrari affini, dobbiamo mostrare che 
$$
\begin{array}{l}
	C_2(C_1(x))=C_3(x)\\
	C_1(x)=(a_1x+b_1)mod \:\:26\\
	C_2(x)=(a_2x+b_2)\:\:mod\:\:26\\
	\:\:\:\:\:\:\:\:\implies C_3(x)=(a_3x+b_3)\:\:mod\:\:26\\
	C_2(C_1(x)) = C_2(a_1)\\
	\textcolor{#f77ead}{riprendi}
\end{array}
$$
### Esercizio 5
1. Da una signola coppia msg-critto si può ricostruire quasi tutta la permutazione, si conosce la cifratura di 22 di 26 lettere in chiaro
2. Potrebbero essere state usate $4! \:\: =24$ chiavi diverse
3. 
### Esercizio 7
1. scegli una permutazione e cifra
2. in generale $26! -1$. In questo caso, per decifrare il singolo crittogramma, si contano il numero di lettere diverse nel crittogramma ($10$) e provare solo le applicazioni di quelle, quindi $46\cdot 25\cdot 24\cdots \cdot17=\frac{26!}{16!}\approx 2^{16}$
3. Si usa la **crittoanalisi statistica**
### Esercizio 8
vedi libro
### Esercizio 9
vedi libro
### Esercizio 12