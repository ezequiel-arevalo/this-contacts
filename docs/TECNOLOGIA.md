# Tecnología — this-contacts

## Formato: PWA (Progressive Web App)

Una web instalable en celular y computadora que funciona sin conexión y guarda los datos en el propio dispositivo. Un solo código para todas las plataformas, sin tiendas de apps.

## Stack

| Capa | Tecnología | Por qué |
|---|---|---|
| Lenguaje | **TypeScript** | El modelo de contacto queda tipado; evita errores al editar, importar y exportar. |
| UI | **React** | Estándar más difundido, mucha documentación, encaja con lista / detalle / formulario. |
| Build | **Vite** | Arranque rápido, poca configuración y plugin oficial para PWA. |
| Offline | **vite-plugin-pwa** | Service worker para que la app cargue sin conexión y sea instalable. |
| Almacenamiento | **localStorage** (MVP) | Simple, nativo del navegador, sin dependencias. Suficiente para una agenda personal. |
| Estilos | **CSS Modules** | Sin dependencias extra; los estilos salen del diseño de la etapa 4. |
| Testing | **Vitest + Testing Library + Playwright** | Lógica, componentes y flujo completo (incluido offline). |
| Deploy | **GitHub Pages + GitHub Actions** | Gratis, en el mismo repo; la app es estática y no necesita servidor. |

## Almacenamiento: localStorage ahora, migrable después

En el MVP los contactos se guardan en `localStorage` bajo una clave versionada (`this-contacts:v1`), serializados como JSON.

Para poder cambiar de almacenamiento más adelante sin reescribir la app, **ningún componente usa `localStorage` directamente**. Todo el acceso pasa por una interfaz:

```ts
interface ContactRepository {
  getAll(): Promise<Contact[]>;
  getById(id: string): Promise<Contact | undefined>;
  create(data: NewContact): Promise<Contact>;
  update(id: string, data: Partial<NewContact>): Promise<Contact>;
  remove(id: string): Promise<void>;
  replaceAll(contacts: Contact[]): Promise<void>; // usado por la importación JSON
}
```

- MVP: `LocalStorageContactRepository`.
- Futuro: `IndexedDBContactRepository` (por ejemplo con Dexie.js), con la misma interfaz.

La interfaz es asíncrona (`Promise`) aunque localStorage sea síncrono, para que la migración a IndexedDB no obligue a cambiar el resto del código. La clave versionada permite escribir una migración de datos cuando cambie el formato.

### Limitaciones conocidas de localStorage

- Capacidad de unos 5 MB por origen: alcanza para miles de contactos de texto, pero no para fotos (que están fuera del MVP).
- El navegador puede borrar los datos si el usuario limpia el almacenamiento del sitio. Por eso la **exportación JSON** es parte del MVP: es el respaldo.

## Principios que respeta el stack

Sin backend, sin cuentas, sin publicidad, sin métricas y sin servicios de terceros en tiempo de ejecución. Los datos nunca salen del dispositivo.

## Descartado

- **App nativa (Android/iOS):** dos códigos y publicación en tiendas; excesivo para el MVP.
- **Electron:** más de 100 MB para una agenda y no funciona en celular.
- **Framework con servidor (Next.js, etc.):** no hay backend que lo justifique.

## Estado

Tecnología definida el 27/10/2026.
