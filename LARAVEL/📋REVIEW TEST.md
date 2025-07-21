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



## 🔧 **Guia de Implementação**

### 1. Configuração Inicial

```
composer create-project laravel/laravel=7.34.7 bike-manager
cd bike-manager
composer require laravel/ui:^2.4
php artisan ui bootstrap --auth

```

### 2. Modelos e Migrações

**Estrutura DB:**

```php

// Migração de Countries
Schema::create('countries', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->timestamps();
    $table->softDeletes();
});

// Users (alterar migração padrão)
Schema::table('users', function (Blueprint $table) {
    $table->unsignedBigInteger('country_id');
    $table->string('first_name');
    $table->string('last_name');
    $table->dateTime('birth_date');
    $table->softDeletes();
});
```


### 3. Relacionamentos

```php
// User.php
public function country() {
    return $this->belongsTo(Country::class);
}

public function bicycles() {
    return $this->hasMany(Bicycle::class);
}
```


### 4. Seeders

```php

// CountrySeeder.php
$countries = ['Portugal', 'Espanha', 'França', 'Polónia'];
foreach ($countries as $country) {
    Country::create(['name' => $country]);
}
```


### 5. Factories

```php
// UserFactory.php
'country_id' => Country::all()->random()->id,
'first_name' => $faker->firstName,
'birth_date' => $faker->dateTimeBetween('-70 years', '-18 years'),

// BicycleFactory.php
'brand' => $faker->randomElement(['Trek', 'Giant']),
'price' => $faker->randomFloat(2, 100, 5000),

```


## 🖥️ **Exemplos de Código**

### Controller Típico

```php
public function index() {
    $bicycles = Bicycle::with('user')->paginate(20);
    return view('bicycles.index', compact('bicycles'));
}
```

### Blade com Bootstrap

```html
<table class="table table-hover">
    <thead class="thead-dark">
        <tr>
            <th>ID</th>
            <th>Brand</th>
            <th>Owner</th>
        </tr>
    </thead>
    <tbody>
        @foreach($bicycles as $bicycle)
        <tr>
            <td>{{ $bicycle->id }}</td>
            <td>{{ $bicycle->brand }}</td>
            <td>{{ $bicycle->user->first_name }}</td>
        </tr>
        @endforeach
    </tbody>
</table>
{{ $bicycles->links() }}
```


## 🚨 **Possíveis Variações no Teste**

1. **Novos Modelos**
    
    - Adicionar `Manufacturer` para bicicletas
        
    - Relacionamento many-to-many com users
        
2. **Funcionalidades Extra**
    
    - Sistema de reservas
        
    - Upload de fotos das bicicletas
        
3. **API Endpoints**
    
    - `/api/bicycles` retornar JSON