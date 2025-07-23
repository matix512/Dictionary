## 🧮 Numérico

### 🔢 Inteiro
Tipos de números inteiros, com ou sem sinal (`SIGNED` ou `UNSIGNED`).

- `TINYINT` (1 byte): de -128 a 127 (SIGNED) ou 0 a 255 (UNSIGNED).
- `SMALLINT` (2 bytes): de -32.768 a 32.767 (SIGNED).
- `INT` ou `INTEGER` (4 bytes): de -2.147.483.648 a 2.147.483.647.
- `MEDIUMINT` (3 bytes): de -8.388.608 a 8.388.607.
- `BIGINT` (8 bytes): de -2⁶³ a 2⁶³ - 1.

🔸 **Usa-se para:** contagens, IDs, flags.

---

### 🔣 Real (Ponto flutuante)
Usados para valores com casas decimais.

- `DECIMAL(M,D)` ou `NUMERIC`: precisão exata. M = total dígitos, D = casas decimais.
- `FLOAT`: precisão simples (até ~7 dígitos).
- `DOUBLE` ou `DOUBLE PRECISION`: precisão dupla (até ~15 dígitos).
- `REAL`: sinónimo de `DOUBLE` (varia por sistema).

🔸 **Usa-se para:** valores financeiros (`DECIMAL`), medições, cálculos científicos.

---

### 🧠 Lógico
Representam valores booleanos ou bits.

- `BIT(N)`: sequência de bits. Ex: `BIT(1)` = 0 ou 1.
- `BOOLEAN` ou `BOOL`: sinónimo de `TINYINT(1)` (0 = falso, ≠0 = verdadeiro).

🔸 **Usa-se para:** flags, opções, verdadeiro/falso.

---

## 🕒 Data e Tempo

Armazenam datas e/ou horas.

- `DATE`: apenas a data (AAAA-MM-DD).
- `DATETIME`: data + hora (AAAA-MM-DD HH:MM:SS).
- `TIMESTAMP`: como `DATETIME`, mas armazena em UTC (ideal para logs).
- `TIME`: apenas hora (HH:MM:SS).
- `YEAR`: apenas o ano (AAAA).

🔸 **Usa-se para:** datas de registo, eventos, logs.

---

## 🔤 Literal

### 🔠 Caractere
Armazenam textos curtos de tamanho fixo ou variável.

- `CHAR(N)`: texto fixo com N caracteres. Preenche com espaços.
- `VARCHAR(N)`: texto variável até N caracteres.

🔸 **Usa-se para:** nomes, emails, códigos.

---

### 📝 Texto
Armazenam textos longos.

- `TINYTEXT`: até 255 caracteres.
- `TEXT`: até 65.535 caracteres (64 KB).
- `MEDIUMTEXT`: até ~16 MB.
- `LONGTEXT`: até ~4 GB.

🔸 **Usa-se para:** descrições, artigos, comentários grandes.

---

### 🧱 Binário
Semelhantes aos tipos de texto, mas armazenam dados binários (imagens, ficheiros...).

- `TINYBLOB`: até 255 bytes.
- `BLOB`: até 64 KB.
- `MEDIUMBLOB`: até ~16 MB.
- `LONGBLOB`: até ~4 GB.

🔸 **Usa-se para:** ficheiros, imagens, áudio, binários.

---

### 🧩 Coleção

- `ENUM('A','B',...)`: aceita apenas um valor de uma lista predefinida.
- `SET('A','B',...)`: aceita vários valores da lista, separados por vírgulas.

🔸 **Usa-se para:** categorias, estados, permissões múltiplas.

---

## 🌍 Espacial

Usado para dados geográficos (GIS).

- `GEOMETRY`: base para todos os tipos espaciais.
- `POINT`: ponto com coordenadas X e Y.
- `POLYGON`: conjunto de linhas fechadas.
- `MULTIPOLYGON`: conjunto de polígonos.

🔸 **Usa-se para:** mapas, localizações, áreas geográficas.

---

## ℹ️ Observações Finais

- Usa `UNSIGNED` se não precisas de valores negativos (ganhas mais alcance positivo).
- Usa `DECIMAL` para dinheiro — evita erros de arredondamento como `FLOAT`.
- Para texto longo, evita `TEXT` se precisares de **indexação** ou `FULLTEXT` search (há limitações).
- `TIMESTAMP` pode ser atualizado automaticamente com `DEFAULT CURRENT_TIMESTAMP`.

