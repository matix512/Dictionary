
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

```html
<div class="custom-file">
  <input type="file" class="custom-file-input" id="arquivo" name="arquivo">
  <label class="custom-file-label" for="arquivo">Escolher arquivo</label>
</div>
```

## Estados e Validação

### Estilos para Estados

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


## Acessibilidade em Formulários

### Práticas Recomendadas

```markdown
<!-- Use labels explicitamente associados aos inputs -->
<label for="nome">Nome:</label>
<input type="text" id="nome" name="nome">

<!-- Use atributos aria para melhor suporte a leitores de tela -->
<div class="form-group">
  <label for="senha">Senha:</label>
  <input 
    type="password" 
    id="senha" 
    name="senha" 
    aria-required="true" 
    aria-describedby="senha-help"
  >
  <div id="senha-help" class="help-text">Senha deve ter pelo menos 8 caracteres.</div>
</div>

<!-- Agrupe campos relacionados com fieldset e legend -->
<fieldset>
  <legend>Endereço de Entrega</legend>
  <!-- campos de endereço -->
</fieldset>

<!-- Indique campos obrigatórios -->
<label for="email">Email: <span class="required">*</span></label>
<input type="email" id="email" name="email" required>

<div class="required-note">* Campos obrigatórios</div>

<!-- Forneça feedback claro para erros -->
<div class="form-group has-error">
  <label for="telefone">Telefone:</label>
  <input 
    type="tel" 
    id="telefone" 
    name="telefone"
    aria-invalid="true"
    aria-describedby="telefone-error"
  >
  <div id="telefone-error" class="error-message" role="alert">
    Por favor, digite um número de telefone válido.
  </div>
</div>

<!-- Uso adequado de botões -->
<button type="submit" aria-label="Enviar formulário">Enviar</button>
<button type="button" aria-label="Cancelar e limpar formulário">Cancelar</button>
```

### CSS para Acessibilidade

```css
/* Destacar o foco claramente */
input:focus,
textarea:focus,
select:focus,
button:focus {
  outline: 2px solid #4a90e2;
  outline-offset: 2px;
}

/* Contraste adequado */
label {
  color: #333; /* Contraste suficiente */
}

.help-text,
::placeholder {
  color: #555; /* Não use cinza claro que é difícil de ler */
}

/* Indicador de campo obrigatório */
.required {
  color: #dc3545;
}

.required-note {
  font-size: 0.8rem;
  margin-top: 1rem;
  color: #555;
}

/* Tamanhos de toque adequados para mobile */
@media (max-width: 768px) {
  button,
  input,
  select {
    min-height: 44px; /* Recomendação WCAG */
  }
  
  [type="checkbox"],
  [type="radio"] {
    min-height: 0;
  }
  
  .custom-checkbox,
  .custom-radio {
    min-height: 44px;
    display: flex;
    align-items: center;
  }
}
```

## Exemplos Práticos

### Formulário de Login

```html
<form class="login-form">
  <h2>Entrar</h2>
  
  <div class="form-group">
    <label for="login-email">Email:</label>
    <input 
      type="email" 
      id="login-email" 
      name="email" 
      required 
      autocomplete="username"
    >
  </div>
  
  <div class="form-group">
    <label for="login-senha">Senha:</label>
    <div class="password-field">
      <input 
        type="password" 
        id="login-senha" 
        name="senha" 
        required 
        autocomplete="current-password"
      >
      <button type="button" class="toggle-password" aria-label="Mostrar senha">
        <span class="icon">👁️</span>
      </button>
    </div>
  </div>
  
  <div class="form-group remember-me">
    <label class="custom-checkbox">
      Lembrar de mim
      <input type="checkbox" name="remember">
      <span class="checkmark"></span>
    </label>
    <a href="#" class="forgot-password">Esqueceu a senha?</a>
  </div>
  
  <button type="submit" class="btn-submit">Entrar</button>
  
  <div class="form-footer">
    Não tem uma conta? <a href="#">Cadastre-se</a>
  </div>
</form>
```


```css
.login-form {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: #fff;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.login-form h2 {
  text-align: center;
  margin-bottom: 20px;
}

.password-field {
  position: relative;
}

.toggle-password {
  position: absolute;
  right: 10px;
  top: 50%;
  transform: translateY(-50%);
  background: none;
  border: none;
  cursor: pointer;
  font-size: 16px;
  padding: 0;
}

.remember-me {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.btn-submit {
  width: 100%;
  padding: 12px;
  margin-top: 10px;
}

.form-footer {
  text-align: center;
  margin-top: 20px;
  font-size: 0.9rem;
}
```

### Formulário de Contato Responsivo

```html
<form class="contact-form">
  <h2>Entre em Contato</h2>
  
  <div class="form-row">
    <div class="form-group">
      <label for="nome">Nome:</label>
      <input type="text" id="nome" name="nome" required>
    </div>
    
    <div class="form-group">
      <label for="email">Email:</label>
      <input type="email" id="email" name="email" required>
    </div>
  </div>
  
  <div class="form-group">
    <label for="assunto">Assunto:</label>
    <input type="text" id="assunto" name="assunto" required>
  </div>
  
  <div class="form-group">
    <label for="mensagem">Mensagem:</label>
    <textarea id="mensagem" name="mensagem" rows="5" required></textarea>
  </div>
  
  <div class="form-group">
    <label class="custom-checkbox">
      Desejo receber notícias e atualizações
      <input type="checkbox" name="newsletter">
      <span class="checkmark"></span>
    </label>
  </div>
  
  <div class="form-row">
    <button type="reset" class="btn-secondary">Limpar</button>
    <button type="submit" class="btn-primary">Enviar Mensagem</button>
  </div>
</form>
```

```css
.contact-form {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}

.form-row {
  display: flex;
  gap: 20px;
  margin-bottom: 15px;
}

.form-row .form-group {
  flex: 1;
}

.btn-primary,
.btn-secondary {
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.btn-primary {
  background-color: #4a90e2;
  color: white;
}

.btn-secondary {
  background-color: #f1f1f1;
  color: #333;
}

@media (max-width: 768px) {
  .form-row {
    flex-direction: column;
    gap: 0;
  }
}
```

### Formulário de Registro com Validação

```html
<form class="signup-form" novalidate>
  <h2>Criar Conta</h2>
  
  <div class="form-group">
    <label for="signup-nome">Nome Completo:</label>
    <input 
      type="text" 
      id="signup-nome" 
      name="nome" 
      required
      minlength="3"
      autocomplete="name"
    >
    <div class="error-message"></div>
  </div>
  
  <div class="form-group">
    <label for="signup-email">Email:</label>
    <input 
      type="email" 
      id="signup-email" 
      name="email" 
      required
      autocomplete="email"
    >
    <div class="error-message"></div>
  </div>
  
  <div class="form-group">
    <label for="signup-senha">Senha:</label>
    <input 
      type="password" 
      id="signup-senha" 
      name="senha" 
      required
      minlength="8"
      aria-describedby="password-requirements"
      autocomplete="new-password"
    >
    <div id="password-requirements" class="help-text">
      Senha deve conter pelo menos 8 caracteres, incluindo letras maiúsculas,
      minúsculas, números e símbolos.
    </div>
    <div class="error-message"></div>
  </div>
  
  <div class="form-group">
    <label for="signup-confirmar">Confirmar Senha:</label>
    <input 
      type="password" 
      id="signup-confirmar" 
      name="confirmar_senha" 
      required
      autocomplete="new-password"
    >
    <div class="error-message"></div>
  </div>
  
  <div class="form-group">
    <label class="custom-checkbox">
      Concordo com os <a href="#">Termos e Condições</a>
      <input type="checkbox" name="termos" required>
      <span class="checkmark"></span>
    </label>
    <div class="error-message"></div>
  </div>
  
  <button type="submit" class="btn-submit">Criar Conta</button>
  
  <div class="form-footer">
    Já tem uma conta? <a href="#">Faça login</a>
  </div>
</form>
```

```css
.signup-form {
  max-width: 500px;
  margin: 0 auto;
  padding: 25px;
  background: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 15px rgba(0, 0, 0, 0.1);
}

.help-text {
  font-size: 0.8rem;
  color: #666;
  margin-top: 5px;
}

.error-message {
  color: #dc3545;
  font-size: 0.85rem;
  margin-top: 5px;
  min-height: 20px;
}

/* Estilos para validação visual */
input.is-valid {
  border-color: #28a745;
  background-image: url("data:image/svg+xml,..."); /* Ícone de verificação */
  background-repeat: no-repeat;
  background-position: right 10px center;
  background-size: 20px;
  padding-right: 40px;
}

input.is-invalid {
  border-color: #dc3545;
  background-image: url("data:image/svg+xml,..."); /* Ícone de erro */
  background-repeat: no-repeat;
  background-position: right 10px center;
  background-size: 20px;
  padding-right: 40px;
}
```

## Considerações de Segurança

### Proteção Contra Ataques

1. **Nunca confie em dados do cliente**: Sempre valide no servidor
2. **Proteja contra CSRF**: Use tokens CSRF
3. **Limite uploads**: Verifique tamanho e tipo de arquivos
4. **Sanitize inputs**: Evite XSS e injeção de código
5. **Use HTTPS**: Proteja dados durante a transmissão
6. **Implementar CAPTCHA/reCAPTCHA**: Para formulários públicos
7. **Limite tentativas**: Evite força bruta em formulários de login

### GDPR e Privacidade

1. Inclua avisos claros sobre o uso dos dados
2. Obtenha consentimento explícito
3. Ofereça opção de opt-out para newsletters
4. Não colete mais dados do que o necessário
5. Informe sobre cookies e rastreamento

---


# 📄 16 - ANIMAÇÕES & TRANSIÇÕES

## Transições Básicas

### Propriedade transition
```css
/* Sintaxe básica */
transition: propriedade duração timing-function delay;

/* Exemplos */
.elemento {
  /* Transição simples */
  transition: color 0.3s ease;
  
  /* Transição múltipla */
  transition: color 0.3s ease, background-color 0.5s ease;
  
  /* Transição para todas as propriedades */
  transition: all 0.3s ease-in-out;
  
  /* Com delay */
  transition: transform 0.5s ease-out 0.2s;
}
````

### Propriedades individuais

```css
.elemento {
  transition-property: opacity, transform;  /* Propriedades que terão transição */
  transition-duration: 0.3s, 0.5s;          /* Duração para cada propriedade */
  transition-timing-function: ease, linear; /* Função de tempo para cada propriedade */
  transition-delay: 0s, 0.2s;               /* Delay para cada propriedade */
}
```

### Funções de Tempo (timing-functions)

```css
.elemento {
  /* Predefinidas */
  transition-timing-function: ease;        /* Início lento, meio rápido, fim lento (padrão) */
  transition-timing-function: linear;      /* Velocidade constante */
  transition-timing-function: ease-in;     /* Início lento */
  transition-timing-function: ease-out;    /* Fim lento */
  transition-timing-function: ease-in-out; /* Início e fim lentos */
  transition-timing-function: step-end;    /* Mudança em etapas no final */
  transition-timing-function: step-start;  /* Mudança em etapas no início */
  
  /* Função cubic-bezier personalizada */
  transition-timing-function: cubic-bezier(0.68, -0.55, 0.27, 1.55); /* Elastico */
  
  /* Função steps */
  transition-timing-function: steps(5, end);  /* 5 etapas discretas */
}
```

## Propriedades Comuns para Transição

```css
/* Cores */
.hover-color {
  color: #333;
  transition: color 0.3s ease;
}
.hover-color:hover {
  color: #0066cc;
}

/* Background */
.hover-bg {
  background-color: #f0f0f0;
  transition: background-color 0.5s ease;
}
.hover-bg:hover {
  background-color: #e0e0ff;
}

/* Opacidade */
.fade {
  opacity: 0.7;
  transition: opacity 0.3s ease;
}
.fade:hover {
  opacity: 1;
}

/* Dimensões */
.grow {
  width: 100px;
  height: 100px;
  transition: width 0.3s, height 0.3s;
}
.grow:hover {
  width: 120px;
  height: 120px;
}

/* Transformações */
.scale {
  transform: scale(1);
  transition: transform 0.3s ease;
}
.scale:hover {
  transform: scale(1.2);
}

.rotate {
  transform: rotate(0deg);
  transition: transform 0.5s ease;
}
.rotate:hover {
  transform: rotate(360deg);
}

/* Posição */
.move {
  position: relative;
  left: 0;
  transition: left 0.5s ease-in-out;
}
.move:hover {
  left: 50px;
}

/* Sombras */
.shadow {
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
  transition: box-shadow 0.3s ease;
}
.shadow:hover {
  box-shadow: 0 5px 15px rgba(0,0,0,0.3);
}

/* Bordas */
.border {
  border: 1px solid #ccc;
  transition: border 0.3s ease;
}
.border:hover {
  border: 1px solid #0066cc;
}
```

## Animações com @keyframes

### Sintaxe Básica
```css
/* Definição da animação */
@keyframes nome-da-animacao {
  0% {
    /* Estilos no início da animação */
  }
  50% {
    /* Estilos no meio da animação */
  }
  100% {
    /* Estilos no final da animação */
  }
}

/* Aplicação da animação */
.elemento {
  animation: nome-da-animacao duração timing-function delay iterações direção fill-mode play-state;
}
```

### Exemplos de Keyframes

```css
/* Animação de Fade In */
@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

/* Animação de Pulso */
@keyframes pulse {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.1);
  }
  100% {
    transform: scale(1);
  }
}

/* Animação de Shake */
@keyframes shake {
  0%, 100% {
    transform: translateX(0);
  }
  10%, 30%, 50%, 70%, 90% {
    transform: translateX(-10px);
  }
  20%, 40%, 60%, 80% {
    transform: translateX(10px);
  }
}

/* Animação de Rotação */
@keyframes rotate {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

/* Animação de Slide In */
@keyframes slideIn {
  from {
    transform: translateY(50px);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}

/* Animação de Bounce */
@keyframes bounce {
  0%, 20%,
```