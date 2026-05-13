# 02 - Stream pipeline base

## 1. Che cos'è uno stream

Uno stream è una sequenza di elementi su cui è possibile costruire una pipeline di operazioni.

Uno stream non è una collezione.

Una `List` contiene dati. Uno stream permette di attraversare quei dati applicando operazioni.

Esempio:

```java
corsi.stream()
     .filter(corso -> corso.isAttivo())
     .map(corso -> corso.getTitolo())
     .forEach(titolo -> System.out.println(titolo));
```

La pipeline legge così:

1. parti dalla lista dei corsi;
2. considera solo i corsi attivi;
3. estrai il titolo;
4. stampa ogni titolo.

## 2. Operazioni intermedie e terminali

Le operazioni stream si dividono in due categorie.

| Tipo | Descrizione | Esempi |
|---|---|---|
| Intermedie | restituiscono un nuovo stream | `filter`, `map`, `sorted`, `limit` |
| Terminali | chiudono la pipeline e producono un risultato | `forEach`, `count`, `collect`, `anyMatch`, `findFirst`, `reduce` |

Una pipeline deve terminare con una operazione terminale.

Esempio incompleto:

```java
corsi.stream()
     .filter(corso -> corso.isAttivo());
```

Il codice compila, ma non produce alcun risultato utile perché manca l'operazione terminale.

Esempio completo:

```java
long totale = corsi.stream()
                   .filter(corso -> corso.isAttivo())
                   .count();
```

## 3. `filter`

`filter` mantiene solo gli elementi che rispettano una condizione.

```java
List<Corso> corsiAttivi = corsi.stream()
        .filter(corso -> corso.isAttivo())
        .collect(Collectors.toList());
```

La condizione deve restituire `true` o `false`.

## 4. `map`

`map` trasforma ogni elemento in qualcos'altro.

Esempio: da `Corso` a `String`.

```java
List<String> titoli = corsi.stream()
        .map(corso -> corso.getTitolo())
        .collect(Collectors.toList());
```

Con method reference:

```java
List<String> titoli = corsi.stream()
        .map(Corso::getTitolo)
        .collect(Collectors.toList());
```

## 5. `sorted`

`sorted` ordina gli elementi.

```java
List<Corso> ordinati = corsi.stream()
        .sorted((c1, c2) -> c1.getTitolo().compareTo(c2.getTitolo()))
        .collect(Collectors.toList());
```

Per ordinare numeri è utile `Integer.compare` o `Double.compare`.

```java
List<Corso> perDurata = corsi.stream()
        .sorted((c1, c2) -> Integer.compare(c1.getDurataOre(), c2.getDurataOre()))
        .collect(Collectors.toList());
```

## 6. `count`

`count` conta gli elementi presenti nello stream.

```java
long corsiJava = corsi.stream()
        .filter(corso -> corso.getArea().equalsIgnoreCase("Java"))
        .count();
```

## 7. `anyMatch`

`anyMatch` verifica se almeno un elemento rispetta una condizione.

```java
boolean esisteCorsoCostoso = corsi.stream()
        .anyMatch(corso -> corso.getPrezzo() > 1000.0);
```

## 8. `findFirst`

`findFirst` restituisce il primo elemento coerente con la pipeline.

```java
Optional<Corso> primoCorsoJava = corsi.stream()
        .filter(corso -> corso.getArea().equalsIgnoreCase("Java"))
        .findFirst();
```

Il risultato è un `Optional<Corso>` perché potrebbe non esistere alcun elemento.

Uso base:

```java
if (primoCorsoJava.isPresent()) {
    System.out.println(primoCorsoJava.get().getTitolo());
}
```

## 9. `reduce`

`reduce` combina gli elementi per ottenere un risultato unico.

Esempio: somma dei prezzi.

```java
double totale = corsi.stream()
        .map(corso -> corso.getPrezzo())
        .reduce(0.0, (a, b) -> a + b);
```

Per questa UD basta comprenderne l'uso base. In molti casi reali si useranno metodi più specifici o collector dedicati.

## 10. `collect`

`collect` produce una nuova collezione o un risultato composto.

```java
List<Corso> corsiAttivi = corsi.stream()
        .filter(Corso::isAttivo)
        .collect(Collectors.toList());
```

Per usare `Collectors` serve l'import:

```java
import java.util.stream.Collectors;
```

## 11. Stream e modifica dei dati

Uno stream non modifica automaticamente la lista originale.

Esempio:

```java
List<Corso> corsiAttivi = corsi.stream()
        .filter(Corso::isAttivo)
        .collect(Collectors.toList());
```

La lista `corsi` resta invariata. La lista `corsiAttivi` contiene il risultato filtrato.

## 12. Stream usa e getta

Uno stream può essere consumato una sola volta.

Esempio errato:

```java
Stream<Corso> stream = corsi.stream();
long totale = stream.count();
long attivi = stream.filter(Corso::isAttivo).count();
```

Dopo la prima operazione terminale lo stream è chiuso.

Soluzione:

```java
long totale = corsi.stream().count();
long attivi = corsi.stream().filter(Corso::isAttivo).count();
```

## 13. Quando non usare stream

Un ciclo tradizionale può essere migliore quando:

- la logica richiede molti passaggi condizionali;
- bisogna modificare molti oggetti;
- servono `break` o `continue` in modo chiaro;
- la pipeline diventerebbe troppo lunga;
- il gruppo non riuscirebbe a leggerla facilmente.

## 14. Sintesi operativa

Una pipeline stream leggibile dovrebbe avere una forma simile:

```java
sorgente.stream()
        .filter(...)
        .map(...)
        .sorted(...)
        .collect(...);
```

Ogni passaggio deve essere comprensibile e giustificato.
