
# 📄 07 - SINTAXE & SELETORES


# Sintaxe & Seletores CSS

## Sintaxe Básica

```css
seletor {
  propriedade: valor;
  outra-propriedade: outro-valor;
}
```


## Como Incluir CSS

```html
<!-- CSS Inline -->
<p style="color: blue; font-size: 16px;">Texto azul</p>

<!-- CSS Interno -->
<head>
  <style>
    p {
      color: blue;
      font-size: 16px;
    }
  </style>
</head>

<!-- CSS Externo (recomendado) -->
<head>
  <link rel="stylesheet" href="styles.css">
</head>
```

## Seletores Básicos

```css
/* Seletor de elemento */
p {
  color: blue;
}

/* Seletor de ID */
#cabecalho {
  background-color: black;
}

/* Seletor de classe */
.destaque {
  font-weight: bold;
}

/* Seletor universal */
* {
  margin: 0;
  padding: 0;
}
```

## Seletores Combinados

```css
/* Descendente: todos os parágrafos dentro de div */
div p {
  margin-left: 20px;
}

/* Filho direto: apenas filhos diretos */
div > p {
  border: 1px solid gray;
}

/* Adjacente: elemento que segue imediatamente */
h2 + p {
  font-style: italic;
}

/* Irmão: todos os elementos irmãos seguintes */
h2 ~ p {
  color: gray;
}
```

## Seletores de Atributo

```css
/* Elementos com atributo específico */
[target] {
  border: 1px solid black;
}

/* Atributo com valor específico */
[type="text"] {
  border-radius: 5px;
}

/* Atributo que contém palavra */
[class~="destaque"] {
  font-weight: bold;
}

/* Atributo que começa com */
[href^="https"] {
  color: green;
}

/* Atributo que termina com */
[href$=".pdf"] {
  color: red;
}

/* Atributo que contém substring */
[href*="exemplo"] {
  text-decoration: underline;
}
```

## Pseudo-classes

```css
/* Estado de links */
a:link { color: blue; }
a:visited { color: purple; }
a:hover { color: red; }
a:active { color: orange; }

/* Estrutura */
li:first-child { font-weight: bold; }
li:last-child { font-style: italic; }
li:nth-child(odd) { background-color: #f2f2f2; }
li:nth-child(3n) { color: red; }

/* Formulários */
input:focus { border-color: blue; }
input:disabled { background-color: #ccc; }
input:checked { border: 2px solid green; }
```

## Pseudo-elementos

```css
/* Primeira letra */
p::first-letter {
  font-size: 200%;
  font-weight: bold;
}

/* Primeira linha */
```markdown
p::first-line {
  font-variant: small-caps;
}

/* Conteúdo antes/depois */
.nota::before {
  content: "Nota: ";
  font-weight: bold;
}

.aviso::after {
  content: " ⚠️";
}

/* Seleção */
::selection {
  background-color: yellow;
  color: black;
}
```

## Especificidade

1. Estilos inline (1000)
2. IDs (100)
3. Classes, atributos e pseudo-classes (10)
4. Elementos e pseudo-elementos (1)

Exemplo de cálculo:

- #nav .lista li:hover = 100 + 10 + 1 + 10 = 121
- body #content .item p = 1 + 100 + 10 + 1 = 112


# 📄 08 - CORES & FUNDOS



# Cores & Fundos

## Formatos de Cores

```css
/* Nome da cor */
color: red;

/* Hexadecimal */
color: #ff0000;  /* Formato longo */
color: #f00;     /* Formato curto */

/* RGB */
color: rgb(255, 0, 0);

/* RGBA (com transparência) */
color: rgba(255, 0, 0, 0.5);  /* 50% transparente */

/* HSL (Hue, Saturation, Lightness) */
color: hsl(0, 100%, 50%);

/* HSLA (com transparência) */
color: hsla(0, 100%, 50%, 0.5);

/* Valor atual + transparência */
color: red;
opacity: 0.5;  /* Afeta todo o elemento e seus filhos */
```

## Propriedades de Cor

```css
/* Cor do texto */
color: #333;

/* Cor de fundo */
background-color: #f0f0f0;

/* Cor da borda */
border-color: #999;

/* Cor de outline */
outline-color: blue;
```

## Propriedades de Fundo

```css
/* Cor de fundo */
background-color: lightblue;

/* Imagem de fundo */
background-image: url('imagem.jpg');

/* Repetição de fundo */
background-repeat: no-repeat;  /* Não repete */
background-repeat: repeat-x;   /* Repete horizontalmente */
background-repeat: repeat-y;   /* Repete verticalmente */
background-repeat: repeat;     /* Repete em ambas direções */

/* Posição do fundo */
background-position: center;
background-position: top right;
background-position: 50% 25%;
background-position: 20px 30px;

/* Tamanho do fundo */
background-size: cover;     /* Cobre todo o container */
background-size: contain;   /* Cabe dentro do container */
background-size: 100% auto; /* Largura 100%, altura automática */
background-size: 200px 150px;

/* Anexação de fundo */
background-attachment: scroll;  /* Rola com a página */
background-attachment: fixed;   /* Fixo na viewport */
background-attachment: local;   /* Rola com o conteúdo do elemento */

/* Origem de fundo */
background-origin: padding-box;  /* Padrão */
background-origin: border-box;   /* Inclui a área da borda */
background-origin: content-box;  /* Apenas área de conteúdo */

/* Área de recorte de fundo */
background-clip: border-box;   /* Padrão */
background-clip: padding-box;  /* Corta na borda interna */
background-clip: content-box;  /* Corta na borda do conteúdo */
```

## Sintaxe Abreviada

```css
/* background: cor imagem repetição anexação posição / tamanho */
background: #f0f0f0 url('imagem.jpg') no-repeat fixed center / cover;
```

## Múltiplos Fundos

```css
/* Ordem: o primeiro declarado fica por cima */
background: 
  url('padrão.png') repeat,
  url('logo.png') no-repeat center,
  linear-gradient(to bottom, #fff, #ccc);
```

## Gradientes

```css
/* Gradiente Linear */
background: linear-gradient(to right, red, blue);
background: linear-gradient(45deg, red, blue);
background: linear-gradient(to bottom right, red, yellow, blue);

/* Gradiente Radial */
background: radial-gradient(circle, yellow, green);
background: radial-gradient(ellipse at top left, yellow, green);
background: radial-gradient(circle at center, white, blue 20%, black);

/* Gradiente Cônico */
background: conic-gradient(red, yellow, green, blue, purple);
background: conic-gradient(from 45deg, red, blue);

/* Gradiente Repetido */
background: repeating-linear-gradient(45deg, red, red 10px, blue 10px, blue 20px);
background: repeating-radial-gradient(circle, white, white 10px, black 10px, black 20px);
```

## Paletas de Cores

```css
/* Esquema monocromático */
:root {
  --cor-primaria: #3498db;
  --cor-primaria-clara: #5dade2;
  --cor-primaria-escura: #2980b9;
}

/* Esquema complementar */
:root {
  --cor-primaria: #3498db; /* Azul */
  --cor-complementar: #e67e22; /* Laranja */
}

/* Esquema análogo */
:root {
  --cor-primaria: #3498db; /* Azul */
  --cor-secundaria-1: #2ecc71; /* Verde */
  --cor-secundaria-2: #9b59b6; /* Roxo */
}
```

## Ferramentas de Cores

- [Adobe Color](https://color.adobe.com/)
- [Coolors](https://coolors.co/)
- [ColorHunt](https://colorhunt.co/)
- [Paletton](https://paletton.com/)


# 📄 09 - BOX MODEL

## Conceito

Todo elemento HTML é representado como uma caixa retangular. O Box Model descreve como essas caixas são dimensionadas, posicionadas e como interagem entre si.

## Componentes do Box Model
```

┌───────────────────────────────────────┐  
│ Margin │  
│ ┌───────────────────────────────────┐ │  
│ │ Border │ │  
│ │ ┌───────────────────────────────┐ │ │  
│ │ │ Padding │ │ │  
│ │ │ ┌───────────────────────────┐ │ │ │  
│ │ │ │ Content │ │ │ │  
│ │ │ └───────────────────────────┘ │ │ │  
│ │ └───────────────────────────────┘ │ │  
│ └───────────────────────────────────┘ │  
└───────────────────────────────────────┘

- **Content**: Área onde o conteúdo é exibido
- **Padding**: Espaço entre o conteúdo e a borda
- **Border**: Linha que circunda o padding
- **Margin**: Espaço externo à borda, entre elementos
```

## Propriedades Básicas

### Width & Height
```css
/* Largura e altura do conteúdo */
width: 300px;
height: 200px;

/* Largura e altura mínima/máxima */
min-width: 100px;
max-width: 500px;
min-height: 100px;
max-height: 300px;

/* Largura percentual (relativa ao container) */
width: 50%;
````

### Padding

```css
/* Padding em todos os lados */
padding: 20px;

/* Padding por lado (topo, direita, baixo, esquerda) */
padding: 10px 20px 15px 25px;

/* Padding em lados específicos */
padding-top: 10px;
padding-right: 20px;
padding-bottom: 15px;
padding-left: 25px;
```

### Border

```css
/* Border completa */
border: 1px solid #000;

/* Border por lado */
border-top: 2px dashed red;
border-right: 1px dotted blue;
border-bottom: 3px double green;
border-left: 1px solid black;

/* Propriedades individuais */
border-width: 1px;
border-style: solid;
border-color: #333;

/* Border-radius (arredondamento) */
border-radius: 5px;  /* Todos os cantos */
border-radius: 10px 5px 8px 15px;  /* TL, TR, BR, BL */

/* Border-radius circular/elíptico */
border-radius: 50%;  /* Círculo (para um quadrado) */
border-radius: 10px / 20px;  /* Elipse (x/y) */
```

### Margin

```css
/* Margin em todos os lados */
margin: 20px;

/* Margin por lado (topo, direita, baixo, esquerda) */
margin: 10px 20px 15px 25px;

/* Margin em lados específicos */
margin-top: 10px;
margin-right: 20px;
margin-bottom: 15px;
margin-left: 25px;

/* Centralização horizontal */
margin: 0 auto;

/* Margin negativa (para sobreposição) */
margin-top: -10px;
```

## Box Sizing

```css
/* Comportamento padrão (content-box) */
box-sizing: content-box;
/* width/height define apenas conteúdo, padding e border são adicionados */

/* Comportamento alternativo (border-box) */
box-sizing: border-box;
/* width/height inclui conteúdo, padding e border */

/* Aplicar a todos os elementos (recomendado) */
* {
  box-sizing: border-box;
}
```

## Overflow

```css
/* Comportamento quando o conteúdo é maior que o container */
overflow: visible;  /* Padrão, conteúdo transborda */
overflow: hidden;   /* Esconde o conteúdo excedente */
overflow: scroll;   /* Sempre mostra barras de rolagem */
overflow: auto;     /* Barras de rolagem apenas quando necessário */

/* Controle por eixo */
overflow-x: scroll;
overflow-y: hidden;
```

## Display

```css
/* Valores comuns */
display: block;        /* Elemento de bloco (toda a largura) */
display: inline;       /* Elemento em linha (lado a lado) */
display: inline-block; /* Híbrido (lado a lado, mas aceita width/height) */
display: none;         /* Remove o elemento do fluxo (não ocupa espaço) */

/* Valores modernos */
display: flex;         /* Layout flexível */
display: grid;         /* Layout em grid */
```

## Visibilidade

```css
visibility: visible;  /* Padrão, elemento visível */
visibility: hidden;   /* Elemento invisível, mas ocupa espaço */
```

## Ordem de Cascata para Shorthand

- **margin/padding**: topo, direita, baixo, esquerda (sentido horário)
- **border**: largura, estilo, cor

## Exemplos Práticos

```css
/* Card básico */
.card {
  width: 300px;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
  margin: 15px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

/* Botão */
.button {
  display: inline-block;
  padding: 10px 20px;
  margin: 5px;
  border: none;
  border-radius: 4px;
  background-color: #3498db;
  color: white;
  text-align: center;
  cursor: pointer;
}
```



# 📄 10 - TIPOGRAFIA
# Tipografia em CSS

## Propriedades Básicas

### Font-Family
```css
/* Fonte específica com fallbacks */
font-family: 'Helvetica Neue', Arial, sans-serif;

/* Famílias genéricas */
font-family: serif;        /* Com serifa (ex: Times New Roman) */
font-family: sans-serif;   /* Sem serifa (ex: Arial) */
font-family: monospace;    /* Mono-espaçada (ex: Courier) */
font-family: cursive;      /* Cursiva (ex: Brush Script) */
font-family: fantasy;      /* Decorativa (ex: Impact) */
```

### Font-Size

```css
/* Tamanhos absolutos */
font-size: 16px;      /* Pixels */
font-size: 12pt;      /* Pontos */

/* Tamanhos relativos */
font-size: 1.2em;     /* Relativo ao elemento pai */
font-size: 1.2rem;    /* Relativo ao elemento raiz (html) */
font-size: 120%;      /* Percentual do tamanho do pai */

/* Tamanhos predefinidos */
font-size: small;     /* Pequeno */
font-size: medium;    /* Médio (padrão) */
font-size: large;     /* Grande */
font-size: x-large;   /* Extra grande */
```

### Font-Weight

```css
/* Numérico (100-900) */
font-weight: 400;     /* Normal */
font-weight: 700;     /* Negrito */

/* Palavras-chave */
font-weight: normal;
font-weight: bold;
font-weight: lighter;  /* Mais leve que o elemento pai */
font-weight: bolder;   /* Mais pesado que o elemento pai */
```

### Font-Style

```css
font-style: normal;    /* Estilo normal */
font-style: italic;    /* Itálico */
font-style: oblique;   /* Oblíquo (similar ao itálico) */
```

### Text-Decoration

```css
text-decoration: none;          /* Sem decoração */
text-decoration: underline;     /* Sublinhado */
text-decoration: overline;      /* Linha acima */
text-decoration: line-through;  /* Riscado */

/* Múltiplas decorações */
text-decoration: underline overline;

/* Propriedades detalhadas */
text-decoration-line: underline;
text-decoration-style: wavy;  /* solid, dashed, dotted, wavy */
text-decoration-color: red;
text-decoration-thickness: 2px;

/* Shorthand completo */
text-decoration: underline wavy red 2px;
```

### Text-Transform

```css
text-transform: none;       /* Sem transformação */
text-transform: capitalize; /* Primeira Letra De Cada Palavra Em Maiúscula */
text-transform: uppercase;  /* TODAS AS LETRAS EM MAIÚSCULA */
text-transform: lowercase;  /* todas as letras em minúscula */
```

## Espaçamento e Alinhamento

### Letter-Spacing

```css
letter-spacing: normal;  /* Padrão */
letter-spacing: 2px;     /* Espaçamento entre letras */
letter-spacing: -1px;    /* Espaçamento negativo (aproxima) */
```

### Word-Spacing

```css
word-spacing: normal;  /* Padrão */
word-spacing: 5px;     /* Espaçamento entre palavras */
```

### Line-Height

```css
line-height: normal;    /* Padrão (aproximadamente 1.2) */
line-height: 1.5;       /* Multiplicador (sem unidade) - recomendado */
line-height: 24px;      /* Valor fixo */
line-height: 150%;      /* Percentual */
```

### Text-Align

```css
text-align: left;      /* Alinhado à esquerda (padrão) */
text-align: right;     /* Alinhado à direita */
text-align: center;    /* Centralizado */
text-align: justify;   /* Justificado */
```

### Text-Indent

```css
text-indent: 2em;     /* Recuo da primeira linha */
text-indent: -20px;   /* Recuo negativo (hanging indent) */
```

### Vertical-Align

```css
/* Para elementos inline ou células de tabela */
vertical-align: baseline;   /* Alinha com a linha de base (padrão) */
vertical-align: top;        /* Alinha com o topo */
vertical-align: middle;     /* Alinha com o meio */
vertical-align: bottom;     /* Alinha com a base */
vertical-align: text-top;   /* Alinha com o topo do texto */
vertical-align: text-bottom; /* Alinha com a base do texto */
vertical-align: 5px;        /* Deslocamento específico */
```

## Fontes Web

### @font-face

cssresponse-action-icon

```css
@font-face {
  font-family: 'MinhaFonte';
  src: url('minhafonte.woff2') format('woff2'),
       url('minhafonte.woff') format('woff');
  font-weight: normal;
  font-style: normal;
  font-display: swap;  /* Estratégia de carregamento */
}

/* Uso da fonte */
.elemento {
  ```markdown
font-family: 'MinhaFonte', sans-serif;
}
```

### Google Fonts

```html
<!-- No head do HTML -->
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
```

```css
/* No CSS */
body {
  font-family: 'Roboto', sans-serif;
}
```

## Propriedades Avançadas

### Text-Shadow

```css
/* Sintaxe: x-offset y-offset blur-radius color */
text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);  /* Sombra simples */
text-shadow: 0 0 5px #fff;                    /* Glow effect */

/* Múltiplas sombras */
text-shadow: 
  1px 1px 2px black, 
  0 0 25px blue, 
  0 0 5px darkblue;
```

### White-Space

```css
white-space: normal;      /* Quebra de linha automática (padrão) */
white-space: nowrap;      /* Sem quebras de linha */
white-space: pre;         /* Preserva espaços e quebras de linha (como a tag <pre>) */
white-space: pre-wrap;    /* Preserva espaços, quebra automaticamente */
white-space: pre-line;    /* Preserva apenas quebras de linha, colapsa espaços */
```

### Word-Break

```css
word-break: normal;       /* Comportamento padrão */
word-break: break-all;    /* Quebra palavras em qualquer caractere */
word-break: keep-all;     /* Não quebra palavras */
```

### Word-Wrap (Overflow-Wrap)

```css
word-wrap: normal;        /* Comportamento padrão */
word-wrap: break-word;    /* Quebra palavras longas */
overflow-wrap: break-word; /* Mesmo que word-wrap (nome mais moderno) */
```

### Text-Overflow

```css
/* Requer white-space: nowrap e overflow: hidden */
.truncate {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: clip;      /* Corta o texto */
  text-overflow: ellipsis;  /* Mostra "..." */
}
```

### Font Shorthand

```css
/* font: [font-style] [font-variant] [font-weight] [font-size/line-height] [font-family] */
font: italic bold 16px/1.5 'Helvetica', sans-serif;
```

## Estilos Responsivos

```css
/* Tamanho base */
body {
  font-size: 16px;
}

/* Em telas médias */
@media (max-width: 768px) {
  body {
    font-size: 14px;
  }
}

/* Em telas pequenas */
@media (max-width: 480px) {
  body {
    font-size: 12px;
  }
  h1 {
    font-size: 1.8rem;
  }
}
```

## Sistema de Tipografia Fluida

```css
/* Tamanho que varia entre 16px e 20px baseado no tamanho da viewport */
:root {
  font-size: clamp(16px, 1vw + 14px, 20px);
}

h1 {
  font-size: clamp(1.5rem, 3vw + 1rem, 2.5rem);
}
```

## Variáveis CSS para Sistema Tipográfico

```css
:root {
  --font-primary: 'Roboto', sans-serif;
  --font-secondary: 'Playfair Display', serif;
  --font-code: 'Source Code Pro', monospace;
  
  --font-size-xs: 0.75rem;
  --font-size-sm: 0.875rem;
  --font-size-md: 1rem;
  --font-size-lg: 1.125rem;
  --font-size-xl: 1.25rem;
  --font-size-2xl: 1.5rem;
  --font-size-3xl: 1.875rem;
  --font-size-4xl: 2.25rem;
  
  --font-weight-light: 300;
  --font-weight-normal: 400;
  --font-weight-medium: 500;
  --font-weight-bold: 700;
  
  --line-height-tight: 1.25;
  --line-height-normal: 1.5;
  --line-height-loose: 1.75;
}

body {
  font-family: var(--font-primary);
  font-size: var(--font-size-md);
  line-height: var(--line-height-normal);
}

h1 {
  font-family: var(--font-secondary);
  font-size: var(--font-size-4xl);
  font-weight: var(--font-weight-bold);
}
```


---

# 📄 11 - LAYOUT & POSICIONAMENTO


## Display

### Display Básico
```css
display: block;        /* Ocupa toda a largura disponível */
display: inline;       /* Em linha, lado a lado, sem width/height */
display: inline-block; /* Combina propriedades de inline e block */
display: none;         /* Remove o elemento (não ocupa espaço) */
```

### Display Moderno
```css
display: flex;         /* Layout flexível unidimensional */
display: grid;         /* Layout em grade bidimensional */
display: table;        /* Comportamento de tabela */
```

## Position

### Valores
```css
position: static;      /* Padrão, segue o fluxo normal */
position: relative;    /* Posicionado relativo à sua posição normal */
position: absolute;    /* Posicionado relativo ao ancestral posicionado mais próximo */
position: fixed;       /* Posicionado relativo à viewport (janela) */
position: sticky;      /* Híbrido entre relative e fixed */
```

### Propriedades de Deslocamento
```css
top: 10px;            /* Distância do topo */
right: 20px;          /* Distância da direita */
bottom: 30px;         /* Distância da base */
left: 40px;           /* Distância da esquerda */
```

### Exemplos

```css
/* Elemento centralizado absolutamente */
.centralizado {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

/* Elemento fixo no topo (navbar) */
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 100;
}

/* Elemento sticky */
.sidebar {
  position: sticky;
  top: 20px;
  /* Permanece em posição "static" até rolar até 20px do topo,
     então se torna "fixed" com top: 20px */
}
```

## Float

### Valores Básicos

```css
float: left;          /* Flutua à esquerda */
float: right;         /* Flutua à direita */
float: none;          /* Não flutua (padrão) */
```

### Clearing Floats

```css
clear: left;          /* Limpa floats à esquerda */
clear: right;         /* Limpa floats à direita */
clear: both;          /* Limpa floats em ambos os lados */

/* Técnica clearfix (para containers com elementos flutuantes) */
.clearfix::after {
  content: "";
  display: block;
  clear: both;
}
```

## Z-Index

```css
/* Controla a ordem de empilhamento (apenas para position != static) */
z-index: 1;           /* Valor numérico, maior = mais à frente */
z-index: 10;
z-index: -1;          /* Valores negativos colocam atrás do fluxo normal */
z-index: auto;        /* Valor padrão */
```

## Flexbox

### Container Flex

```css
.container {
  display: flex;
  
  /* Direção */
  flex-direction: row;            /* Padrão, horizontal */
  flex-direction: row-reverse;    /* Horizontal invertido */
  flex-direction: column;         /* Vertical */
  flex-direction: column-reverse; /* Vertical invertido */
  
  /* Quebra de linha */
  flex-wrap: nowrap;              /* Sem quebra (padrão) */
  flex-wrap: wrap;                /* Quebra quando necessário */
  flex-wrap: wrap-reverse;        /* Quebra na direção oposta */
  
  /* Shorthand para direction + wrap */
  flex-flow: row wrap;
  
  /* Alinhamento horizontal */
  justify-content: flex-start;    /* Início (padrão) */
  justify-content: flex-end;      /* Fim */
  justify-content: center;        /* Centro */
  justify-content: space-between; /* Espaço igual entre itens */
  justify-content: space-around;  /* Espaço igual ao redor dos itens */
  justify-content: space-evenly;  /* Espaço perfeitamente igual */
  
  /* Alinhamento vertical */
  align-items: stretch;           /* Estica para preencher (padrão) */
  align-items: flex-start;        /* Início */
  align-items: flex-end;          /* Fim */
  align-items: center;            /* Centro */
  align-items: baseline;          /* Alinha pela linha de base do texto */
  
  /* Alinhamento de múltiplas linhas */
  align-content: flex-start;      /* Linhas no início */
  align-content: flex-end;        /* Linhas no fim */
  align-content: center;          /* Linhas no centro */
  align-content: space-between;   /* Espaço igual entre linhas */
  align-content: space-around;    /* Espaço igual ao redor das linhas */
  align-content: stretch;         /* Linhas esticadas (padrão) */
  
  /* Espaçamento entre itens */
  gap: 10px;                      /* Igual em todas as direções */
  gap: 20px 10px;                 /* Row-gap column-gap */
  row-gap: 20px;                  /* Apenas entre linhas */
  column-gap: 10px;               /* Apenas entre colunas */
}
```

### Itens Flex
```css
.item {
  /* Ordem de exibição */
  order: 0;                /* Padrão, valor maior = aparece depois */
  
  /* Crescimento proporcional */
  flex-grow: 0;            /* Não cresce (padrão) */
  flex-grow: 1;            /* Cresce proporcionalmente */
  flex-grow: 2;            /* Cresce 2x mais que itens com flex-grow: 1 */
  
  /* Encolhimento proporcional */
  flex-shrink: 1;          /* Encolhe proporcionalmente (padrão) */
  flex-shrink: 0;          /* Não encolhe */
  flex-shrink: 2;          /* Encolhe 2x mais */
  
  /* Tamanho base */
  flex-basis: auto;        /* Baseado no conteúdo (padrão) */
  flex-basis: 200px;       /* Tamanho inicial específico */
  flex-basis: 0;           /* Não considera tamanho inicial */
  
  /* Shorthand para grow, shrink e basis */
  flex: 0 1 auto;          /* Padrão (não cresce, encolhe, auto) */
  flex: 1;                 /* Equivalente a flex: 1 1 0% */
  flex: auto;              /* Equivalente a flex: 1 1 auto */
  flex: none;              /* Equivalente a flex: 0 0 auto */
  
  /* Alinhamento individual (sobrepõe align-items) */
  align-self: auto;        /* Herda do container (padrão) */
  align-self: flex-start;  /* Início */
  align-self: flex-end;    /* Fim */
  align-self: center;      /* Centro */
  align-self: baseline;    /* Linha de base */
  align-self: stretch;     /* Esticado */
}
```

## Grid

### Container Grid

```css
.grid-container {
  display: grid;
  
  /* Definição de colunas */
  grid-template-columns: 100px 200px 100px;          /* 3 colunas fixas */
  grid-template-columns: 1fr 2fr 1fr;                /* Proporção fracionária */
  grid-template-columns: repeat(3, 1fr);             /* 3 colunas iguais */
  grid-template-columns: minmax(100px, 1fr) 2fr;     /* Mínimo e máximo */
  grid-template-columns: auto 1fr auto;              /* Auto = tamanho do conteúdo */
  
  /* Definição de linhas */
  grid-template-rows: 100px 200px;                   /* 2 linhas fixas */
  grid-template-rows: repeat(3, minmax(100px, auto)); /* 3 linhas adaptáveis */
  
  /* Áreas nomeadas */
  grid-template-areas: 
    "header header header"
    "sidebar main main"
    "footer footer footer";
  
  /* Espaçamento entre células */
  gap: 10px;                                         /* Igual em todas as direções */
  row-gap: 15px;                                     /* Apenas entre linhas */
  column-gap: 10px;                                  /* Apenas entre colunas */
  
  /* Alinhamento horizontal de todas as células */
  justify-items: stretch;                            /* Esticado (padrão) */
  justify-items: start;                              /* Início */
  justify-items: end;                                /* Fim */
  justify-items: center;                             /* Centro */
  
  /* Alinhamento vertical de todas as células */
  align-items: stretch;                              /* Esticado (padrão) */
  align-items: start;                                /* Início */
  align-items: end;                                  /* Fim */
  align-items: center;                               /* Centro */
  
  /* Alinhamento horizontal do grid na área */
  justify-content: start;                            /* Início (padrão) */
  justify-content: end;                              /* Fim */
  justify-content: center;                           /* Centro */
  justify-content: space-between;                    /* Espaço entre */
  justify-content: space-around;                     /* Espaço ao redor */
  justify-content: space-evenly;                     /* Espaço igual */
  
  /* Alinhamento vertical do grid na área */
  align-content: start;                              /* Início (padrão) */
  align-content: end;                                /* Fim */
  align-content: center;                             /* Centro */
  align-content: space-between;                      /* Espaço entre */
  align-content: space-around;                       /* Espaço ao redor */
  align-content: space-evenly;                       /* Espaço igual */
  
  /* Fluxo automático */
  grid-auto-flow: row;                               /* Por linha (padrão) */
  grid-auto-flow: column;                            /* Por coluna */
  grid-auto-flow: dense;                             /* Preenche espaços vazios */
  
  /* Tamanho de linhas/colunas implícitas */
  grid-auto-rows: 100px;                             /* Linhas automáticas */
  grid-auto-columns: 1fr;                            /* Colunas automáticas */
}
```

### Itens Grid

```css
.grid-item {
  /* Posicionamento por linha/coluna */
  grid-column-start: 1;                  /* Coluna inicial */
  grid-column-end: 3;                    /* Coluna final (exclusiva) */
  grid-row-start: 2;                     /* Linha inicial */
  grid-row-end: 4;                       /* Linha final (exclusiva) */
  
  /* Shorthand para posicionamento */
  grid-column: 1 / 3;                    /* Coluna início / fim */
  grid-column: 1 / span 2;               /* Coluna início / extensão */
  grid-row: 2 / 4;                       /* Linha início / fim */
  
  /* Posicionamento por área nomeada */
  grid-area: header;                     /* Nome da área definida em grid-template-areas */
  
  /* Shorthand para row-start / column-start / row-end / column-end */
  grid-area: 2 / 1 / 4 / 3;              
  
  /* Alinhamento horizontal individual */
  justify-self: stretch;                 /* Esticado (padrão) */
  justify-self: start;                   /* Início */
  justify-self: end;                     /* Fim */
  justify-self: center;                  /* Centro */
  
  /* Alinhamento vertical individual */
  align-self: stretch;                   /* Esticado (padrão) */
  align-self: start;                     /* Início */
  align-self: end;                       /* Fim */
  align-self: center;                    /* Centro */
}
```

## Exemplos de Layout Completo

### Layout com Flexbox
```css
/* Container principal */
.layout {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

/* Cabeçalho */
.header {
  padding: 20px;
  background-color: #f0f0f0;
}

/* Área de conteúdo (flexível) */
.content-area {
  display: flex;
  flex: 1;  /* Ocupa todo espaço disponível */
}

/* Barra lateral */
.sidebar {
  width: 250px;
  padding: 20px;
  background-color: #e0e0e0;
}

/* Conteúdo principal */
.main-content {
  flex: 1;  /* Ocupa resto do espaço */
  padding: 20px;
}

/* Rodapé */
.footer {
  padding: 20px;
  background-color: #d0d0d0
```

