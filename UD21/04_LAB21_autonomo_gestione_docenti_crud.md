# LAB21 autonomo - Gestione docenti con CRUD console, DAO e Singleton

## Obiettivo del laboratorio

Realizzare una console application per gestire un archivio docenti.

Il laboratorio deve dimostrare la capacità di progettare un CRUD strutturato, usando:

- model;
- DAO tramite interfaccia;
- implementazione DAO Singleton in memoria;
- service con regole applicative;
- view con `Scanner`;
- controller con menu;
- main essenziale.

## Scenario

Un ente di formazione vuole gestire un piccolo archivio docenti interni ed esterni.

Per ogni docente devono essere memorizzati:

| Campo | Regola |
|---|---|
| codice | obbligatorio, identificativo univoco |
| nome | obbligatorio |
| cognome | obbligatorio |
| areaCompetenza | obbligatoria |
| tariffaOraria | maggiore o uguale a zero |
| attivo | `true` o `false` |

## Requisiti software

| Strumento | Utilizzo |
|---|---|
| JDK | Compilazione ed esecuzione |
| VS Code o IDE Java equivalente | Scrittura del codice |
| Terminale | Comandi di compilazione ed esecuzione |
| Git | Versionamento del laboratorio |

## Struttura richiesta

Creare una struttura simile alla seguente:

```text
UD21_gestione_docenti_crud/
  src/
    corso/
      ud21/
        docenti/
          app/
          controller/
          dao/
          model/
          service/
          view/
  docs/
```

## Classi minime richieste

| Package | Classe/interfaccia | Responsabilità |
|---|---|---|
| `model` | `Docente` | rappresenta il docente |
| `dao` | `DocenteDao` | contratto del repository |
| `dao` | `DocenteDaoInMemoria` | repository Singleton basato su `List` |
| `service` | `DocenteService` | regole applicative |
| `view` | `DocenteView` | input/output console |
| `controller` | `DocenteController` | menu e coordinamento |
| `app` | `EseguiGestioneDocenti` | avvio applicazione |

È possibile aggiungere altre classi se la scelta è motivata nel file di evidenza.


## Approfondimento richiesto sul Singleton

Nel file di evidenza il partecipante deve aggiungere una breve sezione intitolata:

```text
Uso del Singleton nel DAO in memoria
```

La sezione deve rispondere alle seguenti domande:

1. Perché `DocenteDaoInMemoria` è stato implementato come Singleton?
2. Che problema potrebbe nascere creando più istanze del DAO?
3. Perché il service deve dipendere dall'interfaccia `DocenteDao` e non direttamente da `DocenteDaoInMemoria`?
4. Perché la versione Singleton usata nel laboratorio è sufficiente in un'applicazione console monothread?
5. Quale problema potrebbe emergere in presenza di più thread?

Non è richiesto implementare il Singleton thread-safe nel laboratorio autonomo.
La versione thread-safe verrà ripresa nella UD22.

## Operazioni CRUD richieste

Il programma deve offrire un menu con almeno queste operazioni:

```text
1. Inserisci docente
2. Elenca tutti i docenti
3. Cerca docente per codice
4. Modifica docente
5. Disattiva docente
6. Elimina docente
0. Esci
```

La disattivazione non elimina il docente, ma imposta `attivo` a `false`.

## Regole applicative richieste

Il service deve garantire almeno queste regole:

1. non si possono inserire due docenti con lo stesso codice;
2. nome e cognome non possono essere vuoti;
3. l'area di competenza non può essere vuota;
4. la tariffa oraria non può essere negativa;
5. la modifica deve fallire se il docente non esiste;
6. la disattivazione deve fallire se il docente non esiste;
7. il DAO non deve leggere input e non deve stampare messaggi;
8. la view non deve modificare direttamente la lista dei docenti.

## Vincoli progettuali

### Uso del DAO

Il service deve dipendere dall'interfaccia:

```java
private final DocenteDao docenteDao;
```

Non deve dipendere direttamente da `DocenteDaoInMemoria`.

### Uso del Singleton

`DocenteDaoInMemoria` deve essere Singleton.

Esempio atteso:

```java
private static final DocenteDaoInMemoria instance = new DocenteDaoInMemoria();

private DocenteDaoInMemoria() {
}

public static DocenteDaoInMemoria getInstance() {
    return instance;
}
```

### Scanner

Lo `Scanner` deve essere usato solo nella view.

### Main

Il `main` deve limitarsi a collegare DAO, service, view e controller.

## Compilazione ed esecuzione

Da dentro la cartella del laboratorio:

```bash
javac -encoding UTF-8 -d out $(find src -name "*.java")
java -cp out corso.ud21.docenti.app.EseguiGestioneDocenti
```

Su Windows PowerShell:

```powershell
Get-ChildItem -Recurse -Filter *.java src | ForEach-Object { $_.FullName } > sources.txt
javac -encoding UTF-8 -d out @sources.txt
java -cp out corso.ud21.docenti.app.EseguiGestioneDocenti
```

## Test minimi richiesti

| Test | Risultato atteso |
|---|---|
| Inserimento docente valido | Docente salvato |
| Inserimento codice duplicato | Operazione rifiutata |
| Ricerca codice esistente | Docente mostrato |
| Ricerca codice inesistente | Messaggio di docente non trovato |
| Modifica docente esistente | Dati aggiornati |
| Modifica docente inesistente | Operazione rifiutata |
| Disattivazione docente | `attivo` diventa `false` |
| Eliminazione docente | Docente rimosso dalla lista |

## Evidenza richiesta

Creare il file:

```text
docs/evidence_LAB21_autonomo.md
```

Il file deve contenere:

1. struttura finale del progetto;
2. breve descrizione delle responsabilità di ogni package;
3. diagramma Mermaid delle classi principali;
4. copia testuale o screenshot dei test minimi;
5. spiegazione del ruolo del DAO;
6. spiegazione del motivo per cui il DAO Singleton è stato introdotto;
7. breve riflessione su cosa cambierebbe sostituendo il DAO in memoria con un DAO JDBC.

## Criterio di completamento

Il laboratorio è completo quando:

- il programma compila senza errori;
- il menu consente tutte le operazioni richieste;
- le regole applicative sono rispettate;
- il codice è separato per responsabilità;
- l'evidenza documenta il funzionamento e le scelte progettuali.
