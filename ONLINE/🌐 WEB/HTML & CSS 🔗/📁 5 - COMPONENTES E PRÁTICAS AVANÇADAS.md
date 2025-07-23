
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



