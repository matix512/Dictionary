
 Guia prático de comandos e fluxos para Laravel 7  
> Ideal para uso com Laragon, Composer e terminal local  
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
# Copiar ficheiro de ambiente de exemplo
cp .env.example .env

# Gerar chave da aplicação
php artisan key:generate
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

