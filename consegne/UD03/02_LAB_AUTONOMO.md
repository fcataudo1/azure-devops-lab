# Consegna UD03 — Laboratorio autonomo

## Analisi dell'accesso

Scenario progettato per il team FinOps:

| Principal anonimizzato | Ruolo  | Scope                                    | Origine   | Accesso effettivo                                         |
| ---------------------- | ------ | ---------------------------------------- | --------- | --------------------------------------------------------- |
| Gruppo team FinOps     | Reader | Resource Group dell'ambiente applicativo | Diretta   | Lettura di risorse e configurazioni                       |
| Utente corrente        | Owner  | Subscription                             | Ereditata | Privilegi amministrativi già presenti                     |
| Utente corrente        | Owner  | Subscription                             | Ereditata | Privilegi amministrativi già presenti                     |
| Utente corrente        | Reader | Resource Group temporaneo                | Diretta   | Lettura, ma senza riduzione dei privilegi Owner ereditati |

Per il team FinOps è stato scelto il ruolo `Reader` con scope limitato al solo Resource Group. Questa configurazione segue il principio del minimo privilegio: il team deve poter consultare risorse e costi, ma non modificare risorse né gestire accessi.

Non è appropriato utilizzare `Contributor` perché consentirebbe di modificare le risorse. `Owner` è ancora più permissivo perché include anche la gestione degli accessi tramite RBAC.

La verifica CLI con `--include-inherited` ha mostrato due assegnazioni `Owner` a livello di subscription e una assegnazione `Reader` direttamente sul Resource Group. Le assegnazioni Owner sono quindi applicabili per ereditarietà.

L'assegnazione `Reader` temporanea non riduce i privilegi più ampi già posseduti: le autorizzazioni RBAC applicabili vengono combinate e il Reader non revoca né limita un Owner ereditato.

## Diagnosi dei casi

### Caso A — Azure CLI non autenticata

**Sintomo**

```text
Please run 'az login' to setup account.
```

**Causa probabile**

La sessione Azure CLI non è autenticata oppure la sessione disponibile non è più valida.

**Verifica**

```bash
az account show --output table
```

**Correzione minima**

```bash
az login
```

Dopo l'autenticazione è necessario verificare nuovamente l'account attivo.

**Risultato atteso**

`az account show` restituisce un account Azure valido e una subscription disponibile.

Nel laboratorio attuale il caso non è presente: `az account show` ha restituito correttamente l'account Azure for Students attivo.

### Caso B — Permesso RBAC insufficiente

**Sintomo**

```text
AuthorizationFailed ... Microsoft.Authorization/roleAssignments/write ...
```

**Causa probabile**

Il principal non dispone del permesso necessario per creare o modificare assegnazioni RBAC allo scope richiesto.

**Verifica**

```bash
az role assignment list \
  --scope "$RG_SCOPE" \
  --include-inherited \
  --output table
```

La verifica deve considerare sia le assegnazioni dirette sia quelle ereditate.

**Correzione minima**

Utilizzare un'identità che disponga delle autorizzazioni necessarie per gestire le assegnazioni RBAC oppure richiedere l'intervento di un amministratore. Non è necessario concedere privilegi più ampi del necessario.

**Risultato atteso**

L'assegnazione RBAC viene effettuata correttamente solo dopo aver verificato che il principal disponga delle autorizzazioni richieste.

### Caso C — Resource Group protetto da lock

**Sintomo**

```text
ScopeLocked: The scope is locked and can't be deleted.
```

**Causa probabile**

Sul Resource Group è presente un lock di tipo `CanNotDelete`.

**Verifica**

```bash
az lock list --resource-group "$LAB_RG" --output table
```

Nel laboratorio è stato verificato il lock `lock-cea-delete` con livello `CanNotDelete`.

È stato inoltre verificato che:

```bash
az group show --name "$LAB_RG" --output table
```

continua a funzionare. Il lock quindi impedisce la cancellazione, ma non la lettura del Resource Group.

**Correzione minima**

Rimuovere il lock solo quando la cancellazione del Resource Group è effettivamente necessaria e dopo aver verificato che la protezione non sia più richiesta.

**Risultato atteso**

Con il lock attivo, la cancellazione viene bloccata; la lettura delle informazioni del Resource Group rimane possibile.

## Budget, lock e cleanup

### Budget

Il budget serve a monitorare la spesa e generare avvisi al raggiungimento delle soglie configurate. Nel laboratorio è stato configurato:

* nome: `budget-cea-207162`
* scope: Resource Group temporaneo
* periodicità: Monthly
* importo: €1,00
* soglia di alert: 80% del budget sul costo effettivo

La verifica CLI ha confermato la presenza del budget mensile da €1,00.

Il budget non costituisce un limite tecnico alla spesa e non arresta automaticamente le risorse quando viene raggiunta la soglia.

### Lock

Il lock `lock-cea-delete` è di tipo `CanNotDelete`.

Il suo scopo è impedire la cancellazione accidentale del Resource Group. La verifica del laboratorio ha mostrato che un tentativo di eliminazione viene rifiutato con errore `ScopeLocked`, mentre `az group show` continua a consentire la lettura.

### Cleanup

Il cleanup finale è stato completato correttamente:

* [x] rimosso il budget temporaneo `budget-cea-207162`;
* [x] rimossa l'assegnazione RBAC `Reader` creata per il laboratorio;
* [x] rimosso il lock `lock-cea-delete`;
* [x] nessun utente o gruppo temporaneo creato nel laboratorio, poiché è stato seguito il percorso B;
* [x] eliminato il Resource Group temporaneo;
* [x] verificato con `az group exists` che il Resource Group non esista più, con risultato `false`.

Durante il cleanup è stato necessario rimuovere prima il lock, poiché la protezione `CanNotDelete` impediva anche le operazioni di eliminazione sul Resource Group. Non sono stati modificati o rimossi ruoli o oggetti preesistenti al laboratorio.

## Risultato finale

* output anonimizzati utilizzati: sì; non sono stati inseriti subscription ID, tenant ID, object ID o indirizzi personali
* cleanup verificato: sì; il Resource Group temporaneo è stato eliminato e `az group exists` ha restituito `false`
* consegna UD03 completata e verificata
