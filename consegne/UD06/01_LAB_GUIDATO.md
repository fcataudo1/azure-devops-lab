# UD06 — Consegna laboratorio guidato

## VM

- image: Ubuntu Server 24.04 LTS (`24.04.202608270`)
- size: Standard_B2ts_v2
- private IP: 10.0.0.4
- public IP: 20.215.56.155
- NIC: vm-ud06-linux884
- subnet: snet-vm
- OS disk: vm-ud06-linux_OsDisk_1_faa9db6f6d27486bbf51f0d07ff0347b

## Accesso e workload

- SSH: riuscito tramite chiave SSH `~/.ssh/ud06_azure` e utente `azureuser`
- Nginx: installato e attivo
- test localhost: `HTTP/1.1 200 OK`
- test esterno: `HTTP/1.1 200 OK`
- IP Flow Verify: Access allowed
- regola responsabile: `Allow-HTTP-MyIP`

## Azure Monitor

- metrica VM: Percentage CPU, Network In Total, Network Out Total
- intervallo: Last 1 hour
- aggregazione: Average
- osservazione: CPU media 0,4488%, Network In 1 MiB e Network Out 74,2 KiB; traffico e utilizzo CPU ridotti, coerenti con un carico leggero

## VMSS / Autoscale

- min: 1
- default: 1
- max: 3
- metrica: Percentage CPU
- soglia: > 70% per 5 minuti
- azione: scale out di 1 istanza
- perché serve un max: limita il numero massimo di istanze e quindi la crescita dei costi

## App Service

- App Service Plan: plan-ud06-web
- tier: Free F1
- Web App: app-ud06-15629
- hostname: app-ud06-15629.azurewebsites.net
- test HTTPS: `HTTP/1.1 200 OK`

## Scaling App Service

| Modalità | Basata su |
|---|---|
| Manual | numero di istanze impostato manualmente |
| Azure Monitor Autoscale | metriche, regole e soglie definite dall'utente |
| Automatic Scaling | gestione automatica del numero di istanze da parte di App Service |

## Azure Monitor App Service

- metrica: Requests (Sum), Response Time (Avg)
- osservazione: 1 richiesta nell'ultima ora e tempo di risposta medio di 1,37 secondi

## Backup

- Recovery Services vault: identificato come componente del workflow di Azure Backup
- frequenza ipotizzata: giornaliera
- retention: 30 giorni
- recovery point: punto di ripristino generato dal backup
- backup reale avviato?: no — non previsto nella UD

## HA / Backup / DR

- Scenario A: HA
- Scenario B: Backup
- Scenario C: DR

## Cleanup

- Resource Group eliminato: sì
- `az group exists`: `false`