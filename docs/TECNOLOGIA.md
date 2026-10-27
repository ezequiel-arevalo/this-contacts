# Tecnología — this-contacts

## Formato: PWA (Progressive Web App)

Una web instalable en celular y computadora que funciona sin conexión y guarda los datos en el propio dispositivo. Un solo código para todas las plataformas, sin tiendas de apps.

## Stack

| Capa | Tecnología | Por qué |
|---|---|---|
| Lenguaje | **TypeScript** | El modelo de contacto queda tipado: el editor avisa errores antes de ejecutar (un campo mal escrito, un dato del tipo equivocado al importar). |
| UI | **React** | Interfaz en componentes reutilizables; la pantalla se actualiza sola cuando cambian los datos. Estándar del mercado. |
| Build | **Vite** | Arranque rápido, recarga al guardar, build listo para producción y plantilla oficial para React + TypeScript. |
| Estilos | **Tailwind CSS** | Clases utilitarias que equivalen a propiedades CSS; estilos consistentes sin archivos CSS que crecen sin control. |
| Componentes | **shadcn/ui** | Componentes accesibles (diálogos, formularios, foco y teclado resueltos) que se copian al proyecto y se pueden modificar. Su tema *neutral* coincide con la paleta en escala de grises del diseño. |
| Offline | **vite-plugin-pwa** | Service worker para que la app cargue sin conexión y sea instalable. |
| Almacenamiento | **localStorage** | Nativo, simple, suficiente para una agenda personal. |
| Testing | **Vitest + Testing Library + Playwright** | Lógica de negocio, componentes y flujo completo (incluido el modo offline). |
| Deploy | **GitHub Pages + GitHub Actions** | Gratis, en el mismo repo; la app es estática y no necesita servidor. |

## Tema y variables

Los colores, radios y tipografías se definen como **variables CSS** (el sistema de temas de shadcn/ui funciona así). Esto deja preparado el futuro **menú de ajustes** (colores, tamaños, tipografías, modo oscuro): cambiar el tema es cambiar esas variables, sin tocar los componentes.

## Almacenamiento: localStorage

Los contactos se guardan en `localStorage` bajo una clave versionada (`this-contacts:v1`), como un arreglo serializado en JSON.

- **Por qué no sessionStorage:** tiene la misma API pero se borra al cerrar la pestaña; no sirve para una agenda.
- **Por qué no IndexedDB:** es más potente, pero su API es más compleja y este proyecto no lo necesita.

### Patrón Repositorio

Ningún componente usa `localStorage` directamente. Todo el acceso pasa por un único módulo que cumple esta interfaz:

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

Beneficios: **separación de responsabilidades** (los componentes muestran, el repositorio guarda), un solo lugar donde buscar errores de guardado y la posibilidad de cambiar de almacenamiento sin tocar el resto de la app. La interfaz es asíncrona aunque localStorage no lo sea, para que un cambio futuro no obligue a reescribir los componentes.

### Limitaciones conocidas de localStorage

- Capacidad de unos 5 MB por origen: alcanza para miles de contactos de texto, pero no para fotos (fuera del MVP).
- El navegador puede borrar los datos si el usuario limpia el almacenamiento del sitio. Por eso la **exportación JSON** es parte del MVP: es el respaldo.

## Versionado

- **SemVer** (`MAYOR.MENOR.PARCHE`); cada versión se marca con un tag de Git.
- **v1.0.0**: MVP completo (desarrollo, testing, deploy y documentación).
- Un cambio grande por versión.

## Principios que respeta el stack

Sin backend, sin cuentas, sin publicidad, sin métricas y sin servicios de terceros en tiempo de ejecución. Los datos nunca salen del dispositivo.

## Descartado

- **App nativa (Android/iOS):** dos códigos y publicación en tiendas; excesivo para el MVP.
- **Electron:** más de 100 MB para una agenda y no funciona en celular.
- **Framework con servidor (Next.js, etc.):** no hay backend que lo justifique.
- **daisyUI:** temas por defecto coloridos que habría que neutralizar; shadcn/ui encaja mejor con el diseño y con React.
- **Versión previa en JavaScript puro:** se priorizó llegar directo al stack del mercado actual.

## Estado

Tecnología definida el 27/10/2026. Revisada el mismo día: stack final con TypeScript, React, Tailwind CSS y shadcn/ui desde la v1.
