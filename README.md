# Trilha HTML - Dio.me

## Módulo 03 - Recriando a Wikipedia com um layout melhor

Desafio de Projeto do Módulo III da trilha de HTML da [DIO](https://www.dio.me/),
com foco em **semântica** e **acessibilidade**.

> **Tema escolhido:** Naruto Clássico — a primeira fase do mangá e do anime
> criados por Masashi Kishimoto.

---

## Sobre o projeto

O desafio pede a recriação de uma página no estilo Wikipédia, partindo de uma
estrutura pré-montada (`logo` / `bar` / `content` / `footer`) e evoluindo-a para
um site semanticamente correto e acessível.

O layout original usava apenas `<div>` genéricas. Aqui, todas foram substituídas
por elementos semânticos, e foram acrescentados os recursos de acessibilidade
que a estrutura original não tinha.

### Páginas

| Arquivo | Conteúdo |
| --- | --- |
| `index.html` | Artigo principal — Naruto Clássico |
| `personagens.html` | Personagens, equipes, Sannin e antagonistas |
| `temporadas.html` | Arcos narrativos, edições e formatos |

### Estrutura de arquivos

```
trilha-html-modulo-3/
├── index.html
├── personagens.html
├── temporadas.html
├── assets/
│   ├── css/
│   │   └── style.css
│   └── images/
│       ├── logo.svg
│       ├── hero-naruto.svg
│       ├── mapa-vilas.svg
│       ├── equipe7.svg
│       └── rasengan.svg
└── README.md
```

---

## Semântica

### De `<div>` para elementos com significado

| Antes (template original) | Depois |
| --- | --- |
| `<div class="logo">` | `<header>` com `<a>` contendo o logo |
| `<div class="bar">` | `<div class="barra">` com `<form role="search">` |
| `<div class="sidebar">` | `<aside>` + `<nav aria-label="...">` |
| `<div class="main">` | `<main id="conteudo-principal">` + `<article>` |
| `<div class="anchors">` | `<aside>` + `<nav aria-label="Índice do artigo">` |
| `<div class="footer">` | `<footer>` |

### Estrutura do documento

- **Um único `<h1>`** por página (o título do artigo), com hierarquia
  `h1 → h2 → h3` sem saltos de nível
- Seções marcadas com `<section aria-labelledby="...">`, cada uma apontando
  para o próprio título
- Conteúdo principal dentro de `<article>`
- Listas de definição (`<dl>`/`<dt>`/`<dd>`) para pares termo/definição
- `<time datetime="...">` para datas
- `<abbr title="...">` para siglas e termos técnicos
- `<del>` para indicar texto removido, `<mark>` para destaque,
  `<blockquote>` com `<footer>` para citar a fonte

### Ordem do documento

O `<main>` vem **antes** dos `<aside>` na ordem do DOM, embora apareça
visualmente no centro (a posição é definida por `grid-template-areas`).

**Por quê:** garante que o `h1` seja o primeiro título da página — quem navega
por títulos encontra o assunto do artigo imediatamente, sem antes esbarrar nos
títulos da barra lateral.

---

## Acessibilidade

### Recursos implementados

| Recurso | Onde | Para quê |
| --- | --- | --- |
| **Skip link** | Primeiro elemento do `<body>` | Pular cabeçalho e busca repetidos (WCAG 2.4.1) |
| **Foco visível** | `:focus-visible` global | Contorno de 3px, nunca removido (WCAG 2.4.7 / 2.4.11) |
| **Contraste AA** | Toda a paleta | Mínimo de 4.5:1 em textos (WCAG 1.4.3) |
| **Alvos de 44px** | Links, botões e campos | Área de toque confortável (WCAG 2.5.8) |
| **`aria-current="page"`** | Item ativo do menu | Indica a página atual |
| **`aria-label`** | Todos os `<nav>` | Diferencia os blocos de navegação |
| **`aria-labelledby`** | Todas as `<section>` | Nomeia cada seção pelo próprio título |
| **`role="search"`** | Formulário de busca | Identifica a região de busca |
| **Texto alternativo** | Todas as imagens | Descrição do conteúdo visual (WCAG 1.1.1) |
| **Rótulo de formulário** | Campo de busca | `<label>` com classe `visualmente-oculto` |
| **`prefers-reduced-motion`** | CSS | Desativa animações (WCAG 2.3.3) |
| **`forced-colors`** | CSS | Suporte a alto contraste do sistema |
| **Estilo de impressão** | CSS | Remove navegação e expande URLs |

### Paleta com contraste verificado

Os valores foram calculados com a fórmula de luminância relativa da WCAG, não
escolhidos "no olho":

| Uso | Cor | Contraste |
| --- | --- | --- |
| Títulos | `#14213d` | 15.97:1 sobre branco |
| Corpo do texto | `#4a4a4a` | 8.86:1 sobre branco |
| Links | `#c2410c` | 5.18:1 sobre branco |
| Texto auxiliar | `#6b6b6b` | 5.4:1 sobre branco |
| Destaque sobre fundo escuro | `#fbbf24` | 9.57:1 sobre `#14213d` |

O laranja vivo `#f97316` (2.80:1) é usado **apenas** em bordas e elementos
decorativos — nunca em texto pequeno, pois não atinge o mínimo AA.

### Cores não são o único indicador

O item ativo do menu usa **três** indicações simultâneas: cor de fundo, borda
esquerda e negrito — atendendo à WCAG 1.4.1 (Uso de Cor).

---

## Verificações executadas

Todas as checagens abaixo foram rodadas de fato, não presumidas:

### axe-core (WCAG 2.0/2.1 A + AA + best-practice)

| Página | Violações | Verificações aprovadas |
| --- | --- | --- |
| `index.html` | **0** | 52 |
| `personagens.html` | **0** | 49 |
| `temporadas.html` | **0** | 51 |

### Navegação por teclado

| Teste | Resultado |
| --- | --- |
| Skip link é o primeiro elemento focável | OK |
| Enter no skip link move o foco para o `<main>` | OK |
| Foco visível em input, botão, links e logo | OK (3px sólido) |
| Skip link aparece ao receber foco | OK |
| Nenhuma armadilha de teclado | OK |

### Estrutura e layout

| Teste | Resultado |
| --- | --- |
| HTML válido (aninhamento) nas 3 páginas | OK |
| Hierarquia de títulos sem saltos | OK |
| Âncoras internas resolvem | OK |
| Links entre páginas existem | OK |
| Recursos (CSS e imagens) existem | OK |
| Sem rolagem horizontal em 1400 / 900 / 420px | OK |
| Layout empilha corretamente no mobile | OK |

---

## Como visualizar

```bash
xdg-open index.html   # Linux
```

As imagens são arquivos `.svg` versionados no projeto, então tudo funciona
**offline**, sem depender de nenhum serviço externo.

### Testar com leitor de tela

- **Linux:** [Orca](https://wiki.gnome.org/Projects/Orca) (`orca`)
- **Windows:** [NVDA](https://www.nvaccess.org/download/) — recomendado na aula
- **Navegação por teclado:** `Tab`, `Shift+Tab`, `Enter` e `Esc`

Sugestão de teste: recarregue a página e pressione `Tab` uma vez. O primeiro
elemento a receber foco deve ser o aviso **“Pular para o conteúdo principal”**.

---

## Referências

- [DIO](https://www.dio.me/)
- [MDN — Acessibilidade na Web](https://developer.mozilla.org/pt-BR/docs/Web/Accessibility)
- [W3C — WCAG 2.2](https://www.w3.org/TR/WCAG22/)
- [W3C — ARIA Authoring Practices](https://www.w3.org/WAI/ARIA/apg/)
- [axe-core](https://github.com/dequelabs/axe-core)
- [NVDA — leitor de tela](https://www.nvaccess.org/download/)
- [Repositório original do desafio](https://github.com/digitalinnovationone/trilha-html-modulo-3)

---

Feito durante a trilha de HTML da DIO - Módulo III.
