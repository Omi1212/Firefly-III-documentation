# Guía de Instalación: Firefly III

Esta guía proporciona instrucciones detalladas para la instalación y configuración de Firefly III en un entorno de desarrollo local. Se enfoca en el uso de Docker para la gestión de la base de datos PostgreSQL, simplificando la configuración inicial.

## 1. Prerrequisitos

Antes de comenzar, asegúrese de que su sistema cumple con los siguientes requisitos:

| Tecnología | Versión Mínima | Enlace de Descarga |
|---|---|---|
| **PHP** | 8.4.0+ | [php.net](https://www.php.net/downloads.php) |
| **Composer** | - | [getcomposer.org](https://getcomposer.org/download/) |
| **Docker & Docker Compose** | - | [docs.docker.com](https://docs.docker.com/get-docker/) |
| **Node.js & npm** | - | [nodejs.org](https://nodejs.org/) |

## 2. Clonar el Repositorio

Obtenga la última versión del código fuente de Firefly III desde el repositorio:

```bash
git clone [https://github.com/Diego-Lara-UCA/firefly-iii](https://github.com/Diego-Lara-UCA/firefly-iii)
cd firefly-iii
```

## 3. Configuración del Entorno

La configuración de la aplicación se gestiona mediante archivos de entorno.

### 3.1. Archivo de Entorno Principal (`.env`)

Este archivo define parámetros cruciales como la conexión a la base de datos.

1.  **Crear el archivo `.env`** a partir de la plantilla:
    ```bash
    cp .env.example .env
    ```

2.  **Ajustar las variables** para la conexión a la base de datos PostgreSQL. Abra `.env` y asegúrese de que estas líneas estén configuradas así:
    ```dotenv
    DB_CONNECTION=pgsql
    DB_HOST=127.0.0.1
    DB_PORT=5432
    DB_DATABASE=firefly
    DB_USERNAME=firefly
    DB_PASSWORD=secret_firefly_password
    ```
    !!! tip "Contraseña Segura"
        Es muy recomendable cambiar `secret_firefly_password` por una contraseña segura y única.

### 3.2. Archivo de Entorno para la Base de Datos (`.db.env`)

Este archivo configura las credenciales del contenedor de la base de datos.

1.  **Cree el archivo `.db.env`** en la raíz del proyecto.
2.  **Añada el siguiente contenido**, asegurándose de que coincida con lo que configuró en el archivo `.env`:
    ```dotenv
    POSTGRES_USER=firefly
    POSTGRES_PASSWORD=secret_firefly_password
    POSTGRES_DATABASE=firefly
    ```

### 3.3. Archivo de Orquestación (`docker-compose.yml`)

Este archivo define el servicio de la base de datos PostgreSQL.

1.  **Cree el archivo `docker-compose.yml`** en la raíz del proyecto.
2.  **Añada la definición del servicio**:
    ```yaml
    version: "3.8"

    services:
        db:
            image: postgres:15
            container_name: firefly_iii_postgres_db_local
            restart: unless-stopped
            env_file:
                - .db.env
            ports:
                - "5432:5432"
            volumes:
                - pgdata_local:/var/lib/postgresql/data
            networks:
                - firefly_local_net

    volumes:
        pgdata_local:

    networks:
        firefly_local_net:
            driver: bridge
    ```

## 4. Iniciar la Base de Datos

Con los archivos de configuración listos, inicie el contenedor de la base de datos:

```bash
docker-compose up -d --build
```
Para verificar que el contenedor está en ejecución, use `docker ps`.

## 5. Instalar Dependencias y Compilar Activos

A continuación, instale las dependencias de PHP y compile los activos del frontend.

=== "Backend (PHP)"
    ```bash
    # Instalar dependencias de PHP
    composer update --no-dev --no-scripts --no-plugins
    ```

=== "Frontend (JS/CSS)"
    ```bash
    # Instalar dependencias de Node.js
    npm install

    # Compilar activos para v1 y v2
    npm run prod --workspace=v1
    npm run build --workspace=v2
    ```

## 6. Ejecutar la Aplicación

Finalmente, prepare la base de datos y lance el servidor de desarrollo.

1.  **Ejecutar las migraciones** para crear las tablas en la base de datos:
    ```bash
    php artisan migrate
    ```

2.  **Iniciar el servidor** de desarrollo de Laravel:
    ```bash
    php artisan serve
    ```

¡Listo! La aplicación estará disponible en `http://127.0.0.1:8000`.
