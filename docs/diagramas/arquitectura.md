# Diagrama de arquitectura

```mermaid
flowchart TB
    U[Usuario: Administrador / Tutor / Estudiante]
    B[Navegador web]
    A[Apache + PHP 8.2]
    C[Controllers]
    M[Models]
    V[Views]
    I[Includes: sesión, auth, validaciones]
    DB[(MySQL 8.0)]
    PMA[phpMyAdmin]

    U --> B
    B --> A
    A --> C
    C --> M
    C --> V
    C --> I
    M --> DB
    PMA --> DB
```

## Explicación

1. El usuario interactúa con el sistema desde el navegador.
2. Apache sirve la aplicación PHP.
3. Los controladores reciben las acciones y aplican las reglas del flujo.
4. Los modelos ejecutan las operaciones sobre MySQL mediante PDO.
5. Las vistas muestran la información al usuario.
6. Los módulos de `includes/` centralizan autenticación, sesiones,
   validaciones y funciones reutilizables.
7. phpMyAdmin permite revisar la base de datos durante desarrollo.
