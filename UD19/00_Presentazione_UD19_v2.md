# 00 - Presentazione UD19.v2

## Titolo

**Validazione applicativa ed eccezioni**

## Contesto

Nelle unità precedenti abbiamo costruito modelli a oggetti, usato interfacce, collections, generics, lambda e stream.

A questo punto diventa necessario affrontare un problema tipico delle applicazioni reali: cosa succede quando un dato non è valido, quando una ricerca non produce risultati o quando un'operazione viola una regola applicativa.

La UD19 introduce un approccio strutturato alla gestione degli errori, evitando soluzioni improvvisate basate su `System.out.println`, valori speciali o controlli duplicati ovunque.

## Obiettivo della giornata

Progettare una piccola applicazione Java nella quale la validazione sia una responsabilità chiara e verificabile.

## Risultati attesi

Al termine della UD19 dovremmo essere in grado di progettare, implementare e motivare una gestione degli errori coerente con una piccola applicazione Java.

In particolare saper:

### 1. Distinguere i tipi di problema

Dato un comportamento anomalo, distinguere tra:

- errore di programmazione;
- dato mancante o formalmente errato;
- entità non trovata;
- violazione di una regola di business;
- situazione recuperabile o non recuperabile.

### 2. Usare le eccezioni senza abusarne

utilizzare:

- `try` e `catch` per gestire un errore nel punto corretto;
- `throw` per segnalare un problema applicativo;
- `finally` quando esiste una risorsa da chiudere o una pulizia da garantire;
- eccezioni standard quando sono sufficienti;
- eccezioni custom quando serve esprimere un significato di dominio.

### 3. Progettare eccezioni custom significative

creare eccezioni come:

```java
ValidazioneException
EntitaNonTrovataException
RegolaBusinessException
```

Queste eccezioni certamente sono più leggibili di un generico `RuntimeException` usato ovunque.

### 4. Centralizzare la validazione

Per evitare codice come il seguente venga ripetuto in più punti dell'applicazione:

```java
if (titolo == null || titolo.isBlank()) {
    System.out.println("Titolo non valido");
}
```

bisognerà introdurre metodi o classi di supporto come:

```java
Validator.requireText(titolo, "Il titolo è obbligatorio");
```

### 5. Separare responsabilità

bisogna saper collocare i controlli nel punto corretto:

| Controllo | Punto consigliato |
|---|---|
| formato base di un valore | validatore o metodo dedicato |
| regola del dominio | servizio applicativo |
| messaggio verso l'utente | vista, main o livello di interazione |
| ricerca di una entità | repository/DAO o servizio, secondo il livello raggiunto |

### 6. Preparare il codice alla persistenza

La validazione introdotta in questa UD sarà riutilizzata nelle unità successive dedicate a:

- persistenza su TXT, CSV e JSON;
- CRUD strutturato;
- DAO;
- JDBC;
- Spring Boot;
- Spring MVC.

Un dato non valido non deve arrivare indisturbato fino al file, al database o alla pagina web.

## Sequenza didattica della giornata

| Fase | Durata indicativa | Attività |
|---|---:|---|
| Richiamo iniziale | 30 min | svantaggi dei controlli dispersi nel codice |
| Teoria 1 | 60 min | eccezioni, flusso, `try/catch/throw/finally` |
| Teoria 2 | 60 min | eccezioni custom, validazione e regole di business |
| Laboratorio guidato | 90 min | catalogo corsi con validazione applicativa |
| Revisione guidata | 30 min | discussione delle scelte progettuali |
| Laboratorio autonomo | 180 min | gestione iscrizioni con errori applicativi |
| Debrief finale | 30 min | evidenze, confronto soluzioni, preparazione UD20 |

## Collegamento con le UD successive

Questa unità didattica introduce un modo più ordinato di trattare gli errori prima che i dati vengano salvati su file o database.

Nella UD20, dedicata alla persistenza, sarà necessario decidere cosa fare quando:

- un file non esiste;
- una riga CSV è malformata;
- un JSON contiene dati incompleti;
- un oggetto letto da file non supera più le regole di validazione.
