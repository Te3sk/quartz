# Legenda
## Attori
* **Utenti A:** Utenti che si registrano per ottenere sconti ed agevolazioni
	* **Utenti A1:** Utente A che riceve il braccialetto durante la fase di lancio insieme a 3 (?) mesi di premium gratuiti, riceve la card quando completa la registrazione
	* **Utenti A2:** Utente A che si registra direttamente sulla piattaforma e riceve la card
* **Utenti B:** Partner, proprietari di locali, organizzatori di eventi e chiunque pubblichi gli annunci sulla piattaforma
* **Admin:** Amministratori che gestiscono la piattaforma
* **External services:**
	* GHL (CRM/backend)
	* AirTable (DB)
	* Scanner per card e braccialetti
	* Stripe (Payment Gateway)
## Core Entities
* **Utenti:** A1, A2, B
* **Card/Bracciale:** Metodi di identificazione per gli utenti
- **Scanner**: Dispositivo per la scansione delle card/braccialetti
- **Piano**: L'utente può scegliere un piano di pagamento con accesso a diverse offerte
- **Partner**: Utenti B che pubblicano annunci
- **Generatore di Card**: Crea le card virtuali per gli utenti
## Tech Nodes
- **Frontend**: Sito web
- **Backend**: GoHighLevel 
- **Database**: AirTable
- **Card Generator & Scanner**: Generazione e lettura delle card/braccialetti
## Eventi Principali
- **Sign Up**:
    - Utenti A1: Registrazione tramite braccialetto
    - Utenti A2: Registrazione tramite piattaforma
- **Scansione**: Scansione del braccialetto o della card
- **Upgrade/Downgrade del Piano**: Gestione dei piani da parte degli utenti
- **Gestione Annunci**: Aggiunta e modifica degli annunci da parte dei partner
# User Personas
## 1. Paolo Bianchi
- **Età**: 25 anni
- **Formazione**: Diploma scientifico
- **Residenza**: Pisa
- **Stato Occupazionale**: Studente
- **Caratteristiche**: Viaggiatore appassionato, in coppia con la fidanzata, cerca sempre sconti e opportunità di viaggio.
- **Core Needs**:
    - Trovare esperienze interessanti da integrare nei suoi viaggi
    - Ottenere opzioni di sconto (paga per due persone)
    - Pianificare la vacanza in anticipo
- **Frustrazioni**:
    - Trovare attività che piacciono a lui e alla fidanzata
    - Riconoscere gli scam
    - Perdere tempo in vacanza
## 2. Martina Rossi
- **Età**: 24 anni
- **Formazione**: Laurea triennale in Economia
- **Residenza**: Roma
- **Stato Occupazionale**: Studentessa e Barista
- **Caratteristiche**: Estroversa e festaiola, non vuole rinunciare a fare le cose divertenti.
- **Core Needs**:
    - Fare esperienze che vuole, nonostante lo stipendio basso
    - Trovare "hook" per socializzare
    - Scoprire nuove opportunità di movida e vita mondana nella sua città
- **Frustrazioni**:
    - Avere vincoli economici
    - Non riuscire a trovare sempre le offerte migliori
## 3. Paola Fontana
- **Età**: 47 anni
- **Formazione**: Diploma di Ragioneria
- **Residenza**: Bolzano
- **Stato Occupazionale**: Direttrice di un negozio Conad
- **Caratteristiche**: Madre di tre figli, viaggia spesso con la famiglia, preferisce vacanze brevi e pratiche.
- **Core Needs**:
    - Pianificare vacanze per la famiglia
    - Trovare attività adatte ai bambini
    - Avere flessibilità nella pianificazione
- **Frustrazioni**:
    - Difficoltà nel trovare attività adatte a tutta la famiglia
    - La stanchezza dei bambini che influisce sulla pianificazione
    - Impossibilità di improvvisare durante le vacanze a causa delle esigenze familiari
## 4. Alessandro Moretti
- **Tipo di Utente**: Proprietario di Ristorante
- **Età**: 38 anni
- **Formazione**: Diploma di scuola alberghiera
- **Residenza**: Roma
- **Stato Occupazionale**: Proprietario e manager di un ristorante
- **Caratteristiche**:
    - Ha un ristorante nel cuore di Roma, che attira sia turisti che romani locali.
    - È attivo sui social media per promuovere eventi e offerte speciali.
    - È sempre alla ricerca di nuovi modi per attrarre clienti e aumentare la visibilità del suo ristorante.
    - Presta molta attenzione alla qualità del servizio e alle recensioni online.
- **Core Needs**:
    - Aumentare la visibilità del ristorante presso turisti e clienti locali.
    - Offrire sconti esclusivi o pacchetti per attrarre nuovi clienti.
    - Avere un canale facile per gestire e pubblicare offerte/promozioni.
- **Frustrazioni**:
    - Difficoltà nel raggiungere un pubblico più vasto oltre i clienti locali.
    - La concorrenza alta tra i ristoranti in città rende difficile distinguersi.
    - La gestione delle prenotazioni e delle offerte last-minute può diventare complicata.
## 5. Marco Giordani
- **Tipo di Utente**: Organizzatore di Tour Guidati
- **Età**: 45 anni
- **Formazione**: Laurea in Storia dell'Arte
- **Residenza**: Roma
- **Stato Occupazionale**: Organizzatore e guida turistica
- **Caratteristiche**:
    - Gestisce un'agenzia che offre tour guidati esclusivi nei luoghi più iconici di Roma, con un focus su esperienze personalizzate.
    - Ha una forte presenza online e lavora con diverse piattaforme di prenotazione turistica.
    - È molto apprezzato per la sua conoscenza profonda della storia e cultura della città.
    - Ama offrire esperienze autentiche ai turisti, inclusi tour notturni e tematici.
- **Core Needs**:
    - Espandere il suo pubblico attraverso canali nuovi e promozionali.
    - Avere una piattaforma semplice per inserire e modificare i tour offerti.
    - Offrire sconti e promozioni per attrarre più prenotazioni durante la bassa stagione.
- **Frustrazioni**:
    - La difficoltà di gestire le prenotazioni in alta stagione e le richieste dell’ultimo minuto.
    - La necessità di distinguersi dalle altre agenzie turistiche che offrono tour simili.
    - Difficoltà nel raggiungere il pubblico internazionale attraverso i canali giusti.
# User Journeys
## n. {{ Titolo del journey }} 
* **Persona:** {{ nome della user persona }}
* **Obiettivi**: {{ Cosa vuole raggiungere l'utente in questa fase? }}
- **Frustrazioni**: {{ Cosa rende difficile il suo obiettivo? Quali sono gli ostacoli? }}
- **Fase del Journey:** (( ad es. Scoperta, Coinvolgimento, Conversione, Uso, Supporto ))
### Passi

| n.  | Inizio                                                                                                                                                                                                                          | Interazione                                                                                                                                                                                                                     | Decisione                                                                                                                                                                                                               | Conversione                                                                                                                                                                                                                                 | Fine                                                                                                                                                                                                                      | Emozioni utente |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------- |
|     | **Cosa fa l'utente?** (Es. "L'utente atterra sulla homepage dopo aver visto un annuncio su Facebook.")<br>    <br>**Comunicazione del sistema**: (Es. "La homepage mostra una call to action chiara per esplorare i prodotti.") | **Cosa fa l'utente?** (Es. "L'utente clicca su un prodotto e naviga verso la pagina di dettaglio.")<br>    <br>**Comunicazione del sistema** (Es. "Il sistema carica la pagina e offre informazioni dettagliate sul prodotto.") | **Cosa fa l'utente?** (Es. "L'utente decide di aggiungere il prodotto al carrello.")<br>    <br>**Comunicazione del sistema** (Es. "Un pop-up conferma l'aggiunta al carrello con l'opzione di procedere al checkout.") | **Cosa fa l'utente?** (Es. "L'utente compila i dettagli di pagamento e conferma l'ordine.")<br>    <br>**Comunicazione del sistema** (Es. "Il sistema mostra una schermata di conferma ordine con dettagli e data di spedizione prevista.") | **Cosa fa l'utente?** (Es. "L'utente riceve una conferma via email del suo acquisto.")<br>    <br>**Comunicazione del sistema** (Es. "Email di conferma ordine inviata all'utente con tutte le informazioni necessarie.") |                 |
|     |                                                                                                                                                                                                                                 |                                                                                                                                                                                                                                 |                                                                                                                                                                                                                         |                                                                                                                                                                                                                                             |                                                                                                                                                                                                                           |                 |
|     |                                                                                                                                                                                                                                 |                                                                                                                                                                                                                                 |                                                                                                                                                                                                                         |                                                                                                                                                                                                                                             |                                                                                                                                                                                                                           |                 |
### Punti di frizione
1. (Es. "La navigazione tra le pagine è lenta e l'utente può sentirsi frustrato.")
2. (Es. "Il processo di checkout non è chiaro, creando dubbi sull'accuratezza dei dati inseriti.")
### Opportunità di Miglioramento
1. (Es. "Ottimizzare il tempo di caricamento per migliorare l'esperienza utente.")
2. (Es. "Semplificare il processo di checkout con meno campi da compilare.")
### KPI 
(Es. "Conversione completata", "Tempo per completare il checkout", "Percentuale di abbandono carrello", ecc.)
## n. {{ Titolo del journey }} 
* **Persona:** {{ nome della user persona }}
* **Obiettivi**: {{ Cosa vuole raggiungere l'utente in questa fase? }}
- **Frustrazioni**: {{ Cosa rende difficile il suo obiettivo? Quali sono gli ostacoli? }}
- **Fase del Journey:** (( ad es. Scoperta, Coinvolgimento, Conversione, Uso, Supporto ))
### Passi

| n.  | Inizio                                                                                                                                                                                                                          | Interazione                                                                                                                                                                                                                     | Decisione                                                                                                                                                                                                               | Conversione                                                                                                                                                                                                                                 | Fine                                                                                                                                                                                                                      | Emozioni utente |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------- |
|     | **Cosa fa l'utente?** (Es. "L'utente atterra sulla homepage dopo aver visto un annuncio su Facebook.")<br>    <br>**Comunicazione del sistema**: (Es. "La homepage mostra una call to action chiara per esplorare i prodotti.") | **Cosa fa l'utente?** (Es. "L'utente clicca su un prodotto e naviga verso la pagina di dettaglio.")<br>    <br>**Comunicazione del sistema** (Es. "Il sistema carica la pagina e offre informazioni dettagliate sul prodotto.") | **Cosa fa l'utente?** (Es. "L'utente decide di aggiungere il prodotto al carrello.")<br>    <br>**Comunicazione del sistema** (Es. "Un pop-up conferma l'aggiunta al carrello con l'opzione di procedere al checkout.") | **Cosa fa l'utente?** (Es. "L'utente compila i dettagli di pagamento e conferma l'ordine.")<br>    <br>**Comunicazione del sistema** (Es. "Il sistema mostra una schermata di conferma ordine con dettagli e data di spedizione prevista.") | **Cosa fa l'utente?** (Es. "L'utente riceve una conferma via email del suo acquisto.")<br>    <br>**Comunicazione del sistema** (Es. "Email di conferma ordine inviata all'utente con tutte le informazioni necessarie.") |                 |
|     |                                                                                                                                                                                                                                 |                                                                                                                                                                                                                                 |                                                                                                                                                                                                                         |                                                                                                                                                                                                                                             |                                                                                                                                                                                                                           |                 |
|     |                                                                                                                                                                                                                                 |                                                                                                                                                                                                                                 |                                                                                                                                                                                                                         |                                                                                                                                                                                                                                             |                                                                                                                                                                                                                           |                 |
### Punti di frizione
1. (Es. "La navigazione tra le pagine è lenta e l'utente può sentirsi frustrato.")
2. (Es. "Il processo di checkout non è chiaro, creando dubbi sull'accuratezza dei dati inseriti.")
### Opportunità di Miglioramento
1. (Es. "Ottimizzare il tempo di caricamento per migliorare l'esperienza utente.")
2. (Es. "Semplificare il processo di checkout con meno campi da compilare.")
### KPI 
(Es. "Conversione completata", "Tempo per completare il checkout", "Percentuale di abbandono carrello", ecc.)
# Service Blueprint (front/back)
# UML Activity (happy path + guards)
# UML State Machine (core entity lifecycle)
# UML Sequence (tech communications)
# UML Class (domain snapshot)
# ERD (implementation‑oriented)
# C4: Context (who talks to what)
# Deployment (where it runs)
# Data Flow Diagram (level‑0)
# Integration Contract (message schema)
# Trigger & Rules Catalog (matrix)
# RBAC Matrix (who can do what)
# Monitoring Map (what to observe)