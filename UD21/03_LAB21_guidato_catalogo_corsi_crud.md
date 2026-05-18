# LAB21 guidato - Catalogo corsi con CRUD console, MVC e DAO

## Obiettivo del laboratorio

Realizzare un CRUD console per la gestione di un catalogo corsi.

Il laboratorio non punta solo a far funzionare il menu, ma a costruire una struttura applicativa ordinata, nella quale ogni classe abbia una responsabilità chiara.

## Requisiti software

| Strumento | Utilizzo |
|---|---|
| JDK | Compilazione ed esecuzione |
| VS Code o IDE Java equivalente | Scrittura del codice |
| Terminale | Comandi di compilazione ed esecuzione |
| Git | Versionamento del laboratorio |

## Struttura da creare

```text
UD21_catalogo_corsi_crud/
  src/
    corso/
      ud21/
        catalogo/
          app/
            EseguiCatalogoCorsi.java
          controller/
            CatalogoController.java
          dao/
            CorsoDao.java
            CorsoDaoInMemoria.java
          model/
            Corso.java
          service/
            CatalogoService.java
          view/
            CatalogoView.java
  docs/
```

Comandi:

```bash
mkdir -p UD21_catalogo_corsi_crud/src/corso/ud21/catalogo/app
mkdir -p UD21_catalogo_corsi_crud/src/corso/ud21/catalogo/controller
mkdir -p UD21_catalogo_corsi_crud/src/corso/ud21/catalogo/dao
mkdir -p UD21_catalogo_corsi_crud/src/corso/ud21/catalogo/model
mkdir -p UD21_catalogo_corsi_crud/src/corso/ud21/catalogo/service
mkdir -p UD21_catalogo_corsi_crud/src/corso/ud21/catalogo/view
mkdir -p UD21_catalogo_corsi_crud/docs
cd UD21_catalogo_corsi_crud
```

## Passo 1 - Creare il model `Corso`

File:

```text
src/corso/ud21/catalogo/model/Corso.java
```

```java
package corso.ud21.catalogo.model;

public class Corso {
    private final String codice;
    private String titolo;
    private int ore;
    private double prezzo;

    public Corso(String codice, String titolo, int ore, double prezzo) {
        if (codice == null || codice.isBlank()) {
            throw new IllegalArgumentException("Il codice è obbligatorio");
        }
        if (titolo == null || titolo.isBlank()) {
            throw new IllegalArgumentException("Il titolo è obbligatorio");
        }
        if (ore <= 0) {
            throw new IllegalArgumentException("Le ore devono essere positive");
        }
        if (prezzo < 0) {
            throw new IllegalArgumentException("Il prezzo non può essere negativo");
        }

        this.codice = codice.trim().toUpperCase();
        this.titolo = titolo.trim();
        this.ore = ore;
        this.prezzo = prezzo;
    }

    public String getCodice() {
        return codice;
    }

    public String getTitolo() {
        return titolo;
    }

    public void setTitolo(String titolo) {
        if (titolo == null || titolo.isBlank()) {
            throw new IllegalArgumentException("Il titolo è obbligatorio");
        }
        this.titolo = titolo.trim();
    }

    public int getOre() {
        return ore;
    }

    public void setOre(int ore) {
        if (ore <= 0) {
            throw new IllegalArgumentException("Le ore devono essere positive");
        }
        this.ore = ore;
    }

    public double getPrezzo() {
        return prezzo;
    }

    public void setPrezzo(double prezzo) {
        if (prezzo < 0) {
            throw new IllegalArgumentException("Il prezzo non può essere negativo");
        }
        this.prezzo = prezzo;
    }

    @Override
    public String toString() {
        return codice + " - " + titolo + " | " + ore + " ore | " + prezzo + " euro";
    }
}
```

Osservazione:

- il codice è `final`, perché rappresenta l'identificativo logico;
- titolo, ore e prezzo possono essere modificati;
- la validazione minima è nel model per proteggere lo stato dell'oggetto.

## Passo 2 - Creare l'interfaccia DAO

File:

```text
src/corso/ud21/catalogo/dao/CorsoDao.java
```

```java
package corso.ud21.catalogo.dao;

import java.util.List;
import java.util.Optional;

import corso.ud21.catalogo.model.Corso;

public interface CorsoDao {
    void salva(Corso corso);
    Optional<Corso> trovaPerCodice(String codice);
    List<Corso> trovaTutti();
    boolean aggiorna(Corso corso);
    boolean elimina(String codice);
}
```

L'interfaccia descrive cosa serve all'applicazione, non come i dati vengono salvati.

## Passo 2A - Verificare il problema senza Singleton

Prima di implementare il DAO Singleton, è utile comprendere il problema che il Singleton intende evitare.

Immaginare una versione non protetta del DAO, con costruttore pubblico:

```java
CorsoDaoInMemoria dao1 = new CorsoDaoInMemoria();
CorsoDaoInMemoria dao2 = new CorsoDaoInMemoria();
```

Se `dao1` salva un corso e `dao2` prova a leggerlo, il corso non viene trovato, perché ogni oggetto possiede la propria lista interna.

Nel nostro laboratorio vogliamo evitare questa situazione.

Il repository in memoria deve essere unico e condiviso dall'applicazione.

Per questo `CorsoDaoInMemoria` verrà implementato come Singleton.

Regola didattica della UD21:

```text
Singleton nel DAO in memoria.
Dipendenze passate tramite costruttore negli altri componenti.
```

Non useremo il Singleton direttamente dentro service, controller o view.

## Passo 3 - Creare il DAO Singleton in memoria

File:

```text
src/corso/ud21/catalogo/dao/CorsoDaoInMemoria.java
```

```java
package corso.ud21.catalogo.dao;

import java.util.ArrayList;
import java.util.List;
import java.util.Optional;

import corso.ud21.catalogo.model.Corso;

public class CorsoDaoInMemoria implements CorsoDao {
    private static CorsoDaoInMemoria instance;

    private final List<Corso> corsi = new ArrayList<>();

    private CorsoDaoInMemoria() {
    }

    public static CorsoDaoInMemoria getInstance() {
        if (instance == null) {
            instance = new CorsoDaoInMemoria();
        }
        return instance;
    }

    @Override
    public void salva(Corso corso) {
        corsi.add(corso);
    }

    @Override
    public Optional<Corso> trovaPerCodice(String codice) {
        return corsi.stream()
                .filter(c -> c.getCodice().equalsIgnoreCase(codice))
                .findFirst();
    }

    @Override
    public List<Corso> trovaTutti() {
        return new ArrayList<>(corsi);
    }

    @Override
    public boolean aggiorna(Corso corso) {
        for (int i = 0; i < corsi.size(); i++) {
            if (corsi.get(i).getCodice().equalsIgnoreCase(corso.getCodice())) {
                corsi.set(i, corso);
                return true;
            }
        }
        return false;
    }

    @Override
    public boolean elimina(String codice) {
        return corsi.removeIf(c -> c.getCodice().equalsIgnoreCase(codice));
    }
}
```

Osservazioni:

- `getInstance()` restituisce l’unico repository condiviso;
- `trovaTutti()` restituisce una copia della lista, non la lista interna;
- il DAO non stampa e non legge input.

## Passo 4 - Creare il service

File:

```text
src/corso/ud21/catalogo/service/CatalogoService.java
```

```java
package corso.ud21.catalogo.service;

import java.util.List;
import java.util.Optional;

import corso.ud21.catalogo.dao.CorsoDao;
import corso.ud21.catalogo.model.Corso;

public class CatalogoService {
    private final CorsoDao corsoDao;

    public CatalogoService(CorsoDao corsoDao) {
        if (corsoDao == null) {
            throw new IllegalArgumentException("Il DAO non può essere null");
        }
        this.corsoDao = corsoDao;
    }

    public void creaCorso(String codice, String titolo, int ore, double prezzo) {
        if (corsoDao.trovaPerCodice(codice).isPresent()) {
            throw new IllegalArgumentException("Esiste già un corso con codice " + codice);
        }
        corsoDao.salva(new Corso(codice, titolo, ore, prezzo));
    }

    public List<Corso> elencoCorsi() {
        return corsoDao.trovaTutti();
    }

    public Optional<Corso> cercaCorso(String codice) {
        return corsoDao.trovaPerCodice(codice);
    }

    public boolean modificaCorso(String codice, String nuovoTitolo, int nuoveOre, double nuovoPrezzo) {
        Optional<Corso> trovato = corsoDao.trovaPerCodice(codice);
        if (trovato.isEmpty()) {
            return false;
        }

        Corso corso = trovato.get();
        corso.setTitolo(nuovoTitolo);
        corso.setOre(nuoveOre);
        corso.setPrezzo(nuovoPrezzo);
        return corsoDao.aggiorna(corso);
    }

    public boolean eliminaCorso(String codice) {
        return corsoDao.elimina(codice);
    }
}
```

Il service conosce le regole applicative e usa il DAO attraverso l'interfaccia.

## Passo 5 - Creare la view

File:

```text
src/corso/ud21/catalogo/view/CatalogoView.java
```

```java
package corso.ud21.catalogo.view;

import java.util.List;
import java.util.Scanner;

import corso.ud21.catalogo.model.Corso;

public class CatalogoView {
    private final Scanner scanner = new Scanner(System.in);

    public int leggiSceltaMenu() {
        System.out.println();
        System.out.println("=== Catalogo corsi ===");
        System.out.println("1. Inserisci corso");
        System.out.println("2. Elenca corsi");
        System.out.println("3. Cerca corso");
        System.out.println("4. Modifica corso");
        System.out.println("5. Elimina corso");
        System.out.println("0. Esci");
        System.out.print("Scelta: ");
        return leggiIntero();
    }

    public String leggiTesto(String messaggio) {
        System.out.print(messaggio);
        return scanner.nextLine().trim();
    }

    public int leggiIntero(String messaggio) {
        System.out.print(messaggio);
        return leggiIntero();
    }

    public double leggiDouble(String messaggio) {
        System.out.print(messaggio);
        while (!scanner.hasNextDouble()) {
            System.out.print("Valore non valido. Riprovare: ");
            scanner.nextLine();
        }
        double valore = scanner.nextDouble();
        scanner.nextLine();
        return valore;
    }

    public void mostraCorsi(List<Corso> corsi) {
        if (corsi.isEmpty()) {
            mostraMessaggio("Nessun corso presente.");
            return;
        }
        for (Corso corso : corsi) {
            System.out.println(corso);
        }
    }

    public void mostraCorso(Corso corso) {
        System.out.println(corso);
    }

    public void mostraMessaggio(String messaggio) {
        System.out.println(messaggio);
    }

    private int leggiIntero() {
        while (!scanner.hasNextInt()) {
            System.out.print("Valore non valido. Riprovare: ");
            scanner.nextLine();
        }
        int valore = scanner.nextInt();
        scanner.nextLine();
        return valore;
    }
}
```

Nota importante: lo `Scanner` è nella view. Le altre classi non leggono direttamente da tastiera.

## Passo 6 - Creare il controller

File:

```text
src/corso/ud21/catalogo/controller/CatalogoController.java
```

```java
package corso.ud21.catalogo.controller;

import java.util.Optional;

import corso.ud21.catalogo.model.Corso;
import corso.ud21.catalogo.service.CatalogoService;
import corso.ud21.catalogo.view.CatalogoView;

public class CatalogoController {
    private final CatalogoService service;
    private final CatalogoView view;

    public CatalogoController(CatalogoService service, CatalogoView view) {
        this.service = service;
        this.view = view;
    }

    public void avvia() {
        int scelta;
        do {
            scelta = view.leggiSceltaMenu();
            gestisciScelta(scelta);
        } while (scelta != 0);
    }

    private void gestisciScelta(int scelta) {
        try {
            switch (scelta) {
                case 1 -> inserisci();
                case 2 -> view.mostraCorsi(service.elencoCorsi());
                case 3 -> cerca();
                case 4 -> modifica();
                case 5 -> elimina();
                case 0 -> view.mostraMessaggio("Chiusura applicazione.");
                default -> view.mostraMessaggio("Scelta non valida.");
            }
        } catch (IllegalArgumentException ex) {
            view.mostraMessaggio("Errore: " + ex.getMessage());
        }
    }

    private void inserisci() {
        String codice = view.leggiTesto("Codice: ");
        String titolo = view.leggiTesto("Titolo: ");
        int ore = view.leggiIntero("Ore: ");
        double prezzo = view.leggiDouble("Prezzo: ");
        service.creaCorso(codice, titolo, ore, prezzo);
        view.mostraMessaggio("Corso inserito.");
    }

    private void cerca() {
        String codice = view.leggiTesto("Codice da cercare: ");
        Optional<Corso> corso = service.cercaCorso(codice);
        if (corso.isPresent()) {
            view.mostraCorso(corso.get());
        } else {
            view.mostraMessaggio("Corso non trovato.");
        }
    }

    private void modifica() {
        String codice = view.leggiTesto("Codice da modificare: ");
        String titolo = view.leggiTesto("Nuovo titolo: ");
        int ore = view.leggiIntero("Nuove ore: ");
        double prezzo = view.leggiDouble("Nuovo prezzo: ");
        boolean modificato = service.modificaCorso(codice, titolo, ore, prezzo);
        view.mostraMessaggio(modificato ? "Corso modificato." : "Corso non trovato.");
    }

    private void elimina() {
        String codice = view.leggiTesto("Codice da eliminare: ");
        boolean eliminato = service.eliminaCorso(codice);
        view.mostraMessaggio(eliminato ? "Corso eliminato." : "Corso non trovato.");
    }
}
```

## Passo 7 - Creare il main

File:

```text
src/corso/ud21/catalogo/app/EseguiCatalogoCorsi.java
```

```java
package corso.ud21.catalogo.app;

import corso.ud21.catalogo.controller.CatalogoController;
import corso.ud21.catalogo.dao.CorsoDao;
import corso.ud21.catalogo.dao.CorsoDaoInMemoria;
import corso.ud21.catalogo.service.CatalogoService;
import corso.ud21.catalogo.view.CatalogoView;

public class EseguiCatalogoCorsi {
    public static void main(String[] args) {
        CorsoDao dao = CorsoDaoInMemoria.getInstance();
        CatalogoService service = new CatalogoService(dao);
        CatalogoView view = new CatalogoView();
        CatalogoController controller = new CatalogoController(service, view);
        controller.avvia();
    }
}
```

Il `main` non contiene logica CRUD. Si limita a collegare gli oggetti necessari.

## Passo 8 - Compilazione ed esecuzione

Da dentro la cartella `UD21_catalogo_corsi_crud`:

```bash
javac -encoding UTF-8 -d out $(find src -name "*.java")
java -cp out corso.ud21.catalogo.app.EseguiCatalogoCorsi
```

Su Windows PowerShell, se `find` non è disponibile:

```powershell
Get-ChildItem -Recurse -Filter *.java src | ForEach-Object { $_.FullName } > sources.txt
javac -encoding UTF-8 -d out @sources.txt
java -cp out corso.ud21.catalogo.app.EseguiCatalogoCorsi
```

## Test minimi da svolgere

Eseguire almeno queste verifiche:

| Test | Risultato atteso |
|---|---|
| Inserire un corso valido | Il corso viene aggiunto |
| Inserire due corsi con stesso codice | Il secondo inserimento viene rifiutato |
| Cercare un codice esistente | Il corso viene mostrato |
| Cercare un codice inesistente | Messaggio di non trovato |
| Modificare un corso esistente | I dati cambiano |
| Eliminare un corso esistente | Il corso viene rimosso |
| Elencare dopo eliminazione | Il corso eliminato non compare |

## Evidenza richiesta

Creare il file:

```text
docs/evidence_LAB21_guidato.md
```

Inserire:

1. struttura delle classi realizzate;
2. screenshot o copia testuale di almeno cinque operazioni CRUD;
3. spiegazione del ruolo di `CorsoDao`;
4. spiegazione del ruolo di `CorsoDaoInMemoria.getInstance()`;
5. breve commento su cosa cambierebbe passando a un DAO JDBC.
