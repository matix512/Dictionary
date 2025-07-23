

🖩 graph TD
    A[Tipos Primitivos]

    A --> B[Numérico]
    B --> B1[Inteiro]
    B1 --> |TinyInt| B1a
    B1 --> |SmallInt| B1b
    B1 --> |Int| B1c
    B1 --> |MediumInt| B1d
    B1 --> |BigInt| B1e

    B --> B2[Real]
    B2 --> |Decimal| B2a
    B2 --> |Float| B2b
    B2 --> |Double| B2c
    B2 --> |Real| B2d

    B --> B3[Lógico]
    B3 --> |Bit| B3a
    B3 --> |Boolean| B3b

    A --> C[Data/Tempo]
    C --> |Date| C1
    C --> |DateTime| C2
    C --> |TimeStamp| C3
    C --> |Time| C4
    C --> |Year| C5

    A --> D[Literal]

    D --> D1[Caractere]
    D1 --> |Char| D1a
    D1 --> |VarChar| D1b

    D --> D2[Texto]
    D2 --> |TinyText| D2a
    D2 --> |Text| D2b
    D2 --> |MediumText| D2c
    D2 --> |LongText| D2d

    D --> D3[Binário]
    D3 --> |TinyBlob| D3a
    D3 --> |Blob| D3b
    D3 --> |MediumBlob| D3c
    D3 --> |LongBlob| D3d

    D --> D4[Coleção]
    D4 --> |Enum| D4a
    D4 --> |Set| D4b

    A --> E[Espacial]
    E --> |Geometry| E1
    E --> |Point| E2
    E --> |Polygon| E3
    E --> |MultiPolygon| E4
