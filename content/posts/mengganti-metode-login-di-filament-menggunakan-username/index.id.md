---
title: 'Mengganti Metode Login di Filament Menggunakan Username'
date: 2023-12-31T18:00:00+07:00
draft: false
# cover: 
#   image: featured-image.png
tags:
  - Filament
  - Laravel
  - PHP
showtoc: true
---

Secara bawaan, halaman login Filament menggunakan kolom email dan _password_ untuk proses otentikasi. Namun, bagaimana jika ingin menjadikan kolom _username_ untuk proses otentikasi? Dalam tutorial singkat ini, mari belajar bagaimana cara masuk ke panel Filament menggunakan _username_ dan _password_.

Semua pengaturan Panel Filament dilakukan di _Service Provider_. Untuk contoh ini, kita akan menggunakan _AdminPanelProvider_ bawaan atau . Di sini kita memiliki metode login() yang menerima kelas tindakan sebagai parameter:

`app/Providers/Filament/AdminPanelProvider.php`:

```php
class AdminPanelProvider extends PanelProvider
{
    public function panel(Panel $panel): Panel
    {
        return $panel
            ->default()
            ->id('admin')
            ->path('admin')
            ->login(Login::class) 
            // ...
    }
}
```

Mari buat halaman Login kustom kita sendiri dan sisipkan ke dalam metode login. Saya akan membuat kelas ini di direktori `App\Filament\Auth`. Ini perlu memperluas kelas Login asli dari Filament.

`app/Filament/Auth/Login.php`:

```php
use Filament\Pages\Auth\Login as BaseAuth;
 
class Login extends BaseAuth
{
    // ...
}
```

Anda dapat memeriksa kursus lengkap kelas Login Filament di repositori GitHub. Untuk kasus penggunaan kami, kita perlu mengganti dua metode:

1. `form()`
2. `getCredentialsFromFormData()`

Juga, kita akan membuat metode baru untuk menggunakan kolom yang berbeda daripada email. Kita akan menyebutnya kolom "Login" yang akan menerima nilai email atau username.

`app/Filament/Auth/Login.php`:

```php
use Filament\Forms\Form;
use Filament\Forms\Components\TextInput;
use Filament\Forms\Components\Component;
use Filament\Pages\Auth\Login as BaseAuth;
 
class Login extends BaseAuth
{
    public function form(Form $form): Form
    {
        return $form
            ->schema([
                $this->getEmailFormComponent(), 
                $this->getLoginFormComponent(), 
                $this->getPasswordFormComponent(),
                $this->getRememberFormComponent(),
            ])
            ->statePath('data');
    }
 
    protected function getLoginFormComponent(): Component 
    {
        return TextInput::make('login')
            ->label('Login')
            ->required()
            ->autocomplete()
            ->autofocus();
    } 
}
```

Kita memiliki formulir login di mana kita dapat memasukkan nama atau email untuk kredensial.

Formulir login filament dengan _username_

Sekarang, mari timpa metode `getCredentialsFromFormData()` untuk menggunakan nama atau email untuk otentikasi.

`app/Filament/Auth/Login.php`:

```php
class Login extends BaseAuth
{
    // ...
 
    protected function getCredentialsFromFormData(array $data): array
    {
        $login_type = filter_var($data['login'], FILTER_VALIDATE_EMAIL ) ? 'email' : 'name';
 
        return [
            $login_type => $data['login'],
            'password'  => $data['password'],
        ];
    }
}
```

Dalam metode ini, kita menggunakan fungsi PHP filter_var untuk memeriksa apakah nilai yang dimasukkan adalah email. Bergantung pada itu, kita menetapkan variabel login_type ke nama email atau nama DB.

Kami telah membuat tindakan Login kustom kami. Sekarang kita hanya perlu menyisipkannya ke dalam metode login di AdminPanelProvider.

`app/Providers/Filament/AdminPanelProvider.php`:

```php
use App\Filament\Auth\Login;
 
class AdminPanelProvider extends PanelProvider
{
    public function panel(Panel $panel): Panel
    {
        return $panel
            ->default()
            ->id('admin')
            ->path('admin')
            ->login(Login::class) 
            // ...
    }
}
```

Itu saja. Pengguna dapat masuk ke panel Filament menggunakan nama atau email dalam kolom yang sama.