# Applicazione Singleton nel CRUD console

## Obiettivo

Vogliamo collegare gli aspetti concettuali del Singleton al laboratorio guidato sul CRUD console.

Serve a chiarire perché nel laboratorio il DAO in memoria viene implementato come Singleton semplice.

---

# 1. Problema del laboratorio

Nel CRUD console abbiamo bisogno di un archivio condiviso in memoria.

Più parti dell'applicazione devono lavorare sugli stessi dati:

- controller;
- service;
- DAO;
- programma principale.

Se ogni classe creasse un proprio DAO con `new`, potremmo avere più archivi separati.

Esempio problematico:

```java
CorsoDao dao1 = new CorsoDaoInMemory();
CorsoDao dao2 = new CorsoDaoInMemory();
```

In questo caso `dao1` e `dao2` potrebbero contenere dati diversi.

---

# 2. Soluzione didattica che varrà adottata

Nel laboratorio usiamo un DAO in memoria come Singleton:

```java
public class CorsoDaoInMemory implements CorsoDao {
    private static CorsoDaoInMemory instance;

    private CorsoDaoInMemory() {
    }

    public static CorsoDaoInMemory getInstance() {
        if (instance == null) {
            instance = new CorsoDaoInMemory();
        }
        return instance;
    }
}
```

Questa soluzione permette di avere una sola istanza condivisa del DAO.

---

# 3. Perché non usiamo la versione thread-safe avanzata

Il laboratorio UD21 è un'applicazione console monothread.

Non ci sono più thread che accedono contemporaneamente al DAO.

Per questo motivo è sufficiente una versione semplice del Singleton.

Le versioni con `synchronized`, `volatile` o double-checked locking sono state mostrate per completezza, ma non sono richieste nel laboratorio UD21.

---

# 4. Cosa ci interessa veramente

Per i nostri scopi è necessario capire che:

- il costruttore privato impedisce la creazione libera dell'oggetto;
- il campo statico conserva l'istanza condivisa;
- il metodo `getInstance()` permette di accedere all'istanza;
- il DAO Singleton rappresenta un archivio unico in memoria;
- questa soluzione è didattica e non deve essere applicata automaticamente ovunque.

---

# 5. Collegamento con il Service Layer

Il service dovrebbe dipendere dall'interfaccia DAO, non direttamente dalla classe concreta.

Esempio:

```java
public class CorsoService {
    private CorsoDao corsoDao;

    public CorsoService(CorsoDao corsoDao) {
        this.corsoDao = corsoDao;
    }
}
```

Nel `main` possiamo collegare le parti:

```java
CorsoDao corsoDao = CorsoDaoInMemory.getInstance();
CorsoService service = new CorsoService(corsoDao);
```

In questo modo:

- il DAO concreto è scelto nel punto di configurazione;
- il service lavora sull'interfaccia;
- in futuro sarà possibile sostituire il DAO in memoria con un DAO JDBC.

---

# 6. Collegamento con le UD successive

| UD | Evoluzione |
|---|---|
| UD21 | DAO in memoria con Singleton semplice |
| UD22 | problemi di stato condiviso e sincronizzazione |
| UD27 | DAO con JDBC |
| UD30 | repository JPA |
| UD31 | repository Spring Data JPA e bean Spring |

Il Singleton in UD21 non è un punto di arrivo. È un passaggio per comprendere meglio la gestione delle responsabilità e dello stato condiviso.
