# Consegna UD05 — Domande sui concetti

Per ciascuna domanda 1–8 riporta una risposta motivata.

## 1. Perché due VNet da collegare non devono avere CIDR sovrapposti?

Due VNet da collegare non devono avere CIDR sovrapposti perché gli indirizzi IP delle due reti devono essere distinguibili. Se gli spazi di indirizzamento si sovrappongono, il routing non può determinare correttamente a quale rete appartiene una determinata destinazione.

## 2. Quale rete è più grande, `/24` o `/26`, e perché?

La rete `/24` è più grande. Un blocco `/24` contiene 256 indirizzi complessivi, mentre un blocco `/26` ne contiene 64. In generale, più alto è il numero del prefisso CIDR, più piccola è la rete.

## 3. Perché un public IP non garantisce raggiungibilità?

Avere un public IP non significa automaticamente che una risorsa sia raggiungibile. Devono essere corretti anche il routing, le regole NSG, la porta utilizzata e il servizio che deve essere in ascolto. La raggiungibilità dipende quindi dall'intero percorso del traffico.

## 4. Come viene scelta una regola NSG tra più corrispondenti?

Le regole NSG vengono valutate in base alla priorità. Il numero di priorità più basso viene valutato per primo. Quando viene trovata la prima regola che corrisponde al traffico, la sua azione viene applicata e la valutazione termina.

## 5. Che cosa significa che un NSG è stateful?

Significa che, se un flusso di traffico viene consentito in una direzione, il traffico di risposta relativo a quella stessa connessione viene consentito automaticamente. Non è quindi necessario creare una regola speculare per il traffico di risposta.

## 6. Perché un `Allow` sulla NIC non supera un `Deny` applicabile sulla subnet?

Perché quando sono presenti NSG sia sulla subnet sia sulla NIC, il traffico deve essere consentito da entrambi. Un `Allow` sulla NIC non può annullare un `Deny` applicato dalla subnet.

## 7. Qual è la differenza tra DNS, routing e NSG?

Il DNS traduce i nomi in indirizzi IP. Il routing determina il percorso che deve seguire il traffico verso una destinazione. L'NSG invece controlla se il traffico è consentito o negato in base a regole come origine, destinazione, porta e protocollo.

## 8. Perché IP Flow Verify verrà completato dopo la creazione della VM?

IP Flow Verify verifica se un determinato flusso di traffico è consentito o negato su un endpoint supportato. In UD05 non è ancora presente una VM, quindi non è possibile completare il test di flusso reale. Il test verrà completato in UD06 dopo la creazione della VM.
