# 00 - Presentazione UD16

# Relazioni tra classi in Java

## Programma della giornata

Questa unità conclude il primo blocco OOP spostando l'attenzione dalla singola classe al modello formato da più classi collegate.

Finora avete lavorato su:

- classi e oggetti;
- incapsulamento;
- ereditarietà;
- polimorfismo;
- interfacce;
- cast e `instanceof`.

In questa UD userete questi concetti per modellare un piccolo dominio composto da oggetti che collaborano.

Il punto centrale non è avere molte classi.

Il punto centrale è saper spiegare perché quelle classi sono collegate in quel modo.

---

## Obiettivo della UD16

Alla fine della giornata dovrete saper distinguere e motivare quattro relazioni fondamentali:

| Relazione | Formula mentale | Esempio della UD |
|---|---|---|
| Associazione | due oggetti sono collegati | `Iscrizione` collega `Studente` e `Corso` |
| Aggregazione | un oggetto raggruppa parti autonome | `Corso` aggrega `Studente` |
| Composizione | un oggetto è composto da parti interne | `Corso` compone `Lezione` |
| Dipendenza | una classe usa un'altra classe temporaneamente | `GestioneCorsi` usa `Corso` come parametro |

---

## Mappa della giornata

```mermaid
flowchart TD
    A["UD16 - Relazioni tra classi"] --> B["Modello mentale"]
    B --> C["Associazione"]
    B --> D["Aggregazione"]
    B --> E["Composizione"]
    B --> F["Dipendenza"]

    C --> G["Classe Iscrizione"]
    D --> H["Corso e Studente"]
    E --> I["Corso e Lezione"]
    F --> L["GestioneCorsi"]

    G --> M["LAB16"]
    H --> M
    I --> M
    L --> M

    M --> N["Esercizi guidati"]
    N --> O["File di evidenza"]
```

---

## Da classi isolate a classi collegate

Nei laboratori precedenti una classe poteva essere studiata quasi da sola.

Esempi:

```text
Libro
Prodotto
Studente
Immobile
```

Ora il modello diventa più realistico:

```text
uno Studente si iscrive a un Corso
un Corso contiene Lezioni
una Iscrizione collega Studente e Corso
una classe GestioneCorsi usa oggetti del dominio
```

La domanda cambia.

Prima:

```text
come è fatta questa classe?
```

Ora:

```text
che relazione ha questa classe con le altre?
```

---

## Le classi del laboratorio

Nel laboratorio principale costruirete questo dominio:

```mermaid
classDiagram
    class Studente {
        -String matricola
        -String nome
        -String cognome
        +toString() String
    }

    class Lezione {
        -String titolo
        -int durataMinuti
        +toString() String
    }

    class Corso {
        -String codice
        -String titolo
        -ArrayList~Studente~ studenti
        -ArrayList~Lezione~ lezioni
        +iscriviStudente(Studente studente) void
        +aggiungiLezione(String titolo, int durataMinuti) void
        +calcolaDurataTotaleMinuti() int
        +toString() String
    }

    class Iscrizione {
        -Studente studente
        -Corso corso
        -String dataIscrizione
        +toString() String
    }

    class GestioneCorsi {
        +stampaCorso(Corso corso) void
        +stampaIscrizione(Iscrizione iscrizione) void
        +trovaStudentePerMatricola(ArrayList~Studente~ studenti, String matricola) Studente
    }

    Corso o-- "0..*" Studente : aggrega
    Corso *-- "1..*" Lezione : compone
    Iscrizione --> Studente : collega
    Iscrizione --> Corso : collega
    GestioneCorsi ..> Corso : usa
    GestioneCorsi ..> Iscrizione : usa
    GestioneCorsi ..> Studente : usa
```

---

## Associazione

Una associazione indica che due oggetti sono collegati.

Nel laboratorio userete una associazione esplicita:

```text
Iscrizione collega Studente e Corso
```

La relazione diventa una classe perché contiene un dato proprio:

```text
dataIscrizione
```

```mermaid
classDiagram
    class Studente
    class Corso
    class Iscrizione {
        -Studente studente
        -Corso corso
        -String dataIscrizione
    }

    Iscrizione --> Studente
    Iscrizione --> Corso
```

Domanda guida:

```text
la relazione ha informazioni proprie?
```

---

## Aggregazione

L'aggregazione è una relazione tutto-parte debole.

Nel laboratorio:

```text
Corso aggrega Studente
```

Uno studente può esistere anche fuori da quel corso.

```mermaid
classDiagram
    class Corso {
        -ArrayList~Studente~ studenti
        +iscriviStudente(Studente studente) void
    }

    class Studente

    Corso o-- "0..*" Studente : aggregazione
```

Domanda guida:

```text
la parte può vivere anche senza il tutto?
```

---

## Composizione

La composizione è una relazione tutto-parte più forte.

Nel laboratorio:

```text
Corso compone Lezione
```

Nel modello scelto, le lezioni sono parti interne del corso.

```mermaid
classDiagram
    class Corso {
        -ArrayList~Lezione~ lezioni
        +aggiungiLezione(String titolo, int durataMinuti) void
    }

    class Lezione

    Corso *-- "1..*" Lezione : composizione
```

Domanda guida:

```text
la parte è interna al tutto nel modello che sto costruendo?
```

---

## Dipendenza

La dipendenza è una relazione più debole.

Una classe usa un'altra classe per svolgere un'operazione, ma non la conserva come attributo.

Nel laboratorio:

```java
public void stampaCorso(Corso corso) {
    System.out.println(corso);
}
```

```mermaid
classDiagram
    class GestioneCorsi {
        +stampaCorso(Corso corso) void
        +stampaIscrizione(Iscrizione iscrizione) void
    }

    class Corso
    class Iscrizione

    GestioneCorsi ..> Corso : parametro
    GestioneCorsi ..> Iscrizione : parametro
```

Domanda guida:

```text
la classe usa l'altra solo per eseguire un'operazione?
```

---

## Schema decisionale

```mermaid
flowchart TD
    A["Due classi sono collegate"] --> B{"Una classe usa l'altra solo dentro un metodo?"}
    B -->|"Si"| C["Dipendenza"]
    B -->|"No"| D{"Una classe conserva l'altra come attributo o lista?"}

    D -->|"No"| E["Nessuna relazione stabile"]
    D -->|"Si"| F{"La relazione ha dati propri?"}

    F -->|"Si"| G["Associazione esplicita"]
    F -->|"No"| H{"E' una relazione tutto-parte?"}

    H -->|"No"| I["Associazione semplice"]
    H -->|"Si"| L{"La parte vive autonomamente?"}

    L -->|"Si"| M["Aggregazione"]
    L -->|"No"| N["Composizione"]
```

---

## `toString()` nella UD16

Quando si stampano oggetti collegati, `toString()` rende l'output leggibile.

```mermaid
flowchart LR
    A["Oggetto Java"] --> B{"toString ridefinito?"}
    B -->|"No"| C["Output poco utile"]
    B -->|"Si"| D["Output leggibile"]
```

---

## Struttura del laboratorio

```text
lab16/
  src/
    corso/
      lab16/
        Studente.java
        Lezione.java
        Corso.java
        Iscrizione.java
        GestioneCorsi.java
        MainLab16.java
  docs/
    evidence_lab16.md
```

---

## Comandi fondamentali

Dalla cartella `lab16`, compilate con:

```bash
javac -d out src/corso/lab16/*.java
```

Eseguite con:

```bash
java -cp out corso.lab16.MainLab16
```

---

## Flusso operativo del laboratorio

```mermaid
flowchart TD
    A["Creare struttura lab16"] --> B["Creare Studente"]
    B --> C["Creare Lezione"]
    C --> D["Creare Corso"]
    D --> E["Aggiungere studenti e lezioni"]
    E --> F["Creare Iscrizione"]
    F --> G["Creare GestioneCorsi"]
    G --> H["Creare MainLab16"]
    H --> I["Compilare"]
    I --> L["Eseguire"]
    L --> M["Compilare evidence_lab16.md"]
```

---

## Test obbligatori

| Test | Verifica |
|---|---|
| Compilazione | comando `javac` senza errori |
| Esecuzione | avvio di `MainLab16` |
| Aggregazione | stampa degli studenti del corso |
| Composizione | stampa delle lezioni del corso |
| Associazione | stampa delle iscrizioni |
| Dipendenza | uso di oggetti passati come parametri |
| Ricerca | matricola esistente e non esistente |

---

## File di evidenza

Create il file:

```text
docs/evidence_lab16.md
```

Struttura consigliata:

```md
# Evidence LAB16

## Dati partecipante

Nome:
Data:

## Obiettivo del laboratorio

## Classi create

## Relazioni modellate

### Associazione

### Aggregazione

### Composizione

### Dipendenza

## Comandi di compilazione

## Comandi di esecuzione

## Output osservato

## Risposte alle domande

## Conclusioni
```

Il file di evidenza deve spiegare il modello, non solo dimostrare che il codice esegue.

---

## Esercizi consigliati in aula

```text
Esercizio 1 - Studente e corso
Esercizio 2 - Auto e motore
Esercizio 4 - Ordine e righe ordine
```

Se il gruppo procede bene:

```text
Esercizio 5 - Biblioteca e prestiti
```

---

## Collegamento con le prossime unità

UD16 prepara argomenti successivi come:

- collezioni di oggetti;
- CRUD in memoria;
- repository;
- DAO;
- progettazione OOP più strutturata.

```mermaid
flowchart LR
    A["UD16 Relazioni tra classi"] --> B["Collezioni di oggetti"]
    B --> C["CRUD in memoria"]
    C --> D["Repository e DAO"]
    D --> E["Progettazione OOP strutturata"]
```

---

## Sintesi finale

La frase da ricordare è:

```text
le classi collaborano perché il dominio lo richiede
```

In UD16 non si valuta solo il codice.

Si valuta la capacità di motivare il modello.
