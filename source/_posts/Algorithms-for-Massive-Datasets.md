---
title: Algorithms for Massive Datasets
date: 2024-08-15 15:13:00
tags: [algorithm]
categories: [algorithm]
mathjax: true
---
## 1.Hashing(Integer Data Structures I)
###  Hashing
###  Perfect Hashing
FKS-scheme

- lookeup: O(1)$$\sqrt{N}$$
- Space: O(n)
- feature: 
  - 2-level
  - collision-free(the second level uses $n^2$)
   
###  String Hashing
- Karp-Rabin fingerprint
  - rolling property
  - If fingerprints match, verify using brute-force comparison. Return “yes!” if we match.

## 2.Predecessor(Integer Data Structures II)
[video from MIT 6.861](https://www.youtube.com/watch?v=u-HHY1ylhHY&t=4032s)
### Predecessor Problem
### van Emde Boas
T(u) = T($\sqrt{u}$) + 1  = O(loglogu)  
O(logw) = O(loglogu)  
O(u) space
- combine perfect hashing we can reduce space to O(n)
### x-Fast and y-Fast Tries
x-Fast
- don't store 0s
- For each level store a dictionary of prefixes of keys
- Binary search over levels to find longest matching prefix with x
- Space: O(nlogu)

y-Fast Tries  
- y-Fast Tries = x-Fast Tries + indirection
- partition S into n/logu groups
  - so space of top part is O(n/logu * logu) = O(n) 
  - space of down part use Binary search tree is O(n)
  - so total space is O(n)
  - Time of top part is O(loglogu)
  - Time of down part is O(loglogu)
  - so total time is O(loglogu)
- Space: O(n)
- Time: O(logw) = O(loglogu)  


## 3.Range Reporting(Geometry)
### 1-D  Range Reporting
  - sort and binary search
  - space: O(n)
  - time: O(log+occ)
  - preprocess: O(nlogn)
### 2-D  Range Reporting
####  Range Trees
- x uses binary search
- then y just follow x to form the same tree  

2D range tree reporting \
using bridges(fractional cascading) \
[Range Tree MIT 6.861](https://ocw.mit.edu/courses/6-851-advanced-data-structures-spring-2012/resources/mit6_851s12_l3/)

range tree explain  
[video from youtube](https://www.youtube.com/watch?v=5a7EYVulN-w&t=491s)  
fractional cascading  
- Time O(logn+occ)
- Space O(nlogn)

#### Predecessor in Nested Sets

#### kD Trees
- divide the set into two parts by x
- then divide each subset into two parts by y
- recursively do two procedures above
- Space: O(n)
- Time: O($\sqrt{n}$)
- Preprocess: O(nlogn)

## 4.LCA and RMQ(Integer Data Structures III)
### Range Minimum Query
[Lowest Common Ancestor And Level Ancestor](https://www.youtube.com/watch?v=0rCFkuQS968)
- Save the result for all intervals of length a power of 2
- Time: O(1)
- Space: O(nlogn)
### $\pm 1$RMQ
- 2-level solution
- divide the array into blocks of size 1/2 logn
- 2-level data structure:
  - Sparse table on blocks
    - space O((n/logn)*log(n/logn)) = O(n)
    - time : O(1)
  - Tabulation inside blocks.
    - Describe block by sequence of +1s and -1s 
    - length of sequence is 1/2 * logn -1
    - so #sequence = $2^{(1/2)*logn -1}$ smaller than $\sqrt{n}$
    - size of table is O(${log}^2n$)
    - space is O($\sqrt{n}$*${log}^2n$) + O(n/logn)(number) =O(n)  
  - in total space is O(n), time is O(1)
### Lowest Common Ancestor
- E: Euler tour representation. preorder walk, write id of node when met.
- A: depth of node node in E[i].
- R: first occurrence in E of node with id i
- LCA(i, j) = E[$RMQ_A$(R[i], R[j])]
- RMQ -> LCA -> $\pm 1$RMQ  
- RMQ and LCA can be solved in O(n) space and O(1) query time.

## 5.Level Ancestor(Trees)
### Level Ancestor
### Path Decompositions
### Tree Decompositions
[Lowest Common Ancestor And Level Ancestor](https://www.youtube.com/watch?v=0rCFkuQS968)
[LCA and Level Ancestor](https://ocw.mit.edu/courses/6-851-advanced-data-structures-spring-2012/resources/mit6_851s12_lec15/)
#### Top-down Decomposition
- jump pointer (make sure at least jump up k/2 nodes)
- ladder decomposition (so the jumped ancestor is on a ladder can be used to find $k_{th}$ ancestor)
- tree trimming(node with >= 1/4 * logn descendants)
- Space
  - top part is O(n + n/logn * logn) = O(n) (the second logn is from ladder)
  - down part uses balanced parentheses representation
    - tree encoding: the tree nodes is < 1/4 logn
    - so the tree encoding is 2* 1/4 logn = 1/2 *logn bits
    - node v and ancestor k encoding is 2 * log(1/4logn) < 2 loglogn bits
    - so all probabilities are < $2^{1/2 logn + 2 loglogn}$ = O(n)
  - if we can find the node in the bottom tree, just use bottom tree. if we need to find ancestor in the top part, just O(1) by ladder and jump pointer.

## 6.Suffix Tree(Strings I)
### Dictionaries
### Tries
### Suffix trees
[note from MIT 6.861](https://ocw.mit.edu/courses/6-851-advanced-data-structures-spring-2012/resources/mit6_851s12_lec16/)]
- space : O(n)
- time: O(m) for searching for a string of length m
- time: O(m + occ) for prefix search, where occ = #occurrences
- the occ could be large

## 7.Suffix Sorting(Strings II)
### Radix Sorting
### Suffix Array
### Suffix Sorting
DC3 Algorithm
- Sort Sample Suffixes
  - Sample all suffixes starting at positions i = 1 mod 3 and i = 2 mod 3  O(n)
  - Recursively sort sample suffixes  O(2/3 n)
- Sort non-sample suffixes
  - Sort the remaining suffixes (starting at positions i = 0 mod 3)  O(n)
- Merge  O(n)
- total time: O(n+2/3n+n+n) = O(n)
- We can suffix sort a string of length n over alphabet Σ of size n in time O(n).
- We can suffix sort a string of length n over alphabet Σ O(sort(n, |Σ|)) time.

## 8.Compression(Compression)
### Lempel-Ziv
#### Lempel-Ziv 77
- using suffix tree
- Parse from left-to-right into phrases.
- Select longest phrase seen before + a single character.
- Encode phrases (previous phrase, character) or single phrase
- time: O(n)
#### Lempel-Ziv 78
- Parse from left-to-right into phrases.
- Select longest phrase seen before + a single character.
- Encode phrases (previous phrase, character) or single phrase
- time: O(n)
### Re-Pair
- Recursive-pairing compression [Larsson and Moffat 2000].
  - Start with string S.
  - Replace a most frequent pair ab by new character Xi. Output rule $X_i$ ➞ ab.(find the most frequent pair by suffix tree?)
  - Repeat until we have a single pair.
### Grammars
- Grammar compression. Encode string S as an grammar G that generates S.

## 9.Approximation Algorithm 1
### Acyclic Graph. Given a directed graph G=(V,E), pick a maximum cardinality set of edges from E such that the resulting graph is acyclic.
- 1/2-Approximation Algorithm
- select one directed path between two directed paths
### Min Max Matching
- 2-Algorithm Algorithm
[Matching](https://www.youtube.com/watch?v=wWk1EmV52Ks&t=86s)
### LPT(scheduling) longest processing time
[LPT](https://www.yatming.net/2018/01/03/Machine-Scheduling-Machine-Learning-in-Engineering/)
- Assume t1 ≥ …. ≥ tn.
- Assume wlog that smallest job finishes last.
- If tn ≤ T*/3 then T ≤ 4/3 T*.
- If tn > T*/3 then each machine can process at most 2 jobs in OPT.
### K-center
- assume r* is the OPT. We can get 2 * r*. It is 2-Approximation Algorithm.
- Because radius of OPT cluster is r* and every node in this cluster is no more far away from 2 * r*. We cannot ensure we always pick the right node in each cluster. But we can ensure we choose a node in OPT cluster so every node in this cluster is no more far away from 2 * r*.
[link](http://staff.ustc.edu.cn/~huding/data_pdf/聚类.pdf)]

## 10.Approximation Algorithm 2
### TSP
- Christofides’ algorithm
  - find MST T(minimum spinning tree by Prime's algorithm)
  - find odd degree O vertices in MST T
  - compute minimum perfect matching M on O
  - Construct Euler tour 𝞃
  - Shortcut such that each vertex only visited once (𝞃’) (in a triangle, a + b > c)
- length(𝞃’) ≤ length(𝞃) = cost(T) + cost(M) ≤ OPT + cost(M).
- cost(M) ≤ OPT/2.
  - $OPT_o$ = OPT restricted to O.
  - $OPT_o$ ≤ OPT.
  - can partition $OPT_o$ into two perfect matchings $O_1$ and $O_2$.
  - cost(M) ≤ min(cost($O_1$), cost($O_2$)) ≤ OPT/2.
- length(𝞃’) ≤ length(𝞃) = cost(T) + cost(M) ≤ OPT + OPT/2 = 3/2 OPT.
- Christofides’ algorithm is a 3/2-approximation algorithm for TSP.  
[link](https://www.youtube.com/watch?v=GiDsjIBOVoA&t=726s)
[Hungarian Matching Algorithm](https://xujinzh.github.io/2021/11/19/hungarian-matching-algorithm/index.html)
### Set Cover
- greedy algorithm
- $\frac{w_i}{S_i \cap R}$ (this is dynamic)  
[set cover](https://www.cnblogs.com/xlucidator/p/17294349.html)
- OPT = 1+x(x is slight bigger than 0)
  - then cost = $H_n$ ($H_n$ is logn)
- in fact, cost = $H_n$ * OPT
- so this is a $H_n$ approximation algorithm.
## 11.External Memory 1
### I/O Model
### Scanning
- Scanning. Given an array A of N values (stored in N/B blocks), process all values from left-to-right.
- I/Os. O(N/B).
### Sorting
- Goal. Sorting in O(N/B logM/B (N/B)) I/Os.
- Solution in 3 steps.
  - Base case.
    - Partition N elements into N/M arrays of size M.
    - Load each into memory and sort.
    - I/Os. O(N/B)
  - External multi-way merge.
    - Multiway merge algorithm.
      - Input is N elements in M/B arrays.
      - Load M/B first blocks into memory and sort.
      - Output B smallest elements.
      - Load more blocks into memory if needed.
      - Repeat.
    - I/Os. O(N/B).
  - External merge sort.
    - Partition N elements into N/M arrays of size M. Load each into memory and sort.
    - Apply M/B way external multiway merge until left with single sorted array.
    - I/Os.
      - Sort N/M arrays: O(N/B) I/Os
      - Height of tree O(logM/B(N/M))
      - Cost per level: O(N/B) I/Os.
  - Total I/Os: O($\frac{N}{B}*log_{\frac{M}{B}}\frac{N}{M}$)=O($\frac{N}{B}*log_{\frac{M}{B}}\frac{N}{B}$)

### Searching
- B-tree
  - I/Os. O($log_B{N}$)

## 12.External Memory 2
### Access Path Traversal
- I/O intuition.
  - Flush moves Θ(B) message together in O(1) I/Os.
  - A message moves at most P times.
  - ⇒ O(P/B + 1) = O(P/B) amortized I/Os.
### Bε-trees
- B tree with buffer some updates at each node
- ε ∈ (0, 1] is a parameter.
  - Solution in 2 steps.
  - Focus on $\sqrt{B}$-tree ( = 1/2).
  - Searching in O($log_B$ N) I/Os.
  - Updates in O(($log_B$ N)/$\sqrt{B}$ ) amortized.
  - Generalize to any ε .

### Range Tree
2D range tree reporting \
using bridges(fractional cascading) \
[Range Tree MIT 6.861](https://ocw.mit.edu/courses/6-851-advanced-data-structures-spring-2012/resources/mit6_851s12_l3/)

[video from youtube](https://www.youtube.com/watch?v=5a7EYVulN-w)


### Lowest Common Ancestor And Level Ancestor
LCA => RMQ => $\pm$RMQ
[Lowest Common Ancestor And Level Ancestor](https://www.youtube.com/watch?v=0rCFkuQS968)


