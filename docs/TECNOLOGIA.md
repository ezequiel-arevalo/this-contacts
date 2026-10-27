# Tecnología — this-contacts

## Formato: PWA (Progressive Web App)

Una web instalable en celular y computadora que funciona sin conexión y guarda los datos en el propio dispositivo. Un solo código para todas las plataformas, sin tiendas de apps.

## Roadmap de versiones

| Versión | Qué cambia | Objetivo de aprendizaje |
|---|---|---|
| **v1.0** | MVP completo en HTML, CSS y JavaScript puro, con localStorage, tests y deploy. | Bases, manipulación del DOM, lógica de negocio, patrón Repositorio. |
| **v2.0** | Migración de la interfaz a React. | Componentes, estado y qué problemas resuelve un framework. |
| **v2.x** *(opcional)* | Migración a TypeScript. | Tipado, sobre una app ya conocida. |

Reglas:

- **La v1.0 se termina completa** (desarrollo, testing, deploy y documentación) antes de empezar la v2.0.
- **Un cambio grande por versión**, para saber qué rompió algo si algo se rompe.
- Versionado con **SemVer** (`MAYOR.MENOR.PARCHE`); cada versión se marca con un tag de Git.

## Stack de la v1.0

| Capa | Tecnología | Por qué |
|---|---|---|
| Estructura | **HTML** | Base de toda web; se aprende sin abstracciones de por medio. |
| Estilos | **CSS** (archivos propios, variables CSS) | Sin dependencias; los estilos salen del diseño de la etapa 4. |
| Lógica | **JavaScript** (módulos ES, `import` / `export`) | Separar el código en archivos con una responsabilidad cada uno. |
| Build y servidor local | **Vite** | Recarga automática al guardar y build listo para producción, sin configuración. |
| Offline | **vite-plugin-pwa** | Service worker para que la app cargue sin conexión y sea instalable. |
| Almacenamiento | **localStorage** | Nativo, simple, suficiente para una agenda personal. |
| Testing | **Vitest** (lógica) + **Playwright** (flujo completo) | Probar reglas de negocio por separado y la app como la usaría una persona. |
| Deploy | **GitHub Pages + GitHub Actions** | Gratis, en el mismo repo; la app es estática y no necesita servidor. |

## Almacenamiento: localStorage

Los contactos se guardan en `localStorage` bajo una clave versionada (`this-contacts:v1`), como un arreglo serializado en JSON.

- **Por qué no sessionStorage:** tiene la misma API pero se borra al cerrar la pestaña; no sirve para una agenda.
- **Por qué no IndexedDB:** es más potente, pero su API es más compleja y este proyecto no lo necesita. Queda para un proyecto futuro donde se justifique (mucho volumen de datos, imágenes o archivos offline).

### Patrón Repositorio

Ningún archivo de la interfaz usa `localStorage` directamente. Todo el acceso pasa por un único módulo, `contactRepository.js`, con funciones fijas:

| Función | Qué hace |
|---|---|
| `obtenerTodos()` | Devuelve todos los contactos. |
| `obtenerPorId(id)` | Devuelve un contacto o `undefined`. |
| `crear(datos)` | Crea un contacto y lo devuelve. |
| `actualizar(id, datos)` | Modifica un contacto y lo devuelve. |
| `eliminar(id)` | Borra un contacto. |
| `reemplazarTodos(contactos)` | Reemplaza la agenda completa (usado al importar JSON). |

Beneficios: **separación de responsabilidades** (la interfaz muestra, el repositorio guarda), un solo lugar donde buscar errores de guardado y la posibilidad de cambiar de almacenamiento sin tocar el resto de la app.

### Limitaciones conocidas de localStorage

- Capacidad de unos 5 MB por origen: alcanza para miles de contactos de texto, pero no para fotos (fuera del MVP).
- El navegador puede borrar los datos si el usuario limpia el almacenamiento del sitio. Por eso la **exportación JSON** es parte del MVP: es el respaldo.

## Principios que respeta el stack

Sin backend, sin cuentas, sin publicidad, sin métricas y sin servicios de terceros en tiempo de ejecución. Los datos nunca salen del dispositivo.

## Descartado

- **App nativa (Android/iOS):** dos códigos y publicación en tiendas; excesivo para el MVP.
- **Electron:** más de 100 MB para una agenda y no funciona en celular.
- **Framework con servidor (Next.js, etc.):** no hay backend que lo justifique.
- **Versiones alternativas por base de datos:** se elige un almacenamiento por proyecto.

## Estado

Tecnología definida el 27/10/2026. Actualizada el mismo día: v1.0 en HTML, CSS y JavaScript puro con migración a React en v2.0.
