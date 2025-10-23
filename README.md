# Books to Scrape – ETL Pipeline (Web Scraping + PostgreSQL)

Este proyecto implementa un flujo **ETL (Extract, Transform, Load)** automatizado para obtener información de libros desde el sitio [Books to Scrape](https://books.toscrape.com/), utilizando **Web Scraping con Selenium y BeautifulSoup**, procesamiento de datos con **Pandas y NumPy**, y almacenamiento estructurado en una base de datos **PostgreSQL**.

## Arquitectura del Proyecto

```bash
BooksToScrape
│
├── extract_book_details()      # Extracción automatizada con Selenium
├── transform_data()            # Limpieza y normalización con Pandas
└── load_data()                 # Inserción en PostgreSQL (tablas normalizadas)
```

## Flujo General

### Extract
> **Utiliza Selenium para navegar por todas las categorías disponibles del sitio**

> **Información extraída**
- Título
- Precio
- Disponibilidad
- Rating (1–5 estrellas)
- Descripción
- Información técnica (UPC, tipo de producto, impuestos, etc.)
- URL del producto

> **JSON**
```bash
[
  {
    "categoria": "Travel",
    "libros": [
      {
        "titulo": "Book Title",
        "precio": "£51.77",
        "disponibilidad": "In stock (19 available)",
        "rating_stars": "Three",
        "url": "https://books.toscrape.com/.../book_123",
        "info": { "UPC": "a123b456c789", "Tax": "£0.00" }
      }
    ]
  }
]

```

### Transform
> **Estandariza los datos, construye dimensiones y relaciones, y genera tablas limpias**

> **Limpia y normaliza los datos**
- Convierte precios de texto a float
- Extrae cantidad disponible usando regex
- Factoriza categorías y asigna IDs únicos
- Estandariza calificaciones (1–5)

> **DataFrames**
- df_categorias
- df_libros
- df_price_libros
- df_availability_libros
- df_reviews_libros

### Load
> **Inserta los datos normalizados en una base PostgreSQL**

```bash
postgresql+psycopg2://postgres:root@localhost:5432/Books_Norm
```

> **Crea las tablas si no existen**

> **Inserta los registros de cada DataFrame con su tipo de dato correspondiente**
```bash
INTEGER, TEXT, REAL, BOOLEAN, etc.
```

> **Usa SQLAlchemy y pandas.to_sql() con dtype explícito para compatibilidad**
