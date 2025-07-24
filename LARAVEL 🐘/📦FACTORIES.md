### **UserFactory.php:**

```php
<?php
use Faker\Generator as Faker;
use App\User;

$factory->define(User::class, function (Faker $faker) {
    return [
        'country_id' => rand(1, 4), // 4 países
        'first_name' => $faker->firstName,
        'last_name' => $faker->lastName,
        'birth_date' => $faker->dateTimeBetween('-80 years', '-18 years'),
        'email' => $faker->unique()->safeEmail,
        'email_verified_at' => now(),
        'password' => bcrypt('password'),
        'remember_token' => \Str::random(10),
    ];
});
```

### **BicycleFactory.php:**

```php
<?php
use Faker\Generator as Faker;
use App\Bicycle;

$factory->define(Bicycle::class, function (Faker $faker) {
    return [
        'user_id' => rand(1, 100), // 100 users criados
        'brand' => $faker->randomElement(['Trek', 'Giant', 'Specialized', 'Cannondale', 'Scott', 'Merida']),
        'model' => $faker->word . ' ' . $faker->randomNumber(3),
        'color' => $faker->randomElement(['Red', 'Blue', 'Black', 'White', 'Green', 'Yellow', 'Silver']),
        'price' => $faker->randomFloat(2, 200, 5000),
    ];
});
```



# Onde Encontrar as Definições do Faker

O Faker é uma biblioteca PHP usada pelo Laravel para gerar dados fictícios. Vamos ver onde você pode encontrar todas as definições e métodos disponíveis:

## 1. Documentação Oficial e Repositório

O Laravel utiliza a biblioteca [FakerPHP/Faker](https://github.com/FakerPHP/Faker), que é um fork mantido da biblioteca Faker original.

- **Repositório GitHub**: [https://github.com/FakerPHP/Faker](https://github.com/FakerPHP/Faker)
- **Documentação oficial**: [https://fakerphp.github.io/](https://fakerphp.github.io/)

## 2. Formatadores Disponíveis (Métodos do Faker)

### Métodos Básicos

- **Nomes**: name(), firstName(), lastName()
- **Endereços**: address(), city(), country()
- **Textos**: text(), paragraph(), sentence()
- **Números**: randomNumber(), numberBetween()
- **Internet**: email(), url(), ipv4()
- **Datas**: dateTime(), date(), time()

### Verificando os Formatadores no Código-Fonte

A melhor maneira de ver todos os formatadores disponíveis é navegar pelo código-fonte, especialmente a pasta src/Faker/Provider:

textresponse-action-icon

```text
src/Faker/Provider/
├── Address.php           # Endereços, cidades, países
├── Barcode.php           # Códigos de barras
├── Color.php             # Nomes de cores, códigos hex
├── Company.php           # Nomes de empresas, cargos
├── DateTime.php          # Datas, horas, timestamps
├── File.php              # Manipulação de arquivos
├── Internet.php          # Emails, domínios, URLs
├── Lorem.php             # Texto aleatório
├── Person.php            # Nomes, títulos
├── PhoneNumber.php       # Números de telefone
├── Text.php              # Manipulação de texto
├── UserAgent.php         # User agents de navegadores
└── ...
```

## 3. Providers Localizados

O Faker também tem providers específicos para diferentes localidades/idiomas, que geram dados adequados para cada região:

```text
src/Faker/Provider/
├── pt_BR/               # Português do Brasil
│   ├── Address.php
│   ├── Company.php
│   ├── Person.php
│   └── ...
├── es_ES/               # Espanhol
├── fr_FR/               # Francês
├── en_US/               # Inglês (EUA)
└── ...
```

Para usar uma localidade específica, você pode configurar no AppServiceProvider:

```php
// app/Providers/AppServiceProvider.php
public function boot()
{
    $this->app->singleton(\Faker\Generator::class, function () {
        return \Faker\Factory::create('pt_BR');
    });
}
```

## 4. Exemplos de Uso Comuns

### Texto e Palavras

phpresponse-action-icon

```php
$faker->word                      // Uma palavra aleatória
$faker->words(3)                  // Array com 3 palavras
$faker->sentence(5)               // Frase com 5 palavras
$faker->paragraph(3)              // Parágrafo com 3 frases
$faker->text(200)                 // Texto com 200 caracteres
```

### Pessoas e Identidades

phpresponse-action-icon

```php
$faker->name                      // 'John Doe'
$faker->firstName                 // 'John'
$faker->lastName                  // 'Doe'
$faker->title                     // 'Dr.'
$faker->titleMale                 // 'Mr.'
$faker->titleFemale               // 'Ms.'
$faker->cpf                       // (no Brasil) '123.456.789-00'
```

### Números e Datas

phpresponse-action-icon

```php
$faker->randomDigit               // Dígito de 0-9
$faker->randomNumber(5)           // Número com 5 dígitos
$faker->numberBetween(10, 99)     // Número entre 10 e 99
$faker->randomFloat(2, 0, 100)    // Ponto flutuante com 2 casas decimais entre 0 e 100

$faker->dateTime                  // DateTime entre '1970-01-01' e agora
$faker->date('Y-m-d')             // '2008-04-25'
$faker->time('H:i:s')             // '20:49:42'
$faker->dayOfMonth                // '04'
$faker->monthName                 // 'Janeiro'
$faker->year                      // '1993'
```

### Internet e Tecnologia

phpresponse-action-icon

```php
$faker->email                     // 'email@example.com'
$faker->safeEmail                 // 'email@example.org'
$faker->domainName                // 'example.com'
$faker->url                       // 'http://www.example.com/'
$faker->ipv4                      // '192.168.1.1'
$faker->userAgent                 // 'Mozilla/5.0 (Windows NT 10.0; Win64; x64)...'
```

### Endereços

phpresponse-action-icon

```php
$faker->address                   // '123 Rua Principal'
$faker->streetAddress             // '123 Rua Principal'
$faker->city                      // 'São Paulo'
$faker->state                     // 'SP'
$faker->country                   // 'Brasil'
$faker->postcode                  // '12345-678'
```

### Cores

phpresponse-action-icon

```php
$faker->colorName                 // 'blue'
$faker->hexColor                  // '#ff0044'
$faker->rgbColor                  // '0,255,122'
```

## 5. Métodos Modificadores

O Faker também possui métodos "modificadores" que podem ser encadeados:

phpresponse-action-icon

```php
$faker->unique()->randomNumber(5)  // Garante que o número seja único
$faker->optional(0.5)->firstName   // 50% de chance de retornar um nome, null caso contrário
$faker->valid(function($v) { return $v > 0; })->randomNumber() // Só aceita números positivos
```

## 6. Criando seus Próprios Geradores

Se precisar de um tipo específico de dado que o Faker não fornece, você pode criar seu próprio provider:

phpresponse-action-icon

```php
// Estendendo o Faker com métodos personalizados
$faker->addProvider(new class($faker) extends \Faker\Provider\Base {
    public function productCode()
    {
        return 'PROD-' . $this->generator->randomNumber(6);
    }
});

// Agora você pode usar:
$faker->productCode(); // 'PROD-123456'
```