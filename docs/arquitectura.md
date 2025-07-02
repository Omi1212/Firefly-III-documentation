# Arquitectura del Sistema (V1)

Firefly III V1 está construido sobre el **framework Laravel** y sigue el patrón arquitectónico **MVC (Modelo-Vista-Controlador)**. Esta estructura organiza el código en capas lógicas, facilitando su mantenimiento y escalabilidad.

## Capas Principales

La arquitectura se puede desglosar en las siguientes capas:

### 1. Capa de Presentación (Frontend)

Responsable de la interfaz de usuario.

* **Vistas**: Plantillas **Blade** de Laravel (`resources/views/`) que renderizan el HTML.
* **Interactividad**: Mejorada con **jQuery** y componentes **Vue.js** (`resources/assets/v1/js/`).
* **Estilos**: Gestionados con SASS (`resources/assets/v1/sass/`).
* **Compilación**: Los activos (CSS, JS) se compilan usando **Laravel Mix**.

### 2. Capa de Aplicación (Backend)

Contiene la lógica central de la aplicación.

* **Rutas (`routes/`)**: Definen los endpoints para la web (`web.php`) y la API (`api.php`).
* **Controladores (`app/Http/Controllers/`)**: Procesan las solicitudes HTTP, interactúan con la lógica de negocio y devuelven una respuesta (una vista o JSON).
* **Middleware (`app/Http/Middleware/`)**: Filtra las solicitudes para tareas como autenticación y autorización.
* **Servicios (`app/Services/`)**: Encapsulan lógica de negocio compleja y reutilizable para mantener los controladores delgados.

### 3. Capa de Dominio y Datos

Define las entidades del negocio y cómo interactúan con la base de datos.

* **Modelos (`app/Models/`)**: Representan las entidades (Cuenta, Transacción, Presupuesto) usando el ORM **Eloquent** de Laravel. Definen las relaciones entre ellas.
* **Repositorios (`app/Repositories/`)**: Abstraen la lógica de acceso a datos, proporcionando una interfaz clara para realizar consultas (CRUD) sobre los modelos.
* **Migraciones (`database/migrations/`)**: Gestionan el versionado del esquema de la base de datos.

### 4. Capa de Infraestructura

Componentes de soporte y tareas en segundo plano.

* **Consola Artisan (`app/Console/Commands/`)**: Comandos para tareas de mantenimiento y cronjobs.
* **Jobs (`app/Jobs/`)**: Tareas asíncronas que se pueden ejecutar en segundo plano (ej. envío de correos).
* **Eventos y Listeners (`app/Events/`, `app/Handlers/`)**: Implementan el patrón Observador para desacoplar componentes del sistema.

## API Principal (RESTful V1)

La API V1 de Firefly III es una implementación **RESTful** que utiliza **JSON** para el intercambio de datos y permite un acceso programático completo a la aplicación.

* **Ubicación**: El código reside en `app/Api/V1/`.
* **Autenticación**: Se gestiona mediante **OAuth2** (para aplicaciones de terceros) y **Tokens de Acceso Personal (PATs)** (para scripts).
* **Funcionalidades Clave**:
  * Operaciones CRUD completas sobre todas las entidades.
  * Endpoints para automatización, como importación y generación de informes.
  * Endpoints de datos agregados para resúmenes y gráficos.

???+ info "Estándares de Desarrollo"
El proyecto sigue estrictas convenciones de nomenclatura (`camelCase` para métodos/variables, `PascalCase` para clases) y una organización de código modular para garantizar la calidad y mantenibilidad. El módulo de exportación es un claro ejemplo de la aplicación de estos estándares.