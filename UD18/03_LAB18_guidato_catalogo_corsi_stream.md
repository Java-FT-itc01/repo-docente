# 03 - LAB18 guidato - Catalogo corsi con Lambda e Stream

## Scenario

Una società di formazione gestisce un piccolo catalogo di corsi. Ogni corso ha titolo, area, durata, prezzo e stato di pubblicazione.

Nella UD17 sarebbe naturale risolvere le richieste con cicli `for`. In questa UD si rifattorizza il codice usando lambda e stream, mantenendo una struttura comprensibile.

## Obiettivi del laboratorio

Al termine del laboratorio il partecipante deve saper:

- creare una `List` di oggetti di dominio;
- filtrare oggetti con `filter`;
- trasformare oggetti con `map`;
- ordinare con lambda e `sorted`;
- contare elementi con `count`;
- verificare condizioni con `anyMatch`;
- cercare il primo elemento con `findFirst`;
- raccogliere risultati in una nuova lista con `collect`;
- confrontare una soluzione con cicli e una soluzione con stream.

## Software e tool necessari

| Strumento | Necessario |
|---|---:|
| JDK | sì |
| VS Code o IDE Java equivalente | sì |
| Git | sì |

Non sono previste nuove installazioni per questo laboratorio.

Verifica minima:

```bash
java -version
javac -version
```

## Struttura del progetto

Creare la seguente struttura:

```text
UD18_catalogo_corsi_stream/
  src/
    corso/
      ud18/
        catalogo/
          Corso.java
          CatalogoService.java
          EseguiCatalogoStream.java
  docs/
    evidence_UD18_guidato.md
```

## Passo 1 - Creare la classe `Corso`

File:

```text
src/corso/ud18/catalogo/Corso.java
```

Codice:

```java
package corso.ud18.catalogo;

public class Corso {
    private String codice;
    private String titolo;
    private String area;
    private int durataOre;
    private double prezzo;
    private boolean pubblicato;

    public Corso(String codice, String titolo, String area, int durataOre, double prezzo, boolean pubblicato) {
        this.codice = codice;
        this.titolo = titolo;
        this.area = area;
        this.durataOre = durataOre;
        this.prezzo = prezzo;
        this.pubblicato = pubblicato;
    }

    public String getCodice() {
        return codice;
    }

    public String getTitolo() {
        return titolo;
    }

    public String getArea() {
        return area;
    }

    public int getDurataOre() {
        return durataOre;
    }

    public double getPrezzo() {
        return prezzo;
    }

    public boolean isPubblicato() {
        return pubblicato;
    }

    @Override
    public String toString() {
        return codice + " - " + titolo + " [" + area + ", " + durataOre + " ore, " + prezzo + " euro, pubblicato=" + pubblicato + "]";
    }
}
```

## Passo 2 - Creare una classe di servizio

File:

```text
src/corso/ud18/catalogo/CatalogoService.java
```

Import necessari:

```java
package corso.ud18.catalogo;

import java.util.List;
import java.util.Optional;
import java.util.stream.Collectors;
```

Creare la classe:

```java
public class CatalogoService {

}
```

Nei passi successivi verranno aggiunti i metodi.

## Passo 3 - Filtrare i corsi pubblicati

Aggiungere in `CatalogoService`:

```java
public List<Corso> trovaCorsiPubblicati(List<Corso> corsi) {
    return corsi.stream()
            .filter(corso -> corso.isPubblicato())
            .collect(Collectors.toList());
}
```

Versione equivalente con method reference:

```java
public List<Corso> trovaCorsiPubblicati(List<Corso> corsi) {
    return corsi.stream()
            .filter(Corso::isPubblicato)
            .collect(Collectors.toList());
}
```

Usare la seconda versione quando il metodo richiamato è già chiaro.

## Passo 4 - Filtrare per area

Aggiungere:

```java
public List<Corso> trovaPerArea(List<Corso> corsi, String area) {
    return corsi.stream()
            .filter(corso -> corso.getArea().equalsIgnoreCase(area))
            .collect(Collectors.toList());
}
```

Domanda guida:

- perché in questo caso non si può usare direttamente `Corso::getArea` dentro `filter`?

Risposta attesa:

- `filter` richiede una condizione booleana;
- `getArea()` restituisce una stringa;
- serve quindi una lambda che confronti l'area del corso con il parametro ricevuto.

## Passo 5 - Estrarre i titoli dei corsi pubblicati

Aggiungere:

```java
public List<String> titoliCorsiPubblicati(List<Corso> corsi) {
    return corsi.stream()
            .filter(Corso::isPubblicato)
            .map(Corso::getTitolo)
            .collect(Collectors.toList());
}
```

Qui `map` trasforma ogni `Corso` in una `String`.

## Passo 6 - Ordinare i corsi per durata

Aggiungere:

```java
public List<Corso> ordinaPerDurataCrescente(List<Corso> corsi) {
    return corsi.stream()
            .sorted((c1, c2) -> Integer.compare(c1.getDurataOre(), c2.getDurataOre()))
            .collect(Collectors.toList());
}
```

Nota:

```java
Integer.compare(a, b)
```

è preferibile a sottrazioni come:

```java
c1.getDurataOre() - c2.getDurataOre()
```

perché esprime meglio l'intenzione ed evita alcune situazioni problematiche con valori numerici grandi.

## Passo 7 - Contare i corsi pubblicati di una certa area

Aggiungere:

```java
public long contaCorsiPubblicatiPerArea(List<Corso> corsi, String area) {
    return corsi.stream()
            .filter(Corso::isPubblicato)
            .filter(corso -> corso.getArea().equalsIgnoreCase(area))
            .count();
}
```

Osservazione:

La pipeline usa due `filter` separati. Questa forma spesso è più leggibile di una sola condizione lunga.

## Passo 8 - Verificare se esiste un corso sopra una certa soglia di prezzo

Aggiungere:

```java
public boolean esisteCorsoConPrezzoMaggioreDi(List<Corso> corsi, double sogliaPrezzo) {
    return corsi.stream()
            .anyMatch(corso -> corso.getPrezzo() > sogliaPrezzo);
}
```

`anyMatch` restituisce `true` appena trova almeno un elemento coerente con la regola.

## Passo 9 - Trovare il primo corso pubblicato di una certa area

Aggiungere:

```java
public Optional<Corso> primoCorsoPubblicatoPerArea(List<Corso> corsi, String area) {
    return corsi.stream()
            .filter(Corso::isPubblicato)
            .filter(corso -> corso.getArea().equalsIgnoreCase(area))
            .findFirst();
}
```

Il risultato è `Optional<Corso>` perché il corso potrebbe non esistere.

Uso base:

```java
Optional<Corso> risultato = service.primoCorsoPubblicatoPerArea(corsi, "Java");

if (risultato.isPresent()) {
    System.out.println(risultato.get());
} else {
    System.out.println("Nessun corso trovato");
}
```

## Passo 10 - Calcolare il valore totale del catalogo pubblicato

Aggiungere:

```java
public double calcolaValoreCatalogoPubblicato(List<Corso> corsi) {
    return corsi.stream()
            .filter(Corso::isPubblicato)
            .map(Corso::getPrezzo)
            .reduce(0.0, (totale, prezzo) -> totale + prezzo);
}
```

Questa pipeline:

1. considera solo i corsi pubblicati;
2. trasforma ogni corso nel suo prezzo;
3. somma i prezzi.

## Passo 11 - Creare la classe di avvio

File:

```text
src/corso/ud18/catalogo/EseguiCatalogoStream.java
```

Codice:

```java
package corso.ud18.catalogo;

import java.util.ArrayList;
import java.util.List;
import java.util.Optional;

public class EseguiCatalogoStream {
    public static void main(String[] args) {
        List<Corso> corsi = new ArrayList<>();
        corsi.add(new Corso("JAVA-BASE", "Java Base", "Java", 40, 900.0, true));
        corsi.add(new Corso("JAVA-OO", "Java Object Oriented", "Java", 32, 850.0, true));
        corsi.add(new Corso("SQL-01", "SQL Operativo", "Database", 24, 600.0, true));
        corsi.add(new Corso("WEB-01", "HTML CSS JavaScript", "Web", 28, 700.0, false));
        corsi.add(new Corso("SPRING-01", "Spring Boot Foundations", "Java", 40, 1200.0, false));

        CatalogoService service = new CatalogoService();

        System.out.println("=== Corsi pubblicati ===");
        service.trovaCorsiPubblicati(corsi).forEach(System.out::println);

        System.out.println("\n=== Titoli corsi pubblicati ===");
        service.titoliCorsiPubblicati(corsi).forEach(System.out::println);

        System.out.println("\n=== Corsi ordinati per durata ===");
        service.ordinaPerDurataCrescente(corsi).forEach(System.out::println);

        System.out.println("\n=== Conteggio corsi Java pubblicati ===");
        long totaleJava = service.contaCorsiPubblicatiPerArea(corsi, "Java");
        System.out.println(totaleJava);

        System.out.println("\n=== Esiste corso oltre 1000 euro? ===");
        boolean esisteCostoso = service.esisteCorsoConPrezzoMaggioreDi(corsi, 1000.0);
        System.out.println(esisteCostoso);

        System.out.println("\n=== Primo corso Java pubblicato ===");
        Optional<Corso> primoJava = service.primoCorsoPubblicatoPerArea(corsi, "Java");
        if (primoJava.isPresent()) {
            System.out.println(primoJava.get());
        } else {
            System.out.println("Nessun corso trovato");
        }

        System.out.println("\n=== Valore catalogo pubblicato ===");
        double valore = service.calcolaValoreCatalogoPubblicato(corsi);
        System.out.println(valore);
    }
}
```

## Passo 12 - Compilazione ed esecuzione

Dalla cartella `UD18_catalogo_corsi_stream` eseguire:

```bash
javac -encoding UTF-8 -d out src/corso/ud18/catalogo/*.java
java -cp out corso.ud18.catalogo.EseguiCatalogoStream
```

## Passo 13 - Evidenze richieste

Nel file:

```text
docs/evidence_UD18_guidato.md
```

inserire:

1. comando di compilazione usato;
2. comando di esecuzione usato;
3. output ottenuto;
4. risposta alle domande:
   - quale metodo usa `filter`?
   - quale metodo usa `map`?
   - quale metodo usa `sorted`?
   - quale metodo usa `anyMatch`?
   - quale metodo usa `findFirst`?
   - quale metodo usa `reduce`?
5. breve confronto tra ciclo tradizionale e stream.

## Domande di consolidamento

### Domanda 1

Perché `filter` deve ricevere una condizione booleana?

### Domanda 2

Perché `map` può cambiare il tipo degli elementi nella pipeline?

### Domanda 3

Perché `findFirst` restituisce un `Optional`?

### Domanda 4

Una pipeline stream modifica la lista originale?

### Domanda 5

Quando un ciclo `for` tradizionale può essere preferibile a uno stream?
