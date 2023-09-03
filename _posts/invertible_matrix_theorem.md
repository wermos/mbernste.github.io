

Determinants and invertability
------------------------------

Axiom 2 for defining the determinant states that if two column vectors of a matrix are equal, then the determinant is zero. The intuition behind this axiom is that if a matrix has two equal column-vectors, then the parallelopided is, in a sense, flat and thus, its volume is zero. From this axiom, we derived Theorem 2, which states that any matrix whose column vectors are linearly dependent has a determinant that is zero. 

Now, recall Theorem 4 from our [previous blog post](https://mbernste.github.io/posts/inverse_matrices/) that a matrix is singular (i.e., not invertible) if and only if its columns are linearly dependent. Combining these two theorems, we arrive at the following conclusion: a matrix is invertible if and only if its determinant is non-zero:



Intuitively, this makes sense. Again, recall from our [discussion of invertible and singular matrices](https://mbernste.github.io/posts/inverse_matrices/) that a singular matrix collapses vectors down into a smaller dimensional subspace. What Theorem 10 tells us is that any such matrix that performs this dimensionality reduction has column vectors that form a flat parallelopiped, which has a volume, and hence determinant, of zero!