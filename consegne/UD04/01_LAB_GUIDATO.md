# Consegna UD04 — Laboratorio guidato

## Contesto anonimizzato

* resource group: `rg-cea-storage-7cc185fa`
* storage account: `stcea7cc185fa`
* region: `italynorth`
* tipo e ridondanza: `StorageV2`, `Standard_LRS`

## Servizi e configurazione

| Elemento          | Configurazione               | Motivazione                                                 |
| ----------------- | ---------------------------- | ----------------------------------------------------------- |
| Blob container    | `documents`, accesso privato | Utilizzato per archiviare i documenti senza accesso anonimo |
| access tier       | Hot                          | Adatto a dati consultati frequentemente                     |
| accesso pubblico  | Disabilitato                 | Evita l'accesso anonimo ai Blob                             |
| trasferimento/TLS | HTTPS obbligatorio, TLS 1.2  | Migliora la sicurezza delle comunicazioni                   |

## Autorizzazione e lifecycle

È stato assegnato il ruolo **Storage Blob Data Contributor** all'utente con scope sullo Storage Account.

L'accesso tramite **Entra ID** è stato verificato utilizzando `--auth-mode login`, permettendo di leggere, caricare e scaricare Blob.

È stata inoltre verificata la differenza con la **Shared Key**, che permette l'accesso tramite una chiave dello Storage Account e ha un impatto maggiore in caso di compromissione.

È stata infine utilizzata una **User Delegation SAS** con permesso di sola lettura, limitata al Blob `01_DOCUMENTO_LAB.txt` e con durata temporanea.

La lifecycle rule `delete-temporary` è stata configurata con:

* Blob prefix: `documents/temporary/`
* condizione: ultimo aggiornamento superiore a 1 giorno
* azione: eliminazione del Blob
* stato: Enabled

La policy è stata verificata tramite Azure CLI.

## Verifiche, costi e cleanup

Il container `documents` e i Blob sono stati verificati tramite Azure CLI utilizzando Entra ID.

Il Blob `01_DOCUMENTO_LAB.txt` è stato scaricato tramite CLI e confrontato con il file originale tramite `cmp`, ottenendo file identici.

È stato inoltre verificato l'accesso tramite Shared Key e User Delegation SAS.

I principali fattori di costo considerati sono:

* capacità utilizzata;
* ridondanza;
* tier di accesso;
* operazioni;
* eventuale recupero dei dati;
* trasferimento dati.

Il cleanup verrà eseguito al termine del laboratorio autonomo e della verifica finale, rimuovendo il ruolo temporaneo e il Resource Group.

## Rilevanza professionale

Sceglierei **Azure Blob Storage** quando devo archiviare dati non strutturati come documenti, immagini, log o backup.

Per l'accesso alle applicazioni preferirei **Microsoft Entra ID + RBAC**, applicando il principio del minimo privilegio.

Azure Files sarebbe invece più adatto quando è necessaria una condivisione di file e directory accessibile tramite protocolli come SMB o NFS.

## Cleanup finale

Il cleanup finale è stato completato dopo il laboratorio autonomo e la verifica.

Sono state eseguite le seguenti operazioni:

- rimossa la role assignment `Storage Blob Data Contributor`;
- eliminati i file temporanei locali;
- rimosse le variabili contenenti credenziali e SAS;
- eliminato il Resource Group del laboratorio;
- verificata l'effettiva eliminazione del Resource Group.

La verifica finale ha restituito:

`false`

per `az group exists --name "$LAB_RG"`, confermando che il Resource Group non esiste più.