
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

cssresponse-action-icon

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

cssresponse-action-icon

```css
@media screen and (min-width: 768px) { ... }  /* AND: ambas condições verdadeiras */
@media screen or (min-width: 768px) { ... }   /* OR: qualquer condição verdadeira */
@media not screen { ... }                     /* NOT: inverte a condição */
```

## Breakpoints Comuns

cssresponse-action-icon

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

htmlresponse-action-icon

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

cssresponse-action-icon

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

cssresponse-action-icon

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

cssresponse-action-icon

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