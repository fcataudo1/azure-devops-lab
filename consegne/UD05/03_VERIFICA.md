# Consegna UD05 — Verifica

## Parte A — Scelta singola

### 1.

**Risposta: B — `/24`**

Una rete `/24` contiene 256 indirizzi totali, mentre una `/26` ne contiene 64. Un prefisso CIDR più basso identifica una rete più grande.

### 2.

**Risposta: B — sovrapposizione degli indirizzi**

Due VNet con lo stesso spazio `10.0.0.0/16` hanno indirizzi sovrapposti. Questo rende ambiguo il routing tra le reti e impedisce una corretta interconnessione.

### 3.

**Risposta: B — 200**

Le regole NSG vengono valutate in ordine di priorità numerica crescente. La priorità 200 viene quindi valutata prima della 300.

### 4.

**Risposta: A — subnet e NIC**

Un NSG può essere associato a una subnet e/o a una NIC per controllare il traffico in ingresso e in uscita.

### 5.

**Risposta: A — consentito da entrambi**

Quando sono presenti NSG sia sulla subnet sia sulla NIC, il traffico deve essere consentito da entrambi i livelli. Un Allow su un livello non annulla un Deny sull'altro.

### 6.

**Risposta: A — tradurre nomi in indirizzi**

Il DNS risolve principalmente i nomi in indirizzi IP. Non decide i permessi di accesso e non determina il percorso del traffico.

### 7.

**Risposta: B — IP Flow Verify**

IP Flow Verify permette di verificare, su una VM supportata, se un determinato flusso viene consentito o negato dalle regole NSG.

### 8.

**Risposta: B — non garantisce raggiungibilità**

Un public IP non è sufficiente a garantire la raggiungibilità. Devono essere corretti anche routing, regole NSG, porta e servizio in ascolto.

## Parte B — Risposte brevi

### 9.

Le subnet devono lasciare margine di crescita perché le risorse future devono poter essere inserite senza dover riprogettare la rete. Una pianificazione troppo stretta può causare esaurimento degli indirizzi disponibili.

### 10.

L'**NSG** controlla se il traffico è consentito o negato.
La **route** determina il percorso che il traffico deve seguire.
Il **DNS** risolve i nomi in indirizzi IP.

Sono funzioni diverse e devono essere analizzate separatamente durante il troubleshooting.

### 11.

Un NSG è stateful: quando una connessione viene consentita, il traffico di risposta relativo a quella stessa connessione viene consentito automaticamente. Non è quindi necessario creare una regola inversa per il traffico di risposta.

Questo non significa che una nuova connessione indipendente sia automaticamente autorizzata.

### 12.

Una baseline NSG sulla subnet può essere più semplice da governare perché applica le regole a tutte le NIC presenti nella subnet. In questo modo i controlli comuni vengono centralizzati e non devono essere replicati su ogni NIC.

### 13.

Senza una VM è possibile verificare la configurazione della VNet, delle subnet, delle NIC, delle associazioni NSG e delle regole. Nel laboratorio è stato anche possibile verificare l'ordine delle regole tramite la loro priorità.

Manca invece la verifica effettiva delle NSG, delle route e del traffico reale: Azure richiede una NIC associata a una VM in esecuzione per questi controlli.

## Parte C — Caso situazionale

### 14.

Il cambio di nome non modifica l'esito perché Azure valuta le regole in base alla **priorità**, non in base al nome.

La regola `Deny-Web` ha priorità `150`, mentre `Allow-Web` ha priorità `400`. Entrambe riguardano TCP 443 dalla stessa origine, quindi viene applicata prima la regola Deny.

### 15.

La correzione minima consiste nel modificare la priorità delle regole in modo che l'Allow venga valutata prima del Deny, oppure rimuovere la regola Deny se non è necessaria.

La correzione deve mantenere come origine `10.60.10.0/24` e porta TCP `443`, senza creare un Allow generico da Internet.

### 16.

Se dopo la correzione l'applicazione resta irraggiungibile, controllerei nell'ordine:

1. **DNS** — verificare che il nome venga risolto correttamente.
2. **Routing** — verificare il percorso verso la destinazione e le eventuali UDR.
3. **NSG** — verificare regole effettive, priorità e associazioni subnet/NIC.
4. **Endpoint e porta** — verificare che TCP 443 sia raggiungibile.
5. **Servizio applicativo** — verificare che l'applicazione sia effettivamente in ascolto sulla porta prevista.
6. **Configurazione dell'applicazione** — verificare eventuali errori interni dopo aver escluso i problemi di rete.
