# Canonical Representations
## Introduction to canon
What is canon?
$$G_1 = V_1, E_1$$
$$G_2 = (V_2, E_2)$$
$G_1$ and $G_2$ must be representationally equal.

How to evaluate representational equallity?

Naive permutation algorithm, check all permutaitons for componentwise smallest.
$$\exp(\mathcal{O}(\sqrt{n \log n}))$$

Rotation problem, rotate vertex ids until equal.

Algorithm that finds representaiton given smallest.\
Componentwise comparison, degree wise, node value wise...
## Morgan's algo
**Numbering** eg connectivity based representation enumeration not sufficient.\
**Extended connectivity** (**EC**) includes adjacent node degree.

#### Chemically
We "evaluate" the chemical structure, eg unpack the whole structure as its entire graph (smiles hides hydrogen bonds etc), and generalize:
* Any bond $\rightarrow$ edge.
* Any atom $\rightarrow$ node.

### Step 1
Assign **Extended connectivity** number to each atom.

#### Pseudo alg
1. Calculate connectivity.
2. Use previous connectivity tagged graph to calculate **EC** graph.
3. If it changes, goto step **2**, if not end.

#### Mathematical alg
- $G = (V, E)$\
- $EC_0(v) = deg(v) \forall v \in V$

For $i > 0$:
- $EC_i(v) = \sum_{U:(u,v)\in E} EC_{i-1}(u)$
- Stop when **EC** number only changes.


### Step 2
**Canonical enumeration** Number atoms based on **EC** (remove ambiguities).


#### Alg
1. Initialize state\
$\,label(v) = 0 \,\, \forall v \in V$\
$i = 1$\
($v =$ argmax $EC(v)$) for $v \in V$
2. Label trivially assigns a label to a vertex and increments $i$\
$label(v) \leftarrow i, i \leftarrow i + 1$
3. For current node v:
    - For all nodes $u$ adjacent to $v$ with $label(u)=0$
      - Label $u$ as above with decreasing $EC(u)$ (decreasing meaning largest $EC$ should be labelled first).
      - Ambiguities: atomic number, bond order.\
        (prioritized ordering (C < N < P...) and (SingleBond < DoubleBond...))
4. $v \leftarrow u, u \in V$ with $label(i) = label(v) + 1$

### Problems
* Unique enumeration not provable
* **Step 1** problem: oscillations
  * Oscillating values => Doesn't terminate
* **Step 2** problem: ambiguities
  * Not all ambiguities can be resolved
* Improved variants exist, but error rate is just reduced.

## SMILES Notation
### Goals
1. Uniqueness of the description of the moleculegraph
2. x-friendly/human readability
3. machine-friendly
### SMILES canon
#### a) Attributes
1. \# connections
2. \# non-h 
3. atomic number
4. sign of charge
5. assoc charge
6. \# attended hydrogens

Create string from attributes eg 1 | 01 | 06 | 0 | 0 | 3

Given different atom types:\
methyl carbon: 10106003\
methylene carbon: 20206002

CCCCC can be expressed as:\
10106003-20206002-20206002-20206002-10106003

Then scale down the large string:
1-2-2-2-1

#### b) Iteraiton algorithm

1. Then for each atom, we then add the neighbours as our value:\
(0+2)-(1+2)-(2+2)-(2+1)-(2-0)\
2-3-4-3-2

2. We can reduce this to\
1-2-3-2-1

3. Keep applying the algorithm until nothing changes

#### c) Using primes for uniqueness
If the earlier method fails, a fallback to permutation based algorithm, but we can use primes instead.

Given rank 1:\
1-4-4\
and rank 2:\
2-2-5

The sum of these are equal, eg 9.\
So we can emply a proveable technique, find nearest primes and multiply:\
rank 1:
$$1-4-4 \rightarrow 2-7-7$$
$$2*7*7=98$$
rank 2:
$$2-2-5 \rightarrow 3-3-11$$
$$3*3*11=99$$
## Mckay
### Intro
Only undirected graphs, 2-set edges.
$$\left( \frac{[n]}{2}\right)$$

Binary sequence of edges comparison.
Doesnt use graph theoretical information.

Build graph landmarks based on degree (max first), eg if one vertex of max(degree), we can ONLY have 1 "starting point"\
Verts with unique degree are always the same if isomorphic.
If found unique degree verts we can recursively define isomorphic comparison easier.

In McKay, distinguish verts according to degree.

Artificial asymmetry in automorphism, we just label 3 automorphic verts 1,2,3.

Finally we use automorphisms discovered while exploring the search tree to prune later computations.

### Notation
#### Action
An action is an endofunctor.\
Given:
$$\Gamma = F[T \rightarrow T]$$
$$action = (\Gamma, Set[T])$$
$$\Gamma_{Int} = F[Int \rightarrow Int]$$
$$f: \Gamma_{Int}$$
$$x: Set[Int]$$
$$action = x.map(f)$$

$\Sigma_n$ is the group of all permutations of $[n]$.

$\sigma$ and $\gamma$ are permutation functions such that they each create an element of $\Sigma_n$.

#### Isomorphism given aciton
Two graphs $G$ and $H$ are isomorphic if there exists a permutations $\gamma$ such that $H = G^\gamma$

### Propagation of degree information
Describe classification of verts using ordered partition.

#### Ordered partition
Partition $\pi$ consists of $\{V_1, V_2, ..., V_r\}$ where $V_i$ is a subset of $[n]$.

If $\pi_1$ is finer than $\pi_2$.\
Then $\pi_2$ is coarser than $\pi_1$.

#### Continueation
Verts in different parts have been distinguished from eachother.\
But verts in same part have not.

If two verts $v,w$ are in same part but have different degrees into another part, then distinguish them.

A part is equitable ordered partition if all $deg(v) = deg(w)$ for all 2 combinations of $v,w \in V$.

A part is a coarsest equitable refinement if it is the finest.

#### How McKay does refinement
Input is $G$ graph and ordered partition $\pi$, and returns coarsest equitable refinement of $\pi$.

Shattering means that all degrees of vertices in part $X_k$ is less than all degress of vertices in $X_l$.\
$v \in X_k$ and $w \in X_l$ then $deg(v) < deg(w)$.\
It basically sorts the verts of $V_i$ by their degree to $V_j$ and subdivides into a smaller set eg:
$$(V_1, V_2, ..., V_{i−1}, X_1, X_2, ..., X_t, V_{i+1}, ..., V_r)$$

Only direct neighbours to what is shattered from should be grouped in shattering.
| $\pi$                                                     | $B$            | $V_i$                                                       | $V_j$                                                       |
|-----------------------------------------------------------|----------------|-------------------------------------------------------------|-------------------------------------------------------------|
| $(1 \, 2 \, 3 \, 4 \, 5 \, 6 \, 7 \, 8 \, 9)$             | $\{(1 , 1)\}$ | $(1 \, 2  \,  3  \,  4  \,  5  \,  6  \,  7  \,  8  \,  9)$ | $(1 \, 2  \,  3  \,  4  \,  5  \,  6  \,  7  \,  8  \,  9)$ |
| $(1 \, 3 \, 7 \, 9 \, \| \, 2 \, 4 \, 6 \, 8 \, \| \, 5)$ | $\empty$       |                                                             |                                                             |

#### Search tree
Initially create degree information from unit partition by equitable refinement.

When we have a equitable refinement, we want to introduce artificial distrinctions (automorphism).

Splitting function:
1. Take element $u$.
2. Move $u$ into own part at current position - 1.

Save vertices and equitable ordered partition in search tree node.

All tree leaves only have part of size 1.

#### Pruning through automophisms
Given permutation group, there may exist $\sigma_{\pi}$ and $\sigma_{\tau}$ and $G^{\sigma_{\pi}}$ and $G^{\sigma_{\tau}}$, then $\sigma_{\pi}(\sigma_{\tau})^{-1}$ which is an automorphism og $G$.

Given that at any level if there exists a $\gamma = \sigma_{\pi}(\sigma_{\tau})^{-1}$
to translate a node ($\pi$) to another (and back), then we don't have to check the other node (or its children), because they are just automorphisms of this graph.

#### Improvement 1
Prune earlier (every nontrivial part has size 2).

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

### Refinement procedure
If all terminal nodes in $M$ namely $M'$, have $m'_{i,j} = 0$ then we can safely say $m_{i,j} = 0$.

* $v_{ai}$ be the $i$'th point in $V_a$
* $v_{bj}$ be the $j$'th point in $V_b$
* $\{v_{a1}, ..., v_{ax}, ..,v_{ay}\}$ be points in $G_a$ that are adjacent to $v_a$
* Considering $M'$, it must be that if $v_{ai} = v_{bj}$ then for each $x=1,...,y$ there must exist a point $v_{by}$ that is adjacent to $v_{bj}$ such that $v_{by}$ corrisponds to $v_{ax}$.
* If $v_{by}$ corrisponds to $v_{ax}$ then element in $M'$ that corrisponds to $\{v_{ax},v_{by}\} = 1$.
* Therefore if $v_{ai}$ corrisponds to $v_{bj}$ in any isomorphism under $M$ then for each $x = 1, .., y$ there must be a $1$ in $M$ corrisponding to some $\{v_{ax},v_{by}\}$ such that $v_{by}$ is adjacent to $v_{bj}$.

For any $m_{ij} = 1$ such that the refinement is not satisfied: $m_{ij} = 0$.\
Refinement runs on each $1$ in $M$, until an iteration is unfruitful (nothing changes).

We must leave all $M'$ unchanged.\
$M'$ specifies a $1:1$ mapping $V_a$ into $V_b$:\
The general idea is if two points are adacent in $G_a$ then two corrosponding points in $G_b$ are adjacent.

The refinement also specifies that if any $1$ in $M'$ changes to $0$, then $M'$ doesn't specify an isomorphism.

During refinement, if any row loses all $0$'s, we instantly exist the algorithm with failure.

Provable terminateable, because 1 can only change to 0.

Paralellizable.

Very very very many optimization features.

## Mckay is the same, isomorphism is based on the automorphism groups.
## Generative chemistry, rule matching (get creative).