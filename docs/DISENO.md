# Diseño — this-contacts

Diseño aprobado el 28/10/2026. Las pantallas están en el lienzo de diseño del proyecto (Claude Design).

## Concepto

Un **lienzo en blanco y negro**: simple, sin colores, con escala de grises. La jerarquía se logra con contraste, peso tipográfico y espacio, no con color.

## Paleta

| Token | Valor | Uso |
|---|---|---|
| `--background` | `#ffffff` | Fondo de la app |
| `--foreground` | `#111111` | Texto principal, botones primarios, avatar de favoritos |
| `--muted-foreground` | `#6b6b6b` | Texto secundario, rótulos, placeholders |
| `--secondary-foreground` | `#444444` | Párrafos de apoyo, notas |
| `--input` | `#bdbdbd` | Bordes de campos y botones secundarios |
| `--border` | `#e2e2e2` | Separadores y bordes suaves |
| `--muted` | `#ebebeb` | Avatar de contactos no favoritos |
| `--accent` | `#f0f0f0` / `#f5f5f5` | Ítem seleccionado, avisos |
| `--overlay` | `rgba(17,17,17,0.55)` | Fondo detrás de diálogos |

Todos los textos cumplen contraste mínimo 4.5:1 (WCAG AA).

Los colores se implementan como **variables CSS** en el tema de shadcn/ui (base *neutral*), ajustadas a estos valores. Esto deja preparado el futuro menú de ajustes (colores, tamaños, tipografías y modo oscuro).

## Tipografía

| Uso | Fuente | Detalle |
|---|---|---|
| Texto general | **IBM Plex Sans** | 400, 500 y 600 |
| Rótulos (FAVORITOS, TELÉFONOS, letras A, B, C) | **IBM Plex Mono** | 12 px, mayúsculas, `letter-spacing: 0.08em` |

| Elemento | Tamaño |
|---|---|
| Título de pantalla | 28 px / 600 |
| Nombre en el detalle | 26 px (celular), 32 px (escritorio) / 600 |
| Texto de lista y campos | 16 px |
| Texto secundario | 14 px |

## Medidas

| Elemento | Valor |
|---|---|
| Alto mínimo de botones y áreas táctiles | 44 px |
| Alto de campos de formulario | 48 px |
| Radio de bordes (botones, campos) | 12 px |
| Radio de tarjetas | 16 px |
| Avatar en lista / detalle | 44 px / 96 px |
| Margen lateral en celular | 20 px |

## Íconos

Íconos de línea (trazo 1.75 px), sin relleno salvo la estrella de favorito. En la implementación se usa **lucide-react** (la librería de íconos de shadcn/ui). Sin emojis.

## Pantallas

| Pantalla | Contenido |
|---|---|
| **Lista** | Buscador, sección de favoritos, contactos agrupados por inicial, botón flotante "Nuevo". |
| **Detalle** | Avatar, nombre, teléfonos y emails como enlaces (`tel:` / `mailto:`), dirección, cumpleaños, notas. Acciones: favorito, editar, eliminar. |
| **Formulario** | Crear y editar. Teléfonos y emails múltiples (agregar / quitar). Checkbox de favorito. |
| **Confirmar eliminación** | Diálogo inferior (celular) con aviso de que no se puede deshacer. |
| **Respaldo** | Exportar e importar JSON, con aviso de que importar reemplaza la agenda. |
| **Agenda vacía** | Primer uso: crear contacto o importar respaldo. |
| **Escritorio** | Dos columnas: lista a la izquierda, detalle a la derecha. |

## Reglas

- **Responsive, mobile first.** En pantallas anchas la lista y el detalle se muestran lado a lado; en angostas, una pantalla por vez.
- **Favoritos sin color:** avatar negro con iniciales blancas y estrella rellena. El resto, avatar gris claro.
- **Errores sin color:** borde de 2 px, ícono y mensaje que explica cómo corregir (`aria-invalid` y `aria-describedby`).
- **Acciones destructivas** siempre con confirmación.
- **Accesibilidad:** elementos semánticos (`button`, `a`, `label`), `aria-label` en botones solo con ícono, foco visible y navegación con teclado.

## Fuera del MVP

- Menú de ajustes: colores, tamaños, tipografías y modo oscuro.
