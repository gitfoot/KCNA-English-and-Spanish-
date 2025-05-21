### 1. Pods (El Componente Más Pequeño) / Pods (The Smallest Component)

* **¿Qué es?** Un **Pod** es la unidad más pequeña y básica de implementación en Kubernetes. Representa una instancia única de una aplicación o una parte de ella.
    * **What is it?** A **Pod** is the smallest and most basic deployable unit in Kubernetes. It represents a single instance of an application or a part of it.
* **Contiene:** Un Pod puede contener **una o más contenedores** (generalmente Docker, pero no exclusivamente). Si tiene múltiples contenedores, estos están estrechamente relacionados y deben funcionar juntos para el mismo propósito (por ejemplo, un contenedor de aplicación y un contenedor "sidecar" para logging o monitoreo). Comparten el mismo almacenamiento, red (dirección IP) y espacio de nombres IPC.
    * **Contains:** A Pod can contain **one or more containers** (generally Docker, but not exclusively). If it has multiple containers, they are tightly coupled and must work together for the same purpose (e.g., an application container and a "sidecar" container for logging or monitoring). They share the same storage, network (IP address), and IPC namespace.
* **Ciclo de vida:** Los Pods son efímeros. Se crean, se ejecutan y se terminan. Kubernetes no los recreará si fallan o mueren por sí mismos. Para manejar esto, usamos controladores de nivel superior.
    * **Lifecycle:** Pods are ephemeral. They are created, run, and terminate. Kubernetes won't recreate them if they fail or die by themselves. To handle this, we use higher-level controllers.
* **Ejemplo:** Un Pod podría contener un contenedor de tu aplicación web y otro contenedor que recopila logs de esa aplicación.
    * **Example:** A Pod could contain a container for your web application and another container that collects logs from that application.

---

### 2. ReplicaSets (Garantizando el Número de Pods) / ReplicaSets (Ensuring the Number of Pods)

* **¿Qué es?** Un **ReplicaSet** es un controlador de Kubernetes que asegura que un número especificado de réplicas de un Pod esté ejecutándose en todo momento.
    * **What is it?** A **ReplicaSet** is a Kubernetes controller that ensures a specified number of Pod replicas are running at all times.
* **Función principal:** Su objetivo principal es mantener la estabilidad de la aplicación. Si un Pod falla o se elimina, el ReplicaSet detecta que el número de réplicas es menor al deseado y crea un nuevo Pod para compensar. Si se crean demasiados Pods, el ReplicaSet elimina los excedentes.
    * **Main function:** Its primary goal is to maintain the application's stability. If a Pod fails or is deleted, the ReplicaSet detects that the number of replicas is less than desired and creates a new Pod to compensate. If too many Pods are created, the ReplicaSet removes the excess.
* **Selección de Pods:** Utiliza un "selector de etiquetas" (`selector`) para identificar los Pods que debe gestionar.
    * **Pod Selection:** It uses a "label selector" (`selector`) to identify the Pods it needs to manage.
* **Uso directo:** Aunque se puede usar un ReplicaSet directamente, en la práctica rara vez se hace. Los **Deployments** (que veremos a continuación) son la forma preferida de gestionar Pods y ReplicaSets, ya que proporcionan más funcionalidades para las actualizaciones.
    * **Direct Use:** While a ReplicaSet can be used directly, in practice, it's rarely done. **Deployments** (which we'll see next) are the preferred way to manage Pods and ReplicaSets, as they provide more functionalities for updates.
* **Ejemplo:** Un ReplicaSet podría asegurar que siempre haya 3 instancias de tu Pod de aplicación web funcionando.
    * **Example:** A ReplicaSet could ensure that there are always 3 instances of your web application Pod running.

---

### 3. Deployments (Gestión Declarativa de Aplicaciones) / Deployments (Declarative Application Management)

* **¿Qué es?** Un **Deployment** es un controlador de Kubernetes de nivel superior que proporciona actualizaciones declarativas para Pods y ReplicaSets. Es la forma más común de ejecutar aplicaciones sin estado en Kubernetes.
    * **What is it?** A **Deployment** is a higher-level Kubernetes controller that provides declarative updates for Pods and ReplicaSets. It's the most common way to run stateless applications in Kubernetes.
* **Funcionalidades Clave:**
    * **Creación de ReplicaSets:** Un Deployment crea y gestiona ReplicaSets en segundo plano. Cuando creas un Deployment, este crea un ReplicaSet que, a su vez, crea los Pods.
        * **ReplicaSet Creation:** A Deployment creates and manages ReplicaSets in the background. When you create a Deployment, it creates a ReplicaSet, which, in turn, creates the Pods.
    * **Actualizaciones y Rollbacks:** Permite actualizar fácilmente tu aplicación a una nueva versión de imagen de contenedor, así como revertir a una versión anterior si algo sale mal (rollback). Gestiona estrategias de actualización como "Rolling Update" (actualización progresiva) para garantizar cero tiempo de inactividad.
        * **Updates and Rollbacks:** It allows you to easily update your application to a new container image version, as well as revert to an earlier version if something goes wrong (rollback). It manages update strategies like "Rolling Update" (progressive update) to ensure zero downtime.
    * **Escalado:** Puedes escalar el número de réplicas de tu aplicación simplemente editando el Deployment.
        * **Scaling:** You can scale the number of replicas of your application simply by editing the Deployment.
* **Relación con ReplicaSets:** Cuando actualizas un Deployment, este crea un *nuevo* ReplicaSet con la nueva versión de los Pods y va reduciendo el número de Pods en el *antiguo* ReplicaSet mientras aumenta el número en el *nuevo*. Esto permite transiciones suaves sin interrupciones.
    * **Relationship with ReplicaSets:** When you update a Deployment, it creates a *new* ReplicaSet with the new Pod version and gradually reduces the number of Pods in the *old* ReplicaSet while increasing the number in the *new* one. This allows for smooth transitions without interruptions.
* **Ejemplo:** Defines un Deployment para tu aplicación web, especificando que quieres 3 réplicas. Cuando lanzas una nueva versión de tu aplicación, actualizas el Deployment, y Kubernetes se encarga de cambiar gradualmente las 3 instancias a la nueva versión.
    * **Example:** You define a Deployment for your web application, specifying that you want 3 replicas. When you release a new version of your application, you update the Deployment, and Kubernetes handles the gradual change of the 3 instances to the new version.

---

### 4. Namespaces (Organización del Clúster) / Namespaces (Cluster Organization)

* **¿Qué es?** Un **Namespace** (Espacio de Nombres) es una forma de dividir un clúster de Kubernetes en múltiples entornos virtuales o "clústeres virtuales" lógicamente aislados.
    * **What is it?** A **Namespace** is a way to divide a Kubernetes cluster into multiple virtual environments or logically isolated "virtual clusters."
* **Propósito:**
    * **Aislamiento:** Permite que varios equipos o proyectos compartan el mismo clúster de Kubernetes sin interferir entre sí. Los recursos en un Namespace están aislados de los recursos en otros Namespaces.
        * **Isolation:** It allows multiple teams or projects to share the same Kubernetes cluster without interfering with each other. Resources in one Namespace are isolated from resources in other Namespaces.
    * **Organización:** Ayuda a organizar los recursos del clúster (Pods, Deployments, Services, etc.) en grupos manejables.
        * **Organization:** It helps organize cluster resources (Pods, Deployments, Services, etc.) into manageable groups.
    * **Control de acceso:** Puedes aplicar políticas de control de acceso basadas en Roles (RBAC) a nivel de Namespace, lo que permite a diferentes usuarios o grupos tener permisos específicos en sus propios Namespaces.
        * **Access Control:** You can apply Role-Based Access Control (RBAC) policies at the Namespace level, allowing different users or groups to have specific permissions within their own Namespaces.
    * **Limitación de recursos (Resource Quotas):** Puedes establecer límites de recursos (CPU, memoria) para cada Namespace para evitar que un proyecto consuma todos los recursos del clúster.
        * **Resource Quotas:** You can set resource limits (CPU, memory) for each Namespace to prevent one project from consuming all cluster resources.
* **Recursos con Scope de Namespace:** La mayoría de los recursos de Kubernetes (Pods, Deployments, ReplicaSets, Services, ConfigMaps, Secrets, etc.) viven dentro de un Namespace.
    * **Namespace-Scoped Resources:** Most Kubernetes resources (Pods, Deployments, ReplicaSets, Services, ConfigMaps, Secrets, etc.) live within a Namespace.
* **Recursos con Scope de Clúster:** Algunos recursos (como Nodes, PersistentVolumes) no pertenecen a ningún Namespace, ya que son recursos a nivel de clúster.
    * **Cluster-Scoped Resources:** Some resources (like Nodes, PersistentVolumes) don't belong to any Namespace, as they are cluster-level resources.
* **Namespaces por defecto:** Un clúster de Kubernetes viene con algunos Namespaces predefinidos:
    * `default`: Para tus recursos si no especificas un Namespace.
        * `default`: For your resources if you don't specify a Namespace.
    * `kube-system`: Para los componentes del propio clúster de Kubernetes.
        * `kube-system`: For the Kubernetes cluster components themselves.
    * `kube-public`: Contiene datos que son legibles públicamente por todos los usuarios del clúster.
        * `kube-public`: Contains data that is publicly readable by all cluster users.
    * `kube-node-lease`: Contiene objetos de "lease" de latidos para mejorar la escalabilidad del nodo.
        * `kube-node-lease`: Contains "lease" objects for node heartbeats to improve node scalability.
* **Ejemplo:** Podrías tener un `namespace-desarrollo` para tu entorno de desarrollo, un `namespace-staging` para pruebas y un `namespace-produccion` para tu aplicación en vivo, todos en el mismo clúster físico.
    * **Example:** You could have a `development-namespace` for your development environment, a `staging-namespace` for testing, and a `production-namespace` for your live application, all within the same physical cluster.

