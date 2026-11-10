xplicación del Patrón @CircuitBreaker (Lab 1.2 - Paso 2.4)

1. Configuración Aplicada

Se integró la protección por Cortocircuito en la firma del método principal:

@CircuitBreaker(
    requestVolumeThreshold = 10,
    failureRatio = 0.7,
    delay = 5000
)


requestVolumeThreshold = 10: Ventana de evaluación de las últimas 10 peticiones.

failureRatio = 0.7: Si el 70% o más de las peticiones en la ventana fallan, el circuito se abre.

delay = 5000: Duración de la fase OPEN (5 segundos).

2. Análisis del Comportamiento Observado

Apertura del Circuito (Fase OPEN): Tras detectar la ráfaga inicial de fallos en la base de datos, el @CircuitBreaker conmutó de estado CLOSED a OPEN. Durante este estado, las peticiones entrantes fueron rechazadas inmediatamente sin intentar consultar la base de datos ni ejecutar reintentos.

Reducción Drástica de Latencia: En estado OPEN, el tiempo de respuesta cayó a valores ultra bajos (entre 0.003s y 0.20s), derivando directamente la solicitud al @Fallback.

Prueba de Autocuración (Fase HALF_OPEN): Al transcurrir los 5 segundos de retardo (delay = 5000), el circuito pasó a HALF_OPEN para permitir peticiones de prueba hacia la base de datos. Si las peticiones de prueba fueron exitosas, el circuito se cerró nuevamente (CLOSED), restaurando la operación normal.