
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