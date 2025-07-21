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
public function run()
{
    $this->call([
        CountrySeeder::class,
        UserSeeder::class,
        BicycleSeeder::class,
    ]);
}```
