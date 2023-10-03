# K8s Pods & Deployments

## Overview

Kubernetes (often abbreviated as K8s) is an open-source container orchestration platform that helps manage and automate the deployment, scaling, and operation of containerized applications. Two fundamental concepts in Kubernetes are "Pods" and "Deployments," and they play crucial roles in the management of containerized workloads.

1. **Pods**:
   - A **Pod** is the smallest deployable unit in Kubernetes. It represents a single instance of a running process in a cluster.
   - Pods are designed to hold one or more containers that are tightly coupled and share the same network namespace, storage, and other resources. These containers within a pod typically work together to perform a specific task or function.
   - Containers within the same Pod can communicate with each other using localhost, making them suitable for applications that require close coordination or data sharing.
   - Pods are ephemeral and can be created, terminated, and replaced easily. However, they are not self-healing; if a Pod fails, it won't automatically recover.

2. **Deployments**:
   - A **Deployment** is a higher-level abstraction in Kubernetes that helps manage the desired state of one or more Pods.
   - Deployments enable you to declare the desired number of replicas (copies) of a Pod template (which defines the container image, resources, and other settings) and maintain that desired state.
   - Deployments provide capabilities for rolling updates and rollbacks. When you want to update the application to a new version or change its configuration, you can update the Deployment, and Kubernetes will manage the process of gradually replacing old Pods with new ones to minimize downtime.
   - Deployments also provide features like scaling (scaling the number of replicas up or down), self-healing (replacing failed Pods), and version management.

Here's a typical workflow involving Pods and Deployments in Kubernetes:

1. Define a Pod template: You create a Pod template specifying the container image(s), resource constraints, and other settings for your application.

2. Create a Deployment: You create a Deployment object, referencing the Pod template. The Deployment ensures that the desired number of Pods, as specified in the template, are running at all times.

3. Scaling: If you need to scale your application, you can update the replica count in the Deployment, and Kubernetes will adjust the number of Pods accordingly.

4. Updates: To update your application, you modify the Pod template in the Deployment. Kubernetes will perform rolling updates, gradually replacing old Pods with new ones, ensuring minimal disruption.

5. Rollbacks: If an update introduces issues, you can perform a rollback to a previous version of your application by specifying the desired revision in the Deployment.

In summary, Pods are the basic building blocks for containerized workloads in Kubernetes, while Deployments provide a higher-level abstraction to manage and control the lifecycle of Pods, making it easier to maintain, scale, and update applications in a Kubernetes cluster.

## Examples

**Generic Pod Manifest (pod.yaml):**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: your-container-image:tag
      ports:
        - containerPort: 80
```

In this example:

- `apiVersion`: Specifies the Kubernetes API version to use.
- `kind`: Indicates the type of resource, which is a Pod in this case.
- `metadata`: Contains metadata about the Pod, including its name.
- `spec`: Defines the specification of the Pod.
  - `containers`: Lists the containers to run within the Pod.
    - `name`: Specifies the name of the container.
    - `image`: Specifies the container image to use.
    - `ports`: Defines the ports to expose within the container.

**Generic Deployment Manifest (deployment.yaml):**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-container
          image: your-container-image:tag
          ports:
            - containerPort: 80
```

In this example:

- `apiVersion`: Specifies the API version for Deployments.
- `kind`: Indicates that this resource is a Deployment.
- `metadata`: Contains metadata about the Deployment, including its name.
- `spec`: Defines the specification of the Deployment.
  - `replicas`: Specifies the desired number of replicas (Pods) to maintain.
  - `selector`: Defines how to select the Pods managed by this Deployment.
  - `template`: Specifies the Pod template used to create new Pods.
    - `metadata`: Labels to apply to the Pods created by this template.
    - `spec`: Defines the specification of the Pods created by this template, including the container(s) to run.

These are basic examples, and you would replace `your-container-image:tag` with the actual container image and tag you want to use. You can also customize other fields as needed, such as resource requests, environment variables, and more, based on the specific requirements of your application.

## Commands

1. **Create or Apply Resources:**
   - Create a resource from a YAML file: `kubectl create -f <filename.yaml>`
   - Apply changes to a resource from a YAML file: `kubectl apply -f <filename.yaml>`

2. **Get Information:**
   - List all resources in the current namespace: `kubectl get all`
   - Describe a specific resource: `kubectl describe <resource_type> <resource_name>`

3. **Delete Resources:**
   - Delete a resource by name: `kubectl delete <resource_type> <resource_name>`
   - Delete all resources in a file: `kubectl delete -f <filename.yaml>`

4. **Editing Resources:**
   - Edit a resource in your preferred text editor: `kubectl edit <resource_type> <resource_name>`

**Generating Resources:**

5. **Generate Resource Definitions:**
   - Generate a resource YAML template: `kubectl create <resource_type> <resource_name> --dry-run=client -o yaml > <filename.yaml>`

**Scaling Resources:**

6. **Scaling Deployments:**
   - Scale a Deployment to a specific number of replicas: `kubectl scale deployment <deployment_name> --replicas=<replica_count>`

**Logs and Debugging:**

8. **Viewing Pod Logs:**
   - View logs for a specific Pod: `kubectl logs <pod_name>`
   - Stream logs in real-time: `kubectl logs -f <pod_name>`

9. **Exec into a Container:**
   - Open a shell inside a running container: `kubectl exec -it <pod_name> -- /bin/bash`

These commands are essential for building, managing, and generating Kubernetes resources and interacting with your Kubernetes cluster using `kubectl`. Be sure to replace placeholders like `<resource_type>`, `<resource_name>`, `<filename.yaml>`, and others with your specific resource names and file paths as needed.

