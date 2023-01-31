# Iaas - Infrastructure as a service
cloud più semplice (di piuù basso livello)
## Server Virtualization
> [!quote] virtualization
> 
> 	* * * * slide * * * *
> introdurre un livello astratto che riesca a supportare OS diversi

## hypervisor
Crea un livello di virtualizzazione e contiene la VMM (virtual machine manager)

### 2 tipologie di hypervisor
Nel tipo 1 la macchina su cui si monta l'hypervisor fa solo da server, negli altri casi si usa il tipo 2 anche se è meno efficiente

# amazon ec2
Una delle funzionalità più importante è l'**autoscaling**: se ho un app che voglio commercializzare, inizio con un server di una certa dimensione, se ricevo più richieste del previsto ci è facilmente permesso di scalare agile.
## pricing
- **OnDemand:** mi collego, scelgo il tipo di VM e la faccio partire
- **Spot instance:** ec2 ha molte più risorse di quante ne usa, quindi è nel suo interesse venderle anche in leasing con grandi sconti
- **reserved instances:** non voglio correre il rischio che, facendo partire la mia app non ci siano risorse disponibili
- **dedicated hosts:** leasing dedicati
ec2 abbraccia l'economia **freemium** concedendo un tot di utilizzo iniziale gratuito
# market share
il mercato dell'iaas è dominato da amazon (ha circa il 40% di tutto). Amazon ha creato questo servizio perché già disponeva di grandi risorse hardware per gestire le sue cose.
