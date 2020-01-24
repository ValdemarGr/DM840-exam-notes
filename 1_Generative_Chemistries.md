# Generative Chemistries
## Graph Grammars & Double pushout
Rules based modification.\
Graph grammar -> Chomsky grammar in language theory.
### Graph transformation
Given two graphs $L$ and $R$, rule $p=(L, R)$, rule says "we can replace L with R", maybe the replacement is the entire graph, maybe only a subgraph.

How to connected replaced graph?

(isomorphic for match, canon for replace)

#### Node label replacement approach
For each removed dangling edge, find matching label and plug it in.

#### Double pushout
$p = (L,K,R)$

* $K$ is intersection, eg intersection $L \cap R$
* $L$ is preconditions of the rule.
* $R$ is the postcondition of the rule.

A _direct graph transformation_ is when $L$ is matched on the host graph $G$.
$L \backslash K$ is the elements to remove.

Given a match $m$ of $L$ on $G$, we create the remaining structure by:
$$D:=(G \backslash m(L)) \cup m(K)$$
And the removed:
$$L \backslash K$$
And to obtain the derived graph:
$$glue(D, R \backslash K)$$

The glue operations are as for input graph $G$ and output graph $H$:
$$G = L+_KD$$
$$H=R +_K D$$

$D$ is our context graph.

Allow non-injective (many matches in one graph).

## Pushout in chemistry
Bonds.

Rules are graph grammars, starting graph and transformation rules:
$$H = (G, R)$$

Generation formula:
$$P_i = \left\{ p \in Q | P = \bigcup_{0 \leq j < i} P_j, t \in R: M(P, c_L (t)) \Rightarrow^t Q \right\} \Bigg\backslash \bigcup_{0 \leq j < i} P_j$$

## Reaction networks
Output is directed hypergraph

$$\{g_1, g_2\}\Rightarrow^p g_3$$

* **Morphism** transformation, preservation off nodes is not important.
* **Monomorphism** nodewise transformation, preservation of nodes and edges must exist, but more edges can be added on the product.
* **Subgraph isomorphism** For a chosen mapping, all nodes and fuck.
* **Isomoprhism** two equal graphs

Representationally equal = excact match.

Some info from paper
## ILP
Ask Søren