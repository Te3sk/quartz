# Introduzione al Corso
```toc
```
## Processo Software
Indica il **modo in cui produciamo il software**, inizia quando iniziamo ad esplorare il problema e finisce quando il prodotto viene ritirato dal mercato. Riguarda anche tutti i tools e le tecniche per lo sviluppo ed il mantenimento e tutti i professionisti coinvolti. Le fasi sono:
- analisi dei requisiti
- specifica
- progettazione 
- implementazione
- integrazione
- mantenimento
- ritiro
> [!def] Ingegneria del Software *secondo IEEE*
> L'approccio sistematico allo sviluppo, all'operatività, alla manutenzione ed al ritiro del software
> 			[Glossario IEEE](https://www.ieee.org/publications/rights/rights-glossary.html)

È una disciplina che ha lo scopo di produrre *fault-free* software, consegnato nei *tempi previsti*, che rispetti il *budget* iniziale, che soddisfi le *necessità del committente* e facile da *modificarre*. È una disciplina sia tecnologica che gestionale.
## Nascita
**Contesto degli anni '60:** Si passa da software sviluppati informalmente a grandi sistemi commerciali, quindi dalla programmazione individuale a quella di squadra.
Nel '68, alla *NATO Software Engineering Conference*, Garmish parla della **crisi del software** (quanto la qualità del software fosse inaccettabilmente bassa) e dell'**ingegneria del software** (la soluzione alla crisi del software). L'**idea** era che la produzione di software deve usare tecniche e paradigmi come le consolidate discipline ingegneristiche.
## Specificità del Software
Il software è diverso da altri prodotti dell'ingegneria: non è vincolato da materiali, né governato da leggi fisiche o da processi manufatturieri, non ha alcun costo marginale, non si consuma (tuttavia si *deteriora*), spesso si *assembla*, ma larga parte è ancora realizzata ad hoc.
### Fault Tollerance
Quando un edificio crolla parzialmente (o una macchina si rompe) non si aggiusta come se fosse un'araba fenice. Allo stesso modo, quando un sistema operativo "crasha" lo facciamo ripartire, questo perché è stato progettato per minimizzare l'effetto del fallimento (es non si perdono i documenti su cui si sta lavorando). La **fault tollerance** è una qualità del Software.
## Aspetti economici nella produzione software
L'ingegnere software è anche interessato a soluzioni economicamente vantaggiose.
![CostiSoftware](assets/is/costSW.png)
## Importanza della fase di Analisi dei requisiti
Se si introduce un erroe durante l'analisis dei requisiti, l'errore apparirà anche nella specifica, nella porograttazione e nel codice. Prima individuiamo l'errore meglio è, perché più avanti lo troviamo, più è alto il costo di risoluzione.
## La manutenzione del software
La manutenzione include tutti i cambiamenti al prodotto software, anche dopo che è stato consegnato al cliente. Si divide in:
- **manutenzione correttiva (20%):** rimuove gli errori lasciando invariata la specifica
- **manutenzione migliorativa:** consiste in cambiamenti alla specifica e nell'implementazione degli stessi, può essere:
	- **perfettiva (60%):** modifiche per migliorare le qualità del software, introduzione di nuove funzionalità, miglioramento delle funzionalità esistenti.
	- **adattiva(20%):** modifiche a seguito di cambiamenti nell'ambiente legislativo, cambiamenti dell'hardware, nel SO, ecc...
## Lavoro in team
La maggior parte del software oggi è prodotto da team di programmatori. Il lavoro in team pone dei problemi di interfaccia tra le diverse componenti del codice e di comunicazione tra i membri del team. Molto tempo deve essere dedicato alle riunioni tra i vari componenti.
L'ingegnere del software deve essere anche capace di gestire i rapporti umani e organizzare un team e di gestire gli aspetti economici e legali.
## Temi di Ingegneria del Software
- **Processo software**
	- organizzazione e gestione dei progetti
	- definizione e correlazione delle attività
	- metodi di analisi dei gruppi di lavoro
	- strumenti di pianificazione, analisi, controllo
	- modelli ideali di processo di sviluppo
- **Realizzazione di sistemi software**
	- Strategie di analisi e progettazione
		tecniche per la comprensione e la soluzione di un problema (top-down, bottom-up, progettazione modulare, OO)
	- Linguaggi di specifica e progettazione
		strumenti per la definizione di sistemi software (reti di petri, Z, OMT, UML)
	- Ambienti di sviluppo
		Strumenti per analisi, progettazione e realizzazione (strumenti tradizionali, CASE, CAST)
- **Qualità del software**
	- Metodi di qualità, definizione di caratteristiche della qualità
	- Metriche software, unità di misura, scale di riferimento, strumenti, indicatori di qualità
	- Metodi di verifica e controllo, criteri di progettazione delle prove, valutazione del processo di sviluppo.
