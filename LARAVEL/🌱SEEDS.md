### **DatabaseSeeder.php:**

```php
public function run()
{
    $this->call([
        CountrySeeder::class,
        UserSeeder::class,
        BicycleSeeder::class,
    ]);
}```


### **CountrySeeder.php:**

```php
<?php
use Illuminate\Database\Seeder;
use App\Country;

class CountrySeeder extends Seeder
{
    public function run()
    {
        $countries = ['Portugal', 'Espanha', 'França', 'Polónia'];
        
        foreach($countries as $country) {
            Country::create(['name' => $country]);
        }
    }
}
```

### **UserSeeder.php:**

```php
public function run()
{
    factory(User::class, 100)->create();
}
```

### BycicleSeeder.php:**

```php
public function run()
{
    factory(User::class, 100)->create();
}
```
