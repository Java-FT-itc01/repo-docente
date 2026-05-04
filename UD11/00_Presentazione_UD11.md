# 00 - Presentazione UD11

## Prime classi, oggetti, costruttori, `this` e package

Questa unità introduce il primo passaggio serio dalla programmazione procedurale alla programmazione orientata agli oggetti.

Finora abbiamo scritto programmi centrati su:

- `main`;
- variabili;
- condizioni;
- cicli;
- metodi statici;
- input da tastiera;
- piccoli algoritmi.

Da questa UD iniziamo a ragionare in modo diverso:

```text
classe -> oggetto -> stato -> comportamento -> collaborazione tra classi
```

L'obiettivo non è ancora progettare sistemi complessi.

L'obiettivo è imparare a creare una classe, costruire oggetti da quella classe e usare metodi di istanza in modo consapevole.

---

## Perché questa UD è importante

La programmazione procedurale organizza il codice intorno alle operazioni.

La programmazione a oggetti organizza il codice intorno a elementi del problema rappresentati come oggetti.

```mermaid
flowchart LR
    A["Programmazione procedurale"] --> B["Funzioni e dati separati"]
    B --> C["Metodi statici"]
    C --> D["Variabili passate come parametri"]

    E["Programmazione a oggetti"] --> F["Classi e oggetti"]
    F --> G["Dati e comportamenti insieme"]
    G --> H["Metodi chiamati sugli oggetti"]
```

Esempio concettuale:

| Prima | Dopo |
|---|---|
| dati del libro in variabili separate | dati del libro dentro un oggetto `Libro` |
| funzione `stampaLibro(...)` | metodo `libro.stampaScheda()` |
| prezzo passato come parametro | prezzo contenuto nell'oggetto |
| programma centrato sul `main` | programma diviso in classi con responsabilità diverse |

---

## Cosa impareremo

Alla fine della UD11 dovrai saper spiegare e usare questi concetti:

```mermaid
mindmap
  root((UD11 - Primo ingresso OOP))
    Classe
      Modello
      Attributi
      Metodi
    Oggetto
      Istanza concreta
      Creato con new
      Stato proprio
    Costruttore
      Inizializza oggetto
      Stesso nome della classe
      Nessun tipo di ritorno
    this
      Oggetto corrente
      Distingue attributi e parametri
    Metodo di istanza
      Chiamato su un oggetto
      Usa lo stato dell oggetto
    Package
      Organizza le classi
      Richiede cartelle coerenti
    Compilazione multi file
      javac -d out
      java -cp out nome completo classe
```

---

## Cosa non faremo ancora

In questa UD non studieremo ancora in modo completo:

- `private`, getter e setter;
- incapsulamento completo;
- ereditarietà;
- interfacce;
- polimorfismo;
- cast tra oggetti;
- associazione, aggregazione, composizione e dipendenza UML.

Vedremo solo un uso minimo di UML per leggere la struttura di una singola classe.

Le relazioni complete tra classi saranno affrontate più avanti, quando avremo abbastanza basi per interpretarle correttamente.

---

## Mappa della giornata

```mermaid
flowchart TD
    A["Avvio UD11"] --> B["Dal procedurale agli oggetti"]
    B --> C["Classe e oggetto"]
    C --> D["Attributi e metodi di istanza"]
    D --> E["Costruttore e this"]
    E --> F["Package e struttura cartelle"]
    F --> G["Laboratorio Libro"]
    G --> H["Test di compilazione ed esecuzione"]
    H --> I["File di evidenza"]
    I --> L["Esercizi selezionati"]
    L --> M["Ponte minimo verso classi che collaborano"]
```

---

## Sequenza dei materiali

Durante la UD useremo questi file nell'ordine indicato:

| Ordine | File | Scopo |
|---:|---|---|
| 1 | `01.Dal_procedurale_agli_oggetti.md` | capire il cambio di mentalità |
| 2 | `02.Classi_oggetti_costruttori_this_package.md` | imparare la sintassi minima OOP |
| 3 | `03.LAB11_Classi_oggetti_costruttori_Libro.md` | realizzare il laboratorio principale |
| 4 | `04.Esercizi_LAB11_classi_oggetti_costruttori.md` | consolidare con esercizi mirati |
| 5 | `05.Ponte_minimo_classi_che_collaborano.md` | preparare il collegamento verso le UD successive |

---

## Da procedurale a OOP

Nel codice procedurale i dati sono spesso separati dalle operazioni.

```mermaid
flowchart LR
    A["String titolo"] --> E["stampaLibro"]
    B["String autore"] --> E
    C["double prezzo"] --> E
    D["int pagine"] --> E
    E --> F["Output"]
```

Nel codice a oggetti i dati e le operazioni vengono raccolti nella stessa classe.

```mermaid
flowchart LR
    A["Oggetto libro"] --> B["Stato"]
    A --> C["Comportamento"]

    B --> B1["titolo"]
    B --> B2["autore"]
    B --> B3["prezzo"]
    B --> B4["numeroPagine"]

    C --> C1["stampaScheda"]
    C --> C2["calcolaPrezzoScontato"]
    C --> C3["libroLungo"]
```

---

## Classe e oggetto

Una classe è un modello.

Un oggetto è un elemento concreto creato da quel modello.

```mermaid
flowchart TD
    A["Classe Libro"] --> B["Definisce attributi"]
    A --> C["Definisce metodi"]
    A --> D["Definisce costruttore"]

    A --> E["new Libro(...)"]
    E --> F["Oggetto libro1"]
    E --> G["Oggetto libro2"]
    E --> H["Oggetto libro3"]

    F --> I["Dati propri"]
    G --> L["Dati propri"]
    H --> M["Dati propri"]
```

Esempio:

```java
Libro libro1 = new Libro("Fondamenti di Java", "Mario Rossi", 39.90, 280);
Libro libro2 = new Libro("Algoritmi di base", "Anna Verdi", 49.50, 420);
```

`libro1` e `libro2` sono due oggetti distinti della stessa classe.

---

## UML minimo della classe `Libro`

In questa UD useremo UML solo per leggere una classe singola.

```mermaid
classDiagram
    class Libro {
        +String titolo
        +String autore
        +double prezzo
        +int numeroPagine
        +Libro(String titolo, String autore, double prezzo, int numeroPagine)
        +void stampaScheda()
        +double calcolaPrezzoScontato(double percentualeSconto)
        +boolean libroLungo()
    }
```

Lettura del diagramma:

| Elemento | Significato |
|---|---|
| `class Libro` | esiste una classe chiamata `Libro` |
| `String titolo` | la classe ha un attributo `titolo` |
| `Libro(...)` | la classe ha un costruttore |
| `stampaScheda()` | la classe ha un metodo di istanza |
| `calcolaPrezzoScontato(...)` | metodo che restituisce un valore numerico |
| `libroLungo()` | metodo che restituisce `true` oppure `false` |

---

## Struttura del laboratorio

Nel laboratorio realizzeremo questa struttura:

```text
lab11/
  src/
    corso/
      lab11/
        Libro.java
        AppLibro.java
  docs/
    evidence_lab11.md
```

Schema della struttura:

```mermaid
flowchart TD
    A["lab11"] --> B["src"]
    A --> C["docs"]
    B --> D["corso"]
    D --> E["lab11"]
    E --> F["Libro.java"]
    E --> G["AppLibro.java"]
    C --> H["evidence_lab11.md"]
```

---

## Ruolo delle classi nel laboratorio

```mermaid
flowchart LR
    A["AppLibro"] --> B["contiene main"]
    A --> C["crea oggetti Libro"]
    A --> D["coordina input e output"]

    E["Libro"] --> F["rappresenta il dominio"]
    E --> G["contiene attributi"]
    E --> H["contiene metodi di istanza"]
```

| Classe | Ruolo |
|---|---|
| `Libro` | rappresenta il concetto di libro nel programma |
| `AppLibro` | avvia il programma e usa gli oggetti `Libro` |

Questa distinzione è fondamentale: il `main` coordina, la classe di dominio rappresenta qualcosa del problema.

---

## Costruttore e `this`

Il costruttore inizializza l'oggetto quando viene creato.

```java
public Libro(String titolo, String autore, double prezzo, int numeroPagine) {
    this.titolo = titolo;
    this.autore = autore;
    this.prezzo = prezzo;
    this.numeroPagine = numeroPagine;
}
```

Schema del significato di `this`:

```mermaid
flowchart LR
    A["Parametro titolo"] --> B["this.titolo"]
    C["Parametro autore"] --> D["this.autore"]
    E["Parametro prezzo"] --> F["this.prezzo"]
    G["Parametro numeroPagine"] --> H["this.numeroPagine"]

    B --> I["Oggetto Libro"]
    D --> I
    F --> I
    H --> I
```

`this.titolo` indica l'attributo dell'oggetto corrente.

`titolo` indica il parametro ricevuto dal costruttore.

---

## Package e compilazione

Il package che useremo è:

```java
package corso.lab11;
```

Il package deve essere coerente con le cartelle:

```text
src/corso/lab11/
```

```mermaid
flowchart TD
    A["package corso.lab11"] --> B["src/corso/lab11"]
    B --> C["Libro.java"]
    B --> D["AppLibro.java"]
    C --> E["javac -d out"]
    D --> E
    E --> F["out/corso/lab11"]
    F --> G["Libro.class"]
    F --> H["AppLibro.class"]
```

Comando di compilazione dalla cartella `lab11`:

```bash
javac -d out src/corso/lab11/Libro.java src/corso/lab11/AppLibro.java
```

Comando di esecuzione:

```bash
java -cp out corso.lab11.AppLibro
```

Non basta scrivere:

```bash
java AppLibro
```

La classe appartiene al package `corso.lab11`, quindi va eseguita usando il nome completo.

---

## Metodo di lavoro

Durante la UD lavoreremo così:

```mermaid
flowchart TD
    A["Leggo il concetto"] --> B["Osservo esempio breve"]
    B --> C["Scrivo codice"]
    C --> D["Compilo"]
    D --> E{"Ci sono errori?"}
    E -->|"Si"| F["Leggo il messaggio"]
    F --> G["Correggo una cosa alla volta"]
    G --> D
    E -->|"No"| H["Eseguo"]
    H --> I["Testo casi previsti"]
    I --> L["Registro evidenze"]
```

Regola pratica:

```text
prima capire il modello, poi scrivere codice, poi compilare, poi testare, poi documentare
```

---

## Test obbligatori del laboratorio

Nel laboratorio dovrai verificare almeno questi casi:

| Test | Cosa verifica |
|---|---|
| compilazione senza errori | struttura file e package corretti |
| esecuzione del programma | comando `java -cp out corso.lab11.AppLibro` corretto |
| stampa dei libri predefiniti | creazione oggetti tramite costruttore |
| inserimento di un nuovo libro | input utente e creazione oggetto da dati inseriti |
| libro con almeno 300 pagine | metodo `libroLungo()` |
| prezzo non valido | gestione input numerico errato |

---

## File di evidenza

Dovrai compilare:

```text
docs/evidence_lab11.md
```

Il file dovrà contenere:

- dati partecipante;
- obiettivo del laboratorio;
- struttura del progetto;
- classi create;
- comandi usati;
- test eseguiti;
- errori incontrati;
- soluzioni adottate;
- risposte alle domande;
- conclusioni.

```mermaid
flowchart LR
    A["Codice scritto"] --> B["Compilazione"]
    B --> C["Esecuzione"]
    C --> D["Test"]
    D --> E["Errori e soluzioni"]
    E --> F["evidence_lab11.md"]
```

---

## Errori comuni da evitare

| Errore | Effetto |
|---|---|
| dimenticare `package corso.lab11;` | la classe non risulta nel package atteso |
| compilare dalla cartella sbagliata | percorsi e package diventano confusi |
| eseguire senza nome completo della classe | Java non trova `AppLibro` |
| dimenticare `new` | non viene creato l'oggetto |
| confondere attributo e parametro | il costruttore non inizializza correttamente l'oggetto |
| mettere tutto nel `main` | si perde il senso della programmazione a oggetti |

---

## Esercizi della UD

Dopo il laboratorio principale useremo alcuni esercizi di consolidamento.

```mermaid
flowchart TD
    A["Esercizi LAB11"] --> B["Base"]
    A --> C["Intermedi"]
    A --> D["Challenge"]

    B --> B1["Prodotto"]
    B --> B2["Studente"]
    B --> B3["Film"]

    C --> C1["ContoCorrente"]
    C --> C2["Auto"]
    C --> C3["Corso"]

    D --> D1["Immobile"]
    D --> D2["Mini catalogo prodotti"]
    D --> D3["Input da tastiera e oggetti"]
```

In aula verranno selezionati solo alcuni esercizi.

Gli altri servono come consolidamento autonomo.

---

## Ponte verso le prossime UD

UD11 apre la strada alle unità successive.

```mermaid
flowchart LR
    A["UD11<br/>Classi e oggetti"] --> B["UD12<br/>Incapsulamento"]
    B --> C["UD13<br/>Ereditarieta"]
    C --> D["UD14<br/>Polimorfismo e interfacce"]
    D --> E["UD15<br/>Cast e instanceof"]
    E --> F["UD16<br/>Relazioni tra classi e UML"]
```

In UD11 vediamo solo che una classe può usare un'altra classe:

```mermaid
classDiagram
    class AppLibro {
        +void main(String[] args)
    }

    class Libro {
        +String titolo
        +String autore
        +double prezzo
        +int numeroPagine
        +void stampaScheda()
    }

    AppLibro ..> Libro : crea e usa oggetti
```

Non classifichiamo ancora tutte le relazioni.

Per ora basta capire questo:

```text
un programma orientato agli oggetti è formato da classi che collaborano
```

---

## Risultato atteso della UD11

Alla fine della UD dovrai essere in grado di:

- spiegare la differenza tra classe e oggetto;
- creare una classe con attributi e metodi;
- scrivere un costruttore;
- usare `this` correttamente;
- creare oggetti con `new`;
- chiamare metodi di istanza;
- separare classe dominio e classe applicativa;
- usare un package coerente con le cartelle;
- compilare ed eseguire un progetto Java multi-file;
- documentare il lavoro nel file di evidenza.

---

## Sintesi finale

Questa UD ha un obiettivo preciso:

```text
passare dal codice centrato sulle funzioni
al codice centrato sugli oggetti
```

La sequenza da ricordare è:

```mermaid
flowchart LR
    A["Classe"] --> B["Oggetto"]
    B --> C["Stato"]
    B --> D["Comportamento"]
    C --> E["Attributi"]
    D --> F["Metodi di istanza"]
    A --> G["Costruttore"]
    G --> H["this"]
    H --> I["new"]
```

Se questi concetti diventano solidi, le prossime UD saranno molto più comprensibili.

Se restano confusi, ereditarietà, interfacce e polimorfismo saranno molto più difficili da comprendere e applicare correttamente.
