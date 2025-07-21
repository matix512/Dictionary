
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

