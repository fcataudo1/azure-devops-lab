# Verifica — Modelli cloud e struttura iniziale di Azure

## Parte A — Scelte operative

### 1

**Risposta: B. IaaS**

IaaS è il modello più coerente perché consente al cliente di avere maggiore controllo sul sistema operativo e di installare componenti non supportati da un servizio gestito.

### 2

**Risposta: C. Configurazione dell'applicazione, identità, accessi e dati.**

Con Azure App Service Azure gestisce l'infrastruttura e il servizio sottostante, mentre il cliente rimane responsabile dell'applicazione, delle configurazioni, delle identità, degli accessi e dei dati.

### 3

**Risposta: C. È un contenitore logico per risorse che possono condividere ciclo di vita e governance.**

Il resource group permette di organizzare risorse correlate e di gestirne il ciclo di vita e la governance in modo coordinato.

### 4

**Risposta: B. La località indica dove Azure conserva i metadati del resource group; le risorse possono avere località proprie.**

La location del resource group non obbliga tutte le risorse contenute a trovarsi nella stessa region.

### 5

**Risposta: B. `az account show`**

Il comando permette di verificare la sottoscrizione e il contesto Azure attualmente attivo prima di creare risorse.

Risultato atteso, in forma sintetica:

```text
Name                State    IsDefault
Azure for Students  Enabled  True
```

### 6

**Risposta: C. `stcea02a7f9`**

Uno storage account Azure deve rispettare specifiche regole di naming, tra cui l'uso di caratteri minuscoli e numeri e l'assenza di trattini o spazi.

### 7

**Risposta: B. Il tag documenta l'intenzione, ma serve ancora una procedura o una policy che esegua l'eliminazione.**

Il tag `deleteAfter` è un'informazione di governance e non provoca automaticamente la cancellazione della risorsa.

### 8

**Risposta: C. `az group exists --name <NOME>` restituisce `false`.**

Questo controllo permette di verificare che il Resource Group non esista più dopo l'eliminazione.

## Parte B — Risposte brevi

### 9

Un'azienda può mantenere nel proprio datacenter privato i sistemi che richiedono controllo diretto e utilizzare un cloud pubblico per applicazioni scalabili e servizi gestiti. In questo caso si parla di **cloud ibrido**, perché vengono utilizzati insieme ambiente privato e cloud pubblico.

### 10

Il **tenant Microsoft Entra** rappresenta il perimetro di identità e autenticazione dell'organizzazione. All'interno del tenant possono essere presenti una o più **sottoscrizioni Azure**, che rappresentano il perimetro di fatturazione e gestione delle risorse. Una sottoscrizione contiene uno o più **resource group**, utilizzati per organizzare le risorse e gestirne il ciclo di vita. Le **risorse** Azure, come una VNet o uno Storage Account, appartengono a una sottoscrizione e sono organizzate all'interno di un resource group.

### 11

Una **region** è un'area geografica Azure che contiene uno o più datacenter. Una **availability zone** è invece una suddivisione fisicamente separata all'interno di una region, progettata per aumentare la resilienza rispetto a guasti locali. Per questo una availability zone non è sinonimo di region.

### 12

Azure Portal permette di creare e configurare le risorse tramite un'interfaccia grafica, mostrando in modo visuale le diverse impostazioni. Azure CLI consente invece di eseguire le stesse operazioni tramite comandi riproducibili e facilmente utilizzabili negli script. Nel laboratorio autonomo ho utilizzato la CLI per la creazione e il Portale per una verifica indipendente.

### 13

I tag applicati al resource group identificano e classificano il contenitore, ma non devono essere considerati automaticamente presenti su tutte le risorse. Per avere una classificazione coerente a livello di risorsa è quindi necessario applicare esplicitamente i tag oppure utilizzare meccanismi di governance, come Azure Policy, che ne gestiscano l'ereditarietà o l'applicazione.

## Parte C — Interpretazione tecnica

L'ID della risorsa fornito è:

```text
/subscriptions/<omitted>/resourceGroups/rg-cea-test/providers/Microsoft.Storage/storageAccounts/stceatest01
```

### 1. Informazioni riconoscibili nell'ID

Dall'ID sono riconoscibili:

* **Sottoscrizione:** `<omitted>`
* **Resource Group:** `rg-cea-test`
* **Provider:** `Microsoft.Storage`
* **Tipo di risorsa:** `storageAccounts`
* **Nome della risorsa:** `stceatest01`

L'identificativo della sottoscrizione è stato volutamente anonimizzato nell'esempio.

### 2. Località e tag

La risorsa si trova nella località:

```text
italynorth
```

I tag presenti sono:

```text
environment=lab
unit=UD02
```

### 3. Verifica dell'appartenenza al Resource Group

Userei Azure CLI specificando resource group, nome e tipo della risorsa:

```bash
az resource show \
  --resource-group rg-cea-test \
  --name stceatest01 \
  --resource-type Microsoft.Storage/storageAccounts \
  --query "{Name:name,ResourceGroup:resourceGroup,Type:type,Location:location}" \
  --output jsonc
```

Il comando dovrebbe restituire la risorsa `stceatest01` associata al resource group `rg-cea-test`, con tipo `Microsoft.Storage/storageAccounts` e località `italynorth`.

### 4. Rimozione dell'intero ambiente

Se il resource group contiene soltanto risorse del laboratorio, userei:

```bash
az group delete --name rg-cea-test --yes
```

La cancellazione del resource group consente di rimuovere l'ambiente e le risorse che contiene come un'unica unità.

### 5. Verifica dopo l'eliminazione

Dopo aver atteso la conclusione dell'operazione, verificherei l'assenza del resource group con:

```bash
az group exists --name rg-cea-test
```

Il risultato atteso è:

```text
false
```
