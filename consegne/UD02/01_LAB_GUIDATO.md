# Consegna UD02 — Laboratorio guidato

## Contesto verificato

* Azure Portal accessibile: sì.
* Azure CLI autenticata: sì.
* Sottoscrizione corretta verificata senza pubblicarne l'ID: sì.
* Località scelta e motivo: `Italy North`, scelta perché è la regione richiesta dal laboratorio e consente di mantenere le risorse nella località prevista dall'esercitazione.

## Ambiente creato

| Elemento        | Nome tecnico           | Tipo                    | Località    | Scopo                                                 |
| --------------- | ---------------------- | ----------------------- | ----------- | ----------------------------------------------------- |
| Resource group  | `rg-cea-ud02-e4b568cc` | Resource Group          | Italy North | Raggruppare e gestire le risorse del laboratorio UD02 |
| Rete virtuale   | `vnet-cea-ud02`        | Virtual Network         | Italy North | Fornire la rete virtuale del laboratorio              |
| Storage account | `stceae4b568cc`        | StorageV2, Standard LRS | Italy North | Fornire lo storage richiesto dal laboratorio          |

La rete virtuale utilizza lo spazio di indirizzamento `10.20.0.0/16` e contiene la subnet `snet-app` con prefisso `10.20.1.0/24`.

## Decisioni e verifiche

Le risorse sono state inserite nello stesso Resource Group perché appartengono allo stesso laboratorio e devono poter essere gestite, inventariate e sottoposte a cleanup come un'unica unità.

Nel modello cloud rimangono al cliente le responsabilità relative alla configurazione delle risorse, alla gestione degli accessi, alla protezione dei dati e alla corretta eliminazione delle risorse non più necessarie.

Sono stati utilizzati nomi tecnici coerenti con l'UD02 e un suffisso univoco per evitare conflitti, in particolare per lo Storage Account. Sono stati applicati i tag:

* `course=cloud-engineer-academy`
* `unit=UD02`
* `environment=lab`
* `deleteAfter=2026-09-10`

Durante il laboratorio è stata osservata una differenza pratica tra Portale Azure e Azure CLI: il Portale è risultato più utile per esplorare e configurare visivamente le numerose proprietà delle risorse, mentre la CLI ha permesso di verificare in modo rapido e ripetibile valori specifici, come località, SKU, TLS, accesso pubblico e tag.

Esempio di verifica CLI utilizzata per lo Storage Account:

```bash
az storage account show \
  --resource-group "$LAB_RG" \
  --name "$LAB_STORAGE" \
  --query "{Name:name,Location:location,Kind:kind,Sku:sku.name,HttpsOnly:enableHttpsTrafficOnly,MinimumTls:minimumTlsVersion,PublicBlobAccess:allowBlobPublicAccess,State:provisioningState,Tags:tags}" \
  --output jsonc
```

La verifica ha confermato `StorageV2`, `Standard_LRS`, `TLS1_2`, trasferimento sicuro abilitato, accesso anonimo ai Blob disabilitato e stato `Succeeded`.

L'inventario finale tramite CLI ha confermato la presenza della VNet e dello Storage Account nel Resource Group, entrambi in `italynorth` e con tag `unit=UD02` e `deleteAfter=2026-09-10`.

## Cleanup

* operazione di eliminazione: eseguita al termine del laboratorio, dopo aver completato anche la parte autonoma e la verifica.

* controllo utilizzato: `az group exists --name "$LAB_RG"` dopo la richiesta di eliminazione.

* risultato finale: il comando ha restituito `false`, confermando che il Resource Group `rg-cea-ud02-e4b568cc` è stato eliminato correttamente.

* eventuale anomalia e soluzione: durante la verifica della VNet, la prima interrogazione CLI della subnet ha restituito `null` perché è stata utilizzata la proprietà `addressPrefixes`, mentre la risorsa esponeva il valore tramite `addressPrefix`. Una verifica specifica con `addressPrefix` ha confermato correttamente `10.20.1.0/24`.

## Rilevanza professionale

L'inventario permette di sapere quali risorse sono state create e con quali caratteristiche. I tag rendono più semplice classificare le risorse, associarle a un progetto o ambiente e individuare quelle che devono essere eliminate.

La verifica del cleanup consente di accertarsi che le risorse temporanee siano state effettivamente rimosse. L'utilizzo combinato di Portale Azure e CLI rende la procedura più ripetibile, verificabile e controllabile, caratteristiche importanti nella gestione professionale di ambienti cloud.
