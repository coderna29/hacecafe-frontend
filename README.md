# HaceCafe · Frontend

App de gestión de cafetería hecha con React 19 + Vite. Tiene cuatro pantallas según quién la use:

| Ruta | Quién | Para qué |
| --- | --- | --- |
| `/menu?mesa=N` | Cliente (sin login) | Ver el menú, pedir desde la mesa y seguir el pedido en tiempo real |
| `/empleado` | Empleado / Dueño | Tomar pedidos, aprobar los del QR, cobrar, ver tickets y stock |
| `/cocina` | Cocina / Empleado / Dueño | Ver y avanzar pedidos, controlar stock |
| `/admin` | Dueño | Dashboard, productos, stock, usuarios, caja, categorías, QR de mesas, apariencia y configuración |

Al iniciar sesión cada rol va directo a su pantalla.

El menú del cliente toma sus colores, nombre, logo, portada y tipografía de *Apariencia* (panel del dueño); si no hay nada guardado usa el diseño por defecto.

## Cómo correrlo

```bash
npm install
cp .env.example .env.local     # VITE_API_URL apunta al backend
npm run dev
```

Necesita el backend corriendo (`hacecafe-backend`). Mientras estés desarrollando es `http://localhost:3001`.

## Variables de entorno

| Variable | Descripción |
| --- | --- |
| `VITE_API_URL` | URL del backend. En Vercel hay que definirla, si no apunta a `localhost` |
| `VITE_SOCKET_URL` | Solo si el socket está en otra URL (por defecto usa `VITE_API_URL`) |
| `VITE_CLOUDINARY_CLOUD_NAME`, `VITE_CLOUDINARY_PRESET` | Subida de imágenes |

## Estructura

```
src/
  pages/        Una pantalla por archivo (se cargan solo al entrar a ellas)
  components/   Ticket, pantalla de pago, subida de imágenes, avisos
  context/      Sesión y tema
  hooks/        useSocket: conexión en tiempo real con reconexión
  services/     api (axios), socket, config (URLs), configLocal (datos del local)
  utils/        roles, fechas, avisos, apariencia del menú
```

## Cosas que conviene saber

- **Datos del local, cobro y apariencia** se guardan en el servidor (Config. y Apariencia del panel del dueño). Antes estaban en el navegador de quien los cargaba y no llegaban a los demás dispositivos.
- **Sesión**: si el token vence o la cuenta se desactiva, se cierra la sesión y se vuelve al login.
- **Avisos**: se muestran con `notificar("mensaje")` (en `utils/notificar.js`) en vez de `alert()`.
- **Roles**: el frontend usa `DUENO` (sin tilde), igual que el backend. `utils/roles.js` tiene el resto de ayudas.

## Despliegue

El frontend va en Vercel (`vercel.json` ya redirige todas las rutas a la app). El backend necesita un servidor persistente para los websockets (Render, Railway, Fly…).
