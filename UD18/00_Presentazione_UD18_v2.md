# UD18.v2 - Lambda e Stream base

## Obiettivo della giornata

La giornata introduce lambda expression e Stream API come strumenti per lavorare in modo più espressivo sulle collezioni Java.

Il punto non è scrivere codice più corto a tutti i costi. Il punto è imparare a rappresentare operazioni come filtro, trasformazione, ordinamento e produzione di report in modo più dichiarativo, leggibile e vicino all'intenzione del programma.

## Collegamento con le UD precedenti

Nelle UD precedenti i partecipanti hanno lavorato con:

- classi e oggetti;
- incapsulamento;
- ereditarietà e polimorfismo;
- interfacce;
- cast e `instanceof`;
- Collections e Generics.

La UD18.v2 usa queste competenze per lavorare su collezioni di oggetti di dominio.

## Risultati attesi

Al termine della giornata il partecipante deve essere in grado di progettare e implementare piccoli servizi di analisi su collezioni Java usando lambda e stream in modo corretto.

In particolare deve essere in grado di:

### 1. Comprendere il ruolo di una lambda

Il partecipante deve saper spiegare che una lambda rappresenta un comportamento passato come valore, quando il contesto richiede una interfaccia funzionale.

Esempio:

```java
corsi.sort((c1, c2) -> c1.getTitolo().compareTo(c2.getTitolo()));
```

Deve saper distinguere:

- parametri della lambda;
- corpo della lambda;
- valore restituito;
- contesto funzionale in cui viene usata.

### 2. Usare stream per interrogare collezioni di oggetti

Dato un elenco di oggetti, il partecipante deve saper produrre operazioni come:

- filtrare elementi;
- estrarre proprietà;
- ordinare risultati;
- contare elementi;
- verificare condizioni;
- ottenere il primo elemento coerente con una regola;
- calcolare un valore aggregato semplice.

### 3. Separare dati, regole e report

Il partecipante deve saper evitare che tutto il codice finisca nel `main`.

La struttura minima attesa è:

- classi di dominio;
- servizio di report o analisi;
- classe di avvio;
- eventuale documentazione delle scelte.

### 4. Effettuare refactoring da ciclo tradizionale a stream

Dato codice simile a questo:

```java
for (Corso corso : corsi) {
    if (corso.isAttivo() && corso.getDurataOre() >= 24) {
        System.out.println(corso.getTitolo());
    }
}
```

il partecipante deve saperlo trasformare in una pipeline leggibile:

```java
corsi.stream()
     .filter(Corso::isAttivo)
     .filter(corso -> corso.getDurataOre() >= 24)
     .map(Corso::getTitolo)
     .forEach(System.out::println);
```

### 5. Riconoscere i limiti dell'approccio stream

Il partecipante deve anche sapere quando non usare stream.

Esempi:

- logica troppo lunga;
- molte modifiche di stato;
- debug più chiaro con ciclo tradizionale;
- pipeline troppo annidata;
- codice meno comprensibile per il gruppo di lavoro.

## Strumenti necessari

| Strumento | Uso nella UD |
|---|---|
| JDK | compilazione ed esecuzione codice Java |
| VS Code o IDE Java equivalente | editing e navigazione del codice |
| Git | salvataggio esercizi ed evidenze |

Non sono richiesti nuovi strumenti rispetto alle UD precedenti.

## Struttura della giornata

| Fase | Durata indicativa | Attività |
|---|---:|---|
| Apertura | 30 min | ripresa di Collections e Generics |
| Teoria 1 | 60 min | lambda expression e interfacce funzionali |
| Teoria 2 | 75 min | Stream API e pipeline base |
| Laboratorio guidato | 120 min | refactoring di un catalogo corsi con stream |
| Laboratorio autonomo | 180 min | report su attività formative |
| Debriefing | 15 min | confronto sulle soluzioni |

## Confine della UD

Questa UD non tratta:

- parallel stream;
- collector avanzati;
- `groupingBy` come argomento principale;
- Optional avanzato;
- Maven;
- database;
- persistenza su file;
- Spring.

Questi argomenti saranno affrontati quando saranno realmente utili nel percorso.
