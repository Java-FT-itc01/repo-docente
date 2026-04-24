# JAVA - Modulo 0.5 - ESERCIZI AUTONOMI
# LAB05 - Metodi statici nello stesso file

## 1. Obiettivo degli esercizi
Questi esercizi servono a consolidare l'uso dei **metodi statici** nello stesso file Java.

In questa fase devi rispettare queste regole:

- usa **un solo file `.java` per ogni esercizio**;
- usa **una sola classe**;
- usa **un solo `main`**;
- crea i metodi statici sotto il `main`;
- non creare ancora classi utility separate;
- non usare ancora package multipli per questi esercizi.

L'obiettivo non è complicare la struttura del progetto, ma imparare a organizzare meglio il codice.

---

## 2. Mini guida iniziale
Ricorda questa struttura generale:

```java
import java.util.Scanner;

public class NomeClasse {

    public static void main(String[] args) {
        // flusso principale
    }

    public static tipo nomeMetodo(parametri) {
        // istruzioni
    }
}
```

### Tipi di metodi che puoi usare
#### Metodo che restituisce un valore
```java
public static double calcolaArea(double lato) {
    return lato * lato;
}
```

#### Metodo che esegue un'azione
```java
public static void stampaMessaggio(String testo) {
    System.out.println(testo);
}
```

#### Metodo che legge input
```java
public static double leggiNumero(Scanner scanner, String messaggio) {
    System.out.print(messaggio);
    return scanner.nextDouble();
}
```

---

## 3. Esercizi Base

### Esercizio Base 1 - Quadrato con metodi statici
Realizza un programma che:

- legge il lato di un quadrato;
- calcola il perimetro;
- calcola l'area;
- stampa i risultati.

### Vincoli
Crea almeno questi metodi:

- `leggiLato(...)`
- `calcolaPerimetro(...)`
- `calcolaArea(...)`
- `stampaRisultati(...)`



---

### Esercizio Base 2 - Cerchio con metodi statici
Realizza un programma che:

- legge il raggio di un cerchio;
- calcola la circonferenza;
- calcola l'area;
- stampa i risultati.

### Vincoli
Crea almeno questi metodi:

- `leggiRaggio(...)`
- `calcolaCirconferenza(...)`
- `calcolaAreaCerchio(...)`
- `stampaRisultati(...)`

Usa `Math.PI`.

### Approfondimento utile
Documentazione ufficiale di `Math`: https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/Math.html

---

## 4. Esercizi Intermedi

### Esercizio Intermedio 1 - Sconto su spesa con metodi statici
Realizza un programma che:

- legge l'importo della spesa;
- applica uno sconto del 5% se la spesa supera 100 euro;
- applica uno sconto del 10% se la spesa supera 250 euro;
- stampa importo iniziale, sconto e totale finale.

### Vincoli
Crea almeno questi metodi:

- `leggiImporto(...)`
- `calcolaSconto(...)`
- `calcolaTotaleFinale(...)`
- `stampaRiepilogo(...)`

### Suggerimento
Il metodo `calcolaSconto(...)` può restituire direttamente l'importo dello sconto, non la percentuale.



---

### Esercizio Intermedio 2 - Numeri pari tra due estremi
Realizza un programma che:

- legge due interi `a` e `b`;
- stampa tutti i numeri pari compresi tra `a` e `b`;
- conta quanti sono;
- stampa il conteggio finale.

### Vincoli
Crea almeno questi metodi:

- `leggiIntero(...)`
- `stampaPari(...)`
- `contaPari(...)`

### Nota
In questo esercizio puoi scegliere se usare `for` o `while`, ma il codice deve essere suddiviso in metodi chiari.



---

## 5. Challenge

### Challenge 1 - Calcolatrice semplice con metodi statici
Realizza una calcolatrice che:

- legge due numeri;
- legge un operatore (`+`, `-`, `*`, `/`);
- esegue l'operazione scelta;
- stampa il risultato.

### Vincoli
Crea almeno questi metodi:

- `leggiNumero(...)`
- `leggiOperatore(...)`
- `calcola(...)`
- `stampaRisultato(...)`

### Suggerimento
Nel metodo `calcola(...)` puoi usare `switch`.

### Attenzione
Gestisci il caso di divisione per zero.



---

### Challenge 2 - Media di N numeri positivi con sentinella
Realizza un programma che:

- legge numeri interi positivi;
- termina la lettura quando l'utente inserisce un valore minore o uguale a zero;
- calcola e stampa la media dei valori validi inseriti.

### Vincoli
Crea almeno questi metodi:

- `leggiIntero(...)`
- `aggiornaSomma(...)` oppure gestisci l'aggiornamento nel `main`
- `calcolaMedia(...)`
- `stampaEsito(...)`

### Nota
Questo esercizio è più impegnativo. Usalo solo se hai già svolto bene i precedenti.



---

## 6. Consegna richiesta
Per ogni esercizio svolto prepara una breve evidenza nel file:

```text
docs/evidence_lab05_esercizi.md
```

### Struttura consigliata
```markdown
# Evidence esercizi autonomi Lab05

## Elenco esercizi svolti
- Base 1
- Base 2
- Intermedio 1
- ...

## Per ogni esercizio
### Nome esercizio
### File Java creato
### Metodi statici usati
### Output di esempio
### Difficoltà incontrate
```

---

## 7. Errori comuni da controllare
Quando correggi i tuoi esercizi verifica di non aver commesso questi errori:

- hai dimenticato `static`;
- hai creato metodi che non vengono mai chiamati;
- hai scritto tutta la logica nel `main` e lasciato i metodi quasi vuoti;
- hai dichiarato un metodo `double` o `int` senza `return`;
- hai chiuso lo `Scanner` prima del tempo;
- hai dato ai metodi nomi poco chiari, come `m1`, `m2`, `prova`.

---

## 8. Checklist finale
- [ ] ogni esercizio è in un file separato;
- [ ] ogni esercizio usa una sola classe;
- [ ] ogni esercizio contiene metodi statici;
- [ ] i nomi dei metodi descrivono bene il loro compito;
- [ ] il `main` coordina il programma senza contenere tutta la logica;
- [ ] ho prodotto almeno un'evidenza sintetica per gli esercizi svolti.

---

## 9. Riferimenti utili
- Oracle Tutorials: https://docs.oracle.com/javase/tutorial/
- API `Scanner`: https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Scanner.html
- API `Math`: https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/Math.html

