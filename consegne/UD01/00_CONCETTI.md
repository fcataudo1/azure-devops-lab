# Domande di controllo prima del laboratorio — UD01

## 1. Perché `git --version` può restituire due risultati diversi in PowerShell e in Ubuntu?

Perché PowerShell e Ubuntu sono due ambienti diversi e possono avere installazioni di Git separate. Windows può avere una versione di Git installata sul sistema host, mentre Ubuntu può avere una propria versione di Git all'interno di WSL. Di conseguenza, il comando `git --version` viene eseguito nell'ambiente corrente e può restituire versioni differenti.

## 2. Quale comando distingue una distribuzione WSL 1 da una WSL 2?

Il comando è:

```bash
wsl --list --verbose
```

Nell'output, la colonna `VERSION` indica se la distribuzione utilizza WSL 1 oppure WSL 2. Se viene mostrato `2`, la distribuzione è in esecuzione con WSL 2.

## 3. Perché conserveremo i progetti in `~/workspace` e non principalmente in `/mnt/c`?

Perché `~/workspace` si trova direttamente nel filesystem Linux di WSL. Questo permette di lavorare in un ambiente più coerente con gli strumenti Linux e con i tool di sviluppo utilizzati nel corso. `/mnt/c`, invece, permette di accedere al filesystem di Windows e non rappresenta la posizione principale consigliata per i progetti utilizzati frequentemente in WSL.

## 4. Che differenza c'è fra configurare l'autore di un commit e autenticarsi su GitHub?

Configurare l'autore del commit significa impostare il nome e l'indirizzo e-mail che Git assocerà ai commit creati localmente. L'autenticazione su GitHub, invece, serve a dimostrare che si dispone dei permessi necessari per interagire con il repository remoto, ad esempio durante un `git push`. Sono quindi due aspetti distinti: uno identifica l'autore del commit, l'altro autorizza l'accesso al repository remoto.

## 5. Perché eseguire `git status` sia prima sia dopo `git add`?

Perché `git status` permette di verificare lo stato del working tree e della staging area. Prima di `git add` permette di vedere quali file sono modificati o non tracciati. Dopo `git add` permette invece di controllare quali modifiche sono state effettivamente inserite nella staging area e quindi saranno incluse nel prossimo commit.