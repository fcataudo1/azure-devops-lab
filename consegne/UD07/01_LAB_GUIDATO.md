# UD07 — Consegna laboratorio guidato

## CLI

* prima esecuzione script: creato il Resource Group `rg-ud07-cli-test`.
* seconda esecuzione: il Resource Group è stato riconosciuto come già esistente e riutilizzato.
* comportamento idempotente: lo script può essere eseguito più volte senza creare duplicati; alla seconda esecuzione riutilizza il Resource Group esistente e aggiorna i tag.
* esempio JMESPath: `"{Name:name,Location:location,Provisioning:properties.provisioningState}"` per selezionare solo le proprietà necessarie.
* quando usare `tsv`: quando serve ottenere un valore semplice, ad esempio il nome di una risorsa, da riutilizzare in una variabile o in un altro comando.

## PowerShell

* `Get-AzContext` verificato: sì, il contesto risultava impostato sulla subscription `Azure for Students`.
* Resource Group test: `rg-ud07-ps-test`.
* prima esecuzione: creato il Resource Group e applicati i tag richiesti.
* seconda esecuzione: il Resource Group esistente è stato riutilizzato e i tag sono stati aggiornati senza creare una nuova risorsa.
* perché il controllo `if` è utile: permette di creare il Resource Group solo quando non esiste, rendendo la procedura ripetibile e idempotente.

## Log Analytics

* workspace: `law-ud07-7726`.
* regione: `polandcentral`.
* query `print`: eseguita correttamente, con risultato `Course=AZ-104`, `UD=7`, `Status=OK`.
* query `datatable`: eseguita correttamente sui componenti `API`, `DB` e `WEB`.
* risultato sintetico: `OK = 2`, `WARN = 1`.

## Activity Log

* evento osservato: `Update resource group`.
* status: `Succeeded`.
* timestamp: `2026-09-15T10:48:23.2870012Z`.
* dati personali omessi: sì.

## Diagnostic Setting

* esito: creata correttamente a livello di subscription.
* destinazione: workspace Log Analytics `law-ud07-7726`.
* AzureActivity disponibile: no, nel periodo osservato non sono stati restituiti eventi.
* fallback usato, se necessario: interpretato il risultato come possibile ritardo di ingestione, senza considerarlo un malfunzionamento del workspace.

## Metrics

* Storage Account: `stud0789469254`.
* metrica: `UsedCapacity`.
* unità: `Bytes`.
* aggregazione: `Average`.
* punto dati disponibile: sì.
* interpretazione: è stato rilevato un punto dati per `UsedCapacity`; la metrica rappresenta la capacità utilizzata dallo Storage Account ed è espressa in byte.

## Alert

* nome: `alert-ud07-storage-transactions`.
* scope corretto: sì.
* condition: `Transactions` con aggregazione `Total`, operatore `Greater than` e soglia `0`.
* severity: `3 - Informational`.
* Action Group: presente, `ag-ud07`, con notifica email.
* perché non è necessario che sia Fired: la regola può essere configurata e attiva senza che la condizione sia stata soddisfatta. Lo stato `Fired` indica invece che la condizione dell'alert è stata effettivamente raggiunta.

## Correlazione

* modifica osservata: aggiornamento del tag `State` dello Storage Account a `Changed`.
* evento Activity Log: `Create/Update Storage Account`, status `Succeeded`.
* la correlazione prova causalità?: no.
* motivazione: la presenza dell'evento conferma che l'operazione amministrativa è stata registrata. La vicinanza temporale con eventuali variazioni delle metriche permette una correlazione temporale, ma non dimostra che una variazione sia stata causata dall'altra.

## Cleanup

* diagnostic setting rimossa correttamente a livello di subscription.
* RG CLI test `rg-ud07-cli-test` eliminato e verificato con `az group exists`: risultato `false`.
* RG PowerShell test `rg-ud07-ps-test` eliminato correttamente.
* RG principale `rg-ud07-monitor` eliminato e verificato con `az group exists`: risultato `false`.
