# UD18 - Uso corretto di `Optional` in Java

## 1. Obiettivo della sezione

In questa unità introduciamo `Optional`, una classe di Java pensata per rappresentare in modo esplicito un valore che **potrebbe esserci oppure no**.

`Optional` non serve a rendere il codice più complicato.  
Serve a rendere più chiaro un caso molto frequente:

> un metodo cerca o calcola qualcosa, ma potrebbe non trovare nessun risultato.

---

## 2. Il problema: il valore `null`

In Java una variabile di tipo riferimento può contenere un oggetto oppure `null`.

Esempio:

```java
Corso corso = null;
```

Il problema nasce quando si prova a usare un oggetto che in realtà non esiste:

```java
System.out.println(corso.getTitolo());
```

Se `corso` vale `null`, il programma genera una `NullPointerException`.

Questo errore significa:

> sto provando a usare un oggetto, ma il riferimento non punta a nessun oggetto reale.

---

## 3. Esempio senza `Optional`

Immaginiamo di avere una classe `Corso`.

```java
public class Corso {
    private String codice;
    private String titolo;

    public Corso(String codice, String titolo) {
        this.codice = codice;
        this.titolo = titolo;
    }

    public String getCodice() {
        return codice;
    }

    public String getTitolo() {
        return titolo;
    }
}
```

Ora immaginiamo un metodo che cerca un corso per codice.

```java
public Corso cercaPerCodice(List<Corso> corsi, String codice) {
    for (Corso corso : corsi) {
        if (corso.getCodice().equals(codice)) {
            return corso;
        }
    }

    return null;
}
```

Il metodo restituisce:

- un oggetto `Corso`, se il corso viene trovato;
- `null`, se il corso non viene trovato.

Il codice che usa questo metodo deve ricordarsi di controllare sempre il `null`.

```java
Corso corso = cercaPerCodice(corsi, "JAVA-01");

if (corso != null) {
    System.out.println(corso.getTitolo());
} else {
    System.out.println("Corso non trovato");
}
```

Il codice funziona, ma il problema è che il significato del metodo non è immediatamente visibile.

Chi legge la firma del metodo vede solo:

```java
public Corso cercaPerCodice(List<Corso> corsi, String codice)
```

Non è evidente che il metodo possa restituire `null`.

---

## 4. Che cos'è `Optional`

`Optional<T>` è un contenitore che può avere due stati:

1. contiene un valore di tipo `T`;
2. non contiene nessun valore.

Esempio:

```java
Optional<Corso> risultato;
```

Questa variabile non rappresenta direttamente un `Corso`.

Rappresenta il risultato di una ricerca che può essere:

- un corso trovato;
- nessun corso trovato.

Il vantaggio è che il possibile valore assente diventa esplicito nel tipo restituito dal metodo.

---

## 5. Esempio corretto con `Optional`

Il metodo precedente può essere riscritto così:

```java
public Optional<Corso> cercaPerCodice(List<Corso> corsi, String codice) {
    for (Corso corso : corsi) {
        if (corso.getCodice().equals(codice)) {
            return Optional.of(corso);
        }
    }

    return Optional.empty();
}
```

Ora la firma del metodo comunica chiaramente il comportamento:

```java
public Optional<Corso> cercaPerCodice(List<Corso> corsi, String codice)
```

Significa:

> il metodo prova a cercare un `Corso`, ma il risultato potrebbe essere assente.

---

## 6. I tre modi principali per creare un `Optional`

### 6.1 `Optional.of(...)`

Si usa quando si è sicuri che il valore non sia `null`.

```java
Corso corso = new Corso("JAVA-01", "Java Base");

Optional<Corso> risultato = Optional.of(corso);
```

Attenzione:

```java
Corso corso = null;

Optional<Corso> risultato = Optional.of(corso);
```

Questo codice genera una `NullPointerException`, perché `Optional.of(...)` non accetta valori `null`.

---

### 6.2 `Optional.ofNullable(...)`

Si usa quando il valore potrebbe essere `null`.

```java
Corso corso = null;

Optional<Corso> risultato = Optional.ofNullable(corso);
```

Se `corso` contiene un oggetto, l'`Optional` conterrà quell'oggetto.

Se `corso` vale `null`, l'`Optional` sarà vuoto.

---

### 6.3 `Optional.empty()`

Si usa per rappresentare esplicitamente l'assenza di un valore.

```java
Optional<Corso> risultato = Optional.empty();
```

Nel metodo di ricerca viene usato quando nessun elemento corrisponde al criterio richiesto.

```java
return Optional.empty();
```

---

## 7. Come usare un `Optional`

### 7.1 Controllare se il valore è presente

```java
Optional<Corso> risultato = cercaPerCodice(corsi, "JAVA-01");

if (risultato.isPresent()) {
    Corso corso = risultato.get();
    System.out.println(corso.getTitolo());
} else {
    System.out.println("Corso non trovato");
}
```

Questo codice è corretto, ma non è sempre lo stile migliore.

Il metodo `get()` va usato con attenzione, perché se l'`Optional` è vuoto genera un'eccezione.

Esempio da evitare:

```java
Optional<Corso> risultato = cercaPerCodice(corsi, "JAVA-99");

Corso corso = risultato.get();
```

Se il corso non viene trovato, il programma genera una `NoSuchElementException`.

---

### 7.2 Usare `ifPresent(...)`

Quando si vuole eseguire un'azione solo se il valore è presente, si può usare `ifPresent`.

```java
Optional<Corso> risultato = cercaPerCodice(corsi, "JAVA-01");

risultato.ifPresent(corso -> {
    System.out.println(corso.getTitolo());
});
```

Il codice dentro la lambda viene eseguito solo se l'`Optional` contiene un valore.

Se l'`Optional` è vuoto, non viene eseguito nulla.

---

### 7.3 Usare `ifPresentOrElse(...)`

Quando si vuole gestire sia il caso in cui il valore esiste sia il caso in cui non esiste, si può usare `ifPresentOrElse`.

```java
Optional<Corso> risultato = cercaPerCodice(corsi, "JAVA-01");

risultato.ifPresentOrElse(
    corso -> System.out.println("Corso trovato: " + corso.getTitolo()),
    () -> System.out.println("Corso non trovato")
);
```

Il primo blocco viene eseguito se il valore è presente.  
Il secondo blocco viene eseguito se il valore è assente.

---

## 8. Fornire un valore alternativo

### 8.1 `orElse(...)`

`orElse(...)` restituisce il valore contenuto nell'`Optional`, se presente.  
Se l'`Optional` è vuoto, restituisce un valore alternativo.

```java
Optional<Corso> risultato = cercaPerCodice(corsi, "JAVA-99");

Corso corso = risultato.orElse(new Corso("DEFAULT", "Corso non disponibile"));

System.out.println(corso.getTitolo());
```

In questo esempio:

- se il corso viene trovato, viene usato il corso trovato;
- se il corso non viene trovato, viene usato il corso alternativo.

---

### 8.2 `orElseGet(...)`

`orElseGet(...)` è simile a `orElse(...)`, ma il valore alternativo viene creato solo se serve.

```java
Corso corso = risultato.orElseGet(() -> new Corso("DEFAULT", "Corso generico"));
```

Differenza importante:

```java
Corso corso = risultato.orElse(creaCorsoDefault());
```

In questo caso `creaCorsoDefault()` viene chiamato comunque, anche se l'`Optional` contiene già un valore.

Con `orElseGet(...)` invece il metodo viene chiamato solo se l'`Optional` è vuoto.

```java
Corso corso = risultato.orElseGet(() -> creaCorsoDefault());
```

---

### 8.3 `orElseThrow(...)`

Si usa quando l'assenza del valore è un errore da segnalare.

```java
Corso corso = cercaPerCodice(corsi, "JAVA-99")
        .orElseThrow(() -> new IllegalArgumentException("Corso non trovato"));
```

Questo approccio è utile quando il programma non può proseguire senza quel valore.

---

## 9. Trasformare un valore con `map(...)`

`map(...)` permette di trasformare il valore contenuto nell'`Optional`, se esiste.

Esempio: vogliamo ottenere il titolo del corso trovato.

```java
Optional<Corso> risultato = cercaPerCodice(corsi, "JAVA-01");

Optional<String> titolo = risultato.map(corso -> corso.getTitolo());
```

Se il corso è presente, viene estratto il titolo.  
Se il corso non è presente, il risultato rimane un `Optional` vuoto.

Si può anche scrivere con method reference:

```java
Optional<String> titolo = risultato.map(Corso::getTitolo);
```

Esempio con valore alternativo:

```java
String titolo = cercaPerCodice(corsi, "JAVA-99")
        .map(Corso::getTitolo)
        .orElse("Titolo non disponibile");

System.out.println(titolo);
```

---

## 10. Filtrare un valore con `filter(...)`

`filter(...)` mantiene il valore solo se rispetta una condizione.

Esempio: vogliamo usare il corso solo se il titolo contiene la parola `"Java"`.

```java
Optional<Corso> risultato = cercaPerCodice(corsi, "JAVA-01");

Optional<Corso> corsoJava = risultato.filter(corso -> corso.getTitolo().contains("Java"));
```

Se il corso esiste e il titolo contiene `"Java"`, l'`Optional` resta pieno.

Se il corso non esiste oppure non rispetta la condizione, l'`Optional` diventa vuoto.

Esempio completo:

```java
String messaggio = cercaPerCodice(corsi, "JAVA-01")
        .filter(corso -> corso.getTitolo().contains("Java"))
        .map(Corso::getTitolo)
        .orElse("Nessun corso Java trovato");

System.out.println(messaggio);
```

---

## 11. `flatMap(...)` in modo semplice

`flatMap(...)` serve quando una trasformazione restituisce già un `Optional`.

Esempio:

```java
public Optional<String> cercaEmailDocente(Corso corso) {
    if (corso.getCodice().startsWith("JAVA")) {
        return Optional.of("docente.java@example.com");
    }

    return Optional.empty();
}
```

Ora partiamo da una ricerca che restituisce `Optional<Corso>`.

```java
Optional<Corso> risultato = cercaPerCodice(corsi, "JAVA-01");
```

Se usassimo `map(...)`, otterremmo un Optional dentro un altro Optional:

```java
Optional<Optional<String>> email = risultato.map(corso -> cercaEmailDocente(corso));
```

Questa forma è scomoda.

Con `flatMap(...)` otteniamo direttamente:

```java
Optional<String> email = risultato.flatMap(corso -> cercaEmailDocente(corso));
```

Quindi:

- `map(...)` si usa quando la trasformazione restituisce un valore normale;
- `flatMap(...)` si usa quando la trasformazione restituisce già un `Optional`.

---

## 12. Esempio completo

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Optional;

public class Main {
    public static void main(String[] args) {
        List<Corso> corsi = new ArrayList<>();

        corsi.add(new Corso("JAVA-01", "Java Base"));
        corsi.add(new Corso("JAVA-02", "Java Object Oriented"));
        corsi.add(new Corso("WEB-01", "HTML CSS JavaScript"));

        Optional<Corso> risultato = cercaPerCodice(corsi, "JAVA-02");

        risultato.ifPresentOrElse(
                corso -> System.out.println("Corso trovato: " + corso.getTitolo()),
                () -> System.out.println("Corso non trovato")
        );

        String titolo = cercaPerCodice(corsi, "JAVA-99")
                .map(Corso::getTitolo)
                .orElse("Titolo non disponibile");

        System.out.println(titolo);
    }

    public static Optional<Corso> cercaPerCodice(List<Corso> corsi, String codice) {
        for (Corso corso : corsi) {
            if (corso.getCodice().equals(codice)) {
                return Optional.of(corso);
            }
        }

        return Optional.empty();
    }
}

class Corso {
    private String codice;
    private String titolo;

    public Corso(String codice, String titolo) {
        this.codice = codice;
        this.titolo = titolo;
    }

    public String getCodice() {
        return codice;
    }

    public String getTitolo() {
        return titolo;
    }
}
```

---

## 13. Dove usare `Optional`

`Optional` è particolarmente adatto come tipo di ritorno di un metodo.

Esempi corretti:

```java
public Optional<Corso> cercaPerCodice(String codice)
```

```java
public Optional<Studente> cercaStudentePerEmail(String email)
```

```java
public Optional<Iscrizione> cercaIscrizione(String codiceStudente, String codiceCorso)
```

In questi casi il metodo sta comunicando:

> provo a cercare un elemento, ma potrebbe non esistere.

---

## 14. Dove evitare `Optional`

### 14.1 Evitare `Optional` come attributo di una classe

Di norma è meglio evitare questo stile:

```java
public class Corso {
    private Optional<String> descrizione;
}
```

Meglio usare un attributo normale:

```java
public class Corso {
    private String descrizione;
}
```

L'`Optional` nasce principalmente per rappresentare il risultato di un metodo, non per modellare direttamente lo stato interno degli oggetti.

---

### 14.2 Evitare `Optional` come parametro

Di norma è meglio evitare:

```java
public void stampaCorso(Optional<Corso> corso)
```

Meglio ricevere direttamente il valore:

```java
public void stampaCorso(Corso corso)
```

Oppure gestire l'assenza prima di chiamare il metodo.

```java
Optional<Corso> risultato = cercaPerCodice(corsi, "JAVA-01");

risultato.ifPresent(corso -> stampaCorso(corso));
```

---

### 14.3 Evitare `Optional` per le collezioni

Meglio evitare:

```java
public Optional<List<Corso>> trovaTutti()
```

Una lista può già essere vuota.

Meglio:

```java
public List<Corso> trovaTutti()
```

Se non ci sono corsi, il metodo restituisce una lista vuota.

```java
return new ArrayList<>();
```

Questo è più semplice da usare.

---

## 15. Regole pratiche

### Regola 1

Usare `Optional` quando un metodo può non trovare un singolo valore.

```java
public Optional<Corso> cercaPerCodice(String codice)
```

---

### Regola 2

Non usare `Optional` solo per evitare di capire il problema del `null`.

`Optional` non elimina automaticamente tutti i problemi.  
Aiuta a rendere esplicito che un valore può mancare.

---

### Regola 3

Evitare `get()` senza prima controllare la presenza del valore.

Da evitare:

```java
Corso corso = risultato.get();
```

Meglio:

```java
risultato.ifPresent(corso -> System.out.println(corso.getTitolo()));
```

Oppure:

```java
Corso corso = risultato.orElseThrow(() -> new IllegalArgumentException("Corso non trovato"));
```

---

### Regola 4

Usare `orElse(...)` o `orElseGet(...)` quando ha senso avere un valore alternativo.

```java
String titolo = cercaPerCodice(corsi, "JAVA-99")
        .map(Corso::getTitolo)
        .orElse("Titolo non disponibile");
```

---

### Regola 5

Per liste, array o collezioni, restituire una collezione vuota invece di un `Optional`.

Meglio:

```java
public List<Corso> cercaPerCategoria(String categoria)
```

e non:

```java
public Optional<List<Corso>> cercaPerCategoria(String categoria)
```

---

## 16. Confronto sintetico

| Situazione | Soluzione consigliata |
|---|---|
| Cercare un singolo corso per codice | `Optional<Corso>` |
| Cercare tutti i corsi disponibili | `List<Corso>` anche vuota |
| Valore obbligatorio nel costruttore | parametro normale |
| Valore assente da gestire nel chiamante | `Optional` come ritorno |
| Attributo interno di una classe | attributo normale |
| Metodo che non può proseguire senza valore | `orElseThrow(...)` |

---

## 17. Mini esercizio guidato

Dato il seguente elenco di corsi:

```java
List<Corso> corsi = new ArrayList<>();

corsi.add(new Corso("JAVA-01", "Java Base"));
corsi.add(new Corso("JAVA-02", "Java Object Oriented"));
corsi.add(new Corso("DB-01", "SQL Base"));
```

Scrivere un metodo:

```java
public static Optional<Corso> cercaPerTitolo(List<Corso> corsi, String titolo)
```

Il metodo deve:

1. scorrere la lista dei corsi;
2. confrontare il titolo di ogni corso con il titolo ricevuto come parametro;
3. restituire `Optional.of(corso)` se trova il corso;
4. restituire `Optional.empty()` se non trova nulla.

Soluzione possibile:

```java
public static Optional<Corso> cercaPerTitolo(List<Corso> corsi, String titolo) {
    for (Corso corso : corsi) {
        if (corso.getTitolo().equals(titolo)) {
            return Optional.of(corso);
        }
    }

    return Optional.empty();
}
```

Uso del metodo:

```java
Optional<Corso> risultato = cercaPerTitolo(corsi, "Java Base");

risultato.ifPresentOrElse(
        corso -> System.out.println("Trovato: " + corso.getCodice()),
        () -> System.out.println("Nessun corso trovato")
);
```

---

## 18. Mini esercizio autonomo

Creare un programma con:

1. una classe `Studente` con:
   - `matricola`;
   - `nome`;
   - `email`.

2. una lista di studenti.

3. un metodo:

```java
public static Optional<Studente> cercaPerMatricola(List<Studente> studenti, String matricola)
```

4. un uso del metodo con `ifPresentOrElse(...)`.

5. un uso del metodo con `map(...)` per ottenere solo l'email dello studente.

Esempio atteso:

```java
String email = cercaPerMatricola(studenti, "S001")
        .map(Studente::getEmail)
        .orElse("Email non disponibile");

System.out.println(email);
```

---

## 19. Sintesi finale

`Optional` serve a rendere esplicito che un valore può mancare.

È utile soprattutto nei metodi di ricerca, come:

```java
public Optional<Corso> cercaPerCodice(String codice)
```

Il chiamante del metodo è così obbligato a gestire il caso in cui il valore non esiste.

Questo rende il codice più chiaro, più sicuro e più leggibile.
