description: LinearOperator acting like a [batch] of permutation matrices.

<div itemscope itemtype="http://developers.google.com/ReferenceObject">
<meta itemprop="name" content="tf.linalg.LinearOperatorPermutation" />
<meta itemprop="path" content="Stable" />
<meta itemprop="property" content="__init__"/>
<meta itemprop="property" content="__matmul__"/>
<meta itemprop="property" content="add_to_tensor"/>
<meta itemprop="property" content="adjoint"/>
<meta itemprop="property" content="assert_non_singular"/>
<meta itemprop="property" content="assert_positive_definite"/>
<meta itemprop="property" content="assert_self_adjoint"/>
<meta itemprop="property" content="batch_shape_tensor"/>
<meta itemprop="property" content="cholesky"/>
<meta itemprop="property" content="cond"/>
<meta itemprop="property" content="determinant"/>
<meta itemprop="property" content="diag_part"/>
<meta itemprop="property" content="domain_dimension_tensor"/>
<meta itemprop="property" content="eigvals"/>
<meta itemprop="property" content="inverse"/>
<meta itemprop="property" content="log_abs_determinant"/>
<meta itemprop="property" content="matmul"/>
<meta itemprop="property" content="matvec"/>
<meta itemprop="property" content="range_dimension_tensor"/>
<meta itemprop="property" content="shape_tensor"/>
<meta itemprop="property" content="solve"/>
<meta itemprop="property" content="solvevec"/>
<meta itemprop="property" content="tensor_rank_tensor"/>
<meta itemprop="property" content="to_dense"/>
<meta itemprop="property" content="trace"/>
</div>

# tf.linalg.LinearOperatorPermutation

<!-- Insert buttons and diff -->

<table class="tfo-notebook-buttons tfo-api nocontent" align="left">
<td>
  <a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator_permutation.py#L39-L262">
    <img src="https://www.tensorflow.org/images/GitHub-Mark-32px.png" />
    View source on GitHub
  </a>
</td>
</table>



`LinearOperator` acting like a [batch] of permutation matrices.

Inherits From: [`LinearOperator`](../../tf/linalg/LinearOperator.md)

<section class="expandable">
  <h4 class="showalways">View aliases</h4>
  <p>
<b>Compat aliases for migration</b>
<p>See
<a href="https://www.tensorflow.org/guide/migrate">Migration guide</a> for
more details.</p>
<p>`tf.compat.v1.linalg.LinearOperatorPermutation`</p>
</p>
</section>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>tf.linalg.LinearOperatorPermutation(
    perm, dtype=tf.dtypes.float32, is_non_singular=None, is_self_adjoint=None,
    is_positive_definite=None, is_square=None, name='LinearOperatorPermutation'
)
</code></pre>



<!-- Placeholder for "Used in" -->

This operator acts like a [batch] of permutations with shape
`[B1,...,Bb, N, N]` for some `b >= 0`.  The first `b` indices index a
batch member.  For every batch index `(i1,...,ib)`, `A[i1,...,ib, : :]` is
an `N x N` matrix.  This matrix `A` is not materialized, but for
purposes of broadcasting this shape will be relevant.

`LinearOperatorPermutation` is initialized with a (batch) vector.

A permutation, is defined by an integer vector `v` whose values are unique
and are in the range `[0, ... n]`. Applying the permutation on an input
matrix has the folllowing meaning: the value of `v` at index `i`
says to move the `v[i]`-th row of the input matrix to the `i`-th row.
Because all values are unique, this will result in a permutation of the
rows the input matrix. Note, that the permutation vector `v` has the same
semantics as <a href="../../tf/transpose.md"><code>tf.transpose</code></a>.

```python
# Create a 3 x 3 permutation matrix that swaps the last two columns.
vec = [0, 2, 1]
operator = LinearOperatorPermutation(vec)

operator.to_dense()
==> [[1., 0., 0.]
     [0., 0., 1.]
     [0., 1., 0.]]

operator.shape
==> [3, 3]

# This will be zero.
operator.log_abs_determinant()
==> scalar Tensor

x = ... Shape [3, 4] Tensor
operator.matmul(x)
==> Shape [3, 4] Tensor
```

#### Shape compatibility

This operator acts on [batch] matrix with compatible shape.
`x` is a batch matrix with compatible shape for `matmul` and `solve` if

```
operator.shape = [B1,...,Bb] + [N, N],  with b >= 0
x.shape =   [C1,...,Cc] + [N, R],
and [C1,...,Cc] broadcasts with [B1,...,Bb] to [D1,...,Dd]
```

#### Matrix property hints

This `LinearOperator` is initialized with boolean flags of the form `is_X`,
for `X = non_singular, self_adjoint, positive_definite, square`.
These have the following meaning:

* If `is_X == True`, callers should expect the operator to have the
  property `X`.  This is a promise that should be fulfilled, but is *not* a
  runtime assert.  For example, finite floating point precision may result
  in these promises being violated.
* If `is_X == False`, callers should expect the operator to not have `X`.
* If `is_X == None` (the default), callers should have no expectation either
  way.

<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2"><h2 class="add-link">Args</h2></th></tr>

<tr>
<td>
`perm`
</td>
<td>
Shape `[B1,...,Bb, N]` Integer `Tensor` with `b >= 0`
`N >= 0`. An integer vector that represents the permutation to apply.
Note that this argument is same as <a href="../../tf/transpose.md"><code>tf.transpose</code></a>. However, this
permutation is applied on the rows, while the permutation in
<a href="../../tf/transpose.md"><code>tf.transpose</code></a> is applied on the dimensions of the `Tensor`. `perm`
is required to have unique entries from `{0, 1, ... N-1}`.
</td>
</tr><tr>
<td>
`dtype`
</td>
<td>
The `dtype` of arguments to this operator. Default: `float32`.
Allowed dtypes: `float16`, `float32`, `float64`, `complex64`,
`complex128`.
</td>
</tr><tr>
<td>
`is_non_singular`
</td>
<td>
Expect that this operator is non-singular.
</td>
</tr><tr>
<td>
`is_self_adjoint`
</td>
<td>
Expect that this operator is equal to its hermitian
transpose.  This is autoset to true
</td>
</tr><tr>
<td>
`is_positive_definite`
</td>
<td>
Expect that this operator is positive definite,
meaning the quadratic form `x^H A x` has positive real part for all
nonzero `x`.  Note that we do not require the operator to be
self-adjoint to be positive-definite.  See:
https://en.wikipedia.org/wiki/Positive-definite_matrix#Extension_for_non-symmetric_matrices
This is autoset to false.
</td>
</tr><tr>
<td>
`is_square`
</td>
<td>
Expect that this operator acts like square [batch] matrices.
This is autoset to true.
</td>
</tr><tr>
<td>
`name`
</td>
<td>
A name for this `LinearOperator`.
</td>
</tr>
</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2"><h2 class="add-link">Raises</h2></th></tr>

<tr>
<td>
`ValueError`
</td>
<td>
`is_self_adjoint` is not `True`, `is_positive_definite` is
not `False` or `is_square` is not `True`.
</td>
</tr>
</table>





<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2"><h2 class="add-link">Attributes</h2></th></tr>

<tr>
<td>
`H`
</td>
<td>
Returns the adjoint of the current `LinearOperator`.

Given `A` representing this `LinearOperator`, return `A*`.
Note that calling `self.adjoint()` and `self.H` are equivalent.
</td>
</tr><tr>
<td>
`batch_shape`
</td>
<td>
`TensorShape` of batch dimensions of this `LinearOperator`.

If this operator acts like the batch matrix `A` with
`A.shape = [B1,...,Bb, M, N]`, then this returns
`TensorShape([B1,...,Bb])`, equivalent to `A.shape[:-2]`
</td>
</tr><tr>
<td>
`domain_dimension`
</td>
<td>
Dimension (in the sense of vector spaces) of the domain of this operator.

If this operator acts like the batch matrix `A` with
`A.shape = [B1,...,Bb, M, N]`, then this returns `N`.
</td>
</tr><tr>
<td>
`dtype`
</td>
<td>
The `DType` of `Tensor`s handled by this `LinearOperator`.
</td>
</tr><tr>
<td>
`graph_parents`
</td>
<td>
List of graph dependencies of this `LinearOperator`. (deprecated)

Warning: THIS FUNCTION IS DEPRECATED. It will be removed in a future version.
Instructions for updating:
Do not call `graph_parents`.
</td>
</tr><tr>
<td>
`is_non_singular`
</td>
<td>

</td>
</tr><tr>
<td>
`is_positive_definite`
</td>
<td>

</td>
</tr><tr>
<td>
`is_self_adjoint`
</td>
<td>

</td>
</tr><tr>
<td>
`is_square`
</td>
<td>
Return `True/False` depending on if this operator is square.
</td>
</tr><tr>
<td>
`parameters`
</td>
<td>
Dictionary of parameters used to instantiate this `LinearOperator`.
</td>
</tr><tr>
<td>
`perm`
</td>
<td>

</td>
</tr><tr>
<td>
`range_dimension`
</td>
<td>
Dimension (in the sense of vector spaces) of the range of this operator.

If this operator acts like the batch matrix `A` with
`A.shape = [B1,...,Bb, M, N]`, then this returns `M`.
</td>
</tr><tr>
<td>
`shape`
</td>
<td>
`TensorShape` of this `LinearOperator`.

If this operator acts like the batch matrix `A` with
`A.shape = [B1,...,Bb, M, N]`, then this returns
`TensorShape([B1,...,Bb, M, N])`, equivalent to `A.shape`.
</td>
</tr><tr>
<td>
`tensor_rank`
</td>
<td>
Rank (in the sense of tensors) of matrix corresponding to this operator.

If this operator acts like the batch matrix `A` with
`A.shape = [B1,...,Bb, M, N]`, then this returns `b + 2`.
</td>
</tr>
</table>



## Methods

<h3 id="add_to_tensor"><code>add_to_tensor</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator.py#L1077-L1090">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>add_to_tensor(
    x, name='add_to_tensor'
)
</code></pre>

Add matrix represented by this operator to `x`.  Equivalent to `A + x`.


<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Args</th></tr>

<tr>
<td>
`x`
</td>
<td>
`Tensor` with same `dtype` and shape broadcastable to `self.shape`.
</td>
</tr><tr>
<td>
`name`
</td>
<td>
A name to give this `Op`.
</td>
</tr>
</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Returns</th></tr>
<tr class="alt">
<td colspan="2">
A `Tensor` with broadcast shape and same `dtype` as `self`.
</td>
</tr>

</table>



<h3 id="adjoint"><code>adjoint</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator.py#L933-L948">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>adjoint(
    name='adjoint'
)
</code></pre>

Returns the adjoint of the current `LinearOperator`.

Given `A` representing this `LinearOperator`, return `A*`.
Note that calling `self.adjoint()` and `self.H` are equivalent.

<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Args</th></tr>

<tr>
<td>
`name`
</td>
<td>
A name for this `Op`.
</td>
</tr>
</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Returns</th></tr>
<tr class="alt">
<td colspan="2">
`LinearOperator` which represents the adjoint of this `LinearOperator`.
</td>
</tr>

</table>



<h3 id="assert_non_singular"><code>assert_non_singular</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator.py#L541-L559">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>assert_non_singular(
    name='assert_non_singular'
)
</code></pre>

Returns an `Op` that asserts this operator is non singular.

This operator is considered non-singular if

```
ConditionNumber < max{100, range_dimension, domain_dimension} * eps,
eps := np.finfo(self.dtype.as_numpy_dtype).eps
```

<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Args</th></tr>

<tr>
<td>
`name`
</td>
<td>
A string name to prepend to created ops.
</td>
</tr>
</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Returns</th></tr>
<tr class="alt">
<td colspan="2">
An `Assert` `Op`, that, when run, will raise an `InvalidArgumentError` if
the operator is singular.
</td>
</tr>

</table>



<h3 id="assert_positive_definite"><code>assert_positive_definite</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator.py#L577-L592">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>assert_positive_definite(
    name='assert_positive_definite'
)
</code></pre>

Returns an `Op` that asserts this operator is positive definite.

Here, positive definite means that the quadratic form `x^H A x` has positive
real part for all nonzero `x`.  Note that we do not require the operator to
be self-adjoint to be positive definite.

<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Args</th></tr>

<tr>
<td>
`name`
</td>
<td>
A name to give this `Op`.
</td>
</tr>
</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Returns</th></tr>
<tr class="alt">
<td colspan="2">
An `Assert` `Op`, that, when run, will raise an `InvalidArgumentError` if
the operator is not positive definite.
</td>
</tr>

</table>



<h3 id="assert_self_adjoint"><code>assert_self_adjoint</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator.py#L604-L618">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>assert_self_adjoint(
    name='assert_self_adjoint'
)
</code></pre>

Returns an `Op` that asserts this operator is self-adjoint.

Here we check that this operator is *exactly* equal to its hermitian
transpose.

<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Args</th></tr>

<tr>
<td>
`name`
</td>
<td>
A string name to prepend to created ops.
</td>
</tr>
</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Returns</th></tr>
<tr class="alt">
<td colspan="2">
An `Assert` `Op`, that, when run, will raise an `InvalidArgumentError` if
the operator is not self-adjoint.
</td>
</tr>

</table>



<h3 id="batch_shape_tensor"><code>batch_shape_tensor</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator.py#L355-L370">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>batch_shape_tensor(
    name='batch_shape_tensor'
)
</code></pre>

Shape of batch dimensions of this operator, determined at runtime.

If this operator acts like the batch matrix `A` with
`A.shape = [B1,...,Bb, M, N]`, then this returns a `Tensor` holding
`[B1,...,Bb]`.

<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Args</th></tr>

<tr>
<td>
`name`
</td>
<td>
A name for this `Op`.
</td>
</tr>
</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Returns</th></tr>
<tr class="alt">
<td colspan="2">
`int32` `Tensor`
</td>
</tr>

</table>



<h3 id="cholesky"><code>cholesky</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator.py#L978-L1001">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>cholesky(
    name='cholesky'
)
</code></pre>

Returns a Cholesky factor as a `LinearOperator`.

Given `A` representing this `LinearOperator`, if `A` is positive definite
self-adjoint, return `L`, where `A = L L^T`, i.e. the cholesky
decomposition.

<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Args</th></tr>

<tr>
<td>
`name`
</td>
<td>
A name for this `Op`.
</td>
</tr>
</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Returns</th></tr>
<tr class="alt">
<td colspan="2">
`LinearOperator` which represents the lower triangular matrix
in the Cholesky decomposition.
</td>
</tr>

</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Raises</th></tr>

<tr>
<td>
`ValueError`
</td>
<td>
When the `LinearOperator` is not hinted to be positive
definite and self adjoint.
</td>
</tr>
</table>



<h3 id="cond"><code>cond</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator.py#L1127-L1137">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>cond(
    name='cond'
)
</code></pre>

Returns the condition number of this linear operator.


<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Args</th></tr>

<tr>
<td>
`name`
</td>
<td>
A name for this `Op`.
</td>
</tr>
</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Returns</th></tr>
<tr class="alt">
<td colspan="2">
Shape `[B1,...,Bb]` `Tensor` of same `dtype` as `self`.
</td>
</tr>

</table>



<h3 id="determinant"><code>determinant</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator.py#L737-L754">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>determinant(
    name='det'
)
</code></pre>

Determinant for every batch member.


<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Args</th></tr>

<tr>
<td>
`name`
</td>
<td>
A name for this `Op`.
</td>
</tr>
</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Returns</th></tr>
<tr class="alt">
<td colspan="2">
`Tensor` with shape `self.batch_shape` and same `dtype` as `self`.
</td>
</tr>

</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Raises</th></tr>

<tr>
<td>
`NotImplementedError`
</td>
<td>
If `self.is_square` is `False`.
</td>
</tr>
</table>



<h3 id="diag_part"><code>diag_part</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator.py#L1028-L1054">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>diag_part(
    name='diag_part'
)
</code></pre>

Efficiently get the [batch] diagonal part of this operator.

If this operator has shape `[B1,...,Bb, M, N]`, this returns a
`Tensor` `diagonal`, of shape `[B1,...,Bb, min(M, N)]`, where
`diagonal[b1,...,bb, i] = self.to_dense()[b1,...,bb, i, i]`.

```
my_operator = LinearOperatorDiag([1., 2.])

# Efficiently get the diagonal
my_operator.diag_part()
==> [1., 2.]

# Equivalent, but inefficient method
tf.linalg.diag_part(my_operator.to_dense())
==> [1., 2.]
```

<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Args</th></tr>

<tr>
<td>
`name`
</td>
<td>
A name for this `Op`.
</td>
</tr>
</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Returns</th></tr>

<tr>
<td>
`diag_part`
</td>
<td>
A `Tensor` of same `dtype` as self.
</td>
</tr>
</table>



<h3 id="domain_dimension_tensor"><code>domain_dimension_tensor</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator.py#L440-L456">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>domain_dimension_tensor(
    name='domain_dimension_tensor'
)
</code></pre>

Dimension (in the sense of vector spaces) of the domain of this operator.

Determined at runtime.

If this operator acts like the batch matrix `A` with
`A.shape = [B1,...,Bb, M, N]`, then this returns `N`.

<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Args</th></tr>

<tr>
<td>
`name`
</td>
<td>
A name for this `Op`.
</td>
</tr>
</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Returns</th></tr>
<tr class="alt">
<td colspan="2">
`int32` `Tensor`
</td>
</tr>

</table>



<h3 id="eigvals"><code>eigvals</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator.py#L1095-L1112">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>eigvals(
    name='eigvals'
)
</code></pre>

Returns the eigenvalues of this linear operator.

If the operator is marked as self-adjoint (via `is_self_adjoint`)
this computation can be more efficient.

Note: This currently only supports self-adjoint operators.

<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Args</th></tr>

<tr>
<td>
`name`
</td>
<td>
A name for this `Op`.
</td>
</tr>
</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Returns</th></tr>
<tr class="alt">
<td colspan="2">
Shape `[B1,...,Bb, N]` `Tensor` of same `dtype` as `self`.
</td>
</tr>

</table>



<h3 id="inverse"><code>inverse</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator.py#L953-L976">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>inverse(
    name='inverse'
)
</code></pre>

Returns the Inverse of this `LinearOperator`.

Given `A` representing this `LinearOperator`, return a `LinearOperator`
representing `A^-1`.

<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Args</th></tr>

<tr>
<td>
`name`
</td>
<td>
A name scope to use for ops added by this method.
</td>
</tr>
</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Returns</th></tr>
<tr class="alt">
<td colspan="2">
`LinearOperator` representing inverse of this matrix.
</td>
</tr>

</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Raises</th></tr>

<tr>
<td>
`ValueError`
</td>
<td>
When the `LinearOperator` is not hinted to be `non_singular`.
</td>
</tr>
</table>



<h3 id="log_abs_determinant"><code>log_abs_determinant</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator.py#L766-L783">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>log_abs_determinant(
    name='log_abs_det'
)
</code></pre>

Log absolute value of determinant for every batch member.


<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Args</th></tr>

<tr>
<td>
`name`
</td>
<td>
A name for this `Op`.
</td>
</tr>
</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Returns</th></tr>
<tr class="alt">
<td colspan="2">
`Tensor` with shape `self.batch_shape` and same `dtype` as `self`.
</td>
</tr>

</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Raises</th></tr>

<tr>
<td>
`NotImplementedError`
</td>
<td>
If `self.is_square` is `False`.
</td>
</tr>
</table>



<h3 id="matmul"><code>matmul</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator.py#L631-L684">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>matmul(
    x, adjoint=(False), adjoint_arg=(False), name='matmul'
)
</code></pre>

Transform [batch] matrix `x` with left multiplication:  `x --> Ax`.

```python
# Make an operator acting like batch matrix A.  Assume A.shape = [..., M, N]
operator = LinearOperator(...)
operator.shape = [..., M, N]

X = ... # shape [..., N, R], batch matrix, R > 0.

Y = operator.matmul(X)
Y.shape
==> [..., M, R]

Y[..., :, r] = sum_j A[..., :, j] X[j, r]
```

<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Args</th></tr>

<tr>
<td>
`x`
</td>
<td>
`LinearOperator` or `Tensor` with compatible shape and same `dtype` as
`self`. See class docstring for definition of compatibility.
</td>
</tr><tr>
<td>
`adjoint`
</td>
<td>
Python `bool`.  If `True`, left multiply by the adjoint: `A^H x`.
</td>
</tr><tr>
<td>
`adjoint_arg`
</td>
<td>
Python `bool`.  If `True`, compute `A x^H` where `x^H` is
the hermitian transpose (transposition and complex conjugation).
</td>
</tr><tr>
<td>
`name`
</td>
<td>
A name for this `Op`.
</td>
</tr>
</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Returns</th></tr>
<tr class="alt">
<td colspan="2">
A `LinearOperator` or `Tensor` with shape `[..., M, R]` and same `dtype`
as `self`.
</td>
</tr>

</table>



<h3 id="matvec"><code>matvec</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator.py#L694-L727">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>matvec(
    x, adjoint=(False), name='matvec'
)
</code></pre>

Transform [batch] vector `x` with left multiplication:  `x --> Ax`.

```python
# Make an operator acting like batch matrix A.  Assume A.shape = [..., M, N]
operator = LinearOperator(...)

X = ... # shape [..., N], batch vector

Y = operator.matvec(X)
Y.shape
==> [..., M]

Y[..., :] = sum_j A[..., :, j] X[..., j]
```

<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Args</th></tr>

<tr>
<td>
`x`
</td>
<td>
`Tensor` with compatible shape and same `dtype` as `self`.
`x` is treated as a [batch] vector meaning for every set of leading
dimensions, the last dimension defines a vector.
See class docstring for definition of compatibility.
</td>
</tr><tr>
<td>
`adjoint`
</td>
<td>
Python `bool`.  If `True`, left multiply by the adjoint: `A^H x`.
</td>
</tr><tr>
<td>
`name`
</td>
<td>
A name for this `Op`.
</td>
</tr>
</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Returns</th></tr>
<tr class="alt">
<td colspan="2">
A `Tensor` with shape `[..., M]` and same `dtype` as `self`.
</td>
</tr>

</table>



<h3 id="range_dimension_tensor"><code>range_dimension_tensor</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator.py#L484-L500">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>range_dimension_tensor(
    name='range_dimension_tensor'
)
</code></pre>

Dimension (in the sense of vector spaces) of the range of this operator.

Determined at runtime.

If this operator acts like the batch matrix `A` with
`A.shape = [B1,...,Bb, M, N]`, then this returns `M`.

<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Args</th></tr>

<tr>
<td>
`name`
</td>
<td>
A name for this `Op`.
</td>
</tr>
</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Returns</th></tr>
<tr class="alt">
<td colspan="2">
`int32` `Tensor`
</td>
</tr>

</table>



<h3 id="shape_tensor"><code>shape_tensor</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator.py#L321-L339">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>shape_tensor(
    name='shape_tensor'
)
</code></pre>

Shape of this `LinearOperator`, determined at runtime.

If this operator acts like the batch matrix `A` with
`A.shape = [B1,...,Bb, M, N]`, then this returns a `Tensor` holding
`[B1,...,Bb, M, N]`, equivalent to <a href="../../tf/shape.md"><code>tf.shape(A)</code></a>.

<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Args</th></tr>

<tr>
<td>
`name`
</td>
<td>
A name for this `Op`.
</td>
</tr>
</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Returns</th></tr>
<tr class="alt">
<td colspan="2">
`int32` `Tensor`
</td>
</tr>

</table>



<h3 id="solve"><code>solve</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator.py#L804-L877">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>solve(
    rhs, adjoint=(False), adjoint_arg=(False), name='solve'
)
</code></pre>

Solve (exact or approx) `R` (batch) systems of equations: `A X = rhs`.

The returned `Tensor` will be close to an exact solution if `A` is well
conditioned. Otherwise closeness will vary. See class docstring for details.

#### Examples:



```python
# Make an operator acting like batch matrix A.  Assume A.shape = [..., M, N]
operator = LinearOperator(...)
operator.shape = [..., M, N]

# Solve R > 0 linear systems for every member of the batch.
RHS = ... # shape [..., M, R]

X = operator.solve(RHS)
# X[..., :, r] is the solution to the r'th linear system
# sum_j A[..., :, j] X[..., j, r] = RHS[..., :, r]

operator.matmul(X)
==> RHS
```

<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Args</th></tr>

<tr>
<td>
`rhs`
</td>
<td>
`Tensor` with same `dtype` as this operator and compatible shape.
`rhs` is treated like a [batch] matrix meaning for every set of leading
dimensions, the last two dimensions defines a matrix.
See class docstring for definition of compatibility.
</td>
</tr><tr>
<td>
`adjoint`
</td>
<td>
Python `bool`.  If `True`, solve the system involving the adjoint
of this `LinearOperator`:  `A^H X = rhs`.
</td>
</tr><tr>
<td>
`adjoint_arg`
</td>
<td>
Python `bool`.  If `True`, solve `A X = rhs^H` where `rhs^H`
is the hermitian transpose (transposition and complex conjugation).
</td>
</tr><tr>
<td>
`name`
</td>
<td>
A name scope to use for ops added by this method.
</td>
</tr>
</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Returns</th></tr>
<tr class="alt">
<td colspan="2">
`Tensor` with shape `[...,N, R]` and same `dtype` as `rhs`.
</td>
</tr>

</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Raises</th></tr>

<tr>
<td>
`NotImplementedError`
</td>
<td>
If `self.is_non_singular` or `is_square` is False.
</td>
</tr>
</table>



<h3 id="solvevec"><code>solvevec</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator.py#L885-L931">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>solvevec(
    rhs, adjoint=(False), name='solve'
)
</code></pre>

Solve single equation with best effort: `A X = rhs`.

The returned `Tensor` will be close to an exact solution if `A` is well
conditioned. Otherwise closeness will vary. See class docstring for details.

#### Examples:



```python
# Make an operator acting like batch matrix A.  Assume A.shape = [..., M, N]
operator = LinearOperator(...)
operator.shape = [..., M, N]

# Solve one linear system for every member of the batch.
RHS = ... # shape [..., M]

X = operator.solvevec(RHS)
# X is the solution to the linear system
# sum_j A[..., :, j] X[..., j] = RHS[..., :]

operator.matvec(X)
==> RHS
```

<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Args</th></tr>

<tr>
<td>
`rhs`
</td>
<td>
`Tensor` with same `dtype` as this operator.
`rhs` is treated like a [batch] vector meaning for every set of leading
dimensions, the last dimension defines a vector.  See class docstring
for definition of compatibility regarding batch dimensions.
</td>
</tr><tr>
<td>
`adjoint`
</td>
<td>
Python `bool`.  If `True`, solve the system involving the adjoint
of this `LinearOperator`:  `A^H X = rhs`.
</td>
</tr><tr>
<td>
`name`
</td>
<td>
A name scope to use for ops added by this method.
</td>
</tr>
</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Returns</th></tr>
<tr class="alt">
<td colspan="2">
`Tensor` with shape `[...,N]` and same `dtype` as `rhs`.
</td>
</tr>

</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Raises</th></tr>

<tr>
<td>
`NotImplementedError`
</td>
<td>
If `self.is_non_singular` or `is_square` is False.
</td>
</tr>
</table>



<h3 id="tensor_rank_tensor"><code>tensor_rank_tensor</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator.py#L399-L413">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>tensor_rank_tensor(
    name='tensor_rank_tensor'
)
</code></pre>

Rank (in the sense of tensors) of matrix corresponding to this operator.

If this operator acts like the batch matrix `A` with
`A.shape = [B1,...,Bb, M, N]`, then this returns `b + 2`.

<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Args</th></tr>

<tr>
<td>
`name`
</td>
<td>
A name for this `Op`.
</td>
</tr>
</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Returns</th></tr>
<tr class="alt">
<td colspan="2">
`int32` `Tensor`, determined at runtime.
</td>
</tr>

</table>



<h3 id="to_dense"><code>to_dense</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator.py#L1019-L1022">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>to_dense(
    name='to_dense'
)
</code></pre>

Return a dense (batch) matrix representing this operator.


<h3 id="trace"><code>trace</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator.py#L1059-L1071">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>trace(
    name='trace'
)
</code></pre>

Trace of the linear operator, equal to sum of `self.diag_part()`.

If the operator is square, this is also the sum of the eigenvalues.

<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Args</th></tr>

<tr>
<td>
`name`
</td>
<td>
A name for this `Op`.
</td>
</tr>
</table>



<!-- Tabular view -->
 <table class="responsive fixed orange">
<colgroup><col width="214px"><col></colgroup>
<tr><th colspan="2">Returns</th></tr>
<tr class="alt">
<td colspan="2">
Shape `[B1,...,Bb]` `Tensor` of same `dtype` as `self`.
</td>
</tr>

</table>



<h3 id="__matmul__"><code>__matmul__</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/blob/r2.4/tensorflow/python/ops/linalg/linear_operator.py#L686-L687">View source</a>

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>__matmul__(
    other
)
</code></pre>





