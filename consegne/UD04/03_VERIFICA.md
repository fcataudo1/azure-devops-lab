# Consegna UD04 — Verifica

## Parte A — Scelta singola

Per le domande 1–8 riporta risposta e motivazione.

1. **A — Blob**

   Azure Blob Storage è adatto ad archiviare oggetti come immagini, documenti e altri dati non strutturati accessibili tramite applicazioni.

2. **B — Files**

   Azure Files offre condivisioni di file gestite e accessibili tramite protocolli come SMB e NFS.

3. **B — No, serve un ruolo del piano dati**

   Il ruolo Contributor permette di gestire la risorsa tramite il management plane, ma non concede automaticamente l'accesso ai dati Blob tramite Entra ID. Per il data plane servono ruoli come Storage Blob Data Reader o Storage Blob Data Contributor.

4. **B — ZRS**

   ZRS replica i dati tra più zone di disponibilità nella stessa regione e protegge dal guasto di una singola zona.

5. **B — SAS**

   Una SAS permette di concedere accesso delegato con permessi specifici e una durata limitata.

6. **C — segnala una soglia ma non blocca i consumi**

   Un budget permette di monitorare una soglia di spesa e ricevere notifiche, ma non blocca automaticamente i consumi.

7. **B — soltanto Blob che corrispondono al filtro**

   Una lifecycle rule con prefisso `documents/temporary/` interessa solamente i Blob che corrispondono a quel prefisso.

8. **A — rende esplicito l'uso dell'identità Entra**

   `--auth-mode login` indica alla CLI di utilizzare l'identità autenticata tramite Microsoft Entra ID invece di utilizzare automaticamente una chiave dello Storage Account.

## Parte B — Risposte brevi

9. **Distingui ridondanza e backup.**

   La **ridondanza** mantiene più copie dei dati per aumentare disponibilità e resilienza in caso di guasti. Il **backup** serve invece a recuperare dati persi, eliminati o modificati accidentalmente. La ridondanza non sostituisce il backup perché una modifica o cancellazione può essere replicata anche nelle altre copie.

10. **Distingui management plane e data plane con un comando per ciascuno.**

Il **management plane** riguarda la gestione delle risorse Azure, ad esempio la creazione di un container:

`az storage container create --account-name "$LAB_STORAGE" --name archive --auth-mode login`

Il **data plane** riguarda invece l'accesso ai dati contenuti nella risorsa, ad esempio la lettura dei Blob:

`az storage blob list --account-name "$LAB_STORAGE" --container-name archive --auth-mode login`

11. **Elenca quattro proprietà di una SAS a minimo privilegio.**

Una SAS a minimo privilegio dovrebbe avere:

* solo i permessi necessari;
* scope limitato alla risorsa necessaria;
* durata breve;
* accesso tramite HTTPS.

12. **Spiega perché Archive non è appropriato per dati da recuperare immediatamente.**

Il tier Archive è pensato per dati consultati molto raramente. Prima di poter accedere ai dati è necessario effettuare la reidratazione, che richiede tempo. Per dati che devono essere recuperati immediatamente è più appropriato utilizzare un tier come Hot.

13. **Perché non bisogna salvare account key o SAS nel repository?**

Account key e SAS sono credenziali che possono permettere l'accesso ai dati. Se vengono salvate nel repository possono essere esposte ad altre persone o diventare accessibili tramite la cronologia Git. Per questo non devono essere inserite nei file, nei commit o pubblicate su GitHub.

## Parte C — Caso situazionale

14. **Individua almeno tre problemi.**

Sono presenti diversi problemi di sicurezza:

* Contributor sullo Storage Account non garantisce automaticamente l'accesso al data plane dei Blob;
* omettere `--auth-mode login` può portare a utilizzare metodi di autenticazione diversi dall'identità Entra prevista;
* condividere una account key concede un accesso molto ampio e aumenta l'impatto di una sua compromissione;
* una SAS con permessi completi non rispetta il principio del minimo privilegio;
* una SAS senza una scadenza breve aumenta il periodo durante il quale un eventuale token compromesso può essere utilizzato.

15. **Proponi autorizzazione e scope più appropriati.**

Utilizzerei **Microsoft Entra ID + Azure RBAC** per l'accesso ai dati, assegnando un ruolo data-plane adeguato alle operazioni necessarie.

Per la sola lettura utilizzerei, ad esempio:

`Storage Blob Data Reader`

limitando lo scope alla risorsa necessaria.

Se fosse necessario modificare i Blob utilizzerei invece:

`Storage Blob Data Contributor`

evitando di assegnare permessi più ampi del necessario.

16. **Indica come verificheresti accesso e cleanup senza pubblicare segreti.**

Verificherei l'accesso utilizzando `--auth-mode login` e controllerei le operazioni tramite Azure CLI.

Per una SAS utilizzerei un accesso temporaneo e limitato, senza visualizzare o salvare il token nella consegna. Dopo la verifica eliminerei la variabile che contiene la SAS.

Per il cleanup verificherei le risorse create e rimuoverei prima eventuali assegnazioni RBAC temporanee e successivamente il Resource Group del laboratorio.

Infine controllerei che le risorse siano state effettivamente eliminate e che nel repository non siano presenti account key, SAS, token o altri segreti.
