# ESERCIZI AUTONOMI LAB02 - Variabili, tipi, operatori e cast primitivi

## 1. Scopo
In questo file trovi una selezione di esercizi autonomi da svolgere dopo il laboratorio guidato.

Gli esercizi sono organizzati in tre livelli:

- Base
- Intermedio
- Challenge

In questa fase del corso gli esercizi devono essere svolti con queste regole:

- un solo file `.java` per esercizio;
- una sola classe;
- un solo metodo `main`;
- nessuna utility class separata;
- nessun package complesso.

## 2. Struttura consigliata delle cartelle
Crea la cartella:

```text
labs/lab02/esercizi/
```

Per ogni esercizio crea un file separato.

Esempio:

```text
Esercizio01Quadrato.java
Esercizio02Cerchio.java
Esercizio03MediaTreNumeri.java
```

## 3. Esercizi Base

### Esercizio Base 1 - Quadrato
Leggi il lato di un quadrato e calcola:

- perimetro
- area

Suggerimento:

- usa `double` per il lato;
- stampa i risultati in modo chiaro.

Questo esercizio è adattato da un classico problema su input/output e operazioni aritmetiche. citeturn918446view0

### Esercizio Base 2 - Cerchio
Leggi il raggio di un cerchio e calcola:

- circonferenza
- area

Suggerimento:

- usa `Math.PI`;
- usa `double` per il raggio.

Anche questo è adattato dal set di esercizi lineari e aritmetici. citeturn918446view0

### Esercizio Base 3 - Tre numeri successivi
Leggi un numero intero e visualizza i tre numeri successivi.

Esempio:

```text
Input: 10
Output: 11 12 13
```

L'idea proviene dallo stesso set di esercizi introduttivi. citeturn918446view0

### Esercizio Base 4 - Media di tre numeri
Leggi tre numeri e calcola la media.

Attenzione:

- usa `double` almeno nel risultato finale;
- verifica cosa succede se usi solo `int`.

L'originale compare nel set di input/output e operazioni aritmetiche. citeturn918446view0

## 4. Esercizi Intermedi

### Esercizio Intermedio 1 - Noleggio DVD
Leggi il numero di DVD noleggiati. Sapendo che ogni DVD costa 10 euro, calcola il costo totale.

Estensione consigliata:

- mostra anche il costo medio per DVD.

Questo esercizio è adattato da un problema aritmetico lineare del primo set. citeturn918446view0

### Esercizio Intermedio 2 - Totale con IVA
Leggi:

- nome prodotto
- quantità
- prezzo unitario
- percentuale IVA

Calcola:

- subtotale
- importo IVA
- totale finale

Poi crea anche una variabile `int` ottenuta con cast esplicito dal totale e confronta i due valori.

Questo esercizio non è preso direttamente da un singolo testo, ma è coerente con gli esercizi lineari del primo set ed è utile per consolidare i cast primitivi. citeturn918446view0

### Esercizio Intermedio 3 - Media con cast consapevole
Leggi tre numeri interi e calcola la media in due modi:

1. senza cast;
2. con cast a `double`.

Confronta i risultati e spiega la differenza nel file di evidenza.

## 5. Challenge

### Challenge 1 - Noleggio auto
Leggi:

- numero di giorni;
- chilometri iniziali;
- chilometri finali.

Sapendo che la tariffa è:

- 50 euro al giorno
- 2 euro per ogni chilometro percorso

calcola il costo totale.

Questo esercizio è adattato da uno dei problemi lineari del primo set. citeturn918446view0

### Challenge 2 - Rilegatura tesi
Leggi:

- numero di pagine in bianco e nero;
- numero di pagine a colori.

Sapendo che:

- ogni pagina in bianco e nero costa 0,10 euro;
- ogni pagina a colori costa 0,15 euro;
- la copertina costa 10 euro;

calcola il costo totale.

Questo problema è anch'esso adattato dal primo set, ma qui viene usato come esercizio di consolidamento. citeturn918446view0

## 6. Evidenze richieste
Crea il file:

```text
labs/lab02_variabili_tipi_operatori_cast/docs/evidence_esercizi_lab02.md
```

Per ogni esercizio svolto inserisci:

- nome del file Java;
- breve descrizione del problema;
- output ottenuto;
- eventuali difficoltà incontrate.

Per almeno un esercizio devi aggiungere una breve spiegazione sul comportamento dei cast primitivi.

## 7. Ordine consigliato di svolgimento
Segui questo ordine:

1. Quadrato
2. Cerchio
3. Tre numeri successivi
4. Media di tre numeri
5. Noleggio DVD
6. Totale con IVA
7. Challenge facoltative

## 8. Errori comuni da evitare

- usare `int` quando il problema richiede decimali;
- dimenticare il cast quando vuoi forzare una divisione reale;
- dichiarare una variabile e non inizializzarla;
- stampare i risultati senza spiegare cosa rappresentano;
- confondere il valore originale con il valore convertito.

## 9. Approfondimenti ufficiali

- Tipi di dato primitivi: https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html
- Operatori in Java: https://docs.oracle.com/javase/tutorial/java/nutsandbolts/operators.html
- Classe `Math`: https://docs.oracle.com/javase/8/docs/api/java/lang/Math.html
