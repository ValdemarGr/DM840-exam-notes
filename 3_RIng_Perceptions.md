# Ring Perceptions
## MCB (minimal cycle basis)
Weighted edges in graph.

Cycle basis, set of cycles that are the basis for all cycles in a graph, eg all cycles can be built using these cycles.

Minimum cycle basis is the cycle basis where the sum of weights is minimum.
## Hansers algo
Find rings via compressing redundant edges, eg:
$$if \,\, deg(x) = 2$$
$$z,y \in neighbourVerts(x)$$
$$remove(x)$$
$$addEdge(z - y)$$
## Horton's
1. Choose 2 verts that have edge $e = \{u, w\}$ and another vert $v \in V$
2. $C = SP(u, v) + \{u, w\} + SP(w,v)$
3. if $C$ is simple then add $C$ to $H$
4. Sort $H$
5. $B = \empty$
6. while $(|B| < |E| - |V| + c(G))$, $i = 1$
   1. if $B \cup \{C_i\}$ is linear independant, then $B = B \cup \{C_i\}$
## De pina's uses magic
$$S_j:=\{e_j\}$$
$C_k :=$ shortest cycle in $G$ with $<C_k,S_k>=1$\
if $<C_k,S_j> = 1$ for some $j > k$ then $S_j := S_j \, combine \, S_k$
