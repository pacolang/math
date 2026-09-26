<p align="center">
  <a href="https://github.com/pacolang/math"><img alt="License" src="https://img.shields.io/github/license/pacolang/math"></a>
  <a href="https://github.com/pacolang/math/stargazers"><img alt="Stars" src="https://img.shields.io/github/stars/pacolang/math"></a>
  <a href="https://github.com/pacolang/math/network/members"><img alt="Forks" src="https://img.shields.io/github/forks/pacolang/math"></a>
  <a href="https://github.com/pacolang/math/issues"><img alt="Issues" src="https://img.shields.io/github/issues/pacolang/math"></a>
</p>

<h1 align="center">Math</h1>

<p align="center">
  <code>Matrix</code> and <code>DataFrame</code>, built on <a href="https://github.com/pacolang/tensor"><code>tensor</code></a>, for <a href="https://github.com/pacolang/paco">Paco</a>.
  <br />
  <a href="docs/spec.md"><strong>Explore the spec »</strong></a>
  <br />
  <br />
  <a href="https://github.com/pacolang/paco">View the Compiler</a>
  ·
  <a href="https://github.com/pacolang/math/issues/new">Report a Bug</a>
  ·
  <a href="https://github.com/pacolang/rfcs">Propose an RFC</a>
</p>

**Read this in:** **English** · [Português](README.pt-BR.md) · [Español](README.es.md)

> **Status:** one importable module, `math` (`Matrix`, `DataFrame`), built on [`pacolang/tensor`](https://github.com/pacolang/tensor) — see [Ecosystem](#ecosystem).

## Table of Contents

- [About](#about)
- [Ecosystem](#ecosystem)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

## About

`math::Matrix<T: Numeric, const R: int, const C: int>` wraps a
`tensor::Tensor<T, R, C>` with the usual 2D operations (`transpose`,
`identity`, `add`/`sub`/`mul`), and `math::DataFrame<S>` is a growable
column of rows of any type `S`, with `#[derive(Schema)]` generating a
typed column accessor per field. See [`docs/spec.md`](docs/spec.md) for
the full API.

Per [RFC 0030](https://github.com/pacolang/rfcs/blob/main/text/0030-repository-organization-and-stdlib-scope.md),
which amends [RFC 0011](https://github.com/pacolang/rfcs/blob/main/text/0011-data-analysis-stdlib.md),
`Matrix` and `DataFrame` satisfy none of the four criteria for staying in
`paco`'s `stdlib`, so they live here instead, as their own official
library, built on [`pacolang/tensor`](https://github.com/pacolang/tensor)
the same way any program depends on it.

## Ecosystem

One organization, `github.com/pacolang`, one repository per project.
[`pacolang/paco`](https://github.com/pacolang/paco) is the core: compiler,
runtime and `stdlib`, one version. `pacolang/math` is an official library
next to it, versioned independently, fetched with
`paco get github.com/pacolang/math@<version>` the way any dependency is,
and itself depends on [`pacolang/tensor`](https://github.com/pacolang/tensor)
(declared in this repository's own `paco.mod`). Design decisions for the
language and its ecosystem are recorded as RFCs in
[`pacolang/rfcs`](https://github.com/pacolang/rfcs), never as ADRs.

## Getting Started

### Prerequisites

- A `paco` toolchain — see [`pacolang/paco`](https://github.com/pacolang/paco)
  for building or installing one.
- A `paco.mod` in your project (`paco mod init` if you don't have one yet).

### Installation

A program that imports `math` directly also needs `tensor`, since
`math`'s public API returns `tensor::ShapeError`:

```bash
paco get github.com/pacolang/tensor@<version>
paco get github.com/pacolang/math@<version>
paco mod tidy
```

## Usage

```paco
use tensor;
use math;

fn main() {
    // Matrix multiply: the inner dimension is proved equal by the type.
    let m = math::Matrix<f32, 2, 3>::zeros();
    let n = math::Matrix<f32, 3, 4>::zeros();
    let product = m.mul(&n); // Result<Matrix<f32, 2, 4>, tensor::ShapeError>
}
```

See [`docs/spec.md`](docs/spec.md) for `DataFrame`/`Schema` and the full
`Matrix` API.

## Roadmap

- `Matrix`'s `add`/`sub`/`mul` still return `Result` rather than having had
  `Tensor`'s panicking/`checked_*` split applied to them.

See this repository's [issues](https://github.com/pacolang/math/issues)
and [milestones](https://github.com/pacolang/math/milestones) for
day-to-day tracking.

## Contributing

Contributions are what make the open-source community such an amazing
place to learn and create. Any contribution you make is **greatly
appreciated**.

1. Fork the repository.
2. Create your feature branch (`git checkout -b feat/my-feature`).
3. Run `paco test` before opening a pull request.
4. Commit your changes and open a pull request.

For a language or ecosystem design change, open an RFC in
[`pacolang/rfcs`](https://github.com/pacolang/rfcs) first.

## License

Distributed under the Apache License, Version 2.0. See [`LICENSE`](LICENSE)
for more information.

## Contact

Project Link: [https://github.com/pacolang/math](https://github.com/pacolang/math)
