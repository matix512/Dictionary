
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


# Box Model (Modelo de Caixa)

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

textresponse-action-icon

````text

- **Content**: Área onde o conteúdo é exibido
- **Padding**: Espaço entre o conteúdo e a borda
- **Border**: Linha que circunda o padding
- **Margin**: Espaço externo à borda, entre elementos

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