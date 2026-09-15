# UD07 — Verifica individuale

## Parte A — Scelta singola

### 1. In Azure CLI, quale opzione estrae proprietà dall'output strutturato?

**Risposta: A. `--query`**

### 2. Quale formato è particolarmente utile per assegnare un singolo valore a una variabile Bash?

**Risposta: B. `tsv`**

### 3. PowerShell lavora principalmente con:

**Risposta: B. oggetti**

### 4. Activity Log riguarda principalmente:

**Risposta: A. eventi del control plane della subscription**

### 5. Una diagnostic setting serve a:

**Risposta: B. instradare segnali diagnostici verso destinazioni**

### 6. Quale linguaggio si usa normalmente per interrogare Log Analytics?

**Risposta: B. KQL**

### 7. Un Action Group definisce principalmente:

**Risposta: B. notifiche e azioni associate agli alert**

### 8. Una metric alert rule in stato Enabled:

**Risposta: B. viene valutata, ma può non essere Fired**

---

## Parte B — Risposte brevi

### 9. Distingui Activity Log, Metrics e Logs.

L'**Activity Log** registra principalmente le operazioni amministrative e gli eventi del control plane delle risorse Azure.

Le **Metrics** sono dati numerici organizzati nel tempo, ad esempio `UsedCapacity`, `Transactions` o `Availability`.

I **Logs** contengono informazioni più dettagliate sugli eventi e possono essere interrogati con KQL all'interno di Log Analytics.

### 10. Spiega l'idempotenza con un esempio amministrativo.

L'idempotenza significa che una procedura può essere eseguita più volte ottenendo lo stesso stato finale senza creare duplicati o effetti indesiderati.

Un esempio è lo script utilizzato nel laboratorio: controlla con `az group exists` se il Resource Group esiste. Se non esiste lo crea, mentre se esiste lo riutilizza e aggiorna i tag.

### 11. Distingui `table` e `tsv`.

`table` restituisce i dati in una forma tabellare leggibile dall'utente.

`tsv` restituisce valori separati da tabulazioni ed è particolarmente utile quando si deve estrarre un singolo valore da assegnare a una variabile Bash o da utilizzare in un altro comando.

### 12. Spiega perché Log Analytics workspace e diagnostic setting non sono la stessa cosa.

Il **Log Analytics workspace** è la destinazione in cui i dati possono essere raccolti, archiviati e interrogati tramite KQL.

La **diagnostic setting** stabilisce quali categorie di log o metriche devono essere raccolte e verso quale destinazione devono essere inviate.

Quindi il workspace è il contenitore di destinazione, mentre la diagnostic setting configura il flusso dei dati.

### 13. Distingui Alert Rule e Action Group.

L'**Alert Rule** definisce il segnale da controllare e la condizione che deve essere soddisfatta, ad esempio `Transactions > 0`.

L'**Action Group** definisce cosa fare quando l'alert viene attivato, ad esempio inviare una notifica email.

### 14. Perché correlazione temporale non implica causalità?

Perché il fatto che due eventi avvengano vicini nel tempo non dimostra che uno abbia causato l'altro.

Per stabilire una causalità sono necessarie ulteriori analisi e altre evidenze che escludano spiegazioni alternative.

---

## Parte C — Scenario

> Alle 10:15 una risorsa Azure viene modificata. Alle 10:16 l'Activity Log mostra una `write` riuscita. Alle 10:20 una metrica aumenta e alle 10:25 un alert passa a Fired.

### 15. Quali fatti puoi affermare con certezza?

Si può affermare che:

- alle 10:15 è stata effettuata una modifica alla risorsa;
- alle 10:16 l'Activity Log ha registrato una `write` con esito positivo;
- alle 10:20 la metrica osservata è aumentata;
- alle 10:25 l'alert è passato allo stato `Fired`.

Questi eventi sono osservazioni distinte e la loro sequenza temporale può essere descritta senza assumere un rapporto di causalità.

### 16. Quale ulteriore analisi è necessaria prima di affermare che la modifica delle 10:15 ha causato l'alert?

È necessario analizzare più dati, verificando almeno la metrica prima e dopo la modifica, gli eventi dell'Activity Log, eventuali log diagnostici e la configurazione e il periodo di valutazione dell'alert.

Bisogna inoltre considerare eventuali altre modifiche o eventi avvenuti tra le 10:15 e le 10:25.

La vicinanza temporale degli eventi non è sufficiente per dimostrare la causalità.