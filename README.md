# kubernetes-test
Kubernetes..... let's play

## 📦 Contenido

Este repositorio contiene ejemplos de manifiestos de Kubernetes para aprender y practicar:

- **3 Pods individuales** con diferentes configuraciones
- **1 Deployment** con múltiples réplicas
- **Servicios, ConfigMaps y Namespaces**

## 🗂️ Estructura del Proyecto

```
k8s/
├── pods/               # Ejemplos de Pods individuales
│   ├── pod1-nginx.yaml    # Pod con servidor Nginx
│   ├── pod2-busybox.yaml  # Pod con BusyBox
│   └── pod3-redis.yaml    # Pod con Redis y volumen
├── deployments/        # Ejemplos de Deployments
│   └── deployment.yaml    # Deployment con 3 réplicas
├── services/          # Ejemplos de Services
│   └── service.yaml       # Service tipo LoadBalancer
└── configs/           # Configuraciones
    ├── configmap.yaml     # ConfigMap con configuraciones
    └── namespace.yaml     # Namespace para desarrollo
```

## 🚀 Uso

### Prerequisitos

- Tener instalado `kubectl`
- Tener acceso a un cluster de Kubernetes (minikube, kind, EKS, GKE, AKS, etc.)

### Crear los Pods individuales

```bash
# Crear el pod de Nginx
kubectl apply -f k8s/pods/pod1-nginx.yaml

# Crear el pod de BusyBox
kubectl apply -f k8s/pods/pod2-busybox.yaml

# Crear el pod de Redis
kubectl apply -f k8s/pods/pod3-redis.yaml

# Verificar que los pods estén corriendo
kubectl get pods
```

### Crear el Deployment

```bash
# Crear el deployment con 3 réplicas
kubectl apply -f k8s/deployments/deployment.yaml

# Verificar el deployment
kubectl get deployments

# Ver los pods creados por el deployment
kubectl get pods -l app=webapp
```

### Crear el Service

```bash
# Crear el service
kubectl apply -f k8s/services/service.yaml

# Verificar el service
kubectl get services
```

### Crear ConfigMap y Namespace

```bash
# Crear namespace
kubectl apply -f k8s/configs/namespace.yaml

# Crear secrets (datos sensibles)
kubectl apply -f k8s/configs/secrets.yaml

# Crear configmap (datos no sensibles)
kubectl apply -f k8s/configs/configmap.yaml

# Ver los recursos creados
kubectl get namespaces
kubectl get secrets
kubectl get configmaps
```

## 🔒 Seguridad y Buenas Prácticas

### Secrets
Los archivos de ejemplo contienen contraseñas y credenciales de demostración. **NUNCA** uses estos valores en producción:

```bash
# Generar una contraseña segura
openssl rand -base64 32

# Crear un Secret desde la línea de comandos (recomendado para producción)
kubectl create secret generic redis-secret \
  --from-literal=redis-password=$(openssl rand -base64 32)

# Ver Secrets (los valores están codificados en base64, no encriptados en la salida)
kubectl get secret redis-secret -o yaml
```

**Importante:**
- Nunca commitear Secrets reales al repositorio
- Usar herramientas como Sealed Secrets o External Secrets para gestionar secretos en producción
- Los Secrets de Kubernetes se almacenan codificados en base64, no encriptados (usar encryption at rest en el cluster)

## 🧹 Limpiar recursos

Para eliminar todos los recursos creados:

```bash
# Eliminar pods individuales
kubectl delete -f k8s/pods/

# Eliminar deployment y service
kubectl delete -f k8s/deployments/
kubectl delete -f k8s/services/

# Eliminar configmap
kubectl delete -f k8s/configs/configmap.yaml
kubectl delete -f k8s/configs/secrets.yaml

# Eliminar namespace (opcional)
kubectl delete -f k8s/configs/namespace.yaml
```

## 📝 Descripción de los Ejemplos

### Pods

1. **pod1-nginx.yaml**: Pod simple con servidor Nginx, incluye límites de recursos
2. **pod2-busybox.yaml**: Pod con BusyBox que ejecuta un comando y duerme
3. **pod3-redis.yaml**: Pod con Redis que incluye variables de entorno y volumen

### Deployment

- **deployment.yaml**: Deployment con 3 réplicas de Nginx
  - Incluye health checks (liveness y readiness probes)
  - Configuración de recursos (requests y limits)
  - Variables de entorno

### Service

- **service.yaml**: Service tipo LoadBalancer para exponer el deployment

### Configs

- **configmap.yaml**: ConfigMap con ejemplos de configuraciones no sensibles de aplicación
- **secrets.yaml**: Secrets para almacenar datos sensibles como contraseñas y URLs de bases de datos
- **namespace.yaml**: Namespace para separar entornos

## 🎓 Comandos Útiles de Kubernetes

```bash
# Ver todos los pods
kubectl get pods

# Ver detalles de un pod
kubectl describe pod <pod-name>

# Ver logs de un pod
kubectl logs <pod-name>

# Ejecutar comando en un pod
kubectl exec -it <pod-name> -- /bin/sh

# Ver todos los recursos en un namespace
kubectl get all

# Ver eventos del cluster
kubectl get events --sort-by=.metadata.creationTimestamp
```
