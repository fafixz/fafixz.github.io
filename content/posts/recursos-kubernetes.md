+++
title = 'Kubernetes'
+++

## Artículo con definiciones y recursos para aprender Kubernetes  

[Kubernetes docs](https://kubernetes.io/docs/home/)

### Definición

**Kubernetes** es un orquestador de contenedores que gestiona el despliegue, escalado y mantenimiento de los mismos.

Para un entorno con pocos contenedores, por ejemplo: una app y una base de datos, no es necesario ya que una solución como **docker compose** resuelve el problema. Ya cuando intervienen muchas aplicaciones, el mantenimiento de tanta cantidad de contenedores se vuelve difícil de manejar.

### Componentes

[Componentes de Kubernetes](https://kubernetes.io/docs/concepts/overview/components/)

Un **clúster de Kubernetes** es un grupo de máquinas (nodos) que ejecutan aplicaciones en contenedores. 

En un **clúster de Kubernetes** existen varios componentes:

```txt
links que llevan a la documentación de cada componente ↓↓↓
```

#### Componentes del Control Plane

- [kube-apiserver](https://kubernetes.io/docs/concepts/architecture/#kube-apiserver)
- [etcd](https://kubernetes.io/docs/concepts/architecture/#etcd)
- [kube-scheduler](https://kubernetes.io/docs/concepts/architecture/#kube-scheduler)
- [kube-controller-manager](https://kubernetes.io/docs/concepts/architecture/#kube-controller-manager)
- [cloud-controller-manager (optional)](https://kubernetes.io/docs/concepts/architecture/#cloud-controller-manager)

#### Componentes de nodo

- [kubelet](https://kubernetes.io/docs/concepts/architecture/#kubelet)
- [kube-proxy (optional)](https://kubernetes.io/docs/concepts/architecture/#kube-proxy)


### minikube

[minikube docs](https://minikube.sigs.k8s.io/docs/)

En el caso de querer correr un cluster en local para aprender Kubernetes, **minikube** es una de las mejores alternativas.

### Recursos externos

[Kubernetes Roadmap](https://roadmap.sh/kubernetes)

[Kube by Example](https://kubebyexample.com/)

[Kubernetes Tutorial DevOpsCube](https://devopscube.com/kubernetes-tutorials-beginners/)

[freecodecamp The Kubernetes Handbook](https://www.freecodecamp.org/news/the-kubernetes-handbook/)

[k8s Architecture](https://www.youtube.com/watch?v=TlHvYWVUZyc&t=39s)

[KUBERNETES De NOVATO a PRO!](https://www.youtube.com/watch?v=DCoBcpOA7W4)

[Complete Kubernetes Course DevOps Directive](https://www.youtube.com/watch?v=2T86xAtR6Fo&t=3095s)
