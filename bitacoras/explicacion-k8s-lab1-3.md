# Explicación Técnica Integrada y Validación (Lab 1.3)

## 1. Explicación Técnica Integrada

El flujo completo inició con el **build local** mediante Maven y Docker, empaquetando el microservicio resiliente en la imagen `tata013/catalog-service:1.0.0`. Posteriormente, mediante `docker push`, se publicó en **Docker Hub**, dejándola accesible desde cualquier entorno.

En **Kubernetes**, el manifiesto `k8s/01-deployment.yaml` declara de forma reproducible qué imagen utilizar y qué sondas de salud ejecutar. Al aplicar los YAML con `kubectl apply`, Kubernetes gestiona la **descarga automática** desde Docker Hub y levanta el contenedor bajo un `Service` `ClusterIP`.

A diferencia del **Lab 1.1**, donde `/health`, `/ready` y `/live` se validaban manualmente con `curl`, en el **Lab 1.3** Kubernetes realiza **health checks automáticos**: `/ready` verifica cada 5 segundos si el pod puede recibir tráfico y `/live` comprueba cada 10 segundos si el proceso sigue activo. Esta transición pedagógica es clave en pipelines CI/CD profesionales, pues permite al orquestador aislar o reiniciar pods degradados automáticamente sin intervención manual (`Build local -> Push a registry -> Deploy -> Validación automática`).

---

## 2. Validación de Funcionamiento del Servicio (Endpoints)

Pruebas ejecutadas mediante túnel `kubectl port-forward svc/catalog-service 8085:8080`:

### `GET /v1/products`
```json
{
  "status": "SUCCESS",
  "data": ["Product A", "Product B", "Product C"]
}
{
  "status": "UP",
  "checks": [
    {
      "name": "catalog-database-check",
      "status": "UP"
    }
  ]
}
{
  "status": "UP",
  "checks": []
}
