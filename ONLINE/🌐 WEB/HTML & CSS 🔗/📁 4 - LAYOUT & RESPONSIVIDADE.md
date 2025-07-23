
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

htmlresponse-action-icon

```html
<picture>
  <source media="(min-width: 1200px)" srcset="imagem-desktop.jpg">
  <source media="(min-width: 768px)" srcset="imagem-tablet.jpg">
  <img src="imagem-mobile.jpg" alt="Descrição da imagem">
</picture>
```

## Tabelas Responsivas

### Abordagem com Overflow

cssresponse-action-icon

```css
.table-container {
  overflow-x: auto;
}
```

htmlresponse-action-icon

```html
<div class="table-container">
  <table>
    <!-- Conteúdo da tabela -->
  </table>
</div>
```

### Reorganização para Mobile

cssresponse-action-icon

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

htmlresponse-action-icon

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

cssresponse-action-icon

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

htmlresponse-action-icon

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