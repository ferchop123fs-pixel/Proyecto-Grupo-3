# Tabla de pruebas del sistema

> Esta matriz sirve como guía de validación funcional para la entrega.
> Los casos que requieren Docker/MySQL deben ejecutarse en el equipo local.
> La documentación de `docs/VALIDACION_FINAL.md` distingue las validaciones
> estáticas ya realizadas de las pruebas de ejecución.

| ID | Módulo | Rol | Prueba | Pasos resumidos | Resultado esperado |
|---|---|---|---|---|---|
| CP-01 | Login | Todos | Inicio de sesión válido | Introducir credenciales válidas | Se abre el panel correspondiente |
| CP-02 | Login | Todos | Credenciales inválidas | Introducir usuario/clave incorrectos | Se rechaza el acceso y se registra el intento |
| CP-03 | Usuarios | Admin | Crear usuario | Completar formulario válido | Se crea la cuenta |
| CP-04 | Usuarios | Admin | Editar usuario | Modificar datos permitidos | Se guardan los cambios |
| CP-05 | Carreras | Admin | Unicidad de carrera | Intentar duplicar nombre | El sistema impide el duplicado |
| CP-06 | Materias | Admin | Unicidad de materia | Registrar misma materia/carrera | Se rechaza el duplicado |
| CP-07 | Disponibilidad | Tutor | Crear horario | Registrar día y rango válido | Se crea disponibilidad |
| CP-08 | Disponibilidad | Tutor | Horario inválido | Fin menor o igual al inicio | Se rechaza la operación |
| CP-09 | Grupal | Estudiante | Inscribirse | Seleccionar sesión disponible | Inscripción queda `pendiente` |
| CP-10 | Grupal | Estudiante | Retirarse pendiente | Retirar inscripción pendiente | Inscripción se elimina/retira según flujo |
| CP-11 | Grupal | Admin | Aprobar inscripción | Aprobar participante pendiente | Pasa a `aprobada` |
| CP-12 | Grupal | Estudiante | Retiro después de aprobar | Revisar inscripción aprobada | No aparece la acción de retirarse |
| CP-13 | Grupal | Admin | Cancelar sesión | Cancelar e introducir motivo | Sesión cancelada y motivo registrado |
| CP-14 | Grupal | Estudiante | Inscripción en cancelada | Revisar sesión cancelada | No aparece como disponible para inscripción |
| CP-15 | Personal | Estudiante | Solicitar tutoría | Elegir proyecto, tutor y hora disponible | Solicitud queda pendiente |
| CP-16 | Personal | Tutor | Aceptar solicitud | Aceptar solicitud pendiente | Estado del tutor pasa a aceptada |
| CP-17 | Personal | Tutor | Rechazar solicitud | Rechazar solicitud | Solicitud pasa a rechazada |
| CP-18 | Personal | Admin | Confirmar solicitud | Asignar modalidad y aula/enlace | Tutoría pasa a programada |
| CP-19 | Personal | Estudiante | Cancelar tutoría | Intentar cancelar | Acción no permitida al estudiante |
| CP-20 | Proyectos | Admin | Cambiar estado | Finalizar proyecto concluido | Proyecto queda finalizado |
| CP-21 | Evaluaciones | Admin/Tutor | Registrar evaluación | Crear evaluación válida | Evaluación almacenada |
| CP-22 | Notificaciones | Estudiante | Ver aviso | Abrir notificaciones | Se muestran avisos disponibles |
| CP-23 | Auditoría | Admin | Consultar auditoría | Abrir módulo de auditoría | Se muestran registros |
| CP-24 | Seguridad | Estudiante | Acceder a módulo administrativo | Intentar URL restringida | Acceso denegado/redirigido |
| CP-25 | Docker | Operador | Levantar sistema | `docker compose up --build -d` | Los tres servicios quedan activos |
| CP-26 | BD | Operador | Inicialización limpia | `docker compose down -v` + `up` | `init.sql` crea esquema y datos |
| CP-27 | Integridad | Operador | Claves foráneas | Crear registro con relación inexistente | Operación rechazada |
| CP-28 | Sesión | Todos | Cerrar sesión | Pulsar cerrar sesión | Sesión termina y se exige nuevo login |

## Registro de ejecución

Para la entrega final, se recomienda completar estas columnas durante la
prueba local:

- fecha;
- ejecutor;
- resultado (`OK` / `FALLÓ`);
- observación;
- evidencia/captura.

Esto evita presentar como ejecutadas pruebas que solo fueron diseñadas o
validadas estáticamente.
