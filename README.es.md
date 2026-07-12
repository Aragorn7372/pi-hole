# Pi-hole con Docker

Una configuración con [Docker Compose](https://docs.docker.com/compose/) para ejecutar [Pi-hole](https://pi-hole.net/) — un bloqueador de anuncios y sumidero DNS a nivel de red que bloquea anuncios, rastreadores y dominios maliciosos a nivel DNS para **todos los dispositivos de tu red**.

## Requisitos previos

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/) (normalmente incluido con Docker Desktop)

## Primeros pasos

### 1. Configurar las variables de entorno

Copia el archivo de ejemplo:

```bash
cp .env.example .env
```

Edita `.env` y establece los valores necesarios:

| Variable | Descripción | Ejemplo |
|---|---|---|
| `TZ` | Zona horaria | `Europe/Madrid` |
| `DATA_ROOT` | Ruta local para datos persistentes | `C:\docker` (Windows) o `/srv/docker` (Linux) |
| `PIHOLE_PASSWORD` | Contraseña de la interfaz web | `tu_contraseña_segura` |

> **Seguridad:** El archivo `.env` está en `.gitignore` para que no se suba al repositorio.

### 2. Iniciar Pi-hole

```bash
docker compose up -d
```

### 3. Acceder a la interfaz web

Abre tu navegador y ve a:

```
http://<ip-de-tu-equipo>:8081/admin
```

Inicia sesión con la contraseña que configuraste en `.env`.

## Uso

| Comando | Descripción |
|---|---|
| `docker compose up -d` | Iniciar Pi-hole en segundo plano |
| `docker compose down` | Detener y eliminar el contenedor |
| `docker compose logs -f` | Ver los logs en tiempo real |
| `docker compose restart` | Reiniciar el contenedor |

## Apuntar los dispositivos a Pi-hole

Para que Pi-hole bloquee anuncios en tu red, los dispositivos deben usarlo como servidor DNS.

- **A nivel de router (recomendado):** Configura el DNS del router con la IP del equipo donde corre Pi-hole. Esto cubre todos los dispositivos automáticamente.
- **Por dispositivo:** Cambia manualmente el servidor DNS en la configuración de red de cada dispositivo a la IP del equipo con Pi-hole.

## Estructura del proyecto

```
├── .env               # Variables de entorno locales (ignorado por git)
├── .env.example       # Plantilla para .env
├── .gitignore
├── docker-compose.yml # Definición del servicio
├── README.md          # Documentación en inglés
├── README.es.md       # Esta documentación en español
└── etc-pihole/        # Datos persistentes (se crea al ejecutar)
```

## Puertos

| Puerto | Protocolo | Propósito |
|---|---|---|
| `53` | TCP/UDP | Consultas DNS |
| `8081` | TCP | Panel de administración web |

> El puerto `53` requiere permisos elevados en la mayoría de sistemas. Docker lo gestiona automáticamente.

## Persistencia de datos

Los datos de Pi-hole se almacenan en `${DATA_ROOT}/pihole/` con dos subdirectorios:
- `etc-pihole/` → `/etc/pihole` en el contenedor (configuración y bases de datos)
- `etc-dnsmasq.d/` → `/etc/dnsmasq.d` en el contenedor (configuración de dnsmasq)

## Actualizar

```bash
docker compose pull && docker compose up -d
```

## Recursos

- [Sitio oficial de Pi-hole](https://pi-hole.net/)
- [Documentación de Pi-hole](https://docs.pi-hole.net/)
- [Imagen Docker de Pi-hole](https://hub.docker.com/r/pihole/pihole)
- [Guía oficial de instalación con Docker](https://github.com/pi-hole/docker-pi-hole)
