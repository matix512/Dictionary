# Script Passo a Passo - Laravel 7 + Bootstrap 4.3

## 1. Setup Inicial

bashresponse-action-icon

```bash
# Criar projeto Laravel com autenticação
composer create-project --prefer-dist laravel/laravel nome-projeto "7.*"
cd nome-projeto
composer require laravel/ui:^2.4
php artisan ui bootstrap --auth
npm install && npm run dev
```

## 2. Criar Models com Resources

bashresponse-action-icon

```bash
php artisan make:model Country -a
php artisan make:model Bicycle -a
```

## 3. Configurar Migrações

### Migration: Countries (criar primeiro)

phpresponse-action-icon

```php
// database/migrations/xxxx_create_countries_table.php
Schema::create('countries', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->timestamps();
});
```

### Migration: Users (editar existente)

phpresponse-action-icon

```php
// database/migrations/xxxx_create_users_table.php
Schema::create('users', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->string('email')->unique();
    $table->timestamp('email_verified_at')->nullable();
    $table->string('password');
    $table->foreignId('country_id')->constrained();
    $table->rememberToken();
    $table->timestamps();
});
```

### Migration: Bicycles

phpresponse-action-icon

```php
// database/migrations/xxxx_create_bicycles_table.php
Schema::create('bicycles', function (Blueprint $table) {
    $table->id();
    $table->string('brand');
    $table->string('model');
    $table->foreignId('user_id')->constrained();
    $table->timestamps();
});
```

## 4. Configurar Models e Relações

### Model: Country

phpresponse-action-icon

```php
// app/Country.php
class Country extends Model
{
    protected $fillable = ['name'];
    
    public function users()
    {
        return $this->hasMany(User::class);
    }
}
```

### Model: User (editar existente)

phpresponse-action-icon

```php
// app/User.php
public function country()
{
    return $this->belongsTo(Country::class);
}

public function bicycles()
{
    return $this->hasMany(Bicycle::class);
}
```

### Model: Bicycle

phpresponse-action-icon

```php
// app/Bicycle.php
class Bicycle extends Model
{
    protected $fillable = ['brand', 'model', 'user_id'];
    
    public function user()
    {
        return $this->belongsTo(User::class);
    }
}
```

## 5. Criar Seeder para Countries

phpresponse-action-icon

```php
// database/seeds/CountrySeeder.php
use App\Country;

class CountrySeeder extends Seeder
{
    public function run()
    {
        $countries = ['Portugal', 'Espanha', 'França', 'Polónia'];
        
        foreach ($countries as $country) {
            Country::create(['name' => $country]);
        }
    }
}
```

## 6. Criar Factories

### Factory: User

phpresponse-action-icon

```php
// database/factories/UserFactory.php (editar existente)
use App\Country;

$factory->define(User::class, function (Faker $faker) {
    return [
        'name' => $faker->name,
        'email' => $faker->unique()->safeEmail,
        'email_verified_at' => now(),
        'password' => '$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi',
        'country_id' => Country::inRandomOrder()->first()->id,
        'remember_token' => Str::random(10),
    ];
});
```

### Factory: Bicycle

phpresponse-action-icon

```php
// database/factories/BicycleFactory.php
use App\Bicycle;
use App\User;

$factory->define(Bicycle::class, function (Faker $faker) {
    return [
        'brand' => $faker->randomElement(['Trek', 'Giant', 'Specialized', 'Cannondale']),
        'model' => $faker->word . ' ' . $faker->numberBetween(100, 999),
        'user_id' => User::factory(),
    ];
});
```

## 7. Atualizar DatabaseSeeder

phpresponse-action-icon

```php
// database/seeds/DatabaseSeeder.php
public function run()
{
    $this->call(CountrySeeder::class);
    
    factory(App\User::class, 100)->create()->each(function ($user) {
        factory(App\Bicycle::class, 2)->create(['user_id' => $user->id]);
    });
}
```

## 8. Executar Migrações e Seeds

bashresponse-action-icon

```bash
php artisan migrate:fresh --seed
```

## 9. Configurar Master Page

bladeresponse-action-icon

```blade
{{-- resources/views/layouts/app.blade.php --}}
{{-- Já existe, apenas certificar que está configurada --}}
```

## 10. Criar Controllers

### CountryController

phpresponse-action-icon

```php
// app/Http/Controllers/CountryController.php
public function index()
{
    $countries = Country::withCount('users')->get();
    return view('countries.index', compact('countries'));
}
```

### UserController

phpresponse-action-icon

```php
// app/Http/Controllers/UserController.php
public function index()
{
    $users = User::with('country')->paginate(20);
    return view('users.index', compact('users'));
}
```

### BicycleController

phpresponse-action-icon

```php
// app/Http/Controllers/BicycleController.php
public function index()
{
    $bicycles = Bicycle::with('user')->paginate(20);
    return view('bicycles.index', compact('bicycles'));
}
```

## 11. Configurar Rotas

phpresponse-action-icon

```php
// routes/web.php
Auth::routes();

Route::get('/home', 'HomeController@index')->name('home');
Route::get('/countries', 'CountryController@index')->name('countries.index');
Route::get('/users', 'UserController@index')->name('users.index');
Route::get('/bicycles', 'BicycleController@index')->name('bicycles.index');
```

## 12. Criar Views

### View: Countries

bladeresponse-action-icon

```blade
{{-- resources/views/countries/index.blade.php --}}
@extends('layouts.app')

@section('content')
<div class="container">
    <h1>Countries</h1>
    <table class="table">
        <thead>
            <tr>
                <th>ID</th>
                <th>Name</th>
                <th>Total Users</th>
            </tr>
        </thead>
        <tbody>
            @foreach($countries as $country)
            <tr>
                <td>{{ $country->id }}</td>
                <td>{{ $country->name }}</td>
                <td>{{ $country->users_count }}</td>
            </tr>
            @endforeach
        </tbody>
    </table>
</div>
@endsection
```

### View: Users

bladeresponse-action-icon

```blade
{{-- resources/views/users/index.blade.php --}}
@extends('layouts.app')

@section('content')
<div class="container">
    <h1>Users</h1>
    <table class="table">
        <thead>
            <tr>
                <th>ID</th>
                <th>Name</th>
                <th>Email</th>
                <th>Country</th>
            </tr>
        </thead>
        <tbody>
            @foreach($users as $user)
            <tr>
                <td>{{ $user->id }}</td>
                <td>{{ $user->name }}</td>
                <td>{{ $user->email }}</td>
                <td>{{ $user->country->name }}</td>
            </tr>
            @endforeach
        </tbody>
    </table>
    {{ $users->links() }}
</div>
@endsection
```

### View: Bicycles

bladeresponse-action-icon

```blade
{{-- resources/views/bicycles/index.blade.php --}}
@extends('layouts.app')

@section('content')
<div class="container">
    <h1>Bicycles</h1>
    <table class="table">
        <thead>
            <tr>
                <th>ID</th>
                <th>Brand</th>
                <th>Model</th>
                <th>Owner</th>
            </tr>
        </thead>
        <tbody>
            @foreach($bicycles as $bicycle)
            <tr>
                <td>{{ $bicycle->id }}</td>
                <td>{{ $bicycle->brand }}</td>
                <td>{{ $bicycle->model }}</td>
                <td>{{ $bicycle->user->name }}</td>
            </tr>
            @endforeach
        </tbody>
    </table>
    {{ $bicycles->links() }}
</div>
@endsection
```

## 13. Ordem de Execução Final

bashresponse-action-icon

```bash
# 1. Executar migrações e seeds
php artisan migrate:fresh --seed

# 2. Compilar assets
npm run dev

# 3. Iniciar servidor
php artisan serve
```

## Checklist Final

- [x]  Auth + Bootstrap instalados
- [x]  Master Page configurada
- [x]  Migrações com relações
- [x]  Seed de Countries
- [x]  Factory de 100 Users com país
- [x]  Factory de 200 Bicycles (2 por user)
- [x]  Rota /countries com listagem
- [x]  Rota /users com listagem
- [x]  Rota /bicycles com listagem