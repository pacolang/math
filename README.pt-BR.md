<p align="center">
  <a href="https://github.com/pacolang/math"><img alt="License" src="https://img.shields.io/github/license/pacolang/math"></a>
  <a href="https://github.com/pacolang/math/stargazers"><img alt="Stars" src="https://img.shields.io/github/stars/pacolang/math"></a>
  <a href="https://github.com/pacolang/math/network/members"><img alt="Forks" src="https://img.shields.io/github/forks/pacolang/math"></a>
  <a href="https://github.com/pacolang/math/issues"><img alt="Issues" src="https://img.shields.io/github/issues/pacolang/math"></a>
</p>

<h1 align="center">Math</h1>

<p align="center">
  <code>Matrix</code> e <code>DataFrame</code>, construídos sobre <a href="https://github.com/pacolang/tensor"><code>tensor</code></a>, para o <a href="https://github.com/pacolang/paco">Paco</a>.
  <br />
  <a href="docs/spec.md"><strong>Veja a especificação »</strong></a>
  <br />
  <br />
  <a href="https://github.com/pacolang/paco">Ver o Compilador</a>
  ·
  <a href="https://github.com/pacolang/math/issues/new">Reportar um Bug</a>
  ·
  <a href="https://github.com/pacolang/rfcs">Propor uma RFC</a>
</p>

**Leia em:** [English](README.md) · **Português** · [Español](README.es.md)

> **Status:** um módulo importável, `math` (`Matrix`, `DataFrame`), construído sobre [`pacolang/tensor`](https://github.com/pacolang/tensor) — veja [Ecossistema](#ecossistema).

## Sumário

- [Sobre](#sobre)
- [Ecossistema](#ecossistema)
- [Primeiros Passos](#primeiros-passos)
- [Uso](#uso)
- [Roteiro](#roteiro)
- [Contribuindo](#contribuindo)
- [Licença](#licença)
- [Contato](#contato)

## Sobre

`math::Matrix<T: Numeric, const R: int, const C: int>` envolve um
`tensor::Tensor<T, R, C>` com as operações 2D usuais (`transpose`,
`identity`, `add`/`sub`/`mul`), e `math::DataFrame<S>` é uma coluna
crescente de linhas de qualquer tipo `S`, com `#[derive(Schema)]` gerando
um acessor de coluna tipado por campo. Veja [`docs/spec.md`](docs/spec.md)
para a API completa.

Pela [RFC 0030](https://github.com/pacolang/rfcs/blob/main/text/0030-repository-organization-and-stdlib-scope.md),
que emenda a [RFC 0011](https://github.com/pacolang/rfcs/blob/main/text/0011-data-analysis-stdlib.md),
`Matrix` e `DataFrame` não satisfazem nenhum dos quatro critérios para
permanecer no `stdlib` do `paco`, então vivem aqui, como sua própria
biblioteca oficial, construída sobre
[`pacolang/tensor`](https://github.com/pacolang/tensor) da mesma forma que
qualquer programa depende dela.

## Ecossistema

Uma organização, `github.com/pacolang`, um repositório por projeto.
[`pacolang/paco`](https://github.com/pacolang/paco) é o núcleo: compilador,
runtime e `stdlib`, uma única versão. `pacolang/math` é uma biblioteca
oficial ao lado dele, versionada de forma independente, obtida com
`paco get github.com/pacolang/math@<versão>` da mesma forma que qualquer
dependência, e ela mesma depende de
[`pacolang/tensor`](https://github.com/pacolang/tensor) (declarado no
próprio `paco.mod` deste repositório). Decisões de design da linguagem e
do seu ecossistema são registradas como RFCs em
[`pacolang/rfcs`](https://github.com/pacolang/rfcs), nunca como ADRs.

## Primeiros Passos

### Pré-requisitos

- Um toolchain `paco` — veja [`pacolang/paco`](https://github.com/pacolang/paco)
  para construir ou instalar um.
- Um `paco.mod` no seu projeto (`paco mod init` se ainda não tiver um).

### Instalação

Um programa que importa `math` diretamente também precisa de `tensor`,
já que a API pública de `math` retorna `tensor::ShapeError`:

```bash
paco get github.com/pacolang/tensor@<versão>
paco get github.com/pacolang/math@<versão>
paco mod tidy
```

## Uso

```paco
use tensor;
use math;

fn main() {
    // Multiplicação de matrizes: a dimensão interna é provada igual pelo tipo.
    let m = math::Matrix<f32, 2, 3>::zeros();
    let n = math::Matrix<f32, 3, 4>::zeros();
    let product = m.mul(&n); // Result<Matrix<f32, 2, 4>, tensor::ShapeError>
}
```

Veja [`docs/spec.md`](docs/spec.md) para `DataFrame`/`Schema` e a API
completa de `Matrix`.

## Roteiro

- `add`/`sub`/`mul` de `Matrix` ainda retornam `Result` em vez de terem
  recebido a mesma divisão panic/`checked_*` que `Tensor` já tem.

Veja as [issues](https://github.com/pacolang/math/issues) e os
[milestones](https://github.com/pacolang/math/milestones) deste
repositório para o acompanhamento do dia a dia.

## Contribuindo

Contribuições são o que fazem da comunidade open-source um lugar incrível
para aprender e criar. Qualquer contribuição sua é **muito bem-vinda**.

1. Faça um fork do repositório.
2. Crie sua branch de feature (`git checkout -b feat/minha-feature`).
3. Rode `paco test` antes de abrir um pull request.
4. Faça commit das suas mudanças e abra um pull request.

Para uma mudança de design de linguagem ou de ecossistema, abra uma RFC em
[`pacolang/rfcs`](https://github.com/pacolang/rfcs) primeiro.

## Licença

Distribuído sob a Apache License, Version 2.0. Veja [`LICENSE`](LICENSE)
para mais informações.

## Contato

Link do projeto: [https://github.com/pacolang/math](https://github.com/pacolang/math)
