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

