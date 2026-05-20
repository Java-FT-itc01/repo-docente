# 05 - Il modello relazionale

## Obiettivi della lezione

Al termine di questa unità il partecipante deve essere in grado di:

- spiegare il passaggio dal modello E-R al modello relazionale;
- trasformare entità, attributi e relazioni in tabelle e colonne;
- comprendere il concetto di normalizzazione;
- leggere un modello relazionale di base.

---

## 1. Dal modello E-R al modello relazionale

Il **modello relazionale** deriva dalla trasformazione del modello Entità-Relazione in una struttura composta da tabelle.

```mermaid
flowchart LR
    ER["Modello E-R<br/>Schema concettuale"] --> TR["Trasformazione"]
    TR --> MR["Modello relazionale<br/>Schema logico"]
```

Il modello E-R descrive il dominio in modo concettuale. Il modello relazionale prepara la struttura logica che poi potrà essere implementata in un DBMS.

---

## 2. Regole di trasformazione principali

Per trasformare un modello E-R in modello relazionale si applicano alcune regole.

```mermaid
flowchart TD
    ER["Modello E-R"] --> R1["Entità -> Tabelle"]
    ER --> R2["Attributi -> Colonne"]
    ER --> R3["Attributi identificativi -> PK o UNIQUE"]
    ER --> R4["Relazioni -> FK o tabelle associative"]
    ER --> R5["Attributi multivalore -> nuove tabelle"]
    ER --> R6["Valori ripetuti -> tabelle tipizzate"]
    R1 --> MR["Modello relazionale"]
    R2 --> MR
    R3 --> MR
    R4 --> MR
    R5 --> MR
    R6 --> MR
```

---

## 3. Entità e attributi

Un'entità diventa una tabella. Gli attributi diventano colonne.

Esempio: entità `LIBRO`.

```mermaid
flowchart LR
    subgraph ER["Modello E-R"]
        LIBRO["Entità: LIBRO"]
        A1(("CODICE_LIBRO")) --> LIBRO
        A2(("CODICE_ISBN")) --> LIBRO
        A3(("TITOLO")) --> LIBRO
        A4(("GENERE")) --> LIBRO
        A5(("EDITORE")) --> LIBRO
        A6(("AUTORE")) --> LIBRO
        A7(("EDIZIONE")) --> LIBRO
    end

    ER --> TR["Trasformazione"]

    subgraph MR["Modello relazionale"]
        T["Tabella LIBRI"]
        C1["CODICE_LIBRO PK"]
        C2["CODICE_ISBN"]
        C3["TITOLO"]
        C4["GENERE"]
        C5["EDITORE"]
        C6["AUTORE"]
        C7["EDIZIONE"]
    end
```

Tabella iniziale:

| LIBRI |
|---|
| CODICE_LIBRO PK |
| CODICE_ISBN |
| TITOLO |
| GENERE |
| EDITORE |
| AUTORE |
| EDIZIONE |

---

## 4. Normalizzazione

La **normalizzazione** serve a organizzare le tabelle in modo da ridurre duplicazioni, anomalie e incoerenze.

Le regole introduttive più importanti sono:

1. ogni tabella deve avere una colonna, o un insieme di colonne, che renda univoco ogni record;
2. i dati ripetuti devono essere spostati in tabelle separate;
3. i campi calcolati non dovrebbero essere memorizzati se possono essere ricavati da altri dati.

```mermaid
flowchart TD
    T["Tabella non normalizzata"] --> R1["Individuare la chiave"]
    R1 --> R2["Eliminare dati ridondanti"]
    R2 --> R3["Separare valori ripetuti"]
    R3 --> R4["Evitare campi calcolati"]
    R4 --> TN["Tabelle normalizzate"]
```

---

## 5. Esempio di normalizzazione: libri, generi, editori, autori

Nella tabella `LIBRI`, valori come genere, editore e autore possono ripetersi molte volte.

Per ridurre la ridondanza, questi valori vengono spostati in tabelle tipizzate.

```mermaid
erDiagram
    GENERI ||--o{ LIBRI : classifica
    EDITORI ||--o{ LIBRI : pubblica
    AUTORI ||--o{ LIBRI : scrive

    LIBRI {
        int codice_libro PK
        string codice_isbn
        string titolo
        int id_genere FK
        int id_editore FK
        int id_autore FK
        string edizione
    }

    GENERI {
        int id PK
        string genere
    }

    EDITORI {
        int id PK
        string editore
    }

    AUTORI {
        int id PK
        string autore
    }
```

### Prima della normalizzazione

| codice_libro | titolo | genere | editore | autore |
|---:|---|---|---|---|
| 1 | Libro A | Informatica | Editore X | Autore 1 |
| 2 | Libro B | Informatica | Editore X | Autore 2 |
| 3 | Libro C | Romanzo | Editore Y | Autore 1 |

### Dopo la normalizzazione

`LIBRI` contiene chiavi esterne verso `GENERI`, `EDITORI` e `AUTORI`.

| codice_libro | titolo | id_genere | id_editore | id_autore |
|---:|---|---:|---:|---:|
| 1 | Libro A | 1 | 1 | 1 |
| 2 | Libro B | 1 | 1 | 2 |
| 3 | Libro C | 2 | 2 | 1 |

---

## 6. Relazioni che diventano tabelle

Alcune relazioni del modello E-R devono diventare tabelle nel modello relazionale.

Esempio: la relazione `CARICO_SCARICO_SCAFFALE` collega libri e scaffali e possiede attributi propri, come stato, progressivo e data operazione.

```mermaid
erDiagram
    LIBRI ||--o{ CARICO_SCARICO_SCAFFALE : movimenta
    SCAFFALE ||--o{ CARICO_SCARICO_SCAFFALE : contiene

    LIBRI {
        int codice_libro PK
        string titolo
    }

    SCAFFALE {
        int prog_scaffale PK
        int stanza
        int piano
    }

    CARICO_SCARICO_SCAFFALE {
        int prog_operazione PK
        int codice_libro FK
        int prog_scaffale FK
        string progressivo
        string stato
        date data_operazione
    }
```

### Perché serve una tabella autonoma

La movimentazione non è solo un collegamento tra `LIBRI` e `SCAFFALE`: contiene dati propri. Per questo diventa una tabella.

---

## 7. Attributi multivalore

Un attributo multivalore non va gestito come una singola colonna piena di valori separati da virgole.

Esempio: un lettore può avere più contatti.

Soluzione: creare una tabella `CONTATTI` collegata a `LETTORE`.

```mermaid
erDiagram
    LETTORE ||--o{ CONTATTI : possiede
    TIPO_CONTATTO ||--o{ CONTATTI : tipizza

    LETTORE {
        int codice_lettore PK
        string nome
        string cognome
        string codice_fiscale
    }

    CONTATTI {
        int codice_contatto PK
        int codice_lettore FK
        int id_tipo FK
        string contatto
    }

    TIPO_CONTATTO {
        int codice_tipo PK
        string tipo
    }
```

### Esempio

| codice_lettore | nome | cognome |
|---:|---|---|
| 1 | Mario | Rossi |

| codice_contatto | codice_lettore | id_tipo | contatto |
|---:|---:|---:|---|
| 1 | 1 | 1 | mario@example.it |
| 2 | 1 | 2 | 3331234567 |

---

## 8. Modello relazionale completo dell'esempio Biblioteca

```mermaid
erDiagram
    GENERI ||--o{ LIBRI : classifica
    EDITORI ||--o{ LIBRI : pubblica
    AUTORI ||--o{ LIBRI : scrive
    SCAFFALE ||--o{ CARICO_SCARICO_SCAFFALE : registra
    LIBRI ||--o{ CARICO_SCARICO_SCAFFALE : movimenta
    LETTORE ||--o{ PRESTITO : effettua
    LIBRI ||--o{ PRESTITO : riguarda
    LETTORE ||--o{ CONTATTI : possiede
    TIPO_CONTATTO ||--o{ CONTATTI : descrive

    LIBRI {
        int codice_libro PK
        string codice_isbn
        string titolo
        int id_genere FK
        int id_editore FK
        int id_autore FK
        string edizione
    }

    GENERI {
        int id PK
        string genere
    }

    EDITORI {
        int id PK
        string editore
    }

    AUTORI {
        int id PK
        string autore
    }

    SCAFFALE {
        int prog_scaffale PK
        int stanza
        int piano
    }

    CARICO_SCARICO_SCAFFALE {
        int prog_operazione PK
        int codice_libro FK
        int prog_scaffale FK
        string progressivo
        string stato
        date data_operazione
    }

    LETTORE {
        int codice_lettore PK
        string nome
        string cognome
        int eta
        string sesso
        string codice_fiscale
        string indirizzo
        string cap
        string citta
        string provincia
        string titolo_di_studio
    }

    CONTATTI {
        int codice_contatto PK
        int codice_lettore FK
        int id_tipo FK
        string contatto
    }

    TIPO_CONTATTO {
        int codice_tipo PK
        string tipo
    }

    PRESTITO {
        int prog_operazione PK
        int codice_libro FK
        int codice_lettore FK
        date data_pres
        date data_rest
    }
```

---

## Sintesi finale

Il modello relazionale è il passaggio intermedio tra modello concettuale e database fisico. Le entità diventano tabelle, gli attributi diventano colonne, le relazioni diventano chiavi esterne o tabelle associative. La normalizzazione riduce duplicazioni e rende il database più robusto.
