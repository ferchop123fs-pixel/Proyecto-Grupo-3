# Migraciones de base de datos

## Criterio del proyecto

Este proyecto **no utiliza migraciones SQL incrementales** para las
correcciones de la aplicación.

La fuente única y definitiva del esquema es:

```text
database/init.sql
```

Cuando se modifica la estructura de la base de datos, la modificación se
incorpora directamente a `init.sql`. Esto permite que una instalación nueva
sea reproducible sin ejecutar una secuencia de migraciones.

## Instalación limpia

Desde la raíz del proyecto:

```bash
docker compose down -v
docker compose up --build -d
```

El contenedor de MySQL ejecutará `database/init.sql` automáticamente al crear
el volumen por primera vez.

## Archivo SQL de referencia

También existe:

```text
scripts_bd/sistema_tutorias.sql
```

Este archivo se conserva como referencia histórica. **No es necesario
ejecutarlo adicionalmente** para una instalación nueva.

## ¿Por qué no hay archivos `001_...sql`, `002_...sql`, etc.?

Porque se estableció como requisito del proyecto mantener una base definitiva
y evitar que el usuario tenga que aplicar correcciones incrementales.

Por tanto, la documentación de migraciones está presente para dejar clara
la decisión técnica, pero no se requieren scripts incrementales para
ejecutar el sistema.
