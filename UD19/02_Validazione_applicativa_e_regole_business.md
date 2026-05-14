# 02 - Validazione applicativa e regole di business

## Validare non significa solo controllare `null`

La validazione applicativa comprende più livelli.

| Livello | Domanda | Esempio |
|---|---|---|
| validazione sintattica | il dato ha una forma accettabile? | email contiene `@` |
| validazione semantica | il dato ha senso per il dominio? | durata corso maggiore di zero |
| regola di business | l'operazione è consentita ora? | iscrizione solo a corsi pubblicati |

Questi livelli non devono essere confusi.

## Esempio di validazione sintattica

```java
public static void requireText(String valore, String messaggio) {
    if (valore == null || valore.isBlank()) {
        throw new ValidazioneException(messaggio);
    }
}
```

Questo metodo controlla che una stringa esista e non sia vuota.

## Esempio di validazione semantica

```java
public static void requirePositive(int valore, String messaggio) {
    if (valore <= 0) {
        throw new ValidazioneException(messaggio);
    }
}
```

Questo metodo controlla che un numero abbia senso per il dominio.

## Esempio di regola di business

```java
if (!corso.isPubblicato()) {
    throw new RegolaBusinessException("Il corso non è pubblicato");
}
```

Il dato può essere formalmente valido, ma l'operazione non è ammessa.

## Dove mettere la validazione

### Nel costruttore?

È possibile validare nel costruttore quando si vuole impedire la creazione di oggetti non validi.

```java
public Corso(String codice, String titolo, int durataOre) {
    Validator.requireText(codice, "Il codice è obbligatorio");
    Validator.requireText(titolo, "Il titolo è obbligatorio");
    Validator.requirePositive(durataOre, "La durata deve essere positiva");
    this.codice = codice;
    this.titolo = titolo;
    this.durataOre = durataOre;
}
```

Vantaggio: l'oggetto nasce già coerente.

Limite: il costruttore può diventare molto carico se contiene troppe regole applicative.

### Nel servizio?

È consigliato mettere nel servizio le regole che riguardano una operazione.

```java
public void pubblicaCorso(String codice) {
    Corso corso = cercaCorso(codice);
    if (corso.isPubblicato()) {
        throw new RegolaBusinessException("Il corso è già pubblicato");
    }
    corso.pubblica();
}
```

La pubblicazione è un'operazione applicativa, quindi la regola sta bene nel servizio.

### Nella vista o nel `main`?

La vista o il `main` dovrebbero principalmente:

- raccogliere dati;
- chiamare servizi;
- mostrare messaggi;
- gestire l'errore a livello utente.

Non dovrebbero contenere tutta la logica di validazione.

## Gerarchia semplice di eccezioni applicative

Una scelta pulita consiste nel creare una base comune:

```java
public class ApplicazioneException extends RuntimeException {
    public ApplicazioneException(String message) {
        super(message);
    }
}
```

Poi specializzare:

```java
public class ValidazioneException extends ApplicazioneException {
    public ValidazioneException(String message) {
        super(message);
    }
}
```

```java
public class EntitaNonTrovataException extends ApplicazioneException {
    public EntitaNonTrovataException(String message) {
        super(message);
    }
}
```

```java
public class RegolaBusinessException extends ApplicazioneException {
    public RegolaBusinessException(String message) {
        super(message);
    }
}
```

## Perché una base comune è utile

Nel punto di interazione con l'utente si può catturare il tipo generale:

```java
try {
    service.iscrivi(codicePartecipante, codiceCorso);
} catch (ApplicazioneException ex) {
    System.out.println("Operazione non completata: " + ex.getMessage());
}
```

Il codice resta semplice, ma il dominio mantiene eccezioni specifiche.

## Messaggi di errore efficaci

Un messaggio efficace deve indicare il problema in modo comprensibile.

Esempi migliori:

```text
Il codice corso è obbligatorio
Il partecipante PAR-99 non esiste
Il corso JAVA-01 ha raggiunto il numero massimo di iscritti
```

Esempi peggiori:

```text
Errore
Input non valido
Exception occurred
```

## Collegamento con DAO e persistenza

Nelle UD successive le eccezioni diventeranno ancora più importanti.

Esempi:

- impossibile leggere un file;
- CSV con colonne mancanti;
- record duplicato;
- connessione al database non disponibile;
- query senza risultati;
- violazione di vincoli nel database.

La UD19 prepara un linguaggio comune per trattare questi casi senza mescolare tutto dentro il `main`.
