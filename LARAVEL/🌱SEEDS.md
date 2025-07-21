public function run()
{
    $this->call([
        CountrySeeder::class,
        UserSeeder::class,
        BicycleSeeder::class,
    ]);
}