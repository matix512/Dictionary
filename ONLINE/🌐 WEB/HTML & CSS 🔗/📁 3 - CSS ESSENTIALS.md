
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

