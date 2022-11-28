# Capitolo 1
## Interpreti e Compilatori
$I^{L_A}_{L_B} \rArr$ Interprete scritto in $L_A$ per il linguaggio $L_B$

$C^{L_A}_{L_B, L_C} \rArr$ Compilatore scritto in $L_A$, che traduce da $L_B$ in $L_C$

# Capitolo 2
## Descrivere un linguaggio di programmazione
- **Sintassi**: Regole di formazione
- **Semantica**: Attribuzione di significato
- **Pragmatica**: In quale modo frasi corrette?
- **Implementazione**: Come eseguire una frase corretta

Quest'ultimo ovvimente solo per i linguaggi di programmazione

Linguaggi:
- Generativi (da Grammatica)
- Riconoscitivi(da Automa)

## Grammatica Libera
- Quadrupla $(NT, T, R, S)$ dove
  - $NT$ insieme finito di simboli non-terminali
  - $T$ insieme finito di simboli terminali
  - $R$ insieme finito di produzioni nella forma
    - $V \rarr w$ dove $V \in NT$ e $w \in (T \cup NT)^*$
  - $S \in NT$ è detto simbolo iniziale
- Esempio: $G = (\{S\}, \{a, b, +, *\}, S, R)$
  - dove $R = \{S \rarr a, S \rarr b, S \rarr S + S, S \rarr S * S\}$

Forma in cui si trova di solito: $S \rarr a | b | S + S | S * S$

## Derivazioni
Data $G = (NT, T, R, S)$ libera diciamo che *$v$ si riscrive in un passo in $w$*, denotato con $v \rArr w$, se

$$\frac{v = xAy \qquad (A \rarr z) \in R \qquad w = xzy}{v \rArr w} \qquad x, y, z \in (T \cup NT)^*$$

Diciamo inoltre che *$v$ si riscrive in $w$*, denotato con $v \rArr^* w$ se $v$ si riscrive in più passi in $w$

## Linguaggio generato
Il linguaggio generato da una grammatica $G = (NT, T, R, S)$ è l'insieme
$$L(G) = \{ w \in T^* | S \rArr^* w\}$$

Esempio: $G: S \rarr aSb | ab$ genera $L(G) = \{a^n b^n | n \geq 1 \}$

## Derivazioni e Alberi
- Consideriamo $S \rarr a | b | c | S+S | S*S$ e l'espressione $a + b * c$
- Consideriamo la **derivazione** $S \rArr \underbar{S} * S \rArr \underbar{S} + S * S \rArr a + \underbar{S} * S \rArr a + b * \underbar{S} \rArr a + b * c$
  - **leftmost**: ad ogni passo riscriviamo il non terminale più a sinistra
  - possibile associarci un *albero di derivazione*
- Consideriamo ora la **derivazione** $S \rArr S * \underbar{S} \rArr \underbar{S} * c \rArr S + \underbar{S} * c \rArr \underbar{S} + b * c \rArr a + b * c$
  - **rightmost**: ad ogni passo riscriviamo il non terminale più a destra
  - possibile associarci lo stesso *albero di derivazione*

**Teorema**: $w \in T^*$ appartiene a $L(G)$ $\iff$ ammette un albero di derivazione completo

## Ambiguità
