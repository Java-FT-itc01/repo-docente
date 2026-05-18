# Presentazione UD21.v2 - CRUD console, DAO, Singleton e pattern applicativi

## Scopo della giornata

La UD21.v2 porta il partecipante da un insieme di classi Java funzionanti a una piccola applicazione console organizzata secondo ruoli applicativi riconoscibili.

Il punto centrale non è soltanto costruire un CRUD, ma capire perché un'applicazione deve essere separata in parti con responsabilità diverse.

La UD introduce inoltre il tema dei pattern come soluzioni ricorrenti a problemi ricorrenti di progettazione.

## Risultato atteso

Al termine della giornata si dovrà essere in grado di progettare e saper motivarne le scelte arichitetturali di una console application CRUD nella quale:

- il model rappresenta gli oggetti del dominio;
- il DAO isola l'accesso ai dati;
- il service contiene regole applicative;
- la view gestisce input e output;
- il controller coordina il flusso dell'applicazione;
- il main si limita ad avviare e collegare i componenti;
- il Singleton viene usato in modo controllato per condividere un repository in memoria.

Il partecipante deve inoltre saper spiegare che un pattern non è una regola obbligatoria, ma una soluzione riutilizzabile a un problema progettuale ricorrente.

## Pattern messi in evidenza nella UD21

| Pattern o principio | Stato nel percorso | Uso nella UD21 |
|---|---|---|
| MVC | già introdotto e riutilizzato | separazione tra model, view e controller |
| DAO | applicato operativamente | accesso ai dati tramite interfaccia |
| Singleton | introdotto in modo esplicito | repository in memoria condiviso |
| Service Layer | applicato operativamente | regole applicative fuori da view e DAO |
| Dependency Injection manuale | accennata e applicata | dipendenze passate tramite costruttore |
| Layered Architecture | progressiva | organizzazione per livelli |

## Collegamento con le UD successive

La UD21 prepara direttamente:

| UD successiva | Collegamento |
|---|---|
| UD22 - Stato condiviso e sincronizzazione | limiti del Singleton in presenza di thread |
| UD23-UD25 - Database e SQL | passaggio da memoria a persistenza relazionale |
| UD26-UD27 - Maven, JDBC e DAO JDBC | sostituzione del DAO in memoria con DAO JDBC |
| UD29 - DTO, API REST e microservizi | separazione tra dati interni e dati trasferiti |
| UD30 - JPA/Hibernate | repository/entity e persistenza ORM |
| UD31 - Spring Boot e MVC | DI reale, repository Spring Data, service e controller |

## Struttura della giornata

| Fase | Contenuto | Durata indicativa |
|---|---|---:|
| 1 | Richiamo su CRUD e responsabilità | 45 min |
| 2 | Introduzione ai pattern e cenno ai GoF | 45 min |
| 3 | Varianti del Singleton e limiti | 60 min |
| 4 | DAO, Singleton e dipendenze nel CRUD | 45 min |
| 5 | Laboratorio guidato | 2 h |
| 6 | Laboratorio autonomo | 3 h |
| 7 | Evidenze, confronto e consolidamento | 45 min |

## Messaggio didattico principale

La progettazione di un'applicazione non consiste soltanto nello scrivere classi che compilano.

Consiste nel decidere:

- dove mettere i dati;
- dove mettere le regole;
- dove mettere l'accesso alla persistenza;
- dove mettere l'interazione con l'utente;
- come ridurre le dipendenze dirette tra classi concrete;
- quali pattern applicare e con quali limiti.

In questa UD il Singleton è importante, ma non è il centro dell'architettura. Il centro dell'architettura resta la separazione delle responsabilità.
