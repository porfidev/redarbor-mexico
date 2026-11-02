# Redarbor México

Aplicación de búsqueda de vacantes con filtros, detalle con swipe, postulaciones, favoritos y notificaciones locales. El frontend utiliza Expo y React Native; el backend expone una API con Hono y TypeScript.

Este repositorio reúne la documentación y las versiones del backend y frontend mediante submódulos de Git.

## Estructura

```text
redarbor-mexico/
├── README.md
├── .gitmodules
├── .env.example
├── docs/       # Documentación técnica y diagramas
├── backend/    # Submódulo de la API
└── frontend/   # Submódulo de la aplicación
```

| Proyecto | Guía de instalación y comandos            | Repositorio                                                        |
| -------- | ----------------------------------------- | ------------------------------------------------------------------ |
| Backend  | [README del backend](backend/README.md)   | [redarbor-backend](https://github.com/porfidev/redarbor-backend)   |
| Frontend | [README del frontend](frontend/README.md) | [redarbor-frontend](https://github.com/porfidev/redarbor-frontend) |

## Requerimientos

- Node.js 24.3 o superior dentro de la rama 24 y npm.
- Git y acceso a los repositorios de los submódulos.
- OpenSSL para generar el secreto JWT del backend.
- Navegador para ejecutar la versión web.
- Para compilaciones nativas locales: Android Studio y Android SDK, o macOS con Xcode para iOS.

## Clonar el proyecto

Sustituye `URL_DEL_REPOSITORIO_PRINCIPAL` por la URL de este repositorio:

```sh
git clone --recurse-submodules URL_DEL_REPOSITORIO_PRINCIPAL redarbor-mexico
cd redarbor-mexico
```

Si ya lo clonaste sin descargar los submódulos:

```sh
git submodule update --init --recursive
```

Los submódulos están configurados con URLs SSH de GitHub; necesitas tener una clave SSH asociada a tu cuenta y permisos de acceso.

## Ejecución local

El backend y frontend se instalan y ejecutan por separado. No hay un comando de arranque conjunto en la raíz.

### 1. Preparar el backend

Desde la raíz del proyecto:

```sh
cd backend
npm ci
npm run lint:install
cp .env.example .env
openssl rand -hex 32
```

Copia el secreto generado en `backend/.env` y configura:

```dotenv
PORT=3000
JWT_SECRET=<secreto_generado>
JWT_EXPIRES_IN_SECONDS=3600
CORS_ORIGINS=http://localhost:8081
```

Inicia la API y deja esta terminal abierta:

```sh
npm run dev
```

Por defecto, estará disponible en `http://localhost:3000`. Consulta otras opciones de ejecución en el [README del backend](backend/README.md).

### 2. Preparar el frontend

Abre otra terminal en la raíz del proyecto:

```sh
cd frontend
npm ci
cp .env.example .env
```

Para ejecutar web en la misma computadora, configura `frontend/.env`:

```dotenv
API_BASE_URL=http://localhost:3000
```

Luego inicia la aplicación:

```sh
npm run web
```

Abre la dirección que indique Expo. Si el origen web cambia de puerto, actualiza `CORS_ORIGINS` en el backend.

Para otras plataformas, ajusta `API_BASE_URL`:

| Entorno                                     | URL del backend                            |
| ------------------------------------------- | ------------------------------------------ |
| Web o simulador iOS en la misma computadora | `http://localhost:3000`                    |
| Emulador Android estándar                   | `http://10.0.2.2:3000`                     |
| Dispositivo físico                          | `http://<IP_LOCAL_DE_TU_COMPUTADORA>:3000` |

En dispositivos físicos, el backend debe ser accesible desde su red. Reinicia los servidores después de cambiar sus archivos `.env`.

El archivo [.env.example](.env.example) de la raíz sirve como referencia; cada aplicación lee su propio `.env`.

### 3. Ejecución nativa y notificaciones

Desde `frontend`, compila e instala la plataforma elegida:

```sh
npm run android
```

O, en macOS con Xcode:

```sh
npm run ios
```

Para iniciar posteriormente el servidor del cliente de desarrollo:

```sh
npm start -- --dev-client
```

Las notificaciones de Notifee requieren una compilación nativa propia y no están disponibles en Expo Go. La pantalla de notificaciones permite probar avisos locales y enlaces a vacantes; no hay un servicio de push remoto implementado. Consulta la [guía del frontend](frontend/README.md) para compilación y verificaciones por plataforma.

## Probar la aplicación

- Usuario de ejemplo: `test@occ.com.mx`.
- Contraseña: `Test1234`.
- La búsqueda y el detalle son públicos; postulaciones y favoritos requieren sesión.
- El backend almacena los datos en memoria: los cambios se pierden al reiniciarlo.
- La API se puede verificar con la [colección de Postman](backend/postman/redarbor.postman_collection.json) y el [entorno local](backend/postman/local.postman_environment.json), siguiendo el [README del backend](backend/README.md#colección-de-postman).

## Comprobaciones de calidad

Desde `backend`:

```sh
npm run build
npm run lint
npm run format:check
```

Desde `frontend`:

```sh
npm run check
```

El frontend conserva la configuración de Jest, pero actualmente no contiene archivos de tests en `src/`. Estas comprobaciones no sustituyen las pruebas manuales de navegación, gestos y notificaciones en dispositivos.

## Documentación técnica

Los documentos están en [docs/](docs/) y sus imágenes y diagramas en [docs/files/](docs/files/).

| Documento                                                                                     | Contenido                                                   |
| --------------------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| [A1 · Arquitectura](docs/A1%20-%20Arquitectura.md)                                            | Organización, capas y decisiones de arquitectura.           |
| [A2 · Estrategia de estado y datos](docs/A2%20-%20%20Estrategia%20de%20estado%20y%20datos.md) | Sesión JWT, caché, acciones optimistas y prefetch.          |
| [A3 · Navegación y Deep Linking](docs/A3%20-%20Navegaci%C3%B3n%20y%20DeepLinking.md)          | Rutas, sesión, notificaciones y sincronización del detalle. |
| [A4 · Estrategia de calidad](docs/A4%20-%20Estrategia%20de%20calidad.md)                      | Verificaciones y criterios de calidad del proyecto.         |
| [A5 · Performance](docs/A5%20-%20Performance.md)                                              | Métricas, optimización de listas y precarga de datos.       |

## Actualizar los submódulos

Para descargar las versiones registradas por el repositorio principal, desde la raíz y con los cambios locales guardados:

```sh
git pull
git submodule update --init --recursive
```

Cada submódulo apunta a un commit concreto. Al desarrollar, trabaja en una rama del submódulo, publica primero sus commits y después guarda en el repositorio principal la nueva referencia correspondiente.
