# Problemas y Preguntas Comunes

Esta sección aborda algunos de los problemas más frecuentes que los usuarios encuentran al importar datos a Firefly III, junto con sus soluciones.

---

### Firefly III muestra una página que dice "Vacío" y nada más

Este es un problema común relacionado con la configuración de OAuth.

* Asegúrate de **desmarcar** la casilla **"Confidencial"** en la configuración de tu Cliente OAuth.
* Asegúrate de que el puerto también esté presente en la variable de entorno `VANITY_URL`, por ejemplo: `http://localhost:9001`.

---

### ¿Todas las transacciones se importan con la fecha de hoy?

Esto suele ocurrir cuando el formato de fecha no se ha configurado correctamente.

* Asegúrate de que el campo de fecha en tu archivo CSV (ej. `2023-09-17`) esté marcado como la fecha de "Transacción".
* Verifica que el formato de fecha que seleccionas (ej. `Y-m-d`) coincida exactamente con el contenido del archivo. Sigue el formato indicado en [esta tabla](https://www.php.net/manual/en/datetime.createfromformat.php).

---

### Recibo errores "504 Gateway Timeout"

Cuando utilizas la interfaz web del importador de datos, la página puede expirar después de unos 600 segundos (10 minutos). Si utilizas un proxy inverso, este límite de tiempo podría ser aún más estricto.

Aunque el proceso de importación continúa en segundo plano, la página se "pierde" y ya no recibirás actualizaciones de estado.

**Solución**: Importa tus datos en lotes más pequeños para que la página no expire.

---

### Contenido mixto

Algunos bancos reutilizan las columnas para diferente contenido. Por ejemplo, el banco "Sparkasse" (Alemania) utiliza la columna BIC para otros datos si no hay un BIC disponible.

Si tu banco tiene problemas similares, la importación probablemente fallará. La mejor solución es ignorar esa columna por completo durante el mapeo.

---

### Filas de encabezado adicionales (solo CSV)

Algunos bancos insertan una fila de encabezado adicional (ej. `Fuente,Monto,Destino`) cada 100 filas, como si un ordenador olvidara los nombres de las columnas.

Si tu banco hace esto, esas filas de encabezado adicionales causarán un error durante la importación. Deberás eliminarlas manualmente de tu archivo CSV antes de importarlo.

---

### No hay suficiente información para que las filas sean únicas

Por defecto, el importador de datos omite las transacciones duplicadas. Si no configuras específicamente el importador para que acepte filas no únicas, puedes encontrarte con este problema.

**Solución**: Abre el archivo en Excel o Numbers y añade una nueva columna con una secuencia numérica básica: 1, 2, 3, 4, etc. Esto debería ser suficiente para que cada fila sea única.

---

### ¿Faltan datos?

Algunos bancos no proporcionan suficientes datos para realizar importaciones de calidad. Puedes notar esto cuando ocurren las siguientes cosas:

* Muchas transacciones donde la cuenta de contrapartida se llama `(sin nombre)`.
* Muchas transacciones con la descripción `(sin descripción)`.

Lamentablemente, no hay mucho que se pueda hacer al respecto desde el importador. Sin embargo, aquí tienes algunos consejos que pueden facilitarte la vida.

#### Solución: Crear reglas para complementar las transacciones

Incluso si hay la más mínima información en tus transacciones, puedes usar el motor de reglas de Firefly III para complementar y completar los datos. Por ejemplo:

* Si la descripción contiene `SPRMKT`, establece la descripción como `Compras` y la cuenta de destino como `Supermercado`.
* Toda transacción de exactamente `6.00` es probablemente de `Starbucks`.

#### Solución: Cuentas de destino / gastos

Algunos usuarios crean cuentas de gastos para cada tienda. Una alternativa más sencilla es agrupar las transacciones en categorías más amplias como "Compras" y "Restaurante" y darlo por terminado.
