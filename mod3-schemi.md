# Introduzione ai tipi
## Un po' di storia
**Paradosso di Russel**: \
Sia $R = {x | x \not \in x}$ \
Allora $R \in R \iff   R \not \in R$

Con il sistema di tipi si riesce ad evitare il paradosso

### Sistema dei tipi
> Un metodo sintattico praticabile per dimostrare l'assenza di determinati comportamenti del
programma, fatto classificando le unità sintattiche in base ai tipi di valore che assumono.

Possiamo interpretare i tipi come **collezioni di valori omogenei e rappresentabili**
- Omogeneità: Condivisione di proprietà di struttura
- Rappresentabilità: Numeri non sempre rappresentabili, come i reali o infinito

## Tipi di Dato
### Supporto all'organizzazione Concettuale
I tipi aiutano nella stesura del programma, dalla documentazione alla progettazione

### Supporto all'Astrazione
Modulazione nei linguaggi, che permettono di "impacchettare" e legare insieme diversi parti di software

L'esempio principe sono le interfacce, che associano un tipo ad operazioni

Tramite l'astrazione possiamo usare parti senza sapere l'implementazione sottostante

### Supporto alla Correttezza
**Type-checking**: Possibilità di usare i tipi per controllare errori sintattici/di tipo

**Refactoring**: Ristrutturazione del codice esistente. \
Con un sistema di tipi statico è possibile cambiando un tipo di una struttura il type checker indica tutti i punti in cui è da cambiare il tipo

#### Safety
Capacità di un lingauggio di garantire l'integrità delle sue astrazioni

Lingauggi *safe* sono detti *strongly typed*

Controlli possibili a Compile Time o a Run Time (assegnazione sbagliata o array Out of Bound)

### Supporto all'Implementazione
I tipi possono contribuire all'efficienza dei programmi, come rimuovere controlli dinamici per garantire la sicurezza

## Dynamic vs Static Typing
Linguaggio tipato **staticamente**: Se possiamo controllare i tipi sul testo del programma, senza doverlo eseguire \
Il compilatore può eliminare le annotazioni di tipo dal codice generato

Linguaggio tipato **dinamicamente**: Quando il controllo dei tipi avviene durante l'esecuzione del programma \

**Manifest typing**: Scrivo esplicitamente il tipo della variabile e delle operazioni (di solito linguaggi con questa caratteristica sono compilati)

**Inferred typing**: Gli algoritmi deducono il tipo dal contesto, non è necessario scrivere in modo esplicito il tipo

# Tipi Base e l'Algebra dei Tipi
## Sistema di Tipi
Ogni linguaggio ha un proprio **sistema di tipi**, i valori dei tipi di chiamano **abitanti** (inhabitants), che comprende:
1. Un insieme di *tipi di base*
2. Meccanismi per *definire nuovi tipi*
3. Meccanismi di *computazione sui tipi*, che includono:
  1. Regole di *equivalenza*
  2. Regole di *compatibilità*
  3. Regole/Tecniche di *inferenza*
4. La definizione sul controllo dei vincoli di tipo statico o dinamico

## Tipi Base
Tipi primitivi/semplici/scalari sono i tipi che definiscono i valori denoabili del linguaggio

## Tipo Unit (vs Void)
Tipo elementare che può contenere l'unità singoletto

`void` in C o Java è simile a Unit, ma non possiamo passarlo come argomento delle operazioni

Unit si usa per segnalare che ci saranno side effect dopo l'utilizzo di una funzione (come stampa a schermo, ecc)

## Tipi Booleani
True e False, operazioni logiche principali

A seconda del linguaggio richiedono 1 o più byte

## Tipi Carattere
- Valori: Un insieme di codici di caratteri (ASCII e UNICODE)
- Operazioni: Dipendono dal linguaggio (`==`, `<`, `>`)

## Tipi Interi
- Valori: Un sottoinsieme finito di numeri interi
- Operazioni: uguaglianza, confronti e principali operazioni aritmetiche

## Tipi Reali
- Valori: Un sottoinsieme finito di numeri reali, memorizati tramite virgola fissa/mobile
- Operazioni: uguaglianza, confronti e principali operazioni aritmetiche

## Tipi Enumeration
`enum RogueOne { Jyn, Cassian, Chirrut, K2SO, Bodhi, Baze }`

Introduce un nuovo insieme chiamato `RogueOne` costituito da un insieme di 6 elemnti

*Non tutti i linguaggi integrano gli enum in modo sicuro*, infatti C equipara completamente i valori all'interno dell'`enum` ad interi

## Extensional vs Intensional Types
- **Estensionale**: modo in cui l'utente specifica gli `enum`, ovvero elencando lui i valori possibili
  - quando è più efficiente (per spazio o computazione) specificare gli abitanti del tipo o non abbiamo un insieme chiaro di regole che li definiscono
- **Intensionale**: modo in cui i lingauggi specificano interi, float, ecc. Ovvero non facendoli decidere all''itente
  - quando si dispone di un insieme definito di proprietà che identificano solo gli abitanti (valori validi) del tipo che stiamo definendo, con il vantaggio di risparmiare memoria se l'insieme degli abitanti è grande e di rendere possibile la definizione, nel caso di insiemi infiniti

## Tipi Composti
Possiamo creare nuovi tipi componendo quelli di base (come gli `enum` in C)

Altre strutture possono essere *array*, *insiemi* e *puntatori*

## Tipi Array
Insieme di elementi dello certo tipo, indicizzato da almeno una *chiave identificativa* di un certo tipo

Operazioni:
- Selezione (`a[e]` e `a[e1][e2][e3]`)
  - Nota: è un'operazione del tipo come `+`, ecc
- Assegnazione
- Confronti
- Operazioni Aritmetiche

I linguaggi safe verificano che ogni accesso ad un elemento di un array avvenga entro i limiti di dimensione, il C non lo fa, ed infatti C è soggetto ad attacchi di **buffer-overflow**

Array multidimensionali possono essere memorizzati in *Row-major order* o in *Column-major order*, a seconda di quale implementazione viene scelta (a runtime) e della modalità di accesso può dare vita a prestazioni diverse (più o meno accessi in cache)

Il **numero di dimensioni** e i **loro intervalli** determinano la forma di un array. A seconda del linguaggio si deve o meno fissare la forma degli array. Se la forma è fissa possiamo metterlo in Stack, altrimenti con array dinamici è necessario metterlo nello Heap

Nel caso di array di forma statica nello Stack di memorizza il *Stack Frame*, che comprende *Frame pointer* e i valori dell'array

In caso di array di forma dinamica il descrittore dell'array è memorizzato nello Stack e si chiama **dope vector**:
- Puntatore a dove si trova l'array nella Heap
- Tipo degli elementi
- Dimensioni (rank) dell'array
- Lunghezza
- *Extent in use*, quantità dinamica di memoria per risparmiare spazio
- *Max extent*, circa uguale
- *Stride*, Tipo degli elementi (comprese strutture dati), spesso uguale al tipo degli elementi base

### Differenze tra i tipi Array in C, Java e Rust
![Differenze Array](img-schemi/arr-diff-c-java-rust.png)

## Tipi Insieme
Denota una struttura dati piatta e senza ordine, di valori unici e dello stesso tipo

Operazioni possibili sono *test di inclusione*, e le comuni operazioni sugli insiemi

Un modo efficiente per rappresentare un insiemee un array di lunghezza pari alla cardinalità del tipo di base in bit, chiamato **array caratteristico** \
In esso il bit j-esimo indica se l'elemento appartiene o meno all'insieme. Questo però occupa moltissimo spazio, infatti oggi si usano le tabelle hash

## Tipi Riferimento
> *Qualcosa* che fa riferimento ad un dato

Operazioni possibili sono *creazione*, *controllo dell'uguaglianza* e *deferenziazione* (accesso al dato referenziato)

Particolarmente presenti in linguaggi a basso livello

L'implementazione più comune è quella del puntatore, usati anche con gli array come istanza dei riferimenti

I riferimenti possono diventare:
- **Wild**: Quando non inizializzati
- **Dangling**: Quando il dato referenziato è stato deallocato

Sintassi C: `int* x`, riferimento (puntatore) ad una locazione di memoria (variabile) che contiene un intero

Esiste il puntatore *canonico* `NULL`

I linguaggi con riferimenti forniscono un **operatore di riferimento alle variabili**, in C `&`

Esiste anche un **operatore di dereferenziazione**, in C `*`

``` c
float pi = 3.1415;
float* p = NULL;
p = &pi;
*p = *p + 1;
```

- In questo esempio assegnamo il float alla variabile di nome `pi`
- Dichiariamo un puntatore ad un float con nome `p`, inizializzandolo a `NULL`
- Assegnamo al dereferenziazione di `pi` alla variabile `p`, così che tramite `p` possiamo accedere al valore
- Il box con dentro `3.1415` avrà `4.1415`

### Memory Leak
Nei linguaggi con riferimenti può incorrere una **deallocazione implicita**, ovvero quando cambiamo il valore del puntatore a `NULL` in C

Questo può causare **memory leak**, ovvero porzioni di memoria non accessibili ma ancora in uso \
Alcune soluzioni sono dei *garbage collector* o *borrow checker*

Un'altra cosa che hanno i linguaggi con riferimenti è un operatore di **deallocazione esplicita**, come la `free()` in C \
È buona norma NULLificare un puntatore liberato da `free()`, così da non avere una *dangling reference*

### Rust
In Rust ci sono i puntatori ma hanno una gestione sicura tramite controlli sintattici effettuati a compile time

## Insiemi potenza
Insieme potenza:
$$\mathbb{P}(A) = \{B : B \subseteq A\}$$

Coppia:
$$(a, b) := \{\{a\}, \{a, b\}\}$$

Insieme cartesiano:
$$A \times B := \{(a, b) : a \in A \land b \in B\}$$

Il prodotto cartesiano è un insieme non proprio di $\mathbb{P}(\mathbb{P}(A \cup B))$

## Tipi prodotto
Quando si combinano due o più tipi in una struttura fissa si parla di **tipi prodotto**, come coppie, tuple, record e varianti

### Coppie e Tuple
> Tupla: Prodotto di $n$ tipi come prodotto cartesiano

> Coppia: Tupla a 2 tipi

### Record
Strutture in C/Rust, Classi/Record in Java. Gli elementi di un **record** sono chiamati *campi*

L'ordine dei campi può essere significativo per la rappresentazione in memoria, poichè spesso memorizzati in posizioni contigue abbiamo bisogno di padding in alcuni casi

Esempio di C di SO, dove l'ordine di `long` e `int` nelle strutture cambia la quantità di spazio richiesto in memoria

In Rust inoltre non possiamo assegnare 3 su 5 elementi di un array, ma mettere un elemento nullo, altrimenti avremo un errore

### Pattern Matching
Abbiamo bisogno di costrutti per *destrutturare* i tipi prodotto, uno di questi è il **pattern matching**

Esempio in Rust:
``` rust
struct Person { name: [ char; 3 ], age: i32 }
struct PersonR { name: [ char; 3 ], age: [ char; 4 ] }

let eva = Person{ name: ['E','v','a'], age: 25 };
let Person{ name, age } = eva; // implicit pattern-matching

let evaR = PersonR{ name, age: match age {
  1..=10 => [ 'K','i','d','' ],
  11..=20 => [ 'T','e','e','n' ],
  _ => [ 'O', 'l', 'd', '' ]
}}
```

### Tipi Ricorsivi
Utili per definire strutture dati che possiamo modificare dinamicamente. Esempio di tipo ricorsivo in Java:
```
record IntList( int n, IntList cons ){}
IntList l = new IntList(
  1, new IntList(
    2, new IntList( 3, null )
  )
);
```

