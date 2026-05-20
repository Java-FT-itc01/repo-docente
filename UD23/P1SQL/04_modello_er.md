# 04 - Il modello Entità-Relazione

## Obiettivi della lezione

Al termine di questa unità il partecipante deve essere in grado di:

- definire il modello Entità-Relazione;
- riconoscere entità, attributi e relazioni;
- comprendere il significato di chiave primaria nel modello concettuale;
- leggere la cardinalità di una relazione;
- interpretare un semplice modello E-R.

---

## 1. Che cos'è il modello E-R

Il **modello Entità-Relazione**, abbreviato in **modello E-R**, è uno schema concettuale usato nella progettazione dei database.

Serve a rappresentare:

- le **entità** del dominio;
- le **caratteristiche** delle entità, cioè gli attributi;
- le **relazioni** tra entità.

Il modello E-R appartiene alla fase di **progettazione concettuale**.

```mermaid
flowchart TD
    PC["Progettazione concettuale"] --> ER["Modello E-R"]
    ER --> E["Entità"]
    ER --> A["Attributi"]
    ER --> R["Relazioni"]
```

---

## 2. Entità

Un'**entità** è un gruppo omogeneo di informazioni che descrive un oggetto, una persona, un evento o un concetto rilevante per il sistema.

Esempi di entità:

- Libro;
- Lettore;
- Scaffale;
- Cliente;
- Fornitore;
- Prodotto.

Un'entità individuata nel modello E-R potrà diventare una tabella nel modello relazionale.

---

## 3. Attributi

Gli **attributi** sono le caratteristiche che descrivono un'entità.

Esempio: l'entità `LIBRO` può avere attributi come codice, ISBN, titolo, genere, autore, editore ed edizione.

```mermaid
flowchart TD
    LIBRO["Entità: LIBRO"]
    COD(("CODICE_LIBRO - PK")) --> LIBRO
    ISBN(("CODICE_ISBN")) --> LIBRO
    TITOLO(("TITOLO")) --> LIBRO
    GENERE(("GENERE")) --> LIBRO
    AUTORE(("AUTORE")) --> LIBRO
    EDITORE(("EDITORE")) --> LIBRO
    EDIZIONE(("EDIZIONE")) --> LIBRO
```

### Chiave primaria

Un attributo che non accetta duplicati e identifica univocamente un elemento può diventare **chiave primaria**.

Nel caso dell'entità `LIBRO`, `CODICE_LIBRO` può essere usato come chiave primaria.

---

## 4. Relazioni

Una **relazione** rappresenta un'associazione significativa tra due o più entità.

Esempio: un lettore può prendere in prestito un libro.

```mermaid
flowchart LR
    LIBRO["LIBRO"] --> PRESTITO{"PRESTITO"}
    PRESTITO --> LETTORE["LETTORE"]
```

La relazione `PRESTITO` collega l'entità `LIBRO` all'entità `LETTORE`.

---

## 5. Esempio: Libro e Lettore

```mermaid
flowchart TD
    subgraph S1["Entità LIBRO"]
        L["LIBRO"]
        L1(("CODICE_LIBRO - PK")) --> L
        L2(("CODICE_ISBN")) --> L
        L3(("TITOLO")) --> L
        L4(("GENERE")) --> L
        L5(("AUTORE")) --> L
        L6(("EDITORE")) --> L
        L7(("EDIZIONE")) --> L
    end

    subgraph S2["Entità LETTORE"]
        LT["LETTORE"]
        LT1(("CODICE_LETTORE - PK")) --> LT
        LT2(("NOME")) --> LT
        LT3(("COGNOME")) --> LT
        LT4(("SESSO")) --> LT
        LT5(("ETA")) --> LT
        LT6(("CODICE_FISCALE")) --> LT
        LT7(("INDIRIZZO")) --> LT
        LT8(("CONTATTI")) --> LT
        LT9(("TITOLO_DI_STUDIO")) --> LT
    end

    L --> P{"PRESTITO"}
    P --> LT
    P --> P1(("DATA_PRES"))
    P --> P2(("DATA_REST"))
```

---

## 6. Simboli del modello E-R

Il modello E-R classico usa simboli grafici specifici. Nel diagramma seguente sono rappresentati con forme equivalenti.

```mermaid
flowchart TD
    E["Entità"]
    R{"Relazione"}
    A(("Attributo"))
    AM((("Attributo multivalore")))
    AC(("Attributo composto"))
    PK(("Attributo identificativo / PK"))

    E --- R
    E --- A
    E --- AM
    E --- AC
    E --- PK
```

### Lettura dei simboli

| Concetto | Significato |
|---|---|
| Entità | Oggetto principale da rappresentare |
| Relazione | Associazione tra entità |
| Attributo | Caratteristica dell'entità o della relazione |
| Attributo multivalore | Attributo che può avere più valori, ad esempio più recapiti |
| Attributo composto | Attributo scomponibile, ad esempio indirizzo in via, CAP, città |
| Attributo identificativo | Attributo che non accetta duplicati |

---

## 7. Specifiche funzionali dell'esempio Database Libri

Il database dell'esempio deve permettere di:

1. caricare i libri nella posizione identificata dallo scaffale;
2. scaricare dallo scaffale i libri prestati ai lettori;
3. individuare il lettore a cui è stato prestato un libro;
4. ottenere un elenco dei libri prestati, dei lettori coinvolti e delle date di restituzione.

---

## 8. Modello E-R dell'esempio Biblioteca

Il seguente schema rappresenta il modello E-R rielaborato dell'esempio.

```mermaid
flowchart LR
    SCAFFALE["SCAFFALE"]
    LIBRO["LIBRO"]
    LETTORE["LETTORE"]

    CSS{"CARICO_SCARICO_SCAFFALE"}
    PRESTITO{"PRESTITO"}

    SCAFFALE --- CSS
    CSS --- LIBRO
    LIBRO --- PRESTITO
    PRESTITO --- LETTORE

    S1(("PROG_SCAFFALE - PK")) --- SCAFFALE
    S2(("STANZA")) --- SCAFFALE
    S3(("PIANO")) --- SCAFFALE

    CSS1(("CODICE_LIBRO")) --- CSS
    CSS2(("PROG_SCAFFALE")) --- CSS
    CSS3(("PROGRESSIVO")) --- CSS
    CSS4(("STATO")) --- CSS
    CSS5(("DATA_OPERAZIONE")) --- CSS

    L1(("CODICE_LIBRO - PK")) --- LIBRO
    L2(("CODICE_ISBN")) --- LIBRO
    L3(("TITOLO")) --- LIBRO
    L4(("GENERE")) --- LIBRO
    L5(("AUTORE")) --- LIBRO
    L6(("EDITORE")) --- LIBRO
    L7(("EDIZIONE")) --- LIBRO

    P1(("CODICE_LIBRO")) --- PRESTITO
    P2(("CODICE_LETTORE")) --- PRESTITO
    P3(("DATA_PRES")) --- PRESTITO
    P4(("DATA_REST")) --- PRESTITO

    LT1(("CODICE_LETTORE - PK")) --- LETTORE
    LT2(("NOME")) --- LETTORE
    LT3(("COGNOME")) --- LETTORE
    LT4(("SESSO")) --- LETTORE
    LT5(("ETA")) --- LETTORE
    LT6(("CODICE_FISCALE")) --- LETTORE
    LT7(("TITOLO_DI_STUDIO")) --- LETTORE
    LT8((("CONTATTI"))) --- LETTORE
    LT9(("INDIRIZZO")) --- LETTORE
```

---

## 9. Cardinalità delle relazioni

Quando si indica una relazione tra due entità, bisogna specificare la **cardinalità**.

La cardinalità indica quante volte un'istanza di un'entità può partecipare a una relazione.

La forma usata è:

```text
(Min, Max)
```

Dove:

- `Min` indica la partecipazione minima;
- `Max` indica la partecipazione massima.

```mermaid
flowchart LR
    E1["Entità A"] -- "(Min, Max)" --> R{"Relazione"}
    R -- "(Min, Max)" --> E2["Entità B"]
```

---

## 10. Significato dei valori più comuni

| Cardinalità | Significato |
|---|---|
| `(1,1)` | Obbligatoria, una sola volta |
| `(1,N)` | Obbligatoria, una o più volte |
| `(0,1)` | Opzionale, al massimo una volta |
| `(0,N)` | Opzionale, zero o più volte |

---

## 11. Tipi principali di relazione

Osservando i valori massimi delle cardinalità si ottengono tre casi principali:

- uno a uno;
- uno a molti;
- molti a molti.

```mermaid
flowchart TD
    REL["Relazioni"]
    REL --> U1["Uno a uno"]
    REL --> UM["Uno a molti"]
    REL --> MM["Molti a molti"]
```

---

## 12. Relazione uno a uno

Esempio: un libro ha un solo codice ISBN e un codice ISBN identifica un solo libro.

```mermaid
erDiagram
    LIBRO ||--|| CODICE_ISBN : possiede
    LIBRO {
        int codice_libro PK
        string titolo
    }
    CODICE_ISBN {
        string codice_isbn PK
    }
```

---

## 13. Relazione molti a molti

Esempio: un libro può essere preso in prestito da lettori diversi nel tempo, e un lettore può prendere in prestito libri diversi.

```mermaid
erDiagram
    LIBRO }o--o{ LETTORE : prestito
    LIBRO {
        int codice_libro PK
        string titolo
    }
    LETTORE {
        int codice_lettore PK
        string nome
        string cognome
    }
```

Nel modello relazionale questa relazione dovrà diventare una tabella intermedia, ad esempio `PRESTITO`.

---

## 14. Relazione uno a molti

Esempio: un autore può scrivere molti libri, mentre un libro dell'esempio è associato a un solo autore.

```mermaid
erDiagram
    AUTORE ||--o{ LIBRO : scrive
    AUTORE {
        int id_autore PK
        string autore
    }
    LIBRO {
        int codice_libro PK
        string titolo
        int id_autore FK
    }
```

---

## Sintesi finale

Il modello E-R permette di descrivere un database prima della sua realizzazione tecnica. È utile perché separa il ragionamento concettuale dalla costruzione fisica delle tabelle. Prima si capisce cosa esiste nel dominio, poi si decide come trasformarlo in strutture relazionali.
