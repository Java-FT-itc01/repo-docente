# TXT, CSV e JSON: formati e scelte

## 1. Perché usare formati diversi

Non tutti i file hanno lo stesso scopo.

Un report per una persona, un file dati per essere riletto dal programma e un export per integrazione non hanno le stesse esigenze.

In questa UD useremo tre formati:

| Formato | Scopo principale |
|---|---|
| TXT | report leggibile da una persona |
| CSV | dati tabellari semplici |
| JSON | dati strutturati e scambiabili |

## 2. TXT

Un file TXT è adatto a produrre un report leggibile.

Esempio:

```text
REPORT CORSI
============
Codice: JAVA-BASE
Titolo: Java Base
Ore: 40
```

Vantaggi:

- facile da leggere;
- semplice da generare;
- utile per evidenze e report.

Limiti:

- poco adatto al caricamento automatico;
- struttura non sempre regolare;
- difficile da validare se il formato è libero.

## 3. CSV

CSV significa Comma-Separated Values, anche se in molti contesti europei si usa il punto e virgola come separatore.

Esempio:

```csv
codice;titolo;ore
JAVA-BASE;Java Base;40
JAVA-WEB;Java Web;48
```

Vantaggi:

- semplice;
- leggibile anche con editor di testo;
- importabile in fogli di calcolo;
- adatto a dati tabellari.

Limiti:

- rappresenta male strutture annidate;
- richiede attenzione ai separatori nei campi;
- richiede validazione in fase di lettura.

## 4. JSON

JSON rappresenta dati strutturati con oggetti e array.

Esempio:

```json
[
  {
    "codice": "JAVA-BASE",
    "titolo": "Java Base",
    "ore": 40
  },
  {
    "codice": "JAVA-WEB",
    "titolo": "Java Web",
    "ore": 48
  }
]
```

Vantaggi:

- adatto a strutture più ricche;
- molto usato nelle API REST;
- leggibile da persone e programmi;
- prepara il lavoro sui microservizi.

Limiti:

- senza librerie dedicate richiede codice manuale;
- la lettura robusta è più complessa del CSV;
- errori di sintassi possono rendere il file non valido.

## 5. Scelta del formato

| Situazione | Formato consigliato |
|---|---|
| Stampare un riepilogo per il docente | TXT |
| Salvare elenco corsi o iscrizioni da ricaricare | CSV |
| Esportare dati verso una possibile API futura | JSON |
| Gestire relazioni complesse e query | Database |

## 6. Scrittura file con Java standard

Le API usate in questa UD appartengono alla libreria standard Java:

```java
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.charset.StandardCharsets;
```

Esempio:

```java
List<String> righe = List.of("riga 1", "riga 2");
Files.write(Path.of("data/output.txt"), righe, StandardCharsets.UTF_8);
```

Lettura:

```java
List<String> righe = Files.readAllLines(Path.of("data/output.txt"), StandardCharsets.UTF_8);
```

## 7. Percorsi relativi

Se si usa:

```java
Path.of("data/corsi.csv")
```

il percorso viene risolto rispetto alla cartella da cui viene avviato il programma.

Per evitare confusione, nei laboratori il programma va eseguito dalla radice del progetto.

Esempio:

```bash
cd UD20_catalogo_corsi_file
javac -encoding UTF-8 -d out src/corso/ud20/catalogo/*.java
java -cp out corso.ud20.catalogo.EseguiCatalogoCorsi
```

## 8. JSON manuale e limite didattico

In questa UD il JSON viene scritto manualmente per non introdurre ancora Maven e librerie esterne.

Questo va bene per dati controllati e semplici.

In applicazioni reali si usano librerie dedicate che gestiscono:

- escaping completo;
- conversione automatica oggetto/JSON;
- lettura robusta;
- tipi annidati;
- date;
- collezioni complesse.

Il passaggio a librerie esterne sarà affrontato dopo l'introduzione di Maven.
