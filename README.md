# Citizen Back — API de Alertas y Lugares de Riesgo

Backend (API REST + WebSockets) para una aplicación de seguridad ciudadana. Permite a los **ciudadanos** reportar alertas (SOS) desde su ubicación en tiempo real, y a los **serenos** (personal de seguridad) recibir y atender esas alertas al instante mediante Socket.IO. Un rol de **administrador** gestiona ciudadanos, serenos, lugares de riesgo, tipos de alerta e información de utilidad.

## Stack

- Node.js + TypeScript + Express 5
- PostgreSQL + Prisma ORM
- Socket.IO (notificación de alertas en tiempo real)
- JWT (autenticación) + bcryptjs (hash de contraseñas)
- express-validator (validación de datos)
- Docker Compose (base de datos local)

## Módulos principales

- **Autenticación**: login y registro de ciudadanos, serenos y héroes/central (JWT, renovación de token).
- **Alertas**: los ciudadanos registran alertas con fecha y coordenadas (longitud/latitud); los serenos las listan y actualizan su estado; se notifican en vivo por WebSocket.
- **Lugares de riesgo**: CRUD de zonas peligrosas con dirección, barrio, nivel de riesgo y coordenadas.
- **Tipos**: catálogo de tipos de alerta con su número asociado.
- **Información**: publicaciones de utilidad (dato, link, fecha) para los ciudadanos.
- **Administración**: gestión de ciudadanos y serenos (listar/crear/eliminar), y consulta de alertas por usuario.

> Nota: las rutas de la API usan alias en japonés (p. ej. `/shimin`, `/basho`, `/odayakana`) como capa de ofuscación de los endpoints reales.

## Roles

| Rol      | Descripción                                              |
|----------|-----------------------------------------------------------|
| Citizen  | Ciudadano que reporta alertas desde su ubicación           |
| Serene   | Sereno/seguridad que atiende y actualiza las alertas       |
| Central / Hero | Administración central del sistema                   |

El acceso a endpoints protegidos se valida con JWT (`validarJWT`) y, en operaciones administrativas, con el rol requerido (`validarRol`, comparado contra la variable `ROLE1`).

## Puesta en marcha

1. Instalar dependencias: `npm install`
2. Crear un archivo `.env` con las variables necesarias (ver abajo)
3. Levantar la base de datos: `docker compose up -d`
4. Correr la migración de Prisma: `npx prisma migrate dev`
5. Levantar el proyecto: `npm run dev`
6. Aplicar cambios de esquema sin migración: `npx prisma db push`

### Variables de entorno

```
PORT=3000
DATABASE_URL=postgresql://usuario:password@localhost:5432/basededatos
DB_USER=usuario
DB_PASSWORD=password
DB_NAME=basededatos
SECRET_JWT_SEED=una_clave_secreta
ROLE1=admin
```

## Scripts

- `npm run dev` — genera el cliente Prisma y levanta el servidor en modo desarrollo (hot reload)
- `npm run build` — genera el cliente Prisma y compila TypeScript
- `npm start` — ejecuta la build compilada (`dist/index.js`)
