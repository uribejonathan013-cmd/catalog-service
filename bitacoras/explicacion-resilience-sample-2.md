# Explicación del Patrón @Timeout y @Retry (Lab 1.2 - Paso 2.2)

## 1. Configuración Aplicada

En la clase `CatalogResource.java` se inyectaron los patrones `@Timeout` y `@Retry`:

* **`@Timeout(800)`**: Establece un límite máximo de ejecución de 800 ms por cada intento.
* **`@Retry(maxRetries = 2, delay = 150)`**: Permite hasta 2 reintentos adicionales (3 intentos en total) con una pausa de 150 ms entre intentos.

---

## 2. Análisis del Comportamiento Observado

* **Mitigación de Latencia Excesiva:** Si la base de datos responde con una latencia simulada mayor a 800 ms (por ejemplo, 3000 ms), el patrón `@Timeout` interrumpe la ejecución del hilo a los 800 ms y lanza una excepción `TimeoutException`.
* **Recuperación ante Fallas Transitorias:** En fallas temporales de lectura, `@Retry` intenta nuevamente consultar la base de datos en dos oportunidades más. Si el segundo o tercer intento tiene éxito dentro de la ventana de 800 ms, la petición logra responder con estado `SUCCESS` (HTTP 200).
* **Costo Operativo:** En peticiones donde el fallo es persistente, los reintentos incrementan la latencia total acumulada (800 ms + 150 ms + 800 ms) y multiplican el número de consultas hacia la base de datos saturada.
