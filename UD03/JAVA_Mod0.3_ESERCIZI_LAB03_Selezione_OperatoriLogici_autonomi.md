# ESERCIZI AUTONOMI - LAB03 - Selezione e operatori logici

## 1. Come usare questo file
Questi esercizi servono a consolidare ciò che hai studiato nel laboratorio su:

- `if`
- `if/else`
- `if / else if / else`
- operatori relazionali
- operatori logici
- input da tastiera con `Scanner`

Lavora sempre, in questa fase, con:

- **un solo file `.java` per esercizio**;
- **una sola classe**;
- **un solo `main`**.

Non usare ancora metodi statici separati o più classi nello stesso esercizio.

---

## 2. Richiamo rapido: `Scanner`
Per leggere dati da tastiera puoi usare questo schema base:

```java
import java.util.Scanner;

public class NomeClasse {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        System.out.print("Inserisci un numero: ");
        int numero = input.nextInt();

        System.out.println("Hai inserito: " + numero);
    }
}
```

Metodi utili per questi esercizi:

- `nextInt()`
- `nextDouble()`
- `next()`

---

## 3. Regole di consegna
Per ogni esercizio:

- crea un file Java dedicato;
- compila ed esegui il programma;
- prova almeno due o tre input diversi;
- annota i risultati nel file `docs/evidence_lab03.md`.

---

## 4. Esercizi Base

### Esercizio Base 1 - Sconto su spesa
Scrivi un programma che richieda l'importo totale di una spesa e applichi:

- nessuno sconto fino a 100 euro;
- sconto del 5% oltre 100 euro;
- sconto del 10% oltre 250 euro.

Il programma deve visualizzare:

- importo iniziale;
- percentuale di sconto;
- totale da pagare.

---

### Esercizio Base 2 - Tre numeri: uguali o diversi
Leggi tre numeri interi e visualizza uno dei seguenti messaggi:

- `Tutti uguali`
- `Due uguali e uno diverso`
- `Tutti diversi`

Suggerimento:
- prova prima a riconoscere il caso più specifico.

---

### Esercizio Base 3 - Maggiorenne o minorenne
Leggi l'età di una persona e stampa:

- `Maggiorenne` se l'età è almeno 18;
- `Minorenne` altrimenti.

Aggiungi poi una seconda versione che distingua anche:

- bambino;
- adolescente;
- adulto.

---

## 5. Esercizi Intermedi

### Esercizio Intermedio 1 - Calcolatrice semplice con `if/else`
Leggi due numeri e un operatore tra:

- `+`
- `-`
- `*`
- `/`

Esegui l'operazione richiesta e stampa il risultato.

Controlla anche il caso della divisione per zero.

Puoi leggere l'operatore come parola singola:

```java
String operatore = input.next();
```

Poi confrontarlo con:

```java
operatore.equals("+")
```

---

### Esercizio Intermedio 2 - Biglietto con categoria utente
Una società di trasporti applica questi sconti rispetto al prezzo pieno:

- Studente `S` → 10%
- Disoccupato `D` → 15%
- Pensionato `P` → 20%
- Altro `A` → 0%

Leggi:

- il costo base del biglietto;
- la categoria dell'utente.

Visualizza l'importo finale da pagare.

Per ora puoi risolvere l'esercizio con `if / else if / else`.

---

### Esercizio Intermedio 3 - Intervallo valido
Leggi un numero intero e stabilisci se appartiene all'intervallo compreso tra 10 e 20 inclusi.

Se appartiene all'intervallo stampa `VALORE VALIDO`, altrimenti `VALORE NON VALIDO`.

Obiettivo dell'esercizio:
- usare correttamente `&&`.

---

### Esercizio Intermedio 4 - Mese valido
Leggi un numero intero che rappresenta un mese.

Se il numero è compreso tra 1 e 12 stampa `MESE VALIDO`, altrimenti `MESE NON VALIDO`.

Anche qui il punto chiave è usare bene gli operatori logici.

---

## 6. Challenge

### Challenge 1 - Triangolo per tipo
Leggi le misure di tre lati e stabilisci se il triangolo è:

- equilatero;
- isoscele;
- scaleno.

Per questa challenge **non devi** ancora calcolare area e perimetro.

Concentrati sulla classificazione in base all'uguaglianza dei lati.

---

### Challenge 2 - Fasce di voto
Leggi un voto intero e stampa:

- `Insufficiente` se minore di 18
- `Sufficiente` se da 18 a 23
- `Buono` se da 24 a 27
- `Ottimo` se da 28 a 30

Attenzione all'ordine delle condizioni.

---

## 7. Suggerimenti di lavoro
Quando affronti un esercizio con selezioni, prova a seguire questo metodo.

### Passo 1
Leggi il testo e individua i casi possibili.

### Passo 2
Scrivi su carta le condizioni in linguaggio naturale.

Esempio:

- se la spesa è maggiore di 250 → sconto 10
- altrimenti se è maggiore di 100 → sconto 5
- altrimenti → sconto 0

### Passo 3
Traduci le condizioni in Java.

### Passo 4
Esegui prove con valori diversi.

### Passo 5
Controlla se l'ordine dei casi è corretto.

---

## 8. Errori frequenti

### Errore 1
Usare `=` invece di `==`.

### Errore 2
Scrivere condizioni matematiche non valide, per esempio:

```java
10 < x < 20
```

### Errore 3
Mettere prima una condizione generica e poi una più specifica.

### Errore 4
Dimenticare il controllo della divisione per zero.

### Errore 5
Confrontare una stringa con `==` invece di usare `.equals()`.

Per esempio:

```java
operatore.equals("+")
```

---

## 9. Evidenze richieste
Nel file `docs/evidence_lab03.md` aggiungi:

- titolo degli esercizi svolti;
- codice o percorso dei file;
- almeno due test per ogni esercizio completato;
- eventuali errori incontrati e correzioni fatte;
- una breve autovalutazione finale.

---

## 10. Obiettivo minimo consigliato
Per considerare concluso bene questo pacchetto dovresti completare almeno:

- 2 esercizi Base;
- 2 esercizi Intermedi.

Se completi anche almeno una Challenge, hai consolidato molto bene il laboratorio.

