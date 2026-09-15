# UD07 — Risposte alle domande sui concetti

## 1.

**Risposta:**
`--query` permette di selezionare direttamente le proprietà che ci interessano dall'output JSON di Azure CLI. È preferibile alla ricerca manuale perché lavora sulla struttura dei dati ed è più preciso e utile per automatizzare le procedure.

## 2.

**Risposta:**
`table` mostra i dati in una forma leggibile per una persona, mentre `tsv` restituisce i valori in un formato semplice, utile soprattutto quando vogliamo salvare un valore in una variabile o riutilizzarlo in un comando successivo.

## 3.

**Risposta:**
Lavorare con oggetti in PowerShell significa che i comandi restituiscono oggetti con proprietà e informazioni che possiamo leggere e utilizzare direttamente. Per esempio, un Resource Group può essere salvato in una variabile e possiamo accedere a proprietà come nome, posizione e tag.

## 4.

**Risposta:**
Una procedura amministrativa è idempotente quando può essere eseguita più volte senza produrre effetti indesiderati e porta comunque la risorsa allo stato desiderato. Prima di modificare una risorsa è quindi utile verificare il suo stato attuale.

## 5.

**Risposta:**
L'**Activity Log** registra principalmente le operazioni di amministrazione sulle risorse e sulla subscription. Le **Metrics** sono valori numerici osservati nel tempo, come CPU o numero di richieste. I **Logs** contengono invece record più ricchi che possono essere interrogati per analizzare meglio ciò che è successo.

## 6.

**Risposta:**
Un Log Analytics workspace serve a raccogliere e conservare dati di log provenienti dalle sorgenti configurate e permette di interrogarli tramite query KQL.

## 7.

**Risposta:**
Una diagnostic setting serve a stabilire quali categorie di log o metriche devono essere inviate e verso quale destinazione, ad esempio un Log Analytics workspace, uno Storage Account o un Event Hub.

## 8.

**Risposta:**
L'Activity Log è il registro degli eventi di gestione di Azure e può essere consultato direttamente. `AzureActivity` è invece la tabella di Log Analytics nella quale gli eventi dell'Activity Log possono essere raccolti tramite una diagnostic setting e successivamente interrogati con KQL.

## 9.

**Risposta:**
L'Alert Rule stabilisce quale segnale osservare e quale condizione deve essere soddisfatta per generare un alert. L'Action Group stabilisce invece cosa fare quando l'alert viene attivato, ad esempio inviare una notifica o avviare un'azione automatica.

## 10.

**Risposta:**
Un alert in stato `Fired` indica che la condizione configurata è stata soddisfatta, ma non significa automaticamente che ci sia un incidente reale. La soglia potrebbe essere configurata male oppure il comportamento potrebbe essere normale. È quindi necessario analizzare le evidenze.

## 11.

**Risposta:**
La correlazione indica che due eventi sono avvenuti in relazione temporale o in modo associato, ma non dimostra che uno abbia causato l'altro. Per stabilire la causalità servono ulteriori evidenze e verifiche.

## 12.

**Risposta:**
I passaggi principali sono: descrivere il sintomo, definire il risultato atteso, raccogliere le evidenze, formulare un'ipotesi, verificare l'ipotesi, applicare la modifica minima, ripetere il test e documentare il risultato.
