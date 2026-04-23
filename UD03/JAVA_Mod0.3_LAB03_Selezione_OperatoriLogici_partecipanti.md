# LAB03 - Selezione con `if`, `if/else`, `else if` e operatori logici

## 1. Obiettivi
In questo laboratorio imparerai a:

- usare correttamente le istruzioni di selezione in Java;
- distinguere tra `if`, `if/else` e catene `if / else if / else`;
- usare gli operatori relazionali e logici per costruire condizioni corrette;
- leggere dati da tastiera con `Scanner`;
- prendere decisioni diverse in base ai valori inseriti;
- scrivere piccoli programmi autonomi senza ancora usare metodi statici separati.

Alla fine del laboratorio dovrai essere in grado di leggere alcuni valori in input, analizzarli con condizioni e stampare il risultato corretto.

---

## 2. Prerequisiti
Prima di iniziare devi sapere già:

- creare un file `.java` ed eseguirlo;
- compilare ed eseguire un programma Java;
- dichiarare variabili di tipo `int`, `double`, `char` e `String`;
- leggere input da tastiera con `Scanner`;
- usare operatori aritmetici e cast primitivi.

Se su questi punti hai dubbi, ripassa prima il laboratorio precedente.

---

## 3. Software necessario
Per questo laboratorio ti servono:

- **JDK** installato e funzionante;
- **VS Code** oppure un altro editor Java già configurato;
- terminale o prompt dei comandi;
- repository del corso già clonato in locale.

---

## 4. Struttura del lavoro
Per questo laboratorio userai una struttura volutamente semplice.

Lavora dentro una cartella come questa:

```text
labs/lab03_selezione_operatori_logici/
```

All'interno crea questi elementi:

```text
labs/lab03_selezione_operatori_logici/
  README.md
  docs/
    evidence_lab03.md
  src/
    Lab03Selezione.java
```

In questa fase userai **un solo file Java** con **una sola classe** e **un solo `main`**.

---

## 5. Concetti da comprendere

### 5.1 Che cos'è una selezione
Una selezione è una scelta nel flusso del programma.

In pratica:

- se la condizione è vera, esegui un blocco di istruzioni;
- se la condizione è falsa, ne esegui un altro oppure non esegui nulla.

Questa è una delle strutture fondamentali della programmazione.

---

### 5.2 `if`
La forma più semplice è:

```java
if (condizione) {
    // istruzioni
}
```

Il blocco viene eseguito solo se la condizione è `true`.

Esempio:

```java
if (spesa > 100) {
    System.out.println("Hai superato 100 euro");
}
```

---

### 5.3 `if/else`
Se vuoi prevedere due strade alternative:

```java
if (condizione) {
    // caso vero
} else {
    // caso falso
}
```

Esempio:

```java
if (eta >= 18) {
    System.out.println("Maggiorenne");
} else {
    System.out.println("Minorenne");
}
```

---

### 5.4 `if / else if / else`
Se i casi possibili sono più di due:

```java
if (condizione1) {
    // primo caso
} else if (condizione2) {
    // secondo caso
} else {
    // caso finale
}
```

Questo schema è molto utile quando devi gestire soglie, classificazioni o fasce di valori.

---

### 5.5 Operatori relazionali
Gli operatori relazionali confrontano due valori e restituiscono un risultato booleano (`true` oppure `false`).

| Operatore | Significato |
|---|---|
| `==` | uguale a |
| `!=` | diverso da |
| `>` | maggiore di |
| `<` | minore di |
| `>=` | maggiore o uguale |
| `<=` | minore o uguale |

Esempi:

```java
spesa > 100
eta >= 18
numero1 == numero2
```

---

### 5.6 Operatori logici
Gli operatori logici servono a combinare più condizioni.

| Operatore | Significato |
|---|---|
| `&&` | AND logico: vero solo se entrambe le condizioni sono vere |
| `||` | OR logico: vero se almeno una condizione è vera |
| `!` | NOT logico: nega una condizione |

Esempi:

```java
eta >= 18 && reddito > 0
mese >= 1 && mese <= 12
!(numero == 0)
```

---

### 5.7 Attenzione a `=` e `==`
Questo è un errore molto comune.

- `=` serve per assegnare un valore;
- `==` serve per confrontare due valori.

Corretto:

```java
int x = 10;
if (x == 10) {
    System.out.println("x vale 10");
}
```

---

### 5.8 La classe `Scanner`
In questo laboratorio userai ancora `Scanner` per leggere input da tastiera.

Importazione:

```java
import java.util.Scanner;
```

Creazione dell'oggetto:

```java
Scanner input = new Scanner(System.in);
```

Metodi utili:

- `nextInt()` per leggere un intero;
- `nextDouble()` per leggere un numero decimale;
- `next()` per leggere una parola singola;
- `nextLine()` per leggere una riga intera.

Esempio:

```java
System.out.print("Inserisci la spesa: ");
double spesa = input.nextDouble();
```

In questa fase **non chiudere** ancora `Scanner` con `input.close()` se stai facendo più prove rapide nello stesso programma e vuoi mantenere il codice il più lineare possibile.

---

## 6. Attività guidata
Realizzerai un programma che legge l'importo di una spesa e applica uno sconto secondo queste regole:

- nessuno sconto fino a 100 euro;
- sconto del 5% se la spesa è maggiore di 100 euro;
- sconto del 10% se la spesa è maggiore di 250 euro.

Il programma deve stampare:

- importo iniziale;
- sconto applicato;
- totale finale.

### 6.1 Crea il file Java
Nel file `src/Lab03Selezione.java` scrivi prima lo scheletro del programma:

```java
import java.util.Scanner;

public class Lab03Selezione {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

    }
}
```

---

### 6.2 Leggi l'importo della spesa
Aggiungi il codice per leggere il valore:

```java
System.out.print("Inserisci l'importo totale della spesa: ");
double spesa = input.nextDouble();
```

---

### 6.3 Prepara le variabili di lavoro
Aggiungi queste variabili:

```java
double percentualeSconto = 0;
double importoSconto = 0;
double totaleDaPagare = 0;
```

---

### 6.4 Costruisci la logica con `if / else if / else`
Aggiungi la selezione:

```java
if (spesa > 250) {
    percentualeSconto = 10;
} else if (spesa > 100) {
    percentualeSconto = 5;
} else {
    percentualeSconto = 0;
}
```

---

### 6.5 Calcola i valori finali
Aggiungi i calcoli:

```java
importoSconto = spesa * percentualeSconto / 100;
totaleDaPagare = spesa - importoSconto;
```

---

### 6.6 Stampa il risultato
Aggiungi la parte finale:

```java
System.out.println("------------------------------");
System.out.println("Spesa iniziale: " + spesa + " euro");
System.out.println("Sconto applicato: " + percentualeSconto + "%");
System.out.println("Importo sconto: " + importoSconto + " euro");
System.out.println("Totale da pagare: " + totaleDaPagare + " euro");
```

---

### 6.7 Codice completo atteso
Il programma completo diventa:

```java
import java.util.Scanner;

public class Lab03Selezione {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        System.out.print("Inserisci l'importo totale della spesa: ");
        double spesa = input.nextDouble();

        double percentualeSconto = 0;
        double importoSconto;
        double totaleDaPagare;

        if (spesa > 250) {
            percentualeSconto = 10;
        } else if (spesa > 100) {
            percentualeSconto = 5;
        } else {
            percentualeSconto = 0;
        }

        importoSconto = spesa * percentualeSconto / 100;
        totaleDaPagare = spesa - importoSconto;

        System.out.println("------------------------------");
        System.out.println("Spesa iniziale: " + spesa + " euro");
        System.out.println("Sconto applicato: " + percentualeSconto + "%");
        System.out.println("Importo sconto: " + importoSconto + " euro");
        System.out.println("Totale da pagare: " + totaleDaPagare + " euro");
    }
}
```

---

## 7. Compilazione ed esecuzione
Posizionati nella cartella del laboratorio e usa questi comandi.

### Compilazione
```bash
javac -d out src/Lab03Selezione.java
```

### Esecuzione
```bash
java -cp out Lab03Selezione
```

---

## 8. Verifica con casi di prova
Esegui il programma almeno con questi casi:

### Caso 1
Input:

```text
80
```

Risultato atteso:
- sconto 0%
- totale 80

### Caso 2
Input:

```text
150
```

Risultato atteso:
- sconto 5%
- totale 142.5

### Caso 3
Input:

```text
300
```

Risultato atteso:
- sconto 10%
- totale 270

---

## 9. Estensione del laboratorio
Dopo aver completato il programma principale, aggiungi una seconda parte nello stesso file oppure crea una seconda versione dello stesso file e risolvi questo problema:

Leggi tre numeri interi e stampa uno dei seguenti risultati:

- `Tutti uguali`
- `Due uguali e uno diverso`
- `Tutti diversi`

Suggerimento:

- usa confronti con `==`;
- ragiona prima sui casi più specifici;
- attenzione all'ordine delle condizioni.

---

## 10. Evidenze da consegnare
Nel file `docs/evidence_lab03.md` inserisci queste sezioni.

### 10.1 Intestazione
- nome laboratorio;
- data;
- nome partecipante.

### 10.2 Obiettivo del laboratorio
Spiega in 5-10 righe cosa hai imparato.

### 10.3 Codice sviluppato
Incolla il codice Java finale oppure riporta il percorso del file creato.

### 10.4 Casi di prova eseguiti
Riporta almeno 3 casi di prova con:
- input inserito;
- output ottenuto;
- breve commento.

### 10.5 Difficoltà incontrate
Spiega eventuali errori o dubbi.

### 10.6 Conclusioni
Scrivi una breve riflessione finale.

---

## 11. Errori comuni da evitare

### Errore 1: usare `=` al posto di `==`
Sbagliato:

```java
if (x = 10)
```

Corretto:

```java
if (x == 10)
```

### Errore 2: condizioni scritte in modo incompleto
Sbagliato:

```java
if (100 < spesa < 250)
```

In Java non si scrive così.

Corretto:

```java
if (spesa > 100 && spesa <= 250)
```

### Errore 3: ordine sbagliato nelle soglie
Se controlli prima `spesa > 100`, il caso `spesa > 250` non verrà mai gestito nel modo previsto.

Corretto:
- prima il caso più specifico;
- poi quello meno specifico.

### Errore 4: dimenticare le parentesi tonde della condizione
Sbagliato:

```java
if spesa > 100 {
```

Corretto:

```java
if (spesa > 100) {
```

---

## 12. Checklist finale
Prima di considerare concluso il laboratorio controlla di aver fatto tutto.

- [ ] Ho creato la cartella del lab.
- [ ] Ho scritto il file `Lab03Selezione.java`.
- [ ] Ho usato `Scanner` per leggere almeno un input.
- [ ] Ho usato `if / else if / else` correttamente.
- [ ] Ho eseguito almeno 3 test diversi.
- [ ] Ho compilato senza errori.
- [ ] Ho aggiornato `evidence_lab03.md`.
- [ ] Ho salvato il lavoro nel monorepo.

---

## 13. Approfondimenti ufficiali
Per approfondire puoi consultare:

- Documentazione Oracle su Java: https://docs.oracle.com/en/java/
- Classe `Scanner`: https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Scanner.html
- Tutorial Java di VS Code: https://code.visualstudio.com/docs/java/java-tutorial

