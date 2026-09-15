# UD07 — Consegna laboratorio autonomo

## 1. Script CLI idempotente

* logica: controllo dell'esistenza del Resource Group con `az group exists`; se non esiste viene creato, altrimenti viene riutilizzato. In entrambi i casi vengono applicati i tag `ManagedBy=Autonomo` e `UD=07` e viene mostrato lo stato finale.
* prima esecuzione: creato il Resource Group `rg-ud07-auto` nella regione `westeurope`. Stato finale `Succeeded`.
* seconda esecuzione: il Resource Group è stato riconosciuto come già esistente e riutilizzato. Stato finale `Succeeded`.
* verifica: la seconda esecuzione non ha creato un nuovo Resource Group, dimostrando il comportamento idempotente.

## 2. PowerShell equivalente

* controllo esistenza: utilizzato `Get-AzResourceGroup` con `-ErrorAction SilentlyContinue`; se il Resource Group non esiste viene creato con `New-AzResourceGroup`, altrimenti viene riutilizzato.
* modifica: applicati/aggiornati i tag `ManagedBy=Autonomo` e `UD=07` tramite `Update-AzTag`.
* output: `rg-ud07-auto-ps`, regione `westeurope`, stato `Succeeded`; i tag risultano presenti. La seconda esecuzione ha riutilizzato il Resource Group esistente senza crearne uno nuovo.

## 3. Activity Log

* operazione: `Update resource group` e `Write tags`.
* status: `Succeeded`.
* timestamp: `2026-09-15T13:32:40.651801Z` per `Update resource group` e `2026-09-15T13:32:43.0163713Z` per `Write tags`.

## 4. KQL

1. Le righe aggregate sono `2`.
2. Lo status con la latenza media maggiore è `WARN`, con `470 ms`.
3. `summarize` modifica la granularità perché raggruppa i dati per `Status` e produce una riga aggregata per ogni valore distinto, calcolando le aggregazioni richieste.

## 5. Metrics

* metrica: `UsedCapacity`.
* unità: `Bytes`.
* aggregazione: `Average`.
* dato presente: sì; è stato rilevato un punto dati con timestamp `2026-09-15T12:39:00Z`.
* interpretazione: la metrica rappresenta la capacità utilizzata dallo Storage Account ed è espressa in byte. È stato possibile leggere un dato per la metrica osservata.

## 6. Alert

* scope: Storage Account `stud0789469254`.
* condition: metrica `Transactions`, aggregazione `Total`, operatore `GreaterThan`, soglia `0`.
* severity: `3 - Informational`.
* evaluation frequency: `5 minuti`; finestra di valutazione `5 minuti`.
* Action Group: `ag-ud07`.
* Enabled vs Fired: l'alert risulta `Enabled=true`, quindi la regola è attiva e configurata. Questo non significa che sia `Fired`: `Fired` indica che la condizione dell'alert è stata effettivamente soddisfatta.

## 7. Guasto amministrativo

* sintomo: il Resource Group richiesto non viene trovato.
* errore: `ResourceGroupNotFound`.
* ipotesi: il nome del Resource Group potrebbe essere errato oppure il Resource Group potrebbe non esistere nella subscription corrente.
* controllo: verificare il contesto Azure e confrontare il nome richiesto con i Resource Group disponibili.
* correzione: utilizzare il nome corretto del Resource Group, senza creare `rg-ud07-NON-ESISTE`.
* verifica: eseguire nuovamente `az group show` con il nome corretto.

## 8. Runbook

### Sintomo

Un comando Azure CLI restituisce `ResourceGroupNotFound` oppure una risorsa attesa non viene trovata.

### Contesto

Verificare innanzitutto l'account e la subscription Azure utilizzati, perché un Resource Group può esistere in una subscription diversa da quella attiva.

### Controlli

1. verificare account e subscription correnti;
2. controllare il nome esatto del Resource Group;
3. verificare il nome della risorsa o il relativo Resource ID;
4. controllare l'Activity Log per individuare operazioni recenti sulla risorsa.

### Comandi

```bash
az account show \
  --query "{Subscription:name,User:user.name}" \
  --output table
```

```bash
az group list \
  --query "[].{Name:name,Location:location,State:properties.provisioningState}" \
  --output table
```

```bash
az group show \
  --name <NOME_RG> \
  --output table
```

```bash
az monitor activity-log list \
  --resource-group <NOME_RG> \
  --offset 1h \
  --max-events 20 \
  --output table
```

### Interpretazione

Se il Resource Group non viene trovato, verificare prima il contesto Azure e il nome utilizzato. L'assenza del Resource Group nella subscription corrente non implica necessariamente che la risorsa sia stata eliminata: potrebbe trovarsi in un altro contesto oppure il nome potrebbe essere errato.

### Correzione minima

Correggere il nome o il contesto Azure utilizzato. Non creare nuove risorse finché non è stata verificata l'effettiva assenza della risorsa attesa.

### Verifica

Ripetere il comando di ricerca con il nome corretto e verificare che il Resource Group o la risorsa attesa venga restituita con stato coerente.

### Cleanup

Dopo la verifica, eliminare eventuali risorse di test create durante la procedura e lasciare inalterate le risorse del laboratorio principale.

## 9. Cleanup

* rg-ud07-auto: eliminato e verificato con `az group exists`; risultato `false`.
* rg-ud07-auto-ps: eliminato e verificato con `az group exists`; risultato `false`.
