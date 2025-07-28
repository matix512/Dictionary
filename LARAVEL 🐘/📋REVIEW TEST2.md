
## 1. Configuração Inicial

### Passo 1: Criar Projeto e Configurar Banco de Dados

```bash
# Criar projeto Laravel (ou usar existente)
composer create-project --prefer-dist laravel/laravel nome-projeto

# Configurar o arquivo .env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=nome_do_banco   # Certifique-se que este banco existe!
DB_USERNAME=root
DB_PASSWORD=
```

### Passo 2: Instalar Autenticação (se necessário)

```bash
composer require laravel/ui
php artisan ui bootstrap --auth
npm install && npm run dev
```

## 2. Criação dos Modelos e Recursos (-a flag)

### Passo 1: Criar Modelos com Todos os Recursos

```bash
# Isto cria modelo, migração, factory, seeder e controller
php artisan make:model NomeModel -a
```

### Passo 2: Ordem de Criação (IMPORTANTE!)

- Sempre crie primeiro os modelos que não dependem de outros (tabelas independentes)
- Depois crie os modelos com relacionamentos

```bash
# Exemplo: primeiro crie Brand (independente)
php artisan make:model Brand -a

# Depois crie Car (depende de Brand)
php artisan make:model Car -a
```

## 3. Migrações - Atenção Especial

### Passo 1: Definição das Tabelas

```php
// Exemplo para tabela brands
Schema::create('brands', function (Blueprint $table) {
    $table->id();
    $table->string('name')->unique();
    $table->timestamps(); // Não esqueça disso se o exercício pedir created_at/updated_at
});

// Exemplo para tabela cars com chave estrangeira
Schema::create('cars', function (Blueprint $table) {
    $table->id();
    $table->string('registration')->unique();
    $table->year('year_of_manufacture');
    $table->string('color');
    
    // Use unsignedBigInteger se tiver problemas com foreignId
    $table->unsignedBigInteger('brand_id');
    $table->foreign('brand_id')->references('id')->on('brands')->onDelete('cascade');
    
    // Ou use a sintaxe mais compacta (preferível):
    // $table->foreignId('brand_id')->constrained()->onDelete('cascade');
    
    $table->timestamps();
});
```

### Passo 2: Solução para Erros de Chave Estrangeira

Se encontrar erros de chave estrangeira:

```php
public function up()
{
    Schema::disableForeignKeyConstraints();
    
    // Suas migrações aqui...
    
    Schema::enableForeignKeyConstraints();
}

public function down()
{
    Schema::disableForeignKeyConstraints();
    
    // Dropagem de tabelas em ordem inversa!
    Schema::dropIfExists('cars');
    Schema::dropIfExists('brands');
    
    Schema::enableForeignKeyConstraints();
}
```

## 4. Modelos e Relacionamentos

### Passo 1: Definir Atributos Preenchíveis

```php
class Brand extends Model
{
    protected $fillable = ['name'];
    
    // Relacionamentos aqui...
}

class Car extends Model
{
    protected $fillable = ['registration', 'year_of_manufacture', 'color', 'brand_id'];
    
    // Relacionamentos aqui...
}
```

### Passo 2: Definir Relacionamentos

```php
// Em Brand.php (Um para Muitos)
public function cars()
{
    return $this->hasMany(Car::class);
}

// Em Car.php (Muitos para Um)
public function brand()
{
    return $this->belongsTo(Brand::class);
}
```

## 5. Factories

### Passo 1: Configurar Factories

```php
// BrandFactory.php
$factory->define(Brand::class, function (Faker $faker) {
    return [
        'name' => $faker->unique()->company,
    ];
});

// CarFactory.php
$factory->define(Car::class, function (Faker $faker) {
    return [
        'registration' => strtoupper($faker->unique()->bothify('??-##-??')),
        'year_of_manufacture' => $faker->numberBetween(2000, 2023),
        'color' => $faker->colorName,
        'brand_id' => function () {
            return Brand::inRandomOrder()->first()->id;
        },
    ];
});
```

### Passo 2: Métodos Faker Úteis

- $faker->name - Nome completo
- $faker->company - Nome de empresa
- $faker->sentence - Frase
- $faker->paragraph - Parágrafo
- $faker->numberBetween(min, max) - Número entre min e max
- $faker->randomElement(['a', 'b', 'c']) - Item aleatório de array
- $faker->date - Data aleatória
- $faker->bothify('??-##-??') - Substitui ? por letras e # por números
- $faker->unique()->x - Garante valores únicos
- $faker->colorName - Nome de cor
- $faker->email - Email

## 6. Seeders

### Passo 1: Criar Dados Fixos

```php
// BrandSeeder.php
public function run()
{
    $brands = [
        ['name' => 'BMW'],
        ['name' => 'Renault'],
        // Mais marcas...
    ];

    foreach ($brands as $brand) {
        Brand::create($brand);
    }
}
```

### Passo 2: Usar Factories

```php
// CarSeeder.php
public function run()
{
    factory(Car::class, 100)->create();
}
```

### Passo 3: Garantir Requisitos Específicos (ex: 2 carros por marca)

```php
public function run()
{
    // Cada marca terá pelo menos 2 carros
    $brands = Brand::all();
    
    foreach ($brands as $brand) {
        factory(Car::class, 2)->create([
            'brand_id' => $brand->id
        ]);
    }
    
    // Adicionar mais carros até chegar em 100 no total
    $totalCars = Car::count();
    $remainingCars = 100 - $totalCars;
    
    if ($remainingCars > 0) {
        factory(Car::class, $remainingCars)->create();
    }
}
```

### Passo 4: Registrar no DatabaseSeeder

```php
// DatabaseSeeder.php
public function run()
{
    $this->call(BrandSeeder::class);
    $this->call(CarSeeder::class);
}
```

## 7. Controllers

### Passo 1: Método de Listagem (index)

```php
// BrandController.php
public function index()
{
    $brands = Brand::withCount('cars')->get();
    return view('brands.index', compact('brands'));
}

// CarController.php
public function index()
{
    $cars = Car::with('brand')->get();
    return view('cars.index', compact('cars'));
}
```

## 8. Rotas

### Passo 1: Definir Rotas de Listagem

```php
// web.php
Route::get('/brands', 'BrandController@index')->name('brands.index');
Route::get('/cars', 'CarController@index')->name('cars.index');
```

## 9. Views

### Passo 1: Criar Layout Base

```html
<!-- resources/views/layouts/app.blade.php -->
<!DOCTYPE html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>@yield('title', 'App Name')</title>
    <link href="{{ asset('css/app.css') }}" rel="stylesheet">
</head>
<body>
    <nav class="navbar navbar-expand-md navbar-light bg-white shadow-sm">
        <div class="container">
            <a class="navbar-brand" href="{{ url('/') }}">{{ config('app.name', 'Laravel') }}</a>
            <div class="collapse navbar-collapse">
                <ul class="navbar-nav mr-auto">
                    <li class="nav-item">
                        <a class="nav-link" href="{{ route('brands.index') }}">Brands</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="{{ route('cars.index') }}">Cars</a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>

    <main class="py-4">
        <div class="container">
            @yield('content')
        </div>
    </main>
    
    <script src="{{ asset('js/app.js') }}"></script>
</body>
</html>
```

### Passo 2: Criar Views de Listagem

```html
<!-- resources/views/brands/index.blade.php -->
@extends('layouts.app')

@section('title', 'Brands List')

@section('content')
<div class="card">
    <div class="card-header">Brands List</div>
    <div class="card-body">
        <table class="table table-striped">
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Name</th>
                    <th>Cars Count</th>
                    <th>Created At</th>
                </tr>
            </thead>
            <tbody>
                @foreach($brands as $brand)
                <tr>
                    <td>{{ $brand->id }}</td>
                    <td>{{ $brand->name }}</td>
                    <td>{{ $brand->cars_count }}</td>
                    <td>{{ $brand->created_at }}</td>
                </tr>
                @endforeach
            </tbody>
        </table>
    </div>
</div>
@endsection
```

## 10. Checklist Final

Antes de entregar, verifique:

- [ ]  Banco de dados existe e está configurado corretamente no .env
- [ ]  Migrations estão na ordem correta
- [ ]  Relacionamentos estão definidos nos modelos
- [ ]  Factories estão configuradas corretamente
- [ ]  Seeders estão registrados no DatabaseSeeder
- [ ]  Controllers têm os métodos necessários
- [ ]  Rotas estão definidas corretamente
- [ ]  Views exibem os dados relacionados
- [ ]  Requisitos específicos (como "2 carros por marca") estão implementados

## 11. Comandos Úteis para Depuração

bashresponse-action-icon

```bash
# Verificar rotas
php artisan route:list

# Limpar cache
php artisan config:clear
php artisan cache:clear
php artisan view:clear
php artisan route:clear

# Resetar banco de dados e executar migrations + seeders
php artisan migrate:fresh --seed

# Verificar artisan tinker para testar queries
php artisan tinker
```

## 12. Erros Comuns e Soluções

1. **Foreign key constraint is incorrectly formed**
    
    - Verifique a ordem das migrações
    - Use Schema::disableForeignKeyConstraints()
    - Verifique compatibilidade dos tipos de colunas
2. **Unknown database**
    
    - Verifique se o banco existe
    - Verifique configuração no .env
    - Use php artisan config:clear
3. **Route [x] not defined**
    
    - Verifique se a rota está registrada em web.php
    - Verifique se está usando o nome correto nas views
4. **View [x] not found**
    
    - Verifique se a pasta e arquivo existem
    - Verifique a hierarquia de pastas






# Solução Completa - Aplicação de Gestão de Projetos de Design de Interiores

## 1. Configuração Inicial [0.5 valores]

```bash
# Criar projeto Laravel
composer create-project laravel/laravel design-interiores

# Instalar Laravel UI e Bootstrap
composer require laravel/ui
php artisan ui bootstrap --auth
npm install && npm run dev
```

## 2. Master Page (Layout) [0.5 valores]

**resources/views/layouts/app.blade.php**
```blade
<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>@yield('title', 'Design de Interiores')</title>
    @vite(['resources/css/app.css', 'resources/js/app.js'])
</head>
<body>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
        <div class="container">
            <a class="navbar-brand" href="/">Design Interiores</a>
            <div class="navbar-nav ms-auto">
                <a class="nav-link" href="{{ route('projects.index') }}">Projetos</a>
                <a class="nav-link" href="{{ route('products.index') }}">Produtos</a>
            </div>
        </div>
    </nav>

    <main class="py-4">
        <div class="container">
            @yield('content')
        </div>
    </main>
</body>
</html>
```

## 3. Migrações [2 valores]

**database/migrations/create_categories_table.php**
```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up()
    {
        Schema::create('categories', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->timestamps();
        });
    }

    public function down()
    {
        Schema::dropIfExists('categories');
    }
};
```

**database/migrations/create_projects_table.php**
```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up()
    {
        Schema::create('projects', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->timestamps();
        });
    }

    public function down()
    {
        Schema::dropIfExists('projects');
    }
};
```

**database/migrations/create_products_table.php**
```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up()
    {
        Schema::create('products', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->text('details');
            $table->foreignId('category_id')->constrained();
            $table->foreignId('project_id')->constrained();
            $table->timestamps();
        });
    }

    public function down()
    {
        Schema::dropIfExists('products');
    }
};
```

## Modelos com Relações

**app/Models/Category.php**
```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Category extends Model
{
    use HasFactory;

    protected $fillable = ['name'];

    public function products()
    {
        return $this->hasMany(Product::class);
    }
}
```

**app/Models/Project.php**
```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Project extends Model
{
    use HasFactory;

    protected $fillable = ['name'];

    public function products()
    {
        return $this->hasMany(Product::class);
    }
}
```

**app/Models/Product.php**
```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Product extends Model
{
    use HasFactory;

    protected $fillable = ['name', 'details', 'category_id', 'project_id'];

    public function category()
    {
        return $this->belongsTo(Category::class);
    }

    public function project()
    {
        return $this->belongsTo(Project::class);
    }
}
```

## 4. Seeder para Categorias [2 valores]

**database/seeders/CategorySeeder.php**
```php
<?php

namespace Database\Seeders;

use Illuminate\Database\Seeder;
use App\Models\Category;

class CategorySeeder extends Seeder
{
    public function run()
    {
        $categories = [
            'Quarto',
            'Sala',
            'Cozinha',
            'Casa de Banho'
        ];

        foreach ($categories as $category) {
            Category::create(['name' => $category]);
        }
    }
}
```

## 5. Factory para Produtos [2 valores]

**database/factories/ProductFactory.php**
```php
<?php

namespace Database\Factories;

use Illuminate\Database\Eloquent\Factories\Factory;
use App\Models\Category;
use App\Models\Project;

class ProductFactory extends Factory
{
    public function definition()
    {
        return [
            'name' => $this->faker->words(3, true),
            'details' => $this->faker->paragraph(),
            'category_id' => Category::inRandomOrder()->first()->id ?? 1,
            'project_id' => Project::inRandomOrder()->first()->id ?? 1,
        ];
    }
}
```

## 6. Factory para Projetos [2 valores]

**database/factories/ProjectFactory.php**
```php
<?php

namespace Database\Factories;

use Illuminate\Database\Eloquent\Factories\Factory;

class ProjectFactory extends Factory
{
    public function definition()
    {
        return [
            'name' => 'Projeto ' . $this->faker->company(),
        ];
    }
}
```

**database/seeders/DatabaseSeeder.php**
```php
<?php

namespace Database\Seeders;

use Illuminate\Database\Seeder;
use App\Models\Project;
use App\Models\Product;

class DatabaseSeeder extends Seeder
{
    public function run()
    {
        // Primeiro, executar o seeder de categorias
        $this->call(CategorySeeder::class);

        // Criar 20 projetos
        Project::factory(20)->create()->each(function ($project) {
            // Para cada projeto, criar 5 produtos
            Product::factory(5)->create([
                'project_id' => $project->id
            ]);
        });
    }
}
```

## 7. Controllers

**app/Http/Controllers/ProjectController.php**
```php
<?php

namespace App\Http\Controllers;

use App\Models\Project;
use Illuminate\Http\Request;

class ProjectController extends Controller
{
    public function index()
    {
        $projects = Project::with('products')->get();
        return view('projects.index', compact('projects'));
    }
}
```

**app/Http/Controllers/ProductController.php**
```php
<?php

namespace App\Http\Controllers;

use App\Models\Product;
use Illuminate\Http\Request;

class ProductController extends Controller
{
    public function index()
    {
        $products = Product::with(['category', 'project'])->get();
        return view('products.index', compact('products'));
    }

    public function show(Product $product)
    {
        $product->load(['category', 'project']);
        return view('products.show', compact('product'));
    }
}
```

## 8. Página de Projetos [3 valores]

**resources/views/projects/index.blade.php**
```blade
@extends('layouts.app')

@section('title', 'Projetos')

@section('content')
<div class="container">
    <h1 class="mb-4">Lista de Projetos</h1>
    
    <div class="table-responsive">
        <table class="table table-striped">
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Nome do Projeto</th>
                    <th>Produtos Associados</th>
                    <th>Data de Criação</th>
                </tr>
            </thead>
            <tbody>
                @foreach($projects as $project)
                <tr>
                    <td>{{ $project->id }}</td>
                    <td>{{ $project->name }}</td>
                    <td>
                        <ul class="list-unstyled mb-0">
                            @foreach($project->products as $product)
                                <li>• {{ $product->name }}</li>
                            @endforeach
                        </ul>
                    </td>
                    <td>{{ $project->created_at->format('d/m/Y') }}</td>
                </tr>
                @endforeach
            </tbody>
        </table>
    </div>
</div>
@endsection
```

## 9. Página de Produtos [4 valores]

**resources/views/products/index.blade.php**
```blade
@extends('layouts.app')

@section('title', 'Produtos')

@section('content')
<div class="container">
    <h1 class="mb-4">Lista de Produtos</h1>
    
    <div class="row">
        @foreach($products as $product)
        <div class="col-md-4 mb-4">
            <div class="card h-100">
                <div class="card-body">
                    <h5 class="card-title">{{ $product->name }}</h5>
                    <p class="card-text">
                        <strong>Categoria:</strong> {{ $product->category->name }}<br>
                        <strong>Projeto:</strong> {{ $product->project->name }}
                    </p>
                    <a href="{{ route('products.show', $product) }}" class="btn btn-primary">Ver Detalhes</a>
                </div>
            </div>
        </div>
        @endforeach
    </div>
</div>
@endsection
```

## 10. Página de Detalhes do Produto [4 valores]

**resources/views/products/show.blade.php**
```blade
@extends('layouts.app')

@section('title', 'Detalhes do Produto')

@section('content')
<div class="container">
    <div class="row">
        <div class="col-lg-8 mx-auto">
            <div class="card">
                <div class="card-header">
                    <h2>{{ $product->name }}</h2>
                </div>
                <div class="card-body">
                    <div class="mb-3">
                        <strong>Categoria:</strong> {{ $product->category->name }}
                    </div>
                    <div class="mb-3">
                        <strong>Projeto:</strong> {{ $product->project->name }}
                    </div>
                    <div class="mb-3">
                        <strong>Descrição:</strong>
                        <p>{{ $product->details }}</p>
                    </div>
                    <div class="mb-3">
                        <strong>Data de Criação:</strong> {{ $product->created_at->format('d/m/Y H:i') }}
                    </div>
                    <a href="{{ route('products.index') }}" class="btn btn-secondary">Voltar à Lista</a>
                </div>
            </div>
        </div>
    </div>
</div>
@endsection
```

## 11. Rotas

**routes/web.php**
```php
<?php

use Illuminate\Support\Facades\Route;
use App\Http\Controllers\ProjectController;
use App\Http\Controllers\ProductController;

Route::get('/', function () {
    return redirect()->route('projects.index');
});

Route::get('/projects', [ProjectController::class, 'index'])->name('projects.index');
Route::get('/products', [ProductController::class, 'index'])->name('products.index');
Route::get('/products/{product}', [ProductController::class, 'show'])->name('products.show');

Auth::routes();
```

## Comandos para Executar

```bash
# Executar as migrações
php artisan migrate

# Executar os seeders
php artisan db:seed

# Iniciar o servidor
php artisan serve
```

Esta solução completa atende a todos os requisitos do trabalho prático, implementando as 3 páginas solicitadas com todas as funcionalidades e relações entre modelos conforme especificado.



**CORRIGIDO LARAVEL 7**
# Solução Completa - Laravel 7 - Aplicação de Gestão de Projetos de Design de Interiores

## 1. Configuração Inicial [0.5 valores]

```bash
# Criar projeto Laravel 7
composer create-project --prefer-dist laravel/laravel design-interiores "7.*"

# Instalar Laravel UI e Bootstrap
composer require laravel/ui "^2.0"
php artisan ui bootstrap --auth
npm install && npm run dev
```

## 2. Master Page (Layout) [0.5 valores]

**resources/views/layouts/app.blade.php**
```blade
<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>@yield('title', 'Design de Interiores')</title>
    <link href="{{ asset('css/app.css') }}" rel="stylesheet">
</head>
<body>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
        <div class="container">
            <a class="navbar-brand" href="/">Design Interiores</a>
            <div class="navbar-nav ml-auto">
                <a class="nav-link" href="{{ route('projects.index') }}">Projetos</a>
                <a class="nav-link" href="{{ route('products.index') }}">Produtos</a>
            </div>
        </div>
    </nav>

    <main class="py-4">
        <div class="container">
            @yield('content')
        </div>
    </main>

    <script src="{{ asset('js/app.js') }}"></script>
</body>
</html>
```

## 3. Migrações [2 valores]

**database/migrations/xxxx_xx_xx_create_categories_table.php**
```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateCategoriesTable extends Migration
{
    public function up()
    {
        Schema::create('categories', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->timestamps();
        });
    }

    public function down()
    {
        Schema::dropIfExists('categories');
    }
}
```

**database/migrations/xxxx_xx_xx_create_projects_table.php**
```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateProjectsTable extends Migration
{
    public function up()
    {
        Schema::create('projects', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->timestamps();
        });
    }

    public function down()
    {
        Schema::dropIfExists('projects');
    }
}
```

**database/migrations/xxxx_xx_xx_create_products_table.php**
```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateProductsTable extends Migration
{
    public function up()
    {
        Schema::create('products', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->text('details');
            $table->unsignedBigInteger('category_id');
            $table->unsignedBigInteger('project_id');
            $table->timestamps();

            $table->foreign('category_id')->references('id')->on('categories');
            $table->foreign('project_id')->references('id')->on('projects');
        });
    }

    public function down()
    {
        Schema::dropIfExists('products');
    }
}
```

## Modelos com Relações

**app/Category.php**
```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Category extends Model
{
    protected $fillable = ['name'];

    public function products()
    {
        return $this->hasMany(Product::class);
    }
}
```

**app/Project.php**
```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Project extends Model
{
    protected $fillable = ['name'];

    public function products()
    {
        return $this->hasMany(Product::class);
    }
}
```

**app/Product.php**
```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Product extends Model
{
    protected $fillable = ['name', 'details', 'category_id', 'project_id'];

    public function category()
    {
        return $this->belongsTo(Category::class);
    }

    public function project()
    {
        return $this->belongsTo(Project::class);
    }
}
```

## 4. Seeder para Categorias [2 valores]

**database/seeds/CategorySeeder.php**
```php
<?php

use Illuminate\Database\Seeder;
use App\Category;

class CategorySeeder extends Seeder
{
    public function run()
    {
        $categories = [
            'Quarto',
            'Sala',
            'Cozinha',
            'Casa de Banho'
        ];

        foreach ($categories as $category) {
            Category::create(['name' => $category]);
        }
    }
}
```

## 5. Factory para Produtos [2 valores]

**database/factories/ProductFactory.php**
```php
<?php

/** @var \Illuminate\Database\Eloquent\Factory $factory */

use App\Product;
use App\Category;
use App\Project;
use Faker\Generator as Faker;

$factory->define(Product::class, function (Faker $faker) {
    return [
        'name' => $faker->words(3, true),
        'details' => $faker->paragraph(),
        'category_id' => function() {
            return Category::inRandomOrder()->first()->id ?? 1;
        },
        'project_id' => function() {
            return Project::inRandomOrder()->first()->id ?? 1;
        },
    ];
});
```

## 6. Factory para Projetos [2 valores]

**database/factories/ProjectFactory.php**
```php
<?php

/** @var \Illuminate\Database\Eloquent\Factory $factory */

use App\Project;
use Faker\Generator as Faker;

$factory->define(Project::class, function (Faker $faker) {
    return [
        'name' => 'Projeto ' . $faker->company(),
    ];
});
```

**database/seeds/DatabaseSeeder.php**
```php
<?php

use Illuminate\Database\Seeder;
use App\Project;
use App\Product;

class DatabaseSeeder extends Seeder
{
    public function run()
    {
        // Primeiro, executar o seeder de categorias
        $this->call(CategorySeeder::class);

        // Criar 20 projetos
        factory(Project::class, 20)->create()->each(function ($project) {
            // Para cada projeto, criar 5 produtos
            factory(Product::class, 5)->create([
                'project_id' => $project->id
            ]);
        });
    }
}
```

## 7. Controllers

**app/Http/Controllers/ProjectController.php**
```php
<?php

namespace App\Http\Controllers;

use App\Project;
use Illuminate\Http\Request;

class ProjectController extends Controller
{
    public function index()
    {
        $projects = Project::with('products')->get();
        return view('projects.index', compact('projects'));
    }
}
```

**app/Http/Controllers/ProductController.php**
```php
<?php

namespace App\Http\Controllers;

use App\Product;
use Illuminate\Http\Request;

class ProductController extends Controller
{
    public function index()
    {
        $products = Product::with(['category', 'project'])->get();
        return view('products.index', compact('products'));
    }

    public function show(Product $product)
    {
        $product->load(['category', 'project']);
        return view('products.show', compact('product'));
    }
}
```

## 8. Página de Projetos [3 valores]

**resources/views/projects/index.blade.php**
```blade
@extends('layouts.app')

@section('title', 'Projetos')

@section('content')
<div class="container">
    <h1 class="mb-4">Lista de Projetos</h1>
    
    <div class="table-responsive">
        <table class="table table-striped">
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Nome do Projeto</th>
                    <th>Produtos Associados</th>
                    <th>Data de Criação</th>
                </tr>
            </thead>
            <tbody>
                @foreach($projects as $project)
                <tr>
                    <td>{{ $project->id }}</td>
                    <td>{{ $project->name }}</td>
                    <td>
                        <ul class="list-unstyled mb-0">
                            @foreach($project->products as $product)
                                <li>• {{ $product->name }}</li>
                            @endforeach
                        </ul>
                    </td>
                    <td>{{ $project->created_at->format('d/m/Y') }}</td>
                </tr>
                @endforeach
            </tbody>
        </table>
    </div>
</div>
@endsection
```

## 9. Página de Produtos [4 valores]

**resources/views/products/index.blade.php**
```blade
@extends('layouts.app')

@section('title', 'Produtos')

@section('content')
<div class="container">
    <h1 class="mb-4">Lista de Produtos</h1>
    
    <div class="row">
        @foreach($products as $product)
        <div class="col-md-4 mb-4">
            <div class="card h-100">
                <div class="card-body">
                    <h5 class="card-title">{{ $product->name }}</h5>
                    <p class="card-text">
                        <strong>Categoria:</strong> {{ $product->category->name }}<br>
                        <strong>Projeto:</strong> {{ $product->project->name }}
                    </p>
                    <a href="{{ route('products.show', $product) }}" class="btn btn-primary">Ver Detalhes</a>
                </div>
            </div>
        </div>
        @endforeach
    </div>
</div>
@endsection
```

## 10. Página de Detalhes do Produto [4 valores]

**resources/views/products/show.blade.php**
```blade
@extends('layouts.app')

@section('title', 'Detalhes do Produto')

@section('content')
<div class="container">
    <div class="row">
        <div class="col-lg-8 mx-auto">
            <div class="card">
                <div class="card-header">
                    <h2>{{ $product->name }}</h2>
                </div>
                <div class="card-body">
                    <div class="mb-3">
                        <strong>Categoria:</strong> {{ $product->category->name }}
                    </div>
                    <div class="mb-3">
                        <strong>Projeto:</strong> {{ $product->project->name }}
                    </div>
                    <div class="mb-3">
                        <strong>Descrição:</strong>
                        <p>{{ $product->details }}</p>
                    </div>
                    <div class="mb-3">
                        <strong>Data de Criação:</strong> {{ $product->created_at->format('d/m/Y H:i') }}
                    </div>
                    <a href="{{ route('products.index') }}" class="btn btn-secondary">Voltar à Lista</a>
                </div>
            </div>
        </div>
    </div>
</div>
@endsection
```

## 11. Rotas

**routes/web.php**
```php
<?php

use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    return redirect()->route('projects.index');
});

Route::get('/projects', 'ProjectController@index')->name('projects.index');
Route::get('/products', 'ProductController@index')->name('products.index');
Route::get('/products/{product}', 'ProductController@show')->name('products.show');

Auth::routes();
```

## Comandos para Executar

```bash
# Gerar as migrações
php artisan make:migration create_categories_table
php artisan make:migration create_projects_table
php artisan make:migration create_products_table

# Executar as migrações
php artisan migrate

# Gerar os seeders
php artisan make:seeder CategorySeeder

# Executar os seeders
php artisan db:seed

# Iniciar o servidor
php artisan serve
```

## Principais Diferenças do Laravel 7:

1. **Factories**: Uso da função `factory()` em vez de `Model::factory()`
2. **Modelos**: Localizados em `app/` em vez de `app/Models/`
3. **Controllers**: Sintaxe de rotas com string (`'Controller@method'`) em vez de array
4. **Assets**: `asset()` em vez de `@vite`
5. **Migrações**: Classes nomeadas em vez de classes anônimas
6. **Seeders**: Diretório `database/seeds/` em vez de `database/seeders/`

Esta solução está totalmente adaptada para Laravel 7 e atende a todos os requisitos do trabalho prático.