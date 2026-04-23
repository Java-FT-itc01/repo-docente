# LAB02 - Variabili, tipi, operatori e cast primitivi

## 1. Obiettivi
In questo laboratorio imparerai a:

- dichiarare e inizializzare variabili in Java;F
- distinguere i principali tipi primitivi (`int`, `double`, `char`, `boolean`);
- usare gli operatori aritmetici fondamentali;
- capire la differenza tra divisione intera e divisione reale;
- usare i cast primitivi in modo consapevole;
- scrivere un semplice programma Java che legge alcuni dati, li elabora e mostra il risultato.

## 2. Prerequisiti
Prima di iniziare devi aver completato il laboratorio precedente e devi saper:

- aprire il progetto locale in VS Code;
- aprire il terminale integrato;
- compilare un file Java con `javac`;
- eseguire una classe con `java`;
- riconoscere il metodo `main`.

## 3. Software necessario
Per questo laboratorio ti servono:

- VS Code;
- JDK correttamente installato;
- terminale integrato funzionante;
- repository del corso già clonato in locale.

## 4. Concetti da comprendere prima di iniziare

### 4.1 Variabile
Una variabile è un contenitore con un nome e un tipo.

Esempio:

```java
int eta = 25;
double prezzo = 19.99;
char iniziale = 'E';
boolean attivo = true;
```

Il tipo indica quali valori puoi memorizzare e quali operazioni puoi eseguire.

### 4.2 Tipi primitivi usati in questo laboratorio
Per iniziare userai soprattutto:

- `int` per numeri interi;
- `double` per numeri con parte decimale;
- `char` per un singolo carattere;
- `boolean` per valori veri/falsi.

### 4.3 Operatori aritmetici
I principali operatori aritmetici sono:

- `+` somma
- `-` sottrazione
- `*` moltiplicazione
- `/` divisione
- `%` resto della divisione

### 4.4 Divisione intera e divisione reale
Questo punto è molto importante.

```java
int a = 5;
int b = 2;
System.out.println(a / b);
```

Il risultato è `2`, non `2.5`, perché `a` e `b` sono interi.

Se vuoi ottenere `2.5`, almeno uno dei due valori deve essere `double` oppure devi fare un cast:

```java
double risultato = (double) a / b;
System.out.println(risultato);
```

### 4.5 Cast primitivi
Il cast serve a convertire un valore da un tipo a un altro.

Esempio:

```java
double prezzo = 12.8;
int prezzoIntero = (int) prezzo;
```

In questo caso il risultato sarà `12`.
La parte decimale viene persa.

## 5. Struttura del lavoro
In questo laboratorio lavorerai in un solo file Java.

Crea questa cartella, se non esiste ancora:

```text
labs/lab02/src/
```

All'interno crea il file:

```text
labs/lab02/src/VariabiliOperatoriCast.java
```

## 6. Attività guidata
Scrivi il programma seguendo i passaggi nell'ordine indicato.

### Step 1 - Crea la classe con `main`
Inserisci questo codice iniziale:

```java
public class VariabiliOperatoriCast {
    public static void main(String[] args) {

    }
}
```

### Step 2 - Dichiara le variabili
Dentro il `main`, dichiara le seguenti variabili:

- `nomeProdotto` di tipo `String`
- `quantita` di tipo `int`
- `prezzoUnitario` di tipo `double`
- `ivaPercentuale` di tipo `int`

Assegna valori di esempio.

### Step 3 - Calcola il subtotale
Crea una variabile `subtotale` e calcola:

```text
quantita * prezzoUnitario
```

### Step 4 - Calcola l'importo IVA
Crea una variabile `importoIva`.

Suggerimento:

```java
double importoIva = subtotale * ivaPercentuale / 100;
```

### Step 5 - Calcola il totale finale
Crea una variabile `totale` come somma di:

- `subtotale`
- `importoIva`

### Step 6 - Mostra i risultati
Stampa a video:

- nome prodotto
- quantità
- prezzo unitario
- subtotale
- importo IVA
- totale finale

### Step 7 - Aggiungi un esempio di cast
Aggiungi una variabile `totaleIntero` di tipo `int` ottenuta convertendo `totale`.

Esempio:

```java
int totaleIntero = (int) totale;
```

Stampa il valore di `totaleIntero` e confrontalo con `totale`.

### Step 8 - Verifica la divisione intera
Aggiungi questo blocco nel `main`:

```java
int x = 7;
int y = 2;
System.out.println("Divisione intera: " + (x / y));
System.out.println("Divisione con cast: " + ((double) x / y));
```

Osserva con attenzione i due risultati.

## 7. Compilazione ed esecuzione
Apri il terminale nella cartella del laboratorio ed esegui:

```bash
javac VariabiliOperatoriCast.java
java VariabiliOperatoriCast
```

## 8. Consegna richiesta
Prepara il file di evidenza:

```text
labs/lab02/src/docs/evidence_lab02.md
```

Nel file di evidenza inserisci:

### Sezione 1 - Obiettivo
Scrivi in poche righe cosa hai fatto in questo laboratorio.

### Sezione 2 - Codice realizzato
Indica il nome del file creato.

### Sezione 3 - Output ottenuto
Copia l'output del programma eseguito.

### Sezione 4 - Osservazioni sui cast
Spiega:

- cosa succede quando converti un `double` in `int`;
- perché `7 / 2` produce un risultato diverso da `(double) 7 / 2`.

### Sezione 5 - Difficoltà incontrate
Descrivi eventuali errori e come li hai corretti.

## 9. Errori comuni da evitare

### Errore 1
Dimenticare il punto e virgola `;` alla fine di una istruzione.

### Errore 2
Usare una variabile senza averla dichiarata.

### Errore 3
Assegnare un valore decimale a una variabile `int` senza cast.

### Errore 4
Aspettarsi che `5 / 2` dia `2.5`.

### Errore 5
Dimenticare che un cast da `double` a `int` elimina la parte decimale.

## 10. Checklist finale
Prima di considerare concluso il laboratorio, verifica di aver fatto tutto.

- Ho creato il file `VariabiliOperatoriCast.java`.
- Ho dichiarato variabili di tipo `int` e `double`.
- Ho eseguito calcoli aritmetici con le variabili.
- Ho usato almeno un cast esplicito.
- Ho verificato la differenza tra divisione intera e divisione con cast.
- Ho compilato il file senza errori.
- Ho creato il file `evidence_lab02.md`.

## 11. Approfondimenti ufficiali
Per approfondire puoi consultare:

- Documentazione ufficiale Oracle sul linguaggio Java: https://docs.oracle.com/javase/tutorial/java/nutsandbolts/
- Tipi di dato primitivi in Java: https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html
- Operatori in Java: https://docs.oracle.com/javase/tutorial/java/nutsandbolts/operators.html
