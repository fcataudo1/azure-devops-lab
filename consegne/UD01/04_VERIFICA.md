# Verifica — Preparazione dell'ambiente e metodo di lavoro

## Parte A — Scelte operative

### 1. Perché `git --version` può restituire due risultati diversi in PowerShell e in Ubuntu?

**Risposta: B**

Windows e Ubuntu possono possedere installazioni distinte di Git. Il comando viene eseguito nell'ambiente corrente e quindi può mostrare versioni differenti.

### 2. Quale comando mostra se la distribuzione Ubuntu sta usando WSL 1 o WSL 2?

**Risposta: C**

Il comando:

```bash
wsl --list --verbose
```

mostra la distribuzione e la versione di WSL utilizzata nella colonna `VERSION`. Ci si aspetta di osservare `2` per Ubuntu.

### 3. Perché il progetto viene conservato in `~/workspace`?

**Risposta: C**

I progetti vengono conservati nel filesystem Linux di WSL, più adatto al successivo workflow con strumenti Linux, Docker e bind mount.

### 4. Git è presente e la sua versione è compatibile. Qual è l'azione corretta?

**Risposta: C**

È corretto verificarne il funzionamento e proseguire senza reinstallazione. Non è necessario installare nuovamente uno strumento già presente e compatibile.

### 5. Dopo `git commit` la pagina GitHub non mostra la modifica. Quale spiegazione è più probabile?

**Risposta: A**

Il commit esiste localmente, ma non è ancora stato eseguito `git push`. Il commit registra la modifica nella cronologia locale, mentre `git push` la invia al repository remoto.

### 6. Qual è il controllo più diretto per capire quali file entreranno nel prossimo commit?

**Risposta: A**

Il comando:

```bash
git status
```

eseguito dopo `git add` mostra i file presenti nella staging area e quindi preparati per il prossimo commit.

### 7. Durante `az login --use-device-code` il terminale mostra un codice temporaneo. Che cosa devi fare?

**Risposta: C**

Il codice deve essere utilizzato nella pagina di autenticazione indicata da Azure e non deve essere conservato o pubblicato nel repository.

### 8. Quale indizio dimostra meglio che VS Code sta operando dentro Ubuntu?

**Risposta: B**

L'indicatore `WSL: Ubuntu` insieme a un terminale che mostra un percorso Linux conferma che VS Code sta operando nell'ambiente Ubuntu tramite WSL.

### 9. Perché il docente deve essere aggiunto come collaboratore al repository personale pubblico?

**Risposta: C**

Per partecipare alle attività di scrittura, revisione e collaborazione previste dal percorso.

---

## Parte B — Risposte brevi

### 10. Spiega in non più di quattro righe la differenza fra Git e GitHub.

Git è il sistema di controllo di versione utilizzato localmente per gestire modifiche, staging e commit.
GitHub è un servizio online che ospita repository Git remoti.
Git permette quindi di gestire la cronologia del progetto, mentre GitHub permette di pubblicarla e collaborare tramite il repository remoto.

### 11. Scrivi la sequenza minima di comandi che useresti per osservare le modifiche, preparare un singolo file, creare un commit e inviarlo al remote.

```bash
git status
git add nome-file
git commit -m "Messaggio descrittivo"
git push
```

`git status` permette di osservare le modifiche, `git add` prepara il file, `git commit` registra la modifica localmente e `git push` la invia al repository remoto.

### 12. Si propone di eseguire immediatamente `wsl --update` su tutte le postazioni. Quali due controlli devono precedere la decisione?

Prima bisogna verificare la versione attuale di WSL con:

```powershell
wsl --version
```

e controllare la distribuzione e la versione WSL utilizzata con:

```powershell
wsl --list --verbose
```

Ci si aspetta di verificare che la versione installata sia già adeguata e che Ubuntu utilizzi WSL 2. Solo dopo si valuta se l'aggiornamento è realmente necessario.

### 13. Il comando `code .` apre VS Code, ma il terminale integrato mostra un percorso `C:\Users\...`. Quale problema sospetti e quale verifica esegui?

Sospetto che VS Code sia stato aperto nel contesto Windows invece che tramite WSL. Verificherei il terminale con:

```bash
pwd
```

e controllerei che il percorso restituito sia nel filesystem Linux, ad esempio sotto `/home/...`, e non sotto `/mnt/c` o `C:\Users\...`.

### 14. Hai pubblicato per errore un vero token in un commit e poi hai cancellato la riga con un secondo commit. Perché il problema non è risolto?

Il token rimane presente nella cronologia Git del repository e può essere recuperato dal commit precedente. È quindi necessario considerarlo compromesso, revocarlo o rigenerarlo e, se necessario, intervenire anche sulla cronologia del repository.