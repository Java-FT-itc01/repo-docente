# Introduzione ai pattern architetturali e ai GoF

## Perché parlare di pattern in questa UD

Nelle unità precedenti sono stati introdotti molti elementi tecnici di Java: classi, ereditarietà, interfacce, collections, stream, eccezioni e persistenza su file.

Con la UD21 questi elementi vengono usati insieme per costruire una piccola applicazione organizzata.

A questo punto diventa utile introdurre il concetto di **pattern**.

Un pattern è una soluzione ricorrente a un problema ricorrente di progettazione.

Non è una libreria.
Non è una classe da copiare sempre.
Non è una regola obbligatoria.

È un modo riconoscibile di organizzare il codice quando si presenta un certo tipo di problema.

## Cenno ai Gang of Four

Nel 1994 Erich Gamma, Richard Helm, Ralph Johnson e John Vlissides pubblicarono il libro **Design Patterns: Elements of Reusable Object-Oriented Software**.

Gli autori sono comunemente noti come **Gang of Four**, spesso abbreviato in **GoF**.

Il libro ha raccolto e formalizzato 23 pattern di progettazione object-oriented, organizzati in tre grandi categorie:

| Categoria | Scopo |
|---|---|
| Creazionali | riguardano la creazione degli oggetti |
| Strutturali | riguardano la composizione tra classi e oggetti |
| Comportamentali | riguardano collaborazione e comportamento tra oggetti |

Il Singleton è uno dei pattern creazionali descritti dai GoF.

## Pattern GoF, architetturali ed enterprise

Nel percorso non useremo solo pattern GoF.

Useremo anche pattern architetturali ed enterprise, cioè soluzioni tipiche delle applicazioni a livelli, delle applicazioni web e delle applicazioni che accedono a dati persistenti.

| Tipo di pattern | Esempi | Uso nel percorso |
|---|---|---|
| GoF | Singleton, Strategy, Factory | introduzione a soluzioni object-oriented ricorrenti |
| Architetturali | MVC, Layered Architecture | organizzazione generale dell'applicazione |
| Enterprise | DAO, Repository, DTO, Service Layer, Mapper | applicazioni con dati, servizi, API e database |
| Framework-related | Dependency Injection, Spring MVC, Spring Data Repository | gestione delle dipendenze e livelli in Spring |

## Pattern già incontrati o applicati nel percorso

| Pattern | Stato nel percorso | Dove compare |
|---|---|---|
| **MVC** | già introdotto e riutilizzato | UD16, UD21, UD31 |
| **DAO** | introdotto operativamente in UD21 | UD21, UD27 |
| **Singleton** | introdotto in UD21 | UD21, UD22 |
| **Service Layer** | applicato progressivamente | UD21, UD27, UD29, UD30, UD31 |
| **Dependency Injection manuale** | applicata in forma semplice | UD21, UD27 |
| **Repository** | introdotto più avanti | UD27, UD30, UD31 |
| **DTO** | introdotto più avanti | UD29, UD30, UD31 |
| **Mapper** | introdotto più avanti | UD29, UD30, UD31 |
| **Layered Architecture** | filo conduttore | UD21-UD31 |
| **Spring MVC** | applicazione finale del modello MVC | UD31 |

## Pattern non trattati direttamente

Nel percorso verranno citati, ma non approfonditi in modo operativo, pattern come:

- Observer;
- Command;
- Builder;
- Adapter;
- Decorator;
- Proxy;
- Template Method;
- Chain of Responsibility.

Questi pattern appartengono al patrimonio generale della progettazione object-oriented, ma non sono centrali per gli obiettivi applicativi del percorso.

## Pattern e responsabilità

Il valore dei pattern si comprende meglio quando si collegano a un problema concreto.

| Problema | Soluzione ricorrente |
|---|---|
| il codice del menu contiene tutto | separazione MVC |
| l'accesso ai dati è sparso nel programma | DAO / Repository |
| le regole applicative sono distribuite ovunque | Service Layer |
| vengono create istanze multiple di un archivio in memoria | Singleton controllato |
| una classe dipende direttamente da una classe concreta | interfaccia + Dependency Injection |
| il client non deve vedere direttamente entity/model interni | DTO + Mapper |

## Regola didattica

In questa UD non si studiano i pattern come elenco teorico.

Si osservano pattern già applicati o prossimi all'applicazione nel percorso.

Il partecipante deve arrivare a riconoscere questo principio:

```text
Un pattern ha senso solo se risolve un problema progettuale reale.
```

## Collegamento con il laboratorio

Nel laboratorio guidato verrà realizzato un CRUD console con questi elementi:

- `model` per rappresentare il corso;
- `dao` per nascondere l'accesso ai dati;
- `service` per le regole applicative;
- `view` per input e output;
- `controller` per coordinare il menu;
- `main` per avviare l'applicazione;
- Singleton per evitare più repository in memoria non coordinati.

Questa struttura prepara il passaggio a JDBC, JPA e Spring.
