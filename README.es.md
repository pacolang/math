<p align="center">
  <a href="https://github.com/pacolang/math"><img alt="License" src="https://img.shields.io/github/license/pacolang/math"></a>
  <a href="https://github.com/pacolang/math/stargazers"><img alt="Stars" src="https://img.shields.io/github/stars/pacolang/math"></a>
  <a href="https://github.com/pacolang/math/network/members"><img alt="Forks" src="https://img.shields.io/github/forks/pacolang/math"></a>
  <a href="https://github.com/pacolang/math/issues"><img alt="Issues" src="https://img.shields.io/github/issues/pacolang/math"></a>
</p>

<h1 align="center">Math</h1>

<p align="center">
  <code>Matrix</code> y <code>DataFrame</code>, construidos sobre <a href="https://github.com/pacolang/tensor"><code>tensor</code></a>, para <a href="https://github.com/pacolang/paco">Paco</a>.
  <br />
  <a href="docs/spec.md"><strong>Ver la especificación »</strong></a>
  <br />
  <br />
  <a href="https://github.com/pacolang/paco">Ver el Compilador</a>
  ·
  <a href="https://github.com/pacolang/math/issues/new">Reportar un Error</a>
  ·
  <a href="https://github.com/pacolang/rfcs">Proponer una RFC</a>
</p>

**Leer en:** [English](README.md) · [Português](README.pt-BR.md) · **Español**

> **Estado:** un módulo importable, `math` (`Matrix`, `DataFrame`), construido sobre [`pacolang/tensor`](https://github.com/pacolang/tensor) — ver [Ecosistema](#ecosistema).

## Índice

- [Acerca de](#acerca-de)
- [Ecosistema](#ecosistema)
- [Primeros Pasos](#primeros-pasos)
- [Uso](#uso)
- [Hoja de Ruta](#hoja-de-ruta)
- [Contribuir](#contribuir)
- [Licencia](#licencia)
- [Contacto](#contacto)

## Acerca de

`math::Matrix<T: Numeric, const R: int, const C: int>` envuelve un
`tensor::Tensor<T, R, C>` con las operaciones 2D habituales (`transpose`,
`identity`, `add`/`sub`/`mul`), y `math::DataFrame<S>` es una columna
creciente de filas de cualquier tipo `S`, con `#[derive(Schema)]`
generando un accesor de columna tipado por campo. Ver
[`docs/spec.md`](docs/spec.md) para la API completa.

Según la [RFC 0030](https://github.com/pacolang/rfcs/blob/main/text/0030-repository-organization-and-stdlib-scope.md),
que enmienda la [RFC 0011](https://github.com/pacolang/rfcs/blob/main/text/0011-data-analysis-stdlib.md),
`Matrix` y `DataFrame` no satisfacen ninguno de los cuatro criterios para
permanecer en el `stdlib` de `paco`, así que viven aquí, como su propia
biblioteca oficial, construida sobre
[`pacolang/tensor`](https://github.com/pacolang/tensor) de la misma forma
que cualquier programa depende de ella.

## Ecosistema

Una organización, `github.com/pacolang`, un repositorio por proyecto.
[`pacolang/paco`](https://github.com/pacolang/paco) es el núcleo:
compilador, runtime y `stdlib`, una sola versión. `pacolang/math` es una
biblioteca oficial junto a él, versionada de forma independiente,
obtenida con `paco get github.com/pacolang/math@<versión>` de la misma
forma que cualquier dependencia, y ella misma depende de
[`pacolang/tensor`](https://github.com/pacolang/tensor) (declarado en el
propio `paco.mod` de este repositorio). Las decisiones de diseño del
lenguaje y de su ecosistema se registran como RFCs en
[`pacolang/rfcs`](https://github.com/pacolang/rfcs), nunca como ADRs.

## Primeros Pasos

### Requisitos previos

- Un toolchain de `paco` — ver [`pacolang/paco`](https://github.com/pacolang/paco)
  para compilar o instalar uno.
- Un `paco.mod` en tu proyecto (`paco mod init` si todavía no tienes uno).

### Instalación

Un programa que importa `math` directamente también necesita `tensor`, ya
que la API pública de `math` devuelve `tensor::ShapeError`:

```bash
paco get github.com/pacolang/tensor@<versión>
paco get github.com/pacolang/math@<versión>
paco mod tidy
```

## Uso

```paco
use tensor;
use math;

fn main() {
    // Multiplicación de matrices: la dimensión interna es probada igual por el tipo.
    let m = math::Matrix<f32, 2, 3>::zeros();
    let n = math::Matrix<f32, 3, 4>::zeros();
    let product = m.mul(&n); // Result<Matrix<f32, 2, 4>, tensor::ShapeError>
}
```

Ver [`docs/spec.md`](docs/spec.md) para `DataFrame`/`Schema` y la API
completa de `Matrix`.

## Hoja de Ruta

- `add`/`sub`/`mul` de `Matrix` todavía devuelven `Result` en lugar de
  haber recibido la misma división panic/`checked_*` que ya tiene `Tensor`.

Ver los [issues](https://github.com/pacolang/math/issues) y los
[milestones](https://github.com/pacolang/math/milestones) de este
repositorio para el seguimiento del día a día.

## Contribuir

Las contribuciones son lo que hace de la comunidad open-source un lugar
increíble para aprender y crear. Cualquier contribución tuya es **muy
bienvenida**.

1. Haz un fork del repositorio.
2. Crea tu rama de feature (`git checkout -b feat/mi-feature`).
3. Ejecuta `paco test` antes de abrir un pull request.
4. Haz commit de tus cambios y abre un pull request.

Para un cambio de diseño del lenguaje o del ecosistema, abre una RFC en
[`pacolang/rfcs`](https://github.com/pacolang/rfcs) primero.

## Licencia

Distribuido bajo la Apache License, Version 2.0. Ver [`LICENSE`](LICENSE)
para más información.

## Contacto

Enlace del proyecto: [https://github.com/pacolang/math](https://github.com/pacolang/math)
