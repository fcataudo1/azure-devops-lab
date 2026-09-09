# Consegna UD03 — Verifica

## Parte A — Scelta singola

Per le domande 1–8 riporta risposta e motivazione.

1. **B — L'autenticazione è riuscita, ma manca un'autorizzazione applicabile.**
   L'utente è riuscito ad accedere al portale, quindi l'identità è stata autenticata. Il problema riguarda invece i permessi necessari per leggere il Resource Group.

2. **D — Password.**
   Una role assignment Azure è composta da principal, role definition e scope. La password riguarda l'autenticazione dell'identità e non fa parte dell'assegnazione RBAC.

3. **C — Reader sul Resource Group.**
   Reader permette di consultare le risorse senza concedere permessi di modifica. È quindi la scelta che applica il principio del minimo privilegio.

4. **B — Ereditato.**
   Un ruolo assegnato alla subscription viene normalmente ereditato dagli scope figli, come Resource Group e risorse contenute.

5. **B — Contributor.**
   Contributor può creare e modificare risorse Azure, ma normalmente non può gestire le assegnazioni RBAC. La gestione degli accessi richiede permessi specifici.

6. **C — Impedisce l'eliminazione finché applicabile.**
   Un lock `CanNotDelete` protegge lo scope dalla cancellazione, pur consentendo normalmente le operazioni di lettura.

7. **B — Genera una condizione di notifica, ma non costituisce un tetto automatico.**
   Il budget permette di monitorare la spesa e generare avvisi al raggiungimento delle soglie configurate, ma non blocca automaticamente le risorse o la spesa.

8. **B — Microsoft Entra ID con ruolo appropriato.**
   La creazione e gestione degli utenti cloud appartiene alla gestione delle identità Microsoft Entra e richiede un ruolo Entra appropriato.

## Parte B — Risposte brevi

9. Assegnare ruoli a un gruppo è spesso preferibile perché permette di gestire gli accessi in modo centralizzato. Si assegna il ruolo una volta al gruppo e gli utenti ricevono i relativi permessi tramite l'appartenenza al gruppo. Questo semplifica l'aggiunta, la rimozione e la gestione degli utenti e riduce le assegnazioni individuali.

10. Un ruolo **Microsoft Entra** gestisce identità e oggetti della directory. Ad esempio, `User Administrator` può essere utilizzato per gestire gli utenti. Un ruolo **Azure RBAC** controlla invece l'accesso alle risorse Azure. Ad esempio, `Reader` assegnato a un Resource Group permette di visualizzarne le risorse senza modificarle.

11. L'accesso effettivo è **Contributor** sul Resource Group. Il ruolo Contributor assegnato alla subscription viene ereditato dal Resource Group e concede permessi più ampi rispetto a Reader. L'assegnazione Reader diretta non riduce né revoca i privilegi Contributor ereditati.

12. Per diagnosticare un errore `AuthorizationFailed` si possono eseguire questi controlli in ordine:

13. verificare l'account attivo con `az account show`;

14. verificare la subscription e lo scope della risorsa;

15. controllare le assegnazioni RBAC dirette;

16. controllare le assegnazioni RBAC ereditate;

17. verificare che il ruolo posseduto permetta effettivamente l'operazione richiesta.

18. Il tag `deleteAfter` è un'informazione utilizzata per indicare quando una risorsa dovrebbe essere eliminata, ma non impedisce la cancellazione. Il lock `CanNotDelete` è invece una protezione tecnica che impedisce l'eliminazione finché rimane applicato. Il budget serve a monitorare i costi e generare avvisi al raggiungimento delle soglie, ma non blocca automaticamente la spesa.

## Parte C — Caso situazionale

14. Le interpretazioni errate sono almeno due. La prima è assegnare **Contributor sull'intera subscription** quando il tecnico deve soltanto consultare una VNet: è un privilegio molto più ampio del necessario. La seconda è pensare che Contributor permetta normalmente di assegnare ruoli RBAC ad altri utenti: la gestione delle role assignment richiede permessi specifici. Inoltre, l'errore `ScopeLocked` non è un problema di RBAC, ma indica la presenza di un lock che protegge lo scope dalla cancellazione.

15. Il ruolo iniziale più appropriato è **Reader** sul Resource Group `rg-network-prod`, se il tecnico deve consultare la VNet e le altre risorse del gruppo. Se deve consultare esclusivamente la VNet, è preferibile restringere ulteriormente lo scope alla singola risorsa.

16. I due problemi hanno cause differenti.
    **Impossibilità di assegnare Reader:** il ruolo Contributor consente normalmente di gestire le risorse, ma non di creare o modificare assegnazioni RBAC; manca quindi un'autorizzazione specifica per la gestione degli accessi.
    **Errore `ScopeLocked`:** sul Resource Group o su uno scope superiore è presente un lock `CanNotDelete`, che impedisce la cancellazione finché il lock rimane applicabile. Il lock è un controllo distinto dai normali permessi Azure RBAC.
