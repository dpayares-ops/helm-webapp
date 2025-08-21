
# Create the helmchart
```
helm create webapp1
```


# Follow along with the video
- Create the files per the video, copying and pasting from templates-original
- you can also use the files in the solution folder

# Install the first one
```
helm install mywebapp-release webapp1/ --values webapp1/values.yaml
```

# Upgrade after templating
```
helm upgrade mywebapp-release webapp1/ --values mywebapp/values.yaml
```

# Accessing it
```
minikube tunnel
```

# Create dev/prod
```
k create namespace dev
k create namespace prod
helm install mywebapp-release-dev webapp1/ --values webapp1/values.yaml -f webapp1/values-dev.yaml -n dev
helm install mywebapp-release-prod webapp1/ --values webapp1/values.yaml -f webapp1/values-prod.yaml -n prod
helm ls --all-namespaces
```


# Cheat Sheet - Helm Básico
# Resumen de Helm

## 1️⃣ Qué es Helm

Helm es un **gestor de paquetes para Kubernetes**, que permite:

- Definir aplicaciones como **charts** (plantillas de Kubernetes).  
- Instalar, actualizar y desinstalar aplicaciones de forma sencilla.  
- Reutilizar configuraciones con valores dinámicos (`values.yaml`).  
- Manejar dependencias y versiones de aplicaciones.  

En otras palabras, Helm funciona como **el “apt/yum” de Kubernetes**, pero para aplicaciones en clusters.

---

## 2️⃣ Cómo funciona Helm

1. **Charts:** Plantillas de recursos de Kubernetes (`Deployment`, `Service`, `Ingress`, etc.) con variables.  
2. **Releases:** Instancias desplegadas de un chart en tu cluster. Cada `install` crea un release.  
3. **Valores (`values.yaml`):** Permiten personalizar la configuración de un chart sin tocar los templates.  
4. **Helm templates:** Renderiza YAML final de Kubernetes antes de aplicar (`helm template`).  
5. **Helm commands:** Facilitan instalar, actualizar, listar, desinstalar y depurar releases.

Flujo típico:

```
chart + values.yaml → helm install → release → Kubernetes resources

Este documento contiene los comandos básicos de Helm para trabajar con charts y releases en Kubernetes.

---

## 1️⃣ Repositorios de Helm

| Comando | Descripción |
|---------|-------------|
| `helm repo add <nombre> <url>` | Agrega un repositorio de charts. Ej: `helm repo add traefik https://helm.traefik.io/traefik` |
| `helm repo update` | Actualiza la información de los charts disponibles. |
| `helm search repo <chart>` | Busca charts en los repositorios agregados. Ej: `helm search repo nginx` |

---

## 2️⃣ Crear un nuevo chart

| Comando | Descripción |
|---------|-------------|
| `helm create <nombre-del-chart>` | Crea la estructura básica de un chart de Helm en un directorio con el nombre indicado. |
| Ejemplo: | `helm create nginx-ex` |

**Archivos generados típicos:**

- `Chart.yaml` → metadata del chart  
- `values.yaml` → valores por defecto  
- `templates/` → templates de Kubernetes  
- `charts/` → subcharts o dependencias  
- `.helmignore` → archivos a ignorar al empaquetar el chart  

---

## 3️⃣ Instalar un chart

| Comando | Descripción |
|---------|-------------|
| `helm install <release-name> <chart> [-n <namespace>] [--values values.yaml]` | Instala un chart con un nombre de release. |
| Ejemplo: | `helm install miweb ./nginx-ex -n default --values values.yaml` |

---

## 4️⃣ Listar releases

| Comando | Descripción |
|---------|-------------|
| `helm list` | Lista los releases en el namespace actual. |
| `helm list -A` | Lista todos los releases en todos los namespaces. |

---

## 5️⃣ Actualizar un release

| Comando | Descripción |
|---------|-------------|
| `helm upgrade <release-name> <chart> [-n <namespace>] [--values values.yaml]` | Actualiza un release existente. |
| Ejemplo: | `helm upgrade miweb ./nginx-ex -n default --values values.yaml` |

---

## 6️⃣ Verificar un release

| Comando | Descripción |
|---------|-------------|
| `helm status <release-name> [-n <namespace>]` | Muestra el estado actual del release. |
| `helm get all <release-name> [-n <namespace>]` | Muestra todos los recursos generados y la configuración completa del release. |
| `helm history <release-name> [-n <namespace>]` | Muestra el historial de actualizaciones del release. |

---

## 7️⃣ Desinstalar un release

| Comando | Descripción |
|---------|-------------|
| `helm uninstall <release-name> [-n <namespace>]` | Elimina un release y todos sus recursos asociados. |
| Ejemplo: | `helm uninstall miweb -n default` |

---

## 8️⃣ Plantillas y depuración

| Comando | Descripción |
|---------|-------------|
| `helm template <chart> [--values values.yaml]` | Renderiza los manifests de Kubernetes **sin instalar** nada. |
| `helm lint <chart>` | Valida que el chart tenga la estructura y sintaxis correcta. |
| `helm rollback <release-name> <revision> [-n <namespace>]` | Revierte un release a una versión anterior. |

---

## 💡 Tips prácticos

- Siempre probar cambios con `helm template` antes de `install` o `upgrade`.  
- Mantener los repos actualizados con `helm repo update`.  
- `helm create` es ideal para iniciar un chart desde cero.  
- NodePorts, hostPorts o MetalLB son útiles para exponer servicios en clusters locales.  

---

