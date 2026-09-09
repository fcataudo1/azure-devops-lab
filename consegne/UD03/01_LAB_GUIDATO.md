# Consegna UD03 — Laboratorio guidato

## Contesto anonimizzato

* sottoscrizione e tenant verificati: account Azure for Students attivo
* percorso Entra eseguito: **B**
* resource group temporaneo: `rg-cea-identity-207162`

Il percorso B è stato seguito perché l'accesso alla pagina Microsoft Entra richiesta dal laboratorio non era disponibile. Il portale ha restituito un errore HTTP `401` durante il caricamento della pagina. Non sono state effettuate richieste di escalation o modifiche dei privilegi.

## Identità e assegnazione RBAC

| Principal anonimizzato | Ruolo  | Scope                     | Diretta/ereditata | Motivo                                                                 |
| ---------------------- | ------ | ------------------------- | ----------------- | ---------------------------------------------------------------------- |
| Utente corrente        | Reader | Resource Group temporaneo | Diretta           | Verificare un'assegnazione RBAC con il principio del minimo privilegio |

Nel percorso B non sono stati creati utenti o gruppi temporanei. È stato utilizzato l'account corrente come principal.

L'assegnazione del ruolo `Reader` è stata effettuata tramite il portale Azure sul solo Resource Group temporaneo. L'assegnazione è risultata **Active** e **Permanent**.

La verifica tramite Azure CLI ha mostrato:

* `Reader` assegnato direttamente al Resource Group;
* due assegnazioni `Owner` presenti a livello di subscription e quindi ereditate dal Resource Group.

L'accesso effettivo osservato è quindi determinato dalla combinazione delle assegnazioni: il ruolo Reader assegnato al Resource Group non rimuove né limita i privilegi Owner già ereditati dalla subscription.

## Governance e costi

- tag e significato: `course=cloud-engineer-academy` identifica il corso; `unit=UD03` identifica l'unità didattica; `environment=lab` indica l'ambiente temporaneo di laboratorio; `deleteAfter=2026-09-10` indica la data prevista per la rimozione
- lock e operazione impedita: lock-cea-delete (CanNotDelete); il tentativo di eliminazione del Resource Group è fallito con errore ScopeLocked
- stato di Cost Analysis: nessun costo riportato nel periodo corrente
- budget creato: budget-cea-207162, mensile, €1,00, alert su Actual cost all'80%
- motivo per cui il budget non blocca la spesa: il budget serve a monitorare e notificare il superamento della soglia, non costituisce un limite tecnico alla spesa

## Cleanup

Il cleanup finale è stato completato correttamente:

* budget temporaneo `budget-cea-207162` eliminato;
* assegnazione `Reader` creata dal laboratorio rimossa;
* assegnazioni `Owner` preesistenti a livello di subscription non modificate;
* lock `lock-cea-delete` rimosso;
* Resource Group temporaneo `rg-cea-identity-207162` eliminato;
* verifica finale con `az group exists` conclusa con risultato `false`.

Non sono stati eliminati o modificati oggetti preesistenti al laboratorio.


## Rilevanza professionale

L'esercizio permette di distinguere tre livelli diversi di controllo:

* **Autenticazione:** verifica l'identità che sta effettuando l'accesso.
* **Autorizzazione RBAC:** stabilisce quali operazioni l'identità può eseguire sulle risorse e a quale scope.
* **Governance:** introduce vincoli o controlli aggiuntivi, come lock e policy, che possono impedire determinate operazioni anche quando l'identità dispone di permessi RBAC sufficienti.
