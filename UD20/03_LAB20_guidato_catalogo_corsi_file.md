# LAB20 guidato - Catalogo corsi con persistenza su file

## Obiettivo del laboratorio

Realizzare una piccola applicazione console che gestisce un catalogo corsi in memoria e produce tre file:

- un report TXT leggibile;
- un file CSV ricaricabile;
- un export JSON semplice.

Il laboratorio introduce un repository su file, preparando il passaggio successivo verso DAO e JDBC.

## Requisiti software

| Strumento | Utilizzo |
|---|---|
| JDK | Compilazione ed esecuzione |
| VS Code o IDE Java equivalente | Scrittura del codice |
| Terminale | Comandi di compilazione ed esecuzione |
| Git | Versionamento del laboratorio |

## Struttura da creare

```text
UD20_catalogo_corsi_file/
  src/
    corso/
      ud20/
        catalogo/
          Corso.java
          CatalogoService.java
          CorsoFileRepository.java
          FileRepositoryException.java
          EseguiCatalogoCorsi.java
  data/
  docs/
```

Comandi:

```bash
mkdir -p UD20_catalogo_corsi_file/src/corso/ud20/catalogo
mkdir -p UD20_catalogo_corsi_file/data
mkdir -p UD20_catalogo_corsi_file/docs
cd UD20_catalogo_corsi_file
```

## Passo 1 - Creare la classe `Corso`

File:

```text
src/corso/ud20/catalogo/Corso.java
```

```java
package corso.ud20.catalogo;

public class Corso {
    private final String codice;
    private final String titolo;
    private final int ore;
    private final double prezzo;

    public Corso(String codice, String titolo, int ore, double prezzo) {
        if (codice == null || codice.isBlank()) {
            throw new IllegalArgumentException("Il codice corso è obbligatorio");
        }
        if (titolo == null || titolo.isBlank()) {
            throw new IllegalArgumentException("Il titolo corso è obbligatorio");
        }
        if (ore <= 0) {
            throw new IllegalArgumentException("Le ore devono essere positive");
        }
        if (prezzo < 0) {
            throw new IllegalArgumentException("Il prezzo non può essere negativo");
        }

        this.codice = codice;
        this.titolo = titolo;
        this.ore = ore;
        this.prezzo = prezzo;
    }

    public String getCodice() {
        return codice;
    }

    public String getTitolo() {
        return titolo;
    }

    public int getOre() {
        return ore;
    }

    public double getPrezzo() {
        return prezzo;
    }

    @Override
    public String toString() {
        return codice + " - " + titolo + " (" + ore + " ore, " + prezzo + " euro)";
    }
}
```

## Passo 2 - Creare il servizio applicativo

File:

```text
src/corso/ud20/catalogo/CatalogoService.java
```

```java
package corso.ud20.catalogo;

import java.util.ArrayList;
import java.util.List;
import java.util.Optional;

public class CatalogoService {
    private final List<Corso> corsi = new ArrayList<>();

    public void aggiungi(Corso corso) {
        if (corso == null) {
            throw new IllegalArgumentException("Il corso non può essere null");
        }
        if (cercaPerCodice(corso.getCodice()).isPresent()) {
            throw new IllegalArgumentException("Esiste già un corso con codice " + corso.getCodice());
        }
        corsi.add(corso);
    }

    public Optional<Corso> cercaPerCodice(String codice) {
        return corsi.stream()
                .filter(c -> c.getCodice().equalsIgnoreCase(codice))
                .findFirst();
    }

    public List<Corso> elenco() {
        return new ArrayList<>(corsi);
    }

    public int numeroCorsi() {
        return corsi.size();
    }
}
```

Osservazione progettuale:

- `CatalogoService` conosce le regole applicative;
- non conosce il formato dei file;
- non usa `Files.write` o `Files.readAllLines`.

## Passo 3 - Creare la classe per eccezioni su File Repository

File:

```text
src/corso/ud20/catalogo/FileRepositoryException.java
```

```java
package corso.ud20.catalogo;

public class FileRepositoryException extends RuntimeException {

    public FileRepositoryException(String message, Throwable cause) {
        super(message, cause);
    }

    public FileRepositoryException(String message) {
        super(message);
    }
}
```

Questa eccezione evita di propagare ovunque dettagli tecnici come `IOException`.

## Passo 4 - Creare il repository file

File:

```text
src/corso/ud20/catalogo/CorsoFileRepository.java
```

```java
package corso.ud20.catalogo;

import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;
import java.util.ArrayList;
import java.util.List;

public class CorsoFileRepository {
    private final Path directory;

    public CorsoFileRepository(Path directory) {
        this.directory = directory;
        creaDirectorySeNecessaria();
    }

    public void salvaReportTxt(List<Corso> corsi) {
        List<String> righe = new ArrayList<>();
        righe.add("REPORT CATALOGO CORSI");
        righe.add("=====================");
        righe.add("Totale corsi: " + corsi.size());
        righe.add("");

        for (Corso corso : corsi) {
            righe.add("Codice: " + corso.getCodice());
            righe.add("Titolo: " + corso.getTitolo());
            righe.add("Ore: " + corso.getOre());
            righe.add("Prezzo: " + corso.getPrezzo());
            righe.add("---");
        }

        scriviRighe(directory.resolve("catalogo_report.txt"), righe);
    }

    public void salvaCsv(List<Corso> corsi) {
        List<String> righe = new ArrayList<>();
        righe.add("codice;titolo;ore;prezzo");

        for (Corso corso : corsi) {
            validaCampoCsv(corso.getCodice());
            validaCampoCsv(corso.getTitolo());
            righe.add(corso.getCodice() + ";" + corso.getTitolo() + ";" + corso.getOre() + ";" + corso.getPrezzo());
        }

        scriviRighe(directory.resolve("catalogo_corsi.csv"), righe);
    }

    public List<Corso> caricaCsv() {
        Path file = directory.resolve("catalogo_corsi.csv");

        try {
            List<String> righe = Files.readAllLines(file, StandardCharsets.UTF_8);
            List<Corso> corsi = new ArrayList<>();

            for (int i = 1; i < righe.size(); i++) {
                String riga = righe.get(i);
                String[] campi = riga.split(";", -1);

                if (campi.length != 4) {
                    throw new FileRepositoryException("Riga CSV non valida alla riga " + (i + 1));
                }

                String codice = campi[0];
                String titolo = campi[1];
                int ore = Integer.parseInt(campi[2]);
                double prezzo = Double.parseDouble(campi[3]);

                corsi.add(new Corso(codice, titolo, ore, prezzo));
            }

            return corsi;
        } catch (IOException | RuntimeException e) {
            throw new FileRepositoryException("Errore durante il caricamento del catalogo CSV", e);
        }
    }

    public void salvaJson(List<Corso> corsi) {
        List<String> righe = new ArrayList<>();
        righe.add("[");

        for (int i = 0; i < corsi.size(); i++) {
            Corso corso = corsi.get(i);
            righe.add("  {");
            righe.add("    \"codice\": \"" + escapeJson(corso.getCodice()) + "\",");
            righe.add("    \"titolo\": \"" + escapeJson(corso.getTitolo()) + "\",");
            righe.add("    \"ore\": " + corso.getOre() + ",");
            righe.add("    \"prezzo\": " + corso.getPrezzo());
            righe.add("  }" + (i < corsi.size() - 1 ? "," : ""));
        }

        righe.add("]");
        scriviRighe(directory.resolve("catalogo_corsi.json"), righe);
    }

    private void creaDirectorySeNecessaria() {
        try {
            Files.createDirectories(directory);
        } catch (IOException e) {
            throw new FileRepositoryException("Impossibile creare la directory dati", e);
        }
    }

    private void scriviRighe(Path file, List<String> righe) {
        try {
            Files.write(file, righe, StandardCharsets.UTF_8);
        } catch (IOException e) {
            throw new FileRepositoryException("Errore durante la scrittura del file " + file, e);
        }
    }

    private void validaCampoCsv(String valore) {
        if (valore.contains(";") || valore.contains("\n") || valore.contains("\r")) {
            throw new FileRepositoryException("Campo CSV non valido: contiene separatori o ritorni a capo");
        }
    }

    private String escapeJson(String valore) {
        return valore.replace("\\", "\\\\").replace("\"", "\\\"");
    }
}
```

## Passo 5 - Creare la classe main

File:

```text
src/corso/ud20/catalogo/EseguiCatalogoCorsi.java
```

```java
package corso.ud20.catalogo;

import java.nio.file.Path;
import java.util.List;

public class EseguiCatalogoCorsi {

    public static void main(String[] args) {
        CatalogoService service = new CatalogoService();

        service.aggiungi(new Corso("JAVA-BASE", "Java Base", 40, 900.0));
        service.aggiungi(new Corso("JAVA-OOP", "Java Object Oriented", 48, 1100.0));
        service.aggiungi(new Corso("JAVA-WEB", "Java Web", 56, 1300.0));

        CorsoFileRepository repository = new CorsoFileRepository(Path.of("data"));

        repository.salvaReportTxt(service.elenco());
        repository.salvaCsv(service.elenco());
        repository.salvaJson(service.elenco());

        List<Corso> corsiCaricati = repository.caricaCsv();

        System.out.println("Corsi salvati: " + service.numeroCorsi());
        System.out.println("Corsi caricati da CSV: " + corsiCaricati.size());

        for (Corso corso : corsiCaricati) {
            System.out.println(corso);
        }
    }
}
```

## Passo 6 - Compilare ed eseguire

Dalla cartella `UD20_catalogo_corsi_file`:

```bash
javac -encoding UTF-8 -d out src/corso/ud20/catalogo/*.java
java -cp out corso.ud20.catalogo.EseguiCatalogoCorsi
```

Output atteso indicativo:

```text
Corsi salvati: 3
Corsi caricati da CSV: 3
JAVA-BASE - Java Base (40 ore, 900.0 euro)
JAVA-OOP - Java Object Oriented (48 ore, 1100.0 euro)
JAVA-WEB - Java Web (56 ore, 1300.0 euro)
```

## Passo 7 - Verificare i file generati

Dopo l'esecuzione devono essere presenti:

```text
data/catalogo_report.txt
data/catalogo_corsi.csv
data/catalogo_corsi.json
```

Comandi utili su Linux/macOS/Git Bash:

```bash
ls -la data
cat data/catalogo_corsi.csv
cat data/catalogo_corsi.json
```

Su PowerShell:

```powershell
Get-ChildItem data
Get-Content data/catalogo_corsi.csv
Get-Content data/catalogo_corsi.json
```

## Passo 8 - Evidenza richiesta

Creare il file:

```text
docs/evidence_LAB20_guidato.md
```

Contenuto minimo:

```md
# Evidence LAB20 guidato

## Comandi eseguiti

```bash
javac -encoding UTF-8 -d out src/corso/ud20/catalogo/*.java
java -cp out corso.ud20.catalogo.EseguiCatalogoCorsi
```

## File prodotti

- data/catalogo_report.txt
- data/catalogo_corsi.csv
- data/catalogo_corsi.json

## Spiegazione progettuale

- `CatalogoService` contiene le regole applicative.
- `CorsoFileRepository` contiene la logica di persistenza.
- Il `main` coordina la demo.

## Domanda conclusiva

Perché la scrittura dei file non è stata inserita direttamente nella classe `CatalogoService`?
```

## Domande di consolidamento

1. Perché il file TXT è meno adatto del CSV al caricamento automatico?
2. Quale responsabilità ha `CorsoFileRepository`?
3. Quale responsabilità ha `CatalogoService`?
4. Perché il metodo `caricaCsv()` deve validare le righe lette?
5. Quale parte del codice cambierebbe se in futuro si salvasse su database?
