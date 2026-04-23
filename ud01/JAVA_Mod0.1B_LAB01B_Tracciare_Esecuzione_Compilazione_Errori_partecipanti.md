# LAB01B - Tracciare l'esecuzione, ricompilare, leggere output ed errori

## Titolo del laboratorio
Consolidare la comprensione dell'esecuzione di un programma Java leggendo il codice prima di eseguirlo, prevedendo l'output, osservando il comportamento del compilatore e distinguendo meglio gli errori.

## Durata stimata
3 ore totali

- **Sessione unica**: 3 ore - tracing, previsione output, ricompilazione, errori controllati

---

## Obiettivi del laboratorio
Al termine di questo laboratorio dovrai essere in grado di:

1. leggere un programma Java semplice e prevederne l'output;
2. spiegare l'ordine con cui vengono eseguite le istruzioni in `main`;
3. capire quando è necessario ricompilare dopo una modifica al file sorgente;
4. distinguere meglio errore di compilazione, errore runtime ed errore logico;
5. lavorare con più classi eseguibili nella stessa cartella `src`.

---

## Prerequisiti
Devi aver completato **Lab01**.

In particolare devi saper già:

- creare un file `.java`;
- compilare con `javac`;
- eseguire con `java`;
- riconoscere il ruolo generale di JDK, JRE e JVM.

---

## Software necessario
Per questo laboratorio userai:

- **VS Code**
- **JDK 21**
- **terminale integrato di VS Code**
- **Git** solo per la consegna finale

---

# Parte 1 - Preparazione della struttura del laboratorio

## 1. Crea la cartella del laboratorio
All'interno del monorepo del corso crea, se non esiste già, questa struttura:

```text
labs/
  lab01b_tracciare_esecuzione/
    README.md
    docs/
    src/
````

Dentro `src/` creerai i programmi Java.
Dentro `docs/` salverai le evidenze.

---

## 2. Apri il terminale nella cartella `src`

Spostati nella cartella del laboratorio:

```bash
cd labs/lab01b_tracciare_esecuzione/src
```

Controlla dove ti trovi e quali file sono presenti:

```bash
pwd
ls
```

Se stai usando PowerShell puoi usare anche:

```powershell
dir
```

---

# Parte 2 - Leggere il programma prima di eseguirlo

## 3. Prevedere l'output

Crea il file:

```text
OrdineIstruzioni.java
```

Inserisci questo contenuto:

```java
public class OrdineIstruzioni {
    public static void main(String[] args) {
        System.out.println("Riga 1");
        System.out.println("Riga 2");
        System.out.println("Riga 3");
    }
}
```

### Attività

Prima di compilare, scrivi nel file evidenza quale output ti aspetti.

Poi compila ed esegui:

```bash
javac OrdineIstruzioni.java
java OrdineIstruzioni
```

### Obiettivo

Capire che, in questo esempio, le istruzioni vengono eseguite nell'ordine in cui compaiono.

---

## 4. Cambiare l'ordine delle istruzioni

Modifica `OrdineIstruzioni.java` così:

```java
public class OrdineIstruzioni {
    public static void main(String[] args) {
        System.out.println("Riga 3");
        System.out.println("Riga 1");
        System.out.println("Riga 2");
    }
}
```

### Attività

1. prevedi il nuovo output;
2. ricompila;
3. riesegui il programma;
4. confronta output previsto e output reale.

---

# Parte 3 - Tracciare il valore delle variabili

## 5. Tracciare i valori passo passo

Crea il file:

```text
TracciaValori.java
```

Inserisci questo contenuto:

```java
public class TracciaValori {
    public static void main(String[] args) {
        int numero = 10;
        System.out.println("Valore iniziale: " + numero);

        numero = 15;
        System.out.println("Valore aggiornato: " + numero);

        numero = numero + 5;
        System.out.println("Valore finale: " + numero);
    }
}
```

Compila ed esegui:

```bash
javac TracciaValori.java
java TracciaValori
```

### Attività

Nel file evidenza descrivi passo passo che cosa succede:

1. quale valore ha `numero` all'inizio;
2. quale valore assume dopo l'assegnazione `numero = 15`;
3. quale valore assume dopo `numero = numero + 5`.

### Obiettivo

Capire che il valore di una variabile può cambiare durante l'esecuzione del programma.

---

## 6. Tabella di tracing

Nel file evidenza crea una piccola tabella come questa:

```text
Istruzione                              | Valore di numero
--------------------------------------- | ----------------
int numero = 10;                        | 10
numero = 15;                            | 15
numero = numero + 5;                    | 20
```

Questa attività serve a costruire il ragionamento passo passo, senza affidarsi al caso o alla memoria.

---

# Parte 4 - Ricompilazione consapevole

## 7. Modificare il sorgente senza ricompilare

Crea il file:

```text
Ricompilare.java
```

Inserisci questo contenuto:

```java
public class Ricompilare {
    public static void main(String[] args) {
        System.out.println("Versione 1");
    }
}
```

Compila ed esegui:

```bash
javac Ricompilare.java
java Ricompilare
```

Dovresti vedere:

```text
Versione 1
```

---

## 8. Cambia il sorgente

Ora modifica il file così:

```java
public class Ricompilare {
    public static void main(String[] args) {
        System.out.println("Versione 2");
    }
}
```

### Attività

Esegui **senza** ricompilare:

```bash
java Ricompilare
```

Osserva il risultato.

Poi esegui:

```bash
javac Ricompilare.java
java Ricompilare
```

### Domande guida

* Perché senza ricompilare non vedi subito il cambiamento?
* Qual è la differenza tra il file `.java` modificato e il file `.class` già esistente?

### Obiettivo

Capire che modificare il sorgente non basta: per aggiornare il programma eseguibile bisogna ricompilare.

---

# Parte 5 - Più classi eseguibili nella stessa cartella

## 9. Una cartella, più programmi

Crea il file:

```text
SalutoAula.java
```

Inserisci questo contenuto:

```java
public class SalutoAula {
    public static void main(String[] args) {
        System.out.println("Benvenuti nel laboratorio Java");
    }
}
```

Compila ed esegui:

```bash
javac SalutoAula.java
java SalutoAula
```

### Attività

Compila ed esegui sia `Ricompilare.java` sia `SalutoAula.java`, uno dopo l'altro.

### Obiettivo

Capire che nella stessa cartella `src` possono convivere più classi eseguibili, ciascuna con il proprio `main`.

---

# Parte 6 - Errori controllati

## 10. Errore runtime semplice

Crea il file:

```text
DivisioneRuntime.java
```

Inserisci questo contenuto:

```java
public class DivisioneRuntime {
    public static void main(String[] args) {
        int a = 10;
        int b = 0;
        int risultato = a / b;

        System.out.println(risultato);
    }
}
```

Compila:

```bash
javac DivisioneRuntime.java
```

Esegui:

```bash
java DivisioneRuntime
```

### Osserva

Il programma compila, ma fallisce in esecuzione.

### Attività

Nel file evidenza spiega:

* perché questo non è un errore di compilazione;
* in quale momento compare il problema;
* perché appartiene alla categoria degli errori runtime.

---

## 11. Errore logico da individuare

Crea il file:

```text
MessaggioConfuso.java
```

Inserisci questo contenuto:

```java
public class MessaggioConfuso {
    public static void main(String[] args) {
        System.out.println("Inizio programma");
        System.out.println("Fine programma");
        System.out.println("Sto ancora lavorando");
    }
}
```

Compila ed esegui:

```bash
javac MessaggioConfuso.java
java MessaggioConfuso
```

### Attività

Spiega perché il codice:

* compila correttamente;
* si esegue correttamente;
* ma comunica un messaggio poco coerente dal punto di vista logico.

### Obiettivo

Capire che un programma può funzionare tecnicamente e restare comunque sbagliato dal punto di vista del significato.

---

## 12. Errore di compilazione rapido

Crea il file:

```text
ErroreCompilazione.java
```

Inserisci questo contenuto con un errore volontario:

```java
public class ErroreCompilazione {
    public static void main(String[] args) {
        System.out.println("Test errore")
    }
}
```

Compila:

```bash
javac ErroreCompilazione.java
```

Leggi il messaggio del compilatore, correggi il file e ricompila.

### Obiettivo

Allenarti a riconoscere e correggere rapidamente un errore di sintassi semplice.

---

# Parte 7 - Attività autonome

## 13. Attività A - Previsione prima dell'esecuzione

Crea il file:

```text
PrevisioneOutput.java
```

Scrivi un programma con tre `System.out.println(...)` in ordine libero.

Prima di eseguirlo:

1. scrivi l'output previsto;
2. compila;
3. esegui;
4. verifica se avevi previsto correttamente.

---

## 14. Attività B - Variabile aggiornata

Crea il file:

```text
AggiornaContatore.java
```

Il programma deve:

* dichiarare una variabile intera;
* stamparne il valore iniziale;
* modificarla almeno due volte;
* stampare ogni aggiornamento.

---

## 15. Attività C - Spiegazione con parole tue

Nel file evidenza spiega con parole tue:

1. perché serve ricompilare dopo aver modificato un file `.java`;
2. che differenza c'è tra output previsto e output reale;
3. perché un errore runtime non impedisce sempre la compilazione.

---

# Parte 8 - Evidenze richieste

## 16. File di evidenza

Crea il file:

```text
labs/lab01b_tracciare_esecuzione/docs/evidence_lab01b.md
```

Usa questa struttura:

```markdown
# Evidence Lab01B

## 1. Programmi creati
- OrdineIstruzioni.java
- TracciaValori.java
- Ricompilare.java
- SalutoAula.java
- DivisioneRuntime.java
- MessaggioConfuso.java
- ErroreCompilazione.java
- PrevisioneOutput.java
- AggiornaContatore.java

## 2. Output previsti e output reali
Confronta ciò che avevi previsto con ciò che hai osservato.

## 3. Ricompilazione
Spiega che cosa succede se modifichi un file `.java` ma non esegui di nuovo `javac`.

## 4. Tipi di errore
Riporta un esempio di:
- errore di compilazione
- errore runtime
- errore logico

## 5. Tabella di tracing
Inserisci almeno una tabella di tracing relativa a una variabile.

## 6. Conclusioni
Scrivi che cosa hai compreso meglio rispetto al Lab01.
```

---

## 17. File da consegnare nel repository

Al termine del laboratorio, nel repository devono essere presenti almeno:

```text
labs/lab01b_tracciare_esecuzione/
  docs/
    evidence_lab01b.md
  src/
    OrdineIstruzioni.java
    TracciaValori.java
    Ricompilare.java
    SalutoAula.java
    DivisioneRuntime.java
    MessaggioConfuso.java
    ErroreCompilazione.java
    PrevisioneOutput.java
    AggiornaContatore.java
```

### Importante

I file `.class` non devono essere consegnati nel repository.

---

# Parte 9 - Errori comuni da evitare

## 18. Errori frequenti

### 1. Modificare il sorgente e dimenticare `javac`

È uno degli errori più comuni nei primi laboratori.

### 2. Confondere il file `.java` con il file `.class`

Il sorgente si modifica. Il `.class` viene generato dalla compilazione.

### 3. Prevedere l'output senza leggere davvero il codice

Il tracing richiede attenzione all'ordine delle istruzioni.

### 4. Pensare che “compila” significhi sempre “è corretto”

Un programma può compilare ed essere logicamente sbagliato.

### 5. Consegnare anche i file compilati

Nel repository vanno consegnati solo i sorgenti e le evidenze.

---

# Parte 10 - Checklist finale

Prima di concludere verifica tutto questo:

* [ ] so prevedere l'output di un semplice programma Java;
* [ ] so spiegare l'ordine di esecuzione delle istruzioni in `main`;
* [ ] so spiegare perché è necessario ricompilare;
* [ ] so distinguere sorgente `.java` e bytecode `.class`;
* [ ] so riconoscere un errore di compilazione;
* [ ] so riconoscere un errore runtime;
* [ ] so riconoscere un errore logico;
* [ ] ho creato il file `evidence_lab01b.md`;
* [ ] ho salvato i file nelle cartelle corrette;
* [ ] ho eseguito `git add`, `git commit` e `git push`.

---

# Parte 11 - Consegna finale

Quando hai completato il laboratorio:

1. controlla i file creati;
2. aggiorna il file `evidence_lab01b.md`;
3. esegui i comandi Git dalla radice del repository.

```bash
git add .
git commit -m "Completato Lab01B - tracing, output, ricompilazione, errori"
git push
```

---

# Parte 12 - Approfondimenti consigliati

Per consolidare questo laboratorio puoi rileggere:

* la documentazione base Oracle su struttura ed esecuzione di un programma Java;
* gli appunti di Lab01 su compilazione, JVM, JRE e JDK.

L'obiettivo di questo laboratorio non è aggiungere nuova sintassi complessa.
L'obiettivo è **capire meglio che cosa accade quando un programma viene letto, compilato, eseguito e corretto**.

```
```
