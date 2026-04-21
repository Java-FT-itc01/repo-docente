# LAB00 - Setup ambiente, runtime Java, monorepo, GitHub Classroom ed evidenze

## Titolo del laboratorio
Setup operativo del corso Java Backend: VS Code, JDK, monorepo locale, GitHub Classroom, struttura standard dei laboratori ed evidenze.

## Durata stimata
8 ore totali

- **Sessione 1**: 4 ore - verifica ambiente, installazione del software mancante, concetti fondamentali, prime prove operative
- **Sessione 2**: 4 ore - clonazione repository, organizzazione del lavoro, primo programma Java, produzione evidenze, commit e push

---

## Obiettivi del laboratorio
Al termine di questo laboratorio dovrai essere in grado di:

1. distinguere correttamente **JDK**, **JRE** e **JVM**;
2. comprendere il flusso **file sorgente `.java` -> compilazione -> bytecode `.class` -> esecuzione sulla JVM**;
3. usare **VS Code** come ambiente standard del corso;
4. verificare e configurare il **runtime Java** in VS Code;
5. clonare in locale il repository assegnato tramite **GitHub Classroom**;
6. riconoscere e rispettare la struttura del **monorepo** del corso;
7. creare, modificare e salvare file nelle cartelle corrette;
8. compilare ed eseguire un semplice programma Java da terminale;
9. produrre e consegnare una prima **evidenza** in formato Markdown;
10. eseguire **commit** e **push** verso il repository remoto.

---

## Prerequisiti
Per svolgere questo laboratorio devi avere a disposizione:

- un PC Windows 10 o Windows 11;
- accesso a Internet;
- accesso all'account GitHub che userai durante il corso;
- il link di invito GitHub Classroom del corso;
- permessi per installare software oppure software già installato.

---

## Software richiesto per Lab00
Per **Lab00** verificherai la presenza di:

- **Visual Studio Code**
- **JDK 21 LTS**
- **Git for Windows**
- un browser aggiornato

## Software non richiesto in Lab00
### Docker Desktop
**Docker non è richiesto in questo laboratorio.**

Nella roadmap attuale del corso, i primi moduli lavorano con:

- Java da terminale
- VS Code
- GitHub Classroom
- SQL/MariaDB
- Spring Boot eseguito in locale

Quindi **non è necessario installare Docker adesso**. Se più avanti il corso introdurrà containerizzazione o build container, l'installazione verrà affrontata in un laboratorio dedicato.

### Regola pratica
Per non appesantire inutilmente il setup iniziale:

- installa subito solo ciò che serve davvero per iniziare;
- installa Docker più avanti solo se un laboratorio lo richiederà esplicitamente.

## Estensioni VS Code richieste
In VS Code dovrai installare almeno:

- **Extension Pack for Java**
- **Markdown All in One**

Estensione facoltativa ma utile:

- **GitHub Pull Requests and Issues**

---

# Parte 1 - Concetti da capire prima di iniziare

## 1. Che cosa stai preparando oggi
Oggi non stai ancora studiando Java nel senso degli algoritmi o dell'OOP. Oggi stai costruendo il **modo corretto di lavorare** per tutto il corso.

Se il setup è ordinato:

- i file sono al posto giusto;
- i package sono coerenti;
- le evidenze si trovano subito;
- i comandi funzionano;
- il lavoro è verificabile;
- puoi riprendere il laboratorio anche nei giorni successivi.

Se il setup è disordinato, ogni laboratorio successivo sarà più faticoso del necessario.

---

## 2. JDK, JRE e JVM

### JVM
La **JVM** è la **Java Virtual Machine**.

È il componente che esegue il **bytecode Java**.

Quando compili un file `.java`, non ottieni direttamente un file eseguibile nativo: ottieni un file `.class` contenente bytecode, che verrà eseguito dalla JVM.

### JRE
Il **JRE** è il **Java Runtime Environment**.

Contiene ciò che serve per **eseguire** programmi Java:

- la JVM;
- librerie runtime essenziali.

### JDK
Il **JDK** è il **Java Development Kit**.

Contiene ciò che serve per **sviluppare** programmi Java:

- compilatore `javac`;
- strumenti di sviluppo;
- debugger;
- runtime necessario.

### Regola pratica del corso
Per il corso userai il **JDK**, non un semplice runtime.

Perché non devi solo eseguire applicazioni Java: devi **scriverle, compilarle, correggerle e farle evolvere**.

---

## 3. Flusso di lavoro di un semplice programma Java
Il flusso minimo è questo:

1. scrivi un file sorgente, per esempio `Main.java`;
2. compili con `javac`;
3. ottieni un file `.class`;
4. esegui il programma con `java`.

### Esempio logico
File sorgente:

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Ciao Java");
    }
}
```

Compilazione:

```bash
javac Main.java
```

Esecuzione:

```bash
java Main
```

Output atteso:

```text
Ciao Java
```

---

## 4. VS Code nel corso
Nel corso **VS Code** sarà l'ambiente standard.

Lo userai per:

- aprire il monorepo del corso;
- leggere README e consegne;
- creare file `.java`, `.md`, `.json`, `.sql`;
- usare il terminale integrato;
- verificare il runtime Java;
- lavorare con Git;
- gestire i laboratori in modo coerente con GitHub Classroom.

### Idea chiave
Il riferimento principale non sarà il progetto interno dell'IDE, ma il **repository sul filesystem reale**.

Questo è importante perché:

- avrai una struttura stabile;
- potrai usare Git correttamente;
- potrai lavorare anche da terminale;
- i laboratori saranno riproducibili.

---

# Parte 2 - Verifica software e installazione guidata se manca qualcosa

## 5. Risorse ufficiali da usare
Usa sempre siti ufficiali per scaricare software o approfondire impostazioni importanti.

### Download software
- **Visual Studio Code**: <https://code.visualstudio.com/download>
- **Eclipse Temurin JDK**: <https://adoptium.net/temurin/releases>
- **Git for Windows**: <https://git-scm.com/install/windows>

### Documentazione ufficiale utile
- **Getting Started with Java in VS Code**: <https://code.visualstudio.com/docs/java/java-tutorial>
- **Managing Java Projects in VS Code**: <https://code.visualstudio.com/docs/java/java-project>

---

## 6. Procedura di verifica iniziale
Prima di installare qualcosa, verifica se il software richiesto è già presente.

### Comandi di verifica
Apri **Prompt dei comandi** oppure **PowerShell** ed esegui:

```bash
java -version
```

```bash
javac -version
```

```bash
git --version
```

### Che cosa significa il risultato
- se `java -version` funziona e `javac -version` funziona, il JDK è probabilmente installato e disponibile;
- se `java -version` funziona ma `javac -version` no, hai solo il runtime oppure PATH incompleto;
- se nessuno dei due funziona, Java non è installato oppure non è configurato;
- se `git --version` non funziona, Git non è installato oppure non è nel PATH.

VS Code si verifica semplicemente provando ad avviarlo dal menu Start oppure cercando `code` nella ricerca di Windows.

---

## 7. Installazione di Visual Studio Code se manca
Segui questa sezione **solo se VS Code non è installato**.

### Passo 1
Apri il browser e vai alla pagina ufficiale di download:

<https://code.visualstudio.com/download>

### Passo 2
Scarica la versione per **Windows** adatta al tuo sistema.

### Passo 3
Esegui l'installer scaricato.

### Passo 4
Durante l'installazione:

- lascia attive le opzioni consigliate;
- abilita, se disponibile, l'aggiunta al **PATH**;
- abilita l'apertura con **Open with Code** se proposta.

### Passo 5
Completa l'installazione e avvia VS Code.

### Passo 6
Verifica che l'applicazione si apra correttamente.

---

## 8. Installazione del JDK se manca
Segui questa sezione **solo se `java -version` o `javac -version` non funzionano correttamente**.

### Passo 1
Apri la pagina ufficiale Temurin:

<https://adoptium.net/temurin/releases>

### Passo 2
Seleziona:

- **Versione**: **21 LTS**
- **Package type**: **JDK**
- **Operating System**: **Windows**
- **Architecture**: quella corretta per il tuo PC, in genere **x64**

### Passo 3
Scarica l'installer `.msi` o il pacchetto consigliato per Windows.

### Passo 4
Esegui l'installer.

### Passo 5
Durante l'installazione, se disponibile:

- abilita l'aggiunta del JDK al **PATH**;
- abilita eventuali opzioni per rendere Java disponibile da terminale.

### Passo 6
Chiudi e riapri **Prompt dei comandi** oppure **PowerShell**.

### Passo 7
Verifica di nuovo:

```bash
java -version
```

```bash
javac -version
```

### Risultato atteso
Entrambi i comandi devono restituire una versione valida del JDK.

---

## 9. Installazione di Git se manca
Segui questa sezione **solo se `git --version` non funziona**.

### Passo 1
Apri la pagina ufficiale di installazione di Git per Windows:

<https://git-scm.com/install/windows>

### Passo 2
Scarica l'installer più recente per Windows.

### Passo 3
Esegui l'installer.

### Passo 4
Durante l'installazione puoi lasciare le opzioni predefinite, salvo indicazioni diverse nei materiali del corso.

### Passo 5
Chiudi e riapri il terminale.

### Passo 6
Verifica:

```bash
git --version
```

### Risultato atteso
Il comando deve restituire la versione installata.

---

## 10. Installazione estensioni VS Code
Una volta aperto VS Code:

### Passo 1
Apri il pannello **Extensions**.

### Passo 2
Cerca e installa:

- **Extension Pack for Java**
- **Markdown All in One**

### Passo 3
Attendi il completamento dell'installazione.

### Passo 4
Riavvia VS Code se richiesto.

---

## 11. Configurazione del runtime Java in VS Code
Questa operazione serve per controllare quale JDK sta usando VS Code.

### Passo 1
Apri VS Code.

### Passo 2
Apri la **Command Palette** con:

- `Ctrl + Shift + P`

### Passo 3
Scrivi:

```text
Java: Configure Java Runtime
```

### Passo 4
Apri la schermata di configurazione del runtime.

### Passo 5
Controlla che il JDK installato sia visibile.

### Passo 6
Se necessario, imposta come predefinito il JDK 21 LTS per le cartelle non gestite da Maven o Gradle.

### Approfondimento ufficiale
Per capire meglio questa schermata puoi consultare:

<https://code.visualstudio.com/docs/java/java-project>

---

# Parte 3 - Struttura di lavoro del corso

## 12. Struttura generale del monorepo
Il corso userà una struttura ordinata e ripetibile. Una struttura minima può essere simile a questa:

```text
java-backend-monorepo/
  README.md
  docs/
    standard/
      CONSEGNE.md
      EVIDENCE_TEMPLATE.md
  labs/
    lab00_setup/
      README.md
      docs/
        evidence_lab00.md
      src/
```

### Significato delle cartelle principali
- `README.md`: descrizione generale del repository;
- `docs/`: materiale di supporto e standard comuni;
- `labs/`: tutti i laboratori del corso;
- `src/`: codice sorgente del laboratorio corrente;
- `docs/evidence_*.md`: evidenze da consegnare.

---

## 13. Perché usare un monorepo
Il monorepo serve a:

- mantenere tutti i laboratori nello stesso contenitore logico;
- evitare decine di repository separati;
- rendere più semplice la consegna;
- mantenere una struttura coerente tra i laboratori;
- lavorare meglio con GitHub Classroom.

---

## 14. Regole operative fondamentali
Durante il corso dovrai rispettare alcune regole operative.

### Regola 1
Lavora sempre **dentro la cartella del repository**.

### Regola 2
Non salvare file Java sul Desktop o in cartelle scollegate dal laboratorio.

### Regola 3
Ogni laboratorio deve avere:

- il proprio `README.md`;
- la propria cartella `src/`;
- la propria cartella `docs/`;
- la propria evidenza `.md`.

### Regola 4
Usa nomi coerenti e prevedibili.

### Regola 5
Prima di fare commit, controlla quali file stai davvero inviando.

---

# Parte 4 - Attività guidate da svolgere

## 15. Accetta l'invito GitHub Classroom e clona il repository
### Passo 1
Apri il link di invito GitHub Classroom del corso.

### Passo 2
Accetta l'assegnazione.

### Passo 3
Attendi la creazione del repository personale.

### Passo 4
Copia l'URL del repository.

### Passo 5
Scegli sul tuo PC una cartella di lavoro stabile, per esempio:

```text
C:\Corsi\JavaBackend
```

### Passo 6
Apri **Prompt dei comandi**, **PowerShell** oppure il terminale di VS Code.

### Passo 7
Spostati nella cartella scelta.

Esempio:

```bash
cd C:\Corsi\JavaBackend
```

### Passo 8
Clona il repository:

```bash
git clone URL_DEL_TUO_REPOSITORY
```

### Passo 9
Entra nella cartella clonata:

```bash
cd NOME_REPOSITORY
```

---

## 16. Apri il repository in VS Code
### Passo 1
Dal terminale posizionato nella cartella del repository esegui:

```bash
code .
```

### Passo 2
Se il comando `code` non funziona, apri VS Code manualmente e usa:

- **File -> Open Folder**

### Passo 3
Seleziona la cartella del repository.

### Passo 4
Controlla nel pannello Explorer che la cartella del repository sia aperta correttamente.

---

## 17. Crea la struttura minima del Lab00 se non è già presente
Se la struttura è già presente nel repository, limitati a verificarla.

Se invece manca, crea almeno questa struttura:

```text
labs/
  lab00_setup/
    README.md
    docs/
      evidence_lab00.md
    src/
```

### Passo 1
Crea la cartella `labs` se non esiste.

### Passo 2
Crea la cartella `lab00_setup`.

### Passo 3
Dentro `lab00_setup` crea:

- `README.md`
- cartella `docs`
- cartella `src`

### Passo 4
Dentro `docs` crea il file:

- `evidence_lab00.md`

---

## 18. Crea ed esegui il primo programma Java
### Passo 1
Dentro `labs/lab00_setup/src/` crea il file:

```text
Main.java
```

### Passo 2
Inserisci questo contenuto:

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Ciao Java Backend");
    }
}
```

### Passo 3
Apri il terminale nella cartella:

```text
labs/lab00_setup/src
```

### Passo 4
Compila:

```bash
javac Main.java
```

### Passo 5
Verifica che sia stato creato il file:

```text
Main.class
```

### Passo 6
Esegui il programma:

```bash
java Main
```

### Output atteso
```text
Ciao Java Backend
```

---

## 19. Compila l'evidenza del laboratorio
Apri il file:

```text
labs/lab00_setup/docs/evidence_lab00.md
```

Inserisci almeno queste sezioni.

```md
# Evidenza LAB00

## Dati essenziali
- Nome e cognome:
- Data:
- Repository:

## Verifica software
- Output di `java -version`:
- Output di `javac -version`:
- Output di `git --version`:
- VS Code avviato correttamente: sì/no

## Runtime Java in VS Code
- Runtime individuato:
- Eventuali problemi riscontrati:
- Soluzione adottata:

## Primo programma Java
- File creato:
- Comando di compilazione usato:
- Comando di esecuzione usato:
- Output ottenuto:

## Struttura repository
- Percorso locale della cartella di lavoro:
- Cartelle create o verificate:

## Difficoltà incontrate
- Problema 1:
- Soluzione 1:
- Problema 2:
- Soluzione 2:
```

---

## 20. Esegui commit e push
### Passo 1
Verifica quali file sono cambiati:

```bash
git status
```

### Passo 2
Aggiungi i file:

```bash
git add .
```

### Passo 3
Crea il commit:

```bash
git commit -m "Completato LAB00 setup ambiente e primo programma Java"
```

### Passo 4
Esegui il push:

```bash
git push
```

### Passo 5
Controlla sul repository remoto che i file siano effettivamente presenti.

---

# Parte 5 - Criteri di completamento

Considera completato il laboratorio solo se:

- il software richiesto è installato e verificato;
- VS Code si apre correttamente;
- il runtime Java è riconosciuto in VS Code;
- il repository è stato clonato correttamente;
- la struttura minima del Lab00 è presente;
- `Main.java` è stato compilato ed eseguito;
- l'evidenza markdown è stata compilata;
- commit e push sono stati eseguiti con successo.

---

# Parte 6 - Errori comuni da evitare

## Errore 1
Installare solo il runtime e non il JDK.

### Effetto
`java` funziona, ma `javac` no.

---

## Errore 2
Lavorare fuori dal repository.

### Effetto
I file non finiscono nella struttura corretta e non vengono consegnati.

---

## Errore 3
Aprire solo il file invece della cartella del repository in VS Code.

### Effetto
La visione del progetto è incompleta e il lavoro risulta disordinato.

---

## Errore 4
Saltare la verifica del runtime Java in VS Code.

### Effetto
Il terminale può funzionare, ma VS Code può usare un runtime diverso o non configurato.

---

## Errore 5
Fare commit senza controllare i file inclusi.

### Effetto
Potresti inviare file sbagliati oppure dimenticare quelli necessari.

---

# Parte 7 - Conclusione

Con questo laboratorio hai preparato l'ambiente standard del corso.

Da questo momento in poi il tuo modo di lavorare dovrà essere sempre questo:

1. aprire il repository corretto;
2. lavorare nella cartella corretta del laboratorio;
3. scrivere codice e documentare l'evidenza;
4. verificare il risultato;
5. eseguire commit e push.

Se questa disciplina operativa diventa automatica, i laboratori successivi saranno molto più chiari e molto più gestibili.
