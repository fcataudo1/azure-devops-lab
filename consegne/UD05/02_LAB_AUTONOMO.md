# Consegna UD05 — Laboratorio autonomo

## Inventario iniziale

| Elemento | Valore |
|---|---|
| VNet | `vnet-cea-b3bdbb` |
| Address space | `10.50.0.0/16` |
| Subnet web | `10.50.10.0/24` |
| Subnet data | `10.50.20.0/24` |
| NIC web | `nic-web-01` — `10.50.10.4` |
| NIC data | `nic-data-01` — `10.50.20.4` |
| NSG | `nsg-data-b3bdbb` |
| Associazione | subnet `snet-data` |
| Regola iniziale | `Allow-Web-Postgres` — priorità `300` — Allow — TCP/5432 |

Non sono stati riportati subscription ID o altri identificativi personali.

## Guasto e diagnosi

- **Regola introdotta:** `Deny-Web-Postgres-Auto`, priorità `250`, Inbound, Deny, TCP, origine `10.50.10.0/24`, porta `5432`.
- **Ordine di priorità osservato:**
  1. priorità `250` → `Deny-Web-Postgres-Auto` → Deny
  2. priorità `300` → `Allow-Web-Postgres` → Allow
- **Sintomo:** il flusso previsto `10.50.10.0/24 → 10.50.20.0/24:5432/TCP` risulterebbe bloccato dalla regola con priorità più alta.
- **Ipotesi:** conflitto tra due regole NSG che corrispondono allo stesso traffico.
- **Controllo:** le regole sono state ordinate per priorità tramite Azure CLI. La regola Deny con priorità `250` viene valutata prima della regola Allow con priorità `300`.
- **Controllo effective NSG:** non eseguibile perché `nic-data-01` non è associata a una VM in esecuzione. Azure ha restituito `NicMustBeAttachedToRunningVmToGetEffectiveSecurityGroups`.
- **Controllo effective routes:** non eseguibile per lo stesso motivo. Azure ha restituito `NicMustBeAttachedToRunningVmToGetEffectiveRoutes`.
- **Correzione minima:** rimossa esclusivamente la regola autonoma `Deny-Web-Postgres-Auto`.
- **Verifica dopo la correzione:** la lista delle regole mostra nuovamente `Allow-Web-Postgres` come prima regola custom, con priorità `300`.

La diagnosi del conflitto è stata quindi effettuata sulla configurazione NSG. Non è stata dichiarata una verifica della connettività applicativa reale perché nel laboratorio non è presente una VM in esecuzione.

## Casi ulteriori

### InvalidAddressPrefix

- **Livello:** indirizzamento di rete / CIDR.
- **Controllo:** verificare VNet, subnet e prefissi IP configurati.
- **Correzione minima:** utilizzare un prefisso valido, contenuto nella VNet e non sovrapposto ad altre subnet.

### SecurityRuleConflict

- **Livello:** NSG.
- **Controllo:** confrontare priorità, direzione, origine, destinazione, protocollo e porta delle regole.
- **Correzione minima:** modificare o rimuovere la regola custom in conflitto, mantenendo la configurazione minima necessaria.

### DNS name is not resolved

- **Livello:** DNS.
- **Controllo:** verificare la risoluzione del nome e la configurazione DNS.
- **Correzione minima:** correggere il server DNS, il record o il nome utilizzato.
- **Nota:** DNS non autorizza né blocca direttamente il traffico.

### Port filtered by an NSG

- **Livello:** rete / NSG.
- **Controllo:** verificare priorità delle regole e, quando disponibile, effective NSG o IP Flow Verify.
- **Correzione minima:** rimuovere o modificare la regola Deny oppure aggiungere una regola Allow coerente con il flusso richiesto.
- **Nota:** senza una VM in esecuzione non è possibile eseguire un test reale del flusso.

### Routing

- **Livello:** routing.
- **Controllo:** verificare effective routes o Next Hop quando è disponibile una VM.
- **Nota:** una regola NSG corretta non garantisce da sola che il traffico abbia un percorso valido.

### Servizio non in ascolto

- **Livello:** workload / applicazione.
- **Controllo:** verificare il servizio sulla macchina di destinazione e la porta in ascolto.
- **Nota:** questo controllo non è eseguibile nel laboratorio perché non è presente una VM con un servizio reale sulla porta `5432`.

## Distinzione tra fatti e deduzioni

### Fatti osservati

- La VNet e le subnet hanno i CIDR previsti.
- Le due NIC sono state create e associate alle rispettive subnet.
- L'NSG è associato a `snet-data`.
- La regola `Deny-Web-Postgres-Auto` aveva priorità `250`.
- La regola `Allow-Web-Postgres` aveva priorità `300`.
- Dopo la rimozione della regola autonoma è rimasta la `Allow-Web-Postgres` a priorità `300`.
- Azure non ha consentito la verifica delle effective NSG e delle effective routes senza una VM in esecuzione.

### Deduzioni

- La regola Deny con priorità `250` avrebbe prevalso sulla Allow a `300` per il flusso corrispondente.
- Il conflitto delle regole NSG è sufficiente per identificare il problema configurativo.
- Non è possibile dimostrare la connettività applicativa reale senza un workload in esecuzione.

## Cleanup e risultato finale

- **Regola autonoma rimossa:** `Deny-Web-Postgres-Auto`.
- **Regola prevista mantenuta:** `Allow-Web-Postgres`, priorità `300`.
- **Resource group:** eliminato al termine del laboratorio e della verifica finale UD05.
- **Cleanup completo:** eseguito con eliminazione del resource group e verifica della sua rimozione.
- **Commit:** da completare dopo la verifica finale del repository.

Il laboratorio ha dimostrato come una regola NSG con priorità più alta possa rendere inefficace una regola Allow successiva e come distinguere una diagnosi configurativa da una verifica reale del traffico.
