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

