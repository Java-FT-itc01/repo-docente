# UD22.v2 - Dalla thread safety del Singleton alla sincronizzazione

## Obiettivo del file

Nella UD21 è stato introdotto il pattern Singleton.

È stata mostrata anche una versione più protetta con `synchronized` e `volatile`, senza approfondire i dettagli della concorrenza.

In questa UD riprendiamo quel punto e lo colleghiamo al tema principale: **lo stato condiviso tra thread**.

---

# 1. Il problema del Singleton semplice

La versione semplice del Singleton è questa:

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

In un programma monothread questa soluzione funziona correttamente.

In un programma multithread, invece, più thread potrebbero eseguire contemporaneamente il metodo `getInstance()`.

---

# 2. Scenario problematico

Immaginiamo due thread:

```text
Thread A entra in getInstance()
Thread B entra in getInstance()
```

Entrambi potrebbero controllare la condizione:

```java
if (instance == null) {
```

ed entrambi potrebbero trovare `instance == null`.

Risultato possibile:

```text
Thread A crea una nuova istanza.
Thread B crea una nuova istanza.
```

Il Singleton non garantirebbe più una sola istanza.

---

# 3. Race condition

Una **race condition** si verifica quando il risultato del programma dipende dall'ordine con cui più thread eseguono operazioni sullo stesso dato condiviso.

Nel caso del Singleton semplice, il dato condiviso è:

```java
private static Singleton instance;
```

Più thread possono leggere e modificare quella variabile.

---

# 4. Sezione critica

Una **sezione critica** è una parte di codice che accede a uno stato condiviso e che non dovrebbe essere eseguita contemporaneamente da più thread.

Nel Singleton, la sezione critica è la creazione dell'istanza:

```java
if (instance == null) {
    instance = new Singleton();
}
```

---

# 5. Uso di synchronized

`synchronized` permette di proteggere una sezione critica.

Esempio:

```java
public static synchronized Singleton getInstance() {
    if (instance == null) {
        instance = new Singleton();
    }
    return instance;
}
```

In questa versione, un solo thread alla volta può eseguire il metodo `getInstance()`.

---

# 6. Double-checked locking

La versione più avanzata riduce la sincronizzazione quando l'istanza esiste già:

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

Questa versione contiene tre elementi:

| Elemento | Scopo |
|---|---|
| primo `if` | evita sincronizzazione quando l'istanza esiste già |
| `synchronized` | protegge la creazione dell'istanza |
| secondo `if` | controlla se un altro thread ha già creato l'istanza |
| `volatile` | garantisce visibilità corretta della variabile tra thread |

---

# 7. Livello di approfondimento richiesto

In questa UD non è necessario studiare il Java Memory Model in modo completo.

È sufficiente comprendere che:

- più thread possono accedere allo stesso stato;
- l'accesso concorrente può produrre risultati errati;
- `synchronized` protegge una sezione critica;
- `volatile` riguarda la visibilità di una variabile tra thread;
- il codice concorrente richiede più attenzione del codice monothread.

---

# 8. Collegamento con i laboratori UD22

Nei laboratori non lavoreremo solo sul Singleton.

Useremo esempi più semplici per osservare concretamente:

- thread multipli;
- stato condiviso;
- accessi concorrenti;
- sincronizzazione;
- risultati corretti e risultati errati.

Il Singleton è il punto di collegamento con la UD21.
La UD22 generalizza il problema a tutti gli oggetti condivisi.
