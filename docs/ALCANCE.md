# Alcance del proyecto — this-contacts

> Aplicación para agendar y gestionar contactos.

## Principios

- **Personal:** un solo usuario, sin cuentas ni login.
- **Offline:** los datos se guardan en el dispositivo del usuario y nunca salen de él.
- **Sin nada raro:** sin publicidad, métricas, trackers ni servicios de terceros.

## Funcionalidades del MVP

| # | Funcionalidad | Detalle |
|---|---|---|
| 1 | Crear contacto | Formulario con validación básica (formato de email y teléfono). |
| 2 | Listar contactos | Orden alfabético, agrupados por inicial. |
| 3 | Ver detalle | Vista con todos los datos del contacto. |
| 4 | Editar contacto | Mismo formulario que la creación, con los datos cargados. |
| 5 | Eliminar contacto | Requiere confirmación. |
| 6 | Buscar | Por nombre, teléfono o email. |
| 7 | Favoritos | Se marcan con una estrella y se muestran primero en la lista. |
| 8 | Exportar / importar JSON | Respaldo y traspaso de la agenda entre dispositivos. |

## Modelo de datos: Contacto

| Campo | Tipo | Obligatorio |
|---|---|---|
| `id` | string (UUID) | ✅ (interno) |
| `nombre` | string | ✅ |
| `apellido` | string | — |
| `telefonos` | string[] | — |
| `emails` | string[] | — |
| `direccion` | string | — |
| `cumpleanos` | fecha (YYYY-MM-DD) | — |
| `notas` | string | — |
| `favorito` | boolean (por defecto `false`) | — |
| `creadoEn` | fecha y hora ISO | ✅ (interno) |
| `actualizadoEn` | fecha y hora ISO | ✅ (interno) |

## Fuera del MVP (mejoras futuras)

- Foto de perfil
- Etiquetas o grupos
- Importar / exportar vCard (`.vcf`)
- Recordatorio de cumpleaños
- Modo oscuro

## Estado

Alcance definido el 27/10/2026. Los campos pueden revisarse en etapas posteriores.
