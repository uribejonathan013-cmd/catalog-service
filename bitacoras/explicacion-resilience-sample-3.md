Explicación del Patrón @Fallback (Lab 1.2 - Paso 2.3)

1. Configuración Aplicada

Se definió el patrón de respaldo o comportamiento degradado mediante la anotación:

@Fallback(fallbackMethod = "getCatalogFallback")


El método alternativo retorna una estructura JSON degradada servida desde una memoria caché simulada:

{
  "status": "DEGRADED_CACHE",
  "data": ["Cached Product A", "Cached Product B"]
}


2. Análisis del Comportamiento Observado

Eliminación Total de Errores HTTP 500: Cuando los intentos de reintento (@Retry) se agotan o el tiempo límite (@Timeout) expira, la excepción ya no se devuelve al usuario final. En su lugar, MicroProfile redirige la llamada de manera transparente hacia getCatalogFallback().

Transformación de Respuesta: La tasa de errores HTTP 500 cayó al 0.00%. El 100% de las solicitudes con fallos persistentes retornaron un código de respuesta exitoso HTTP 200 OK con el estado DEGRADED_CACHE.

Experiencia de Usuario: Garantiza que el cliente reciba datos funcionales degradados en lugar de una pantalla de error del sistema.