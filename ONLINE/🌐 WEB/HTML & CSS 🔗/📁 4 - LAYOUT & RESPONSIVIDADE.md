
# 📄 12 - MEDIA QUERIES


## Conceito

Media queries permitem aplicar estilos CSS diferentes com base nas características do dispositivo ou navegador, como largura, altura, orientação e tipo de mídia.

## Sintaxe Básica
```css
@media tipo-de-midia and (condição) {
  /* Regras CSS que se aplicam quando a condição é verdadeira */
}
````

## Tipos de Mídia

```css
@media all { ... }        /* Todos os dispositivos (padrão) */
@media screen { ... }     /* Telas */
@media print { ... }      /* Impressão */
@media speech { ... }     /* Leitores de tela */
```

## Condições Comuns

### Largura da Viewport

```css
@media (min-width: 768px) { ... }      /* Largura mínima (a partir de) */
@media (max-width: 767px) { ... }      /* Largura máxima (até) */
@media (width: 1024px) { ... }         /* Largura exata */
@media (width >= 768px) { ... }        /* Sintaxe moderna: largura >= 768px */
@media (width <= 767px) { ... }        /* Sintaxe moderna: largura <= 767px */
```

### Altura da Viewport

```css
@media (min-height: 600px) { ... }     /* Altura mínima */
@media (max-height: 599px) { ... }     /* Altura máxima */
```

### Intervalo de Largura

```css
@media (min-width: 768px) and (max-width: 1023px) { ... }  /* Entre 768px e 1023px */
@media (768px <= width <= 1023px) { ... }                  /* Sintaxe moderna */
```

### Orientação

```css
@media (orientation: portrait) { ... }  /* Altura > Largura */
@media (orientation: landscape) { ... } /* Largura > Altura */
```

### Proporção da Tela

```css
@media (aspect-ratio: 16/9) { ... }     /* Proporção exata */
@media (min-aspect-ratio: 1/1) { ... }  /* Proporção mínima */
@media (max-aspect-ratio: 2/1) { ... }  /* Proporção máxima */
```

### Densidade de Pixels

```css
@media (resolution: 96dpi) { ... }           /* Resolução exata */
@media (min-resolution: 300dpi) { ... }      /* Resolução mínima (telas de alta densidade) */
@media (-webkit-min-device-pixel-ratio: 2),  /* Para compatibilidade com Safari */
       (min-resolution: 192dpi) { ... }
```

### Recursos do Dispositivo

```css
@media (hover: hover) { ... }           /* Dispositivo tem capacidade de hover */
@media (hover: none) { ... }            /* Dispositivo não tem hover (geralmente touch) */
@media (pointer: fine) { ... }          /* Dispositivo tem ponteiro preciso (mouse) */
@media (pointer: coarse) { ... }        /* Dispositivo tem ponteiro impreciso (touch) */
@media (prefers-reduced-motion) { ... } /* Usuário prefere menos animações */
@media (prefers-color-scheme: dark) { ... } /* Usuário prefere tema escuro */
@media (prefers-color-scheme: light) { ... } /* Usuário prefere tema claro */
```

## Operadores Lógicos

```css
@media screen and (min-width: 768px) { ... }  /* AND: ambas condições verdadeiras */
@media screen or (min-width: 768px) { ... }   /* OR: qualquer condição verdadeira */
@media not screen { ... }                     /* NOT: inverte a condição */
```

## Breakpoints Comuns

```css
/* Mobile First (recomendado) */
/* Estilos base para mobile */
body {
  font-size: 16px;
}

/* Tablets */
@media (min-width: 768px) {
  body {
    font-size: 18px;
  }
}

/* Desktops */
@media (min-width: 1024px) {
  body {
    font-size: 20px;
  }
}

/* Telas grandes */
@media (min-width: 1440px) {
  body {
    max-width: 1200px;
    margin: 0 auto;
  }
}
```

## Media Queries no HTML

```html
<!-- Adapta viewport para dispositivos móveis -->
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<!-- CSS específico para impressão -->
<link rel="stylesheet" href="print.css" media="print">

<!-- CSS específico para diferentes tamanhos de tela -->
<link rel="stylesheet" href="mobile.css" media="(max-width: 767px)">
<link rel="stylesheet" href="tablet.css" media="(min-width: 768px) and (max-width: 1023px)">
<link rel="stylesheet" href="desktop.css" media="(min-width: 1024px)">
```

## Imagens Responsivas com Media Queries

```css
/* Imagem de fundo diferente para cada tamanho de tela */
.hero {
  background-image: url('img/hero-mobile.jpg');
}

@media (min-width: 768px) {
  .hero {
    background-image: url('img/hero-tablet.jpg');
  }
}

@media (min-width: 1024px) {
  .hero {
    background-image: url('img/hero-desktop.jpg');
  }
}
```

## Layout Responsivo com Grid

```css
.grid-container {
  display: grid;
  gap: 15px;
  
  /* Layout mobile: 1 coluna */
  grid-template-columns: 1fr;
}

/* Tablet: 2 colunas */
@media (min-width: 768px) {
  .grid-container {
    grid-template-columns: repeat(2, 1fr);
  }
}

/* Desktop: 3 colunas */
@media (min-width: 1024px) {
  .grid-container {
    grid-template-columns: repeat(3, 1fr);
  }
}

/* Telas grandes: 4 colunas */
@media (min-width: 1440px) {
  .grid-container {
    grid-template-columns: repeat(4, 1fr);
  }
}
```

## Menu Responsivo

```css
/* Estilo base do menu (mobile) */
.nav-menu {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

/* Botão do menu mobile */
.menu-toggle {
  display: block;
}

/* Em telas maiores */
@media (min-width: 768px) {
  .nav-menu {
    flex-direction: row;
  }
  
  .menu-toggle {
    display: none; /* Esconde o botão em telas maiores */
  }
}
```

## Dicas e Melhores Práticas

1. **Mobile First**: Comece com estilos para dispositivos móveis e depois adicione media queries para telas maiores.
2. **Use breakpoints significativos**: Adapte o design quando o layout começar a quebrar, não em tamanhos arbitrários.
3. **Evite muitos breakpoints**: Mantenha o número de breakpoints gerenciável (3-5 é comum).
4. **Teste em dispositivos reais**: Ou use ferramentas de inspeção do navegador para simular.
5. **Use unidades relativas**: Prefira rem, em, % em vez de pixels fixos.
6. **Foque em componentes**: Escreva media queries por componente, não para toda a página.

---

# 📄 13 - DESIGN RESPONSIVO

## Princípios Fundamentais

### 1. Layout Fluido
Usar unidades relativas (%, vw, vh, rem) em vez de unidades fixas (px):

```css
/* Layout fluido */
.container {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 5%;
}

/* Texto fluido */
body {
  font-size: calc(16px + 0.5vw);
}

h1 {
  font-size: clamp(2rem, 5vw, 3.5rem);
}
````

### 2. Media Queries
Adaptar o layout para diferentes tamanhos de tela:

```css
/* Base (mobile) */
.card {
  width: 100%;
}

/* Tablet */
@media (min-width: 768px) {
  .card {
    width: 48%;
  }
}

/* Desktop */
@media (min-width: 1024px) {
  .card {
    width: 30%;
  }
}
```

### 3. Imagens Flexíveis
Garantir que as imagens se adaptem ao container:

```css
img {
  max-width: 100%;
  height: auto;
}
```

## Viewport Meta Tag
Essencial para sites responsivos em dispositivos móveis:


```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

## Estratégias de Layout
### Sistema de Grid Responsivo

```css
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
}
```

### Flexbox para Componentes Responsivos
```css
.nav {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.nav-item {
  flex: 1 1 100px;
}

@media (max-width: 768px) {
  .nav {
    flex-direction: column;
  }
}
```

### Containers com Largura Máxima

```css
.container {
  width: 100%;
  padding: 0 20px;
  margin: 0 auto;
}

/* Breakpoints comuns */
@media (min-width: 576px) {
  .container {
    max-width: 540px;
  }
}

@media (min-width: 768px) {
  .container {
    max-width: 720px;
  }
}

@media (min-width: 992px) {
  .container {
    max-width: 960px;
  }
}

@media (min-width: 1200px) {
  .container {
    max-width: 1140px;
  }
}
```

## Navegação Responsiva

### Menu Hamburguer

```html
<button class="menu-toggle" aria-expanded="false" aria-controls="main-nav">
  <span class="sr-only">Menu</span>
  <span class="hamburger"></span>
</button>

<nav id="main-nav" class="nav">
  <ul class="nav-list">
    <li><a href="#">Home</a></li>
    <li><a href="#">Sobre</a></li>
    <li><a href="#">Serviços</a></li>
    <li><a href="#">Contato</a></li>
  </ul>
</nav>
```

```css
/* Estilo do botão hamburguer */
.menu-toggle {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
  padding: 10px;
}

.hamburger,
.hamburger::before,
.hamburger::after {
  display: block;
  width: 25px;
  height: 3px;
  background: #333;
  position: relative;
}

.hamburger::before,
.hamburger::after {
  content: '';
  position: absolute;
}

.hamburger::before { top: -8px; }
.hamburger::after { bottom: -8px; }

/* Navegação responsiva */
@media (max-width: 768px) {
  .menu-toggle {
    display: block;
  }
  
  .nav-list {
    display: none;
    flex-direction: column;
    width: 100%;
  }
  
  .nav-list.active {
    display: flex;
  }
}

/* Para acessibilidade, usar JavaScript para alternar a classe .active
   e atualizar aria-expanded quando o botão é clicado */
```

## Tipografia Responsiva

### Font-Size Responsivo

```css
/* Escala básica de tipografia */
:root {
  --font-size-base: 16px;
  --font-size-ratio: 1.25;
}

body {
  font-size: var(--font-size-base);
}

h1 { font-size: calc(var(--font-size-base) * var(--font-size-ratio) * var(--font-size-ratio) * var(--font-size-ratio)); }
h2 { font-size: calc(var(--font-size-base) * var(--font-size-ratio) * var(--font-size-ratio)); }
h3 { font-size: calc(var(--font-size-base) * var(--font-size-ratio)); }

@media (min-width: 768px) {
  :root {
    --font-size-base: 18px;
  }
}
```

### Clamp para Tamanhos Fluidos

```css
h1 {
  font-size: clamp(1.75rem, 4vw, 3.5rem);
}

.hero-title {
  font-size: clamp(2rem, 5vw + 1rem, 5rem);
}

.container {
  width: clamp(320px, 90vw, 1200px);
}
```

## Imagens Responsivas

### Atributo srcset

```html
<img 
  src="imagem-default.jpg" 
  srcset="imagem-small.jpg 500w,
          imagem-medium.jpg 1000w,
          imagem-large.jpg 1500w"
  sizes="(max-width: 600px) 100vw,
         (max-width: 1200px) 50vw,
         33vw"
  alt="Descrição da imagem">
```

### Picture Element

```html
<picture>
  <source media="(min-width: 1200px)" srcset="imagem-desktop.jpg">
  <source media="(min-width: 768px)" srcset="imagem-tablet.jpg">
  <img src="imagem-mobile.jpg" alt="Descrição da imagem">
</picture>
```

## Tabelas Responsivas

### Abordagem com Overflow

```css
.table-container {
  overflow-x: auto;
}
```

```html
<div class="table-container">
  <table>
    <!-- Conteúdo da tabela -->
  </table>
</div>
```

### Reorganização para Mobile

```css
@media (max-width: 768px) {
  table, thead, tbody, th, td, tr {
    display: block;
  }
  
  thead tr {
    position: absolute;
    top: -9999px;
    left: -9999px;
  }
  
  tr {
    margin-bottom: 15px;
    border: 1px solid #ccc;
  }
  
  td {
    position: relative;
    padding-left: 50%;
    text-align: left;
  }
  
  td:before {
    position: absolute;
    left: 10px;
    width: 45%;
    padding-right: 10px;
    white-space: nowrap;
    content: attr(data-label);
    font-weight: bold;
  }
}
```

```html
<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Idade</th>
      <th>Profissão</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-label="Nome">João Silva</td>
      <td data-label="Idade">28</td>
      <td data-label="Profissão">Desenvolvedor</td>
    </tr>
    <!-- Mais linhas -->
  </tbody>
</table>
```

## Formulários Responsivos

```css
.form-group {
  margin-bottom: 20px;
}

.form-control {
  display: block;
  width: 100%;
  padding: 10px;
  font-size: 16px; /* Previne zoom em dispositivos iOS */
}

@media (min-width: 768px) {
  .form-row {
    display: flex;
    gap: 20px;
  }
  
  .form-group.half {
    flex: 1;
  }
}
```

```html
<form>
  <div class="form-row">
    <div class="form-group half">
      <label for="firstname">Nome</label>
      <input type="text" id="firstname" class="form-control">
    </div>
    
    <div class="form-group half">
      <label for="lastname">Sobrenome</label>
```

```CSS
    <div class="form-group half">
      <label for="lastname">Sobrenome</label>
      <input type="text" id="lastname" class="form-control">
    </div>
  </div>
  
  <div class="form-group">
    <label for="email">Email</label>
    <input type="email" id="email" class="form-control">
  </div>
  
  <div class="form-group">
    <label for="message">Mensagem</label>
    <textarea id="message" class="form-control" rows="5"></textarea>
  </div>
  
  <button type="submit" class="btn">Enviar</button>
</form>
```

## Vídeos Responsivos

### Container com Proporção Fixa
```css
.video-container {
  position: relative;
  width: 100%;
  padding-bottom: 56.25%; /* 16:9 */
  height: 0;
  overflow: hidden;
}

.video-container iframe,
.video-container video {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
```

```html
<div class="video-container">
  <iframe src="https://www.youtube.com/embed/VIDEO_ID" frameborder="0" allowfullscreen></iframe>
</div>
```

## Padrões de Layout Responsivo

### Layout de Cards

```css
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
}

.card {
  display: flex;
  flex-direction: column;
  border: 1px solid #ddd;
  border-radius: 8px;
  overflow: hidden;
}

.card-image {
  width: 100%;
  aspect-ratio: 16/9;
  object-fit: cover;
}

.card-content {
  padding: 15px;
  flex-grow: 1;
}
```

### Layout de Alternância Texto-Imagem
```css
.text-image-section {
  display: flex;
  flex-direction: column;
  margin: 40px 0;
}

.text-content,
.image-container {
  width: 100%;
}

@media (min-width: 768px) {
  .text-image-section {
    flex-direction: row;
    align-items: center;
    gap: 40px;
  }
  
  .text-content,
  .image-container {
    width: 50%;
  }
  
  /* Alternar direção para seções pares */
  .text-image-section:nth-child(even) {
    flex-direction: row-reverse;
  }
}
```

### Layout de Hero Responsivo

```css
.hero {
  position: relative;
  height: 100vh;
  min-height: 500px;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 0 20px;
  background-size: cover;
  background-position: center;
}

.hero-content {
  max-width: 600px;
  z-index: 1;
}

.hero-title {
  font-size: clamp(2rem, 8vw, 4rem);
  margin-bottom: 20px;
  color: white;
}

.hero-text {
  font-size: clamp(1rem, 4vw, 1.5rem);
  margin-bottom: 30px;
  color: white;
}

.hero::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5); /* Overlay escuro */
}

@media (max-width: 768px) {
  .hero {
    height: 70vh;
  }
}
```

## Testes de Responsividade

### Ferramentas de Teste

1. **DevTools do Navegador** - Chrome, Firefox, Edge têm modo responsivo
2. **Simuladores de Dispositivo** - BrowserStack, LambdaTest
3. **Testes em Dispositivos Reais** - Smartphones, tablets de diferentes tamanhos

### Checklist de Responsividade

- [ ]  Site funciona em todas as larguras de tela (320px - 1920px+)
- [ ]  Texto é legível sem zoom em dispositivos móveis
- [ ]  Elementos interativos têm tamanho adequado para toque (min. 44x44px)
- [ ]  Menus navegáveis em telas pequenas
- [ ]  Não há rolagem horizontal em telas pequenas
- [ ]  Imagens carregam corretamente e mantêm proporção
- [ ]  Formulários são utilizáveis em dispositivos móveis
- [ ]  Contraste adequado em diferentes tamanhos de tela
- [ ]  Performance é boa em conexões móveis

## Dicas e Melhores Práticas

1. **Comece pelo mobile** (Mobile First Design)
2. **Use unidades relativas** (rem, em, %, vw, vh)
3. **Teste frequentemente** em diferentes dispositivos
4. **Mantenha a performance** otimizando imagens e recursos
5. **Considere a acessibilidade** em todos os tamanhos de tela
6. **Simplifique para telas menores** quando necessário
7. **Priorize o conteúdo mais importante** em telas pequenas
8. **Evite elementos que dependem de hover** em interfaces touch

---


# 📄 14 - FLEXBOX AVANÇADO

## Fundamentos Revisados

### Container Flex
```css
.container {
  display: flex;
  /* ou */
  display: inline-flex;
}
````

### Direção e Quebra

```css
.container {
  flex-direction: row | row-reverse | column | column-reverse;
  flex-wrap: nowrap | wrap | wrap-reverse;
  
  /* Shorthand */
  flex-flow: row wrap;
}
```

### Alinhamento

```css
.container {
  /* Alinhamento no eixo principal */
  justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly;
  
  /* Alinhamento no eixo transversal */
  align-items: stretch | flex-start | flex-end | center | baseline;
  
  /* Alinhamento de múltiplas linhas */
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

## Técnicas Avançadas

### Holy Grail Layout com Flexbox

Layout clássico com cabeçalho, rodapé, conteúdo principal e barras laterais.

```html
<div class="holy-grail">
  <header class="holy-grail-header">Cabeçalho</header>
  <div class="holy-grail-body">
    <nav class="holy-grail-nav">Navegação</nav>
    <main class="holy-grail-content">Conteúdo Principal</main>
    <aside class="holy-grail-aside">Barra Lateral</aside>
  </div>
  <footer class="holy-grail-footer">Rodapé</footer>
</div>
```

```css
.holy-grail {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.holy-grail-header,
.holy-grail-footer {
  flex: 0 0 auto;
  padding: 20px;
}

.holy-grail-body {
  display: flex;
  flex: 1 0 auto;
}

.holy-grail-content {
  flex: 1 0 0;
  padding: 20px;
}

.holy-grail-nav,
.holy-grail-aside {
  flex: 0 0 200px;
  padding: 20px;
}

/* Responsividade */
@media (max-width: 768px) {
  .holy-grail-body {
    flex-direction: column;
  }
  
  .holy-grail-nav,
  .holy-grail-aside,
  .holy-grail-content {
    flex-basis: auto;
  }
}
```

### Card com Proporção Fixa

```css
.card {
  display: flex;
  flex-direction: column;
  border: 1px solid #ddd;
  border-radius: 8px;
  overflow: hidden;
}

.card-image {
  /* Mantém proporção 16:9 */
  padding-top: 56.25%;
  position: relative;
}

.card-image img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.card-content {
  padding: 16px;
  flex-grow: 1; /* Cresce para ocupar espaço disponível */
}

.card-footer {
  padding: 16px;
  border-top: 1px solid #eee;
}
```

### Navegação Horizontal/Vertical Responsiva

```css
.nav {
  display: flex;
  list-style: none;
  padding: 0;
  margin: 0;
}

.nav-item {
  padding: 10px 15px;
}

/* Vertical em mobile, horizontal em desktop */
@media (max-width: 768px) {
  .nav {
    flex-direction: column;
  }
}

/* Ou usando flex-wrap para menu que quebra */
.nav-wrap {
  display: flex;
  flex-wrap: wrap;
}

.nav-wrap .nav-item {
  flex: 1 1 auto;
  text-align: center;
  min-width: 100px;
}
```

### Centralizando com Flexbox

```css
/* Centralização perfeita horizontal e vertical */
.center-flex {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh; /* ou qualquer altura desejada */
}

/* Centralização em coluna */
.center-column {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  height: 100vh;
}
```

### Layout de Grade com Flexbox

Antes do CSS Grid, flexbox era usado para criar layouts em grade:

```css
.flex-grid {
  display: flex;
  flex-wrap: wrap;
  margin: -10px; /* Compensa o padding dos itens */
}

.flex-grid-item {
  flex: 1 1 300px; /* Cresce, encolhe, base 300px */
  margin: 10px;
}

/* Definindo número de colunas */
.flex-grid-2 .flex-grid-item {
  flex-basis: calc(50% - 20px);
}

.flex-grid-3 .flex-grid-item {
  flex-basis: calc(33.333% - 20px);
}

.flex-grid-4 .flex-grid-item {
  flex-basis: calc(25% - 20px);
}

@media (max-width: 768px) {
  .flex-grid-item {
    flex-basis: calc(50% - 20px);
  }
}

@media (max-width: 480px) {
  .flex-grid-item {
    flex-basis: 100%;
  }
}
```

### Sticky Footer com Flexbox

```css
body {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  margin: 0;
}

.content {
  flex: 1 0 auto; /* Cresce para preencher espaço */
}

.footer {
  flex-shrink: 0; /* Não encolhe */
}
```

### Layout de Cartões com Alturas Iguais

```css
.card-container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.card {
  flex: 1 1 300px;
  display: flex;
  flex-direction: column;
  border: 1px solid #ddd;
}

.card-body {
  flex-grow: 1; /* Faz todos os cards terem a mesma altura */
}
```

### Espaçamento com gap

O gap é uma propriedade moderna que substitui margens entre itens flex:

```css
.flex-container {
  display: flex;
  gap: 20px; /* Espaçamento igual entre todos os itens */
  row-gap: 20px; /* Apenas entre linhas */
  column-gap: 10px; /* Apenas entre colunas */
}
```

### Ordem Flexível dos Elementos

```css
.flex-container {
  display: flex;
}

.first {
  order: -1; /* Aparece primeiro */
}

.last {
  order: 1; /* Aparece por último */
}

/* Reordenando em mobile */
@media (max-width: 768px) {
  .mobile-first {
    order: -1;
  }
}
```

### Alinhamento Responsivo

```css
.flex-container {
  display: flex;
  justify-content: space-between; /* Desktop */
}

@media (max-width: 768px) {
  .flex-container {
    flex-direction: column;
    justify-content: flex-start; /* Mobile */
  }
}
```

### Flexbox e Overflow

```css
.flex-container {
  display: flex;
}

.truncate {
  flex: 1;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.fixed {
  flex: 0 0 auto; /* Não cresce, não encolhe */
}
```

### Flexbox para Formulários

```css
.form-row {
  display: flex;
  gap: 15px;
  margin-bottom: 15px;
}

.form-group {
  flex: 1; /* Distribui espaço igualmente */
}

.form-group.small {
  flex: 0 0 100px; /* Largura fixa */
}

.form-group.large {
  flex: 2; /* Duas vezes mais largo que os outros */
}

/* Responsivo */
@media (max-width: 768px) {
  .form-row {
    flex-direction: column;
  }
  
  .form-group.small,
  .form-group.large {
    flex: auto;
  }
}
```

### Diagonal Layout com Flexbox

```css
.diagonal-layout {
  display: flex;
  height: 300px;
}

.diagonal-item {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
}

/* Cada item desloca mais que o anterior */
.diagonal-item:nth-child(1) { align-self: flex-start; }
.diagonal-item:nth-child(2) { align-self: center; }
.diagonal-item:nth-child(3) { align-self: flex-end; }
```

### Split Screen com Flexbox

```css
.split-screen {
  display: flex;
  height: 100vh;
}

.split-left,
.split-right {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
}

/* Divisão assimétrica */
.split-left {
  flex: 0 0 60%; /* 60% da largura */
  background-color: #f5f5f5;
}

.split-right {
  flex: 1; /* Resto do espaço */
  background-color: #333;
  color: white;
}

@media (max-width: 768px) {
  .split-screen {
    flex-direction: column;
  }
  
  .split-left {
    flex: 0 0 50vh; /* Metade da altura da viewport */
  }
}
```

## Flexbox vs. Grid

- **Use Flexbox para:**
    - Layouts unidimensionais (linha OU coluna)
    - Quando o conteúdo deve ditar o tamanho
    - Distribuição de espaço entre itens
    - Alinhamento preciso de itens
- **Use Grid para:**
    - Layouts bidimensionais (linhas E colunas)
    - Quando o layout deve ditar o conteúdo
    - Posicionamento preciso em duas dimensões
    - Sobreposição de elementos

## Desafios Comuns e Soluções

### Larguras Iguais Independente do Conteúdo

```css
.equal-width {
  display: flex;
}

.equal-width > * {
  flex: 1;
}
```

### Último Item na Direita

```css
.navbar {
  display: flex;
}

.spacer {
  flex: 1; /* Empurra o próximo item para a direita */
}
```

### Evitando que Itens Encolham Abaixo do Conteúdo

```css
.no-shrink {
  flex-shrink: 0; /* Não encolhe abaixo do tamanho do conteúdo */
}
```

### Distribução Automática em Múltiplas Linhas

```css
.auto-grid {
  display: flex;
  flex-wrap: wrap;
}

.auto-grid-item {
  flex: 1 1 300px; /* Cresce, encolhe, base mínima */
}
```