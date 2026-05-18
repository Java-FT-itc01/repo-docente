# Singleton: varianti, limiti e thread-safety

## Obiettivo 

Questo documento introduce il pattern **Singleton** e mostra alcune sue varianti.

L'obiettivo non è studiare la concorrenza in modo completo. L'obiettivo è comprendere:

- quale problema risolve il Singleton;
- quali sono le sue varianti principali;
- quali limiti presenta;
- perché nella UD21 useremo una versione semplice;
- perché la versione thread-safe sarà collegata alla UD22.

## Problema di partenza

Il Singleton viene usato quando si vuole controllare la creazione di una classe in modo che esista una sola istanza condivisa.

Casi d'uso:

- archivio unico in memoria;
- configurazione applicativa;
- componente centrale di accesso a una risorsa;
- oggetto condiviso da più parti del programma.

Nel laboratorio UD21 lo usiamo per un DAO in memoria.
Supponiamo di avere un repository in memoria:

```java
public class CorsoDaoInMemoria {
    private List<Corso> corsi = new ArrayList<>();
}
```

Se il programma crea più istanze della classe, ogni istanza avrà la propria lista:

```java
CorsoDaoInMemoria dao1 = new CorsoDaoInMemoria();
CorsoDaoInMemoria dao2 = new CorsoDaoInMemoria();
```

Il risultato è che:

- `dao1` salva in una lista;
- `dao2` legge da un'altra lista;
- i dati sembrano non essere condivisi;
- il comportamento dell'applicazione diventa incoerente.

Il Singleton impedisce la creazione libera di più istanze e fornisce un punto unico di accesso all'oggetto condiviso.

## 1. Singleton lazy semplice

Questa è una delle forme più semplici del Singleton.

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {
    }

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```
## Spiegazione

| Elemento | Significato |
|---|---|
| `private static Singleton instance` | conserva l'unica istanza |
| `private Singleton()` | impedisce l'uso di `new` dall'esterno |
| `getInstance()` | restituisce l'istanza condivisa |

Questa versione si chiama spesso **lazy**, perché l'oggetto viene creato solo quando viene richiesto per la prima volta.

## Limite

Non è thread-safe.

In un'applicazione con più thread, due thread potrebbero entrare contemporaneamente in `getInstance()` e trovare entrambi `instance == null`.

Nel laboratorio UD21 questo problema non emerge perché l'applicazione è monothread.

---


### Caratteristiche

| Aspetto | Descrizione |
|---|---|
| Creazione | l'istanza viene creata solo alla prima richiesta |
| Costruttore | privato |
| Accesso | tramite metodo statico `getInstance()` |
| Thread-safety | non garantita |

### Quando è accettabile

Questa forma è accettabile in un programma semplice monothread, come il laboratorio console della UD21.

Non è sicura in un'applicazione concorrente.



## 2. Singleton eager

In questa variante l'istanza viene creata subito, quando la classe viene caricata.

```java
public class EagerSingleton {
    private static final EagerSingleton INSTANCE = new EagerSingleton();

    private EagerSingleton() {
    }

    public static EagerSingleton getInstance() {
        return INSTANCE;
    }
}
```


---

### Caratteristiche

| Aspetto | Descrizione |
|---|---|
| Creazione | immediata |
| Codice | semplice |
| Thread-safety | garantita dal caricamento della classe |
| Limite | l'istanza viene creata anche se non viene mai usata |

Questa versione è semplice e sicura, ma non è lazy. Per oggetti leggeri questo non è un problema. Per oggetti costosi o dipendenti da risorse esterne può essere una scelta meno adatta.

## 3. Singleton con metodo synchronized

Una prima soluzione per rendere thread-safe il Singleton lazy è sincronizzare il metodo `getInstance()`.

```java
public class SynchronizedSingleton {
    private static SynchronizedSingleton instance;

    private SynchronizedSingleton() {
    }

    public static synchronized SynchronizedSingleton getInstance() {
        if (instance == null) {
            instance = new SynchronizedSingleton();
        }
        return instance;
    }
}
```

### Caratteristiche

| Aspetto | Descrizione |
|---|---|
| Creazione | lazy |
| Thread-safety | garantita |
| Limite | ogni chiamata a `getInstance()` entra in una sezione sincronizzata |

È una soluzione corretta, ma può avere un costo inutile se `getInstance()` viene chiamato molte volte.

## 4. Singleton con double-checked locking e volatile

Questa variante riduce il costo della sincronizzazione dopo la creazione dell'istanza.

```java
public class ThreadSafeSingleton {
    private static volatile ThreadSafeSingleton instance;

    private ThreadSafeSingleton() {
    }

    public static ThreadSafeSingleton getInstance() {
        if (instance == null) {
            synchronized (ThreadSafeSingleton.class) {
                if (instance == null) {
                    instance = new ThreadSafeSingleton();
                }
            }
        }
        return instance;
    }
}
```

### Perché ci sono due controlli `if`

Il primo controllo evita di entrare nel blocco `synchronized` quando l'istanza esiste già.

Il secondo controllo serve perché due thread potrebbero arrivare quasi contemporaneamente al primo `if`.

Il modificatore `volatile` garantisce una corretta visibilità della variabile `instance` tra thread.

### Caratteristiche

| Aspetto | Descrizione |
|---|---|
| Creazione | lazy |
| Thread-safety | garantita se implementata correttamente |
| Complessità | maggiore |
| Uso didattico | utile per collegarsi alla UD22 |

In UD21 questa versione viene mostrata per completezza, ma non viene usata nel laboratorio guidato.

## 5. Singleton con enum

In Java esiste anche una forma molto compatta basata su `enum`.

```java
public enum EnumSingleton {
    INSTANCE;

    public void eseguiOperazione() {
        System.out.println("Operazione eseguita");
    }
}
```

Uso:

```java
EnumSingleton.INSTANCE.eseguiOperazione();
```

### Caratteristiche

| Aspetto | Descrizione |
|---|---|
| Semplicità | molto alta |
| Thread-safety | garantita |
| Serializzazione | gestita in modo robusto |
| Limite didattico | meno esplicita per comprendere costruttore privato e `getInstance()` |

Nel corso useremo la forma classica perché rende più visibile la struttura del pattern.

## Confronto tra varianti

| Variante | Lazy | Thread-safe | Complessità | Uso nella UD21 |
|---|---:|---:|---:|---|
| Lazy semplice | sì | no | bassa | usata nel laboratorio |
| Eager | no | sì | bassa | citata |
| `synchronized` | sì | sì | media | citata |
| Double-checked locking | sì | sì | alta | citata e collegata alla UD22 |
| `enum` | no | sì | bassa | citata |

## Perché nella UD21 useremo il Singleton semplice

Il laboratorio della UD21 è una console application monothread.

L'obiettivo è comprendere:

- costruttore privato;
- campo statico;
- metodo `getInstance()`;
- istanza condivisa;
- relazione tra DAO e stato in memoria.

Per questo useremo la versione semplice.

Esempio applicato al DAO:

```java
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
}
```

## Limiti del Singleton

Il Singleton deve essere usato con attenzione.

Possibili problemi:

- introduce stato globale;
- rende più difficile sostituire l'istanza nei test;
- può nascondere dipendenze reali;
- può diventare un punto di accoppiamento forte;
- richiede attenzione in scenari concorrenti.

Per questo nel laboratorio verrà usato solo nel DAO in memoria, non in tutte le classi dell'applicazione.

## Collegamento con Dependency Injection

Nel laboratorio non faremo usare il Singleton direttamente da tutte le classi.

Questa forma è da evitare:

```java
public class CatalogoService {
    public void aggiungiCorso(Corso corso) {
        CorsoDaoInMemoria.getInstance().salva(corso);
    }
}
```

Questa forma è preferibile:

```java
public class CatalogoService {
    private final CorsoDao corsoDao;

    public CatalogoService(CorsoDao corsoDao) {
        this.corsoDao = corsoDao;
    }
}
```

Il Singleton viene recuperato nel punto di avvio:

```java
CorsoDao corsoDao = CorsoDaoInMemoria.getInstance();
CatalogoService service = new CatalogoService(corsoDao);
```

In questo modo la dipendenza resta visibile.

## Collegamento con UD22

La UD22 approfondirà:

- thread;
- `Runnable`;
- stato condiviso;
- race condition;
- `synchronized`;
- sezioni critiche.

La versione thread-safe del Singleton verrà ripresa come esempio di oggetto condiviso protetto in scenari concorrenti.
