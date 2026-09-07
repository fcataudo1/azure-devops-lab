# Nota operativa — UD01

La cartella di lavoro si trova nel filesystem Linux di WSL 2 ed è stata aperta con Visual Studio Code tramite l'estensione WSL.

Il controllo che ha dimostrato il corretto contesto di esecuzione è:

```bash
pwd
```

L'output indicava un percorso interno alla home Linux e non un percorso `/mnt/c`.
Il comando `pwd` è stato particolarmente utile perché permette di verificare rapidamente la posizione corrente e confermare che si sta lavorando nel filesystem Linux di WSL.
