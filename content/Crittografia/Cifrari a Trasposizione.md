---
title: Cifrari a Trasposizione
tags: #critto
---
##### 2022/10/11 #date
## Idea
Eliminare qualsiasi struttura linguistica presente nel crittogramma permutando le lettere del messaggio in chiaro e inserendone eventualmente altre che vengono ignorate nella decifrazione.
## Cifrario a Permutazione Semplice
<span style="color:#ff82b2"><b>Chiave:</b></span> intero $h$; permutazione $\pi$ degli interi ${1,2,...,h}$
<span style="color:#ff82b2"><b>Cifratura:</b></span>
* si suddivide il messaggio $m$ in blocchi di $h$ lettere
* si permutano le lettere di ciascun blocco secondo $\pi$
> [!tip] Observation 
> 
> Se la lunghezza di $m$ non è divisibile per $h$, si aggiungono alla fine delle lettere qualsiasi (<span style="color:#ff82b2"><b>padding</b></span>), che partecipano alla trasposizione, ma non sono ignorate dal destinatario perché la decifrazione le riporta alla fine del messaggio.

Il <span style="color:#ff82b2"><b>numero di chiavi</b></span> è $\textcolor{#f77ead}{h!-1}$, $h$ non è fissato a priori, al crescere di $h$ è sia più difficile impostare un attacco esauriente, sia ricordare la chiave $\pi$
## Cifrario a permutazione di colonne
$\textcolor{#f77ead}{k=<c,r,\pi>}$ dove
- $\textcolor{#f77ead}{c}$ e $\textcolor{#f77ead}{r}$ denotano il numero di colonne e di righe di una <span style="color:#ff82b2"><i>tabella di lavoro T</i></span>
- $\textcolor{#f77ead}{\pi}$ è la permutazione degli interi $\{1,2,...,c\}$
<span style="color:#ff82b2"><b>Messaggio</b></span> $\textcolor{#f77ead}{m}$<span style="color:#ff82b2"><b>:</b></span> decomposto in blocchi $m_1,m_2,...$ di $c\times r$ caratteri ciascuno
<span style="color:#ff82b2"><b>Cifratura del blocco</b></span> $\textcolor{#f77ead}{m_i}$<span style="color:#ff82b2"><b>:</b></span> I caratteri sono distribuiti tra le celle di T in modo regolare, scrivendoli per righe dall'alto verso il basso. Le colonne di $T$ sono permutate secondo $\pi$, $T$ viene azzerata e il processo si ripete.
> [!example] Example
> 
> $\pi=\{2,1,5,3,4,6\}$
> ![Cifrario a permutazione di colonne](Crittografia/assets/permutazioneColonne.jpg)
> $c=|OIE|NOP|OOL|NLV|SCO|NLE|$

<span style="color:#ff82b2"><b>Numero di Chiavi:</b></span> teoricamente è esponenziale nella lunghezza del messaggio, non essendoci vincoli sulla scelta di $r$ e $c$.
## Cifrario a griglia
In questo cifrario, derivato dal <span style="color:#ff82b2"><i>cifrario di Richelieu</i></span>, il crittogramma può essere celato in un libro qualsiasi. La chiave è data da una scheda perforata e dall'indicazione di una pagina del libro. La decifrazione consiste nels ovrapporre la scheda alla pagina: le lettere visibili attraverso la perforazione costituiscono il messaggio in chiaro.
### Variante
La chiave segreta è una griglia quadrata di dimensioni $q\times q$, con $q$ pari, $s=\frac{q^2}{4}$ celle della griglia (un quarto del totale) sono trasparente, le altre opache, si scrivono i primi $s$ caratteri del messaggio nelle posizioni corrispondenti alle celle trasparenti. La grigli aviene rotata tre volte di 90° in senso orario, e si ripete per ogni rotazione l'operazione di scrittura
> [!example] Example
> 
>$q=6, s=9, m=\text{L'ASSASSINO È ARCHIMEDES TARRINGTON}$
>![Cifrario a rotazione](Crittografia/assets/CifRotazione.jpg)

La griglia deve essere scelta in modo che le posizioni corrispondenti alle celle trasparenti non si sovrappongano mai nelle quattro rotazioni. Se la lunghezza del messaggio è minore di $4s$, le posizioni della pagina $P$ rimaste vuote si riempiono con caratteri scelti a caso. Se la lunghezza del messaggio è maggiore di $4s$, il messaggio viene decomposto in blocchi di $4s$ caratteri ciascuno, e ogni blocco è cifrato indipendentemente dagli altri. La <span style="color:#ff82b2"><b>decifrazione</b></span> di $P$ è eseguita sovrapponendovi 4 volte la griglia.
<span style="color:#ff82b2"><b>Chiavi:</b></span> Vi sono $G=4^s$ griglie, cioè chiavi possibili, numero sufficiente a porre il cifrario al sicuro da un attacco esauriente.