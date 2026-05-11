# 04 - LAB15 autonomo: gestione attività di un corso

## Obiettivo

Realizzare una piccola applicazione Java per gestire attività diverse di un corso, applicando consapevolmente:

- classe astratta;
- ereditarietà;
- overriding;
- interfaccia per comportamento opzionale;
- array polimorfico;
- `instanceof` e cast sicuro;
- refactoring per ridurre controlli di tipo inutili;
- servizio separato dal `main`.

## Scenario

Un corso Java è composto da attività diverse:

- lezioni teoriche;
- laboratori guidati;
- challenge autonome;
- quiz valutativi.

Tutte le attività hanno:

- codice;
- titolo;
- durata in minuti.

Tutte devono poter restituire una descrizione leggibile.

Solo alcune attività sono valutabili:

- `ChallengeAutonoma`;
- `QuizValutativo`.

Le lezioni teoriche e i laboratori guidati non sono valutabili.

## Struttura richiesta

Creare il progetto:

```text
UD15_gestione_attivita_corso/
  src/
    corso/
      ud15/
        attivita/
          AttivitaCorso.java
          LezioneTeorica.java
          LaboratorioGuidato.java
          ChallengeAutonoma.java
          QuizValutativo.java
          Valutabile.java
          AttivitaService.java
          EseguiGestioneAttivita.java
  docs/
    evidence_UD15_autonomo.md
```

## Verifica strumenti

Prima di iniziare:

```bash
java -version
javac -version
```

Non usare Maven in questa esercitazione. La compilazione deve essere fatta con `javac`, per consolidare package, classpath e struttura delle cartelle.

Non sono richiesti DBMS, Docker, phpMyAdmin, Bootstrap o jQuery.

## Requisiti del modello

### 1. Classe astratta `AttivitaCorso`

Deve contenere almeno:

- `codice`;
- `titolo`;
- `durataMinuti`.

Deve avere:

- costruttore;
- getter;
- metodo astratto `descriviAttivita()`;
- metodo concreto `isLunga()` che restituisce `true` se la durata supera 90 minuti.

### 2. Classe `LezioneTeorica`

Estende `AttivitaCorso`.

Attributo specifico:

- `argomentoPrincipale`.

Deve ridefinire `descriviAttivita()`.

### 3. Classe `LaboratorioGuidato`

Estende `AttivitaCorso`.

Attributi specifici:

- `richiedePc`;
- `numeroPassaggiGuidati`.

Deve ridefinire `descriviAttivita()`.

### 4. Interfaccia `Valutabile`

Deve dichiarare:

```java
int getPunteggioMassimo();
boolean isSuperato(int punteggioOttenuto);
String getCriterioValutazione();
```

### 5. Classe `ChallengeAutonoma`

Estende `AttivitaCorso` e implementa `Valutabile`.

Attributi specifici:

- `livelloDifficolta`;
- `punteggioMassimo`;
- `sogliaSuperamento`.

### 6. Classe `QuizValutativo`

Estende `AttivitaCorso` e implementa `Valutabile`.

Attributi specifici:

- `numeroDomande`;
- `punteggioMassimo`;
- `sogliaSuperamento`.

### 7. Classe `AttivitaService`

Deve contenere almeno questi metodi:

```java
void stampaProgramma(AttivitaCorso[] attivita);
void stampaAttivitaLunghe(AttivitaCorso[] attivita);
void stampaAttivitaValutabili(AttivitaCorso[] attivita);
int contaAttivitaValutabili(AttivitaCorso[] attivita);
void simulaValutazione(AttivitaCorso[] attivita, int punteggioOttenuto);
```

### Requisiti sul servizio

Nel servizio:

- `stampaProgramma()` deve usare il metodo polimorfico `descriviAttivita()`;
- `stampaAttivitaLunghe()` deve usare il metodo `isLunga()`;
- `stampaAttivitaValutabili()` deve usare `instanceof Valutabile`;
- `simulaValutazione()` deve usare `instanceof Valutabile` e cast sicuro;
- non devono essere presenti blocchi lunghi basati su `instanceof LezioneTeorica`, `instanceof LaboratorioGuidato`, `instanceof ChallengeAutonoma`, `instanceof QuizValutativo` per descrivere le attività.

## Requisiti del `main`

Nel file `EseguiGestioneAttivita.java`:

1. creare un array `AttivitaCorso[]` con almeno 6 attività;
2. inserire almeno:
   - una `LezioneTeorica`;
   - due `LaboratorioGuidato`;
   - una `ChallengeAutonoma`;
   - due `QuizValutativo`;
3. creare un oggetto `AttivitaService`;
4. stampare:
   - programma completo;
   - attività lunghe;
   - attività valutabili;
   - numero totale di attività valutabili;
   - simulazione di valutazione con almeno due punteggi diversi.

## Vincoli

- Usare array, non `ArrayList`.
- Non usare Maven.
- Non usare input da tastiera.
- Non concentrare tutta la logica nel `main`.
- Usare package coerente: `corso.ud15.attivita`.
- I nomi delle classi devono essere coerenti con il dominio.
- Il codice deve compilare da terminale.

## Compilazione

Dalla radice del progetto:

```bash
javac -encoding UTF-8 -d out src/corso/ud15/attivita/*.java
```

## Esecuzione

```bash
java -cp out corso.ud15.attivita.EseguiGestioneAttivita
```

## File di evidenza

Creare:

```text
docs/evidence_UD15_autonomo.md
```

Il file deve contenere:

### 1. Struttura del progetto

Riportare l'albero delle cartelle principali.

### 2. Diagramma UML Mermaid

Inserire un diagramma classi Mermaid con:

- classe astratta;
- sottoclassi;
- interfaccia `Valutabile`;
- relazione di implementazione.

### 3. Comandi usati

Riportare:

```bash
javac -encoding UTF-8 -d out src/corso/ud15/attivita/*.java
java -cp out corso.ud15.attivita.EseguiGestioneAttivita
```

### 4. Output ottenuto

Incollare l'output del programma.

### 5. Risposte progettuali

Rispondere alle seguenti domande:

1. Dove è stato usato l'upcasting?
2. Dove è stato usato `instanceof`?
3. Perché è stato usato `instanceof Valutabile` invece di controllare ogni classe concreta?
4. Quale metodo dimostra il polimorfismo?
5. Quale modifica sarebbe necessaria per aggiungere una nuova attività valutabile?
6. Quale modifica sarebbe necessaria per aggiungere una nuova attività non valutabile?
7. Quale parte del codice sarebbe peggiorata se tutto fosse stato gestito con `if` e `instanceof` sulle classi concrete?

## Criteri di accettazione

Il laboratorio è accettabile se:

- il codice compila;
- il programma produce output leggibile;
- il servizio non contiene logica eccessiva basata sulle singole classi concrete;
- il cast è sempre protetto da `instanceof`;
- l'interfaccia `Valutabile` è usata correttamente;
- il file di evidenza contiene diagramma, comandi, output e risposte progettuali.

## Estensione facoltativa

Aggiungere una nuova classe `ProjectWorkFinale` che:

- estende `AttivitaCorso`;
- implementa `Valutabile`;
- ha attributo `nomeProgetto`;
- viene inserita nell'array del `main`.

Verificare che i metodi del servizio che lavorano su `Valutabile` continuino a funzionare senza modifiche.
