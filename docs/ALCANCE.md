# Alcance del proyecto — this-contacts

> Aplicación para agendar y gestionar contactos.

Aplicación **full stack** que corre **localmente**: frontend, backend (API) y base de datos en la máquina del usuario.

## Principios

- **Un solo usuario, sin cuentas ni login.**
- **Local:** la API y la base de datos corren en la máquina del usuario. La API escucha solo en `localhost`, así nadie desde otra máquina puede acceder sin autenticación.
- **Privacidad:** los datos no salen de la máquina del usuario.
- **Sin nada raro:** sin publicidad, métricas ni trackers.

## Funcionalidades del MVP

| # | Funcionalidad | Detalle |
|---|---|---|
| 1 | Crear contacto | Formulario con validación (formato de email y teléfono), en el frontend **y** en el backend. |
| 2 | Listar contactos | Orden alfabético, agrupados por inicial. |
| 3 | Ver detalle | Vista con todos los datos del contacto. |
| 4 | Editar contacto | Mismo formulario que la creación, con los datos cargados. |
| 5 | Eliminar contacto | Requiere confirmación. |
| 6 | Buscar | Por nombre, teléfono o email. |
| 7 | Favoritos | Se marcan con una estrella y se muestran primero en la lista. |
| 8 | Exportar / importar JSON | Respaldo descargable de la agenda y restauración desde ese archivo. |

## Modelo de datos

Modelo **relacional**: un contacto tiene muchos teléfonos y muchos emails.

```
Contacto 1 ──── N Telefono
         1 ──── N Email
```

### Contacto

| Campo | Tipo | Notas |
|---|---|---|
| `id` | UUID | Clave primaria |
| `nombre` | texto | Obligatorio |
| `apellido` | texto | Opcional |
| `direccion` | texto | Opcional |
| `cumpleanos` | fecha | Opcional |
| `notas` | texto | Opcional |
| `favorito` | booleano | Por defecto `false` |
| `creadoEn` / `actualizadoEn` | fecha y hora | Automáticos |

### Telefono y Email

| Campo | Tipo | Notas |
|---|---|---|
| `id` | UUID | Clave primaria |
| `contactoId` | UUID | Clave foránea → Contacto. Borrar un contacto borra sus teléfonos y emails |
| `valor` | texto | El número o la dirección de email |

## Fuera del MVP (mejoras futuras)

- Foto de perfil
- Etiquetas o grupos
- Importar / exportar vCard (`.vcf`)
- Recordatorio de cumpleaños
- Menú de ajustes: colores, tamaños y tipografías (incluye modo oscuro)

## Estado

Alcance definido el 27/10/2026. Revisado el mismo día: pasa a ser una aplicación full stack local, con un solo usuario y sin login.
