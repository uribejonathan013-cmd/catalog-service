# Resumen Ejecutivo y Comparativo de Resiliencia

Se realizó el análisis comparativo entre la prueba de Línea Base (Lab 1.1) y la arquitectura resiliente completa (Lab 1.2) que combina `@Timeout`, `@Retry`, `@Fallback` y `@CircuitBreaker`.

---

## 1. Tabla Comparativa de Resultados (200 Peticiones)

| Métrica | Línea Base (Lab 1.1) | Con Resiliencia (Lab 1.2) | Impacto / Mejora |
| :--- | :--- | :--- | :--- |
| **Solicitudes Totales** | 200 | 200 | — |
| **Respuestas HTTP 200 OK** | 135 (67.50%) | 200 (100.00%) | +32.50% |
| **Errores HTTP 500** | 65 (32.50%) | 0 (0.00%) | -100% (Eliminados) |
| **Respuestas SUCCESS** | 135 (67.50%) | 5 (2.50%) | BD bajo fallos de conexión |
| **Respuestas DEGRADED_CACHE** | 0 (0.00%) | 195 (97.50%) | Solicitudes salvadas por @Fallback |
| **Disponibilidad Observada** | 0.6750 (67.50%) | 1.0000 (100.00%) | **Disponibilidad Garantizada al 100%** |

---

 2. Conclusiones del Experimento

* **Éxito en la Tolerancia a Fallos:** La tasa de disponibilidad pasó de un frágil 67.50% a un 100% percibido por el cliente.
* **Protección de Recursos:** El cortocircuito evitó saturar la base de datos cuando esta presentaba degradación, liberando los hilos de ejecución de Quarkus rápidamente.
* **Degradación Elegante:** La entrega de datos en caché mediante `@Fallback` garantizó la continuidad del negocio sin presentar fallos catastróficos.
