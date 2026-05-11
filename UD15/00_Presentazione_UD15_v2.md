# UD15.v2 - Cast tra oggetti, `instanceof` e refactoring polimorfico

## Durata

8 ore totali, divise in due sessioni da 4 ore:

- mattina: concetti, esempi ragionati e laboratorio guidato;
- pomeriggio: laboratorio autonomo, evidenze e revisione.

## Punto di partenza

Nella UD14.v2 sono stati introdotti polimorfismo e interfacce come strumenti per trattare oggetti diversi in modo uniforme.

In questa UD si affronta il problema complementare:

> cosa accade quando un riferimento generico non espone tutti i comportamenti disponibili sull'oggetto reale?

Esempio:

```java
AttivitaCorso attivita = new QuizValutativo("Quiz Java OO", 30, 15, 18);
```

Dal riferimento `AttivitaCorso` sono visibili solo i metodi dichiarati nella classe `AttivitaCorso`. I metodi specifici di `QuizValutativo` non sono accessibili direttamente, anche se l'oggetto reale è un `QuizValutativo`.

## Obiettivo della giornata

L'obiettivo non è imparare a usare `instanceof` in modo meccanico, ma capire quando il controllo del tipo è necessario, quando è pericoloso e quando invece indica che il modello deve essere migliorato.

## Risultati attesi

Al termine della UD15.v2 il partecipante deve essere in grado di:

### 1. Distinguere tipo del riferimento e tipo reale dell'oggetto

Dato un codice come:

```java
AttivitaCorso attivita = new LaboratorioGuidato("Lab polimorfismo", 120, true);
```

deve saper spiegare:

- qual è il tipo del riferimento;
- qual è il tipo reale dell'oggetto;
- quali metodi sono visibili in compilazione;
- quali metodi vengono risolti dinamicamente a runtime;
- quali metodi specifici non sono disponibili senza cast.

### 2. Usare upcasting e downcasting in modo corretto

Il partecipante deve saper riconoscere:

- un upcasting implicito e sicuro;
- un downcasting esplicito;
- un cast valido;
- un cast non valido;
- il rischio di `ClassCastException`.

### 3. Usare `instanceof` come controllo di sicurezza

Il partecipante deve saper proteggere un downcasting con `instanceof`:

```java
if (attivita instanceof QuizValutativo) {
    QuizValutativo quiz = (QuizValutativo) attivita;
    System.out.println(quiz.getNumeroDomande());
}
```

Deve inoltre saper spiegare perché il controllo precede il cast.

### 4. Riconoscere l'abuso di `instanceof`

Il partecipante deve saper individuare codice fragile basato su molti controlli di tipo:

```java
if (attivita instanceof LezioneTeorica) {
    // caso lezione
} else if (attivita instanceof LaboratorioGuidato) {
    // caso laboratorio
} else if (attivita instanceof QuizValutativo) {
    // caso quiz
}
```

E deve saper proporre una soluzione alternativa basata su overriding o interfacce.

### 5. Progettare metodi polimorfici per ridurre i controlli espliciti

Il partecipante deve saper spostare comportamenti comuni nelle classi corrette, evitando di concentrare tutta la logica nel `main` o in un unico metodo procedurale.

Esempio atteso:

```java
for (AttivitaCorso attivita : attivitaCorso) {
    System.out.println(attivita.descriviAttivita());
}
```

invece di:

```java
for (AttivitaCorso attivita : attivitaCorso) {
    if (attivita instanceof QuizValutativo) {
        // stampa dati quiz
    } else if (attivita instanceof LaboratorioGuidato) {
        // stampa dati laboratorio
    }
}
```

### 6. Introdurre interfacce per comportamenti opzionali

Il partecipante deve saper usare un'interfaccia per rappresentare un comportamento non comune a tutte le classi.

Esempio:

```java
public interface Valutabile {
    int getPunteggioMassimo();
    boolean isSuperato(int punteggioOttenuto);
}
```

Non tutte le attività del corso sono valutabili. Solo alcune classi implementano il contratto `Valutabile`.

### 7. Motivare le scelte progettuali

Nel file di evidenza il partecipante deve saper motivare:

- dove è stato usato il polimorfismo;
- dove è stato usato `instanceof`;
- perché il cast è sicuro;
- quale codice sarebbe fragile se gestito solo con `if` o `switch`;
- quale parte del modello è più estendibile dopo il refactoring.

## Competenze in uscita

Alla fine della UD il partecipante deve essere in grado di leggere e costruire un piccolo modello Java in cui:

- una classe astratta rappresenta una categoria generale;
- più sottoclassi specializzano il comportamento;
- alcuni comportamenti sono comuni e polimorfici;
- altri comportamenti sono opzionali e gestiti tramite interfacce;
- `instanceof` viene usato in modo limitato, motivato e sicuro.

## Collegamento con il percorso successivo

Questa UD prepara alle Collections, perché presto lo stesso problema verrà affrontato non più con array, ma con `List<AttivitaCorso>`, `List<Pubblicabile>` e strutture dati più flessibili.

Prepara inoltre ai DAO, alla Dependency Injection e a Spring, dove lavorare con tipi astratti e interfacce è una pratica ordinaria.
