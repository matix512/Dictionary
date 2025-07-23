
## ==Visão Geral dos Frameworks CSS==

### O que são Frameworks CSS?
Frameworks CSS são bibliotecas pré-escritas de CSS (e às vezes JavaScript) que fornecem componentes, layouts e utilidades prontos para usar, acelerando o desenvolvimento web e garantindo consistência.

### Vantagens e Desvantagens

#### Vantagens
- **Desenvolvimento Rápido**: Componentes prontos para usar
- **Responsividade**: Sistemas de grid e layouts responsivos integrados
- **Consistência**: Aparência e comportamento padronizados
- **Testados em Diversos Navegadores**: Compatibilidade garantida
- **Comunidade e Documentação**: Amplo suporte e recursos

#### Desvantagens
- **Código Excessivo**: Inclui mais do que você geralmente precisa
- **Curva de Aprendizado**: Requer tempo para aprender convenções específicas
- **Estilo Genérico**: Sites podem parecer semelhantes
- **Customização Limitada**: Modificar componentes pode ser desafiador
- **Dependência de Classes CSS**: Markup HTML frequentemente carregado de classes

## Bootstrap

### Visão Geral
Bootstrap é um dos frameworks CSS mais populares, desenvolvido pelo Twitter. Oferece design responsivo, mobile-first e uma ampla gama de componentes.

### Instalação

```html
<!-- CSS apenas -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">

<!-- JavaScript Bundle com Popper (necessário para componentes interativos) -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
````

### Sistema de Grid

```html
<div class="container">
  <div class="row">
    <div class="col-md-6 col-lg-4">Coluna 1</div>
    <div class="col-md-6 col-lg-4">Coluna 2</div>
    <div class="col-md-12 col-lg-4">Coluna 3</div>
  </div>
</div>
```

### Componentes Comuns

```html
<!-- Botões -->
<button class="btn btn-primary">Primário</button>
<button class="btn btn-secondary">Secundário</button>
<button class="btn btn-success">Sucesso</button>
<button class="btn btn-danger">Perigo</button>
<button class="btn btn-warning">Alerta</button>
<button class="btn btn-info">Info</button>

<!-- Cards -->
<div class="card" style="width: 18rem;">
  <img src="..." class="card-img-top" alt="...">
  <div class="card-body">
    <h5 class="card-title">Título do Card</h5>
    <p class="card-text">Conteúdo do card.</p>
    <a href="#" class="btn btn-primary">Botão</a>
  </div>
</div>

<!-- Navbar -->
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">Brand</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNav">
      <ul class="navbar-nav">
        <li class="nav-item">
          <a class="nav-link active" href="#">Home</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Recursos</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Preços</a>
        </li>
      </ul>
    </div>
  </div>
</nav>
```

### Utilitários

```html
<!-- Espaçamento (margin e padding) -->
<div class="mt-4 mb-2 ms-3 me-3 p-3">
  <!-- m: margin, p: padding -->
  <!-- t: top, b: bottom, s: start, e: end, x: horizontal, y: vertical -->
  <!-- Tamanhos de 0 a 5 -->
</div>

<!-- Texto -->
<p class="text-primary text-center fw-bold fs-4">Texto formatado</p>

<!-- Display -->
<div class="d-none d-md-block">Visível apenas em médio ou maior</div>

<!-- Flex -->
<div class="d-flex justify-content-between align-items-center">
  <div>Item 1</div>
  <div>Item 2</div>
</div>
```

### Personalização
```scss
// Usando Sass para personalizar
$primary: #0074d9;
$danger: #ff4136;

$theme-colors: (
  "primary": $primary,
  "danger": $danger
);

// Importar Bootstrap depois de definir as variáveis
@import "bootstrap/scss/bootstrap";
```

## Tailwind CSS

### Visão Geral

Tailwind CSS é um framework utilitário que permite construir designs personalizados diretamente no HTML usando classes de utilidade predefinidas.

### Instalação

```html
<!-- Via CDN (não recomendado para produção) -->
<link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
```

Para projetos reais, é recomendada a instalação via npm e configuração com PostCSS.

### Filosofia Utility-First

```html
<!-- Em vez de estilos semânticos como -->
<button class="btn-primary">Botão</button>

<!-- Tailwind usa classes utilitárias -->
<button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
  Botão
</button>
```

### Grid e Layout

```html
<!-- Grid simples de 3 colunas -->
<div class="grid grid-cols-3 gap-4">
  <div>Item 1</div>
  <div>Item 2</div>
  <div>Item 3</div>
</div>

<!-- Grid responsivo -->
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
  <div>Item 1</div>
  <div>Item 2</div>
  <div>Item 3</div>
</div>

<!-- Flexbox -->
<div class="flex justify-between items-center">
  <div>Esquerda</div>
  <div>Centro</div>
  <div>Direita</div>
</div>
```

### Componentes Comuns

```html
<!-- Card -->
<div class="max-w-sm rounded overflow-hidden shadow-lg">
  <img class="w-full" src="..." alt="Imagem">
  <div class="px-6 py-4">
    <div class="font-bold text-xl mb-2">Título do Card</div>
    <p class="text-gray-700 text-base">
      Conteúdo do card.
    </p>
  </div>
  <div class="px-6 pt-4 pb-2">
    <span class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">#tag1</span>
    <span class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">#tag2</span>
  </div>
</div>

<!-- Botão -->
<button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline">
  Botão
</button>

<!-- Alerta -->
<div class="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded relative" role="alert">
  <strong class="font-bold">Erro!</strong>
  <span class="block sm:inline">Algo deu errado.</span>
</div>
```

### Personalização

No arquivo tailwind.config.js:

```javascript
module.exports = {
  theme: {
    extend: {
      colors: {
        'brand-blue': '#0074d9',
        'brand-red': '#ff4136',
      },
      spacing: {
        '72': '18rem',
        '84': '21rem',
        '96': '24rem',
      },
      fontFamily: {
        'sans': ['Roboto', 'sans-serif'],
        'heading': ['Montserrat', 'sans-serif'],
      },
    }
  },
  variants: {
    extend: {
      backgroundColor: ['active'],
      textColor: ['visited'],
    }
  },
  plugins: [],
}
```

### Classes Responsivas

```html
<div class="text-sm md:text-base lg:text-lg">
  Texto que muda de tamanho em diferentes breakpoints
</div>

<div class="hidden md:block">
  Visível apenas em tamanhos médios e acima
</div>
```

## Bulma

### Visão Geral

Bulma é um framework CSS moderno baseado em Flexbox, sem JavaScript, modular e com sintaxe legível.

### Instalação

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bulma@0.9.4/css/bulma.min.css">
```

### Componentes e Layout

```html
<!-- Colunas -->
<div class="columns">
  <div class="column">Primeira coluna</div>
  <div class="column">Segunda coluna</div>
  <div class="column">Terceira coluna</div>
</div>

<!-- Colunas responsivas -->
<div class="columns is-mobile">
  <div class="column is-half-mobile is-one-third-tablet is-one-quarter-desktop">
    Coluna responsiva
  </div>
</div>

<!-- Card -->
<div class="card">
  <div class="card-image">
    <figure class="image is-4by3">
      <img src="..." alt="Imagem">
    </figure>
  </div>
  <div class="card-content">
    <div class="media">
      <div class="media-content">
        <p class="title is-4">Título do Card</p>
      </div>
    </div>
    <div class="content">
      Conteúdo do card.
    </div>
  </div>
</div>

<!-- Navbar -->
<nav class="navbar" role="navigation" aria-label="main navigation">
  <div class="navbar-brand">
    <a class="navbar-item" href="#">
      <img src="logo.png" width="112" height="28">
    </a>
    <a role="button" class="navbar-burger" aria-label="menu" aria-expanded="false">
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
    </a>
  </div>
  <div class="navbar-menu">
    <div class="navbar-start">
      <a class="navbar-item">Home</a>
      <a class="navbar-item">Documentação</a>
    </div>
  </div>
</nav>
```

## Foundation

### Visão Geral

Foundation é um framework front-end responsivo, mobile-first, altamente personalizável e com foco em acessibilidade.

### Instalação

```html
<!-- CSS -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/foundation-sites@6.7.5/dist/css/foundation.min.css">

<!-- JavaScript -->
<script src="https://cdn.jsdelivr.net/npm/jquery@3.6.0/dist/jquery.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/foundation-sites@6.7.5/dist/js/foundation.min.js"></script>
```

### Grid e Componentes

```html
<!-- Grid responsivo -->
<div class="grid-container">
  <div class="grid-x grid-margin-x">
    <div class="cell small-12 medium-6 large-4">Célula 1</div>
    <div class="cell small-12 medium-6 large-4">Célula 2</div>
    <div class="cell small-12 medium-12 large-4">Célula 3</div>
  </div>
</div>

<!-- Botões -->
<button class="button">Padrão</button>
<button class="button primary">Primário</button>
<button class="button secondary">Secundário</button>
<button class="button success">Sucesso</button>
<button class="button alert">Alerta</button>

<!-- Card -->
<div class="card">
  <div class="card-divider">
    Cabeçalho
  </div>
  <img src="...">
  <div class="card-section">
    <h4>Título</h4>
    <p>Conteúdo do card.</p>
  </div>
</div>
```

## Material UI (para React)

### Visão Geral

Material UI é uma biblioteca de componentes React que implementa o Material Design do Google. Embora seja primariamente para React, é importante conhecê-lo como parte do ecossistema de frameworks de design.

### Instalação

```bash
npm install @mui/material @emotion/react @emotion/styled
```

### Exemplo de Uso

```jsx
import React from 'react';
import Button from '@mui/material/Button';
import Card from '@mui/material/Card';
import CardContent from '@mui/material/CardContent';
import Typography from '@mui/material/Typography';

function App() {
  return (
    <div>
      <Button variant="contained" color="primary">
        Botão Material
      </Button>
      
      <Card sx={{ minWidth: 275, marginTop: 2 }}>
        <CardContent>
          <Typography variant="h5" component="div">
            Título do Card
          </Typography>
          <Typography variant="body2">
            Conteúdo do card em Material UI.
          </Typography>
        </CardContent>
      </Card>
    </div>
  );
}
```

## Como Escolher um Framework

### Fatores a Considerar

1. **Tamanho do Projeto**: Projetos menores podem se beneficiar de frameworks mais leves
2. **Curva de Aprendizado**: Quanto tempo você tem para aprender?
3. **Personalização**: Quão único seu design precisa ser?
4. **Desempenho**: Considere o tamanho do framework e seu impacto no carregamento
5. **Suporte a Navegadores**: Verifique a compatibilidade com navegadores antigos
6. **Comunidade e Manutenção**: Frameworks ativamente mantidos têm menos bugs

### Tabela Comparativa

|Framework|Tamanho (min+gzip)|Abordagem|Pontos Fortes|Melhor Para|
|---|---|---|---|---|
|Bootstrap|~60KB|Componentes|Pronto para usar, documentação|Protótipos, projetos corporativos|
|Tailwind|~10KB (purged)|Utilitário|Flexível, personalizável|Designs únicos, desenvolvedores experientes|
|Bulma|~30KB|Componentes|Baseado em Flexbox, legível|Projetos médios, fácil aprendizado|
|Foundation|~40KB|Componentes|Acessibilidade, personalização|Projetos empresariais, acessibilidade|

## Dicas e Boas Práticas

### Otimização

1. **Carregue apenas o necessário**: A maioria dos frameworks permite importar apenas os componentes utilizados
2. **Tree-shaking**: Use ferramentas de bundling como Webpack para eliminar código não utilizado
3. **Purgamento de CSS**: Remova classes não utilizadas (especialmente importante com Tailwind)
4. **Lazy loading**: Carregue componentes JavaScript sob demanda
5. **Minificação**: Sempre use versões minificadas em produção

### Personalização Eficiente
1. **Evite sobrescrever**: Modifique variáveis quando possível em vez de sobrescrever CSS
2. **Mantenha um arquivo separado**: Coloque suas personalizações em um arquivo separado
3. **Use pré-processadores**: Sass/SCSS facilitam a personalização de frameworks
4. **Mantenha a consistência**: Crie um sistema de design mesmo ao usar um framework

### Acessibilidade
1. **Não confie cegamente**: Teste componentes para garantir acessibilidade
2. **Melhore quando necessário**: Adicione atributos ARIA e melhore a acessibilidade
3. **Contraste**: Verifique se suas cores personalizadas mantêm contraste adequado
4. **Navegação por teclado**: Teste componentes interativos com navegação por teclado

### Armadilhas Comuns
1. **Excesso de classes**: Evite div-itis (excesso de divs) e class-itis (markup sobrecarregado)
2. **Dependência excessiva**: Não deixe o framework determinar seu design
3. **Misturar frameworks**: Evite usar múltiplos frameworks CSS juntos
4. **Versões obsoletas**: Mantenha-se atualizado para correções de segurança e novos recursos

---


# 📄 19 - PRÉ-PROCESSADORES CSS


# Pré-processadores CSS

## Introdução aos Pré-processadores

### O que são Pré-processadores CSS?
Pré-processadores CSS são ferramentas que estendem as capacidades do CSS com funcionalidades como variáveis, aninhamento, mixins, funções e mais. Eles permitem escrever código mais limpo, organizado e reutilizável, que é então compilado para CSS padrão que os navegadores podem entender.

### Principais Benefícios
1. **Variáveis**: Armazene valores para reutilização e fácil atualização
2. **Aninhamento**: Escreva seletores de forma hierárquica e organizada
3. **Mixins e Funções**: Reutilize blocos de código e crie lógica
4. **Modularização**: Divida seu CSS em arquivos menores e organize-os
5. **Operações Matemáticas**: Realize cálculos diretamente no CSS
6. **Extensões/Herança**: Compartilhe propriedades entre seletores

### Principais Pré-processadores
- **Sass/SCSS**: O mais popular e maduro
- **Less**: Simples e semelhante ao CSS
- **Stylus**: Sintaxe flexível e poderosa
- **PostCSS**: Transformador de CSS com plugins

## SASS/SCSS

### Diferenças entre Sass e SCSS
- **Sass (Syntactically Awesome Style Sheets)**: Sintaxe indentada sem chaves ou ponto e vírgula
- **SCSS (Sassy CSS)**: Sintaxe mais próxima do CSS, usando chaves e ponto e vírgula

```scss
// Sass (sintaxe indentada)
nav
  ul
    margin: 0
    padding: 0
    list-style: none
  li
    display: inline-block
  a
    display: block
    padding: 6px 12px
    text-decoration: none

// SCSS (sintaxe semelhante ao CSS)
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }
  li {
    display: inline-block;
  }
  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}
````

### Instalação e Uso

#### Usando Node.js/npm

```bash
# Instalar Sass globalmente
npm install -g sass

# Compilar arquivo
sass input.scss output.css

# Compilar e assistir mudanças
sass --watch input.scss:output.css

# Compilar diretório e assistir mudanças
sass --watch scss/:css/
```

#### Usando aplicativos GUI

- **Prepros**: Interface amigável para compilar pré-processadores
- **Koala**: Aplicativo GUI para Less, Sass, Compass e CoffeeScript
- **Scout-App**: Aplicativo Sass/Compass multiplataforma

### Variáveis

```scss
// Definindo variáveis
$primary-color: #3498db;
$secondary-color: #2ecc71;
$font-stack: 'Helvetica', Arial, sans-serif;
$base-spacing: 16px;

// Usando variáveis
body {
  font-family: $font-stack;
  color: $primary-color;
  padding: $base-spacing;
}

h1, h2, h3 {
  color: $secondary-color;
  margin-bottom: $base-spacing * 1.5;
}
```

### Aninhamento

```scss
// Aninhamento básico
.container {
  max-width: 1200px;
  margin: 0 auto;
  
  header {
    background-color: #f8f8f8;
    padding: 20px;
    
    h1 {
      margin: 0;
      color: #333;
    }
  }
  
  .content {
    padding: 20px;
    
    p {
      line-height: 1.6;
      
      &:first-child {
        font-weight: bold;
      }
    }
  }
}

// Referência ao seletor pai (&)
.btn {
  padding: 10px 15px;
  background: blue;
  color: white;
  
  &:hover {
    background: darkblue;
  }
  
  &.btn-large {
    padding: 15px 25px;
    font-size: 18px;
  }
  
  &--primary {
    background: green;
  }
  
  &--secondary {
    background: gray;
  }
}
```

### Partials e @import

```scss
// _variables.scss (partial)
$primary-color: #3498db;
$secondary-color: #2ecc71;

// _mixins.scss (partial)
@mixin flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

// main.scss
@import 'variables';
@import 'mixins';

.container {
  background-color: $primary-color;
  @include flex-center;
}
```

### Mixins

```scss
// Definindo mixins simples
@mixin border-radius($radius) {
  -webkit-border-radius: $radius;
  -moz-border-radius: $radius;
  border-radius: $radius;
}

// Mixin com valores padrão
@mixin box-shadow($x: 0, $y: 2px, $blur: 4px, $color: rgba(0,0,0,0.2)) {
  -webkit-box-shadow: $x $y $blur $color;
  -moz-box-shadow: $x $y $blur $color;
  box-shadow: $x $y $blur $color;
}

// Mixin com conteúdo passado
@mixin media-query($breakpoint) {
  @if $breakpoint == small {
    @media (max-width: 767px) { @content; }
  } @else if $breakpoint == medium {
    @media (min-width: 768px) and (max-width: 1023px) { @content; }
  } @else if $breakpoint == large {
    @media (min-width: 1024px) { @content; }
  }
}

// Usando mixins
.box {
  @include border-radius(5px);
  @include box-shadow(0, 3px, 10px, rgba(0,0,0,0.3));
  
  @include media-query(small) {
    padding: 10px;
    font-size: 14px;
  }
  
  @include media-query(large) {
    padding: 20px;
    font-size: 16px;
  }
}
```

### Extensão/Herança

```scss
// Definindo um placeholder
%button-base {
  display: inline-block;
  padding: 10px 15px;
  border: none;
  cursor: pointer;
  font-size: 16px;
  border-radius: 4px;
}

// Usando extensão
.button-primary {
  @extend %button-base;
  background-color: #3498db;
  color: white;
  
  &:hover {
    background-color: darken(#3498db, 10%);
  }
}

.button-secondary {
  @extend %button-base;
  background-color: #f1f1f1;
  color: #333;
  
  &:hover {
    background-color: darken(#f1f1f1, 10%);
  }
}
```

### Funções e Operações

```scss
// Funções nativas
$base-color: #3498db;

.element {
  // Manipulação de cores
  color: $base-color;
  background-color: lighten($base-color, 15%);
  border-color: darken($base-color, 15%);
  box-shadow: 0 2px 5px rgba($base-color, 0.5);
  
  // Operações matemáticas
  $base-padding: 10px;
  padding: $base-padding $base-padding * 2;
  margin: $base-padding / 2;
  width: calc(100% - #{$base-padding * 4});
}

// Funções personalizadas
@function rem($pixels) {
  @return $pixels / 16px * 1rem;
}

.text {
  font-size: rem(24);  // Converte 24px para rem
}
```

### Diretivas de Controle

```scss
// Condicionais (@if, @else if, @else)
@mixin text-color($bg-color) {
  @if lightness($bg-color) > 50% {
    color: #000;
  } @else {
    color: #fff;
  }
}

.light-bg {
  background-color: #f8f8f8;
  @include text-color(#f8f8f8);  // Aplicará color: #000
}

.dark-bg {
  background-color: #333;
  @include text-color(#333);     // Aplicará color: #fff
}

// Loops (@for, @each, @while)
@for $i from 1 through 5 {
  .col-#{$i} {
    width: 20% * $i;
  }
}

$sizes: (small: 12px, medium: 16px, large: 24px);
@each $name, $size in $sizes {
  .text-#{$name} {
    font-size: $size;
  }
}

$i: 1;
@while $i <= 5 {
  .item-#{$i} {
    z-index: 10 - $i;
  }
  $i: $i + 1;
}
```

## LESS

### Instalação e Uso

#### Usando Node.js/npm

```bash
# Instalar Less globalmente
npm install -g less

# Compilar arquivo
lessc input.less output.css
```

### Variáveis

```less
// Definindo variáveis
@primary-color: #3498db;
@secondary-color: #2ecc71;
@font-stack: 'Helvetica', Arial, sans-serif;
@base-spacing: 16px;

// Usando variáveis
body {
  font-family: @font-stack;
  color: @primary-color;
  padding: @base-spacing;
}

// Variáveis como seletores
@mySelector: banner;
.@{mySelector} {
  background: #f0f0f0;
}
```

### Aninhamento

```less
// Aninhamento básico
.container {
  max-width: 1200px;
  margin: 0 auto;
  
  header {
    background-color: #f8f8f8;
    
    h1 {
      margin: 0;
    }
  }
  
  // Referência ao seletor pai (&)
  .btn {
    padding: 10px 15px;
    
    &:hover {
      background: darken(@primary-color, 10%);
    }
    
    &.large {
      font-size: 18px;
    }
  }
}
```

### Mixins

```less
// Mixin básico
.border-radius(@radius) {
  -webkit-border-radius: @radius;
  -moz-border-radius: @radius;
  border-radius: @radius;
}

// Mixin com parâmetros padrão
.box-shadow(@x: 0, @y: 2px, @blur: 4px, @color: rgba(0,0,0,0.2)) {
  -webkit-box-shadow: @x @y @blur @color;
  -moz-box-shadow: @x @y @blur @color;
  box-shadow: @x @y @blur @color;
}

// Usando mixins
.box {
  .border-radius(5px);
  .box-shadow(0, 3px, 10px, rgba(0,0,0,0.3));
}

// Mixins como funções
.average(@a, @b) {
  @result: ((@a + @b) / 2);
}

.calc-size {
  .average(16px, 24px);  // @result é calculado
  font-size: @result;    // Usa o valor calculado
}
```

### Operações e Funções

```less
@base: 5px;
@color: #888;

.element {
  // Operações
  padding: @base * 2;
  margin: @base + 2px;
  
  // Funções de cor
  color: lighten(@color, 20%);
  background-color: darken(@color, 10%);
  border: 1px solid fadein(@color, 10%);
}
```

### Importação

```less
// Importando arquivos
@import "variables.less";
@import "mixins.less";
```

### Diferenças em relação ao Sass

- Usa @ em vez de $ para variáveis
- Os mixins são usados como classes
- Não há placeholders (como %placeholder no Sass)
- Não tem diretivas como @each ou @for

## Stylus

### Instalação e Uso

#### Usando Node.js/npm

```bash
# Instalar Stylus globalmente
npm install -g stylus

# Compilar arquivo
stylus input.styl -o output.css

# Compilar e assistir mudanças
stylus -w input.styl -o output.css
```

### Sintaxe Flexível

```stylus
// Sem chaves, ponto e vírgula ou dois-pontos (opcional)
.container
  max-width 1200px
  margin 0 auto
  
  // Aninhamento
  header
    background-color #f8f8f8
    padding 20px
    
    h1
      margin 0
      color #333

// Também suporta sintaxe padrão CSS
.example {
  color: red;
  background: blue;
}
```

### Variáveis

```stylus
// Definindo variáveis
primary-color = #3498db
secondary-color = #2ecc71
font-stack = 'Helvetica', Arial, sans-serif

// Usando variáveis
body
  font-family font-stack
  color primary-color
```

### Mixins

```stylus
// Definindo mixin
border-radius(radius = 5px)
  -webkit-border-radius radius
  -moz-border-radius radius
  border-radius radius

// Usando mixin
.button
  border-radius(3px)
  
.bubble
  border-radius(50%)

// Mixins com blocos
media-query(breakpoint)
  if breakpoint == small
    @media (max-width: 767px)
      {block}
  else if breakpoint == large
    @media (min-width: 1024px)
      {block}

// Uso do mixin com bloco
+media-query(small)
  .container
    padding 10px
```

### Funções

```stylus
// Função simples
subtract(a, b)
  a - b

// Usando a função
.element
  margin subtract(20px, 5px)

// Condicionais
light-or-dark(color)
  if lightness(color) > 50%
    return #000
  else
    return #fff

// Usando a função condicional
.box
  background-color #f8f8f8
  color light-or-dark(#f8f8f8)
```

### Iteração

```stylus
// For loop
for i in (1..5)
  .col-{i}
    width (i * 20%)

// Iteração sobre lista
sizes = {
  small: 12px,
  medium: 16px,
  large: 24px
}

for key, value in sizes
  .text-{key}
    font-size value
```

## PostCSS

### O que é PostCSS?

PostCSS é uma ferramenta para transformar CSS com plugins JavaScript. Diferente dos pré-processadores tradicionais, PostCSS é modular e permite que você escolha apenas as funcionalidades que precisa.

### Plugins Populares

- **Autoprefixer**: Adiciona prefixos de navegador automaticamente
- **postcss-preset-env**: Converte CSS moderno para compatibilidade com navegadores
- **cssnano**: Minifica e otimiza CSS
- **postcss-nested**: Suporte para regras aninhadas como Sass
- **postcss-import**: Resolve e junta imports de @import
- **postcss-simple-vars**: Suporte para variáveis simples
- **postcss-mixins**: Suporte para mixins

### Instalação e Configuração

#### Usando Node.js/npm
```bash
# Instalar PostCSS CLI e plugins
npm install --save-dev postcss postcss-cli autoprefixer postcss-preset-env postcss-nested

# Criar arquivo de configuração (postcss.config.js)
module.exports = {
  plugins: [
    require('postcss-import'),
    require('postcss-nested'),
    require('postcss-preset-env')({ stage: 1 }),
    require('autoprefixer')
  ]
}

# Compilar CSS
postcss input.css -o output.css
````

### Exemplo de Uso

```css
/* Usando variáveis com postcss-simple-vars */
$primary-color: #3498db;
$secondary-color: #2ecc71;

/* Usando aninhamento com postcss-nested */
.container {
  max-width: 1200px;
  margin: 0 auto;
  
  & header {
    background-color: $primary-color;
    color: white;
    
    & h1 {
      margin: 0;
    }
  }
}

/* Usando recursos CSS modernos com postcss-preset-env */
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 1rem;
}

.box {
  background-color: hsl(210 50% 50% / 80%);
  color: white;
}
```

## Organização e Metodologias CSS

### Estrutura de Arquivos Recomendada (7-1 Pattern)

```text
scss/
|-- abstracts/         # Variáveis, mixins, funções
|   |-- _variables.scss
|   |-- _mixins.scss
|   |-- _functions.scss
|
|-- base/              # Estilos base, reset, tipografia
|   |-- _reset.scss
|   |-- _typography.scss
|   |-- _animations.scss
|
|-- components/        # Componentes reutilizáveis
|   |-- _buttons.scss
|   |-- _cards.scss
|   |-- _forms.scss
|
|-- layout/            # Estruturas maiores
|   |-- _header.scss
|   |-- _navigation.scss
|   |-- _grid.scss
|   |-- _footer.scss
|
|-- pages/             # Estilos específicos de páginas
|   |-- _home.scss
|   |-- _about.scss
|   |-- _contact.scss
|
|-- themes/            # Diferentes temas
|   |-- _default.scss
|   |-- _dark.scss
|
|-- vendors/           # CSS de terceiros
|   |-- _bootstrap.scss
|   |-- _jquery-ui.scss
|
|-- main.scss          # Arquivo principal que importa todos os outros
```

### BEM (Block Element Modifier)

```scss
// Block (bloco): componente independente
.card {
  background: white;
  border-radius: 4px;
  
  // Element (elemento): parte de um bloco
  &__title {
    font-size: 18px;
    font-weight: bold;
  }
  
  &__content {
    padding: 15px;
  }
  
  &__image {
    width: 100%;
  }
  
  // Modifier (modificador): variação do bloco ou elemento
  &--featured {
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
  }
  
  &__title--large {
    font-size: 24px;
  }
}

// Uso no HTML:
// <div class="card card--featured">
//   <h2 class="card__title card__title--large">Título</h2>
//   <div class="card__content">Conteúdo</div>
// </div>
```

### SMACSS (Scalable and Modular Architecture for CSS)

```scss
// Base
body, h1, p {
  margin: 0;
  padding: 0;
}

// Layout
.l-header {
  height: 80px;
}

.l-sidebar {
  width: 250px;
}

// Módulos
.btn {
  // estilos base
  
  &.is-active {
    // estado ativo
  }
}

// Estado
.is-hidden {
  display: none;
}

.is-expanded {
  height: auto;
}

// Tema
.theme-dark {
  background: #333;
  color: white;
}
```

## Guia de Melhores Práticas

### Desempenho e Manutenção

1. **Evite aninhamento excessivo**: Limite a 3 níveis para melhor desempenho e legibilidade
        
    ```scss
    // Ruim
    .container {
      .header {
        .navigation {
          .menu {
            .item {
              a {
                color: red;
              }
            }
          }
        }
      }
    }
    
    // Melhor
    .container .header .navigation {
      .menu-item a {
        color: red;
      }
    }
    
    // Ou melhor ainda
    .nav-link {
      color: red;
    }
    ```
    
2. **Use variáveis para valores repetidos**:
        
    ```scss
    // Defina e use variáveis para cores, espaçamentos, fontes
    $primary-color: #3498db;
    $spacing-unit: 8px;
    
    .element {
      color: $primary-color;
      margin: $spacing-unit * 2;
    }
    ```
    
3. **Modularize seu código**:
    
    ```scss
    // Divida em arquivos menores e importe-os
    @import 'variables';
    @import 'reset';
    @import 'typography';
    @import 'buttons';
    @import 'forms';
    ```
    
4. **Prefira extend/herança com moderação**:
    
    ```scss
    // Extender pode gerar CSS bloat se usado em excesso
    // Use mixins para blocos repetidos com variáveis
    @mixin button($bg-color) {
      display: inline-block;
      padding: 10px 15px;
      background-color: $bg-color;
      border-radius: 4px;
    }
    
    .primary-button {
      @include button(blue);
    }
    
    .secondary-button {
      @include button(gray);
    }
    ```
    

### Dicas Avançadas

1. **Use maps para conjuntos de valores relacionados**:
        
    ```scss
    // Mapa de cores
    $colors: (
      'primary': #3498db,
      'secondary': #2ecc71,
      'danger': #e74c3c,
      'warning': #f39c12,
      'info': #1abc9c
    );
    
    // Função para acessar o mapa
    @function color($key) {
      @return map-get($colors, $key);
    }
    
    // Uso
    .alert {
      background-color: color('warning');
    }
    
    .button-danger {
      background-color: color('danger');
    }
    ```
    
2. **Crie mixins para media queries**:
        
    ```scss
    // Definindo breakpoints
    $breakpoints: (
      'small': 576px,
      'medium': 768px,
      'large': 992px,
      'xlarge': 1200px
    );
    
    // Mixin de media query
    @mixin media-breakpoint-up($breakpoint) {
      $min-width: map-get($breakpoints, $breakpoint);
      @if $min-width {
        @media (min-width: $min-width) {
          @content;
        }
      } @else {
        @error "Breakpoint '#{$breakpoint}' não encontrado.";
      }
    }
    
    // Uso
    .container {
      padding: 15px;
      
      @include media-breakpoint-up('medium') {
        padding: 30px;
      }
      
      @include media-breakpoint-up('large') {
        max-width: 960px;
        margin: 0 auto;
      }
    }
    ```
    
3. **Utilização de funções para cálculos**:
    
    ```scss
    // Função para converter px para rem
    @function rem($px, $base: 16px) {
      @return ($px / $base) * 1rem;
    }
    
    // Função para calcular porcentagens
    @function percentage-width($target, $container) {
      @return ($target / $container) * 100%;
    }
    
    // Uso
    .heading {
      font-size: rem(24px);
      margin-bottom: rem(16px);
    }
    
    .sidebar {
      width: percentage-width(300px, 1200px);  // 25%
    }
    ```
    
4. **Estilos condicionais com mixins**:
    
    ```scss
    // Mixin para gerar variantes de componentes
    @mixin theme($theme: 'light') {
      @if $theme == 'light' {
        background-color: white;
        color: #333;
      } @else if $theme == 'dark' {
        background-color: #333;
        color: white;
      } @else if $theme == 'primary' {
        background-color: $primary-color;
        color: white;
      }
    }
    
    // Uso
    .card {
      @include theme('light');
      
      &--dark {
        @include theme('dark');
      }
      
      &--primary {
        @include theme('primary');
      }
    }
    ```

### Comparação Final dos Pré-processadores

|Característica|Sass/SCSS|Less|Stylus|PostCSS|
|---|---|---|---|---|
|Sintaxe|Indentada (Sass) ou CSS-like (SCSS)|CSS-like|Flexível, opcional|CSS padrão|
|Variáveis|$var|@var|var|--var ou plugins|
|Aninhamento|Sim|Sim|Sim|Com plugin|
|Mixins|@mixin, @include|.mixin()|mixin()|Com plugin|
|Herança|@extend|N/A|@extend|Com plugin|
|Funções|Sim|Sim|Sim|Com plugin|
|Condicionais|@if, @else|guard mixins|if/else|Com plugin|
|Loops|@for, @each, @while|recursão|for/each|Com plugin|
|Popularidade|Alta|Média|Baixa|Alta|
|Curva de aprendizado|Média|Baixa|Média-Alta|Baixa|
|Customização|Boa|Boa|Excelente|Extremamente flexível|
|Quando usar|Projetos grandes|Projetos simples|Desenvolvedores avançados|Personalização específica|





---



# 📄 20 - FERRAMENTAS DE DESENVOLVIMENTO


## Editores de Código

### Visual Studio Code
Visual Studio Code (VS Code) é um editor de código-fonte gratuito e altamente personalizável desenvolvido pela Microsoft.

#### Instalação
1. Acesse [code.visualstudio.com](https://code.visualstudio.com/)
2. Baixe a versão para seu sistema operacional
3. Siga as instruções de instalação

#### Extensões Essenciais para HTML/CSS
- **Live Server**: Servidor local com reload automático
- **HTML CSS Support**: Autocompletar para HTML/CSS
- **IntelliSense for CSS**: Autocomplete e informações para classes e IDs
- **HTML Snippets**: Snippets para HTML5
- **CSS Peek**: Navegar de seletores HTML para definições CSS
- **Prettier**: Formatador de código
- **Auto Rename Tag**: Renomeia tags de fechamento automaticamente
- **Live Sass Compiler**: Compila arquivos Sass/SCSS em tempo real
- **Color Highlight**: Destaca cores no código

#### Atalhos Úteis
## Windows/Linux Mac Ação

Ctrl+S Cmd+S Salvar  
Ctrl+C, Ctrl+V Cmd+C, Cmd+V Copiar, Colar  
Ctrl+Z, Ctrl+Y Cmd+Z, Cmd+Shift+Z Desfazer, Refazer  
Ctrl+F Cmd+F Procurar  
Ctrl+H Cmd+Option+F Substituir  
Alt+Up/Down Option+Up/Down Mover linha para cima/baixo  
Shift+Alt+Down Shift+Option+Down Duplicar linha  
Ctrl+/ Cmd+/ Comentar linha  
Ctrl+Space Cmd+Space Sugestões de código  
Ctrl+Shift+P Cmd+Shift+P Abrir paleta de comandos  
Alt+Click Option+Click Múltiplos cursores

### Sublime Text
Editor de código leve e rápido com suporte para plugins.

#### Instalação
1. Acesse [sublimetext.com](https://www.sublimetext.com/)
2. Baixe e instale conforme instruções

#### Pacotes Recomendados
- **Package Control**: Gerenciador de pacotes
- **Emmet**: Atalhos para HTML/CSS
- **HTML5**: Snippets e autocompletar para HTML5
- **Color Highlighter**: Destaca cores no código
- **AutoFileName**: Completa nomes de arquivos
- **SideBarEnhancements**: Melhora a barra lateral

### Atom
Editor de código desenvolvido pelo GitHub (descontinuado, mas ainda usado).

### Brackets
Editor focado em web design desenvolvido pela Adobe.

## Browsers e DevTools

### Chrome DevTools
Ferramentas de desenvolvimento integradas ao Google Chrome.

#### Abrindo o DevTools
- **Windows/Linux**: F12 ou Ctrl+Shift+I ou clique direito > Inspecionar
- **Mac**: Cmd+Option+I ou clique direito > Inspecionar

#### Principais Painéis
1. **Elements/Elementos**: Inspeção e edição de HTML/CSS
   - Visualize e edite a árvore DOM
   - Modifique estilos CSS em tempo real
   - Veja o box model
   - Altere estados (hover, focus, etc.)

2. **Console**: Interação com JavaScript e mensagens
   - Visualize erros e warnings
   - Execute comandos JavaScript
   - Teste snippets de código

3. **Network/Rede**: Monitoramento de requisições
   - Analise tempo de carregamento
   - Veja tamanho dos arquivos
   - Inspecione cabeçalhos e respostas

4. **Performance**: Análise de desempenho
   - Grave e analise performance de carregamento
   - Identifique gargalos

5. **Application/Aplicativo**: Armazenamento e recursos
   - Gerencie cookies, localStorage, sessionStorage
   - Inspecione service workers
   - Gerencie cache

#### Recursos Úteis para HTML/CSS
- **DOM Tree**: Visualize e edite a estrutura HTML
- **Styles Panel**: Edite estilos CSS em tempo real
- **Computed**: Veja os estilos finais aplicados
- **Layout**: Visualize o box model
- **Device Mode**: Teste responsividade em diferentes dispositivos
- **CSS Overview**: Analise o uso de CSS no site (cores, fontes, etc.)
- **Animations**: Controle e depure animações CSS

### Firefox Developer Tools
Ferramentas similares às do Chrome, com alguns recursos exclusivos.

#### Recursos Destacados
- **Grid Inspector**: Visualização avançada de CSS Grid
- **Flexbox Inspector**: Visualização de layouts Flexbox
- **Font Inspector**: Informações detalhadas sobre fontes
- **Responsive Design Mode**: Teste responsivo mais avançado
- **Accessibility Inspector**: Verifica problemas de acessibilidade

### Safari Web Inspector
Ferramentas de desenvolvedor para Safari (Mac/iOS).

### Edge DevTools
Semelhante ao Chrome DevTools, com algumas integrações específicas.

## Ferramentas de Validação e Análise

### W3C Validators
Ferramentas oficiais de validação do W3C.

#### HTML Validator
- Site: [validator.w3.org](https://validator.w3.org/)
- Valida código HTML conforme os padrões web
- Opções para validar por URL, upload ou entrada direta
- Mostra erros e avisos detalhados

#### CSS Validator
- Site: [jigsaw.w3.org/css-validator](https://jigsaw.w3.org/css-validator/)
- Valida código CSS conforme especificações
- Identifica erros de sintaxe e propriedades inválidas

### Lighthouse
Ferramenta de auditoria automatizada para melhorar a qualidade das páginas web.

#### Uso
1. Abra o Chrome DevTools
2. Vá para a aba "Lighthouse"
3. Selecione as categorias (Performance, Acessibilidade, SEO, etc.)
4. Clique em "Generate report"

#### Categorias de Auditoria
- **Performance**: Velocidade e otimização
- **Accessibility**: Acessibilidade
- **Best Practices**: Boas práticas de desenvolvimento
- **SEO**: Otimização para motores de busca
- **PWA**: Recursos de Progressive Web App


````markdown
### Google PageSpeed Insights
- Site: [pagespeed.web.dev](https://pagespeed.web.dev/)
- Análise de desempenho baseada no Lighthouse
- Fornece sugestões específicas para melhorias
- Testa em dispositivos móveis e desktop

### Wave (Web Accessibility Evaluation Tool)
- Site: [wave.webaim.org](https://wave.webaim.org/)
- Avalia a acessibilidade de páginas web
- Identifica erros e avisos
- Mostra problemas visualmente na própria página

### CSS Stats
- Site: [cssstats.com](https://cssstats.com/)
- Analisa estatísticas do CSS
- Mostra uso de cores, fontes, especificidade
- Ajuda a identificar redundâncias e oportunidades de otimização

## Sistemas de Build e Bundlers

### Webpack
Empacotador de módulos JavaScript que também processa HTML, CSS e assets.

#### Instalação Básica
```bash
# Iniciar um projeto npm
npm init -y

# Instalar webpack
npm install --save-dev webpack webpack-cli

# Instalar loaders para CSS
npm install --save-dev css-loader style-loader

# Instalar plugins úteis
npm install --save-dev html-webpack-plugin mini-css-extract-plugin
````

#### Configuração Básica (webpack.config.js)

```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          MiniCssExtractPlugin.loader,
          'css-loader',
        ],
      },
      {
        test: /\.(png|svg|jpg|jpeg|gif)$/i,
        type: 'asset/resource',
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html',
    }),
    new MiniCssExtractPlugin({
      filename: 'styles.css',
    }),
  ],
  devServer: {
    static: './dist',
    hot: true,
  },
};
```

#### Comandos Básicos

```bash
# Construir o projeto
npx webpack

# Iniciar servidor de desenvolvimento
npx webpack serve
```

### Vite

Ferramenta de build mais moderna e rápida, com suporte nativo a ESM.

#### Instalação e Uso

```bash
# Criar um novo projeto
npm create vite@latest my-project -- --template vanilla

# Navegar para o diretório do projeto
cd my-project

# Instalar dependências
npm install

# Iniciar servidor de desenvolvimento
npm run dev

# Construir para produção
npm run build
```

### Parcel

Bundler zero-config que funciona bem para projetos simples.

#### Instalação e Uso

```bash
# Instalar globalmente
npm install -g parcel-bundler

# Ou no projeto
npm install --save-dev parcel

# Executar em modo de desenvolvimento
parcel index.html

# Construir para produção
parcel build index.html
```

### Gulp

Automatizador de tarefas para workflows personalizados.

#### Instalação Básica

```bash
# Instalar globalmente
npm install --global gulp-cli

# Instalar no projeto
npm install --save-dev gulp
```

#### gulpfile.js Básico

```javascript
const gulp = require('gulp');
const sass = require('gulp-sass')(require('sass'));
const autoprefixer = require('gulp-autoprefixer');
const cleanCSS = require('gulp-clean-css');
const browserSync = require('browser-sync').create();

// Compilar Sass
function compileSass() {
  return gulp.src('./src/scss/**/*.scss')
    .pipe(sass().on('error', sass.logError))
    .pipe(autoprefixer())
    .pipe(cleanCSS())
    .pipe(gulp.dest('./dist/css'))
    .pipe(browserSync.stream());
}

// Servidor de desenvolvimento
function serve() {
  browserSync.init({
    server: './'
  });

  gulp.watch('./src/scss/**/*.scss', compileSass);
  gulp.watch('./*.html').on('change', browserSync.reload);
}

exports.sass = compileSass;
exports.serve = serve;
exports.default = serve;
```

## Ferramentas de Wireframing e Prototipagem

### Figma

Ferramenta de design colaborativo baseada na web.

#### Recursos

- Interface intuitiva
- Componentes reutilizáveis
- Design responsivo
- Colaboração em tempo real
- Protótipos interativos
- Modo de desenvolvedor para extrair código

#### Uso para Desenvolvedores

- Acesse o modo "Desenvolvedor" para ver propriedades CSS
- Use a ferramenta de inspeção para obter medidas precisas
- Exporte assets em múltiplos formatos
- Veja fluxos de protótipos para entender interações

### Adobe XD

Ferramenta de design e prototipagem da Adobe.

### Sketch

Ferramenta de design para Mac com foco em UI/UX.

### Balsamiq

Ferramenta para wireframes de baixa fidelidade.


#### Fluxo de Trabalho Básico

1. Crie uma branch para trabalhar: git checkout -b feature-name
2. Faça alterações no código
3. Adicione e commit as alterações: git add . && git commit -m "Descrição"
4. Mescle de volta para a branch principal: git checkout main && git merge feature-name
5. Envie para o repositório remoto: git push origin main

### GitHub/GitLab/Bitbucket

Plataformas de hospedagem Git com ferramentas adicionais.

#### Recursos

- Hospedagem de repositórios
- Issues e Pull Requests
- Code Reviews
- Integração contínua
- Pages (hospedagem estática gratuita)
- Wikis e documentação

#### GitHub Pages

Hospedagem gratuita para sites estáticos diretamente do repositório:

1. Crie um repositório: username.github.io
2. Clone para sua máquina
3. Adicione seus arquivos HTML/CSS
4. Faça push para o GitHub
5. Acesse username.github.io no navegador

Ou para um projeto específico:

1. Vá para Settings > Pages no seu repositório
2. Selecione a branch (geralmente main ou gh-pages)
3. Acesse username.github.io/repository-name

## Ferramentas de Teste de Responsividade

### Responsive Design Mode (Navegadores)

- **Chrome**: DevTools > Toggle Device Toolbar (Ctrl+Shift+M / Cmd+Option+M)
- **Firefox**: DevTools > Responsive Design Mode (Ctrl+Shift+M / Cmd+Option+M)
- **Edge**: DevTools > Toggle device emulation (Ctrl+Shift+M)

### Screenfly

- Site: [screenfly.org](http://screenfly.org/)
- Teste seu site em diferentes resoluções de tela
- Suporte para múltiplos dispositivos

### Responsively App

- Site: [responsively.app](https://responsively.app/)
- Aplicativo desktop para testar designs responsivos
- Mostra múltiplas telas simultaneamente
- Espelhamento de ações entre dispositivos
- Ferramentas de depuração integradas

### BrowserStack

- Site: [browserstack.com](https://www.browserstack.com/)
- Teste em navegadores e dispositivos reais
- Suporte para iOS, Android, diversos navegadores
- Captura de tela e ferramentas de depuração

## Ferramentas de Otimização

### Compressores de Imagem

- **TinyPNG/TinyJPG**: [tinypng.com](https://tinypng.com/)
- **Squoosh**: [squoosh.app](https://squoosh.app/)
- **ImageOptim**: [imageoptim.com](https://imageoptim.com/) (Mac)
- **Kraken.io**: [kraken.io](https://kraken.io/)

### Otimizadores de SVG

- **SVGOMG**: [jakearchibald.github.io/svgomg](https://jakearchibald.github.io/svgomg/)
- **SVG Optimizer**: [github.com/svg/svgo](https://github.com/svg/svgo)

### Minificadores CSS/JS

- **CSS Minifier**: [cssminifier.com](https://cssminifier.com/)
- **UglifyJS**: [github.com/mishoo/UglifyJS](https://github.com/mishoo/UglifyJS)
- **Clean-CSS**: [github.com/clean-css/clean-css](https://github.com/clean-css/clean-css)

### Analisadores de Performance

- **WebPageTest**: [webpagetest.org](https://www.webpagetest.org/)
- **GTmetrix**: [gtmetrix.com](https://gtmetrix.com/)
- **Yellow Lab Tools**: [yellowlab.tools](https://yellowlab.tools/)

## Geradores e Ferramentas de Referência

### Geradores CSS

- **CSS Grid Generator**: [cssgrid-generator.netlify.app](https://cssgrid-generator.netlify.app/)
- **Flexbox Generator**: [loading.io/flexbox](https://loading.io/flexbox/)
- **Gradient Generator**: [cssgradient.io](https://cssgradient.io/)
- **Box Shadow Generator**: [cssmatic.com/box-shadow](https://www.cssmatic.com/box-shadow)
- **Animation Generator**: [animista.net](https://animista.net/)

### Ferramentas de Cores

- **Color Hunt**: [colorhunt.co](https://colorhunt.co/)
- **Coolors**: [coolors.co](https://coolors.co/)
- **Adobe Color**: [color.adobe.com](https://color.adobe.com/)
- **Paletton**: [paletton.com](https://paletton.com/)
- **Contrast Checker**: [webaim.org/resources/contrastchecker](https://webaim.org/resources/contrastchecker/)

### Ferramentas de Fontes

- **Google Fonts**: [fonts.google.com](https://fonts.google.com/)
- **Font Pair**: [fontpair.co](https://www.fontpair.co/)
- **Fontjoy**: [fontjoy.com](https://fontjoy.com/)
- **Type Scale**: [type-scale.com](https://type-scale.com/)

### Inspiração e Referência

- **CSS-Tricks**: [css-tricks.com](https://css-tricks.com/)
- **CodePen**: [codepen.io](https://codepen.io/)
- **Dribbble**: [dribbble.com](https://dribbble.com/)
- **Behance**: [behance.net](https://www.behance.net/)
- **Awwwards**: [awwwards.com](https://www.awwwards.com/)

## Configuração de Ambiente de Desenvolvimento

### Ambiente Local

#### XAMPP/MAMP/WAMP

Pacotes que incluem Apache, MySQL, PHP e outras ferramentas.

- **XAMPP**: [apachefriends.org](https://www.apachefriends.org/) (Windows, Mac, Linux)
- **MAMP**: [mamp.info](https://www.mamp.info/) (Mac, Windows)
- **WAMP**: [wampserver.com](https://www.wampserver.com/) (Windows)

#### Node.js e npm

Ambiente JavaScript para desenvolvimento local e gerenciamento de pacotes.

```bash
# Verificar instalação
node -v
npm -v

# Iniciar projeto
npm init -y

# Instalar pacote
npm install package-name

# Instalar pacote de desenvolvimento
npm install --save-dev package-name

# Executar script
npm run script-name
```

#### Servidor Local Simples

Opções para executar um servidor HTTP básico:

**Com Python**:

```bash
# Python 3
python -m http.server

# Python 2
python -m SimpleHTTPServer
```

**Com Node.js**:

```bash
# Instalar http-server globalmente
npm install -g http-server

# Executar no diretório atual
http-server
```

**Com VS Code**:

- Instale a extensão "Live Server"
- Clique em "Go Live" na barra de status

### Ambientes Online

#### CodePen

- Site: [codepen.io](https://codepen.io/)
- Editor online para HTML, CSS e JavaScript
- Ideal para protótipos rápidos e compartilhamento
- Suporte para preprocessadores (Sass, LESS)
- Acesso a bibliotecas populares

#### JSFiddle

- Site: [jsfiddle.net](https://jsfiddle.net/)
- Ambiente de teste online
- Suporte para frameworks

#### CodeSandbox

- Site: [codesandbox.io](https://codesandbox.io/)
- IDE online com suporte a projetos completos
- Integração com GitHub
- Suporte para templates (React, Vue, Angular, etc.)

#### StackBlitz

- Site: [stackblitz.com](https://stackblitz.com/)
- IDE web para aplicações completas
- Ambiente Node.js no navegador
- Suporte para frameworks modernos

## Dicas e Truques para Desenvolvimento Eficiente

### Snippets e Templates

Crie e salve trechos de código reutilizáveis para agilizar o desenvolvimento.

**HTML Boilerplate**:

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Título da Página</title>
    <link rel="stylesheet" href="css/styles.css">
</head>
<body>
    <header>
        <h1>Título do Site</h1>
        <nav>
            <ul>
                <li><a href="#">Home</a></li>
                <li><a href="#">Sobre</a></li>
                <li><a href="#">Contato</a></li>
            </ul>
        </nav>
    </header>
    
    <main>
        <section>
            <h2>Seção Principal</h2>
            <p>Conteúdo aqui...</p>
        </section>
    </main>
    
    <footer>
        <p>&copy; 2023 Meu Site. Todos os direitos reservados.</p>
    </footer>
    
    <script src="js/scripts.js"></script>
</body>
</html>
```

**CSS Reset/Normalize**:

```css
/* Reset básico */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

/* Tipografia base */
body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  line-height: 1.6;
  color: #333;
}

/* Links */
a {
  color: #0066cc;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

/* Imagens responsivas */
img {
  max-width: 100%;
  height: auto;
}

/* Listas */
ul, ol {
  list-style-position: inside;
}
```

### Atalhos e Produtividade

#### Emmet

Linguagem de abreviação para HTML e CSS, integrada em muitos editores.

**Exemplos HTML**:

- ! → Doctype HTML5 completo
- header>nav>ul>li*5>a → Estrutura de navegação com 5 itens
- .container>.row>.col-md-6*2 → Layout de grid Bootstrap
- ul>li.item$*5{Item $} → Lista com 5 itens numerados

**Exemplos CSS**:

- m10 → margin: 10px;
- p20-30 → padding: 20px 30px;
- fw:b → font-weight: bold;
- bg:url → background: url();

#### DevTools como Editor

Use as ferramentas de desenvolvedor do navegador para experimentar antes de implementar:

1. Inspecione um elemento
2. Modifique CSS no painel Styles
3. Teste diferentes valores e veja resultados em tempo real
4. Copie o CSS final para seu arquivo

#### Extensões de Navegador Úteis

- **Web Developer**: Ferramentas adicionais para desenvolvimento web
- **ColorZilla**: Seletor de cores e conta-gotas
- **Dimensions**: Medição de elementos na página
- **Wappalyzer**: Detecta tecnologias usadas em sites
- **Responsive Viewer**: Visualiza site em múltiplas resoluções
- **CSS Peeper**: Extrai CSS e assets de sites
