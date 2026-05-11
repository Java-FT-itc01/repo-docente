# 03 - LAB15 guidato: materiali didattici, cast e refactoring polimorfico

## Obiettivo del laboratorio

Realizzare un piccolo modello Java per rappresentare materiali didattici diversi, osservando progressivamente:

- upcasting automatico;
- visibilità dei metodi dal tipo del riferimento;
- downcasting esplicito;
- uso sicuro di `instanceof`;
- refactoring da controlli di tipo a metodi polimorfici;
- uso di un'interfaccia per un comportamento opzionale.

## Scenario

Una piattaforma didattica contiene materiali di diverso tipo:

- dispense PDF;
- video lezioni;
- quiz di verifica.

Tutti i materiali hanno un titolo, una durata stimata di studio e una descrizione pubblica.

Solo alcuni materiali sono valutabili. In particolare, il quiz è valutabile, mentre una dispensa o una video lezione non lo sono.

## Struttura consigliata

Creare questa struttura:

```text
UD15_lab_guidato_materiali/
  src/
    corso/
      ud15/
        materiali/
          MaterialeDidattico.java
          DispensaPdf.java
          VideoLezione.java
          QuizVerifica.java
          Valutabile.java
          MaterialeService.java
          EseguiMaterialiDidattici.java
```

## Verifica strumenti prima del laboratorio

Da terminale verificare:

```bash
java -version
javac -version
```

Se si usa VS Code, verificare che sia disponibile il supporto Java creando un file `.java` e controllando che l'IDE riconosca parole chiave, package e suggerimenti di compilazione.

Per questo laboratorio non servono:

- Maven;
- Docker;
- DBMS;
- phpMyAdmin;
- Bootstrap;
- jQuery.

La compilazione avverrà con `javac`.

## Passo 1 - Creare la classe astratta `MaterialeDidattico`

Creare il file `MaterialeDidattico.java`.

```java
package corso.ud15.materiali;

public abstract class MaterialeDidattico {
    private String titolo;
    private int durataStimataMinuti;

    public MaterialeDidattico(String titolo, int durataStimataMinuti) {
        this.titolo = titolo;
        this.durataStimataMinuti = durataStimataMinuti;
    }

    public String getTitolo() {
        return titolo;
    }

    public int getDurataStimataMinuti() {
        return durataStimataMinuti;
    }

    public abstract String descriviMateriale();
}
```

### Osservazione

`descriviMateriale()` è astratto perché ogni materiale deve descriversi in modo diverso.

Questo prepara il laboratorio al refactoring polimorfico: il servizio non dovrà chiedere continuamente se un materiale è una dispensa, un video o un quiz.

## Passo 2 - Creare le sottoclassi concrete

### `DispensaPdf.java`

```java
package corso.ud15.materiali;

public class DispensaPdf extends MaterialeDidattico {
    private int numeroPagine;

    public DispensaPdf(String titolo, int durataStimataMinuti, int numeroPagine) {
        super(titolo, durataStimataMinuti);
        this.numeroPagine = numeroPagine;
    }

    public int getNumeroPagine() {
        return numeroPagine;
    }

    @Override
    public String descriviMateriale() {
        return "Dispensa PDF: " + getTitolo()
                + " - pagine: " + numeroPagine
                + " - studio stimato: " + getDurataStimataMinuti() + " min";
    }
}
```

### `VideoLezione.java`

```java
package corso.ud15.materiali;

public class VideoLezione extends MaterialeDidattico {
    private String docente;

    public VideoLezione(String titolo, int durataStimataMinuti, String docente) {
        super(titolo, durataStimataMinuti);
        this.docente = docente;
    }

    public String getDocente() {
        return docente;
    }

    @Override
    public String descriviMateriale() {
        return "Video lezione: " + getTitolo()
                + " - docente: " + docente
                + " - durata: " + getDurataStimataMinuti() + " min";
    }
}
```

## Passo 3 - Creare l'interfaccia `Valutabile`

Creare il file `Valutabile.java`.

```java
package corso.ud15.materiali;

public interface Valutabile {
    int getPunteggioMassimo();
    boolean isSuperato(int punteggioOttenuto);
}
```

Questa interfaccia rappresenta una capacità opzionale.

Non tutti i materiali sono valutabili. Solo alcuni oggetti potranno essere trattati anche come `Valutabile`.

## Passo 4 - Creare `QuizVerifica`

```java
package corso.ud15.materiali;

public class QuizVerifica extends MaterialeDidattico implements Valutabile {
    private int numeroDomande;
    private int punteggioMassimo;
    private int sogliaSuperamento;

    public QuizVerifica(String titolo, int durataStimataMinuti, int numeroDomande,
            int punteggioMassimo, int sogliaSuperamento) {
        super(titolo, durataStimataMinuti);
        this.numeroDomande = numeroDomande;
        this.punteggioMassimo = punteggioMassimo;
        this.sogliaSuperamento = sogliaSuperamento;
    }

    public int getNumeroDomande() {
        return numeroDomande;
    }

    @Override
    public int getPunteggioMassimo() {
        return punteggioMassimo;
    }

    @Override
    public boolean isSuperato(int punteggioOttenuto) {
        return punteggioOttenuto >= sogliaSuperamento;
    }

    @Override
    public String descriviMateriale() {
        return "Quiz di verifica: " + getTitolo()
                + " - domande: " + numeroDomande
                + " - punteggio massimo: " + punteggioMassimo;
    }
}
```

## Passo 5 - Creare un array polimorfico

Nel file `EseguiMaterialiDidattici.java` creare un array di `MaterialeDidattico`.

```java
package corso.ud15.materiali;

public class EseguiMaterialiDidattici {
    public static void main(String[] args) {
        MaterialeDidattico[] materiali = new MaterialeDidattico[3];

        materiali[0] = new DispensaPdf("Polimorfismo in Java", 45, 18);
        materiali[1] = new VideoLezione("Interfacce e contratti", 60, "Docente Java");
        materiali[2] = new QuizVerifica("Quiz su ereditarietà", 20, 10, 30, 18);

        for (int i = 0; i < materiali.length; i++) {
            System.out.println(materiali[i].descriviMateriale());
        }
    }
}
```

### Domanda guida

Perché l'array può contenere oggetti di tre classi diverse?

Risposta attesa: perché tutte le classi concrete estendono `MaterialeDidattico`, quindi ogni oggetto può essere trattato come `MaterialeDidattico`.

## Passo 6 - Osservare il limite del riferimento generale

Aggiungere temporaneamente questa riga nel ciclo:

```java
System.out.println(materiali[i].getNumeroDomande());
```

La riga non compila, perché `getNumeroDomande()` non appartiene a `MaterialeDidattico`.

Questo è un punto importante: il metodo esiste sull'oggetto reale solo quando l'oggetto è un `QuizVerifica`, ma il riferimento `materiali[i]` è dichiarato come `MaterialeDidattico`.

Rimuovere la riga dopo aver osservato l'errore.

## Passo 7 - Usare `instanceof` e downcasting

Aggiungere nel ciclo:

```java
if (materiali[i] instanceof QuizVerifica) {
    QuizVerifica quiz = (QuizVerifica) materiali[i];
    System.out.println("Numero domande del quiz: " + quiz.getNumeroDomande());
}
```

### Osservazione

Il cast è sicuro perché viene eseguito solo dopo il controllo `instanceof`.

## Passo 8 - Usare `instanceof` sull'interfaccia

Aggiungere un controllo più flessibile:

```java
if (materiali[i] instanceof Valutabile) {
    Valutabile valutabile = (Valutabile) materiali[i];
    System.out.println("Punteggio massimo: " + valutabile.getPunteggioMassimo());
    System.out.println("Con 20 punti è superato? " + valutabile.isSuperato(20));
}
```

Questa soluzione non dipende dalla classe concreta `QuizVerifica`, ma dal contratto `Valutabile`.

Se in futuro anche una `ChallengePratica` implementasse `Valutabile`, questo codice continuerebbe a funzionare senza modifiche.

## Passo 9 - Creare `MaterialeService`

Creare il file `MaterialeService.java`.

```java
package corso.ud15.materiali;

public class MaterialeService {
    public void stampaCatalogo(MaterialeDidattico[] materiali) {
        for (int i = 0; i < materiali.length; i++) {
            if (materiali[i] != null) {
                System.out.println(materiali[i].descriviMateriale());
            }
        }
    }

    public void stampaMaterialiValutabili(MaterialeDidattico[] materiali) {
        for (int i = 0; i < materiali.length; i++) {
            if (materiali[i] instanceof Valutabile) {
                Valutabile valutabile = (Valutabile) materiali[i];
                System.out.println(materiali[i].getTitolo()
                        + " - max: " + valutabile.getPunteggioMassimo());
            }
        }
    }
}
```

## Passo 10 - Aggiornare il `main`

```java
package corso.ud15.materiali;

public class EseguiMaterialiDidattici {
    public static void main(String[] args) {
        MaterialeDidattico[] materiali = new MaterialeDidattico[3];

        materiali[0] = new DispensaPdf("Polimorfismo in Java", 45, 18);
        materiali[1] = new VideoLezione("Interfacce e contratti", 60, "Docente Java");
        materiali[2] = new QuizVerifica("Quiz su ereditarietà", 20, 10, 30, 18);

        MaterialeService service = new MaterialeService();

        System.out.println("=== Catalogo materiali ===");
        service.stampaCatalogo(materiali);

        System.out.println();
        System.out.println("=== Materiali valutabili ===");
        service.stampaMaterialiValutabili(materiali);
    }
}
```

## Compilazione ed esecuzione

Dalla cartella principale del laboratorio:

```bash
javac -encoding UTF-8 -d out src/corso/ud15/materiali/*.java
java -cp out corso.ud15.materiali.EseguiMaterialiDidattici
```

## Output atteso indicativo

```text
=== Catalogo materiali ===
Dispensa PDF: Polimorfismo in Java - pagine: 18 - studio stimato: 45 min
Video lezione: Interfacce e contratti - docente: Docente Java - durata: 60 min
Quiz di verifica: Quiz su ereditarietà - domande: 10 - punteggio massimo: 30

=== Materiali valutabili ===
Quiz su ereditarietà - max: 30
```

## Evidenza richiesta

Creare il file:

```text
docs/evidence_UD15_guidato.md
```

Inserire:

1. struttura delle classi creata;
2. comando di compilazione;
3. comando di esecuzione;
4. output ottenuto;
5. risposta alle seguenti domande:

### Domande

1. Perché `MaterialeDidattico[]` può contenere oggetti `DispensaPdf`, `VideoLezione` e `QuizVerifica`?
2. Perché `getNumeroDomande()` non è visibile da un riferimento `MaterialeDidattico`?
3. Perché il cast verso `QuizVerifica` deve essere protetto da `instanceof`?
4. Perché il controllo `instanceof Valutabile` è più flessibile di `instanceof QuizVerifica`?
5. Quale parte del codice è polimorfica e quale parte usa un controllo di tipo esplicito?

## Estensione guidata facoltativa

Aggiungere una nuova classe `ChallengePratica` che estende `MaterialeDidattico` e implementa `Valutabile`.

Verificare che `MaterialeService.stampaMaterialiValutabili()` funzioni senza modifiche.

Questo conferma che il servizio dipende dall'interfaccia `Valutabile`, non dalla singola classe `QuizVerifica`.
