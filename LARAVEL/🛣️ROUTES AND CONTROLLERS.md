### **Rotas (routes/web.php):**

```php
<?php
use Illuminate\Support\Facades\Route;

// Página inicial
Route::get('/', function () {
    return view('welcome');
});

// Auth routes (se instalado)
Auth::routes();
Route::get('/home', 'HomeController@index')->name('home');

// Rotas principais do teste
Route::get('/countries', 'CountryController@index')->name('countries.index');
Route::get('/users', 'UserController@index')->name('users.index');
Route::get('/bicycles', 'BicycleController@index')->name('bicycles.index');

// Rotas completas CRUD (se necessário)
Route::resource('countries', 'CountryController');
Route::resource('users', 'UserController');
Route::resource('bicycles', 'BicycleController');
```

### **CountryController.php:**

phpresponse-action-icon

```php
<?php
namespace App\Http\Controllers;
use App\Country;
use Illuminate\Http\Request;

class CountryController extends Controller
{
    public function index()
    {
        $countries = Country::withCount('users')->orderBy('name')->get();
        return view('countries.index', compact('countries'));
    }
    
    public function show($id)
    {
        $country = Country::with('users')->findOrFail($id);
        return view('countries.show', compact('country'));
    }
}
```