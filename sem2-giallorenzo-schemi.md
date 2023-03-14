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









