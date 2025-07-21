### **🔗 Links Essenciais Bootstrap:**

#### **Documentação Oficial:**

- **Bootstrap 5.3 Docs:** [https://getbootstrap.com/docs/5.3/getting-started/introduction/](https://getbootstrap.com/docs/5.3/getting-started/introduction/)
- **Components:** [https://getbootstrap.com/docs/5.3/components/alerts/](https://getbootstrap.com/docs/5.3/components/alerts/)
- **Utilities:** [https://getbootstrap.com/docs/5.3/utilities/api/](https://getbootstrap.com/docs/5.3/utilities/api/)
- **Examples:** [https://getbootstrap.com/docs/5.3/examples/](https://getbootstrap.com/docs/5.3/examples/)

#### **Ferramentas Visuais:**

- **Bootstrap Builder:** [https://bootstrap.build/](https://bootstrap.build/)
- **BootstrapMade:** [https://bootstrapmade.com/](https://bootstrapmade.com/)
- **Bootstrap Cheat Sheet:** [https://bootstrap-cheatsheet.themeselection.com/](https://bootstrap-cheatsheet.themeselection.com/)

#### **Ícones e Emojis:**

- **Font Awesome:** [https://fontawesome.com/search](https://fontawesome.com/search)
- **Bootstrap Icons:** [https://icons.getbootstrap.com/](https://icons.getbootstrap.com/)
- **Emojipedia:** [https://emojipedia.org/](https://emojipedia.org/)
- **Unicode Emoji:** [https://unicode.org/emoji/charts/full-emoji-list.html](https://unicode.org/emoji/charts/full-emoji-list.html)

## 📁 **09 - Emojis e Ícones Prontos**

### **🎯 Estratégia de Uso:**

#### **Font Awesome (Recomendado):**

```html
<!-- CDN no layout principal -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

<!-- Uso nos templates -->
<i class="fas fa-bicycle"></i>     <!-- Bicicleta sólida -->
<i class="far fa-heart"></i>       <!-- Coração vazio -->
<i class="fab fa-github"></i>      <!-- GitHub brand -->
```

#### **Emojis Unicode (Copy-Paste direto):**

```html
<!-- Países -->
🇵🇹 Portugal   🇪🇸 Espanha   🇫🇷 França   🇵🇱 Polónia

<!-- Transportes -->
🚴 Bicicleta   🚲 Bike   🏍️ Moto   🚗 Carro

<!-- Ações -->
✅ Sucesso   ❌ Erro   ⚠️ Aviso   ℹ️ Info
📝 Editar   👀 Ver   🗑️ Apagar   ➕ Adicionar

<!-- Pessoas -->
👤 Utilizador   👥 Utilizadores   🏠 Casa   📧 Email
📅 Data   💰 Preço   🎯 Objetivo   📊 Stats
```

## 📁 **10 - Componentes Bootstrap Copy-Paste**

### **🎯 Componentes Prontos para Usar:**

#### **Cards com Ícones:**

htmlresponse-action-icon

```html
<!-- Card básico -->
<div class="card shadow">
    <div class="card-header bg-primary text-white">
        <h5 class="mb-0"><i class="fas fa-ICON me-2"></i>TÍTULO</h5>
    </div>
    <div class="card-body">
        CONTEÚDO
    </div>
</div>

<!-- Card com footer -->
<div class="card shadow">
    <div class="card-body">CONTEÚDO</div>
    <div class="card-footer bg-light">FOOTER</div>
</div>
```

#### **Botões com Ícones:**

htmlresponse-action-icon

```html
<!-- Botão primário -->
<a href="#" class="btn btn-primary">
    <i class="fas fa-plus me-2"></i>Adicionar
</a>

<!-- Grupo de botões -->
<div class="btn-group" role="group">
    <a href="#" class="btn btn-sm btn-outline-primary"><i class="fas fa-eye"></i></a>
    <a href="#" class="btn btn-sm btn-outline-warning"><i class="fas fa-edit"></i></a>
    <a href="#" class="btn btn-sm btn-outline-danger"><i class="fas fa-trash"></i></a>
</div>
```

#### **Badges e Status:**

htmlresponse-action-icon

```html
<!-- Badges com cores -->
<span class="badge bg-primary">{{ $count }}</span>
<span class="badge bg-success">Active</span>
<span class="badge bg-warning text-dark">Pending</span>
<span class="badge bg-danger">Inactive</span>

<!-- Badge com ícone -->
<span class="badge bg-info">
    <i class="fas fa-bicycle me-1"></i>{{ $count }}
</span>
```

#### **Alertas com Ícones:**

htmlresponse-action-icon

```html
<!-- Sucesso -->
<div class="alert alert-success alert-dismissible fade show" role="alert">
    <i class="fas fa-check-circle me-2"></i>Operação realizada com sucesso!
    <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
</div>

<!-- Erro -->
<div class="alert alert-danger alert-dismissible fade show" role="alert">
    <i class="fas fa-exclamation-circle me-2"></i>Ocorreu um erro!
    <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
</div>
```

#### **Tabelas Estilizadas:**

htmlresponse-action-icon

```html
<div class="table-responsive">
    <table class="table table-striped table-hover">
        <thead class="table-dark">
            <tr>
                <th>ID</th>
                <th>Nome</th>
                <th>Ações</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>1</td>
                <td>Exemplo</td>
                <td>
                    <div class="btn-group">
                        <a href="#" class="btn btn-sm btn-outline-primary">
                            <i class="fas fa-eye"></i>
                        </a>
                    </div>
                </td>
            </tr>
        </tbody>
    </table>
</div>
```

#### **Formulários com Ícones:**

htmlresponse-action-icon

```html
<!-- Input com ícone -->
<div class="input-group mb-3">
    <span class="input-group-text"><i class="fas fa-search"></i></span>
    <input type="text" class="form-control" placeholder="Search...">
</div>

<!-- Select estilizado -->
<div class="mb-3">
    <label class="form-label">País</label>
    <select class="form-select">
        <option>Selecionar...</option>
        <option value="1">🇵🇹 Portugal</option>
        <option value="2">🇪🇸 Espanha</option>
    </select>
</div>
```

### **🎨 Cores Bootstrap:**

htmlresponse-action-icon

```html
<!-- Cores principais -->
bg-primary    text-primary    btn-primary
bg-secondary  text-secondary  btn-secondary
bg-success    text-success    btn-success
bg-danger     text-danger     btn-danger
bg-warning    text-warning    btn-warning
bg-info       text-info       btn-info
bg-light      text-light      btn-light
bg-dark       text-dark       btn-dark
```

### **📱 Classes Responsivas:**

htmlresponse-action-icon

```html
<!-- Esconder/mostrar por dispositivo -->
d-none d-md-block          <!-- Esconder mobile, mostrar desktop -->
d-block d-md-none          <!-- Mostrar mobile, esconder desktop -->

<!-- Grid responsivo -->
col-12 col-md-6 col-lg-4   <!-- Full mobile, half tablet, 1/3 desktop -->
col-sm-6 col-md-4 col-lg-3 <!-- Responsivo progressivo -->

<!-- Espaçamentos responsivos -->
p-2 p-md-4                 <!-- Padding 0.5rem mobile, 1.5rem desktop -->
mt-3 mt-lg-5               <!-- Margin-top diferentes por device -->
```