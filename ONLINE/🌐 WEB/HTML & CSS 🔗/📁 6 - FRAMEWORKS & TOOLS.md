
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


````markdown
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

bashresponse-action-icon

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

scssresponse-action-icon

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

scssresponse-action-icon

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

scssresponse-action-icon

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

scssresponse-action-icon

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

scssresponse-action-icon

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

scssresponse-action-icon

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

scssresponse-action-icon

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

bashresponse-action-icon

```bash
# Instalar Less globalmente
npm install -g less

# Compilar arquivo
lessc input.less output.css
```

### Variáveis

lessresponse-action-icon

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

lessresponse-action-icon

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

lessresponse-action-icon

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

lessresponse-action-icon

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

lessresponse-action-icon

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

bashresponse-action-icon

```bash
# Instalar Stylus globalmente
npm install -g stylus

# Compilar arquivo
stylus input.styl -o output.css

# Compilar e assistir mudanças
stylus -w input.styl -o output.css
```

### Sintaxe Flexível

stylusresponse-action-icon

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

stylusresponse-action-icon

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

stylusresponse-action-icon

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

stylusresponse-action-icon

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

stylusresponse-action-icon

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