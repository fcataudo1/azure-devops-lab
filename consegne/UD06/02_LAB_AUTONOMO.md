# UD06 — Consegna laboratorio autonomo

## 1. Baseline

* VM: `vm-ud06-linux`, stato `running`
* Nginx: attivo (`active`)
* HTTP: `HTTP/1.1 200 OK`
* regola NSG: `Allow-HTTP-MyIP`, priorità `310`, TCP porta `80`

## 2–4. Guasto, diagnosi, ripristino

* regola introdotta: `Deny-HTTP-Auto`, priorità `200`, TCP porta `80`, sorgente `185.169.237.155`
* sintomo: il test HTTP esterno va in timeout (`curl: (28) Failed to connect`)
* ipotesi: il traffico HTTP viene bloccato da una regola NSG
* controllo: elenco delle regole NSG, IP Flow Verify, stato di Nginx e `curl localhost`
* causa: `Deny-HTTP-Auto`, che con priorità `200` viene valutata prima di `Allow-HTTP-MyIP` con priorità `310`
* correzione minima: eliminazione della sola regola `Deny-HTTP-Auto`
* verifica: dopo la rimozione, `curl -I http://$LAB_VM_IP` restituisce `HTTP/1.1 200 OK`

## 5. Monitoring

* metrica: `Percentage CPU`
* intervallo: `1 ora`
* aggregazione: `Average`
* valore rilevato: `0,4823%`
* deduzione: la VM ha avuto un utilizzo medio della CPU molto basso, coerente con un carico leggero
* cosa non posso dedurre: da questa sola metrica non posso sapere se Nginx funziona correttamente, se ci sono errori applicativi o se il servizio è raggiungibile dall'esterno

## 6. VMSS Autoscale

* min: `1`
* default: `1`
* max: `4`
* metrica: `Percentage CPU`
* condizione: CPU media `> 70%` per `5 minuti`
* azione: scale out di `1` istanza
* perché max=4: limita il numero massimo di istanze, evitando una crescita incontrollata delle risorse e dei costi

## 7. App Service scaling

* A: `Scale up`
* B: `Azure Monitor Autoscale`
* C: `Automatic Scaling`
* D: `Scale out manuale`

## 8. Backup policy

* frequenza: giornaliera
* orario: `02:00`
* retention: `30 giorni`
* motivazione: proteggere i dati da cancellazioni accidentali, errori o problemi della VM, mantenendo un compromesso tra protezione e costi

## 9. HA / Backup / DR

* A: `HA` — una singola istanza si guasta, ma il servizio continua a funzionare
* B: `Backup` — è necessario recuperare uno stato precedente dopo una cancellazione accidentale dei dati
* C: `DR` — la regione primaria non è disponibile e il servizio deve essere riattivato in un'altra regione

## 10. RPO / RTO

* RPO: `15 minuti` — perdita massima di dati accettabile pari a 15 minuti
* RTO: `60 minuti` — tempo massimo entro cui il servizio deve tornare operativo
