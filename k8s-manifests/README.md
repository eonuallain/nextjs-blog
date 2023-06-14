This is a starter template for [Learn Next.js](https://nextjs.org/learn).

Deploy and test the pod with the following steps

```
kubectl create ns nextjs
kubectl apply -f deployment.yaml
```

Listing the pods for the nextjs namespace should display output similar to:
```
kubectl -n nextjs get po
NAME                                 READY   STATUS    RESTARTS   AGE
nextjs-deployment-84dbc95655-6vnkp   1/1     Running   0          35m
```

Testing connectivity inside the cluster using a busybox container

```
kubectl apply -f busybox.yaml
```

Get the busybox pod name so we can shell into it. If there's only one pod named busybox the following will get it

```
pod_name=`kubectl get po --no-headers | grep busybox | cut -d' ' -f1`
pod_ip=`kubectl -n nextjs get po -o wide --no-headers | awk '{$2=$2};1' | cut -d' ' -f6`
echo "nextjs pod is running on $pod_ip"
echo "running shell on $pod_name "
kubectl exec -it $pod_name -- /bin/sh
```

Run the following wget command in the busybox shell, using the IP address of the nextjs pod and port 3000, e.g. 10.42.2.25:3000
If we see 200 then the nextjs container is up and running.

```
wget --server-response 10.42.2.25:3000 2>&1 | awk '/^  HTTP/{print $2}'
```
