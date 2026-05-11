# 03 - LAB14 guidato - Vetrina immobiliare polimorfica

## Scenario

Una piccola agenzia immobiliare vuole realizzare una vetrina console per mostrare annunci di immobili diversi.

Gli immobili hanno dati comuni:

- indirizzo;
- superficie;
- prezzo.

Ogni tipologia ha però dettagli specifici:

- `Appartamento`: numero vani e piano;
- `Villa`: metri quadri di giardino e presenza piscina;
- `BoxAuto`: presenza serranda automatica.

La vetrina deve lavorare con oggetti diversi senza dipendere direttamente dalle classi concrete.

---

## Obiettivi del laboratorio

Al termine del laboratorio il partecipante deve aver realizzato una struttura in cui:

1. `Pubblicabile` definisce il contratto degli oggetti pubblicabili;
2. `Immobile` fornisce stato e comportamento comuni;
3. `Appartamento`, `Villa` e `BoxAuto` specializzano il comportamento;
4. `VetrinaService` usa `Pubblicabile[]` e non dipende dalle classi concrete;
5. il programma principale dimostra il binding dinamico;
6. l'aggiunta di una nuova tipologia richiede modifiche limitate.

---

## Strumenti e installazioni necessarie

Per questo laboratorio non servono strumenti aggiuntivi rispetto all'ambiente base Java.

| Strumento | Serve in questo lab | Note |
|---|---:|---|
| JDK | sì | necessario per `javac` e `java` |
| VS Code | sì | editor consigliato |
| Git | sì | per versionare il lavoro |
| Maven | no | non viene usato in UD14.v2 |
| Docker | no | non viene usato in UD14.v2 |
| MariaDB/MySQL | no | verrà introdotto nelle UD database |
| phpMyAdmin | no | verrà introdotto nelle UD database |
| Bootstrap/jQuery | no | verranno introdotti nella parte web |

### Verifica ambiente

Da terminale:

```bash
java -version
javac -version
git --version
```

Se uno dei comandi non è riconosciuto, usare i passaggi indicati nel `README.md` della UD.

---

## Struttura delle cartelle

Creare questa struttura:

```text
UD14_vetrina_immobiliare/
  src/
    corso/
      ud14/
        vetrina/
          Pubblicabile.java
          Immobile.java
          Appartamento.java
          Villa.java
          BoxAuto.java
          VetrinaService.java
          EseguiVetrinaImmobiliare.java
  docs/
    evidence_UD14.md
```

Da terminale:

```bash
mkdir -p UD14_vetrina_immobiliare/src/corso/ud14/vetrina
mkdir -p UD14_vetrina_immobiliare/docs
cd UD14_vetrina_immobiliare
```

Su Windows PowerShell, se `mkdir -p` non funziona:

```powershell
mkdir UD14_vetrina_immobiliare
cd UD14_vetrina_immobiliare
mkdir src
mkdir src\corso
mkdir src\corso\ud14
mkdir src\corso\ud14\vetrina
mkdir docs
```

---

## Passo 1 - Creare l'interfaccia Pubblicabile

File:

```text
src/corso/ud14/vetrina/Pubblicabile.java
```

Codice:

```java
package corso.ud14.vetrina;

public interface Pubblicabile {
    String getTitoloPubblico();
    String getDescrizionePubblica();
    double getPrezzoPubblico();
    boolean isPubblicabile();
}
```

### Osservazione

L'interfaccia non contiene i dati dell'immobile.

Definisce solo ciò che serve alla vetrina:

- un titolo;
- una descrizione;
- un prezzo da mostrare;
- una regola per capire se l'elemento è pubblicabile.

---

## Passo 2 - Creare la classe astratta Immobile

File:

```text
src/corso/ud14/vetrina/Immobile.java
```

Codice:

```java
package corso.ud14.vetrina;

public abstract class Immobile implements Pubblicabile {
    private String indirizzo;
    private double superficieMq;
    private double prezzo;

    public Immobile(String indirizzo, double superficieMq, double prezzo) {
        this.indirizzo = indirizzo;
        this.superficieMq = superficieMq;
        this.prezzo = prezzo;
    }

    public String getIndirizzo() {
        return indirizzo;
    }

    public double getSuperficieMq() {
        return superficieMq;
    }

    public double getPrezzo() {
        return prezzo;
    }

    public double getPrezzoMq() {
        if (superficieMq <= 0) {
            return 0;
        }
        return prezzo / superficieMq;
    }

    public abstract String getTipologia();

    @Override
    public String getTitoloPubblico() {
        return getTipologia() + " - " + indirizzo;
    }

    @Override
    public String getDescrizionePubblica() {
        return getTipologia()
                + " in " + indirizzo
                + ", superficie " + superficieMq + " mq"
                + ", prezzo " + prezzo + " euro";
    }

    @Override
    public double getPrezzoPubblico() {
        return prezzo;
    }

    @Override
    public boolean isPubblicabile() {
        return indirizzo != null
                && !indirizzo.isBlank()
                && superficieMq > 0
                && prezzo > 0;
    }
}
```

### Osservazione

`Immobile` è astratta perché non vogliamo creare un immobile generico senza tipologia.

La classe contiene:

- attributi comuni;
- costruttore comune;
- metodi comuni;
- un metodo astratto `getTipologia()` che le sottoclassi devono implementare.

---

## Passo 3 - Creare Appartamento

File:

```text
src/corso/ud14/vetrina/Appartamento.java
```

Codice:

```java
package corso.ud14.vetrina;

public class Appartamento extends Immobile {
    private int numeroVani;
    private int piano;

    public Appartamento(String indirizzo, double superficieMq, double prezzo, int numeroVani, int piano) {
        super(indirizzo, superficieMq, prezzo);
        this.numeroVani = numeroVani;
        this.piano = piano;
    }

    public int getNumeroVani() {
        return numeroVani;
    }

    public int getPiano() {
        return piano;
    }

    @Override
    public String getTipologia() {
        return "Appartamento";
    }

    @Override
    public String getDescrizionePubblica() {
        return super.getDescrizionePubblica()
                + ", vani " + numeroVani
                + ", piano " + piano
                + ", prezzo/mq " + String.format("%.2f", getPrezzoMq()) + " euro";
    }
}
```

### Punto didattico

`Appartamento` eredita molti comportamenti da `Immobile`, ma ridefinisce la descrizione pubblica per aggiungere i propri dettagli.

---

## Passo 4 - Creare Villa

File:

```text
src/corso/ud14/vetrina/Villa.java
```

Codice:

```java
package corso.ud14.vetrina;

public class Villa extends Immobile {
    private double giardinoMq;
    private boolean piscina;

    public Villa(String indirizzo, double superficieMq, double prezzo, double giardinoMq, boolean piscina) {
        super(indirizzo, superficieMq, prezzo);
        this.giardinoMq = giardinoMq;
        this.piscina = piscina;
    }

    public double getGiardinoMq() {
        return giardinoMq;
    }

    public boolean hasPiscina() {
        return piscina;
    }

    @Override
    public String getTipologia() {
        return "Villa";
    }

    @Override
    public String getDescrizionePubblica() {
        String testoPiscina = piscina ? "con piscina" : "senza piscina";

        return super.getDescrizionePubblica()
                + ", giardino " + giardinoMq + " mq"
                + ", " + testoPiscina
                + ", prezzo/mq " + String.format("%.2f", getPrezzoMq()) + " euro";
    }

    @Override
    public boolean isPubblicabile() {
        return super.isPubblicabile() && giardinoMq >= 0;
    }
}
```

### Punto didattico

`Villa` ridefinisce sia la descrizione sia la regola di pubblicabilità.

Questo è importante: il servizio non deve sapere quali controlli sono specifici di una villa. Deve solo chiamare `isPubblicabile()`.

---

## Passo 5 - Creare BoxAuto

File:

```text
src/corso/ud14/vetrina/BoxAuto.java
```

Codice:

```java
package corso.ud14.vetrina;

public class BoxAuto extends Immobile {
    private boolean serrandaAutomatica;

    public BoxAuto(String indirizzo, double superficieMq, double prezzo, boolean serrandaAutomatica) {
        super(indirizzo, superficieMq, prezzo);
        this.serrandaAutomatica = serrandaAutomatica;
    }

    public boolean hasSerrandaAutomatica() {
        return serrandaAutomatica;
    }

    @Override
    public String getTipologia() {
        return "Box auto";
    }

    @Override
    public String getDescrizionePubblica() {
        String testoSerranda = serrandaAutomatica ? "serranda automatica" : "serranda manuale";

        return super.getDescrizionePubblica()
                + ", " + testoSerranda;
    }

    @Override
    public boolean isPubblicabile() {
        return super.isPubblicabile() && getSuperficieMq() >= 12;
    }
}
```

### Punto didattico

`BoxAuto` modifica la regola di pubblicabilità: un box troppo piccolo non deve essere pubblicato.

Il controllo resta dentro la classe concreta.

---

## Passo 6 - Creare VetrinaService

File:

```text
src/corso/ud14/vetrina/VetrinaService.java
```

Codice:

```java
package corso.ud14.vetrina;

public class VetrinaService {

    public void stampaScheda(Pubblicabile elemento) {
        if (elemento == null) {
            System.out.println("Elemento non disponibile");
            return;
        }

        if (!elemento.isPubblicabile()) {
            System.out.println("Elemento non pubblicabile: " + elemento.getTitoloPubblico());
            return;
        }

        System.out.println("------------------------------");
        System.out.println(elemento.getTitoloPubblico());
        System.out.println(elemento.getDescrizionePubblica());
        System.out.println("Prezzo pubblico: " + elemento.getPrezzoPubblico() + " euro");
    }

    public void stampaCatalogo(Pubblicabile[] elementi) {
        if (elementi == null || elementi.length == 0) {
            System.out.println("Catalogo vuoto");
            return;
        }

        for (Pubblicabile elemento : elementi) {
            stampaScheda(elemento);
        }
    }

    public int contaPubblicabili(Pubblicabile[] elementi) {
        if (elementi == null) {
            return 0;
        }

        int contatore = 0;

        for (Pubblicabile elemento : elementi) {
            if (elemento != null && elemento.isPubblicabile()) {
                contatore++;
            }
        }

        return contatore;
    }

    public double calcolaValoreTotalePubblicabile(Pubblicabile[] elementi) {
        if (elementi == null) {
            return 0;
        }

        double totale = 0;

        for (Pubblicabile elemento : elementi) {
            if (elemento != null && elemento.isPubblicabile()) {
                totale += elemento.getPrezzoPubblico();
            }
        }

        return totale;
    }

    public Pubblicabile trovaElementoMenoCostoso(Pubblicabile[] elementi) {
        if (elementi == null) {
            return null;
        }

        Pubblicabile migliore = null;

        for (Pubblicabile elemento : elementi) {
            if (elemento == null || !elemento.isPubblicabile()) {
                continue;
            }

            if (migliore == null || elemento.getPrezzoPubblico() < migliore.getPrezzoPubblico()) {
                migliore = elemento;
            }
        }

        return migliore;
    }
}
```

### Punto didattico

`VetrinaService` non riceve `Villa`, `Appartamento` o `BoxAuto`.

Riceve `Pubblicabile`.

Questo significa che il servizio lavora sul contratto necessario alla pubblicazione, non sulle classi concrete.

---

## Passo 7 - Creare il programma principale

File:

```text
src/corso/ud14/vetrina/EseguiVetrinaImmobiliare.java
```

Codice:

```java
package corso.ud14.vetrina;

public class EseguiVetrinaImmobiliare {
    public static void main(String[] args) {
        Pubblicabile[] elementi = new Pubblicabile[5];

        elementi[0] = new Appartamento("Via Roma 10", 85.0, 168000.0, 3, 4);
        elementi[1] = new Villa("Via dei Pini 7", 180.0, 420000.0, 500.0, true);
        elementi[2] = new BoxAuto("Via Verdi 2", 18.0, 28000.0, true);
        elementi[3] = new BoxAuto("Via Piccola 1", 9.0, 12000.0, false);
        elementi[4] = new Villa("", 150.0, 350000.0, 300.0, false);

        VetrinaService service = new VetrinaService();

        System.out.println("CATALOGO PUBBLICABILE");
        service.stampaCatalogo(elementi);

        System.out.println();
        System.out.println("RIEPILOGO");
        System.out.println("Elementi pubblicabili: " + service.contaPubblicabili(elementi));
        System.out.println("Valore totale pubblicabile: " + service.calcolaValoreTotalePubblicabile(elementi) + " euro");

        Pubblicabile menoCostoso = service.trovaElementoMenoCostoso(elementi);

        if (menoCostoso != null) {
            System.out.println("Elemento meno costoso: " + menoCostoso.getTitoloPubblico());
        }
    }
}
```

### Punto didattico

L'array è dichiarato così:

```java
Pubblicabile[] elementi = new Pubblicabile[5];
```

ma contiene oggetti concreti diversi:

```java
new Appartamento(...)
new Villa(...)
new BoxAuto(...)
```

Il programma dimostra che ciò che conta per la vetrina è il contratto `Pubblicabile`.

---

## Passo 8 - Compilare

Dalla cartella `UD14_vetrina_immobiliare`:

```bash
javac -d out src/corso/ud14/vetrina/*.java
```

Se la compilazione va a buon fine, viene creata la cartella:

```text
out/
```

con i file `.class` organizzati secondo il package.

---

## Passo 9 - Eseguire

Sempre dalla cartella `UD14_vetrina_immobiliare`:

```bash
java -cp out corso.ud14.vetrina.EseguiVetrinaImmobiliare
```

Output atteso simile:

```text
CATALOGO PUBBLICABILE
------------------------------
Appartamento - Via Roma 10
Appartamento in Via Roma 10, superficie 85.0 mq, prezzo 168000.0 euro, vani 3, piano 4, prezzo/mq 1976.47 euro
Prezzo pubblico: 168000.0 euro
------------------------------
Villa - Via dei Pini 7
Villa in Via dei Pini 7, superficie 180.0 mq, prezzo 420000.0 euro, giardino 500.0 mq, con piscina, prezzo/mq 2333.33 euro
Prezzo pubblico: 420000.0 euro
------------------------------
Box auto - Via Verdi 2
Box auto in Via Verdi 2, superficie 18.0 mq, prezzo 28000.0 euro, serranda automatica
Prezzo pubblico: 28000.0 euro
Elemento non pubblicabile: Box auto - Via Piccola 1
Elemento non pubblicabile: Villa - 

RIEPILOGO
Elementi pubblicabili: 3
Valore totale pubblicabile: 616000.0 euro
Elemento meno costoso: Box auto - Via Verdi 2
```

---

## Passo 10 - Verifica progettuale

Rispondere nel file `docs/evidence_UD14.md`.

### Domanda 1

Perché `VetrinaService` riceve `Pubblicabile` e non `Villa` o `Appartamento`?

### Domanda 2

Quale metodo viene eseguito quando il servizio chiama:

```java
elemento.getDescrizionePubblica();
```

su un oggetto reale `Villa`?

### Domanda 3

Che cosa bisognerebbe modificare per aggiungere una nuova classe `LocaleCommerciale`?

### Domanda 4

Perché in `VetrinaService` non ci sono controlli come:

```java
if (elemento instanceof Villa) {
    ...
}
```

### Domanda 5

Quale responsabilità ha `Immobile` e quale responsabilità ha `Pubblicabile`?

---

## Passo 11 - Estensione guidata

Aggiungere una nuova classe:

```text
LocaleCommerciale
```

Requisiti:

- deve estendere `Immobile`;
- deve avere un attributo `categoriaCatastale`;
- deve avere un attributo `adattoRistorazione`;
- deve ridefinire `getTipologia()`;
- deve ridefinire `getDescrizionePubblica()`;
- deve essere pubblicabile solo se il prezzo è maggiore di zero, la superficie è almeno 30 mq e la categoria catastale non è vuota.

Dopo aver creato la classe, aggiungere un oggetto all'array nel `main`.

Verificare se `VetrinaService` deve essere modificato.

Risposta attesa:

```text
VetrinaService non deve essere modificato, perché lavora con Pubblicabile.
```

---

## Passo 12 - Evidenza finale

Nel file:

```text
docs/evidence_UD14.md
```

inserire:

```md
# Evidence UD14

## Ambiente

- java -version:
- javac -version:
- git --version:

## Comandi eseguiti

```bash
javac -d out src/corso/ud14/vetrina/*.java
java -cp out corso.ud14.vetrina.EseguiVetrinaImmobiliare
```

## Output

Incollare qui l'output.

## Schema classi

Inserire uno schema Mermaid o testuale.

## Risposte progettuali

1. ...
2. ...
3. ...
4. ...
5. ...

## Estensione LocaleCommerciale

Descrivere le modifiche fatte e indicare se il servizio è stato modificato.
```

---

## Criteri di riuscita

Il laboratorio è completato quando:

- il codice compila;
- il programma viene eseguito;
- il servizio usa `Pubblicabile`;
- le classi concrete ridefiniscono i metodi necessari;
- è stata aggiunta almeno una nuova tipologia senza modificare la logica centrale del servizio;
- il file di evidenza contiene output e motivazioni progettuali.
