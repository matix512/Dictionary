### 📄 01 - INTRODUÇÃO AO HTML & CSS

markdownresponse-action-icon


# Introdução ao HTML & CSS

## O que é HTML?
HTML (HyperText Markup Language) é a linguagem padrão para criar páginas web. Define a estrutura e o conteúdo de uma página usando **elementos** representados por **tags**.

## O que é CSS?
CSS (Cascading Style Sheets) é a linguagem usada para estilizar os elementos HTML. Controla a aparência, layout e apresentação visual das páginas web.

## Como funcionam juntos?
- **HTML**: Estrutura e conteúdo (o "esqueleto")
- **CSS**: Aparência e layout (o "visual")

## Estrutura básica de uma página web
```html
<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Minha Página</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>Olá Mundo!</h1>
    <p>Este é um parágrafo.</p>
</body>
</html>
````


### 📄 02 - FERRAMENTAS & AMBIENTE


# Ferramentas & Ambiente de Desenvolvimento

## Editores de Código
- **Visual Studio Code**: Editor mais popular, extensões para HTML/CSS
- **Sublime Text**: Leve e rápido
- **Atom**: Personalizável e integrado com GitHub
- **Brackets**: Focado em desenvolvimento web

## Extensões Úteis (VS Code)
- Live Server
- HTML CSS Support
- Auto Rename Tag
- Prettier
- HTML Snippets

## Browsers & Dev Tools
- Chrome DevTools (F12 ou Ctrl+Shift+I)
- Firefox Developer Edition
- Edge DevTools

## Validators
- [W3C HTML Validator](https://validator.w3.org/)
- [W3C CSS Validator](https://jigsaw.w3.org/css-validator/)

## Configuração básica do VS Code
```json
{
  "editor.formatOnSave": true,
  "editor.tabSize": 2,
  "html.format.wrapLineLength": 100,
  "css.validate": true
}
````