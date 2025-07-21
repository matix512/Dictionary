
 Guia prático de comandos e fluxos para Laravel 7  
> Ideal para uso com XAMPP, Composer e terminal local  
> Compatível com PHP 7.3+

---

## 📦 INSTALAÇÃO E SETUP

```bash
# Instalar Laravel globalmente (opcional)
composer global require laravel/installer  

# Criar um novo projeto Laravel
composer create-project --prefer-dist laravel/laravel:^7.0 nomedoprojeto

# iniciar servidor
php artisan serve
```


## ⚙️ CONFIGURAÇÃO DB

```bash
# Abrir o ficheiro .env (environment) e alterar a database depois de previamente ter sido criada a mesma no phpmyadmin
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=personpet
DB_USERNAME=root
DB_PASSWORD=
```


## 👤 AUTENTICAÇÃO 

```bash
# package de autenticação:  
composer require laravel/ui ^2.4

# instalar o package:  
php artisan ui vue --auth  
npm install  
npm run dev
```

## **Base de Dados:**

```bash
# Migrations
php artisan make:migration create_table_name
php artisan migrate
php artisan migrate:fresh        # Reset tudo
php artisan migrate:fresh --seed # Reset + seeds
php artisan migrate:status       # Ver status

# Seeds
php artisan make:seeder NameSeeder
php artisan db:seed
php artisan db:seed --class=NameSeeder

# Factories
php artisan make:factory NameFactory
```

### **🔍 Debug Commands:**

```bash
# Ver rotas
php artisan route:list
php artisan route:list --name=users

# Tinker (console interativo)
php artisan tinker
> User::count()
> User::first()
> User::where('email', 'test@test.com')->first()

# Logs
tail -f storage/logs/laravel.log
```

### **🐛 Laravel Debugging:**

#### **dd() e dump():**

```php
// Controller
public function index()
{
    $data = User::all();
    dd($data);              // Die and dump
    dump($data);            // Dump and continue
    return view('users.index', compact('data'));
}

// Blade
{{ dd($variable) }}
{{ dump($variable) }}
```

### **🚨 Problemas Comuns:**

#### **Bootstrap não funciona:**


```html
<!-- Verificar CDN no head -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
<!-- Antes do </body> -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
```

#### **Ícones não aparecem:**

```html
<!-- Font Awesome CDN -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
```

#### **Rotas não funcionam:**

```bash
# Limpar cache
php artisan route:clear

# Ver rotas disponíveis
php artisan route:list
```

#### **Base de dados:**

```bash
# Verificar conexão
php artisan tinker
> DB::connection()->getPdo()

# Ver migrations
php artisan migrate:status
```