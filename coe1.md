Create a new Pod named nginx running the image nginx:1.17.10. Expose the container port 80. The Pod should live in the namespace named ckad.
```
kubectl create ns ckad
```
```
kubectl run nginx --image=nginx:1.17.10 --port=80 --namespace=ckad
```

Get the details of the Pod including its IP address.
```
```

Create a temporary Pod that uses the busybox image to execute a wget command inside of the container. The wget command should access the endpoint exposed by the nginx container. You should see the HTML response body rendered in the terminal.

```
```
Get the logs of the nginx container.
```
```

Add the environment variables DB_URL=postgresql://mydb:5432 and DB_USERNAME=admin to the container of the nginx Pod.
```
```

Open a shell for the nginx container and inspect the contents of the current directory ls -l.
```
```
Create a YAML manifest for a Pod named loop that runs the busybox image in a container. The container should run the following command: for i in {1..10}; do echo "Welcome $i times"; done. Create the Pod from the YAML manifest. What’s the status of the Pod?
```
```

Edit the Pod named loop. Change the command to run in an endless loop. Each iteration should echo the current date.
```
```

Inspect the events and the status of the Pod loop.

```
```
Delete the “Delete the namespace ckad and its Pods.
```
```
