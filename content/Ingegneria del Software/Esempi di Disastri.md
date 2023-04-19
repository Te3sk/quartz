# Esempi di disastri
```toc
```
## Gemini V (1965)
La navicella è atterrata in mare a 80km dal punto previsto a causa di un errore di un programmatore che aveva inserito la velocità di rotazione della terra come 360° per 24 ore invece che 360,24°.
## Denver airport (1995 - 2005)
Viene inauguarato un sistema di smistamento dei bagagli automatico, 35km di rete, 4000 carrelli e 5000 "occhi", con un investimento totale di $193000000. I risultato fu che l'inauguarazione dell'aereoporto fu ritardata di 16 mesi (a 1 milione $ al giorno), si sforò di 3,2 miliardi di dollari rispetto ai preventivi, e dopo anni di tentativi di fix, gli fu staccata la spina nel 2005.
Gli errori principali furono:
- Il sistema elettrico soffriva di sbalzi di potenza che mandavano in tilt il sistema
- Più di 100 PC singoli fisicamente distribuiti
- Il guasto di un PC poteva causare un interruzione: non esisteva un backup automatico per i componenti guasti (**no fault tollerance**). Si perdeva traccia di quali carrelli fossero pieni e quali vuoti dopo un riavvio
- Il sistema non era in grado di rilevare inceppamenti e, di conseguenza, quando se ne verificava uno, il sistema continuava ad accumulare sempre più valige, peggiorando la situazione (**non robusto**)
## Therac (1985 - 1987)
Era un sistema che stabiliva la terapia di radiazioni per i pazienti. Tra USA e Canada 3 persone rimasero uccise per sovradosaggi di radiazioni. Il problema fu causato da un editing troppo veloce dell'operatore e sulla mancanza di controllo sui valori immessi. Questo ha portato ad errori nel sistema software ed errori di interfacciamento software/hardware (**poca robustezza** e **difetto latitante**)
## Sistema antimissile Patriot (1991)
Una caserma a Dhahran (Arabia Saudita) fu colpita per un difetto nel sistema di guida, morirono 28 soldati americani. Il sistema era concepito per funzionare initerrottamente per un massimo di *14 ore*, ma fu usato per *100h*. Questo portò ad errori nell'orologio interno del sistema accumulati al punto da rendere inservibile il sistema di tracciamento dei missili da abbattere (**scarsa robustezza**)
## London Ambulance Service (1992)
Si trattava di un sistema per gestire il servizio di ambulanze, consisteva in ottimizzazione dei percorsi e guida vocale agli autisti. Ne furono sviluppate 3 versioni, per un costo totale di €11000000, e l'ultima versione fu abbandonata dopo soli 3 giorni. I problemi furono:
	- Interfaccia utente inadeguata
	- poco addestramento utenti
	- sovraccarico non considerato
	- nessuna procedura di backup
	- scarsa verifica del sistema
## Ariane 5 (1996)
[Video Youtube](https://www.youtube.com/watch?v=5tJPXYA0Nec)
Il sistema, progettato per l'Ariane 4, tenta di convertire la velocità laterale del missile dal formato 64 bit al formato a 16 bit. Ma l'ariane 5 vola molto più velocemente dell'ariane 4, e il valore della velocità laterale è più elevato di quanto possa essere gestito dalla routine di conversione.
	**Overflow:** spegnimento del sistema di guida, e trasferimento del controllo al 2° sistema di guida, che però, essendo progettato allo stesso modo, andò in tilt nello stesso modo
Il problema fù che i **test** vennero fatti con dati **vecchi**: fu necessario quasi un anno e mezzo per capire quale fosse stato il malfunzionamento che aveva portato alla distruzione del razzo.
