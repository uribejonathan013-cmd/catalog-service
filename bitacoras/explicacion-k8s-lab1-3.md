# Despliegue en Kubernetes y Validación de Health Checks (Lab 1.3)

## 1. Empaquetado y Publicación en Docker Hub
* **Imagen generada:** `tata013/catalog-service:1.0.0`
* **Verificación de registro:** Publicada mediante `docker push` y validada con descarga limpia (`docker pull`) desde Docker Hub.

---

## 2. Arquitectura Declarativa en Kubernetes
Se desplegó el microservicio resiliente (con `@Timeout`, `@Retry`, `@Fallback` y `@CircuitBreaker` del Lab 1.2) mediante manifiestos YAML:

| Recurso | Archivo | Configuración Clave |
| :--- | :--- | :--- |
| **Deployment** | `k8s/01-deployment.yaml` | 1 réplica, `imagePullPolicy: IfNotPresent`, sondas HTTP en puerto `8080` |
| **Service** | `k8s/02-service.yaml` | Tipo `ClusterIP`, exposición interna `8080/TCP` |

---

## 3. Análisis de Health Checks Automatizados
* **`livenessProbe` (`/live`):** Mantuvo el contenedor activo con `0` reinicios (`Restart Count: 0`).
* **`readinessProbe` (`/ready`):** Ante fallos simulados de conexión a base de datos (`HTTP 503`), Kubernetes retiró temporalmente el pod del balanceo de tráfico sin reiniciar el contenedor, restaurando el estado `Ready: True` automáticamente en cuanto la siguiente sonda tuvo éxito.