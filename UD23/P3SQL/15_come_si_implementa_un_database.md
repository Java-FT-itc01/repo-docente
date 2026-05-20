# 15 - Come si implementa un database

## Obiettivi della lezione

Al termine di questa unità il partecipante deve essere in grado di:

- spiegare che cosa significa implementare un database relazionale;
- individuare gli oggetti da creare partendo dallo schema fisico;
- riconoscere l'ordine corretto di creazione di database, tabelle, vincoli, indici e viste;
- creare il database `LIBRI_PRESTATI` in MySQL;
- comprendere perché dopo l'implementazione è necessaria una fase di test.

---

## 1. Che cosa significa implementare un database

Implementare un database significa passare dallo **schema fisico** alla creazione concreta degli oggetti dentro un DBMS.

Nel caso del laboratorio viene usato MySQL. Il database da creare si chiama:

```sql
LIBRI_PRESTATI
```

Lo schema fisico descrive già:

- le tabelle;
- le colonne;
- i tipi di dato;
- le chiavi primarie;
- le chiavi esterne;
- gli indici;
- i vincoli;
- le viste da costruire successivamente.

```mermaid
flowchart LR
    A["Schema fisico"] --> B["Script SQL"]
    B --> C["DBMS MySQL"]
    C --> D["Database implementato"]
    D --> E["Test del database"]
```

---

## 2. Dove si colloca l'implementazione

Nel modello a cascata, l'implementazione appartiene alla fase di sviluppo: non si sta più solo progettando il database, ma si stanno creando realmente gli oggetti.

```mermaid
flowchart TD
    A["Analisi dei requisiti"] --> B["Progettazione concettuale"]
    B --> C["Progettazione logica"]
    C --> D["Progettazione fisica"]
    D --> E["Implementazione del database"]
    E --> F["Test"]
    F --> G["Manutenzione"]
```

L'attività può essere svolta da figure diverse:

- Database Administrator;
- Developer;
- progettista dati;
- docente o partecipante in un contesto di laboratorio.

Nel mondo reale queste figure possono coincidere nella stessa persona, soprattutto in team piccoli o in contesti di laboratorio.

---

## 3. Ordine consigliato di implementazione

Prima di scrivere codice SQL è necessario leggere lo schema fisico e stabilire l'ordine corretto di creazione degli oggetti.

L'ordine consigliato è:

1. creare il database;
2. creare le tabelle master o tipizzate;
3. creare le tabelle di dettaglio;
4. aggiungere i vincoli di chiave esterna, se non sono già stati definiti nei `CREATE TABLE`;
5. creare gli indici elementari;
6. creare gli indici composti;
7. creare le viste;
8. creare gli utenti;
9. assegnare autorizzazioni e privilegi;
10. testare il database.

```mermaid
flowchart TD
    A["1. Database"] --> B["2. Tabelle master e tipizzate"]
    B --> C["3. Tabelle di dettaglio"]
    C --> D["4. Vincoli FK"]
    D --> E["5. Indici elementari"]
    E --> F["6. Indici composti"]
    F --> G["7. Viste"]
    G --> H["8. Utenti"]
    H --> I["9. Privilegi"]
    I --> L["10. Test"]
```

---

## 4. Tabelle master, tipizzate e di dettaglio

Nel laboratorio compaiono tre categorie operative di tabelle.

| Tipo tabella | Significato | Esempi |
|---|---|---|
| Tabella tipizzata | Contiene valori di classificazione | `GENERI`, `AUTORI`, `EDITORI`, `TITOLI_DI_STUDIO`, `TIPI_CONTATTO` |
| Tabella master | Contiene un'entità principale | `LETTORI`, `LIBRI`, `MAGAZZINO` |
| Tabella di dettaglio | Contiene dati collegati ad altre tabelle | `CONTATTI`, `PRESTITI` |

Questa distinzione aiuta a scegliere l'ordine di creazione.

```mermaid
flowchart LR
    A["Tabelle tipizzate"] --> D["Tabelle di dettaglio"]
    B["Tabelle master"] --> D

    A --> A1["GENERI
AUTORI
EDITORI
TITOLI_DI_STUDIO
TIPI_CONTATTO"]
    B --> B1["LIBRI
LETTORI
MAGAZZINO"]
    D --> D1["CONTATTI
PRESTITI"]
```

---

## 5. Schema generale del database `LIBRI_PRESTATI`

```mermaid
erDiagram
    GENERI ||--o{ LIBRI : classifica
    AUTORI ||--o{ LIBRI : scrive
    EDITORI ||--o{ LIBRI : pubblica
    TITOLI_DI_STUDIO ||--o{ LETTORI : qualifica
    ACCOUNTS ||--|| LETTORI : autentica
    LETTORI ||--o{ CONTATTI : possiede
    TIPI_CONTATTO ||--o{ CONTATTI : tipizza
    LIBRI ||--o{ MAGAZZINO : copia
    LETTORI ||--o{ PRESTITI : richiede
    MAGAZZINO ||--o{ PRESTITI : riguarda

    GENERI {
        int id PK
        string genere UK
    }

    AUTORI {
        int id PK
        string autore UK
    }

    EDITORI {
        int id PK
        string editore UK
    }

    LIBRI {
        int id PK
        string codice_isbn UK
        string titolo
        int id_genere FK
        int id_autore FK
        int id_editore FK
        string edizione
    }

    TITOLI_DI_STUDIO {
        int id PK
        string titolo_di_studio UK
    }

    ACCOUNTS {
        int id PK
        string nome_utente UK
        string passwd
    }

    LETTORI {
        int id PK
        string codice_lettore UK
        string nome
        string cognome
        date data_di_nascita
        boolean sesso
        string codice_fiscale UK
        int id_titolo_di_studio FK
        string indirizzo
        string cap
        string citta
        string provincia
        int id_account FK
    }

    TIPI_CONTATTO {
        int id PK
        string tipo_contatto UK
        blob icona
    }

    CONTATTI {
        int id PK
        int id_lettore FK
        int id_tipo_contatto FK
        string contatto
    }

    MAGAZZINO {
        int id PK
        int id_libro FK
        string codice_libro UK
        string codice_scaffale UK
        date data_carico
        boolean prestato
        decimal prezzo_carico
        decimal prezzo_scarico
    }

    PRESTITI {
        int id PK
        string codice_operazione UK
        int id_lettore FK
        int id_magazzino FK
        date data_operazione
        date data_ritiro
        date data_prestito
        date data_restituzione
        date data_consegna
        string note
    }
```

---

## 6. Creazione del database

Per iniziare il laboratorio occorre creare il database.

```sql
CREATE DATABASE IF NOT EXISTS LIBRI_PRESTATI
    DEFAULT CHARACTER SET utf8mb4
    COLLATE utf8mb4_unicode_ci;

USE LIBRI_PRESTATI;
```

### Spiegazione del comando

| Parte del comando | Significato |
|---|---|
| `CREATE DATABASE` | Crea un nuovo database |
| `IF NOT EXISTS` | Evita errore se il database esiste già |
| `DEFAULT CHARACTER SET utf8mb4` | Imposta una codifica moderna per i caratteri |
| `COLLATE utf8mb4_unicode_ci` | Imposta il criterio di confronto testuale |
| `USE LIBRI_PRESTATI` | Seleziona il database su cui lavorare |

---

## 7. Verifica iniziale

Dopo la creazione, è possibile controllare la presenza del database con:

```sql
SHOW DATABASES;
```

Oppure, dopo averlo selezionato:

```sql
SELECT DATABASE();
```

Risultato atteso:

```text
LIBRI_PRESTATI
```

---

## 8. Sintesi operativa

```mermaid
flowchart TD
    A["Leggere lo schema fisico"] --> B["Capire le dipendenze tra tabelle"]
    B --> C["Creare database"]
    C --> D["Creare tabelle senza dipendenze"]
    D --> E["Creare tabelle con dipendenze"]
    E --> F["Creare indici"]
    F --> G["Creare viste"]
    G --> H["Testare vincoli e interrogazioni"]
```

L'implementazione è quindi il momento in cui il modello smette di essere un disegno e diventa un database eseguibile. Da quel momento in poi, gli errori vengono rilevati direttamente dal DBMS durante l'esecuzione dei comandi.
