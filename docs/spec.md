# `math` library spec

`math` (`github.com/pacolang/math`) builds two-dimensional and tabular
types on top of `tensor` (`github.com/pacolang/tensor`), declared as a
dependency in `paco.mod` and fetched with `paco get`; call sites use
plain `tensor::` paths once fetched, the same as any other import.

## `Matrix<T: Numeric, const R: int, const C: int>`

A dense row-major matrix whose dimensions are part of its type
(`Matrix<float, 2, 3>`), wrapping a `tensor::Tensor<T, R, C>`.

### Construction

- `Matrix::zeros() -> Self` — all zeros; a `Dyn` dimension gets extent `0`.
- `Matrix::with_shape(rows: i64, cols: i64) -> Result<Self, tensor::ShapeError>`
  — all zeros with the given extents, which must match every static
  dimension; they fill in the `Dyn` ones. `Err(ShapeError)` on a mismatch.
- `Matrix::identity() -> Self` (`T: Numeric + Add + Sub + Mul`) — the
  identity matrix (`1` on the diagonal, `0` elsewhere).

### Query and element access

- `rows(&self) -> i64`, `cols(&self) -> i64`.
- `get(&self, r: i64, c: i64) -> T`, `set(&mut self, r: i64, c: i64, value: T)`.
- `data(&self) -> &[]T`, `data_mut(&mut self) -> &mut []T` — the row-major
  buffer.
- `tensor(&self) -> &tensor::Tensor<T, R, C>` — the underlying tensor.
- `transpose(&self) -> Matrix<T, C, R>`.

### Arithmetic (`T: Numeric + Add + Sub + Mul`)

Unlike `Tensor`'s `add`/`sub`/`mul`/`matmul`, `Matrix` has not yet had the
checked/panicking split applied (out of scope for this extraction — see the
extraction change's tasks.md task 3.3): `add`/`sub`/`mul` each still return
`Result`, delegating directly to `checked_add`/`checked_sub`/`checked_mul`,
and `a + b`/`a - b`/`a * b` (Paco dispatches an operator to the
identically-named method, structurally) each therefore evaluate to a
`Result` too, not a bare `Matrix`.

- `add`/`sub(&self, other: &Self) -> Result<Self, tensor::ShapeError>` —
  element-wise, `Err(ShapeError)` on a shape mismatch.
- `checked_add`/`checked_sub<const R2: int, const C2: int>(&self, other: &Matrix<T, R2, C2>) -> Result<Self, tensor::ShapeError>`
  — currently identical to `add`/`sub` (kept as separate names so a future
  panicking split does not have to invent them later).
- `mul<const N: int>(&self, other: &Matrix<T, C, N>) -> Result<Matrix<T, R, N>, tensor::ShapeError>`
  — `self` (R×C) times `other` (C×N); the inner dimension is proved equal
  by the type.
- `checked_mul<const J: int, const N: int>(&self, other: &Matrix<T, J, N>) -> Result<Matrix<T, R, N>, tensor::ShapeError>`
  — `self` (R×C) times `other` (J×N) where `C` and `J` are not proved
  equal; compared at run time.
- `scale(&self, k: T) -> Self` — multiply every element by a scalar.

## `DataFrame<S>`

A growable column of rows of any type `S`.

- `DataFrame::new() -> Self`.
- `push(&mut self, row: S)`.
- `len(&self) -> i64`.
- `row(&self, i: i64) -> Option<S>` — `None` when `i` is out of range.

## `Schema` and `#[derive(Schema)]`

`#[derivable(derive_schema)] pub trait Schema` lets any struct opt in with
`#[derive(Schema)]`. For each field `f: T` of the struct, `derive_schema`
generates a column accessor `S::f(frame: &math::DataFrame<S>) -> Vec<T>`
that reads that field out of every row, in order — a comptime-generated
column projection over a `DataFrame` of that row type.
