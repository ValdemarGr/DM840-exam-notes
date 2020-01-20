# Graph Isomorphism
## Ullmann subgraph isomorphism
Basically in pure Ullmann we wish to eliminate redundant or impossible permutations.

We can do this with some matrix magic, eg by doing a DFS of potential mapping matricies, which means 1 if this $(i,j)$ maps between the subgraph and supergraph.

Formally given $G_A$ as super and $G_B$ as sub.\
$A$ and $B$ as adjacency matricies.

$n = |V_A|$ and $m=|V_N|$ then permutation matrix can be given by $n * m$ matrix $M$.

* $M$ exactly 1 $1$ on each row.
* $M$ maximally 1 $1$ (can be purely 0) each column.

Permute $B$ by multiplying by M.

$M * B \rightarrow$ Move row $j$ to row $i \,\, \forall M_{i,j} = 1$ 

* Initially we build start matrix $M^0$ by setting all values to 1.
* Then remove impossible permutations: $0$ for all $M_{i,j}^0$ where $deg(B_j) < deg(A_i)$

