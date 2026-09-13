# Consegna UD04 — Domande sui concetti

Per ciascuna domanda 1–8 riporta una risposta motivata.

1. **Perché Azure Blob e Azure Files non sono intercambiabili?**

   Azure Blob è pensato per archiviare oggetti organizzati in container, come immagini, documenti, log e backup. Azure Files invece fornisce condivisioni di file e directory accessibili tramite protocolli come SMB e NFS. La scelta dipende quindi dal modo in cui applicazioni e utenti devono accedere ai dati.

2. **Qual è la differenza tra management plane e data plane?**

   Il **management plane** riguarda la gestione della risorsa Azure, ad esempio creare o modificare uno storage account. Il **data plane** riguarda invece i dati contenuti nella risorsa, ad esempio caricare, leggere o eliminare un Blob.

3. **Perché Contributor sullo storage account non implica accesso Blob con Entra ID?**

   Il ruolo **Contributor** permette di gestire la risorsa Azure tramite il management plane, ma non concede automaticamente i permessi per accedere ai dati Blob tramite Microsoft Entra ID. Per il data plane servono ruoli specifici, come **Storage Blob Data Reader** o **Storage Blob Data Contributor**.

4. **Perché geo-ridondanza e backup risolvono problemi diversi?**

   La geo-ridondanza mantiene copie dei dati in un'altra area geografica per aumentare la disponibilità in caso di problemi nella regione primaria. Il backup invece serve a recuperare dati persi o modificati accidentalmente. Una cancellazione può infatti essere replicata anche nella copia geografica.

5. **Quali fattori valuteresti prima di scegliere Archive?**

   Valuterei soprattutto la frequenza di accesso ai dati, il tempo massimo accettabile per recuperarli, i costi di conservazione e di recupero e la permanenza minima richiesta. Archive è adatto a dati consultati raramente perché richiede la reidratazione prima dell'accesso.

6. **Perché una account key ha un impatto maggiore di una SAS limitata?**

   Una account key permette un accesso molto ampio alle risorse dell'account. Una SAS può invece limitare le operazioni, le risorse e la durata dell'accesso. Per questo la compromissione di una account key può avere conseguenze maggiori.

7. **Quali proprietà rendono una SAS coerente con il minimo privilegio?**

   Una SAS dovrebbe avere solo i permessi necessari, uno scope il più ristretto possibile e una scadenza breve. Deve inoltre essere utilizzata tramite HTTPS e non deve essere pubblicata in repository, screenshot o altri luoghi accessibili.

8. **Perché non possiamo verificare una policy lifecycle aspettando pochi minuti?**

   Una policy lifecycle non viene necessariamente applicata immediatamente. L'elaborazione delle regole può iniziare dopo alcune ore. Per questo non è sufficiente aspettare pochi minuti per verificare se un Blob è stato spostato di tier o eliminato.
