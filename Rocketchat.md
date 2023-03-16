To install Rocket Chat on Kubernetes with an external database using Helm, you can follow these steps:


1. Make sure you have Kubernetes and Helm installed on your system.
2. Create a Kubernetes namespace for Rocket Chat.

```
kubectl create namespace rocketchat
```

3. Add the Rocket Chat Helm chart repository to Helm.

```
helm repo add rocketchat https://rocketchat.github.io/helm-charts
```
4. Update the Helm chart repository.
```
helm repo update
```

5. Create a values file for the Rocket Chat installation with an external database. Here is an example values file:
```yaml
mongodb:
  mongodbUri: mongodb://<dbuser>:<dbpassword>@<dbhost>:<dbport>/<dbname>
rocketchat:
  externalDatabase:
    enabled: true
    mongodbUri: "{{ .Values.mongodb.mongodbUri }}"
```

Replace <dbuser>, <dbpassword>, <dbhost>, <dbport>, and <dbname> with the credentials and details of your external MongoDB database. Save this file as values.yaml.

6. Install Rocket Chat using Helm and the values file.
```
helm install rocketchat rocketchat/rocketchat --namespace rocketchat -f values.yaml
```

7. Wait for the Rocket Chat pods to start running.
```
kubectl get pods -n rocketchat
```
The output should show one or more pods with names starting with rocketchat-.

8. Create a Kubernetes service to expose the Rocket Chat deployment.
```
kubectl expose deployment rocketchat-rocketchat --type=LoadBalancer --name=rocketchat --namespace rocketchat
```

This creates a service named rocketchat in the rocketchat namespace that exposes the Rocket Chat deployment using a load balancer.

9. Wait for the service to obtain an external IP address.

```
kubectl get services rocketchat -n rocketchat -w
```
The output should show an external IP address in the EXTERNAL-IP column.

10. Access Rocket Chat using the external IP address in your web browser.

11. You can easily connect to the remote instance from your browser. Forward the webserver port to localhost:8888

```
kubectl port-forward --namespace rocketchat $(kubectl get pods --namespace rocketchat -l "app.kubernetes.io/name=rocketchat,app.kubernetes.io/instance=rocketchat" -o jsonpath='{ .items[0].metadata.name }') 8888:3000
``
https://localhost:8888


That's it! You have now installed Rocket Chat on Kubernetes with an external MongoDB database using Helm.