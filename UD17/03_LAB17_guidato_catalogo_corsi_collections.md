# 03 - LAB17 guidato: catalogo corsi con Collections

## Obiettivo del laboratorio

Realizzare un piccolo catalogo di corsi usando Collections tipizzate.

Il laboratorio mostra progressivamente:

- sostituzione di un array con `List<Corso>`;
- uso di `ArrayList`;
- inserimento, elenco, ricerca e rimozione;
- uso di `Set<String>` per ricavare categorie uniche;
- uso di `Map<String, Corso>` per costruire un indice per codice;
- metodi che ricevono e restituiscono collections;
- collegamento con polimorfismo e interfacce già viste.

## Scenario

Una struttura formativa gestisce un catalogo di corsi.

Esistono corsi in aula e corsi online.

Tutti i corsi hanno:

- codice;
- titolo;
- durata in ore;
- categoria.

I corsi in aula hanno anche una sede.

I corsi online hanno anche una piattaforma.

Il sistema deve consentire:

- aggiunta di corsi;
- elenco completo;
- ricerca per codice;
- ricerca per parte del titolo;
- rimozione per codice;
- elenco delle categorie presenti;
- creazione di un indice `Map` codice -> corso.

## Struttura consigliata

Creare questa struttura:

```text
UD17_lab_guidato_catalogo_corsi/
  src/
    corso/
      ud17/
        catalogo/
          Corso.java
          CorsoAula.java
          CorsoOnline.java
          CatalogoService.java
          EseguiCatalogoCorsi.java
```

## Verifica strumenti prima del laboratorio

Da terminale verificare:

```bash
java -version
javac -version
```

Se si usa VS Code, verificare che il progetto sia aperto dalla cartella corretta e che i file `.java` siano dentro la cartella `src`.

Per questo laboratorio non servono:

- Maven;
- Docker;
- DBMS;
- phpMyAdmin;
- Bootstrap;
- jQuery;
- Postman.

La compilazione avverrà con `javac`.

## Passo 1 - Creare la classe astratta `Corso`

Creare il file `Corso.java`.

```java
package corso.ud17.catalogo;

public abstract class Corso {
    private String codice;
    private String titolo;
    private int durataOre;
    private String categoria;

    public Corso(String codice, String titolo, int durataOre, String categoria) {
        this.codice = codice;
        this.titolo = titolo;
        this.durataOre = durataOre;
        this.categoria = categoria;
    }

    public String getCodice() {
        return codice;
    }

    public String getTitolo() {
        return titolo;
    }

    public int getDurataOre() {
        return durataOre;
    }

    public String getCategoria() {
        return categoria;
    }

    public abstract String getModalita();

    public String descrivi() {
        return codice + " - " + titolo + " - " + durataOre + " ore - " + categoria;
    }
}
```

Osservazioni:

- `Corso` è astratta perché rappresenta un concetto generale;
- `getModalita()` sarà implementato dalle sottoclassi;
- `descrivi()` fornisce una descrizione comune riusabile.

## Passo 2 - Creare `CorsoAula`

Creare il file `CorsoAula.java`.

```java
package corso.ud17.catalogo;

public class CorsoAula extends Corso {
    private String sede;

    public CorsoAula(String codice, String titolo, int durataOre, String categoria, String sede) {
        super(codice, titolo, durataOre, categoria);
        this.sede = sede;
    }

    public String getSede() {
        return sede;
    }

    @Override
    public String getModalita() {
        return "Aula";
    }

    @Override
    public String descrivi() {
        return super.descrivi() + " - modalità: " + getModalita() + " - sede: " + sede;
    }
}
```

## Passo 3 - Creare `CorsoOnline`

Creare il file `CorsoOnline.java`.

```java
package corso.ud17.catalogo;

public class CorsoOnline extends Corso {
    private String piattaforma;

    public CorsoOnline(String codice, String titolo, int durataOre, String categoria, String piattaforma) {
        super(codice, titolo, durataOre, categoria);
        this.piattaforma = piattaforma;
    }

    public String getPiattaforma() {
        return piattaforma;
    }

    @Override
    public String getModalita() {
        return "Online";
    }

    @Override
    public String descrivi() {
        return super.descrivi() + " - modalità: " + getModalita() + " - piattaforma: " + piattaforma;
    }
}
```

## Passo 4 - Creare `CatalogoService` con `List<Corso>`

Creare il file `CatalogoService.java`.

```java
package corso.ud17.catalogo;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.HashSet;
import java.util.List;
import java.util.Map;
import java.util.Set;

public class CatalogoService {
    private List<Corso> corsi;

    public CatalogoService() {
        corsi = new ArrayList<>();
    }

    public boolean aggiungi(Corso corso) {
        if (corso == null) {
            return false;
        }

        if (cercaPerCodice(corso.getCodice()) != null) {
            return false;
        }

        corsi.add(corso);
        return true;
    }

    public List<Corso> elenco() {
        return corsi;
    }

    public Corso cercaPerCodice(String codice) {
        for (Corso corso : corsi) {
            if (corso.getCodice().equalsIgnoreCase(codice)) {
                return corso;
            }
        }

        return null;
    }

    public List<Corso> cercaPerTitolo(String testo) {
        List<Corso> risultati = new ArrayList<>();

        for (Corso corso : corsi) {
            if (corso.getTitolo().toLowerCase().contains(testo.toLowerCase())) {
                risultati.add(corso);
            }
        }

        return risultati;
    }

    public boolean rimuoviPerCodice(String codice) {
        Corso corso = cercaPerCodice(codice);

        if (corso == null) {
            return false;
        }

        corsi.remove(corso);
        return true;
    }

    public Set<String> categoriePresenti() {
        Set<String> categorie = new HashSet<>();

        for (Corso corso : corsi) {
            categorie.add(corso.getCategoria());
        }

        return categorie;
    }

    public Map<String, Corso> indicePerCodice() {
        Map<String, Corso> indice = new HashMap<>();

        for (Corso corso : corsi) {
            indice.put(corso.getCodice(), corso);
        }

        return indice;
    }
}
```

Punti da osservare:

- `corsi` è una `List<Corso>`;
- `ArrayList` è solo l'implementazione scelta nel costruttore;
- `aggiungi` impedisce codici duplicati;
- `cercaPerTitolo` restituisce una lista di risultati;
- `categoriePresenti` usa un `Set`;
- `indicePerCodice` costruisce una `Map`.

## Passo 5 - Creare `EseguiCatalogoCorsi`

Creare il file `EseguiCatalogoCorsi.java`.

```java
package corso.ud17.catalogo;

import java.util.List;
import java.util.Map;
import java.util.Set;

public class EseguiCatalogoCorsi {
    public static void main(String[] args) {
        CatalogoService catalogo = new CatalogoService();

        catalogo.aggiungi(new CorsoAula("JAVA-OO", "Java Object Oriented", 40, "Java", "Aula Napoli"));
        catalogo.aggiungi(new CorsoOnline("JAVA-WEB", "Java Web Base", 32, "Java", "Moodle"));
        catalogo.aggiungi(new CorsoAula("LINUX-BASE", "Linux amministrazione base", 24, "Sistemi", "Aula Roma"));
        catalogo.aggiungi(new CorsoOnline("DEVOPS-INTRO", "Introduzione a DevOps", 16, "DevOps", "Teams"));

        System.out.println("=== Elenco completo ===");
        stampaLista(catalogo.elenco());

        System.out.println("\n=== Ricerca per codice ===");
        Corso trovato = catalogo.cercaPerCodice("JAVA-WEB");
        if (trovato != null) {
            System.out.println(trovato.descrivi());
        }

        System.out.println("\n=== Ricerca per titolo: Java ===");
        List<Corso> risultati = catalogo.cercaPerTitolo("Java");
        stampaLista(risultati);

        System.out.println("\n=== Categorie presenti ===");
        Set<String> categorie = catalogo.categoriePresenti();
        for (String categoria : categorie) {
            System.out.println(categoria);
        }

        System.out.println("\n=== Indice per codice ===");
        Map<String, Corso> indice = catalogo.indicePerCodice();
        System.out.println(indice.get("JAVA-OO").descrivi());

        System.out.println("\n=== Rimozione ===");
        boolean rimosso = catalogo.rimuoviPerCodice("LINUX-BASE");
        System.out.println("Rimosso: " + rimosso);
        stampaLista(catalogo.elenco());
    }

    private static void stampaLista(List<Corso> corsi) {
        for (Corso corso : corsi) {
            System.out.println(corso.descrivi());
        }
    }
}
```

## Passo 6 - Compilare ed eseguire

Dalla cartella principale del laboratorio:

```bash
javac -encoding UTF-8 -d out src/corso/ud17/catalogo/*.java
java -cp out corso.ud17.catalogo.EseguiCatalogoCorsi
```

## Passo 7 - Analisi richiesta

Nel file di evidenza rispondere alle domande:

1. Perché `CatalogoService` usa `List<Corso>` e non `Corso[]`?
2. Perché il riferimento è dichiarato come `List<Corso>` e non come `ArrayList<Corso>`?
3. Perché `cercaPerTitolo` restituisce una `List<Corso>`?
4. Che differenza c'è tra `Set<String>` e `List<String>` nel metodo `categoriePresenti`?
5. In quali casi una `Map<String, Corso>` è più comoda di una `List<Corso>`?
6. Dove si vede il collegamento tra Collections e polimorfismo?

## Evidenze richieste

Creare il file:

```text
docs/evidence_UD17_guidato.md
```

Inserire:

- comando di compilazione usato;
- comando di esecuzione usato;
- output principale del programma;
- risposte alle domande di analisi;
- eventuali problemi incontrati e soluzione applicata.
