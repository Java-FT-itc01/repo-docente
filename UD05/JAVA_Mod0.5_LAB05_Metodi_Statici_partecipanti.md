# JAVA - Modulo 0.5 - LAB05
# Metodi statici nello stesso file

## 1. Titolo del laboratorio
**Metodi statici nello stesso file: organizzare meglio il programma senza introdurre ancora più classi**

---

## 2. Obiettivi del laboratorio
In questo laboratorio imparerai a:

- capire perché un programma troppo lungo scritto tutto dentro `main` diventa difficile da leggere, correggere e riutilizzare;
- creare e usare **metodi statici** nello stesso file Java;
- distinguere tra un metodo che **restituisce un valore** e un metodo che **esegue un'azione**;
- passare **parametri** a un metodo;
- richiamare metodi statici dal `main`;
- rifattorizzare un programma semplice dividendolo in parti più chiare;
- mantenere ancora una struttura semplice: **una sola classe, un solo file, un solo `main`**.

Questo laboratorio non introduce ancora classi separate, package multipli o utility esterne. Lo scopo è fare un passo intermedio corretto: prima impari a organizzare il codice, poi imparerai a distribuirlo su più file.

---

## 3. Prerequisiti
Prima di iniziare devi aver già compreso:

- struttura minima di un programma Java;
- uso di `Scanner` per leggere input;
- variabili e tipi primitivi;
- operatori aritmetici;
- `if`, `if/else`, `switch`;
- `while`, `do-while`, `for`;
- compilazione ed esecuzione da terminale o da VS Code.

Se su questi punti hai ancora incertezze, ripassa i laboratori precedenti prima di proseguire.

---

## 4. Software necessario
Per questo laboratorio ti servono:

- **VS Code** con supporto Java;
- **JDK** installato e configurato;
- **Git**;
- terminale integrato di VS Code oppure terminale di sistema.

### Verifica rapida
Esegui:

```bash
java -version
javac -version
git --version
```

Se uno di questi comandi non funziona, risolvi prima il problema dell'ambiente.

Per installazioni o aggiornamenti usa sempre le fonti ufficiali:

- VS Code: https://code.visualstudio.com/download
- Eclipse Temurin JDK: https://adoptium.net/temurin/releases/
- Git: https://git-scm.com/downloads
- Java in VS Code: https://code.visualstudio.com/docs/java/java-tutorial

---

## 5. Concetti da comprendere prima di scrivere codice

### 5.1 Perché usare i metodi
Finora hai scritto programmi piccoli, spesso con tutto il codice dentro il `main`.

Questo va bene all'inizio, ma appena un programma cresce compaiono problemi molto comuni:

- il codice è lungo e poco leggibile;
- la stessa logica viene ripetuta più volte;
- correggere un errore diventa più difficile;
- non è chiaro dove finisce una fase del programma e dove ne inizia un'altra.

I **metodi** servono proprio a dividere il programma in parti con un compito preciso.

Per esempio, invece di scrivere tutto in sequenza, puoi creare metodi come:

- `leggiNumero()`
- `calcolaMedia(...)`
- `stampaRisultato(...)`

Ogni metodo deve avere una responsabilità chiara.

---

### 5.2 Che cosa significa `static`
In questo laboratorio userai **metodi statici**.

Per ora puoi interpretarli così:

- appartengono alla **classe**;
- possono essere richiamati direttamente dal `main`;
- non richiedono ancora la creazione di oggetti.

Dato che in questa fase stai lavorando con una sola classe che contiene il `main`, usare metodi statici è la scelta giusta e più semplice.

Esempio:

```java
public static void saluta() {
    System.out.println("Benvenuto nel programma");
}
```

Questo metodo può essere richiamato scrivendo:

```java
saluta();
```

---

### 5.3 Metodi `void` e metodi che restituiscono un valore
Un metodo può:

- **fare qualcosa senza restituire nulla**;
- **calcolare e restituire un valore**.

#### Metodo `void`
Un metodo `void` esegue un'azione.

Esempio:

```java
public static void stampaTitolo() {
    System.out.println("Calcolo della media");
}
```

#### Metodo con `return`
Un metodo con un tipo di ritorno calcola qualcosa e restituisce il risultato.

Esempio:

```java
public static double calcolaMedia(double a, double b, double c) {
    return (a + b + c) / 3;
}
```

---

### 5.4 Parametri
I parametri sono valori ricevuti dal metodo.

Esempio:

```java
public static double calcolaAreaQuadrato(double lato) {
    return lato * lato;
}
```

Qui `lato` è un parametro.

Quando richiami il metodo:

```java
double area = calcolaAreaQuadrato(5.0);
```

il valore `5.0` viene passato al parametro `lato`.

---

### 5.5 Ordine generale del file
In questa fase userai una struttura molto semplice:

```java
import java.util.Scanner;

public class NomeClasse {

    public static void main(String[] args) {
        // codice principale
    }

    public static ... metodo1(...) {
        ...
    }

    public static ... metodo2(...) {
        ...
    }
}
```

Questa struttura è importante perché rende leggibile il file:

- in alto gli `import`;
- poi la classe;
- poi il `main`;
- poi i metodi di supporto.

---

## 6. Attività guidata
In questo laboratorio realizzerai un programma che:

1. legge tre numeri reali;
2. calcola la media;
3. stampa il risultato;
4. usa metodi statici per separare bene i compiti.

### Nome consigliato del file
Crea un file chiamato:

```text
Lab05MediaMetodi.java
```

---

## 7. Implementazione passo-passo

### Step 1 - Crea la struttura minima del file
Inserisci questo codice iniziale:

```java
import java.util.Scanner;

public class Lab05MediaMetodi {

    public static void main(String[] args) {

    }
}
```

Salva il file.

---

### Step 2 - Crea il metodo per leggere un numero reale
Aggiungi sotto il `main` il seguente metodo:

```java
public static double leggiNumero(Scanner scanner, String messaggio) {
    System.out.print(messaggio);
    return scanner.nextDouble();
}
```

Questo metodo:

- riceve lo `Scanner`;
- riceve un messaggio da mostrare;
- legge un numero reale;
- restituisce il valore letto.

---

### Step 3 - Crea il metodo per calcolare la media
Aggiungi questo metodo:

```java
public static double calcolaMedia(double a, double b, double c) {
    return (a + b + c) / 3;
}
```

Questo metodo ha una sola responsabilità: calcolare la media di tre numeri.

---

### Step 4 - Crea il metodo per stampare il risultato
Aggiungi questo metodo:

```java
public static void stampaRisultato(double media) {
    System.out.println("La media dei tre numeri è: " + media);
}
```

---

### Step 5 - Completa il `main`
Ora completa il `main` in questo modo:

```java
public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);

    double x = leggiNumero(scanner, "Inserisci il primo numero: ");
    double y = leggiNumero(scanner, "Inserisci il secondo numero: ");
    double z = leggiNumero(scanner, "Inserisci il terzo numero: ");

    double media = calcolaMedia(x, y, z);

    stampaRisultato(media);

    scanner.close();
}
```

---

### Step 6 - Codice completo atteso
Alla fine il file dovrebbe essere simile a questo:

```java
import java.util.Scanner;

public class Lab05MediaMetodi {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        double x = leggiNumero(scanner, "Inserisci il primo numero: ");
        double y = leggiNumero(scanner, "Inserisci il secondo numero: ");
        double z = leggiNumero(scanner, "Inserisci il terzo numero: ");

        double media = calcolaMedia(x, y, z);

        stampaRisultato(media);

        scanner.close();
    }

    public static double leggiNumero(Scanner scanner, String messaggio) {
        System.out.print(messaggio);
        return scanner.nextDouble();
    }

    public static double calcolaMedia(double a, double b, double c) {
        return (a + b + c) / 3;
    }

    public static void stampaRisultato(double media) {
        System.out.println("La media dei tre numeri è: " + media);
    }
}
```

---

## 8. Compilazione ed esecuzione
Dal terminale, nella cartella che contiene il file, esegui:

```bash
javac Lab05MediaMetodi.java
java Lab05MediaMetodi
```

Prova con questi valori:

- 10
- 20
- 30

Dovresti ottenere media = 20.0

---

## 9. Verifiche da fare
Verifica che il tuo programma:

- compili senza errori;
- legga correttamente i tre numeri;
- calcoli la media corretta;
- stampi il risultato una sola volta;
- usi davvero i metodi statici, senza scrivere tutta la logica nel `main`.

---

## 10. Consegna richiesta
Prepara una breve evidenza nel file:

```text
docs/evidence_lab05.md
```

Se la cartella `docs` non esiste ancora, creala.

### Struttura minima dell'evidenza
Inserisci almeno queste sezioni:

```markdown
# Evidence Lab05

## Obiettivo
Descrivi in 3-5 righe l'obiettivo del laboratorio.

## File realizzato
Indica il nome del file Java creato.

## Metodi usati
Elenca i metodi creati e spiega in una frase a cosa servono.

## Output di esempio
Riporta almeno una esecuzione completa del programma.

## Difficoltà incontrate
Descrivi eventuali errori o dubbi.

## Conclusione
Spiega che cosa hai capito sull'uso dei metodi statici.
```

---

## 11. Errori comuni da evitare

### Errore 1 - Dimenticare `static`
Se scrivi:

```java
public double calcolaMedia(double a, double b, double c)
```

poi dal `main` potresti ottenere errore, perché in questa fase stai richiamando il metodo in modo statico.

Scrivi invece:

```java
public static double calcolaMedia(double a, double b, double c)
```

---

### Errore 2 - Dimenticare il `return`
Se un metodo dichiara di restituire `double`, deve restituire davvero un valore.

---

### Errore 3 - Chiudere lo `Scanner` troppo presto
Chiudi lo `Scanner` solo dopo aver finito tutte le letture.

---

### Errore 4 - Mettere tutta la logica nel `main`
Lo scopo di questo laboratorio è proprio evitare questo errore.

Il `main` deve coordinare il programma, non contenere tutto il dettaglio dell'elaborazione.

---

## 12. Estensioni facoltative
Se hai finito prima del tempo, prova una o più di queste varianti:

### Variante A
Aggiungi un metodo che stampi un titolo iniziale.

### Variante B
Aggiungi un metodo che determini il valore massimo tra i tre numeri.

### Variante C
Chiedi all'utente quanti numeri vuole inserire e prepara il terreno per un laboratorio futuro più generale.

---

## 13. Checklist finale
Prima di considerare concluso il laboratorio, controlla di aver fatto tutto questo:

- [ ] ho creato un solo file Java;
- [ ] ho usato una sola classe con `main`;
- [ ] ho creato almeno tre metodi statici;
- [ ] il programma compila senza errori;
- [ ] il programma esegue correttamente la media di tre numeri;
- [ ] ho prodotto il file di evidenza;
- [ ] so spiegare a cosa serve ciascun metodo.

---

## 14. Approfondimenti ufficiali
Per approfondire:

- Oracle Java Tutorials: https://docs.oracle.com/javase/tutorial/
- Documentazione `Scanner`: https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Scanner.html
- Guida Java in VS Code: https://code.visualstudio.com/docs/java/java-tutorial

