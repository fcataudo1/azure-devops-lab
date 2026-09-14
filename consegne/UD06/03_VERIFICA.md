# UD06 — Verifica individuale

## Parte A — Scelta singola

### 1. Quale componente collega normalmente una VM Azure alla subnet?

* **A. NIC**

### 2. Quale tecnologia gestisce un insieme di VM e può supportare autoscaling?

* **B. VM Scale Set**

### 3. Passare da 2 a 4 istanze è:

* **B. scale out**

### 4. Quale elemento definisce capacità e tier di una Web App?

* **A. App Service Plan**

### 5. Azure Monitor Autoscale può usare:

* **B. metriche e regole**

### 6. Quale affermazione è corretta?

* **B. Metrics rappresentano valori numerici nel tempo; Logs sono record interrogabili**

### 7. Quale risorsa è centrale nel workflow tradizionale di Azure Backup per VM?

* **A. Recovery Services vault**

### 8. Azure Site Recovery è principalmente associato a:

* **B. Disaster Recovery**

---

## Parte B — Risposte brevi

### 9. Distingui Availability Zone, Availability Set e VM Scale Set.

Le **Availability Zone** sono zone fisicamente separate all'interno di una stessa regione Azure e aumentano la resilienza rispetto a guasti a livello di zona.

Gli **Availability Set** distribuiscono le VM tra fault domain e update domain per ridurre l'impatto di guasti hardware o manutenzioni.

Un **VM Scale Set** gestisce un insieme di VM con configurazione simile e può supportare scaling orizzontale e autoscaling.

### 10. Distingui scale up e scale out.

Lo **scale up** aumenta le risorse di una singola istanza, ad esempio CPU o RAM.

Lo **scale out** aumenta il numero di istanze che eseguono il servizio.

### 11. Distingui App Service Autoscale e Automatic Scaling.

Con **App Service Autoscale** si definiscono metriche, soglie e regole che determinano quando aumentare o diminuire il numero di istanze.

Con **Automatic Scaling** App Service gestisce automaticamente il numero di istanze in base al carico e al traffico, senza richiedere le stesse regole metriche definite manualmente dall'utente.

### 12. Distingui High Availability, Backup e Disaster Recovery.

La **High Availability (HA)** mantiene il servizio disponibile anche in caso di guasto di una componente.

Il **Backup** permette di recuperare dati o uno stato precedente dopo una perdita o modifica indesiderata.

Il **Disaster Recovery (DR)** permette di ripristinare il servizio dopo un evento grave che rende indisponibile l'ambiente principale, eventualmente utilizzando un ambiente alternativo.

### 13. Spiega Recovery Services vault, backup policy e recovery point.

Il **Recovery Services vault** è una risorsa Azure utilizzata per gestire e conservare i dati di backup e le configurazioni di protezione.

La **backup policy** definisce parametri come frequenza dei backup e durata della retention.

Il **recovery point** è un punto nel tempo dal quale è possibile effettuare un ripristino.

### 14. Distingui RPO e RTO.

L'**RPO (Recovery Point Objective)** indica la quantità massima di dati che si può perdere, espressa come intervallo di tempo.

L'**RTO (Recovery Time Objective)** indica il tempo massimo entro cui il servizio deve essere nuovamente operativo dopo un incidente.

---

## Parte C — Caso situazionale

### 15. Qual è la causa più probabile e perché?

La causa più probabile è la regola **`Deny-HTTP` con priorità 100**. Blocca il traffico TCP sulla porta 80 proveniente dal mio IP e viene valutata prima della regola `Allow-HTTP` con priorità 300. Di conseguenza, la richiesta viene negata dall'NSG anche se esiste una regola Allow.

### 16. Qual è la correzione minima e quali verifiche useresti per dimostrare il ripristino end-to-end?

La correzione minima è **rimuovere la regola `Deny-HTTP`** oppure modificarla in modo da non bloccare il traffico HTTP previsto.

Per verificare il ripristino userei:

1. **IP Flow Verify** per verificare che il traffico TCP sulla porta 80 risulti `Allow` e identificare la regola responsabile.
2. Verifica delle **regole NSG**, controllando che la regola di deny non blocchi più il traffico.
3. Verifica dello stato di **Nginx** sulla VM.
4. `curl localhost` per verificare che Nginx risponda internamente.
5. `curl http://<Public-IP>` dall'esterno per verificare il funzionamento end-to-end.
6. Verifica del risultato HTTP `200 OK`.
