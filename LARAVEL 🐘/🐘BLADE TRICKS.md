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

phpresponse-action-icon

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


