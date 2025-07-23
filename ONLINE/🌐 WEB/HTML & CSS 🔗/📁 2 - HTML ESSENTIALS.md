

### 📄 03 - ESTRUTURA & SEMÂNTICA

````HTML
## Elementos de Estrutura Básica
- `<!DOCTYPE html>`: Declaração do tipo de documento
- `<html>`: Elemento raiz
- `<head>`: Metadados e recursos
- `<body>`: Conteúdo visível

## Elementos Semânticos
- `<header>`: Cabeçalho da página ou seção
- `<nav>`: Menu de navegação
- `<main>`: Conteúdo principal
- `<section>`: Seção genérica de conteúdo
- `<article>`: Conteúdo independente
- `<aside>`: Conteúdo relacionado mas separado
- `<footer>`: Rodapé da página ou seção

## Estrutura Semântica Completa
```html
<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Página Semântica</title>
</head>
<body>
    <header>
        <h1>Meu Site</h1>
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
            <article>
                <h3>Artigo 1</h3>
                <p>Conteúdo do artigo.</p>
            </article>
        </section>
        
        <aside>
            <h2>Informações Relacionadas</h2>
            <p>Conteúdo complementar.</p>
        </aside>
    </main>
    
    <footer>
        <p>&copy; 2023 Meu Site</p>
    </footer>
</body>
</html>
````

## Benefícios da Semântica

- Melhor acessibilidade
- SEO aprimorado
- Código mais legível
- Manutenção facilitada

# 📄 04 - TEXTO & TIPOGRAFIA

# Texto & Tipografia HTML

## Títulos
```html
<h1>Título Principal</h1>
<h2>Subtítulo</h2>
<h3>Título de seção</h3>
<h4>Subtítulo de seção</h4>
<h5>Título menor</h5>
<h6>Título menor ainda</h6>
````

## Parágrafos e Quebras
```html
<p>Este é um parágrafo de texto.</p>
<br> <!-- Quebra de linha -->
<hr> <!-- Linha horizontal -->
```

## Formatação de Texto
```html
<strong>Texto em negrito</strong> ou <b>negrito</b>
<em>Texto em itálico</em> ou <i>itálico</i>
<u>Texto sublinhado</u>
<s>Texto riscado</s> ou <del>texto deletado</del>
<mark>Texto destacado</mark>
<small>Texto pequeno</small>
<sub>Subscrito</sub> e <sup>Sobrescrito</sup>
```

## Listas
```html
<!-- Lista não ordenada -->
<ul>
    <li>Item 1</li>
    <li>Item 2</li>
    <li>Item 3</li>
</ul>

<!-- Lista ordenada -->
<ol>
    <li>Primeiro item</li>
    <li>Segundo item</li>
    <li>Terceiro item</li>
</ol>

<!-- Lista de definição -->
<dl>
    <dt>HTML</dt>
    <dd>Linguagem de marcação para páginas web</dd>
    <dt>CSS</dt>
    <dd>Linguagem de estilização para páginas web</dd>
</dl>
```

## Citações
```html
<blockquote>
    <p>Esta é uma citação longa que ocupa um bloco separado.</p>
    <cite>Autor da citação</cite>
</blockquote>

<p>Como alguém disse, <q>Esta é uma citação inline</q>.</p>
```

## Código
```html
<code>console.log("Código inline");</code>

<pre>
<code>
// Bloco de código com formatação preservada
function exemplo() {
    return "Olá mundo";
}
</code>
</pre>
```

## Práticas Recomendadas

- Use os elementos semânticos corretos para o conteúdo
- Mantenha a hierarquia de títulos (h1 → h2 → h3)
- Não use quebras de linha para espaçamento (use CSS)
- Evite formatar texto com HTML quando o estilo deve ser CSS


# 📄 05 - LINKS & NAVEGAÇÃO


````text


```markdown
# Links & Navegação

## Links Básicos
```html
<!-- Link para página externa -->
<a href="https://www.exemplo.com">Visite Exemplo.com</a>

<!-- Link para página interna -->
<a href="pagina2.html">Ir para Página 2</a>

<!-- Link para email -->
<a href="mailto:contato@exemplo.com">Envie um email</a>

<!-- Link para telefone -->
<a href="tel:+551199999999">Ligue para nós</a>
````

## Atributos de Links

htmlresponse-action-icon

```html
<!-- Abrir em nova aba -->
<a href="https://www.exemplo.com" target="_blank">Link externo</a>

<!-- Título de tooltip -->
<a href="pagina.html" title="Clique para mais informações">Mais info</a>

<!-- Download -->
<a href="arquivo.pdf" download>Baixar PDF</a>

<!-- Relação do link -->
<a href="https://www.exemplo.com" rel="nofollow">Link sem seguir</a>
```

## Links para Seções na Mesma Página

htmlresponse-action-icon

```html
<!-- Link para uma seção -->
<a href="#secao1">Ir para Seção 1</a>

<!-- Seção de destino -->
<h2 id="secao1">Seção 1</h2>
```

## Menu de Navegação

htmlresponse-action-icon

```html
<nav>
  <ul>
    <li><a href="index.html">Início</a></li>
    <li><a href="sobre.html">Sobre</a></li>
    <li><a href="servicos.html">Serviços</a></li>
    <li><a href="contato.html">Contato</a></li>
  </ul>
</nav>
```

## Navegação em Breadcrumb

htmlresponse-action-icon

```html
<nav aria-label="breadcrumb">
  <ol class="breadcrumb">
    <li><a href="/">Início</a></li>
    <li><a href="/produtos">Produtos</a></li>
    <li aria-current="page">Categoria</li>
  </ol>
</nav>
```

## Práticas Recomendadas

- Use texto descritivo nos links (evite "clique aqui")
- Indique links externos que abrem em nova janela
- Mantenha navegação consistente em todo o site
- Use aria-current="page" para indicar a página atual
- Considere usar rel="noopener" em links externos com target="_blank"

textresponse-action-icon

````text

### 📄 06 - IMAGENS & MÍDIA

```markdown
# Imagens & Mídia

## Imagens Básicas
```html
<!-- Imagem básica -->
<img src="imagem.jpg" alt="Descrição da imagem">

<!-- Com dimensões definidas -->
<img src="imagem.jpg" alt="Descrição da imagem" width="300" height="200">

<!-- Com título (tooltip) -->
<img src="imagem.jpg" alt="Descrição da imagem" title="Título da imagem">
````

## Atributos de Imagem

- src: Caminho da imagem (obrigatório)
- alt: Texto alternativo para acessibilidade (obrigatório)
- width: Largura da imagem
- height: Altura da imagem
- title: Título/tooltip da imagem
- loading: Comportamento de carregamento (eager, lazy)

## Formatos de Imagem

- **JPG/JPEG**: Fotos e imagens complexas
- **PNG**: Gráficos e imagens com transparência
- **GIF**: Animações simples
- **SVG**: Gráficos vetoriais escaláveis
- **WebP**: Formato moderno de alta compressão

## Figure & Figcaption

htmlresponse-action-icon

```html
<figure>
  <img src="grafico.png" alt="Gráfico de vendas 2023">
  <figcaption>Gráfico 1: Vendas por trimestre em 2023</figcaption>
</figure>
```

## Imagens Responsivas

htmlresponse-action-icon

```html
<!-- Imagem que se adapta ao container -->
<img src="imagem.jpg" alt="Descrição" style="max-width: 100%; height: auto;">

<!-- Diferentes imagens para diferentes tamanhos de tela -->
<picture>
  <source media="(min-width: 1200px)" srcset="imagem-grande.jpg">
  <source media="(min-width: 600px)" srcset="imagem-media.jpg">
  <img src="imagem-pequena.jpg" alt="Descrição da imagem">
</picture>

<!-- Múltiplas resoluções da mesma imagem -->
<img src="imagem-base.jpg" 
     srcset="imagem-1x.jpg 1x, imagem-2x.jpg 2x, imagem-3x.jpg 3x" 
     alt="Descrição da imagem">
```

## Vídeo

htmlresponse-action-icon

```html
<video width="320" height="240" controls>
  <source src="video.mp4" type="video/mp4">
  <source src="video.webm" type="video/webm">
  Seu navegador não suporta a tag de vídeo.
</video>
```

## Atributos de Vídeo

- controls: Mostra controles de reprodução
- autoplay: Reproduz automaticamente
- loop: Repete o vídeo
- muted: Inicia sem áudio
- poster="thumbnail.jpg": Imagem de thumbnail
- preload="auto|metadata|none": Comportamento de pré-carregamento

## Áudio

htmlresponse-action-icon

```html
<audio controls>
  <source src="audio.mp3" type="audio/mpeg">
  <source src="audio.ogg" type="audio/ogg">
  Seu navegador não suporta a tag de áudio.
</audio>
```

## Incorporando Conteúdo Externo

htmlresponse-action-icon

```html
<!-- YouTube -->
<iframe width="560" height="315" 
        src="https://www.youtube.com/embed/VIDEO_ID" 
        frameborder="0" 
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
        allowfullscreen>
</iframe>

<!-- Google Maps -->
<iframe src="https://www.google.com/maps/embed?pb=!1m18!..." 
        width="600" height="450" 
        style="border:0;" 
        allowfullscreen="" 
        loading="lazy">
</iframe>
```

## Otimização de Imagens

- Redimensione imagens para o tamanho necessário
- Comprima imagens antes de usar
- Use WebP quando possível
- Inclua sempre atributos width e height
- Implemente carregamento lazy
- Forneça alternativas de baixa resolução para conexões lentas