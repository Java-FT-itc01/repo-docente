# UD22.v2 - Glossario minimo di sincronizzazione

## Obiettivo del file

Questo glossario raccoglie i termini minimi necessari per affrontare la UD22.

Non è una trattazione avanzata della concorrenza Java.

Serve a dare un significato operativo ai termini che verranno usati nei laboratori.

---

# Thread

Un thread è un flusso di esecuzione all'interno di un programma.

Un programma può avere un solo thread principale oppure più thread che lavorano contemporaneamente.

---

# Runnable

`Runnable` è un'interfaccia funzionale che rappresenta un'azione eseguibile da un thread.

Contiene il metodo:

```java
void run();
```

Esempio:

```java
Runnable azione = () -> System.out.println("Esecuzione thread");
Thread thread = new Thread(azione);
thread.start();
```

---

# Stato condiviso

Lo stato condiviso è un dato accessibile da più thread.

Esempi:

- una lista condivisa;
- un contatore condiviso;
- un archivio in memoria;
- una variabile statica;
- un Singleton con dati modificabili.

---

# Race condition

Una race condition si verifica quando il risultato dipende dall'ordine con cui i thread accedono a uno stato condiviso.

Esempio semplice:

```text
Due thread leggono lo stesso valore.
Entrambi lo modificano.
Uno dei due aggiornamenti viene perso.
```

---

# Sezione critica

Una sezione critica è una parte di codice che accede a uno stato condiviso e deve essere protetta.

Esempio:

```java
postiDisponibili--;
```

Se più thread modificano `postiDisponibili` contemporaneamente, il risultato può diventare errato.

---

# synchronized

`synchronized` serve a proteggere una sezione critica.

Permette a un solo thread alla volta di eseguire un metodo o un blocco di codice protetto.

Esempio:

```java
public synchronized void prenota() {
    postiDisponibili--;
}
```

---

# volatile

`volatile` riguarda la visibilità del valore di una variabile tra thread diversi.

Se una variabile è `volatile`, gli aggiornamenti fatti da un thread sono resi visibili agli altri thread in modo più controllato.

In questa UD è sufficiente ricordare:

```text
synchronized protegge l'accesso.
volatile riguarda la visibilità.
```

---

# Thread-safe

Un codice è thread-safe quando continua a comportarsi correttamente anche se usato da più thread contemporaneamente.

Un oggetto thread-safe protegge correttamente il proprio stato condiviso.

---

# Regola pratica

Quando più thread condividono dati modificabili, bisogna chiedersi:

```text
Chi può leggere il dato?
Chi può modificarlo?
Cosa succede se due thread lo fanno nello stesso momento?
Serve una protezione?
```
