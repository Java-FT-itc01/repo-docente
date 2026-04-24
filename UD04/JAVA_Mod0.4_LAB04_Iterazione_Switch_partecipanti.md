# LAB04 - Iterazione e `switch`

## 1. Titolo del laboratorio
**Iterazione e `switch`: ripetere istruzioni, contare, accumulare, validare scelte**

---

## 2. Obiettivi del laboratorio
In questo laboratorio imparerai a:

- usare `switch` per scegliere tra più alternative;
- usare i cicli `while`, `do-while` e `for`;
- distinguere tra:
  - **contatore**;
  - **accumulatore**;
  - **condizione di arresto**;
  - **sentinella**;
- evitare alcuni errori tipici dei cicli, come:
  - ciclo infinito;
  - condizione sbagliata;
  - contatore non aggiornato;
  - uso scorretto degli estremi;
- scrivere piccoli programmi completi che leggono input, eseguono controlli e producono un risultato.

---

## 3. Prerequisiti
Prima di iniziare dovresti avere già compreso:

- struttura minima di un programma Java;
- uso di `Scanner` per leggere input;
- variabili e tipi primitivi;
- operatori aritmetici;
- operatori relazionali e logici;
- strutture `if`, `if/else`, `if / else if / else`.

Se questi punti non ti sono chiari, ripassa i laboratori precedenti prima di proseguire.

---

## 4. Software necessario
Per questo laboratorio ti servono:

- **Visual Studio Code**;
- **JDK** installato e funzionante;
- estensioni Java per VS Code;
- terminale integrato di VS Code oppure prompt dei comandi / terminale di sistema.

---

## 5. Verifica dell'ambiente
Apri il terminale e verifica:

```bash
java -version
javac -version
```

Se i comandi restituiscono la versione del runtime e del compilatore, puoi iniziare.

---

## 6. Concetti da comprendere prima di scrivere codice

### 6.1 Quando serve un ciclo
Un ciclo serve quando devi ripetere una o più istruzioni.

Esempi tipici:

- stampare i numeri da 1 a 10;
- sommare i numeri compresi tra due estremi;
- leggere più valori dallo stesso utente;
- ripetere una richiesta finché l'input non è valido.

---

### 6.2 Contatore e accumulatore
Durante i cicli compaiono spesso due ruoli diversi.

#### Contatore
Tiene traccia di **quante volte** è avvenuto qualcosa.

Esempio:

```java
int contatore = 0;
```

Poi, quando trovi un numero pari:

```java
contatore = contatore + 1;
```

#### Accumulatore
Serve a costruire progressivamente un totale.

Esempio:

```java
int somma = 0;
```

Poi, se leggi un nuovo numero:

```java
somma = somma + numero;
```

---

### 6.3 Il ciclo `while`
Il ciclo `while` controlla la condizione **prima** di entrare nel blocco.

Schema generale:

```java
while (condizione) {
    // istruzioni da ripetere
}
```

Se la condizione è falsa subito, il blocco non viene eseguito neanche una volta.

---

### 6.4 Il ciclo `do-while`
Il ciclo `do-while` esegue prima il blocco e controlla la condizione **dopo**.

Schema generale:

```java
do {
    // istruzioni da ripetere
} while (condizione);
```

Quindi il blocco viene eseguito **almeno una volta**.

---

### 6.5 Il ciclo `for`
Il ciclo `for` è molto utile quando sai già come si comportano:

- inizializzazione;
- condizione;
- aggiornamento.

Schema generale:

```java
for (inizializzazione; condizione; aggiornamento) {
    // istruzioni da ripetere
}
```

Esempio:

```java
for (int i = 1; i <= 10; i++) {
    System.out.println(i);
}
```

---

### 6.6 La struttura `switch`
`switch` è utile quando una stessa variabile può assumere più valori distinti e ad ogni valore vuoi associare un comportamento.

Schema generale:

```java
switch (scelta) {
    case 1:
        // istruzioni
        break;
    case 2:
        // istruzioni
        break;
    default:
        // caso non previsto
}
```

Ricorda:

- `case` definisce un caso specifico;
- `break` interrompe lo `switch`;
- `default` gestisce i casi non previsti.

---

## 7. Attività guidata
In questo laboratorio svolgerai due attività guidate.

### Attività 1 - Calcolatrice semplice con `switch`
Realizzerai un programma che:

1. chiede due numeri;
2. chiede un'operazione tra `+`, `-`, `*`, `/`;
3. esegue l'operazione scelta;
4. stampa il risultato.

### Attività 2 - Numeri pari e conteggio tra due estremi
Realizzerai un programma che:

1. legge due interi `a` e `b`;
2. se `a > b`, li scambia oppure segnala l'errore;
3. stampa tutti i numeri pari compresi tra `a` e `b`;
4. conta quanti numeri pari sono stati trovati;
5. stampa il conteggio finale.

---

## 8. Preparazione del file Java
Per questo laboratorio usa ancora una struttura semplice:

- un solo file `.java`;
- una sola classe;
- un solo metodo `main`.

Crea un file chiamato ad esempio:

```text
Lab04IterazioneSwitch.java
```

Struttura iniziale:

```java
import java.util.Scanner;

public class Lab04IterazioneSwitch {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        // qui inserirai il codice del laboratorio
    }
}
```

---

## 9. Attività 1 - Calcolatrice semplice con `switch`

### 9.1 Leggere i dati
Inserisci nel `main` il codice per leggere:

- primo numero;
- secondo numero;
- operatore.

Esempio:

```java
System.out.print("Inserisci il primo numero: ");
double numero1 = input.nextDouble();

System.out.print("Inserisci il secondo numero: ");
double numero2 = input.nextDouble();

System.out.print("Inserisci l'operazione (+, -, *, /): ");
String operazione = input.next();
```

### 9.2 Gestire l'operazione con `switch`
Usa `switch` sul valore di `operazione`.

Struttura consigliata:

```java
switch (operazione) {
    case "+":
        System.out.println("Risultato: " + (numero1 + numero2));
        break;
    case "-":
        System.out.println("Risultato: " + (numero1 - numero2));
        break;
    case "*":
        System.out.println("Risultato: " + (numero1 * numero2));
        break;
    case "/":
        if (numero2 != 0) {
            System.out.println("Risultato: " + (numero1 / numero2));
        } else {
            System.out.println("Errore: divisione per zero non consentita.");
        }
        break;
    default:
        System.out.println("Operazione non valida.");
}
```

### 9.3 Cosa controllare
Verifica almeno questi casi:

- `10 + 5`
- `10 - 5`
- `10 * 5`
- `10 / 5`
- `10 / 0`
- operatore non valido, ad esempio `%`

---

## 10. Attività 2 - Numeri pari tra due estremi

### 10.1 Leggere gli estremi
Dopo l'attività precedente, oppure in una nuova versione del file, leggi due interi:

```java
System.out.print("Inserisci il valore iniziale: ");
int a = input.nextInt();

System.out.print("Inserisci il valore finale: ");
int b = input.nextInt();
```

### 10.2 Gestire il caso in cui `a > b`
Hai due possibilità.

#### Soluzione A - scambio dei valori

```java
if (a > b) {
    int temp = a;
    a = b;
    b = temp;
}
```

#### Soluzione B - messaggio di errore

```java
if (a > b) {
    System.out.println("Intervallo non valido: il primo numero deve essere minore o uguale al secondo.");
}
```

Per questo laboratorio è preferibile la **soluzione A**, perché ti permette comunque di proseguire.

### 10.3 Costruire il ciclo `for`
Inizializza un contatore:

```java
int contatorePari = 0;
```

Poi usa un `for` per attraversare tutti i numeri compresi tra `a` e `b`.

```java
for (int i = a; i <= b; i++) {
    if (i % 2 == 0) {
        System.out.println(i);
        contatorePari = contatorePari + 1;
    }
}
```

### 10.4 Stampare il conteggio finale
Alla fine del ciclo:

```java
System.out.println("Totale numeri pari trovati: " + contatorePari);
```

### 10.5 Cosa controllare
Prova almeno questi casi:

- `a = 1`, `b = 10`
- `a = 4`, `b = 4`
- `a = 10`, `b = 1`
- `a = -4`, `b = 6`

---

## 11. Estensione consigliata
Se completi le due attività senza difficoltà, aggiungi una terza parte.

### Attività 3 - Somma tra due estremi
Leggi due interi `a` e `b` e calcola la somma di tutti i numeri compresi tra loro.

Suggerimento:

```java
int somma = 0;

for (int i = a; i <= b; i++) {
    somma = somma + i;
}

System.out.println("Somma totale: " + somma);
```

Qui il ruolo della variabile `somma` è quello di **accumulatore**.

---

## 12. Consegna richiesta
Prepara le evidenze del laboratorio nel file previsto dal tuo monorepo, ad esempio:

```text
docs/evidence_lab04.md
```

Inserisci almeno queste sezioni:

### 12.1 Obiettivo del laboratorio
Spiega con parole tue che cosa hai realizzato.

### 12.2 Ambiente utilizzato
Indica:

- sistema operativo;
- JDK;
- editor usato;
- modalità di esecuzione.

### 12.3 Attività svolte
Descrivi sinteticamente:

- programma 1: calcolatrice con `switch`;
- programma 2: numeri pari tra due estremi;
- eventuale estensione sulla somma.

### 12.4 Test eseguiti
Riporta gli input usati e gli output ottenuti.

### 12.5 Problemi incontrati
Esempi:

- `switch` scritto con sintassi errata;
- `break` dimenticato;
- ciclo che non termina;
- errore nei limiti del `for`.

### 12.6 Soluzione adottata
Spiega come hai corretto il problema.

### 12.7 Conclusione
Scrivi che cosa hai capito meglio grazie a questo laboratorio.

---

## 13. Errori comuni da evitare

### 13.1 Dimenticare `break` nello `switch`
Se dimentichi `break`, il programma può continuare anche nel caso successivo.

### 13.2 Confondere `=` con `==`
Ricorda:

- `=` assegna;
- `==` confronta.

### 13.3 Scrivere una condizione che non cambia mai
Esempio tipico:

```java
int i = 1;
while (i <= 10) {
    System.out.println(i);
}
```

Qui `i` non cambia mai, quindi il ciclo non termina.

### 13.4 Sbagliare gli estremi del `for`
Controlla sempre se vuoi includere oppure escludere il valore finale.

- `i <= b` include `b`
- `i < b` esclude `b`

### 13.5 Dimenticare il controllo sulla divisione per zero
La divisione richiede sempre attenzione.

---

## 14. Checklist finale
Prima di considerare concluso il laboratorio verifica di aver:

- creato un file Java corretto;
- importato `Scanner`;
- letto input numerici e testuali;
- usato almeno uno `switch`;
- usato almeno un ciclo `for`;
- usato una condizione `if` dentro un ciclo;
- distinto tra contatore e accumulatore;
- salvato le evidenze nel file richiesto.

---

## 15. Approfondimenti consigliati
Se vuoi ripassare o approfondire, puoi consultare la documentazione ufficiale Java sui controlli di flusso e sulle classi della libreria standard.

