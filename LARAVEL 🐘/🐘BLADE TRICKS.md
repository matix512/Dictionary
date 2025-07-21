### **🔄 Loops e Condições:**

```php
<!-- Loop básico -->
@foreach($items as $item)
    <p>{{ $item->name }}</p>
@endforeach

<!-- Loop com empty -->
@forelse($items as $item)
    <p>{{ $item->name }}</p>
@empty
    <p>Nenhum item encontrado</p>
@endforelse

<!-- Condições -->
@if($condition)
    <p>Verdadeiro</p>
@elseif($other)
    <p>Outro caso</p>
@else
    <p>Falso</p>
@endif

<!-- Condição inline -->
@isset($variable)
    <p>Variável existe</p>
@endisset
```

### **🏗️ Layouts e Includes:**


```php
<!-- Estender layout -->
@extends('layouts.app')

@section('title', 'Página')

@section('content')
    Conteúdo da página
@endsection

<!-- Incluir partial -->
@include('partials.navbar')
@include('partials.alerts')

<!-- Include com dados -->
@include('components.card', ['title' => 'Título'])
```

### **🔗 URLs e Rotas:**

phpresponse-action-icon

```php
<!-- URLs -->
{{ url('/') }}                    <!-- URL base -->
{{ asset('css/app.css') }}        <!-- Asset público -->

<!-- Rotas nomeadas -->
{{ route('users.index') }}        <!-- /users -->
{{ route('users.show', $user) }}  <!-- /users/{id} -->

<!-- Links condicionais -->
<a href="{{ route('users.index') }}" 
   class="nav-link {{ Request::is('users*') ? 'active' : '' }}">
   Users
</a>
```

### **💾 Formulários:**


```php
<!-- Formulário básico -->
<form method="POST" action="{{ route('users.store') }}">
    @csrf
    <input type="text" name="name" value="{{ old('name') }}">
    <button type="submit">Enviar</button>
</form>

<!-- Método HTTP -->
<form method="POST" action="{{ route('users.update', $user) }}">
    @csrf
    @method('PUT')
    <!-- campos -->
</form>

<!-- Erros de validação -->
@if($errors->has('name'))
    <div class="text-danger">{{ $errors->first('name') }}</div>
@endif
```

### **⏰ Datas e Formatação:**


```php
<!-- Datas -->
{{ $user->created_at->format('d/m/Y') }}
{{ $user->created_at->format('d/m/Y H:i') }}
{{ $user->created_at->diffForHumans() }}

<!-- Números -->
{{ number_format($price, 2) }}         <!-- 1234.56 -->
{{ number_format($count) }}            <!-- 1,234 -->

<!-- Strings -->
{{ Str::limit($text, 100) }}          <!-- Cortar texto -->
{{ ucfirst($name) }}                  <!-- Primeira maiúscula -->
```