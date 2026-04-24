# ESERCIZI AUTONOMI - LAB04 - Iterazione e `switch`

## 1. Scopo del file
In questo file trovi una serie di esercizi da svolgere in autonomia per consolidare:

- `switch`;
- `while`;
- `do-while`;
- `for`;
- uso di contatori e accumulatori;
- lettura di input con `Scanner`.

Gli esercizi sono divisi in tre gruppi:

- **Base**
- **Intermedi**
- **Challenge**

Per questa fase del corso mantieni una struttura semplice:

- un solo file `.java` per ogni esercizio;
- una sola classe;
- un solo metodo `main`.

---

## 2. Promemoria rapido: `Scanner`
Prima di iniziare, ricorda la struttura minima per leggere input.

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

Metodi utili in questi esercizi:

- `input.nextInt()`
- `input.nextDouble()`
- `input.next()`

---

## 3. Esercizi base

### Esercizio B1 - Calcolatrice con `switch`
Scrivi un programma che:

1. legge due numeri;
2. legge un operatore tra `+`, `-`, `*`, `/`;
3. usa `switch` per scegliere l'operazione;
4. stampa il risultato;
5. gestisce la divisione per zero.

#### Verifiche minime
Prova almeno questi casi:

- `7 + 3`
- `7 - 3`
- `7 * 3`
- `7 / 3`
- `7 / 0`
- operatore non valido

---

### Esercizio B2 - Numeri pari tra due estremi
Scrivi un programma che:

1. legge due numeri interi `a` e `b`;
2. stampa tutti i numeri pari compresi tra `a` e `b`;
3. conta quanti numeri pari ha trovato;
4. stampa il totale finale.

#### Suggerimento
Se `a > b`, puoi scambiare i valori.

---

### Esercizio B3 - Somma tra due estremi
Scrivi un programma che:

1. legge due interi `a` e `b`;
2. calcola la somma di tutti i numeri compresi tra `a` e `b`;
3. stampa il risultato.

#### Obiettivo didattico
Allenare l'uso dell'accumulatore.

---

## 4. Esercizi intermedi

### Esercizio I1 - Conteggio dei numeri maggiori e minori di una soglia
Scrivi un programma che:

1. legge un intero `n`;
2. legge un valore di riferimento `val`;
3. legge poi `n` numeri interi;
4. conta quanti numeri sono maggiori di `val`;
5. conta quanti numeri sono minori di `val`;
6. stampa i due conteggi finali.

#### Obiettivo didattico
Usare un ciclo controllato da contatore e due contatori distinti.

---

### Esercizio I2 - Input valido con `do-while`
Scrivi un programma che:

1. chiede all'utente di inserire un carattere;
2. accetta solo `s` oppure `n`;
3. se l'input non è valido, ripete la richiesta;
4. quando l'input è corretto, stampa `OK`.

#### Suggerimento
Leggi il carattere come stringa con `next()` e confronta il valore letto.

---

### Esercizio I3 - Somma e media dei primi `n` numeri pari successivi ad `a`
Scrivi un programma che:

1. legge due interi `a` e `n`;
2. considera i primi `n` numeri pari successivi ad `a`;
3. ne calcola la somma;
4. ne calcola la media;
5. stampa i risultati.

#### Esempio di ragionamento
Se `a = 3` e `n = 4`, i numeri pari successivi sono:

- 4
- 6
- 8
- 10

---

## 5. Esercizi challenge

### Esercizio C1 - Cubi dei primi `k` numeri pari
Scrivi un programma che:

1. legge un intero positivo `k`;
2. stampa i primi `k` numeri pari;
3. per ognuno stampa anche il suo cubo.

#### Esempio
Se `k = 4`, i numeri da considerare sono:

- 2
- 4
- 6
- 8

---

### Esercizio C2 - Sequenza con controllo della condizione e media finale
Scrivi un programma che:

1. legge una sequenza di interi positivi;
2. continua a leggere finché l'utente inserisce un numero positivo;
3. si ferma quando viene inserito un numero minore o uguale a zero;
4. calcola la media dei soli valori positivi inseriti;
5. stampa la media finale.

#### Obiettivo didattico
Introdurre il concetto di **controllo della condizione**, cioè un valore che segnala la fine della sequenza.

---

## 6. Consegna consigliata
Per ogni esercizio completato:

1. salva il file `.java` con un nome chiaro;
2. esegui almeno 2 o 3 test diversi;
3. documenta nel file delle evidenze:
   - input usati;
   - output ottenuti;
   - errori incontrati;
   - correzioni applicate.

Se il tuo monorepo prevede una struttura standard, aggiorna il file delle evidenze del lab oppure crea una sezione separata per gli esercizi autonomi.

---

## 7. Errori tipici da controllare
Durante lo svolgimento degli esercizi verifica con attenzione di non aver commesso questi errori:

- dimenticare `import java.util.Scanner;`
- dimenticare `break` nei `case` dello `switch`
- usare una condizione di ciclo sbagliata
- non aggiornare la variabile di controllo
- confondere contatore e accumulatore
- non gestire il caso di input non valido
- non controllare la divisione per zero

---

## 8. Ordine consigliato di svolgimento
Ti conviene seguire questo ordine:

1. B1
2. B2
3. B3
4. I1
5. I2
6. I3
7. C1
8. C2

In questo modo affronti prima i problemi più lineari e poi quelli che richiedono più controllo sul flusso.

