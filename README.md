# Radar Solidario

Sistema web que muestra en un mapa interactivo de Mendoza puntos de ayuda comunitaria: ollas populares, refugios y lugares para donar ropa. El objetivo es que cualquier persona pueda encontrar estos puntos sin necesidad de registrarse, y que la comunidad pueda sugerir nuevos puntos para que un administrador los verifique.

## Motivación

No existe actualmente una herramienta centralizada y de fácil acceso para ubicar este tipo de recursos en Mendoza. El proyecto está pensado con foco en accesibilidad: el mapa es de uso público y no requiere cuenta de usuario, ya que muchas personas en situación de vulnerabilidad no cuentan con correo electrónico u otras credenciales digitales.

## Funcionalidades

- Mapa público con los puntos verificados (sin necesidad de login)
- Formulario público para sugerir un nuevo punto (queda en estado pendiente)
- Panel de administración con login para revisar, aprobar o rechazar puntos sugeridos
- Categorías de puntos: ollas populares, refugios, donación de ropa

## Stack tecnológico

- **Frontend:** [Astro](https://astro.build/) + isla de [React](https://react.dev/)
- **Mapa:** [Leaflet](https://leafletjs.com/) + OpenStreetMap
- **Backend:** Node.js
- **Base de datos:** MongoDB (índice geoespacial `2dsphere`)
- **Autenticación:** JWT (solo para rutas de administración)

## Modelo de datos

**Colección `puntos`**

| Campo | Tipo | Descripción |
|---|---|---|
| nombre | String | Nombre del punto |
| tipo | String | `olla` \| `refugio` \| `donacion_ropa` |
| ubicacion | GeoJSON Point | Coordenadas `[lng, lat]` |
| descripcion | String | Detalle del punto |
| contacto | String | Opcional |
| estado | String | `pendiente` \| `verificado` \| `rechazado` |
| creadoEn | Date | Fecha de creación |

**Colección `admins`**

| Campo | Tipo |
|---|---|
| usuario | String |
| passwordHash | String |

## Endpoints principales

| Método | Ruta | Acceso |
|---|---|---|
| GET | `/api/puntos` | Público — solo puntos verificados |
| POST | `/api/puntos` | Público — crea un punto en estado pendiente |
| GET | `/api/admin/puntos` | Admin — lista puntos pendientes |
| PUT | `/api/puntos/:id` | Admin — editar / aprobar / rechazar |
| DELETE | `/api/puntos/:id` | Admin |
| POST | `/api/auth/login` | Público — login de administrador |

## Estructura del proyecto

```
/src
  /pages
    index.astro          → mapa público
    sugerir.astro        → formulario de sugerencia
    admin/
      login.astro
      panel.astro         → puntos pendientes (protegida)
  /components
    Mapa.jsx              → isla React (Leaflet)
    FormularioSugerencia.jsx
  /api
    puntos.js
    puntos/[id].js
    auth/login.js
```

## Instalación

```bash
git clone <url-del-repo>
cd radar-solidario
npm install
```

Crear un archivo `.env` con:

```
MONGODB_URI=<tu_conexion_mongo>
JWT_SECRET=<tu_secreto>
```

Levantar el proyecto en modo desarrollo:

```bash
npm run dev
```

## Roadmap

- [ ] v1: mapa público, sugerencia de puntos, login admin, aprobación/rechazo
- [ ] v2: notificaciones, clustering de marcadores, filtros por tipo/horario
- [ ] v3: registro opcional de organizaciones para mantener sus propios puntos

## Estado del proyecto

En desarrollo — proyecto de portafolio personal.
