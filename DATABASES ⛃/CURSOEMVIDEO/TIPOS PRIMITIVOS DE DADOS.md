# Tipos Primitivos em MySQL

## Categorias principais:
- [[Tipo Numérico]]
- [[Tipo Data/Tempo]]
- [[Tipo Literal]]
- [[Tipo Espacial]]


# Tipo Numérico

```mermaid
graph TD
  Numerico[Numérico] --> Inteiro[Inteiro]
  Inteiro --> TinyInt
  Inteiro --> SmallInt
  Inteiro --> Int
  Inteiro --> MediumInt
  Inteiro --> BigInt

  Numerico --> Real[Real]
  Real --> Decimal
  Real --> Float
  Real --> Double
  Real --> Real2[Real]

  Numerico --> Logico[Lógico]
  Logico --> Bit
  Logico --> Boolean
  
