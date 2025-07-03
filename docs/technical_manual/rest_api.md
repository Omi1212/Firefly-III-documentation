# API REST de Firefly III

La Interfaz de Programación de Aplicaciones (API) v1 de Firefly III es una implementación **RESTful** que opera sobre el protocolo HTTP y utiliza el formato **JSON** para el intercambio de datos. Esta API ha sido diseñada para proporcionar acceso programático exhaustivo a la funcionalidad y los datos gestionados por la plataforma de finanzas personales Firefly III.

## Estructura de la API

El diseño de la API se adhiere a los principios REST, utilizando los verbos HTTP estándar para definir las operaciones sobre los recursos:

-   **`GET`**: Para recuperar datos (ej. obtener una lista de transacciones).
-   **`POST`**: Para crear nuevos recursos (ej. registrar una nueva cuenta).
-   **`PUT` / `PATCH`**: Para actualizar recursos existentes (ej. modificar un presupuesto).
-   **`DELETE`**: Para eliminar recursos (ej. borrar una factura).

Permite la creación, recuperación, actualización y eliminación (operaciones **CRUD**) de entidades fundamentales como transacciones, cuentas (de activo, pasivo, ingresos, gastos), presupuestos, facturas y categorías. Facilita la automatización de tareas recurrentes, como la importación de extractos bancarios o la generación de informes financieros. Ofrece los mecanismos necesarios para integrar Firefly III con otras aplicaciones, servicios de terceros o flujos de trabajo personalizados. Proporciona endpoints para la obtención de datos agregados, resúmenes financieros, información sobre saldos y análisis de tendencias.

---

## Modificación: Módulo de Exportación de Reportes

La funcionalidad para exportar datos en formatos **`.xlsx`** y **`.pdf`** se implementó de manera centralizada en un módulo de exportación dedicado, facilitando su mantenimiento y futuras extensiones.

### Diseño y Buenas Prácticas

Durante su desarrollo, se aplicaron buenas prácticas, incluyendo una arquitectura modular con separación de responsabilidades para mayor claridad y facilidad de prueba. Se utilizó el manejo de errores de Firefly III y se utilizaron librerías externas estables y probadas para la generación de los archivos.

El código se mantuvo limpio, legible y bien documentado, complementado con pruebas unitarias y de integración, además de buscar la configurabilidad de la exportación.

### Convenciones de Nomenclatura

En cuanto a la nomenclatura de variables, se adoptó una convención consistente y descriptiva, utilizando principalmente `camelCase` para variables y funciones, `PascalCase` para clases y métodos, nombres auto-documentados que evitan abreviaturas ambiguas, prefijos/sufijos para tipos comunes, y constantes en `SNAKE_CASE` en mayúsculas, mientras que las funciones se nombraron con verbos de acción. Estas prácticas y convenciones aseguran la calidad, mantenibilidad y evolución del software.

---

## Rutas de las modificaciones realizadas

A continuación se detallan los archivos que fueron creados o modificados en la base de código de la API para implementar esta nueva funcionalidad.

### Request

Gestionan la validación de los datos de entrada para cada tipo de reporte solicitado a través de la API.

```php
app/Api/V1/Requests/Data/Export/TagReportRequest.php
app/Api/V1/Requests/Data/Export/DefaultReportExportRequest.php
app/Api/V1/Requests/Data/Export/TransactionHistoryExportRequest.php
app/Api/V1/Requests/Data/Export/BudgetExportRequest.php
app/Api/V1/Requests/Data/Export/CategoryReportRequest.php
app/Api/V1/Requests/Data/Export/ExpenseRevenueReportRequest.php
```

### Controllers
Contienen la lógica de negocio para procesar las solicitudes, generar el archivo en el formato deseado (.pdf o .xlsx) y devolverlo al cliente.
```php
app/Api/V1/Controllers/Data/Export/PDF/ReportExportController.php
app/Api/V1/Controllers/Data/Export/XLS/ReportExportController.php
```
