# TLS Protocol
```toc
```
##### 2022/11/29 #date 
## Transport Layer Security - TLS
É un protocollo basato su SSL, la cui prima versione è uscita nel 1999 e l'ultima nel 2018. La descrizione è semplificata (e ad alto livello) e permette ad un *client*(*web browser*) e ad un *server*(*sito web*) di **stabilire un insieme di chiavi simmetriche condivise**, da utilizzare per **cifrare e autenticare** la sessione di comunicazione.
## Il protocollo
Come SSL, consente di due parti
|                                                   **Protocollo di Handshake** | **Protocollo Record-layers**                                            |
| -----------------------------------------------------------------------------:|:----------------------------------------------------------------------- |
| protocollo di scambio di chiavi per stabilire le chiavi simmetriche condivise | utilizza le chiavi condivise per cifrare e autenticare la comunicazione |
|                                                                               |                                                                         |

TLS permette al *client* ed al *server* di *autenticarsi reciprocamente*. É tipicamente usato solo per autenticare il *server*. L'autenticazione del *client*, se necessaria, avviene quando la sessione TLS è avviata, ad esempio tramite login/password
### Protocollo di Handshake
| Client $\textcolor{#ff82b2}{C}$                                                      | Server $\textcolor{#ff82b2}{S}$ |
| ------------------------------------------------------------------------------------ | ------------------------------- |
| possiede un insieme di chiavi pubbliche di CA $$\textcolor{#ff82b2}{pk_1,pk_2,...}$$ | possiede una coppia chiave pubblica/chiave privata per la firma digitale $$\textcolor{#ff82b2}{pk_S, sk_S}$$ e un certificato digitale $\textcolor{#ff82b2}{cert_S}$ per $\textcolor{#ff82b2}{pk_S}$ rilasciato da una delle CA di cui C ha la chiave pubblica                                |
#### Passo 1 - Client
$C$ invia a $S$ il messaggio iniziale del protocollo DH per lo scambio di chiavi, che include
- la specifica del *gruppo* $\textcolor{#ff82b2}{G}$ *usato dal client*. Il gruppo $G$ può essere $\textcolor{#ff82b2}{\mathbf{Z}_p^*}$ ($\textcolor{#ff82b2}{p}$ *primo*), oppure una *curva ellittica*
- Il *generatore* $\textcolor{#ff82b2}{g}$, oppure il *punto* $\textcolor{#ff82b2}{B}$ della curva e il suo *ordine*
- il valore $\textcolor{#ff82b2}{g^X}$, oppure il punto $\textcolor{#ff82b2}{xB}$, calcolato usando un *intero segreto* $\textcolor{#ff82b2}{x}$ *scelto casualmente* da $C$
- un *nonce* (sequenza casuale di bit) $\textcolor{#ff82b2}{N_C}$
- informazioni sulla *ciphersuite* che è in grado di supportare
#### Passo 2 - Server
- $S$ completa il protocollo DH inviando un messaggio al client che contiene $\textcolor{#ff82b2}{g^y}$, oppure il punto $\textcolor{#ff82b2}{yB}$, calcolato usando un *intero segreto* $\textcolor{#ff82b2}{y}$ *scelto casualmente* da $S$
- $S$ invia il suo *nonce* $\textcolor{#ff82b2}{N_S}$
- $S$ calcola $\textcolor{#ff82b2}{K=g^{x\:y}}$ (oppure $\textcolor{#ff82b2}{K=y\,x\,B}$) e applica una *KEY DERIVATION FUNCTION* per estrarre da $\textcolor{#ff82b2}{K}$ le chiavi $$\textcolor{#ff82b2}{K'_S, K'_C, K_S, K_C}$$ per una *cifratura autenticata*
- $S$ invia la propria chiave pubblica $\textcolor{#ff82b2}{pk_S}$, il certificato $\textcolor{#ff82b2}{cert_S}$, la *firma* $\textcolor{#ff82b2}{\sigma}$ calcolata usando la chiave privata $\textcolor{#ff82b2}{sk_S}$ su tutti i *messaggi di handshake* inviati
- *Tutti i dati inviati sono cifrati con* $\textcolor{#ff82b2}{K_S'}$
#### Passo 3 - Client
- $C$ calcola $\textcolor{#ff82b2}{K=g^{x\:y}}$ (oppure $\textcolor{#ff82b2}{K=x\:y\:B}$) e deriva le chiavi$$\textcolor{#ff82b2}{K'_S, K'_C, K_S, K_C}$$
- Usa $\textcolor{#ff82b2}{K_S'}$ per recuperarre $\textcolor{#ff82b2}{pk_S}$, il certificato $\textcolor{#ff82b2}{cert_S}$ e la *firma* $\textcolor{#ff82b2}{\sigma}$ (che sono stati cifrati con $\textcolor{#ff82b2}{K_S'}$)
- Se possiede la chiave pubblica della CA che ha rilasciato $\textcolor{#ff82b2}{cert_S}$, verifica il certificato
- Verifica la *firma* $\textcolor{#ff82b2}{\sigma}$ sui messaggi di handshake usando $\textcolor{#ff82b2}{K_C'}$ e lo invia a $S$
### Protocollo Record Layer
Se tutti i passi vanno a buon fine si procede al *protocollo Record Layer*: $C$ usa $\textcolor{#ff82b2}{K_C}$ per cifrare i messaggi che invia a $S$ e $S$ usa $\textcolor{#ff82b2}{K_S}$ per cifrare i messaggi per $C$.
### Discussione
Alla fine del protocollo di Handshake, $\textcolor{#ff82b2}{C}$ *e* $\textcolor{#ff82b2}{S}$ *condividono* le chiavi di sessione $\textcolor{#ff82b2}{K_C}$ e $\textcolor{#ff82b2}{K_S}$ che possono usare per *cifrare e autenticare la comunicazione* ($\textcolor{#ff82b2}{K_C'}$ e $\textcolor{#ff82b2}{K_S'}$ sono usate solo per la fase di Handshake).
Dato che $C$ verifica il certificato, sa che $\textcolor{#ff82b2}{pk_S}$ è la chivae pubblica corretta di $S$. Se la *firma* $\textcolor{#ff82b2}{\sigma}$ è valida, $C$ conclude che sta comunicando con $S$, il quale è l'unico che conosce la chiave privata $\textcolor{#ff82b2}{sk_S}$ associata a $\textcolor{#ff82b2}{pk_S}$.
$S$ firma tutti i messaggi scambiati per l'esecuzione del protocollo DH, $C$ sa che nessun valore è stato modificato. Il protocollo DH assicura che nessun crittoanalista passivo possa ottenere informazioni su $K$ e sulle chiavi derivate da $K$.
*Alla fine della fase di Handshake,* $\textcolor{#ff82b2}{C}$ *sa che condivide le chiavi* $\textcolor{#ff82b2}{K_C}$ *e* $\textcolor{#ff82b2}{K_S}$ *con il legittimo* $\textcolor{#ff82b2}{S}$.
## Differenze tra le versioni
Nelle versioni precedenti, client e server potevano stabilire la chiave $K$ usando un cifrario a chiave pubblica al posto del protocollo DH, il client sceglieva la chiave $K$ e la cifrava con la chaive pubblica $pk_S$ del server. Nell'ultima versione (*TLS 1.3*) non è più possibile, per *garantire la* **[[Glossary#Forward Secrecy|FORWARD SECRECY]]**, cioè la *segretezza delle chiavi di sessioni precedenti nel caso di un server compromesso*.
DH garantisce *forward secrecy* dato che il valore di $\textcolor{#ff82b2}{y}$ del server usato nel protocollo di Handshake può essere cancellato alla fine del protocollo. Senza $\textcolor{#ff82b2}{y}$, il crittoanalista non può ricostruire la chiave di sessione $\textcolor{#ff82b2}{K}$.
Usando un cifrario a chiave pubblica, non si ha *forward secrecy* dato che la chiave privata del server non può essere cancellata. Se un avversario la ottiene, può decifrare i crittogrammi scambiati nelle esecuzioni passate del protocollo di Handshake e recuperare le chiavi di sessioni usate dalle parti coinvolte
## Autenticad Encryption
Combinazione tra *cifratura e autenticazione* del messaggio, al fine di fornire simultaneamente confidenzialità, autenticità e integrità del dato trasmesso
| **Encrypt-then-MAC**                                                                                                                                    |                            **Encrypt-and-MAC**                            | **MAC-then-Encrypt** |                                                                                         |     |               |     |     |
|:------------------------------------------------------------------------------------------------------------------------------------------------------- |:-------------------------------------------------------------------------:| --------------------:| 
| prima si cifra, poi si genera il MAC a partire dal cifrato. Due chiavi: una per cifrare, una per il MAC $$\textcolor{#2e80f2}{m'=C(m,k)\|MAC(C(m,K))}$$ | Una sola chiave: per cifrare e per il MAC $$\textcolor{#2e80f2}{m'=C(m,K)\|MAC(m)}$$ | (*usato in TLS*) Una sola chiave: per cifrare e per il MAC $$\textcolor{#2e80f2}{m'=C(m\|MAC(m), K)}$$ |     |     |

