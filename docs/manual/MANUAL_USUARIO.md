# Manual de usuario — Sistema de Tutorías UPDS

## 1. Acceso

1. Abrir `http://localhost:8000`.
2. Introducir usuario y contraseña de una cuenta de prueba.
3. El sistema redirige al panel correspondiente al rol.

Las cuentas de prueba disponibles están documentadas en
`docs/DATOS_PRUEBA.md`.

---

## 2. Administrador

### Panel

El panel administrativo concentra indicadores, accesos y operaciones
institucionales.

### Usuarios

Desde la gestión de usuarios se pueden consultar, crear, editar o desactivar
cuentas según los permisos del sistema.

### Estudiantes y tutores

Permite mantener la información académica y profesional asociada a cada
usuario.

### Carreras y materias

Permite administrar carreras y materias, manteniendo las restricciones de
unicidad definidas por la base de datos.

### Solicitudes de tutoría

El administrador revisa solicitudes y realiza las acciones que corresponden
al ciclo de programación.

### Tutorías grupales

Puede consultar participantes, aprobar inscripciones y cancelar sesiones.
Cuando se cancela una sesión, el motivo queda registrado para auditoría.

### Tutorías personales

El administrador confirma las solicitudes aceptadas por el tutor y asigna
la modalidad y el aula o enlace cuando corresponde.

### Proyectos de grado

Puede consultar y administrar el estado del proyecto conforme al flujo
definido:

```text
Propuesto -> En proceso -> Finalizado
```

También existe el estado `Cancelado` cuando corresponde.

### Auditoría

Permite revisar trazabilidad de acciones y registros relevantes del sistema.

---

## 3. Tutor

### Perfil

Permite revisar la información propia.

### Disponibilidad

El tutor registra los días y horas en los que puede atender tutorías
personales.

### Tutorías grupales

Puede consultar las sesiones de materias que tiene asignadas y gestionar las
acciones permitidas por su rol.

### Tutorías personales

Puede revisar solicitudes y aceptar o rechazar las que correspondan.

Una solicitud aceptada continúa al proceso de confirmación administrativa.

### Proyectos de grado

Permite consultar los proyectos relacionados con su trabajo como tutor.

---

## 4. Estudiante

### Perfil y materias

Permite consultar datos personales, carrera y materias.

### Tutoría grupal

1. Entrar a tutorías grupales.
2. Seleccionar una sesión disponible.
3. Pulsar **Inscribirme**.
4. La inscripción queda como `pendiente`.
5. Mientras permanece pendiente puede utilizarse **Retirarme**.
6. Después de la aprobación administrativa pasa a `aprobada`.

Las sesiones canceladas no deben aparecer como disponibles para inscripción.

### Tutoría personal

1. Seleccionar el proyecto de grado.
2. Seleccionar tutor.
3. Revisar los horarios disponibles.
4. Enviar la solicitud.
5. Esperar aceptación o rechazo del tutor.
6. Si es aceptada, esperar la confirmación administrativa.
7. Una vez programada se muestra la modalidad y el aula/enlace asignados.

El estudiante no asigna el aula o enlace de la tutoría.

### Evaluaciones

Permite consultar las evaluaciones asociadas a sus tutorías.

### Notificaciones

Muestra avisos generados por acciones importantes del sistema, como
aprobaciones.

---

## 5. Estados principales

### Tutoría grupal

- `pendiente`
- `aprobada`
- `asistio`
- `no_asistio`

### Tutoría personal

- `pendiente`
- `programada`
- `rechazada`
- `realizada`
- `cancelada`
- `en_proceso`
- `detenido`

### Proyecto de grado

- `propuesto`
- `en_proceso`
- `finalizado`
- `cancelado`

---

## 6. Cierre de sesión

Utilizar la opción **Cerrar sesión** del menú lateral.

Para proteger la cuenta, no compartir las credenciales de acceso.

---

## 7. Problemas frecuentes

### No aparecen los datos

Comprobar que MySQL esté activo:

```bash
docker compose ps
```

### Se modificó `init.sql` y la base no cambia

MySQL ejecuta el archivo de inicialización al crear el volumen. Para
reconstruir una instalación local:

```bash
docker compose down -v
docker compose up --build -d
```

### No se puede acceder al sistema

Revisar:

```bash
docker compose logs -f web
```

y verificar que `tutorias_web` esté ejecutándose.
