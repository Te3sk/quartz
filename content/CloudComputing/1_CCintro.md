---
title: Introduzione al Cloud Computing
alias: CC intro
tags: #cc
---
# Service-based economy
## Service contracts
I clienti non vogliono sapere come è implementato il servizio, ma dovrebbero scegliere quale servizio usare basandosi sul <span style="color:#ff82b2"><b>contratto</b></span> che viene (o dovrebbe) esposto dal provider.
# QoF & SLAs
La <span style="color:#ff82b2"><b>Qualità del Servizio (</b></span>QoF<span style="color:#ff82b2"><b>)</b></span> è importante tanto quanto il risparmoi, e il <span style="color:#ff82b2"><b>Service Level Agreements (</b></span>SLAs<span style="color:#ff82b2"><b>)</b></span> sono gli strumenti contrattuali attraverso i quali si definiscono i servizi ed i doveri del fornitore verso i clienti.
# Cloud Computing 101
## Stimare la domanda del servizio
La domanda del servizio cambia nel tempo in modo imprevedibile. Quando si crea un servizio i problemi possono essere:
- <span style="color:#ff82b2"><b>Overpositioning:</b></span> assicurarsi prima il provisoring in base al picco atteso porta a uno spreco di risorse
- <span style="color:#ff82b2"><b>Underprovisioning:</b></span> Se la richiesta è sottostimata non si riesce a soddisfarre la richiesta dei clienti. Può far si che un numero di utenti abbandoni il servizio così da arrivare a un picco abbordabile
## Definition of Cloud
Si ha una quantità di risorse <span style="color:#ff82b2"><i>apparentemente infinito</i></span> disponibile ondemand.
> [!quote] Cloud: definizione del NIST
> Il cloud computing è un <span style="color:#ff82b2"><i>modello</i></span> per consentire un accesso attraverso la rete ubique, conveniente, su richiesta ad un insieme condiviso di risorse di calcolo, che possono essere richieste e forniti dopo una richiesta del provider.

Le <span style="color:#ff82b2"><b>idee chiave</b></span> sono:
- un efficiente pooling on demand, con un'interfaccia virtuale autogestito, consumato come servizio
- Poter scalare dinamicamente risorse virtuali attraverso la rete a più clienti
- Decoupling di servizi di computzioni da una specifica tecnologia
Dal punto di vista <span style="color:#ff82b2"><b>economico</b></span> le svolte sono state
- Eliminazione di up font committent per i clienti del cloud
- <span style="color:#ff82b2"><i>pay-per-use</i></span>, apprezzato dai consumatori. Anche se fosse più costoso, è molto vantaggioso in termine di <span style="color:#ff82b2"><i>elasticità</i></span> e <span style="color:#ff82b2"><i>trasferimento di rischio</i></span>
## Modelli di servizio
### pizza as a service - IBM
IBM ha fornito 4 modelli sul *come mangiare una pizza*:
1. fare la pizza a casa (<span style="color:#ff82b2"><b>Traditional on-Premises</b></span>)
2. cuocere a casa una pizza precotta (<span style="color:#ff82b2"><b>Infrastructure as a service</b></span>)
3. ordinare una pizza a casa (<span style="color:#ff82b2"><b>Platform as a service</b></span>)
4. andarla a mangiare al ristorante (<span style="color:#ff82b2"><b>Software as a service</b></span>)
Dal primo al quarto servizio, l'utente deve occuparsi sempre di meno cose (se la fa a casa deve fare tutto, se la va a mangiare al ristorante non deve fare niente).
![[CloudComputing/assets/PizzaAsAService.png]]
## Modelli
|                                           SaaS |                           PaaS                            | laaS                                                               |
| ----------------------------------------------:|:---------------------------------------------------------:|:------------------------------------------------------------------ |
| fornisce software ondemand accessibili con API |   fornisce piattaforme come servizi (VMs, OS, SDKs,...)   | fornisce server virtuali, storage e networking                     |
|       fornisce strutture, OS e app organizzate |           fornisce strutture, OS e enabling SW            | fornisce gestione di tutte le infrastrutture                       |
|                        il cliente non è respon | i clienti sono responsabili di installare e gestire l'app | il cliente è responsabile di tutti gli altri aspetti dello svilupo |
## Deployment Models
![Deployment Models Graph](CloudComputing/assets/deploymentModels.png)
# Ostacoli nell'adozione dei cloud
## Confidenzialità dei dati
- Dove vengono storati i dati concretamente?
- La privacy e l'integrità dei dati sarà garantita? Come?
- Come possiamo sapere se si verifica un errore?
## Businnes continuity / service availability
- Che succede se il cloud provider fallisce?
- CS mantra: "*no single point of failure*"
## Vendor lock-in?
# New Business models
>[!quote]
> *If I had asked people what they wanted, they would have said “faster horses”*
> 			- Henry Ford

I cloud hanno portato a nuovi modelli di business, in particolare il modello <span style="color:#ff82b2"><b>freemium</b></span> (es. spotify), nel quale il servizio gratuito (spesso con pubblicità) è parziale e si può pagare per quello completo, e quello <span style="color:#ff82b2"><b>consumer advertising</b></span>, dove (es. google) dove a pagare è chi vuole pubblicare la pubblicità.