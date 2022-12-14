# Classification

**Goal**: Given an object (x, y) drawn from D, we want to predict its label y from its attribute values x[A1], ..., x[Ad ].



## Decision Tree

The decision tree method represents a classifier *h* as a tree.

Formally, a decision tree *T* is a binary tree where

- each leaf node carries a class label: yes or no (namely, 1 or −1);
- each internal node *u* has two child nodes, and carries a predicate *P~u~* on an attribute *A~u~*.



### Hunt's Algorithm

The algorithm builds a decision tree *T* in a top-down and greedy manner. At each node *u*, it finds the “best” way to split *S(u)* according to a certain quality metric.

```
algorithm Hunt(S)
/* S is the training set; the function returns the root of a decision tree */
if all the objects in S belong to the same class
 		return a leaf node with the value of this class
if (all the objects in S have the same attribute values) or (|S| is too small) /*to solve overfitting*/
 		return a leaf node whose class value is the majority one in S
find the “best” split attribute A∗ and predicate P∗
S1 ← the set of objects in R satisfying P∗; S2 ← S \ S1
u1 ← Hunt(R1); u2 ← Hunt(R2)
create a root u with left child u1 and right child u2
set Au ← A∗ and Pu ← P∗
return u
```



### Quality of a Split

The GINI index of S is: 
$$
GINI(S)=1-(P^2_y+P^2_n)
$$
Suppose that S has been split into S1 and S2. We define the GINI index of the split as:
$$
GINI_split=\frac{\left|S_1\right|}{\left|S\right|}GINI(S_1)+\frac{\left|S_2\right|}{\left|S\right|}GINI(S_2)
$$
The **smaller** GINIsplit is, the better the split quality.





## Generalization Theorem

- *errS (h)* is often called the empirical error of *h*. 
- *errD(h)* is often called the generalization error of *h*.



**Theorem**: Let *H* be the set of classifiers that can possibly be returned. The following statement holds with probability at least 1 − δ (where 0 < δ ≤ 1): for any *h* ∈ *H*:
$$
err_D(h)\leq err_S(h)+\sqrt{\frac{ln(1/δ)+ln(\left|H\right|)}{2\left|S\right|}}
$$
**Implications**:we should

- look for a decision tree that is both accurate on the training set and small in size; 

- increase the size of S as much as possible.



## The Bayesian Method

Bayesian classification works most effectively when each attribute has a small domain, namely, the attribute has only a small number of possible values. When an attribute has a large domain, we may reduce its domain size through discretization.



Bayes’ Theorem:   


$$
Pr[X|Y]=\frac{Pr[Y|X]\cdot Pr[X]}{Pr[Y]}
$$
Given an instance x, (as in hopt) we predict its label as −1 if and only if
$$
Pr[y=-1|x]\geq Pr[y=1|x]
$$
Applying Bayes’ theorem, we get:
$$
Pr[y=1|x]=\frac{Pr[x|y=1]\cdot Pr[y=1]}{Pr[x]}
$$
Similarly
$$
Pr[y=-1|x]=\frac{Pr[x|y=-1]\cdot Pr[y=-1]}{Pr[x]}
$$
It suffices to decide which of the following is larger: 
$$
Pr[x|y=-1]\cdot Pr[y=-1],or,Pr[x|y=1]\cdot Pr[y=1]
$$
Bayesian classification makes an conditional independence assumption here:
$$
Pr[x|y=1]=\prod_{i=1}^dPr[x[A_i]|y=1]
$$
When this assumption is seriously violated, the accuracy of the method drops significantly.



## Linear Classification

### Perceptron

The algorithm starts with ***w*** = (0, 0, ..., 0) and, then, runs in iterations. 
In each iteration, it looks for a violation point ***p*** ∈ *S*:

- If ***p*** has label 1, ***p*** is a violation point if ***w*** · ***p*** ≤ 0;
- If ***p*** has label −1, ***p*** is a violation point if ***w*** · ***p*** ≥ 0;

If ***p*** exists, the algorithm adjusts ***w*** as follows:

- If ***p*** has label 1, then ***w*** ← ***w*** + ***p***.
- If ***p*** has label −1, then ***w*** ← ***w*** − ***p***.

The algorithm finishes when there are no more violation points.

**Theorem**: Perceptron terminates after at most (*R*/γ)^2^ adjustments of ***w***.



### SVM

In the large margin separation problem, we want to find a separation plane with the maximum margin.

An algorithm solving this problem is called a **support vector machine**.



### Margin Perceptron

A point ***p*** causes a violation in any of the following situations:

- Its distance to the plane ***w*** · ***x*** = 0 is less than γ~guess~/2, regardless of its label.
- ***p*** has label 1 but ***w*** · ***p*** < 0.
- ***p*** has label −1 but ***w*** · ***p*** > 0.

The algorithm starts with ***w*** = 0 and runs in iterations.

In each iteration, it tries to find a violation point ***p*** ∈ *S*. If found, the algorithm adjusts ***w*** as follows:

- if ***p*** has label 1, ***w*** ← ***w*** + ***p***.
- otherwise, ***w*** ← ***w*** − ***p***.

The algorithm finishes where no violation points are found.

**Theorem**: If γ~guess~ ≤ γ~opt~, margin Perceptron terminates in at most 12R^2^ /γ~opt~^2^ iterations and returns a separation plane with margin at least γ~guess~/2.



**An Incremental Algorithm** 

```
*R* ← the maximum distance from the origin to the points in *S*
*γ~guess~* ← *R*
Run margin Perceptron with parameter *γ~guess~* 
		[Self-Termination]
    If the algorithm terminates with a plane ***π***, return ***π*** as the final answer. 
    [Forced-Termination]
    If the algorithm has not terminated after 12R^2^/γ~guess~^2^ iterations: 
   		 Stop the algorithm manually.
       Set γ~guess~ ← γ~guess/2~.
       Repeat Line 3.
```

**Theorem**: Our incremental algorithm returns a separation plane with margin at least γ~opt~/4. Furthermore, it performs O(R^2^/γ~opt~^2^) iterations in total (including all the repeats at Line 3).



### The Kernel Method

The kernel method maps a dataset into another space of higher dimensionality. By applying the method appropriately, we can always guarantee linear separability.

A kernel function K is a function from R^d^ × R^d^ to R with the following property: there is a mapping *ϕ* : R^d^ → R^d‘^  such that, given any two points *p*, *q* ∈ R^d^ , *K(p, q)* equals the dot product of *ϕ(p)* and *ϕ(q)*.



#### Polynomial Kernel

Let *p* and *q* be two points in R^d^ . A polynomial kernel has the form: 
$$
K(p,q)=(p\cdot q +1)^c
$$
for some integer degree *c* ≥ 1.



#### Gaussian Kernel (a.k.a. RBF Kernel)

Let *p* and *q* be two points in R^d^ . A Gaussian kernel has the form: 
$$
K(p,q)=exp(-\frac{dist(p,q)^2}{2σ^2})
$$
for a real value σ > 0 called the bandwidth. Note that dist(p, q) is the Euclidean distance between p and q.

In general, a Gaussian kernel converts d-dimensional space to another space with infinite dimensionality!



#### Finding a Separation Plane in the Converted Space

**Perceptron**

The algorithm starts with ***w*** = (0, 0, ..., 0), and then runs in iterations.

In each iteration, it simply checks whether any point in *ϕ*(*p*) ∈ *P′* violates our requirement according to ***w***. If so, the algorithm adjusts ***w*** as follows:

- If *ϕ(p)* has label 1, then ***w*** ← ***w*** + *ϕ(p)*.
- If *ϕ(p)* has label −1, then ***w*** ← ***w*** − *ϕ(p)*.

The algorithm finishes if the iteration finds all points of *P′* on the right side of the plane.



### Multiclass Perceptron

```
wi ← 0 for all i ∈ [1, k]
while there is a violation point p ∈ S
/* namely, p mis-classified by {w1, ..., wk } */
		ℓ → the real label of p
		z → the predicted label of p
		/* ℓ ̸= z since p is a violation point */
		wℓ ← wℓ + p
		wz ← wz − p
```



**Theorem**: Multiclass Perceptron stops after processing at most R^2^/γ^2^ violation points.



# Clustering

Typically, the similarity between two objects o1, o2 is measured by a distance function dist(o1, o2): the larger dist(o1, o2), the less similar they are.



## Centroid Method

### k-center

```
algorithm k-center (P)
/* this algorithm returns a 2-approximate subset C */
C ← ∅
add to C an arbitrary object in P
for i = 2 to k
	o ← an object in P with the maximum dC (o)
	add o to C
return C
```



### k-means

![image-20221211151457110](./photo/k-means.png)



## Connectivity Method

### Agglomerative clustering (a.k.a. Hierarchical Clustering）

Given a set P of n objects, the agglomerative method works as follows: 

1. At the beginning, each object in P forms a cluster by itself. 
2. Merge the two clusters that are most similar to each other. 
3. Repeat the previous step until only one cluster is left.



### Density-based clustering

DBSCAN

1. Cluster core points. 
2. Assign non-core points.
