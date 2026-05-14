# Masterweb CC — Workspace de Desarrollo Web

## Visión General
Workspace para crear sitios web profesionales: landing pages, blogs, e-commerce, y web apps.
Cada proyecto vive en su propia subcarpeta con su propio `package.json` y repositorio git.

## Stack Tecnológico Estándar
- **Framework:** Next.js 14+ con App Router
- **Estilos:** Tailwind CSS v3/v4
- **Lenguaje:** TypeScript (estricto — nunca usar `any`)
- **Deploy:** Vercel (MCP conectado)
- **Diseño:** Figma → código via MCP (cuenta: Camilo G, solo lectura)

## MCPs Conectados
| MCP | Uso |
|-----|-----|
| **Figma** | Leer diseños y convertirlos a código. Pasar URL de Figma para que Claude la procese |
| **Vercel** | Deploy, env vars, dominios y proyectos directamente desde Claude |

## Estructura del Workspace
```
/Proyecto Masterweb CC/
  ├── CLAUDE.md              ← configuración global del workspace
  ├── _shared/               ← componentes y utilidades reutilizables entre proyectos
  │   ├── components/        ← componentes React compartidos
  │   ├── hooks/             ← hooks reutilizables
  │   └── lib/               ← utilidades y helpers
  ├── [nombre-proyecto-1]/   ← Next.js app independiente
  ├── [nombre-proyecto-2]/   ← Next.js app independiente
  └── ...
```

## Crear un Nuevo Proyecto
```bash
# Desde la raíz del workspace:
npx create-next-app@latest [nombre-proyecto] \
  --typescript --tailwind --eslint --app --src-dir --import-alias "@/*"

cd [nombre-proyecto]
npm run dev   # http://localhost:3000
```

## Librerías de Uso Frecuente
```bash
# Componentes UI base (instalar por proyecto)
npx shadcn@latest init

# Iconos
npm install lucide-react

# Animaciones
npm install framer-motion

# Formularios
npm install react-hook-form zod @hookform/resolvers

# Fetching (para web apps)
npm install @tanstack/react-query

# E-commerce (pagos)
npm install stripe @stripe/stripe-js
```

## Flujo de Trabajo con Figma
1. Abrir Figma y copiar la URL del frame o componente a implementar
2. Pegar la URL en el chat — Claude usa el MCP para leer el diseño directamente
3. Claude genera el componente adaptado al stack del proyecto
4. Revisar, ajustar y limpiar el código generado

> **Nota:** El MCP de Figma devuelve código React+Tailwind como referencia. Siempre adaptar
> a la estructura real del proyecto (rutas, componentes existentes, design tokens).

## Flujo de Deploy con Vercel
1. Crear repositorio en GitHub para el proyecto
2. Conectar en Vercel via MCP o dashboard
3. Cada `git push` a `main` despliega automáticamente
4. Variables de entorno se gestionan en Vercel (nunca en `.env` commiteado)

## Convenciones de Código
- Componentes: PascalCase en `src/components/` (ej: `HeroSection.tsx`)
- Páginas: en `src/app/` siguiendo el App Router de Next.js
- Hooks: `use` prefix, camelCase (ej: `useCart.ts`)
- Preferir **Server Components** por defecto; usar `"use client"` solo cuando hay interactividad
- Imágenes: siempre `next/image` para optimización automática
- Links: siempre `next/link` para navegación SPA

## Skills de Claude Disponibles
| Skill | Cuándo usar |
|-------|-------------|
| `/review` | Antes de cada deploy para revisar el código |
| `/security-review` | En proyectos con auth, pagos o datos de usuarios |
| `/simplify` | Cuando el código generado quedó muy verboso |
| `/init` | Para crear un CLAUDE.md dentro de un subproyecto |

## Instrucciones para Claude
- Cuando recibas una URL de Figma, usar el MCP de Figma antes de escribir cualquier código
- Para proyectos nuevos, preguntar siempre si hay diseño en Figma disponible
- No crear comentarios obvios en el código; solo comentar lógica no evidente
- Para e-commerce, asumir Stripe como procesador de pagos por defecto
- Para auth, asumir NextAuth.js (Auth.js) por defecto
- Los componentes de shadcn/ui son preferidos sobre componentes custom cuando existe equivalente
- Siempre generar TypeScript con tipos explícitos, sin `any`
