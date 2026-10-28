# TP2: Standardized PCA, from scratch vs scikit-learn

## Method
The grades of 6 students in 4 subjects were standardized (mean 0, standard
deviation 1, population std). PCA was computed in two ways:
1. From scratch: correlation matrix, eigen-decomposition with NumPy,
   sorting, selection of 2 eigenvectors, projection.
2. With scikit-learn: StandardScaler + PCA(n_components=2).

## Results
| Component | Eigenvalue | Explained variance | Cumulative |
|-----------|-----------|--------------------|------------|
| PC1       | 3.7397    | 93.49%             | 93.49%     |
| PC2       | 0.2402    | 6.01%              | 99.50%     |

The two implementations give identical coordinates, up to a possible sign
change on each component, and identical explained variance ratios.

## Differences observed
- **Sign changes:** an eigenvector v and its opposite -v are both valid
  solutions of the eigenvalue problem. NumPy (eigh) and scikit-learn (SVD)
  choose the sign differently. The result is a mirror image of the plot
  along one axis. The distances between students, the variance explained
  and the interpretation do not change.
- **Eigenvalues:** scikit-learn's explained_variance_ uses the n-1 divisor,
  while the correlation matrix of the scratch version uses a consistent
  normalization. The eigenvalues therefore differ by a constant factor
  n/(n-1) = 6/5 = 1.2. The explained variance ratios are identical because
  the factor cancels out.

## Interpretation
- **PC1 (93.49%)** has loadings of almost equal size and the same sign for
  all four subjects (about 0.5 each). It is a general performance axis.
  S4 and S2 are at one end (best students), S3 is at the other end (weakest),
  S5 and S1 are in between.
- **PC2 (6.01%)** opposes Statistics (loading +0.78) to Programming (-0.59).
  Students high on this axis (S6, S5) are relatively stronger in Statistics
  than in Programming. S1 is the opposite case.
- **Variance retained by the first two components: 99.50%.** Reducing the
  data from 4 dimensions to 2 loses only 0.5% of the information. Even PC1
  alone retains 93.49%, because the four subjects are strongly correlated
  (correlations between 0.77 and 0.99).

## Conclusion
The from-scratch implementation reproduces scikit-learn's results exactly,
apart from the arbitrary sign of the components and the constant factor on
the eigenvalues. A 2D representation summarizes the grades almost completely.