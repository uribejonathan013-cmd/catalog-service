Explicación de Resultados: Línea Base de Disponibilidad (Lab 1.1)

1. Descripción de la Prueba

Se ejecutó la prueba de carga inicial sobre el microservicio catalog-service sin la implementación de patrones de resiliencia ni tolerancia a fallos. Se enviaron 200 peticiones HTTP GET al endpoint /v1/products (puerto 8085) mediante el script test-availability.ps1.

2. Resultados Obtenidos (availability-baseline.txt)

Solicitudes Totales: 200

Respuestas Exitosas (HTTP 200 OK - SUCCESS): 135 (~67.50%)

Respuestas de Error (HTTP 500 Internal Server Error): 65 (~32.50%)

Disponibilidad Observada: 0.6750 (67.50%)

3. Análisis del Comportamiento del Servicio

Causa de las Fallas: La simulación inyectó timeouts y fallos aleatorios de conexión en la base de datos primaria (clase CatalogResource.java). Al no existir mecanismos de contención, la excepción RuntimeException("Database connection timeout") se propagó directamente hacia la capa de transporte HTTP.

Impacto en Latencia: En cada fallo de base de datos, el hilo de ejecución permaneció bloqueado esperando la respuesta durante el tiempo de retardo simulado (hasta 3000 ms), degradando el tiempo de respuesta promedio de las peticiones fallidas antes de retornar un error catastrófico HTTP 500 al cliente.