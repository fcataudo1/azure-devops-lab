# Domande di controllo — UD03

## 1. Perché autenticazione riuscita e autorizzazione sufficiente non sono equivalenti?

L'autenticazione verifica chi sta effettuando l'accesso, mentre l'autorizzazione verifica quali operazioni quell'identità può eseguire e su quali risorse. Per questo un accesso riuscito non significa avere automaticamente i permessi necessari.

## 2. Quale differenza operativa esiste tra ruolo Microsoft Entra e ruolo Azure?

Un ruolo Microsoft Entra gestisce autorizzazioni sugli oggetti della directory, come utenti e gruppi. Un ruolo Azure tramite Azure RBAC controlla invece le operazioni sulle risorse Azure, come resource group, VNet e storage account.

## 3. Quali tre elementi formano una role assignment?

Una role assignment è formata da tre elementi: **principal + role definition + scope**. Il principal è l'identità, la role definition stabilisce i permessi e lo scope indica dove tali permessi si applicano.

## 4. Perché Reader su un resource group è preferibile a Contributor sulla sottoscrizione quando serve soltanto consultare quel progetto?

Reader sul Resource Group segue il principio del minimo privilegio perché permette di consultare le risorse necessarie senza concedere modifiche all'intera sottoscrizione. Contributor sulla sottoscrizione darebbe invece permessi molto più ampi del necessario.

## 5. Perché un ruolo ereditato non si rimuove dalla risorsa figlia?

Un ruolo assegnato a uno scope superiore, come sottoscrizione o Resource Group, viene ereditato dagli scope inferiori. Per questo non può essere semplicemente rimosso dalla singola risorsa figlia: bisogna intervenire sull'assegnazione nello scope da cui viene ereditato.

## 6. Un tag `deleteAfter` impedisce l'eliminazione? Motiva.

No. Un tag è solo un'informazione descrittiva utilizzata per classificazione, governance e gestione dei costi. Non impedisce tecnicamente l'eliminazione della risorsa.

## 7. Che cosa cambia tra lock `CanNotDelete` e ruolo Reader?

`Reader` è un ruolo RBAC che permette di leggere una risorsa senza modificarla. `CanNotDelete` è invece un lock che impedisce l'eliminazione dello scope protetto anche a un'identità che possiede permessi sufficienti per eliminare la risorsa.

## 8. Perché un budget non è sufficiente a garantire che la spesa non superi una cifra?

Un budget non è un limite di spesa. Serve a confrontare la spesa con una soglia e può generare notifiche quando vengono raggiunti determinati livelli. Non blocca automaticamente le risorse né impedisce che la spesa superi la cifra impostata.

