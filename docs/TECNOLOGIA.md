# Tecnología — this-contacts

Aplicación **full stack local**: frontend, API y base de datos corren en la máquina del usuario. Un solo usuario, sin login.

## Arquitectura

```
Navegador (React)  ──HTTP/JSON──▶  API (Laravel, solo localhost)  ──Eloquent──▶  PostgreSQL
```

El frontend es una SPA en React; Laravel se usa **solo como API REST** (sin vistas Blade ni Inertia).

## Stack

### Frontend

| Tecnología | Por qué |
|---|---|
| **TypeScript** | Tipado en todo el frontend: el editor avisa errores antes de ejecutar. |
| **React** | Interfaz en componentes; estándar del mercado. |
| **Vite** | Arranque rápido, recarga al guardar y plantilla oficial para React + TypeScript. Su proxy evita configurar CORS en desarrollo. |
| **Tailwind CSS** | Estilos con clases utilitarias, consistentes y sin archivos CSS que crecen sin control. |
| **shadcn/ui** | Componentes accesibles (diálogos, formularios, foco y teclado) copiados al proyecto. Su tema *neutral* coincide con la paleta en grises del diseño. |
| **React Router** | Navegación entre lista, detalle, formulario y respaldo. |
| **TanStack Query** | Maneja los datos que vienen de la API: carga, caché, errores y actualización después de crear, editar o borrar. Reemplaza a Redux para datos del servidor. |
| **React Hook Form + Zod** | Formularios con validación en el cliente, para dar respuesta inmediata al usuario. |
| **Biome** | Linter y formateador en una sola herramienta (reemplaza a ESLint + Prettier), muy rápido. Elegido además porque typescript-eslint todavía no soporta TypeScript 7, lo que impedía instalar el proyecto con ESLint. |

### Backend

| Tecnología | Por qué |
|---|---|
| **PHP + Laravel** | Framework con todo integrado (rutas, validación, ORM, migraciones, tests y consola `artisan`), convenciones claras y documentación excelente. PHP tiene gran presencia en el mercado. |
| **Eloquent** | ORM de Laravel: modelos, relaciones (`hasMany`, `belongsTo`) y consultas sin SQL a mano cuando no hace falta. |
| **Form Requests** | Validación de todo lo que llega a la API antes de tocar la base de datos. |
| **Migraciones de Laravel** | Las tablas se definen en código y se versionan con Git. |

### Base de datos

**PostgreSQL**, instalado localmente.

- Base relacional: tablas, claves foráneas y relaciones uno-a-muchos (Contacto → Teléfonos / Emails).
- Tipos estrictos y nativos (`UUID`, `BOOLEAN`, `DATE`).
- La más pedida hoy entre las bases SQL.
- Búsqueda sin distinguir mayúsculas con `ILIKE`.

## Estructura del repositorio

Monorepo: dos proyectos independientes en el mismo repositorio, comunicados por la API REST.

```
this-contacts/
├── frontend/       # SPA en React + TypeScript — npm
├── backend/        # API en Laravel (instalación limpia, sin starter kit) — Composer
└── docs/
```

Se evaluó el **starter kit de React de Laravel** (Inertia + React + shadcn/ui) y se descartó para este proyecto: une frontend y backend en un solo proyecto sin API REST e incluye login por defecto, que esta app no usa. Queda como candidato para un próximo proyecto.

## Validación en dos lados

Al usar lenguajes distintos, las reglas de validación se escriben **dos veces**: Zod en el frontend y Form Requests en Laravel. La fuente de verdad es **el backend**: el frontend valida para dar respuesta rápida, pero la API nunca confía en lo que recibe. Las reglas se documentan en un solo lugar (`docs/`) para mantener ambos lados alineados.

## Organización del código

**Separación de responsabilidades** en capas, con el patrón Repositorio en ambos lados:

- **Backend:** `rutas → controladores → servicios → repositorio → Eloquent`
  - *Controladores:* reciben el pedido HTTP (ya validado por un Form Request) y responden.
  - *Servicios:* reglas de negocio (por ejemplo, favoritos primero, importación JSON).
  - *Repositorio:* único lugar que habla con Eloquent.
- **Frontend:** los componentes no hacen `fetch` directo; usan un cliente de API (`contactsApi`) consumido desde TanStack Query.

## API REST

| Método | Ruta | Acción |
|---|---|---|
| `GET` | `/api/contacts?q=` | Listar contactos (con búsqueda opcional) |
| `GET` | `/api/contacts/{id}` | Obtener un contacto |
| `POST` | `/api/contacts` | Crear contacto |
| `PUT` | `/api/contacts/{id}` | Editar contacto |
| `PATCH` | `/api/contacts/{id}/favorite` | Marcar o desmarcar favorito |
| `DELETE` | `/api/contacts/{id}` | Eliminar contacto |
| `GET` | `/api/export` | Descargar respaldo JSON |
| `POST` | `/api/import` | Restaurar desde respaldo JSON (reemplaza la agenda) |

La API escucha solo en `127.0.0.1`, así no es accesible desde otras máquinas de la red.

## Testing

| Herramienta | Qué prueba |
|---|---|
| **Pest** (sobre PHPUnit) | Servicios, validaciones y endpoints de la API con los helpers HTTP de Laravel, contra una base de datos de prueba. |
| **Vitest** | Lógica y esquemas de validación del frontend. |
| **Testing Library** | Componentes de React. |
| **Playwright** | Flujo completo en el navegador (crear, buscar, editar, eliminar, exportar). |

## Instalación ("deploy" local)

Al ser una app local, el deploy es poder clonarla y levantarla en cualquier máquina:

1. Instalar PHP, Composer, Node.js y PostgreSQL.
2. Clonar el repo.
3. Backend: `composer install`, copiar `.env.example` a `.env` con los datos de la base, generar la clave de la app y ejecutar las migraciones.
4. Frontend: `npm install`.
5. Iniciar la API y el frontend.

Los pasos exactos se documentan en el README durante el desarrollo. Se usa la última versión estable de Laravel al iniciar el desarrollo.

## Versionado

- **SemVer** (`MAYOR.MENOR.PARCHE`); cada versión se marca con un tag de Git.
- **v1.0.0**: MVP completo (desarrollo, testing, instalación documentada).

## Descartado

| Opción | Motivo |
|---|---|
| localStorage / app solo frontend | Se eligió aprender backend y base de datos. |
| Login y cuentas | App local de un solo usuario; no aporta. |
| Docker | Agrega complejidad innecesaria para una app local. |
| Node.js + Express | Evaluado; se eligió Laravel por preferencia personal y su presencia en el mercado. |
| NestJS | Mucha estructura y abstracción sobre Express. |
| SQLite | Más simple, pero se priorizó PostgreSQL por potencia, tipos estrictos y mercado. |
| MySQL / MariaDB | Misma complejidad de instalación que PostgreSQL, con menos demanda en proyectos nuevos. |
| MongoDB | No permite aprender relaciones ni SQL, lo más formativo para este proyecto. |
| Blade / Inertia / starter kit de React | El frontend es una SPA independiente en React; Laravel funciona solo como API. El starter kit además trae login por defecto. |
| Redux | TanStack Query resuelve mejor los datos que vienen de una API. |
| daisyUI | Temas coloridos que habría que neutralizar; shadcn/ui encaja mejor con el diseño. |
| ESLint | typescript-eslint (8.72) solo soporta TypeScript < 6.1 y la plantilla de Vite trae TypeScript 7: conflicto de dependencias. Se optó por Biome en lugar de bajar TypeScript a 6.0. |
| PWA / modo offline | La app depende de una API local; queda como mejora futura. |

## Estado

Tecnología cerrada el 27/10/2026: full stack local con React, Laravel y PostgreSQL. Cualquier cambio de stack queda para después de la v1.0.0.
