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
1. if all the objects in S belong to the same class
2. 		return a leaf node with the value of this class
3. if (all the objects in S have the same attribute values) or (|S| is too small) /*to solve overfitting*/
4. 		return a leaf node whose class value is the majority one in S
5. find the “best” split attribute A∗ and predicate P∗
6. S1 ← the set of objects in R satisfying P∗; S2 ← S \ S1
7. u1 ← Hunt(R1); u2 ← Hunt(R2)
8. create a root u with left child u1 and right child u2
9. set Au ← A∗ and Pu ← P∗
10. return u
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



