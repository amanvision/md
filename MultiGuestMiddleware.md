Create Middleware using below command

```php
php artisan make:middleware MultipleGuest
```

Paste below code in the handle method of above middleware

```php
$response = $next($request);

if(auth()->check()) {
    return redirect(url('/'));
}

if(auth('tutor')->check()) {
    return redirect(url('/tutor'));
}

if(auth('admin')->check()) {
    return redirect(url('/admin'));
}


return $response;
 ```
 
 Open Kernal.php and add below line to $routeMiddleware
 
 ```php
 'multiple-guest' => \App\Http\Middleware\MultipleGuest::class,
 ```
 
 Finally add the middleware to LoginController & RegisterController of each guard (in __construct method), see e.g below
 
 ```php
 // In Login Controller
public function __construct()
{
    $this->middleware('multiple-guest')->except('logout');
}
 
 // In Register Controller
public function __construct()
{
    $this->middleware('multiple-guest');
}
 ```
 
