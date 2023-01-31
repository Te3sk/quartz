# History
```toc
```
Il concetto di computazione nasce nei primi 30 anni del '900 come risposta alla **[[crisi dei fondamenti]]**.
# Procedura di Calcolo
Cerchiamo qualcosa che prenda dei dati in input e restituisce dati in outpu $S\rightarrow D$. $S:\mathbb{N}\rightarrow \mathbb{N}$ è una **funzione parziale**, perché non è detto che i risultati sono solo $\in \mathbb{N}$. Vediamo 3 approcci:
1. [[ProgrammingPradigms#Approccio Ricorsivo|Approccio Ricorsivo]]
2. [[ProgrammingPradigms#Approccio di Turing|Approccio di Turing]]
3. [[ProgrammingPradigms#Approccio di Church lambda -calcolo|Approccio di Church]]
### Approccio Ricorsivo
1. Le funzioni elementari sono calcolabili
2. Date le funzioni $f$ e $g$ caloclabili, $f\circ g$ è calcolabile
3. Come conseguenza di 1. e 2., la ricorsione è calcolabile
### Approccio di Turing
Il programma è un elenco di regole che una <u>persona</u> può seguire inmodo preciso (**algoritmo**), per Turing una funzione è calcolabile $iff$ una *macchina di Turing* riesce a calcolarla. $\exists$ funzioni non calcolabili.
La **Macchina di Turing Universale** prende in input un'altra macchina ("*normale*") e un valore, restituisce come output il valore elaborato dalla macchina *normale*. Questa fu una rivoluzione essendo la prima differenze palese tra hardware e software, fu il primo prototipo di **interprete** e di **programma memorizzato**. È importante che questa macchina sia programmabile.
Von Neuman si basa proprio su di essa per la sua architettura, dove la memoria contiene dati ed istruzioni
![Von Neuman Architecture](assets/PdP/VonNeumanArch.png)
### Approccio di Church (λ-Calcolo)
Contemporaneamente [Church](https://it.wikipedia.org/wiki/Alonzo_Church) propone il **$\lambda$-calcolo** $\approx$ ricorsione $\approx$ TM $\Rightarrow$ nasce la tesi di Church e Turing. Quello di Church è un metodo sistematico per costruire, data un'espressione $t$ (verosimilmente in una variabile $x$), un'**espressione** per la corrispondente funzione in $x$.