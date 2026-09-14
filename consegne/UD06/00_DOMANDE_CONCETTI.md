# UD06 — Risposte alle domande sui concetti

## 1.

**Risposta:**

Una VM Azure è composta da diversi elementi che collaborano tra loro. L'**Image** rappresenta la base utilizzata per creare il sistema operativo della VM, mentre la **Size** determina le risorse computazionali disponibili, come vCPU e RAM. L'**OS Disk** contiene il sistema operativo, mentre uno o più **Data Disk** possono essere utilizzati per dati e applicazioni. La **NIC (Network Interface Card)** collega la VM alla rete Azure e gestisce la connettività tramite IP privato. Alla configurazione di rete possono essere associati anche un **Public IP**, una **Subnet** e un **NSG** per controllare il traffico.

## 2.

**Risposta:**

La presenza di un Public IP non garantisce che un servizio sulla VM sia raggiungibile da Internet. Il traffico deve poter seguire un percorso di rete valido e deve essere consentito dalle regole dell'**NSG**. Inoltre, il firewall del sistema operativo potrebbe bloccare la connessione e il servizio applicativo deve essere attivo e in ascolto sulla porta corretta. Quindi un Public IP permette di indirizzare il traffico verso la risorsa, ma non autorizza automaticamente la connessione.

## 3.

**Risposta:**

Le **Availability Zone** sono zone fisicamente separate all'interno della stessa regione Azure e permettono di distribuire le risorse tra infrastrutture fisiche differenti. Un'**Availability Set** distribuisce invece più VM tra Fault Domain e Update Domain, riducendo l'impatto di guasti fisici e manutenzioni pianificate. Il **VM Scale Set** gestisce invece un insieme di VM secondo una configurazione coordinata e permette di aumentare o ridurre il numero di istanze. Quindi Zone e Set riguardano principalmente la disponibilità, mentre il Scale Set è orientato anche alla gestione e all'elasticità delle istanze.

## 4.

**Risposta:**

Lo **scale up** consiste nell'aumentare le risorse di una singola istanza, ad esempio passando da 2 a 4 vCPU. Lo **scale out** consiste invece nell'aumentare il numero di istanze che eseguono il workload, ad esempio passando da 1 a 3 VM. Lo scale up rende quindi più potente la stessa macchina, mentre lo scale out distribuisce il carico tra più macchine.

## 5.

**Risposta:**

Azure Monitor Autoscale può modificare automaticamente il numero di istanze di un **VM Scale Set** in base a regole definite. Una regola può utilizzare una metrica, una soglia e un intervallo di tempo, ad esempio aggiungendo istanze se la CPU media rimane superiore al 70% per 5 minuti. Sono importanti i limiti **minimum** e **maximum** perché stabiliscono rispettivamente il numero minimo e massimo di istanze che possono essere utilizzate. In questo modo si evita sia di avere una capacità insufficiente sia di aumentare le risorse senza controllo, con conseguenze anche sui costi.

## 6.

**Risposta:**

L'**App Service Plan** definisce l'ambiente di capacità e le caratteristiche della piattaforma sulla quale vengono eseguite le applicazioni, come regione, sistema operativo, tier e capacità. La **Web App** rappresenta invece l'applicazione ospitata all'interno del piano. Più Web App possono essere associate allo stesso App Service Plan e condividere la capacità disponibile.

## 7.

**Risposta:**

**Azure Monitor Autoscale** e **Automatic Scaling** sono due modalità differenti. Azure Monitor Autoscale utilizza regole definite esplicitamente dal cliente, basate ad esempio su metriche, soglie, finestre temporali o schedule. L'**Automatic Scaling** di App Service è invece una modalità maggiormente orientata al traffico, nella quale la piattaforma gestisce lo scaling utilizzando parametri specifici, come Always ready, Maximum burst e Maximum scale limit. Non sono quindi due nomi diversi per la stessa funzionalità.

## 8.

**Risposta:**

Le **Metrics** sono valori numerici associati al tempo e sono utili per osservare l'andamento di parametri come CPU, traffico, richieste o tempo di risposta. I **Logs** sono invece record più dettagliati che possono essere interrogati e analizzati, ad esempio tramite un **Log Analytics workspace** utilizzando KQL. Le metriche sono quindi particolarmente utili per trend, grafici, soglie e alert, mentre i log sono utili per ricercare e correlare eventi e capire più nel dettaglio cosa è successo.

## 9.

**Risposta:**

Il **Recovery Services vault** è una risorsa Azure utilizzata per organizzare e gestire la protezione e i recovery point dei workload supportati. La **Backup Policy** definisce come deve essere eseguito il backup, stabilendo frequenza, schedule e retention. Il **Recovery Point** rappresenta invece uno stato recuperabile prodotto dal processo di backup. In sintesi, il vault gestisce la protezione, la policy stabilisce quando e per quanto tempo conservare i backup e il recovery point rappresenta lo stato dal quale è possibile effettuare un recupero.

## 10.

**Risposta:**

L'**High Availability** serve a ridurre il downtime causato dal guasto di una singola istanza o componente, ad esempio utilizzando più istanze in modo che il servizio possa continuare a funzionare. Il **Backup** serve a recuperare dati o uno stato precedente, ad esempio dopo la cancellazione o la corruzione dei dati. Il **Disaster Recovery** serve invece a ripristinare il workload dopo un evento grave che rende inutilizzabile l'ambiente principale, eventualmente in un ambiente o in una regione alternativa.

## 11.

**Risposta:**

L'**RPO (Recovery Point Objective)** indica quanta perdita di dati è accettabile in caso di incidente, espressa come intervallo temporale. Ad esempio, un RPO di 1 ora significa che si accetta al massimo la perdita dei dati prodotti nell'ultima ora. L'**RTO (Recovery Time Objective)** indica invece quanto tempo può trascorrere prima che il servizio torni operativo, ad esempio 30 minuti. Quindi RPO riguarda la quantità di dati che posso perdere, mentre RTO riguarda il tempo massimo di indisponibilità accettabile.

## 12.

**Risposta:**

Una VM `deallocated` non viene eliminata. La deallocazione rilascia la capacità **compute** associata alla VM, ma possono continuare a esistere altre risorse collegate che possono generare costi, come dischi, backup o alcuni indirizzi IP in base alla configurazione e allo SKU. Per questo `deallocated` non significa che la risorsa sia stata eliminata e che tutti i costi siano automaticamente azzerati.
