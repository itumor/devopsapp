NAME: rocketchat
LAST DEPLOYED: Thu Mar 16 08:13:05 2023
NAMESPACE: rocketchat
STATUS: deployed
REVISION: 1
NOTES:
Rocket.Chat can be accessed via port 80 on the following DNS name from within your cluster:

- http://rocketchat-rocketchat.rocketchat

You can easily connect to the remote instance from your browser. Forward the webserver port to localhost:8888

- kubectl port-forward --namespace rocketchat $(kubectl get pods --namespace rocketchat -l "app.kubernetes.io/name=rocketchat,app.kubernetes.io/instance=rocketchat" -o jsonpath='{ .items[0].metadata.name }') 8888:3000

You can also connect to the container running Rocket.Chat. To open a shell session in the pod run the following:

- kubectl exec -i -t --namespace rocketchat $(kubectl get pods --namespace rocketchat -l "app.kubernetes.io/name=rocketchat,app.kubernetes.io/instance=rocketchat" -o jsonpath='{.items[0].metadata.name}') /bin/sh

To trail the logs for the Rocket.Chat pod run the following:

- kubectl logs -f --namespace rocketchat $(kubectl get pods --namespace rocketchat -l "app.kubernetes.io/name=rocketchat,app.kubernetes.io/instance=rocketchat" -o jsonpath='{ .items[0].metadata.name }')

To expose Rocket.Chat via an Ingress you need to set host and enable ingress.

helm install --set host=chat.yourdomain.com --set ingress.enabled=true stable/rocketchat