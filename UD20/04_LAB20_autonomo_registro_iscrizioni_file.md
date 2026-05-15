# LAB20 autonomo - Registro iscrizioni con persistenza su file

## Scenario

Un ente di formazione vuole gestire un piccolo registro di iscrizioni ai corsi.

Per ora non è disponibile un database.

L'applicazione deve quindi mantenere le iscrizioni in memoria durante l'esecuzione e salvarle su file al termine delle operazioni.

Il laboratorio richiede di applicare in autonomia:

- model coerente;
- service applicativo;
- repository su file;
- validazione;
- eccezioni;
- CSV per salvataggio e caricamento;
- TXT per report;
- JSON per export.

## Requisiti software

| Strumento | Utilizzo |
|---|---|
| JDK | Compilazione ed esecuzione |
| VS Code o IDE Java equivalente | Scrittura del codice |
| Terminale | Comandi di compilazione ed esecuzione |
| Git | Versionamento del laboratorio |

## Struttura richiesta

```text
UD20_registro_iscrizioni_file/
  src/
    corso/
      ud20/
        iscrizioni/
          Partecipante.java
          Corso.java
          StatoIscrizione.java
          Iscrizione.java
          RegistroIscrizioniService.java
          IscrizioneFileRepository.java
          FileRepositoryException.java
          EseguiRegistroIscrizioni.java
  data/
  docs/
```

## Requisiti funzionali

### 1. Model

Creare le seguenti classi.

#### `Partecipante`

Campi minimi:

- `matricola`;
- `nomeCompleto`;
- `email`.

Validazioni minime:

- matricola obbligatoria;
- nome obbligatorio;
- email obbligatoria e contenente `@`.

#### `Corso`

Campi minimi:

- `codice`;
- `titolo`;
- `ore`.

Validazioni minime:

- codice obbligatorio;
- titolo obbligatorio;
- ore maggiori di zero.

#### `StatoIscrizione`

Enum con almeno questi valori:

```java
ATTIVA,
COMPLETATA,
ANNULLATA
```

#### `Iscrizione`

Campi minimi:

- `id`;
- `Partecipante partecipante`;
- `Corso corso`;
- `StatoIscrizione stato`.

Validazioni minime:

- id obbligatorio;
- partecipante obbligatorio;
- corso obbligatorio;
- stato obbligatorio.

### 2. Service

Creare `RegistroIscrizioniService` con almeno questi metodi:

```java
void aggiungi(Iscrizione iscrizione)
List<Iscrizione> elenco()
Optional<Iscrizione> cercaPerId(String id)
List<Iscrizione> filtraPerCodiceCorso(String codiceCorso)
List<Iscrizione> filtraPerStato(StatoIscrizione stato)
int totaleIscrizioni()
```

Vincoli:

- non devono essere accettati ID duplicati;
- la lista restituita da `elenco()` non deve esporre direttamente la lista interna;
- il service non deve scrivere direttamente su file.

### 3. Repository file

Creare `IscrizioneFileRepository`.

Il repository deve produrre almeno questi file:

```text
data/iscrizioni.csv
data/iscrizioni_report.txt
data/iscrizioni.json
```

Metodi richiesti:

```java
void salvaCsv(List<Iscrizione> iscrizioni)
List<Iscrizione> caricaCsv()
void salvaReportTxt(List<Iscrizione> iscrizioni)
void salvaJson(List<Iscrizione> iscrizioni)
```

### 4. Formato CSV richiesto

Il file CSV deve contenere intestazione e righe dati.

Esempio:

```csv
id;matricola;nomeCompleto;email;codiceCorso;titoloCorso;oreCorso;stato
ISC-001;P001;Anna Verdi;anna.verdi@example.com;JAVA-BASE;Java Base;40;ATTIVA
```

Durante il caricamento, ogni riga deve ricostruire:

- un `Partecipante`;
- un `Corso`;
- una `Iscrizione`.

### 5. Report TXT richiesto

Il report TXT deve essere leggibile e contenere almeno:

- titolo del report;
- totale iscrizioni;
- elenco iscrizioni;
- conteggio delle iscrizioni per stato.

### 6. Export JSON richiesto

Il file JSON deve contenere un array di oggetti iscrizione.

Esempio indicativo:

```json
[
  {
    "id": "ISC-001",
    "partecipante": {
      "matricola": "P001",
      "nomeCompleto": "Anna Verdi",
      "email": "anna.verdi@example.com"
    },
    "corso": {
      "codice": "JAVA-BASE",
      "titolo": "Java Base",
      "ore": 40
    },
    "stato": "ATTIVA"
  }
]
```

È sufficiente produrre JSON in modo controllato. Non è richiesta la lettura del JSON.

## Requisiti progettuali

Il laboratorio deve rispettare queste regole:

- il `main` deve coordinare la demo, non contenere tutta la logica;
- il service deve gestire regole applicative e ricerche;
- il repository deve occuparsi dei file;
- eventuali errori di I/O devono essere incapsulati in `FileRepositoryException`;
- i dati letti da CSV devono essere validati tramite i costruttori delle classi model;
- non devono essere usate librerie esterne.

## Demo minima richiesta

La classe `EseguiRegistroIscrizioni` deve:

1. creare almeno 4 iscrizioni;
2. aggiungerle al service;
3. salvare CSV, TXT e JSON;
4. ricaricare le iscrizioni da CSV;
5. stampare a console il numero di iscrizioni caricate;
6. stampare almeno un filtro per stato;
7. stampare almeno un filtro per codice corso.

## Compilazione ed esecuzione

Dalla radice del progetto:

```bash
javac -encoding UTF-8 -d out src/corso/ud20/iscrizioni/*.java
java -cp out corso.ud20.iscrizioni.EseguiRegistroIscrizioni
```

## Evidenza richiesta

Creare il file:

```text
docs/evidence_UD20_autonomo.md
```

Il file deve contenere:

```md
# Evidence UD20 autonomo

## Comandi eseguiti

Inserire comandi di compilazione ed esecuzione.

## Output console

Incollare l'output principale.

## File prodotti

Elencare i file generati in `data/`.

## Schema responsabilità

Inserire uno schema Mermaid che mostri main, service, repository, model e file.

## Scelte progettuali

Rispondere:

1. Perché il service non scrive direttamente su file?
2. Perché il repository non dovrebbe contenere regole di business?
3. Perché il CSV è stato usato per il caricamento?
4. Perché il JSON è stato usato solo come export?
5. Quale classe dovrebbe cambiare quando si passerà a JDBC?
```

## Criteri minimi di accettazione

Il laboratorio è considerato accettabile se:

- il progetto compila;
- l'applicazione genera i tre file richiesti;
- il caricamento da CSV funziona;
- il service non espone direttamente la lista interna;
- le classi hanno responsabilità distinte;
- l'evidenza contiene spiegazioni progettuali, non solo screenshot o output.

## Estensioni facoltative

- Ordinare le iscrizioni per codice corso e nome partecipante.
- Aggiungere un metodo per esportare solo le iscrizioni attive.
- Aggiungere un controllo sul dominio email.
- Aggiungere un report separato per corso.
- Gestire righe CSV vuote o commentate.
