---
title: Ambienti
tags: #introai
---
Definire un problema per un agente significa <span style="color:#ff82b2"><i>caratterizzare l'ambiente</i></span> in cui l'agente opera (ambiente operativo). I problemi vengono descritti con il metodo <span style="color:#ff82b2"><b>PEAS</b></span>:
- <span style="color:#ff82b2"><b>P</b></span>erformance
- <span style="color:#ff82b2"><b>E</b></span>nvironment
- <span style="color:#ff82b2"><b>A</b></span>ctuators
- <span style="color:#ff82b2"><b>S</b></span>ensors

# Formulazione PEAS dei problemi
| Problema              |                    P                     |                    E                    |                      A                      |                     S                     |
|:--------------------- |:----------------------------------------:|:---------------------------------------:|:-------------------------------------------:|:-----------------------------------------:|
| Diagnosi medica       |            diagnosi corretta             |           pazienti, ospedale            |    domande, suggerimenti test, diagnosi     | sintomi, test clinici, risposte paziente  |
| analisi immagini      |  % img/zone correttamente classificate   |        collezioni di fotografie         |     etichettatore di zone nell'immagine     |              array di pixel               |
| rebot "selezionatore" | % delle parti correttamente classificate |          nastro trasportatore           | raccogliere le parti e metterle nei cestini |   telecamera (pixel di varia intensità)   |
| giocatore di calcio   |       fare più gol dell'avversario       | altri giocatori, campo di calcio, porte |       dare calci al pallone, correre        | locazione pallone, altri giocatori, porte |

> [!Example] Esempio: agente guidatore di taxi
> 
> | Performance                                                                                                                     | Environment                            | Actuators                                                                             | Sensors                                                                                                                                          |
> | ------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------- | ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
> | Arrivare alla destinazione, sicuro, veloce, ligio alla legge, viaggio confortevole, minimo consumo di benzina, profitti massimi | Strada, altri veicoli, pedoni, clienti | Sterzo, accelleratore, freni, frecce, clacson, schermi di inerfaccia o sintesi vocale | Telecamere, sensori a infrarossi e sonar, tachimetro, GPS, contachilometri, accellerometro, sensori sullo stato del motore, tastiera o microfono |

# Proprietà dell'ambiente-problema
## Osservabilità
|                                                                                                                 Ambiente <span style="color:#ff82b2"><b>completamente osservabile</b></span> | Ambiente <span style="color:#ff82b2"><b>parzialmente osservabile</b></span> |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:--------------------------------------------------------------------------- |
| L'apparato percettivo è in grado di dare una conoscenza completa dell'ambiente o almeno tutto quello che serve a decidere l'azione. Non c'è bisogno di mantenere uno stato del mondo esterno | Sono presenti limiti o inaccuratezze nell'apparato sensoriale               |
|                                                                                                                                                                                              |                                                                             |

## Ambiente singolo / multiagente
Il mondo può anche cambiare per <span style="color:#ff82b2"><b>eventi</b></span>, non solo per azioni di agenti. Un ambiente multiagente presenta più di un agente e si distingue in:

| Ambiente multi-agente <span style="color:#ff82b2"><b>competitivo</b></span> | Ambiente multi-agente <span style="color:#ff82b2"><b>cooperativo</b></span> |
| ---------------------------------------------------------------------------:|:--------------------------------------------------------------------------- |
|                                    comportamento randomizzato (è razionale) | Stesso obiettivo, comunicazione                                             |

La scelta tra questi è spesso di <span style="color:#ff82b2"><i>designe</i></span>.
## Predicibilità
|                              <span style="color:#ff82b2"><b>Deterministico</b></span> |     <span style="color:#ff82b2"><b>Stocastico</b></span>     | <span style="color:#ff82b2"><b>Non deterministico</b></span>                                      |
| -------------------------------------------------------------------------------------:|:------------------------------------------------------------:|:------------------------------------------------------------------------------------------------- |
| Se lo stato successivo è completamente determinato dallo stato corrente e dall'azione | esistono due elemnti di incertezza con associata probabilità | si tiene traccia di più stati possibili risultato dell'azione (ma non in base ad una probabilità) |
|                                                                           es. scacchi |                   es. guida, tiri in porta                   |                                                                                                   |

## Episodico / sequenziale
|                                                                                                                                     <span style="color:#ff82b2"><b>Episodico</b></span> | <span style="color:#ff82b2"><b>Sequenziale</b></span>               |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------- |
| L'esperienza dell'agente è divisa in episodi atomici indipendenti (es. partite diverse). In ambienti episodici <span style="color:#ff82b2"><i>non c'è bisogno di pianificare</i></span> | Ogni decisione influenza le successive (es. scacchi in una partita) |

## Statico / dinamico
|                    <span style="color:#ff82b2"><b>Statico</b></span> | <span style="color:#ff82b2"><b>Dinamico</b></span>                            | <span style="color:#ff82b2"><b>Semi-Dinamico</b></span>                        |
| --------------------------------------------------------------------:|:----------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| Il mondo non cambia mentre l'agente decide l'azione (es. cruciverba) | Cambia nel tempo, va osservata la contingenza (es. taxi). Tardare = non agire | L'ambiente non cambia ma la valutazione dell'agente si (es. scacchi con timer) |

## Discreto / Continuo
Lo stato (numero finito o meno), il tempo, le percezioni e le azioni possono assumere valori discreti o continui.

## Noto / ignoto
Distinzione riferita allo stato di conoscenza dell'agente sulle leggi fisiche dell'abiente. L'agente conosce l'ambiente oppure deve compiere <span style="color:#ff82b2"><i>azioni esplorative</i></span>?
Noto è diverso da *osservabile*: se ho delle carte coperte, è noto perché conosco le regole, ma non osservabile perché non conosco le carte.
Gli ambienti reali sono i peggiori perché parzialmente osservabili, stocastici, sequenziali, dinamici, continui, multi agente, ignoti.

# Simulatore di ambienti
Si tratta di uno strumento software che si occupa di:
- generare stimoli per gli agenti
- raccogliere le azioni in risposta
- aggiornare lo stato dell'ambiente
- attivare altri processi che influenzano l'ambiente
- valutare le prestazioni degli agenti
- Esperimenti su classi di ambienti (variando le condizioni) essenziale per valutare le capacità di generalizzare
- Valutazione di prestazione come medie su più istanze
Questi strumenti sono utili per gli esperimenti sulle classi di ambienti. Non risolveremo un'istanza, ma diversi tipi (non una partita, ma più tipi di partite) in condizioni diverse. *capire* significa saper *generalizzare*, altrimenti la risposta è stereotipata alla specifica istanza.
