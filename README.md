# Sistema de Tutorías UPDS

Sistema web académico para la gestión de tutorías, estudiantes, tutores,
materias, proyectos de grado, evaluaciones, horarios, notificaciones y
auditoría institucional.

## 1. Tecnologías utilizadas

- **PHP 8.2 + Apache**
- **MySQL 8.0**
- **PDO / PDO MySQL**
- **HTML5, CSS3 y JavaScript**
- **Bootstrap 5, Bootstrap Icons, SweetAlert2 y Chart.js**
- **Docker y Docker Compose**
- **phpMyAdmin** para administración visual de la base de datos

La aplicación utiliza una arquitectura MVC ligera:

```text
controllers/  -> recibe solicitudes y ejecuta acciones
models/       -> consultas y lógica de acceso a datos
views/        -> interfaz del sistema
includes/     -> sesiones, autenticación, validaciones y helpers
config/       -> conexión a la base de datos
database/     -> estructura y datos iniciales
assets/       -> CSS, JavaScript, imágenes y recursos visuales
docs/         -> documentación técnica y funcional
```

---

## 2. Requisitos previos

Antes de instalar el sistema se necesita:

1. **Docker Desktop** instalado y ejecutándose.
2. **Visual Studio Code** (recomendado).
3. Git, si se desea trabajar con el repositorio.
4. Puertos disponibles:
   - `8000` para la aplicación web.
   - `8080` para phpMyAdmin.
   - `3306` para MySQL.

> El proyecto está preparado para ejecutarse con Docker. No se requiere
> XAMPP para levantar la aplicación.

---

## 3. Instalación

### Paso 1 — Abrir el proyecto

Descomprimir el proyecto y abrir la carpeta `tecnologiasweb` en VS Code.

### Paso 2 — Crear la configuración local

Copiar:

```text
.env.example
```

como:

```text
.env
```

El archivo `.env` es local y **no debe subirse al repositorio**.

Ejemplo:

```env
DB_HOST=db
DB_NAME=tutorias_db
DB_USER=tutorias_user
DB_PASS=cambiar_esta_clave
DB_ROOT_PASSWORD=cambiar_clave_root
PMA_USER=root
```

Las claves mostradas en `.env.example` son valores de ejemplo y deben
reemplazarse en una instalación real.

### Paso 3 — Levantar los contenedores

Desde la terminal de VS Code, dentro de `tecnologiasweb`:

```bash
docker compose up --build -d
```

Comprobar:

```bash
docker compose ps
```

Se deben observar los servicios:

- `tutorias_web`
- `tutorias_db`
- `tutorias_phpmyadmin`

### Paso 4 — Abrir el sistema

Aplicación:

```text
http://localhost:8000
```

phpMyAdmin:

```text
http://localhost:8080
```

En phpMyAdmin, la base de datos utilizada por el sistema es:

```text
tutorias_db
```

---

## 4. Base de datos

### Fuente definitiva

El archivo principal es:

```text
database/init.sql
```

Este archivo contiene:

- creación de la base de datos;
- tablas;
- claves primarias y foráneas;
- restricciones;
- índices;
- datos iniciales;
- datos de prueba;
- vistas y estructuras auxiliares necesarias por el sistema.

### ¿Existen migraciones?

Por decisión del proyecto, **no se utilizan migraciones incrementales**.

La fuente única y definitiva del esquema es:

```text
database/init.sql
```

Cuando se necesita una modificación estructural, se actualiza directamente
`init.sql`, de manera que una instalación limpia siempre genere la versión
correcta.

La carpeta:

```text
database/migrations/
```

contiene la explicación de esta decisión y el historial/documentación de
criterio. No es necesario ejecutar archivos SQL incrementales.

También existe:

```text
scripts_bd/sistema_tutorias.sql
```

como script SQL de referencia histórica. Para una instalación nueva se debe
usar **`database/init.sql`**.

### Recrear completamente la base de datos

Si se desea comenzar desde cero:

```bash
docker compose down -v
docker compose up --build -d
```

> `down -v` elimina el volumen de MySQL. Utilizarlo solamente cuando se
> quiera borrar la base local y reconstruirla.

---

## 5. Configuración y contraseñas

El sistema toma las variables principales desde el entorno Docker:

```text
DB_HOST
DB_NAME
DB_USER
DB_PASS
```

El archivo de ejemplo:

```text
.env.example
```

**no contiene contraseñas reales**.

El archivo:

```text
config/conexion.php
```

mantiene la conexión existente del proyecto y no se creó una segunda capa
de conexión.

---

## 6. Datos de prueba

La instalación incluye datos para probar los tres roles principales y los
diferentes estados del sistema.

La explicación de los datos y cuentas de prueba se encuentra en:

```text
docs/DATOS_PRUEBA.md
```

Los datos incluyen, entre otros:

- administradores;
- tutores;
- estudiantes;
- carreras;
- materias;
- disponibilidad;
- proyectos de grado;
- tutorías personales;
- tutorías grupales;
- participantes;
- evaluaciones;
- notificaciones;
- registros de auditoría.

---

## 7. Funcionalidades principales

### Administrador

Permite gestionar y supervisar:

- usuarios;
- estudiantes;
- tutores;
- carreras;
- materias;
- disponibilidades;
- tutorías grupales;
- solicitudes de tutorías;
- programación;
- proyectos de grado;
- evaluaciones;
- reportes;
- auditoría;
- cancelaciones y motivos;
- estados finales de los proyectos.

### Tutor

Permite:

- consultar sus tutorías;
- gestionar disponibilidad;
- revisar materias asignadas;
- aceptar o rechazar solicitudes personales;
- atender tutorías grupales;
- consultar estudiantes;
- registrar acciones relacionadas con sus sesiones;
- trabajar con proyectos de grado según los permisos definidos.

### Estudiante

Permite:

- consultar perfil y carrera;
- consultar materias;
- solicitar tutorías;
- inscribirse en tutorías grupales;
- consultar solicitudes;
- consultar proyectos de grado;
- revisar evaluaciones;
- consultar notificaciones;
- visualizar tutorías programadas.

---

## 8. Flujos de tutorías

### Tutoría grupal de materia

El flujo implementado es:

```text
Estudiante
    |
    v
Solicita inscripción
    |
    v
Pendiente
    |
    +---- Retirarse permitido
    |
    v
Administrador revisa
    |
    v
Aprobada
    |
    +---- Retirarse ya no permitido
    |
    v
Participación activa
```

Las sesiones grupales utilizan los bloques institucionales definidos en el
sistema:

- Mañana: `07:00 - 10:00`
- Tarde: `15:00 - 18:00`
- Noche: `19:00 - 22:00`

### Tutoría personal / proyecto de grado

```text
Estudiante
    |
    v
Selecciona proyecto + tutor + horario disponible
    |
    v
Pendiente
    |
    v
Tutor acepta / rechaza
    |
    +---- Rechazada
    |
    v
Administrador confirma
    |
    +---- Asigna modalidad y aula/enlace
    |
    v
Programada
    |
    v
Realizada
```

El estudiante no asigna el aula o enlace definitivo; esa acción corresponde
a administración.

---

## 9. Documentación incluida

La documentación del proyecto está organizada así:

```text
docs/
├── diagramas/
│   ├── arquitectura.md
│   └── modelo-datos.md
├── manual/
│   └── MANUAL_USUARIO.md
├── pruebas/
│   └── TABLA_PRUEBAS.md
├── AUDITORIA_FUSION.md
├── BASE_REFERENCIA_README.md
├── CHANGELOG_CORRECCIONES.md
├── DATOS_PRUEBA.md
├── README_FUSION.md
└── VALIDACION_FINAL.md
```

### Diagramas

Los diagramas se encuentran en:

```text
docs/diagramas/
```

Están escritos en Mermaid para que puedan visualizarse en VS Code,
GitHub u otros visores compatibles.

### Manual de usuario

El manual completo está en:

```text
docs/manual/MANUAL_USUARIO.md
```

Incluye las operaciones principales para administrador, tutor y estudiante.

### Tabla de pruebas

La matriz de pruebas está en:

```text
docs/pruebas/TABLA_PRUEBAS.md
```

Contiene casos para login, CRUD, permisos, tutorías grupales, tutorías
personales, proyectos, evaluaciones, auditoría y seguridad básica.

---

## 10. Validación del proyecto

La documentación existente registra una revisión estática del proyecto.

Consultar:

```text
docs/VALIDACION_FINAL.md
```

y:

```text
docs/AUDITORIA_FUSION.md
```

Estas validaciones documentan la revisión de sintaxis PHP, rutas, estructura
y configuración.

La prueba real de Docker/MySQL debe realizarse en el equipo donde Docker
esté instalado.

---

## 11. Comandos útiles

### Ver contenedores

```bash
docker compose ps
```

### Ver logs

```bash
docker compose logs -f
```

Solo web:

```bash
docker compose logs -f web
```

Solo base de datos:

```bash
docker compose logs -f db
```

### Detener el proyecto

```bash
docker compose down
```

### Reconstruir después de cambios

```bash
docker compose up --build -d
```

### Reiniciar desde una base limpia

```bash
docker compose down -v
docker compose up --build -d
```

---

## 12. Solución rápida de problemas

### El sistema no abre en `localhost:8000`

Comprobar:

```bash
docker compose ps
```

y:

```bash
docker compose logs -f web
```

### La base no tiene las tablas

La inicialización de MySQL solo se ejecuta automáticamente cuando se crea
el volumen por primera vez.

Recrear:

```bash
docker compose down -v
docker compose up --build -d
```

### Hay problemas con cambios recientes del SQL

No ejecutar migraciones incrementales. Recrear la base utilizando el
`database/init.sql` definitivo:

```bash
docker compose down -v
docker compose up --build -d
```

---

## 13. Entrega académica

Para la entrega se consideran los siguientes elementos:

| Requisito | Ubicación |
|---|---|
| README de instalación/ejecución | `README.md` |
| Script definitivo de BD | `database/init.sql` |
| Criterio de migraciones | `database/migrations/README.md` |
| Configuración de ejemplo | `.env.example` |
| Diagramas | `docs/diagramas/` |
| Manual de usuario | `docs/manual/MANUAL_USUARIO.md` |
| Tabla de pruebas | `docs/pruebas/TABLA_PRUEBAS.md` |
| Datos de prueba | `docs/DATOS_PRUEBA.md` |
| Auditoría técnica | `docs/AUDITORIA_FUSION.md` |
| Validación final | `docs/VALIDACION_FINAL.md` |
| Historial de correcciones | `docs/CHANGELOG_CORRECCIONES.md` |

Si un requisito ya forma parte del código funcional del sistema, se
documenta en este README y en el manual correspondiente en lugar de
duplicarlo innecesariamente.
