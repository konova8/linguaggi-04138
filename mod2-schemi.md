# Introduzione
- Linguaggi Imperativi
  - Basati sulla nozione di stato
  - Le istruzioni sono **comandi** che cambiano lo stato
  - Es. C, Pascal, FORTRAN, COBOL, ...
- Linguaggi Dichiarativi
  - Basati sulla nozione di funzione
  - Le istruzioni sono **dichiarazioni** di nuovi valori, tramite singole funzioni o composizione di esse

I lingauggi dichiarativi si dividono in due gruppi
- Funzionali (Lisp, Scheme, ML, Haskell)
  - Basati sulle funzioni
- Logici (Prolog, CLP)
  - Basati sulle relazioni

## Come Confrontare i Linguaggi

    DA FARE

# Nomi
- Un **nome** è una sequenza di caratteri usata per *denotare* qualcos'altro
- Nei linguaggi sono spesso **identificatori**

Usiamo questi termini in modo equivalente:
- binding = legame = associazione
- environment = ambiente
- scope = portata, estensione
- lifetime = vita

## Binding
- **Statico**
  - Progettazione del linguaggio
  - Scrittura del programma
  - Compilazione
- **Dinamico**
  - Esecuzione

## Ambiente
**Ambiente**: Insieme di associazioni fra nomi e oggetti denotabili esistenti a run-time in uno specifico punto del programma ed in uno specifico momento dell'esecuzione

**Dichiarazioni**: Meccanismo col quale si crea un'associazione in ambiente

Quindi lo stesso nome può indicare oggetti diversi all'interno del programma

**Aliasing**: Nomi diversi denotano lo stesso oggetto, passaggio per riferimento, puntatori, ecc.

## Blocchi
Regione testuale del programma, identificvato da un inizio e una fine. Può contenere dichiarazioni *locali* a quella regione

Vengono usati per fare chiarezza, per dare struttura al programma. Inoltre permettono di ottimizzare l'occupazione della memoria e permettono la ricorsione

### Suddividiamo l'ambiente
L'ambiente in uno specifico blocco può essere suddiviso in:
- **Ambiente Locale**: Associazioni create all'interno del blocco
- **Ambiente non Locale**: Associazioni ereditate da altri blocchi
- **Ambiente globale**: Quella parte di ambiente non locale relativo alle associazioni comuni a tutti i blocchi

## Operazioni
### Sull'Ambiente
- Creazione: associazione nome - oggetto denotato
  - Dichiarazione locale in blocco
- Riferimento: oggetto denotato mediante il suo nome
  - Uso di un nome
- Disattivazione: assocazione nome - oggetto denotato (maschera di un nome)
- Riattivazione: assocazione nome - oggetto denotato (uscita dal blocco che mascherava il nome)
- Distruzione: assocazione nome - oggetto denotato (uscita dal blocco con dichiarazione locale)

### Sugli oggetti denotabili
- Creazione
- Accesso
- Modifica (se modificabile)
- Distruzione

### Eventi fondamentali
1. Creazione di un oggetto
2. Creazione di un legame per l'oggetto
3. Riferimento all'oggetto, tramite il legame
4. Disattivazione di un legame
5. Riattivazione di un legame
6. Distruzione di un legame
7. Distruzione di un oggetto

Il tempo tra 1 e 7 è la **vita dell'oggetto**

Il tempo tra 2 e 6 è la **vita dell'assocazione**

## Vita
La vita di un oggetto *non* coincide con la vita dei legami per quell'oggetto, può essere sia più breve che più lunga

``` pascal
procedure P (var X:integer); begin ... end; // X è un parametro formale
...
var A:integer;
...
P(A); // A è un parametro attuale
```
Durante l'esecuzione di `P` esiste un legame tra `X` e un oggetto che esiste prima e dopo l'esecuzione

``` C
int *X, *Y;
...
X = (int *) malloc (sizeof (int));
Y = X;
...
free(X);
X = null;
```

Dopo la `free` non esiste più l'oggetto, ma esiste ancora un legame ("pendente") per esso (Y): *dangling reference*

## Scope
In presenza di procedure lo scope varia a seconda del linguaggio, se ha scope statico o dinamico

Un riferimento non-locale in un blocco B può essere risolto:
- Nel blocco che *include sintatticamente* B (scope statico)
- Nel bloco che è *eseguito immediatamente prima* di B (scope dinamico)

### Esempio di esercizio, cosa stampa questo programma?
``` C
{int x = 0;
 void pippo(int n){
    x = n + 1;
 }
 pippo(3);
 write(x);
    {int x = 0;
     pippo(3);
     write(x);
    }
 write(x);
}
```

Questo stampa `444` con scope dinamico, `404` con scope statico.
