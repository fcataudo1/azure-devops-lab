# Domande di controllo — UD02

## 1. Perché una macchina virtuale lascia al cliente più responsabilità operative rispetto ad App Service?

Una macchina virtuale è un servizio IaaS: Microsoft gestisce l'infrastruttura fisica e la virtualizzazione, mentre il cliente deve gestire anche il sistema operativo, gli aggiornamenti, il software installato e la configurazione della macchina. App Service è invece un servizio PaaS, quindi Microsoft gestisce anche il sistema operativo, il runtime e parte della piattaforma. Il cliente può quindi concentrarsi maggiormente sul codice e sulla configurazione dell'applicazione.

## 2. Qual è la differenza tra tenant, sottoscrizione e resource group?

Il tenant Microsoft Entra rappresenta la directory che contiene identità e oggetti dell'organizzazione. La sottoscrizione è il confine utilizzato per fatturazione, quote, accesso e organizzazione delle risorse. Il resource group è invece un contenitore logico all'interno di una sottoscrizione, utilizzato per raggruppare risorse che hanno normalmente uno scopo o un ciclo di vita comune.

## 3. Perché la località del resource group non obbliga tutte le risorse a usare la stessa region?

La località del resource group indica principalmente dove Azure conserva i metadati del contenitore. Le singole risorse possono avere una propria località e possono quindi essere distribuite in region differenti, compatibilmente con la disponibilità del servizio e con i suoi vincoli.

## 4. Quale differenza esiste tra una region e un'availability zone?

Una region è un'area geografica di Azure che contiene uno o più datacenter collegati. Un availability zone è invece una parte separata di una region, costituita da uno o più datacenter con infrastruttura indipendente per alimentazione, raffreddamento e rete. Le availability zone permettono di aumentare la resilienza distribuendo le risorse tra zone differenti.

## 5. Perché `az account show` deve precedere la creazione di una risorsa?

Perché permette di verificare il contesto Azure attualmente utilizzato, in particolare la sottoscrizione attiva. Se sono disponibili più sottoscrizioni, controllare il contesto prima del deployment evita di creare una risorsa nella sottoscrizione sbagliata.

## 6. Perché i tag non devono essere usati come meccanismo di sicurezza?

I tag sono coppie chiave-valore utilizzate principalmente per organizzazione, inventario, governance e analisi dei costi. Non controllano i permessi di accesso alle risorse e quindi non sostituiscono identità, ruoli e autorizzazioni.

## 7. Quale vantaggio offre Azure CLI rispetto alla sola operazione nel portale?

Azure CLI permette di eseguire procedure riproducibili attraverso comandi che possono essere salvati, confrontati, riutilizzati e successivamente inseriti in script o pipeline. Il portale è utile per esplorare e verificare le risorse, ma le operazioni manuali sono generalmente meno facilmente ripetibili.

## 8. Perché l'esecuzione del comando di eliminazione non dimostra da sola che il cleanup sia concluso?

Perché dopo aver avviato l'eliminazione bisogna verificare che Azure abbia effettivamente completato l'operazione e che il resource group non esista più. La verifica finale permette di evitare che risorse rimangano attive e possano continuare a generare costi.
