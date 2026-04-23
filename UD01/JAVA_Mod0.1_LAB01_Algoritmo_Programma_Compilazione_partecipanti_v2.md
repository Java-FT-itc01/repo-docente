# LAB01 - Algoritmo, programma, compilazione ed esecuzione in Java

## Titolo del laboratorio
Comprendere che cosa sono un algoritmo e un programma, riconoscere il ruolo di compilatore, interprete, JDK, JRE e JVM, e compilare ed eseguire i primi semplici programmi Java da terminale.

## Durata stimata
5 ore totali

- **Sessione 1**: 2,5 ore - concetti fondamentali, primi esempi, struttura del programma Java
- **Sessione 2**: 2,5 ore - compilazione, esecuzione, errori iniziali, evidenze e consegna

---

## Obiettivi del laboratorio
Al termine di questo laboratorio dovrai essere in grado di:

1. spiegare con parole tue che cosa sono un **algoritmo** e un **programma**;
2. distinguere correttamente **compilatore**, **interprete**, **JDK**, **JRE** e **JVM**;
3. descrivere il flusso **file sorgente `.java` -> compilazione -> file `.class` -> esecuzione**;
4. scrivere un programma Java minimo con il metodo `main`;
5. compilare un file Java da terminale con `javac`;
6. eseguire un programma Java da terminale con `java`;
7. riconoscere la differenza tra **errore di sintassi**, **errore a runtime** ed **errore logico**;
8. produrre un'evidenza chiara del lavoro svolto.

---

## Prerequisiti
Prima di iniziare questo laboratorio assicurati di avere completato correttamente **Lab00**.

In particolare devi avere:

- **VS Code** installato;
- **JDK 21 LTS** installato e funzionante;
- **Git** installato;
- il monorepo del corso già clonato in locale;
- il terminale di VS Code funzionante.

---

## Software necessario
Per questo laboratorio userai:

- **VS Code**
- **JDK 21**
- **terminale integrato di VS Code**
- **Git** solo per la consegna finale

### Verifica rapida del JDK
Apri il terminale integrato di VS Code ed esegui:

```bash
java -version
javac -version
````

Se i comandi non funzionano, torna a **Lab00** e completa la verifica o installazione del runtime Java.

---

# Parte 1 - Concetti fondamentali

## 1. Che cos'è un algoritmo

Un **algoritmo** è una sequenza finita, ordinata e non ambigua di passi che permette di risolvere un problema.

### Esempio non informatico

Preparare un tè può essere descritto come algoritmo:

1. riempi un pentolino con acqua;
2. porta l'acqua a ebollizione;
3. versa l'acqua nella tazza;
4. inserisci la bustina di tè;
5. attendi qualche minuto;
6. rimuovi la bustina.

Se i passi sono chiari e in ordine, l'algoritmo è eseguibile.

### Esempio informatico

Calcolare la somma di due numeri può essere descritto così:

1. leggi il primo numero;
2. leggi il secondo numero;
3. somma i due numeri;
4. visualizza il risultato.

Questa è già una forma elementare di algoritmo.

---

## 2. Che cos'è un programma

Un **programma** è la traduzione di uno o più algoritmi in un linguaggio comprensibile al computer attraverso strumenti software adeguati.

### Idea chiave

* L'**algoritmo** è la logica della soluzione.
* Il **programma** è la realizzazione concreta di quella logica in un linguaggio di programmazione.

Un algoritmo può esistere anche senza computer.
Un programma no.

---

## 3. Compilatore e interprete

### Compilatore

Un **compilatore** traduce il programma sorgente in una forma eseguibile o intermedia prima dell'esecuzione.

Nel caso di Java, il compilatore è:

```bash
javac
```

Il compilatore legge un file `.java` e produce un file `.class`.

### Interprete

Un **interprete** esegue il programma leggendo e traducendo le istruzioni durante l'esecuzione.

### Java: caso particolare

Java non si lascia descrivere bene con la formula scolastica “solo compilato” o “solo interpretato”.

Il flusso corretto è questo:

1. scrivi il file sorgente `.java`;
2. il compilatore `javac` produce il bytecode `.class`;
3. la **JVM** esegue il bytecode.

Quindi Java usa una **compilazione verso bytecode** e una **esecuzione tramite macchina virtuale**.

---

## 4. JDK, JRE e JVM

### JVM

La **JVM** è la Java Virtual Machine.

È il componente che esegue il bytecode Java.

### JRE

Il **JRE** è il Java Runtime Environment.

Contiene la JVM e le librerie necessarie per eseguire programmi Java.

### JDK

Il **JDK** è il Java Development Kit.

Contiene tutto ciò che serve per sviluppare:

* compilatore `javac`
* strumenti di sviluppo
* debugger
* componenti runtime

### Regola pratica

Per il corso ti serve il **JDK**, perché devi:

* scrivere codice;
* compilarlo;
* eseguirlo;
* correggerlo.

---

## 5. Il metodo `main`

Ogni programma Java eseguibile da terminale ha bisogno di un punto di ingresso.

Quel punto di ingresso, nei programmi semplici che svilupperai all'inizio del corso, è il metodo:

```java
public static void main(String[] args)
```

### Significato pratico

Per adesso ti basta sapere questo:

* `main` è il primo metodo che viene eseguito;
* senza `main`, un programma Java semplice non parte;
* più avanti capirai nel dettaglio il significato di ogni parola della firma del metodo.

---

## 6. Struttura minima di un programma Java

Ecco un programma Java minimo:

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Ciao Java");
    }
}
```

### Elementi da osservare

* `public class Main` definisce una classe chiamata `Main`;
* il file deve chiamarsi **Main.java**;
* il metodo `main` è il punto di ingresso;
* `System.out.println(...)` stampa un messaggio sul terminale.

---

## 7. Tipi di errore

Durante questo laboratorio inizierai a distinguere tre famiglie di errori.

### Errore di sintassi o compilazione

Il codice non rispetta le regole del linguaggio.

Esempio:

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Ciao")
    }
}
```

Manca `;`.
Il compilatore segnalerà un errore.

### Errore a runtime

Il programma compila, ma si rompe durante l'esecuzione.

Esempio tipico:

* divisione per zero in certi casi;
* accesso errato a un indice.

### Errore logico

Il programma compila ed esegue, ma produce un risultato sbagliato.

Esempio:

* dovevi fare una somma e hai scritto una sottrazione.

---

# Parte 2 - Preparazione della cartella del laboratorio

## 8. Posizionati nella cartella corretta del monorepo

Apri VS Code sulla radice del monorepo del corso.

Crea, se non esiste già, questa struttura:

```text
labs/
  lab01_algoritmo_programma_compilazione/
    README.md
    docs/
    src/
```

### File da creare in questo laboratorio

Dentro `src/` creerai i file Java.
Dentro `docs/` salverai le evidenze.

---

## 9. Crea il file principale

All'interno di:

```text
labs/lab01_algoritmo_programma_compilazione/src/
```

crea il file:

```text
Main.java
```

Inserisci questo contenuto:

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Ciao Java");
    }
}
```

Salva il file.

---

# Parte 3 - Compilazione ed esecuzione

## 10. Apri il terminale nella cartella `src`

Nel terminale integrato di VS Code spostati nella cartella del file:

```bash
cd labs/lab01_algoritmo_programma_compilazione/src
```

Verifica la presenza del file:

```bash
ls
```

Se stai usando PowerShell puoi usare anche:

```powershell
dir
```

---

## 11. Compila il programma

Esegui:

```bash
javac Main.java
```

### Risultato atteso

Se tutto è corretto:

* non vedrai messaggi di errore;
* nella stessa cartella comparirà il file:

```text
Main.class
```

### Controllo

Esegui di nuovo:

```bash
ls
```

oppure, su PowerShell:

```powershell
dir
```

Dovresti vedere almeno:

* `Main.java`
* `Main.class`

---

## 12. Esegui il programma

Ora esegui:

```bash
java Main
```

### Output atteso

```text
Ciao Java
```

Se compare l'output corretto, hai eseguito il tuo primo programma Java del corso usando il compilatore e il runtime.

---

# Parte 4 - Esperimenti guidati

## 13. Esperimento 1 - Modifica del messaggio

Apri `Main.java` e modifica il contenuto così:

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Sto compilando ed eseguendo il mio primo programma Java");
    }
}
```

Salva il file, poi esegui di nuovo:

```bash
javac Main.java
java Main
```

Verifica che il messaggio visualizzato sia cambiato.

---

## 14. Esperimento 2 - Errore di sintassi controllato

Modifica volontariamente il file rimuovendo il `;` finale:

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Errore voluto")
    }
}
```

Salva e prova a compilare:

```bash
javac Main.java
```

### Osserva

Il compilatore segnalerà un errore.

### Cosa devi fare

1. leggi il messaggio di errore;
2. individua la riga problematica;
3. correggi il file reinserendo `;`;
4. ricompila.

---

## 15. Esperimento 3 - Errore logico elementare

Crea un nuovo file chiamato:

```text
SommaSbagliata.java
```

Inserisci questo contenuto:

```java
public class SommaSbagliata {
    public static void main(String[] args) {
        int a = 10;
        int b = 5;
        int risultato = a - b;

        System.out.println("Il risultato della somma e': " + risultato);
    }
}
```

Compila ed esegui:

```bash
javac SommaSbagliata.java
java SommaSbagliata
```

### Domanda da porti

Il programma compila?
Sì.

Il programma si esegue?
Sì.

Il risultato è corretto rispetto al testo stampato?
No.

Questo è un **errore logico**.

### Correzione

Sostituisci:

```java
int risultato = a - b;
```

con:

```java
int risultato = a + b;
```

Ricompila ed esegui di nuovo.

---

# Parte 5 - Attività da svolgere autonomamente

## 16. Attività A - Spiega con parole tue

Crea nel file evidenza una sezione in cui spieghi con parole tue:

1. che cos'è un algoritmo;
2. che cos'è un programma;
3. che differenza c'è tra compilatore e interprete;
4. qual è il ruolo di JDK, JRE e JVM.

Non copiare definizioni da Internet. Scrivi una spiegazione semplice, corretta e leggibile.

---

## 17. Attività B - Crea un programma `MessaggioCorso`

Nella cartella `src/` crea il file:

```text
MessaggioCorso.java
```

Scrivi un programma che stampi un breve messaggio legato al tuo ingresso nel corso Java.

### Esempio di output atteso

```text
Sto iniziando il mio percorso nel corso Java.
```

Compila ed esegui il programma.

---

## 18. Attività C - Riconosci i tipi di errore

Nel file evidenza inserisci tre mini esempi:

* un esempio di **errore di compilazione**;
* un esempio di **errore a runtime**;
* un esempio di **errore logico**.

Per ciascun esempio devi scrivere:

* che cosa hai fatto;
* che cosa è successo;
* perché appartiene a quella categoria di errore.

---

# Parte 6 - Evidenze richieste

## 19. File di evidenza

Crea il file:

```text
labs/lab01_algoritmo_programma_compilazione/docs/evidence_lab01.md
```

Usa questa struttura.

```markdown
# Evidence Lab01

## 1. Ambiente utilizzato
- Sistema operativo:
- Versione Java (`java -version`):
- Versione compilatore (`javac -version`):
- Editor usato:

## 2. Concetti compresi
- Che cos'è un algoritmo
- Che cos'è un programma
- Differenza tra compilatore e interprete
- Differenza tra JDK, JRE e JVM

## 3. Programmi realizzati
- Main.java
- SommaSbagliata.java
- MessaggioCorso.java

## 4. Output osservati
Riporta gli output più significativi.

## 5. Errori incontrati
Descrivi almeno un errore di compilazione e come lo hai corretto.

## 6. Tipi di errore
Spiega con esempi:
- errore di compilazione
- errore a runtime
- errore logico

## 7. Conclusioni
Scrivi che cosa hai capito meglio grazie a questo laboratorio.
```

---

## 20. File da consegnare nel repository

Al termine del laboratorio, nel repository devono essere presenti almeno:

```text
labs/lab01_algoritmo_programma_compilazione/
  docs/
    evidence_lab01.md
  src/
    Main.java
    SommaSbagliata.java
    MessaggioCorso.java
```

### Importante

I file `.class` non devono essere consegnati nel repository.

---

# Parte 7 - Errori comuni da evitare

## 21. Errori frequenti

### 1. Nome file diverso dal nome della classe pubblica

Se la classe si chiama `Main`, il file deve chiamarsi:

```text
Main.java
```

### 2. Dimenticare `;`

È uno degli errori di compilazione più frequenti.

### 3. Compilare dalla cartella sbagliata

Prima di lanciare `javac`, verifica sempre dove ti trovi.

### 4. Eseguire una classe diversa da quella compilata

Controlla il nome esatto nella riga di comando.

### 5. Confondere errore logico con errore di sintassi

Un programma può compilare correttamente e produrre comunque un risultato sbagliato.

### 6. Consegnare file `.class`

Nel repository vanno consegnati i sorgenti e le evidenze, non i file generati dalla compilazione.

---

# Parte 8 - Checklist finale

Prima di concludere verifica tutto questo:

* [ ] so spiegare che cos'è un algoritmo;
* [ ] so spiegare che cos'è un programma;
* [ ] so distinguere compilatore e interprete;
* [ ] so distinguere JDK, JRE e JVM;
* [ ] so creare un file `.java`;
* [ ] so compilare con `javac`;
* [ ] so eseguire con `java`;
* [ ] ho riconosciuto almeno un errore di compilazione;
* [ ] ho riconosciuto almeno un errore logico;
* [ ] ho creato il file `evidence_lab01.md`;
* [ ] ho salvato i file nelle cartelle corrette;
* [ ] ho eseguito `git add`, `git commit` e `git push`.

---

# Parte 9 - Consegna finale

Quando hai completato il laboratorio:

1. controlla i file creati;
2. aggiorna il file `evidence_lab01.md`;
3. esegui i comandi Git dalla radice del repository.

```bash
git add .
git commit -m "Completato Lab01 - algoritmo, programma, compilazione"
git push
```

---

# Parte 10 - Approfondimenti consigliati

Se vuoi consolidare ulteriormente questo laboratorio, puoi rileggere:

* documentazione Oracle introduttiva su Java: [https://docs.oracle.com/javase/tutorial/getStarted/intro/index.html](https://docs.oracle.com/javase/tutorial/getStarted/intro/index.html)
* tutorial Java in VS Code: [https://code.visualstudio.com/docs/java/java-tutorial](https://code.visualstudio.com/docs/java/java-tutorial)

L'obiettivo, per adesso, non è imparare molte istruzioni Java, ma capire **che cosa stai facendo quando scrivi, compili ed esegui un programma**.

