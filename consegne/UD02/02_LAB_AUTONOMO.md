# Consegna UD02 — Laboratorio autonomo

## Requisito e piano

Lo scenario richiede un ambiente Azure temporaneo e separato, destinato allo sviluppo, composto da un Resource Group dedicato, una VNet con una subnet specifica e uno Storage Account vuoto.

Il Resource Group viene utilizzato come confine del ciclo di vita dell'ambiente, così da poter gestire e rimuovere le risorse del laboratorio come un'unica unità.

La località utilizzata è la stessa verificata nel laboratorio guidato: `Italy North` (`italynorth`).

La VNet fornisce la rete del workload, mentre lo Storage Account fornisce lo spazio di archiviazione richiesto. I tag permettono di identificare corso, unità, ambiente, scenario e data prevista per il cleanup.

Risorse previste:

* Resource Group con prefisso `rg-cea-ud02-auto-`
* VNet `vnet-cea-auto`
* Subnet `snet-workload`
* spazio indirizzi VNet `10.30.0.0/16`
* spazio indirizzi subnet `10.30.10.0/24`
* Storage Account `StorageV2`
* SKU `Standard_LRS`
* ambiente `dev`
* scenario `autonomous`

Tag previsti:

* `course=cloud-engineer-academy`
* `unit=UD02`
* `environment=dev`
* `scenario=autonomous`
* `deleteAfter` entro due giorni

Nella relazione non vengono riportati identificativi della sottoscrizione, chiavi o token.

## Svolgimento

Prima della creazione è stato verificato il contesto Azure attivo con Azure CLI. La sottoscrizione risultava abilitata e impostata come predefinita.

Sono state generate variabili dedicate allo scenario autonomo senza modificare quelle del laboratorio guidato:

* `AUTO_LOCATION=italynorth`
* Resource Group: `rg-cea-ud02-auto-f1c27ba7`
* VNet: `vnet-cea-auto`
* Subnet: `snet-workload`
* Storage Account: `stceaautof1c27ba7`
* `deleteAfter=2026-09-10`

Prima della creazione dello Storage Account è stata verificata la disponibilità globale del nome, che è risultata disponibile.

### Resource Group

Il Resource Group è stato creato tramite Azure CLI nella località `italynorth`.

Risultato:

* nome: `rg-cea-ud02-auto-f1c27ba7`
* location: `italynorth`
* provisioning state: `Succeeded`

I tag richiesti sono stati applicati correttamente.

### VNet e subnet

La VNet è stata creata tramite Azure CLI con:

* nome: `vnet-cea-auto`
* address space: `10.30.0.0/16`
* subnet: `snet-workload`
* subnet prefix: `10.30.10.0/24`

La VNet è risultata nello stato `Succeeded` e con tutti i tag richiesti.

### Storage Account

Lo Storage Account è stato creato tramite Azure CLI con:

* nome: `stceaautof1c27ba7`
* location: `italynorth`
* kind: `StorageV2`
* SKU: `Standard_LRS`
* HTTPS obbligatorio: `true`
* TLS minimo: `TLS1_2`
* accesso pubblico ai Blob: `false`

Anche in questo caso tutti i tag richiesti risultano presenti e lo stato della risorsa è `Succeeded`.

Non sono stati creati container e non sono state utilizzate o inserite nella consegna chiavi di accesso o token.

### Verifica indipendente tramite Portale Azure

Dopo la creazione tramite CLI, il Resource Group è stato verificato dal Portale Azure.

Nel Resource Group risultano esattamente le due risorse previste:

1. `vnet-cea-auto`
2. `stceaautof1c27ba7`

La verifica della VNet dal Portale ha confermato:

* location `italynorth`
* address space `10.30.0.0/16`
* subnet `snet-workload`
* subnet prefix `10.30.10.0/24`
* stato `Succeeded`
* tag corretti

La verifica dello Storage Account dal Portale ha confermato:

* `StorageV2`
* `Standard_LRS`
* location `italynorth`
* HTTPS obbligatorio
* TLS `1.2`
* accesso pubblico ai Blob disabilitato
* stato `Succeeded`
* tag corretti

Il confronto finale con `az resource list` ha mostrato lo stesso insieme di due risorse presente nel Portale.

## Diagnosi

Durante la verifica della subnet è stata riscontrata un'anomalia nella query utilizzata per leggere il prefisso.

La prima query utilizzava la proprietà `addressPrefixes` e restituiva:

```text
"Prefixes": null
```

È stato quindi verificato il modello restituito dalla CLI interrogando sia `addressPrefix` sia `addressPrefixes`.

La seconda verifica ha restituito:

```text
{
  "AddressPrefix": "10.30.10.0/24",
  "AddressPrefixes": null,
  "Name": "snet-workload"
}
```

L'anomalia riguardava quindi la proprietà utilizzata nella query di verifica e non la configurazione della subnet. Non è stato necessario modificare o ricreare la risorsa.

La subnet risultava correttamente configurata con `10.30.10.0/24`.

È stata inoltre verificata preventivamente la disponibilità del nome dello Storage Account tramite `az storage account check-name`, ottenendo `Available: true`.

## Cleanup e consegna

Al termine delle verifiche è stato eseguito il cleanup dell'ambiente autonomo eliminando il Resource Group dedicato:

`rg-cea-ud02-auto-f1c27ba7`

L'eliminazione è stata avviata tramite:

```bash
az group delete --name "$AUTO_RG" --yes --no-wait
```

Successivamente è stata attesa la conclusione dell'operazione tramite:

```bash
az group wait --name "$AUTO_RG" --deleted
```

La verifica finale è stata eseguita con:

```bash
az group exists --name "$AUTO_RG"
```

Il comando ha restituito:

```text
false
```

Il Resource Group autonomo e le risorse contenute risultano quindi eliminati correttamente.

## Autovalutazione

- Traduco requisiti in nomi, tag e risorse: **Completato**
- Verifico account e località prima della creazione: **Completato**
- Interpreto le proprietà di VNet e Storage: **Completato**
- Confronto Portale e CLI: **Completato**
- Anonimizzo gli identificativi: **Completato**
- Diagnostico una variabile o un nome errato: **Da ripetere**
- Verifico la conclusione del cleanup: **Completato**