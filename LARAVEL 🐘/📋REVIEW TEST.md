# 🚴‍♂️ Projeto Laravel: Gestão de Bicicletas

## 📝 **Enunciado do Teste**
**Objetivo:** Criar sistema de gestão de bicicletas com Laravel 7.34.7 + Bootstrap 4.3

### 📌 **Requisitos Técnicos**
1. **Arquitetura Base**  
   - Laravel com autenticação (`php artisan ui bootstrap --auth`)  
   - Bootstrap 4.3 para frontend  

2. **Estrutura de Páginas**  
   - Layout principal em `resources/views/layouts/app.blade.php`  
   - Deve incluir navbar e footer  

3. **Base de Dados**  
   ```mermaid
   erDiagram
     COUNTRIES ||--o{ USERS : has
     USERS ||--o{ BICYCLES : owns
     ```


4. **Seeders/Factories**
    
    - 4 países fixos (Portugal, Espanha, França, Polónia)
        
    - 100 users (2 bicicletas cada)
        
5. **Rotas Obrigatórias**
    
    - `/countries` → Listagem
        
    - `/users` → Listagem paginada
        
    - `/bicycles` → Listagem com relacionamentos



## 1. Arquitetura de Software Base (Laravel com Auth e Bootstrap)

### Passo 1: Instalação e Configuração do Ambiente

bashresponse-action-icon

```bash
# Certifique-se que o XAMPP está instalado e os serviços Apache e MySQL estão ativos

# Navegue até a pasta htdocs do XAMPP
cd C:/xampp/htdocs

# Instale o Laravel 7.3 via Composer
composer create-project --prefer-dist laravel/laravel:^7.0 projeto-laravel

# Entre na pasta do projeto
cd projeto-laravel
```

### Passo 2: Configuração do Banco de Dados

Edite o arquivo .env na raiz do projeto:

textresponse-action-icon

```text
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=projeto_laravel
DB_USERNAME=root
DB_PASSWORD=
```

Crie o banco de dados no phpMyAdmin acessando http://localhost/phpmyadmin.

### Passo 3: Instalação do Laravel UI para Auth e Bootstrap

bashresponse-action-icon

```bash
# Instale o pacote Laravel UI
composer require laravel/ui:^2.4

# Instale o scaffold de autenticação com Bootstrap
php artisan ui bootstrap --auth

# Instale as dependências JavaScript
npm install

# Compile os assets
npm run dev
```

## 2. Definição da Master Page

### Passo 1: Crie um Layout Base

Crie/edite o arquivo resources/views/layouts/app.blade.php:

```html
<!DOCTYPE html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="csrf-token" content="{{ csrf_token() }}">

    <title>@yield('title', 'Projeto Laravel')</title>

    <!-- Fonts -->
    <link rel="dns-prefetch" href="//fonts.gstatic.com">
    <link href="https://fonts.googleapis.com/css?family=Nunito" rel="stylesheet">

    <!-- Styles -->
    <link href="{{ asset('css/app.css') }}" rel="stylesheet">
</head>
<body>
    <div id="app">
        <nav class="navbar navbar-expand-md navbar-light bg-white shadow-sm">
            <div class="container">
                <a class="navbar-brand" href="{{ url('/') }}">
                    {{ config('app.name', 'Laravel') }}
                </a>
                <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="{{ __('Toggle navigation') }}">
                    <span class="navbar-toggler-icon"></span>
                </button>

                <div class="collapse navbar-collapse" id="navbarSupportedContent">
                    <!-- Left Side Of Navbar -->
                    <ul class="navbar-nav mr-auto">
                        <li class="nav-item">
                            <a class="nav-link" href="{{ route('countries.index') }}">Países</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="{{ route('users.index') }}">Usuários</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="{{ route('bicycles.index') }}">Bicicletas</a>
                        </li>
                    </ul>

                    <!-- Right Side Of Navbar -->
                    <ul class="navbar-nav ml-auto">
                        <!-- Authentication Links -->
                        @guest
                            <li class="nav-item">
                                <a class="nav-link" href="{{ route('login') }}">{{ __('Login') }}</a>
                            </li>
                            @if (Route::has('register'))
                                <li class="nav-item">
                                    <a class="nav-link" href="{{ route('register') }}">{{ __('Register') }}</a>
                                </li>
                            @endif
                        @else
                            <li class="nav-item dropdown">
                                <a id="navbarDropdown" class="nav-link dropdown-toggle" href="#" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false" v-pre>
                                    {{ Auth::user()->name }} <span class="caret"></span>
                                </a>

                                <div class="dropdown-menu dropdown-menu-right" aria-labelledby="navbarDropdown">
                                    <a class="dropdown-item" href="{{ route('logout') }}"
                                       onclick="event.preventDefault();
                                                     document.getElementById('logout-form').submit();">
                                        {{ __('Logout') }}
                                    </a>

                                    <form id="logout-form" action="{{ route('logout') }}" method="POST" style="display: none;">
                                        @csrf
                                    </form>
                                </div>
                            </li>
                        @endguest
                    </ul>
                </div>
            </div>
        </nav>

        <main class="py-4">
            <div class="container">
                @yield('content')
            </div>
        </main>
    </div>

    <!-- Scripts -->
    <script src="{{ asset('js/app.js') }}"></script>
    @yield('scripts')
</body>
</html>
```

## 3. Migrações para as Tabelas (Countries, Users, Bicycles)

### Passo 1: Criação das Migrações

```bash
# Migração para Countries (a tabela de usuários já existe por padrão)
php artisan make:migration create_countries_table

# Migração para Bicycles
php artisan make:migration create_bicycles_table
```

### Passo 2: Definir a Estrutura das Migrações

Edite database/migrations/xxxx_xx_xx_create_countries_table.php:

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateCountriesTable extends Migration
{
    public function up()
    {
        Schema::create('countries', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->string('code', 3)->unique();
            $table->timestamps();
        });
    }

    public function down()
    {
        Schema::dropIfExists('countries');
    }
}
```

Modifique a migração de usuários para adicionar a relação com países:

```bash
php artisan make:migration add_country_id_to_users_table
```

Edite a nova migração:

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class AddCountryIdToUsersTable extends Migration
{
    public function up()
    {
        Schema::table('users', function (Blueprint $table) {
            $table->foreignId('country_id')->nullable()->constrained()->onDelete('set null');
        });
    }

    public function down()
    {
        Schema::table('users', function (Blueprint $table) {
            $table->dropForeign(['country_id']);
            $table->dropColumn('country_id');
        });
    }
}
```

Edite database/migrations/xxxx_xx_xx_create_bicycles_table.php:

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateBicyclesTable extends Migration
{
    public function up()
    {
        Schema::create('bicycles', function (Blueprint $table) {
            $table->id();
            $table->string('model');
            $table->foreignId('user_id')->constrained()->onDelete('cascade');
            $table->timestamps();
        });
    }

    public function down()
    {
        Schema::dropIfExists('bicycles');
    }
}
```

### Passo 3: Execução das Migrações

```bash
# Execute as migrações
php artisan migrate
```

## 4. Criação dos Modelos e Relações

### Passo 1: Criar os Modelos

```bash
# Criar modelo Country
php artisan make:model Country

# Modificar modelo User (já existe)

# Criar modelo Bicycle
php artisan make:model Bicycle
```

### Passo 2: Definir as Relações nos Modelos

Edite app/Country.php:

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Country extends Model
{
    protected $fillable = ['name', 'code'];

    public function users()
    {
        return $this->hasMany(User::class);
    }
}
```

Edite app/User.php:

```php
<?php

namespace App;

use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;

class User extends Authenticatable
{
    use Notifiable;

    protected $fillable = [
        'name', 'email', 'password', 'country_id',
    ];

    protected $hidden = [
        'password', 'remember_token',
    ];

    protected $casts = [
        'email_verified_at' => 'datetime',
    ];

    public function country()
    {
        return $this->belongsTo(Country::class);
    }

    public function bicycles()
    {
        return $this->hasMany(Bicycle::class);
    }
}
```

Edite app/Bicycle.php:

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Bicycle extends Model
{
    protected $fillable = ['model', 'user_id'];

    public function user()
    {
        return $this->belongsTo(User::class);
    }
}
```

## 5. Seed para Preencher Dados da Tabela Países

### Passo 1: Criar o Seeder

bashresponse-action-icon

```bash
php artisan make:seeder CountriesTableSeeder
```

### Passo 2: Definir os Dados no Seeder

Edite database/seeds/CountriesTableSeeder.php:

phpresponse-action-icon

```php
<?php

use Illuminate\Database\Seeder;
use App\Country;

class CountriesTableSeeder extends Seeder
{
    public function run()
    {
        $countries = [
            ['name' => 'Portugal', 'code' => 'PT'],
            ['name' => 'Espanha', 'code' => 'ES'],
            ['name' => 'França', 'code' => 'FR'],
            ['name' => 'Polónia', 'code' => 'PL'],
        ];

        foreach ($countries as $country) {
            Country::create($country);
        }
    }
}
```

### Passo 3: Registrar o Seeder no DatabaseSeeder

Edite database/seeds/DatabaseSeeder.php:

phpresponse-action-icon

```php
<?php

use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
    public function run()
    {
        $this->call(CountriesTableSeeder::class);
    }
}
```

### Passo 4: Executar o Seeder

bashresponse-action-icon

```bash
php artisan db:seed
```

## 6. Factory para Gerar 100 Users com País Associado

### Passo 1: Criar/Modificar a Factory para Users

Edite database/factories/UserFactory.php:

phpresponse-action-icon

```php
<?php

/** @var \Illuminate\Database\Eloquent\Factory $factory */

use App\User;
use App\Country;
use Faker\Generator as Faker;
use Illuminate\Support\Str;

$factory->define(User::class, function (Faker $faker) {
    $countries = Country::pluck('id')->toArray();
    
    return [
        'name' => $faker->name,
        'email' => $faker->unique()->safeEmail,
        'email_verified_at' => now(),
        'password' => '$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi', // password
        'remember_token' => Str::random(10),
        'country_id' => $faker->randomElement($countries),
    ];
});
```

### Passo 2: Criar um Seeder para Usuários

bashresponse-action-icon

```bash
php artisan make:seeder UsersTableSeeder
```

Edite database/seeds/UsersTableSeeder.php:

phpresponse-action-icon

```php
<?php

use Illuminate\Database\Seeder;
use App\User;

class UsersTableSeeder extends Seeder
{
    public function run()
    {
        factory(User::class, 100)->create();
    }
}
```

### Passo 3: Registrar o Seeder de Usuários

Edite database/seeds/DatabaseSeeder.php:

phpresponse-action-icon

```php
<?php

use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
    public function run()
    {
        $this->call(CountriesTableSeeder::class);
        $this->call(UsersTableSeeder::class);
    }
}
```

### Passo 4: Executar o Seeder

bashresponse-action-icon

```bash
php artisan db:seed --class=UsersTableSeeder
```

## 7. Factory para Gerar 200 Bicycles Associadas a Usuários

### Passo 1: Criar Factory para Bicycles

Crie o arquivo database/factories/BicycleFactory.php:

phpresponse-action-icon

```php
<?php

/** @var \Illuminate\Database\Eloquent\Factory $factory */

use App\Bicycle;
use App\User;
use Faker\Generator as Faker;

$factory->define(Bicycle::class, function (Faker $faker) {
    $users = User::pluck('id')->toArray();
    
    return [
        'model' => $faker->randomElement(['Mountain Bike', 'Road Bike', 'City Bike', 'Hybrid Bike', 'BMX']),
        'user_id' => $faker->randomElement($users),
    ];
});
```

### Passo 2: Criar um Seeder para Bicicletas

bashresponse-action-icon

```bash
php artisan make:seeder BicyclesTableSeeder
```

Edite database/seeds/BicyclesTableSeeder.php:

phpresponse-action-icon

```php
<?php

use Illuminate\Database\Seeder;
use App\Bicycle;
use App\User;

class BicyclesTableSeeder extends Seeder
{
    public function run()
    {
        // Garantir que cada usuário tenha pelo menos 2 bicicletas
        $users = User::all();
        
        foreach ($users as $user) {
            factory(Bicycle::class, 2)->create([
                'user_id' => $user->id
            ]);
        }
        
        // Adicionar mais bicicletas até chegar em 200 no total
        $totalBicycles = Bicycle::count();
        $remainingBicycles = 200 - $totalBicycles;
        
        if ($remainingBicycles > 0) {
            factory(Bicycle::class, $remainingBicycles)->create();
        }
    }
}
```

### Passo 3: Registrar o Seeder de Bicicletas

Edite database/seeds/DatabaseSeeder.php:

phpresponse-action-icon

```php
<?php

use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
    public function run()
    {
        $this->call(CountriesTableSeeder::class);
        $this->call(UsersTableSeeder::class);
        $this->call(BicyclesTableSeeder::class);
    }
}
```

### Passo 4: Executar o Seeder

bashresponse-action-icon

```bash
php artisan db:seed --class=BicyclesTableSeeder
```

## 8-9. Criação dos Controllers e Views para as Listagens

### Passo 1: Criar os Controllers

bashresponse-action-icon

```bash
php artisan make:controller CountryController --resource
php artisan make:controller UserController --resource
php artisan make:controller BicycleController --resource
```

### Passo 2: Implementar os Métodos de Listagem nos Controllers

Edite app/Http/Controllers/CountryController.php:

phpresponse-action-icon

```php
<?php

namespace App\Http\Controllers;

use App\Country;
use Illuminate\Http\Request;

class CountryController extends Controller
{
    public function index()
    {
        $countries = Country::withCount('users')->get();
        return view('countries.index', compact('countries'));
    }

    // Outros métodos aqui...
}
```

Edite app/Http/Controllers/UserController.php:

phpresponse-action-icon

```php
<?php

namespace App\Http\Controllers;

use App\User;
use Illuminate\Http\Request;

class UserController extends Controller
{
    public function index()
    {
        $users = User::with('country')->withCount('bicycles')->get();
        return view('users.index', compact('users'));
    }

    // Outros métodos aqui...
}
```

Edite app/Http/Controllers/BicycleController.php:

phpresponse-action-icon

```php
<?php

namespace App\Http\Controllers;

use App\Bicycle;
use Illuminate\Http\Request;

class BicycleController extends Controller
{
    public function index()
    {
        $bicycles = Bicycle::with('user.country')->get();
        return view('bicycles.index', compact('bicycles'));
    }

    // Outros métodos aqui...
}
```

### Passo 3: Criar as Rotas para as Listagens

Edite routes/web.php:

phpresponse-action-icon

```php
<?php

use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    return view('welcome');
});

Auth::routes();

Route::get('/home', 'HomeController@index')->name('home');

// Rotas para Countries
Route::get('/countries', 'CountryController@index')->name('countries.index');

// Rotas para Users
Route::get('/users', 'UserController@index')->name('users.index');

// Rotas para Bicycles
Route::get('/bicycles', 'BicycleController@index')->name('bicycles.index');
```

### Passo 4: Criar as Views para as Listagens

Crie o diretório resources/views/countries e o arquivo index.blade.php:

htmlresponse-action-icon

```html
@extends('layouts.app')

@section('title', 'Lista de Países')

@section('content')
    <div class="card">
        <div class="card-header">
            <h2>Lista de Países</h2>
        </div>
        <div class="card-body">
            <table class="table table-striped">
                <thead>
                    <tr>
                        <th>ID</th>
                        <th>Nome</th>
                        <th>Código</th>
                        <th>Qtd. Usuários</th>
                    </tr>
                </thead>
                <tbody>
                    @foreach($countries as $country)
                    <tr>
                        <td>{{ $country->id }}</td>
                        <td>{{ $country->name }}</td>
                        <td>{{ $country->code }}</td>
                        <td>{{ $country->users_count }}</td>
                    </tr>
                    @endforeach
                </tbody>
            </table>
        </div>
    </div>
@endsection
```

Crie o diretório resources/views/users e o arquivo index.blade.php:

htmlresponse-action-icon

```html
@extends('layouts.app')

@section('title', 'Lista de Usuários')

@section('content')
    <div class="card">
        <div class="card-header">
            <h2>Lista de Usuários</h2>
        </div>
        <div class="card-body">
            <table class="table table-striped">
                <thead>
                    <tr>
                        <th>ID</th>
                        <th>Nome</th>
                        <th>Email</th>
                        <th>País</th>
                        <th>Qtd. Bicicletas</th>
                    </tr>
                </thead>
                <tbody>
                    @foreach($users as $user)
                    <tr>
                        <td>{{ $user->id }}</td>
                        <td>{{ $user->name }}</td>
                        <td>{{ $user->email }}</td>
                        <td>{{ $user->country ? $user->country->name : 'N/A' }}</td>
                        <td>{{ $user->bicycles_count }}</td>
                    </tr>
                    @endforeach
                </tbody>
            </table>
        </div>
    </div>
@endsection
```

Crie o diretório resources/views/bicycles e o arquivo index.blade.php:

htmlresponse-action-icon

```html
@extends('layouts.app')

@section('title', 'Lista de Bicicletas')

@section('content')
    <div class="card">
        <div class="card-header">
            <h2>Lista de Bicicletas</h2>
        </div>
        <div class="card-body">
            <table class="table table-striped">
                <thead>
                    <tr>
                        <th>ID</th>
                        <th>Modelo</th>
                        <th>Usuário</th>
                        <th>País do Usuário</th>
                    </tr>
                </thead>
                <tbody>
                    @foreach($bicycles as $bicycle)
                    <tr>
                        <td>{{ $bicycle->id }}</td>
                        <td>{{ $bicycle->model }}</td>
                        <td>{{ $bicycle->user->name }}</td>
                        <td>{{ $bicycle->user->country ? $bicycle->user->country->name : 'N/A' }}</td>
                    </tr>
                    @endforeach
                </tbody>
            </table>
        </div>
    </div>
@endsection
```