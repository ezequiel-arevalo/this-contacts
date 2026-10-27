# Tecnología — this-contacts

Aplicación **full stack local**: frontend, API y base de datos corren en la máquina del usuario. Un solo usuario, sin login.

## Arquitectura

```
Navegador (React)  ──HTTP/JSON──▶  API (Express, solo localhost)  ──Prisma──▶  PostgreSQL
```

## Stack

### Frontend

| Tecnología | Por qué |
|---|---|
| **TypeScript** | Tipado en todo el proyecto; los mismos tipos se comparten con el backend. |
| **React** | Interfaz en componentes; estándar del mercado. |
| **Vite** | Arranque rápido, recarga al guardar y plantilla oficial para React + TypeScript. |
| **Tailwind CSS** | Estilos con clases utilitarias, consistentes y sin archivos CSS que crecen sin control. |
| **shadcn/ui** | Componentes accesibles (diálogos, formularios, foco y teclado) copiados al proyecto. Su tema *neutral* coincide con la paleta en grises del diseño. |
| **React Router** | Navegación entre lista, detalle, formulario y respaldo. |
| **TanStack Query** | Maneja los datos que vienen de la API: carga, caché, errores y actualización después de crear, editar o borrar. Reemplaza a Redux para datos del servidor. |
| **React Hook Form + Zod** | Formularios con validación, usando los mismos esquemas que valida el backend. |

### Backend

| Tecnología | Por qué |
|---|---|
| **Node.js + TypeScript** | Mismo lenguaje que el frontend: el esfuerzo de aprendizaje va a lo nuevo (HTTP, API, SQL). |
| **Express** | El framework de Node más usado y documentado. Mínimo, sin "magia": se entiende qué pasa en cada pedido. |
| **Zod** | Valida todo lo que llega a la API antes de tocar la base de datos. |
| **Prisma** | Define las tablas en un esquema legible, genera migraciones y consultas tipadas. |

### Base de datos

**PostgreSQL**, instalado localmente.

- Base relacional: tablas, claves foráneas y relaciones uno-a-muchos (Contacto → Teléfonos / Emails).
- Tipos estrictos y nativos (`UUID`, `BOOLEAN`, `DATE`).
- La más pedida hoy entre las bases SQL.
- Búsqueda sin distinguir mayúsculas con `ILIKE`.

## Estructura del repositorio

Monorepo con **npm workspaces**: un solo repo, un solo `npm install`.

```
this-contacts/
├── apps/
│   ├── web/        # Frontend (React)
│   └── api/        # Backend (Express + Prisma)
├── packages/
│   └── shared/     # Esquemas Zod y tipos compartidos
└── docs/
```

## Organización del código

**Separación de responsabilidades** en capas, con el patrón Repositorio en ambos lados:

- **Backend:** `rutas → servicios → repositorio → base de datos`
  - *Rutas:* reciben el pedido HTTP, validan con Zod y responden.
  - *Servicios:* reglas de negocio (por ejemplo, favoritos primero, importación JSON).
  - *Repositorio:* único lugar que habla con Prisma.
- **Frontend:** los componentes no hacen `fetch` directo; usan un cliente de API (`contactsApi`) consumido desde TanStack Query.

## API REST

| Método | Ruta | Acción |
|---|---|---|
| `GET` | `/api/contacts?q=` | Listar contactos (con búsqueda opcional) |
| `GET` | `/api/contacts/:id` | Obtener un contacto |
| `POST` | `/api/contacts` | Crear contacto |
| `PUT` | `/api/contacts/:id` | Editar contacto |
| `PATCH` | `/api/contacts/:id/favorite` | Marcar o desmarcar favorito |
| `DELETE` | `/api/contacts/:id` | Eliminar contacto |
| `GET` | `/api/export` | Descargar respaldo JSON |
| `POST` | `/api/import` | Restaurar desde respaldo JSON (reemplaza la agenda) |

La API escucha solo en `127.0.0.1`, así no es accesible desde otras máquinas de la red.

## Testing

| Herramienta | Qué prueba |
|---|---|
| **Vitest** | Lógica de negocio y esquemas de validación (frontend y backend). |
| **Supertest** | Endpoints de la API contra una base de datos de prueba. |
| **Testing Library** | Componentes de React. |
| **Playwright** | Flujo completo en el navegador (crear, buscar, editar, eliminar, exportar). |

## Instalación ("deploy" local)

Al ser una app local, el deploy es poder clonarla y levantarla en cualquier máquina:

1. Instalar Node.js y PostgreSQL.
2. Clonar el repo y ejecutar `npm install`.
3. Copiar `.env.example` a `.env` con la URL de la base de datos.
4. Crear las tablas con las migraciones de Prisma.
5. Iniciar con `npm run dev` (desarrollo) o compilar y ejecutar con `npm start`.

Los pasos exactos se documentan en el README durante el desarrollo.

## Versionado

- **SemVer** (`MAYOR.MENOR.PARCHE`); cada versión se marca con un tag de Git.
- **v1.0.0**: MVP completo (desarrollo, testing, instalación documentada).

## Descartado

| Opción | Motivo |
|---|---|
| localStorage / app solo frontend | Se eligió aprender backend y base de datos. |
| Login y cuentas | App local de un solo usuario; no aporta. |
| Docker | Agrega complejidad innecesaria para una app local. |
| SQLite | Más simple, pero se priorizó PostgreSQL por potencia, tipos estrictos y mercado. |
| MySQL / MariaDB | Misma complejidad de instalación que PostgreSQL, con menos demanda en proyectos nuevos. |
| MongoDB | No permite aprender relaciones ni SQL, lo más formativo para este proyecto. |
| NestJS | Mucha estructura y abstracción sobre Express; primero conviene entender Express. |
| Redux | TanStack Query resuelve mejor los datos que vienen de una API. |
| daisyUI | Temas coloridos que habría que neutralizar; shadcn/ui encaja mejor con el diseño. |
| PWA / modo offline | La app depende de una API local; queda como mejora futura. |

## Estado

Tecnología cerrada el 27/10/2026: full stack local con React, Express, Prisma y PostgreSQL. Cualquier cambio de stack queda para después de la v1.0.0.
