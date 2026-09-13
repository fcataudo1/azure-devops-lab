# Consegna UD05 — Laboratorio guidato

## Piano di indirizzamento

| Elemento | CIDR | Scopo | Sovrapposizioni |
|---|---|---|---|
| VNet | `10.50.0.0/16` | Spazio IP complessivo della rete virtuale | Nessuna |
| subnet web | `10.50.10.0/24` | Segmento per le risorse web | Nessuna |
| subnet data | `10.50.20.0/24` | Segmento per le risorse dati | Nessuna |

Le subnet sono contenute nello spazio della VNet `10.50.0.0/16` e utilizzano intervalli distinti, quindi non si sovrappongono.

## NSG e associazioni

| NSG | Scope associato | Regola | Priorità | Origine | Porta | Esito |
|---|---|---|---|---|---|---|
| `nsg-data-b3bdbb` | subnet `snet-data` | `Allow-Web-Postgres` | `300` | `10.50.10.0/24` | `5432/TCP` | Verificata |

L'NSG è associato alla subnet `snet-data`.

## Verifica effettiva

- NIC create e subnet:
  - `nic-data-01` → `10.50.20.4`, subnet `snet-data`, nessun Public IP.
  - `nic-web-01` → `10.50.10.4`, subnet `snet-web`, nessun Public IP.

- NSG effettivi osservati:
  - La configurazione dell'NSG e l'associazione alla subnet sono state verificate.
  - `az network nic list-effective-nsg` non è disponibile perché `nic-data-01` non è associata a una VM in esecuzione.
  - Azure ha restituito `NicMustBeAttachedToRunningVmToGetEffectiveSecurityGroups`.

- route effettive osservate:
  - `az network nic show-effective-route-table` non è disponibile perché `nic-data-01` non è associata a una VM in esecuzione.
  - Azure ha restituito `NicMustBeAttachedToRunningVmToGetEffectiveRoutes`.

- ciò che è stato verificato:
  - VNet `10.50.0.0/16`.
  - Subnet `snet-web` `10.50.10.0/24`.
  - Subnet `snet-data` `10.50.20.0/24`.
  - Associazione dell'NSG alla subnet `snet-data`.
  - Regola `Allow-Web-Postgres` con priorità `300`.
  - NIC e relativi indirizzi privati.
  - Priorità delle regole NSG tramite un guasto didattico.

- ciò che richiede ancora un workload:
  - Verifica delle effective NSG e delle effective routes sulla NIC.
  - Verifica della connettività reale verso la porta `5432/TCP`.
  - IP Flow Verify e test applicativo reale.

## Costi e cleanup

Le risorse create appartenevano al resource group `rg-cea-network-b3bdbb`.

Il laboratorio non includeva VM o altri workload che potessero generare costi significativi. Al termine del laboratorio autonomo e della verifica è stato eseguito il cleanup, eliminando il resource group e verificandone la rimozione.

Il resource group è risultato eliminato correttamente.

## Rilevanza professionale

La progettazione del CIDR e il controllo delle regole devono precedere il deployment dei workload per evitare sovrapposizioni, configurazioni incoerenti e problemi di connettività difficili da correggere dopo l'integrazione delle risorse.

Il laboratorio mostra inoltre l'importanza di distinguere tra configurazione della rete e verifica del traffico reale: senza un workload in esecuzione è possibile verificare la configurazione, ma non dichiarare la connettività applicativa come verificata.