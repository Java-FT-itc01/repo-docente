# ADDENDUM AL LAB00
## Checklist completa di readiness del setup

Questo addendum integra il file **LAB00 - Setup ambiente, runtime Java, monorepo, GitHub Classroom ed evidenze** e aggiunge una verifica operativa più rigorosa.

L'obiettivo non è solo controllare che "Git", "VS Code" e "Java" siano installati, ma verificare che l'ambiente sia davvero **pronto all'uso** per i laboratori successivi.

---

# Parte 1 - Principio fondamentale: installato non significa pronto

Un software può essere installato ma non essere utilizzabile correttamente nel corso.

Esempi tipici:

- `java` funziona ma `javac` no;
- Git è installato ma non ha nome ed email configurati;
- VS Code è installato ma non riconosce il runtime Java corretto;
- lo studente entra in GitHub ma non ha verificato l'email;
- il repository Classroom viene accettato ma non viene clonato correttamente;
- il push fallisce perché l'autenticazione GitHub non è stata completata.

Per questo motivo, prima di iniziare davvero il corso, ogni partecipante deve superare una **checklist di readiness**.

---

# Parte 2 - Checklist di readiness per i partecipanti

## 1. Checklist account GitHub

Il partecipante deve verificare di avere:

- un account GitHub personale funzionante;
- accesso riuscito via browser;
- email dell'account verificata;
- username GitHub annotato nell'evidenza;
- link di invito GitHub Classroom ricevuto.

### Verifica pratica

Aprire GitHub dal browser e controllare:

- accesso riuscito;
- nessuna richiesta pendente di verifica account;
- nessun dubbio su quale account sia attivo.

### Esito atteso

Il partecipante sa con certezza:

- quale account GitHub userà;
- quale email è associata;
- quale username userà durante tutto il corso.

---

## 2. Checklist Git locale

Non basta che Git sia installato. Deve essere anche configurato.

### Comandi da eseguire

```bash
git --version
```

```bash
git config --global user.name
```

```bash
git config --global user.email
```

```bash
git config --list --show-origin
```

### Se nome ed email non sono configurati

Configurare subito:

```bash
git config --global user.name "Nome Cognome"
```

```bash
git config --global user.email "email@example.com"
```

### Verifica finale

Eseguire di nuovo:

```bash
git config --global user.name
```

```bash
git config --global user.email
```

### Esito atteso

- Git installato;
- nome configurato;
- email configurata;
- configurazione globale visibile.

### Nota didattica importante

Il nome e l'email Git finiscono nei commit. Se non vengono configurati bene all'inizio, i repository del corso diventano poco leggibili e il tracciamento del lavoro peggiora.

---

## 3. Checklist Java da terminale

### Comandi da eseguire

```bash
java -version
```

```bash
javac -version
```

```bash
where java
```

```bash
where javac
```

### Cosa verificare

- `java` disponibile;
- `javac` disponibile;
- versione coerente con JDK 21 LTS;
- nessuna ambiguità su installazioni multiple indesiderate.

### Esito atteso

Il partecipante riesce a dimostrare che il compilatore Java è realmente disponibile, non solo il runtime.

---

## 4. Checklist VS Code

### Verifiche da fare

- VS Code si apre correttamente;
- la cartella del repository si apre come cartella, non come singolo file;
- il terminale integrato si avvia;
- le estensioni richieste sono installate;
- VS Code riconosce il runtime Java corretto.

### Controlli operativi

Verificare in **Extensions** la presenza di:

- Extension Pack for Java
- Markdown All in One

Aprire la Command Palette:

```text
Java: Configure Java Runtime
```

### Esito atteso

- il JDK corretto è visibile;
- VS Code non segnala errori evidenti sul runtime;
- il partecipante sa aprire il terminale integrato e usarlo.

---

## 5. Checklist filesystem e cartella di lavoro

### Il partecipante deve verificare che

- esista una cartella stabile di lavoro, per esempio `C:\Corsi\JavaBackend`;
- il repository del corso non sia sul Desktop o in una cartella casuale;
- sappia entrare nel repository da terminale con `cd`;
- sappia aprire la cartella corretta con `code .`.

### Esito atteso

Il partecipante non lavora in percorsi improvvisati.

---

## 6. Checklist GitHub Classroom lato partecipante

### Verifica minima

Il partecipante deve riuscire a:

1. aprire il link di invito;
2. accettare l'assegnazione;
3. attendere la creazione del repository personale;
4. aprire il repository remoto nel browser;
5. clonarlo in locale;
6. aprirlo in VS Code.

### Comandi minimi

```bash
git clone URL_DEL_REPOSITORY
```

```bash
cd NOME_REPOSITORY
```

```bash
code .
```

### Esito atteso

Il partecipante vede:

- il repository remoto nel browser;
- il repository locale sul PC;
- la stessa struttura aperta in VS Code.

---

## 7. Checklist autenticazione GitHub per push

Molti setup sembrano corretti fino al primo `git push`, poi crollano con la grazia di un carrello del supermercato con una ruota bloccata.

### Verifica minima

Dopo una modifica banale a un file di test:

```bash
git status
```

```bash
git add .
```

```bash
git commit -m "Test setup iniziale"
```

```bash
git push
```

### Esito atteso

Il push va a buon fine.

Se fallisce, il setup non è pronto.

---

## 8. Checklist prova completa finale del partecipante

Il setup del partecipante è da considerarsi pronto solo se riesce a completare tutta questa sequenza senza assistenza sostanziale:

1. aprire GitHub con l'account corretto;
2. accettare un assignment Classroom;
3. clonare il repository;
4. aprire la cartella in VS Code;
5. verificare Java e Git da terminale;
6. compilare un file Java;
7. eseguire il programma;
8. aggiornare l'evidenza markdown;
9. fare commit;
10. fare push.

---

# Parte 3 - Checklist evidenza da far compilare ai partecipanti

Puoi aggiungere questa sezione al file `evidence_lab00.md`.

```md
## Checklist readiness setup

### Account GitHub
- Username GitHub:
- Email GitHub verificata: sì/no
- Link Classroom ricevuto: sì/no
- Assignment accettato: sì/no
- Repository remoto visibile nel browser: sì/no

### Git locale
- Output di `git --version`:
- Output di `git config --global user.name`:
- Output di `git config --global user.email`:
- Configurazione Git completata: sì/no

### Java
- Output di `java -version`:
- Output di `javac -version`:
- Output di `where java`:
- Output di `where javac`:
- JDK corretto disponibile: sì/no

### VS Code
- VS Code avviato: sì/no
- Extension Pack for Java installato: sì/no
- Markdown All in One installato: sì/no
- Runtime Java riconosciuto in VS Code: sì/no

### Repository locale
- Percorso locale del repository:
- Comando `git clone` eseguito con successo: sì/no
- Comando `code .` eseguito con successo: sì/no

### Test finale
- Compilazione Java riuscita: sì/no
- Esecuzione Java riuscita: sì/no
- Commit riuscito: sì/no
- Push riuscito: sì/no

### Problemi residui
- Problema 1:
- Problema 2:
- Problema 3:
```

---

