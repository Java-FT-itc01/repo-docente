# 04 - LAB18 autonomo - Report attività formative con Stream

## Scenario

Un ente di formazione vuole produrre report rapidi sulle attività formative pianificate.

Ogni attività ha:

- codice;
- titolo;
- area;
- durata in ore;
- numero massimo di posti;
- numero di iscritti;
- prezzo per partecipante;
- stato di pubblicazione.

Il partecipante deve realizzare una piccola applicazione Java che analizzi una lista di attività usando lambda e stream.

## Obiettivi del laboratorio

Il laboratorio verifica la capacità di:

- modellare una classe di dominio;
- creare una lista tipizzata;
- usare stream su oggetti;
- separare logica di report dal `main`;
- produrre risultati filtrati, trasformati, ordinati e aggregati;
- motivare l'uso di stream rispetto ai cicli tradizionali.

## Software e tool necessari

| Strumento | Necessario |
|---|---:|
| JDK | sì |
| VS Code o IDE Java equivalente | sì |
| Git | sì |

Non sono previste nuove installazioni per questo laboratorio.

## Struttura richiesta

Creare la seguente struttura:

```text
UD18_report_attivita_stream/
  src/
    corso/
      ud18/
        report/
          AttivitaFormativa.java
          ReportAttivitaService.java
          EseguiReportAttivita.java
  docs/
    evidence_UD18_autonomo.md
```

## Requisiti funzionali

### 1. Classe `AttivitaFormativa`

La classe deve contenere almeno:

- `codice`;
- `titolo`;
- `area`;
- `durataOre`;
- `postiMassimi`;
- `iscritti`;
- `prezzoPartecipante`;
- `pubblicata`.

Devono essere presenti:

- costruttore completo;
- getter;
- `toString()` leggibile.

### 2. Classe `ReportAttivitaService`

La classe deve contenere metodi basati su stream.

Sono obbligatori almeno i seguenti metodi:

#### Metodo 1 - Attività pubblicate

Restituisce la lista delle attività pubblicate.

Firma suggerita:

```java
public List<AttivitaFormativa> trovaPubblicate(List<AttivitaFormativa> attivita)
```

#### Metodo 2 - Attività per area

Restituisce le attività di una determinata area.

```java
public List<AttivitaFormativa> trovaPerArea(List<AttivitaFormativa> attivita, String area)
```

#### Metodo 3 - Titoli delle attività pubblicate

Restituisce solo i titoli delle attività pubblicate, ordinati alfabeticamente.

```java
public List<String> titoliPubblicatiOrdinati(List<AttivitaFormativa> attivita)
```

#### Metodo 4 - Attività con posti disponibili

Restituisce le attività pubblicate che hanno ancora posti disponibili.

```java
public List<AttivitaFormativa> trovaConPostiDisponibili(List<AttivitaFormativa> attivita)
```

#### Metodo 5 - Conteggio attività lunghe

Conta le attività con durata maggiore o uguale a una soglia.

```java
public long contaAttivitaConDurataMinima(List<AttivitaFormativa> attivita, int sogliaOre)
```

#### Metodo 6 - Verifica attività sold out

Verifica se esiste almeno una attività pubblicata con iscritti uguali o superiori ai posti massimi.

```java
public boolean esisteAttivitaSoldOut(List<AttivitaFormativa> attivita)
```

#### Metodo 7 - Prima attività pubblicata per area

Restituisce la prima attività pubblicata di una certa area.

```java
public Optional<AttivitaFormativa> primaPubblicataPerArea(List<AttivitaFormativa> attivita, String area)
```

#### Metodo 8 - Ricavo teorico delle attività pubblicate

Calcola il ricavo teorico complessivo delle attività pubblicate:

```text
iscritti * prezzoPartecipante
```

```java
public double calcolaRicavoTeoricoPubblicate(List<AttivitaFormativa> attivita)
```

### 3. Classe `EseguiReportAttivita`

La classe deve:

- creare almeno 7 attività formative;
- includere almeno 3 aree diverse;
- includere attività pubblicate e non pubblicate;
- includere almeno una attività sold out;
- usare tutti i metodi del servizio;
- stampare un report leggibile.

## Vincoli tecnici

- Usare `List<AttivitaFormativa>`.
- Usare `ArrayList` per popolare i dati iniziali.
- Non usare array per la gestione principale dei dati.
- Non usare `Map` come soluzione principale.
- Non usare database.
- Non usare file.
- Non usare Maven.
- Non inserire tutta la logica nel `main`.
- Ogni metodo di report deve usare almeno una pipeline stream.

## Requisito di qualità

Il codice deve essere leggibile.

Una pipeline troppo lunga deve essere spezzata o resa più chiara attraverso metodi di supporto privati.

Esempio accettabile:

```java
private boolean haPostiDisponibili(AttivitaFormativa attivita) {
    return attivita.getIscritti() < attivita.getPostiMassimi();
}
```

Uso:

```java
return attivita.stream()
        .filter(AttivitaFormativa::isPubblicata)
        .filter(this::haPostiDisponibili)
        .collect(Collectors.toList());
```

## Compilazione ed esecuzione

Dalla cartella `UD18_report_attivita_stream`:

```bash
javac -encoding UTF-8 -d out src/corso/ud18/report/*.java
java -cp out corso.ud18.report.EseguiReportAttivita
```

## Evidenze richieste

Nel file:

```text
docs/evidence_UD18_autonomo.md
```

inserire:

1. descrizione sintetica del modello dati;
2. elenco dei metodi implementati nel servizio;
3. comando di compilazione;
4. comando di esecuzione;
5. output ottenuto;
6. risposta alle domande di autovalutazione.

## Domande di autovalutazione

### Domanda 1

Quale metodo del servizio usa `map` e perché?

### Domanda 2

Quale metodo usa `anyMatch` e quale vantaggio offre rispetto a un ciclo completo?

### Domanda 3

Perché il metodo `primaPubblicataPerArea` restituisce un `Optional`?

### Domanda 4

Il metodo che calcola il ricavo teorico modifica gli oggetti della lista?

### Domanda 5

Quale metodo sarebbe più difficile da leggere se scritto con una pipeline troppo lunga?

### Domanda 6

In quale punto del codice uno stream rende il programma più chiaro rispetto a un ciclo `for`?

## Estensioni facoltative

Chi completa il laboratorio può aggiungere:

- ordinamento delle attività per ricavo teorico decrescente;
- filtro per prezzo massimo;
- elenco dei codici delle attività non pubblicate;
- conteggio delle attività per area usando una soluzione semplice con cicli o `Map`, senza anticipare collector avanzati.
