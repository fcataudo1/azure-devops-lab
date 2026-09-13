# Consegna UD04 — Laboratorio autonomo

## Scelta del servizio e della ridondanza

Il requisito prevede l'archiviazione di documenti consultati frequentemente e di file temporanei eliminabili dopo un giorno.

È stato scelto **Azure Blob Storage** perché è adatto all'archiviazione di dati non strutturati come documenti, immagini, log e backup.

Azure Files sarebbe più adatto nel caso fosse necessaria una condivisione di file e directory accessibile tramite protocolli come SMB o NFS.

Azure Queue Storage è pensato per la gestione di messaggi tra componenti applicative, mentre Azure Table Storage è adatto a dati strutturati NoSQL.

Per la ridondanza:

- **LRS** mantiene più copie dei dati all'interno dello stesso datacenter e viene utilizzato nel laboratorio per contenere complessità e costi.
- **ZRS** replica i dati tra più zone di disponibilità nella stessa regione e sarebbe preferibile in produzione quando è richiesta resilienza al guasto di una singola zona.

## Operazioni e verifica

È stato creato il container privato:

`archive`

utilizzando l'autenticazione tramite **Microsoft Entra ID**.

È stato caricato il file:

`consegne/UD04/01_DOCUMENTO_LAB.txt`

come:

`archive/current/documento.txt`

La verifica ha restituito:

- Blob: `current/documento.txt`
- Tier: `Hot`
- Dimensione: `49 byte`

È stata generata una **User Delegation SAS** con:

- permesso: sola lettura
- scope: singolo Blob `current/documento.txt`
- durata: 15 minuti

L'accesso tramite SAS è stato verificato con `curl` e il file scaricato è risultato identico all'originale tramite `cmp`.

La SAS non è stata pubblicata nella consegna e la variabile contenente il token è stata rimossa al termine della verifica.

## Diagnosi

### AuthorizationPermissionMismatch

- **Sintomo:** l'operazione sul Blob viene rifiutata.
- **Piano:** data plane.
- **Causa:** l'identità utilizzata non possiede il permesso necessario per l'operazione richiesta sui dati.
- **Controllo:** verificare identità, scope e ruolo RBAC assegnato.
- **Correzione:** assegnare il ruolo data-plane minimo necessario, ad esempio `Storage Blob Data Reader` o `Storage Blob Data Contributor`.

### ResourceNotFound: The specified container does not exist

- **Sintomo:** Azure segnala che il container richiesto non esiste.
- **Piano:** data plane.
- **Causa:** nome del container errato, container inesistente oppure Storage Account errato.
- **Controllo:** verificare Storage Account e nome del container.
- **Correzione:** utilizzare il container corretto oppure crearlo se necessario.

### curl: (22) The requested URL returned error: 403

- **Sintomo:** la richiesta HTTP viene rifiutata con errore 403.
- **Piano:** data plane.
- **Causa:** la richiesta non è autorizzata, ad esempio per SAS assente, scaduta, non valida o priva del permesso necessario.
- **Controllo:** verificare permessi, scadenza, scope e validità della SAS.
- **Correzione:** generare una SAS correttamente limitata e valida.

## Lifecycle, costi e cleanup

La lifecycle rule configurata nel laboratorio guidato utilizza il prefisso:

`documents/temporary/`

e quindi si applica solamente ai Blob che iniziano con quel percorso.

Il Blob:

`archive/current/documento.txt`

non viene interessato dalla regola perché appartiene a un container e a un prefisso differenti.

I principali driver di costo considerati sono:

- capacità dei dati archiviati;
- tier di accesso;
- ridondanza;
- operazioni;
- recupero o reidratazione dei dati, quando previsto;
- trasferimento dei dati.

Il cleanup previsto consiste nella rimozione delle assegnazioni RBAC temporanee e successivamente del Resource Group utilizzato per il laboratorio:

`rg-cea-storage-7cc185fa`

La cancellazione verrà effettuata dopo aver completato tutte le verifiche dell'UD04.

## Risultato finale

- nessun segreto pubblicato: sì
- container `archive` creato: sì
- Blob `current/documento.txt` verificato: sì
- tier `Hot`: sì
- dimensione `49 byte`: sì
- SAS read-only limitata a 15 minuti: sì
- accesso tramite SAS verificato: sì
- file confrontato con l'originale tramite `cmp`: sì
- lifecycle analizzato: sì
- cleanup pianificato: sì
