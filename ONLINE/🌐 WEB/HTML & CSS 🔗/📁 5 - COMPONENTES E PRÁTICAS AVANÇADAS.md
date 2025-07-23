
# 📄 15 - FORMULÁRIOS


## Estrutura Básica de Formulário

```html
<form action="/processar" method="post">
  <div class="form-group">
    <label for="nome">Nome:</label>
    <input type="text" id="nome" name="nome" required>
  </div>
  
  <div class="form-group">
    <label for="email">Email:</label>
    <input type="email" id="email" name="email" required>
  </div>
  
  <div class="form-group">
    <label for="mensagem">Mensagem:</label>
    <textarea id="mensagem" name="mensagem" rows="5"></textarea>
  </div>
  
  <button type="submit">Enviar</button>
</form>
```


## Atributos de Formulário

### Form

```html
<form 
  action="/processar"       <!-- URL para onde os dados serão enviados -->
  method="post"             <!-- Método HTTP (get ou post) -->
  enctype="multipart/form-data"  <!-- Para envio de arquivos -->
  autocomplete="on"         <!-- Sugestões de preenchimento -->
  novalidate                <!-- Desativa validação nativa -->
  target="_blank"           <!-- Abre resposta em nova janela -->
>
  <!-- Elementos do formulário -->
</form>
```

### Input

```html
<input 
  type="text"               <!-- Tipo de input (text, email, password, etc.) -->
  id="username"             <!-- Identificador único -->
  name="username"           <!-- Nome para envio ao servidor -->
  value="valor_padrão"      <!-- Valor inicial -->
  placeholder="Digite seu nome"  <!-- Texto de ajuda -->
  required                  <!-- Campo obrigatório -->
  disabled                  <!-- Desabilita o campo -->
  readonly                  <!-- Apenas leitura -->
  maxlength="50"            <!-- Número máximo de caracteres -->
  minlength="3"             <!-- Número mínimo de caracteres -->
  pattern="[A-Za-z]+"       <!-- Padrão regex para validação -->
  autocomplete="off"        <!-- Desativa autocompletar -->
>
```

## Tipos de Input

### Texto e Variações

```html
<!-- Texto comum -->
<input type="text" name="nome" placeholder="Seu nome">

<!-- Email -->
<input type="email" name="email" placeholder="seu@email.com">

<!-- Senha -->
<input type="password" name="senha" placeholder="Sua senha">

<!-- Texto longo -->
<textarea name="mensagem" rows="5" cols="30" placeholder="Sua mensagem"></textarea>

<!-- URL -->
<input type="url" name="website" placeholder="https://seusite.com">

<!-- Telefone -->
<input type="tel" name="telefone" placeholder="(00) 00000-0000">

<!-- Pesquisa -->
<input type="search" name="busca" placeholder="Buscar...">
```

### Números e Datas

```html
<!-- Número -->
<input type="number" name="quantidade" min="1" max="100" step="1" value="1">

<!-- Range (slider) -->
<input type="range" name="avaliacao" min="0" max="10" step="1" value="5">

<!-- Data -->
<input type="date" name="data" min="2023-01-01" max="2023-12-31">

<!-- Hora -->
<input type="time" name="hora">

<!-- Data e hora -->
<input type="datetime-local" name="data_hora">

<!-- Mês -->
<input type="month" name="mes">

<!-- Semana -->
<input type="week" name="semana">
```

### Seleção

```html
<!-- Checkbox -->
<input type="checkbox" id="concordo" name="concordo" value="sim">
<label for="concordo">Concordo com os termos</label>

<!-- Grupo de checkboxes -->
<fieldset>
  <legend>Interesses:</legend>
  <div>
    <input type="checkbox" id="interesse1" name="interesses" value="tecnologia">
    <label for="interesse1">Tecnologia</label>
  </div>
  <div>
    <input type="checkbox" id="interesse2" name="interesses" value="esportes">
    <label for="interesse2">Esportes</label>
  </div>
</fieldset>

<!-- Radio buttons -->
<fieldset>
  <legend>Gênero:</legend>
  <div>
    <input type="radio" id="feminino" name="genero" value="feminino">
    <label for="feminino">Feminino</label>
  </div>
  <div>
    <input type="radio" id="masculino" name="genero" value="masculino">
    <label for="masculino">Masculino</label>
  </div>
  <div>
    <input type="radio" id="outro" name="genero" value="outro">
    <label for="outro">Outro</label>
  </div>
</fieldset>

<!-- Select (dropdown) -->
<label for="pais">País:</label>
<select id="pais" name="pais">
  <option value="">Selecione um país</option>
  <option value="br">Brasil</option>
  <option value="pt">Portugal</option>
  <option value="us">Estados Unidos</option>
</select>

<!-- Select múltiplo -->
<label for="linguagens">Linguagens:</label>
<select id="linguagens" name="linguagens" multiple size="3">
  <option value="html">HTML</option>
  <option value="css">CSS</option>
  <option value="js">JavaScript</option>
</select>

<!-- Agrupamento em select -->
<label for="categoria">Categoria:</label>
<select id="categoria" name="categoria">
  <optgroup label="Front-end">
    <option value="html">HTML</option>
    <option value="css">CSS</option>
    <option value="js">JavaScript</option>
  </optgroup>
  <optgroup label="Back-end">
    <option value="php">PHP</option>
    <option value="python">Python</option>
    <option value="ruby">Ruby</option>
  </optgroup>
</select>

<!-- Datalist (sugestões) -->
<label for="navegador">Navegador:</label>
<input list="navegadores" id="navegador" name="navegador">
<datalist id="navegadores">
  <option value="Chrome">
  <option value="Firefox">
  <option value="Safari">
  <option value="Edge">
</datalist>
```

### Arquivos e Botões

```html
<!-- Upload de arquivo -->
<label for="arquivo">Selecione um arquivo:</label>
<input type="file" id="arquivo" name="arquivo">

<!-- Upload múltiplo -->
<label for="fotos">Selecione fotos:</label>
<input type="file" id="fotos" name="fotos" multiple accept="image/*">

<!-- Botão de envio -->
<button type="submit">Enviar</button>

<!-- Botão de reset -->
<button type="reset">Limpar</button>

<!-- Botão genérico -->
<button type="button" onclick="minhaFuncao()">Clique Aqui</button>

<!-- Botão de imagem -->
<input type="image" src="botao.png" alt="Enviar" width="100" height="30">

<!-- Campo oculto -->
<input type="hidden" id="timestamp" name="timestamp" value="1698765432">
```

## Agrupamento e Organização

### Fieldset e Legend

```html
<fieldset>
  <legend>Informações Pessoais</legend>
  
  <div class="form-group">
    <label for="nome">Nome:</label>
    <input type="text" id="nome" name="nome">
  </div>
  
  <div class="form-group">
    <label for="email">Email:</label>
    <input type="email" id="email" name="email">
  </div>
</fieldset>
```

### Label

```html
<!-- Label associado por id (recomendado) -->
<label for="nome">Nome:</label>
<input type="text" id="nome" name="nome">

<!-- Label envolvendo o input -->
<label>
  Email:
  <input type="email" name="email">
</label>
```

## Estilização de Formulários

### Reset e Estilo Base

```css
/* Reset básico para formulários */
input,
button,
select,
textarea {
  font-family: inherit;
  font-size: 100%;
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}

/* Previne zoom em iOS */
input,
textarea,
select {
  font-size: 16px;
}

/* Estilo base */
.form-group {
  margin-bottom: 1rem;
}

label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: bold;
}

input[type="text"],
input[type="email"],
input[type="password"],
input[type="number"],
input[type="tel"],
input[type="url"],
textarea,
select {
  width: 100%;
  padding: 0.5rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  transition: border-color 0.3s, box-shadow 0.3s;
}

input:focus,
textarea:focus,
select:focus {
  border-color: #4a90e2;
  box-shadow: 0 0 0 3px rgba(74, 144, 226, 0.25);
  outline: none;
}

button {
  padding: 0.5rem 1rem;
  background-color: #4a90e2;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #3a80d2;
}

button:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}
```

### Checkbox e Radio Estilizados

```css
/* Esconde o input original */
.custom-checkbox input,
.custom-radio input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

/* Container para label e input */
.custom-checkbox,
.custom-radio {
  display: block;
  position: relative;
  padding-left: 35px;
  margin-bottom: 12px;
  cursor: pointer;
  user-select: none;
}

/* Estilo do indicador personalizado */
.custom-checkbox .checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 20px;
  width: 20px;
  background-color: #eee;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.custom-radio .radiomark {
  position: absolute;
  top: 0;
  left: 0;
  height: 20px;
  width: 20px;
  background-color: #eee;
  border: 1px solid #ddd;
  border-radius: 50%;
}

/* Hover */
.custom-checkbox:hover input ~ .checkmark,
.custom-radio:hover input ~ .radiomark {
  background-color: #ccc;
}

/* Checked */
.custom-checkbox input:checked ~ .checkmark,
.custom-radio input:checked ~ .radiomark {
  background-color: #4a90e2;
  border-color: #4a90e2;
}

/* Checkmark (√) */
.custom-checkbox .checkmark:after {
  content: "";
  position: absolute;
  display: none;
  left: 7px;
  top: 3px;
  width: 5px;
  height: 10px;
  border: solid white;
  border-width: 0 2px 2px 0;
  transform: rotate(45deg);
}

.custom-checkbox input:checked ~ .checkmark:after {
  display: block;
}

/* Radiomark (•) */
.custom-radio .radiomark:after {
  content: "";
  position: absolute;
  display: none;
  top: 6px;
  left: 6px;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: white;
}

.custom-radio input:checked ~ .radiomark:after {
  display: block;
}
```

htmlresponse-action-icon

```html
<!-- Uso dos estilos personalizados -->
<label class="custom-checkbox">
  Concordo com os termos
  <input type="checkbox" name="termos">
  <span class="checkmark"></span>
</label>

<label class="custom-radio">
  Opção 1
  <input type="radio" name="opcao" value="1">
  <span class="radiomark"></span>
</label>
```

### Estilizando Select

cssresponse-action-icon

```css
.custom-select {
  position: relative;
}

.custom-select select {
  appearance: none;
  -webkit-appearance: none;
  padding-right: 30px;
  background-color: white;
}

.custom-select::after {
  content: '▼';
  font-size: 0.8em;
  position: absolute;
  right: 10px;
  top: 50%;
  transform: translateY(-50%);
  pointer-events: none;
}
```

htmlresponse-action-icon

```html
<div class="custom-select">
  <select name="categoria">
    <option value="">Selecione uma categoria</option>
    <option value="1">Categoria 1</option>
    <option value="2">Categoria 2</option>
  </select>
</div>
```

### Input de Arquivo Personalizado

cssresponse-action-icon

```css
.custom-file {
  position: relative;
  display: inline-block;
  width: 100%;
}

.custom-file-input {
  position: absolute;
  left: 0;
  top: 0;
  opacity: 0;
  width: 100%;
  height: 100%;
  cursor: pointer;
}

.custom-file-label {
  display: block;
  padding: 10px 15px;
  background-color: #f8f8f8;
  border: 1px solid #ddd;
  border-radius: 4px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.custom-file-label::after {
  content: 'Procurar';
  position: absolute;
  right: 0;
  top: 0;
  bottom: 0;
  padding: 10px 15px;
  background-color: #e0e0e0;
  border-left: 1px solid #ddd;
  border-radius: 0 4px 4px 0;
}
```

htmlresponse-action-icon

```html
<div class="custom-file">
  <input type="file" class="custom-file-input" id="arquivo" name="arquivo">
  <label class="custom-file-label" for="arquivo">Escolher arquivo</label>
</div>
```

## Estados e Validação

### Estilos para Estados

cssresponse-action-icon

```css
/* Validação nativa */
input:valid {
  border-color: #28a745;
}

input:invalid {
  border-color: #dc3545;
}

/* Mensagem de erro */
.error-message {
  color: #dc3545;
  font-size: 0.875rem;
  margin-top: 0.25rem;
}

/* Classe para validação personalizada */
.is-valid {
  border-color: #28a745 !important;
}

.is-invalid {
  border-color: #dc3545 !important;
}

/* Input desabilitado */
input:disabled,
textarea:disabled,
select:disabled {
  background-color: #e9ecef;
  opacity: 0.7;
  cursor: not-allowed;
}

/* Input somente leitura */
input:read-only,
textarea:read-only {
  background-color: #f8f9fa;
}

/* Placeholder */
::placeholder {
  color: #6c757d;
  opacity: 0.7;
}
```

### Validação de Formulário com HTML5

htmlresponse-action-icon

```html
<form novalidate>
  <div class="form-group">
    <label for="email">Email:</label>
    <input 
      type="email" 
      id="email" 
      name="email" 
      required 
      pattern="[a-z0-9._%+-]+@[a-z0-9.-]+\.[a-z]{2,}$"
    >
    <div class="error-message" id="email-error"></div>
  </div>
  
  <div class="form-group">
    <label for="senha">Senha:</label>
    <input 
      type="password" 
      id="senha" 
      name="senha" 
      required 
      minlength="8"
    >
    <div class="error-message" id="senha-error"></div>
  </div>
  
  <button type="submit">Enviar</button>
</form>
```

javascriptresponse-action-icon

```javascript
// Validação básica com JavaScript
const form = document.querySelector('form');
const email = document.getElementById('email');
const senha = document.getElementById('senha');
const emailError = document.getElementById('email-error');
const senhaError = document.getElementById('senha-error');

form.addEventListener('submit', function(event) {
  let valid = true;
  
  // Validação de email
  if (!email.validity.valid) {
    valid = false;
    emailError.textContent = 'Por favor, insira um email válido.';
    email.classList.add('is-invalid');
  } else {
    emailError.textContent = '';
    email.classList.remove('is-invalid');
    email.classList.add('is-valid');
  }
  
  // Validação de senha
  if (senha.value.length < 8) {
    valid = false;
    senhaError.textContent = 'A senha deve ter pelo menos 8 caracteres.';
    senha.classList.add('is-invalid');
  } else {
    senhaError.textContent = '';
    senha.classList.remove('is-invalid');
    senha.classList.add('is-valid');
  }
  
  if (!valid) {
    event.preventDefault();
  }
});
```

## Layouts de Formulário

### Formulário em Linha

cssresponse-action-icon

```css
.form-inline {
  display: flex;
  flex-flow: row wrap;
  align-items: center;
}

.form-inline .form-group {
  display: flex;
  flex: 0 0 auto;
  margin-right: 1rem;
}

.form-inline label {
  margin-right: 0.5rem;
  margin-bottom: 0;
}

@media (max-width: 768px) {
  .form-inline {
    flex-direction: column;
    align-items: stretch;
  }
  
  .form-inline .form-group {
    margin-right: 0;
    margin-bottom: 1rem;
  }
}
```

### Formulário em Grid

cssresponse-action-icon

```css
.form-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 1rem;
}

.form-grid .span-2 {
  grid-column: span 2;
}

@media (max-width: 768px) {
  .form-grid .span-2 {
    grid-column: auto;
  }
}
```

### Formulário em Etapas

cssresponse-action-icon

```css
.form-steps {
  position: relative;
}

.step {
  display: none;
}

.step.active {
  display: block;
}

.step-buttons {
  display: flex;
  justify-content: space-between;
  margin-top: 20px;
}

.step-indicator {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.step-indicator .step-dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background-color: #ddd;
  margin: 0 5px;
}

.step-indicator .step-dot.active {
  background-color: #4a90e2;
}
```


