##### 2022/11/29 #date 
Il protocollo SSL viene sviluppato da <span style="color:#ff82b2"><i>Netscape</i></span> nel 1994 (prima versione) per effettuare comunicazioni sicure con il protocollo HTTP. É progettato in modo da permettere la comunicazione tra computer che non conoscono le reciproche funzionalità.
Oggi è stato sostituito dal [[Crittografia/TSL Protocol|TLS (Transport Layer Security)]], che è una sua variante moderna.
# Il Protocollo
Quando <span style="color:#ff82b2"><i>utente</i></span> $\textcolor{#ff82b2}{U}$ desidera accedere via internet ad un servizio offerto dal <span style="color:#ff82b2"><i>sistema</i></span> $\textcolor{#ff82b2}{S}$, SSL garantisce <span style="color:#ff82b2"><b>confidenzialità</b></span>: la trasmissione è cifrata mediante un sistema ibrido
- <span style="color:#ff82b2"><b>cifrario asimmetrico</b></span> per <span style="color:#ff82b2"><i>costruire</i></span> e <span style="color:#ff82b2"><i>scambiare chiavi di sessione</i></span>
- <span style="color:#ff82b2"><b>cifrario simmetrico</b></span> che utilizza queste chiavi per <span style="color:#ff82b2"><i>criptare dati</i></span> nelle comunicazioni successive
Il protocollo SSL garantisce <span style="color:#ff82b2"><i>l'autenticazione dei messaggi accertando l'identità dei due partner</i></span>, attraverso un cifrario asimmetrico o facendo uso di certificati digitali e MAC.
SSL si innesta tra un protocollo di trasporto affidabile (TCP/IP) e un protocollo di applicazione (es. HTTP). SSL è completamente indipendente dal protocollo di applicazione sovrastante. Il <span style="color:#ff82b2"><i>protocollo HTTPS</i></span> infatti, è una combinazione tra SSL e HTTP, utilizzato da Web-server sicuri.
# Struttura
É organizzato su <span style="color:#ff82b2"><b>2 livelli:</b></span>
|                                                                                                                                                                                                                      <span style="color:#2e80f2"><b>SSL Record</b></span> | <span style="color:#2e80f2"><b>SSL Handshake</b></span>                                                                                                                                                                                                                                       |
| -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
|       É al livello più basso, connesso direttamente al protocollo di trasporto ed ha l'obiettivo di incapsulare i dati spediti dai protocolli dei livelli superiori, assicurando <span style="color:#ff82b2"><i>confidenzialità</i></span> e <span style="color:#ff82b2"><i>integrità</i></span> della comunicazion | Responsabile dell'instaurazione e del mantenimento dei parametri usati da SSL Record. Permette all'utente e al sistema di autenticarsi, negoziare gli algoritmi di cifratura e firma, stabilire le chiavi per i singoli algoritmi crittografici e per il MAC |
| <span style="color:#ff82b2"><i>Realizza fisicamente questo canale (</i></span> meccanismo di rete <span style="color:#ff82b2"><i>):</i></span> utilizza la cipher suite stabilita da SSL Handshake per cifrare e autenticare i blocchi di dati, prima di spedirli attraverso il protocollo di trasporto sottostante | crea un canale <span style="color:#ff82b2"><b>sicuro</b></span>, <span style="color:#ff82b2"><b>affidabile</b></span> e <span style="color:#ff82b2"><b>autentico</b></span> tra utente e sistema, entro il quale SSL Record fa viaggiare i messaggi                                                                                                                            |
## SSL Handshake
Il <span style="color:#ff82b2"><b>master secret</b></span> è utilizzato da $U$ e da $S$ per costruire una propria tripla contenente:
- La <span style="color:#ff82b2"><i>chiave segreta</i></span> da adottare nel <span style="color:#ff82b2"><i>cifrario simmetrico</i></span>
- La <span style="color:#ff82b2"><i>chiave</i></span> per l'<span style="color:#ff82b2"><i>autenticazione del messaggio</i></span> (costruzione del *MAC*) 
- La <span style="color:#ff82b2"><i>sequenza di inizializzazione</i></span> per cifrare in modo aperiodico messaggi molto lunghi
Le triple di $U$ e di $S$ sono diverse tra loro ma note ad entrambi i partner: ciascuno usa la propria, il che aumenta la sicurezza delle comunicazioni.
Scambio di messaggi preliminari (<span style="color:#ff82b2"><i>Handshake</i></span>) indispensabili per la creazione del canale sicuro, attraverso questi messaggi il sistema $S$ (<span style="color:#ff82b2"><i>server</i></span>) e l'utente $U$ (<span style="color:#ff82b2"><i>client</i></span>), si identificano a vicenda e cooperano alla costruzione delle chiavi segrete da impiegare nelle comunicazioni simmetriche successive. Il protocollo è organizzato in <span style="color:#ff82b2"><b>passi:</b></span>
## Procedimento
### 1. Utente: Client Hello
$U$ manda a $S$ un messaggio di <span style="color:#ff82b2"><i>client hello</i></span>, con cui:
- richiede la creazione di una connessione SSL
- specifica le prestazioni di 'sicurezza' che desidera siano garantite durante la connessione (cifrari e meccanismi di autenticazione che $U$ può supportare)
- invia una sequenza di byte casuali
> [!exp] Example: cipher suite
> <span style="color:#ff82b2"><b>SSL_RSA_WITH_AES_CBC_SHA1</b></span> dove
> - <span style="color:#ff82b2"><b>RSA</b></span> è per lo scambio di chiavi di sessione
> - <span style="color:#ff82b2"><b>AES</b></span> è per la cifratura simmetrica
> - <span style="color:#ff82b2"><b>CBC</b></span> indica l'impiego di un cifrario a composizione di blocchi
> - <span style="color:#ff82b2"><b>SHA1</b></span> è la funzione hash one-way per il MAC

### 2. Sistema: Server Hello
Il sistema $S$
- <span style="color:#ff82b2"><i>riceve</i></span> il messaggio di <span style="color:#ff82b2"><i>client hello</i></span>
- <span style="color:#ff82b2"><i>seleziona</i></span> una <span style="color:#ff82b2"><i>cipher suite</i></span> che anch'esso è in grado di supportare
- invia a $U$ un messaggio di <span style="color:#ff82b2"><i>server hello</i></span> che specifica la sua scelta e contiene una nuova <span style="color:#ff82b2"><i>sequenza di bit casuali</i></span>
Se $U$ non riceve il messaggio di <span style="color:#ff82b2"><i>server hello</i></span> interrompe la comunicazione
### 3. Sistema: invio del certificato
$\textcolor{#ff82b2}{S}$ <span style="color:#ff82b2"><i>si autentica</i></span> con $U$ inviandogli il proprio <span style="color:#ff82b2"><i>certificato digitale</i></span> e gli eventuali altri certificati della catena di certificazione della sua CA fino alla CA radice.
Se i servizi offerti da $S$ devono essere protetti negli accessi, $\textcolor{#ff82b2}{S}$ <span style="color:#ff82b2"><i>può richiedere a</i></span> $\textcolor{#ff82b2}{U}$ <span style="color:#ff82b2"><i>di autenticcarsi</i></span> inviando il suo certificato digitale. Avviene raramente: la maggior parte degli utenti non ha un certificato personale, il sistema dovrà accertarsi dell'identità dell0utente in un secondo tempo
### 4. Sistema: Server Hello Done
$S$ invia il messaggio <span style="color:#ff82b2"><i>server hello done</i></span> con cui sancisce la fine degli accordi sulla cipher suite e sui parametri crittografici a essa associati
### 5. Utente: autenticazione del sistema
Per accertare l'autenticità del certificato ricevuto da $S$, l'utente $U$
- <span style="color:#ff82b2"><i>controlla</i></span> che la corrente sia inclusa nel <span style="color:#ff82b2"><i>periodo di validità</i></span> del certificato
- che la <span style="color:#ff82b2"><i>CA</i></span> che ha firmato il certificato sia <span style="color:#ff82b2"><i>affidabile</i></span>
- che la <span style="color:#ff82b2"><i>firma</i></span> apposta dalla CA sa <span style="color:#ff82b2"><i>autentica</i></span>
### 6. Utente: invio del pre-master secret e costruzione del master secret
L'utente $U$ costruisce un <span style="color:#ff82b2"><b>pre-master secret</b></span> costituito da una nuova <span style="color:#ff82b2"><b>sequenza di byte casuali</b></span> (i.e. genera un numero segreto), <span style="color:#ff82b2"><i>lo cifra</i></span> con il cifrario a chiave pubblica su cui si è accordato ocn $S$ e <span style="color:#ff82b2"><i>spedisce il relativo crittogramma</i></span> a $S$.
Il <span style="color:#ff82b2"><i>pre-master secret</i></span> viene combinato da $U$ con alcune stringhe note e con i byte casuali presenti sia nel messaggio di <span style="color:#ff82b2"><i>client hello</i></span> che in quello di <span style="color:#ff82b2"><i>server hello</i></span>. A tutte queste sequenze $U$ applica delle funzioni hash one-way secondo una combinazione opportuna, ottiene così il <span style="color:#ff82b2"><b>master secret</b></span>.
### 7. Sistema: ricezione del pre-master secret e costruzione del master secret
$S$ <span style="color:#ff82b2"><i>decifra</i></span> il crittogramma contenente il <span style="color:#ff82b2"><i>pre-master secret</i></span> ricevuto da $U$, calcola il <span style="color:#ff82b2"><b>master secret</b></span> mediante le stesse operazioni eseguite da $U$ al passo 6 (dispongono delle stesse informazioni) ed a questo punto sia $S$ che $U$ conoscono
- il <span style="color:#ff82b2"><i>numero casuale</i></span> che $U$ ha mandato a $S$ (inviato in <span style="color:#ff82b2"><i>chiaro</i></span>)
- il <span style="color:#ff82b2"><i>numero casuale</i></span> che $S$ ha mandato a $U$ (inviato in <span style="color:#ff82b2"><i>chiaro</i></span>)
- il <span style="color:#ff82b2"><b>pre-master secret</b></span> (inviato <span style="color:#ff82b2"><i>cifrato</i></span>)
Ora sia $U$ che $S$ calcolano il <span style="color:#ff82b2"><b>master secret</b></span>
### 8. Utente: invio del certificato (optional)
Se all'utente $U$ viene richiesto un certificato ([[Crittografia/SSL Protocol#3. Sistema: invio del certificato|passo 3]]) ed egli non lo possiede, il sistema interrompe l'esecuzione del protocollo. Altrimenti $U$ invia il proprio certificato con allegate una serie di informazione firmate con la sua chiave privata, tra cui il <span style="color:#ff82b2"><i>master secret</i></span> e tutti i messaggi scambiati fino a quel momento (<span style="color:#ff82b2"><i>SSL-history</i></span>).
$S$ controlla il certificato di $U$ e verifica l'autenticità e correttezza dell SSL-history, in presenza di anomalie, la comunicazione con $U$ viene interrotta.
### 9. Utente/Sistema: messaggio finished
É il primo messaggio protettomediante il <span style="color:#ff82b2"><i>master secret</i></span> e la <span style="color:#ff82b2"><i>cipher suite</i></span> su cui i due partner si sono accordati. Il messaggio viene prima costruito da $U$ e spedito a $S$, poi viceversa. Nei due invii la struttura del messaggio è la stessa, ma cambiano le informazioni in esso contenute. La costruzione avviene in 2 passi:
- si <span style="color:#ff82b2"><i>concatenano</i></span> il <span style="color:#ff82b2"><b>master secret</b></span>, tutti i messaggi di <span style="color:#ff82b2"><b>handshake</b></span> scambiati fino a quel momento e <span style="color:#ff82b2"><b>l'idendità del mittente</b></span> ($U$ o $S$)
- la stringa ottenuta viene trasformata applicando le funzioni SHA-1 e MD5: si ottiene una coppia di valori che costituisce il messaggio <span style="color:#ff82b2"><i>finished</i></span>
Il messaggio è diverso nele due comunicazione perché $S$ aggiunge ai messaggi di <span style="color:#ff82b2"><i>handshake</i></span> anche il messaggio <span style="color:#ff82b2"><i>finished</i></span> ricevuto da $U$. Il destinatario della coppia ($S$ o $U$) non può invertire la computazione precedente in quanto generata da funzioni one-way, ricostruisce l'ingresso delle due funzioni SHA-1 e MD5, le ricalcola e controlla che la coppia generata coincida con quella ricevuta.
## Protocollo macro
Il canale sicuro approntato dal protocollo <span style="color:#ff82b2"><i>SSL Handshake</i></span> viene realizzato dal protocollo <span style="color:#ff82b2"><i>SSL record</i></span>: i dati sono <span style="color:#ff82b2"><b>frammentati in blocchi</b></span> e ciascun blcco viene:
- <span style="color:#ff82b2"><i>numerato, compresso e autenticato</i></span> mediante l'aggiunta di un MAC
- <span style="color:#ff82b2"><i>cifrato</i></span> mediante il cifrario simmetrico su cui i due partner si sono accordati
- <span style="color:#ff82b2"><i>trasmesso</i></span> dall'SSL record utilizzando il protocollo di trasporto
Il destinatario esegue un procedimento inverso sui blocchi ricevuti
- <span style="color:#ff82b2"><i>decifra e verifica la loro integrità</i></span> attraverso il MAC
- <span style="color:#ff82b2"><i>decomprime e riassembla</i></span> i blocchi in chiaro
- li <span style="color:#ff82b2"><i>consegna</i></span> all'applicazione sovrastante
## Sicurezza: Client Hello e Server Hello
Nei passi di <span style="color:#ff82b2"><i>hello</i></span> i due partner creano e si inviano due <span style="color:#ff82b2"><i>sequenze casuali</i></span> per la costruzione del <span style="color:#ff82b2"><i>master secret</i></span>, che risulta così <span style="color:#ff82b2"><b>diverso in ogni sessione</b></span> di SSL. Un crittoanalista non può riutilizzare i messaggi di handshake catturati sul canale per sostituirsi ad $S$ in una successiva comunicazione con $U$.
## MAC dei blocchi di dati
<span style="color:#ff82b2"><i>SSL record</i></span> numera in modo incrementale ogni blocco di dati e <span style="color:#ff82b2"><i>aumenta il blocco</i></span> attraverso un [[Crittografia/Firma Digitale#MAC|MAC]]. Per prevenirela modifica del blocco da parte di un crittoanalista attivo, il MAC viene calcolato come <span style="color:#ff82b2"><i>immagine hash one-way</i></span> di una stringa costruita concatenando
- il contenuto del blocc
- il numero del blocco nella sequenza
- la chiave del MAC
- alcune stringhe note e fissate a priori
Dato che i MAC sono cifrati insieme al messaggio, un crittanalista non può alterarli senza aver forzato prima la chiave simmetrica di cifratura. Un attacco volto a modificare la comunicazione tra i due partner è difficile almeno quanto quello volto alla sua decrittazione.
### Il sistema è autenticato
Il canale definito da <span style="color:#ff82b2"><i>SSL handshake</i></span> è immune da attacchi attivi <span style="color:#ff82b2"><i>man-in-the-middle</i></span> poiché il sistema $S$ viene autenticato con un certificato digitale
L'utente $U$ può comunicare con il <span style="color:#ff82b2"><i>pre-master secret</i></span> al sistema $S$ in modo sicuro attraverso la chiave pubblica presente nel certificato $S$.
Solo $S$ può decifrare quel crittogramma e costruire il <span style="color:#ff82b2"><i>master secret</i></span>, su cui si fonda la costruzione di tutte le chiavi segrete adottate nelle comunicazioni successive.
Solo il sistema $S$, quello cui si riferisce il certificato, potrà quindi entrare nella comunicazione con l'utente $U$
### L'utente può essere autenticato
L'opzionalità dell'autenticazione dell'utente ha reso l'SSL molto diffuso nelle transazioni commerciali via internet, <span style="color:#ff82b2"><i>per gli utenti la necessità di una certificazione può costruire un ostacolo pratico ed economico</i></span>. L'utente può essere autenticato con altri metodi, come <span style="color:#ff82b2"><i>login e password</i></span> o <span style="color:#ff82b2"><i>numero della carta di credito</i></span>.
### Generazione delle sequenze casuali
Le tre sequenze casuali generate da $U$ e da $S$ e comunicate nei messaggi di <span style="color:#ff82b2"><i>client hello</i></span>, <span style="color:#ff82b2"><i>server hello</i></span>, <span style="color:#ff82b2"><i>pre-master secret</i></span> sono usate per creare il <span style="color:#ff82b2"><i>master secret</i></span>, quindi per generare le chiavi segrete di sessione. La sequenza corrispondente al <span style="color:#ff82b2"><i>pre-master secret</i></span> viene generata da $U$ e comunicata per via cifrata a $S$.
<span style="color:#ff82b2"><i>La non predicibilità di questa sequenza è cruciale per la sicurezza</i></span> del canale SSL: una sua cattiva generazione renderebbe il protocollo molto debole.
