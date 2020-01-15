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
#### a) Initial graph state
1. \# connections
2. \# non-h 
3. atomic number
4. sign of charge
5. assoc charge
6. \# attended hydrogens

#### c) Simple extened connectivity
