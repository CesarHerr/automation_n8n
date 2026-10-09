# automation_n8n

Proyecto para desplegar un entorno de n8n con PostgreSQL usando Docker Compose.

## Descripción

Este repositorio levanta una instancia de n8n lista para automatizar workflows, junto con una base de datos PostgreSQL para persistir datos, credenciales y ejecuciones.

Incluye:
- n8n en Docker
- PostgreSQL 17
- almacenamiento persistente para datos de n8n y PostgreSQL
- configuración de zona horaria y clave de encriptación
- acceso local en `http://localhost:5678`

## Requisitos

- Docker
- Docker Compose v2
- Un puerto disponible en el sistema: `5678`

## Estructura del proyecto

- `docker-compose.yml`: definición de servicios de n8n y PostgreSQL
- `.env`: variables de entorno locales (no versionado)
- `.env.example`: plantilla de configuración
- `workflows/`: carpeta para workflows y artefactos asociados
- `backups/`: carpeta para respaldos manuales o exportaciones

## Configuración inicial

1. Copia la plantilla de entorno:

```bash
cp .env.example .env
```

2. Edita el archivo `.env` y reemplaza los valores por tus propios secretos:

```env
POSTGRES_USER=n8n
POSTGRES_PASSWORD=TU_PASSWORD_SEGURA
POSTGRES_DB=n8n
N8N_ENCRYPTION_KEY=TU_CLAVE_DE_ENCRIPTACION
TZ=America/Santiago
```

Importante:
- `POSTGRES_PASSWORD` debe tener una contraseña segura.
- `N8N_ENCRYPTION_KEY` debe ser una cadena larga y aleatoria.
- `TZ` se usa para sincronizar la zona horaria del contenedor.

## Levantar el entorno

```bash
docker compose up -d
```

Esto creará y levantará los contenedores de PostgreSQL y n8n.

## Verificar que esté funcionando

```bash
docker compose ps
```

Luego abre en el navegador:

```text
http://localhost:5678
```

## Logs

Para ver los logs del servicio de n8n:

```bash
docker compose logs -f n8n
```

Para ver los logs de PostgreSQL:

```bash
docker compose logs -f postgres
```

## Comandos útiles

Detener los contenedores:

```bash
docker compose down
```

Reiniciar los servicios:

```bash
docker compose restart
```

Actualizar la imagen de n8n:

```bash
docker compose pull
docker compose up -d
```

## Volúmenes persistentes

La configuración usa volúmenes Docker para conservar los datos entre reinicios:
- `postgres_data`: base de datos PostgreSQL
- `n8n_data`: archivos y configuración de n8n

## Seguridad

- El servicio de n8n queda publicado solo en `127.0.0.1:5678` por defecto.
- No compartas valores de `.env` ni claves sensibles en repositorios públicos.
- Si vas a exponer n8n a la red, revisa el uso de HTTPS, autenticación y firewall.

## Notas

Este proyecto está pensado para uso local o entorno controlado. Si necesitas exponerlo fuera del equipo, conviene revisar la configuración de reverse proxy y autenticación antes de hacerlo.
