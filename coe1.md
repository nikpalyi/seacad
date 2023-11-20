Create a new Pod named nginx running the image nginx:1.17.10. Expose the container port 80. The Pod should live in the namespace named ckad.
```
kubectl create ns ckad
```
```
kubectl run nginx --image=nginx:1.17.10 --port=80 --namespace=ckad
```
Get the details of the Pod including its IP address.
```
kubectl get pod nginx --namespace=ckad -o wide
```
```
kubectl describe pod nginx --namespace=ckad | grep IP:
```
Create a temporary Pod that uses the busybox image to execute a wget command inside of the container. The wget command should access the endpoint exposed by the nginx container. You should see the HTML response body rendered in the terminal.

You can use the command-line options --rm and -it to start a temporary Pod. The following command assumes that the IP address of the Pod named nginx is 10.1.0.66:
```
kubectl run busybox --image=busybox --restart=Never --rm -it -n ckad \
  -- wget -O- 10.1.0.66:80
```
Get the logs of the nginx container.
```
kubectl logs nginx --namespace=ckad
```

Add the environment variables DB_URL=postgresql://mydb:5432 and DB_USERNAME=admin to the container of the nginx Pod.
Editing live object is not allowed.
```
kubectl delete pod nginx --namesapce=ckad
```
Edit the the file nginx-pod.yaml
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.17.10
    ports:
    - containerPort: 80
    env:
    - name: DB_URL
      value: postgresql://mydb:5432
    - name: DB_USERNAME
      value: admin
```
```
kubectl create -f nginx-pod.yaml --namespace=ckad
```
Open a shell for the nginx container and inspect the contents of the current directory ls -l.
Use the exec command to open an interactive shell to the container:
```
kubectl exec -it nginx --namespace=ckad -- /bin/sh
```
```
ls -l
```
Create a YAML manifest for a Pod named loop that runs the busybox image in a container. 
The container should run the following command: 
for i in {1..10}; do echo "Welcome $i times"; done. 
Create the Pod from the YAML manifest. 
What’s the status of the Pod?
```
kubectl run loop --image=busybox -o yaml --dry-run=client \
  --restart=Never -- /bin/sh -c 'for i in 1 2 3 4 5 6 7 8 9 10; \
  do echo "Welcome $i times"; done' \
  > pod.yaml
```
```
kubectl create -f pod.yaml --namespace=ckad
```
The status of the Pod will say Completed, as the executed command in the container does not run in an infinite loop:
```
kubectl get pod loop --namespace=ckad
```
Edit the Pod named loop. 
Change the command to run in an endless loop. 
Each iteration should echo the current date.

The container command cannot be changed for existing Pods. 
Delete the Pod so you can modify the manifest file and re-create the object:
```
kubectl delete pod loop --namespace=ckad
```
change the yaml file content:
```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: loop
  name: loop
spec:
  containers:
  - args:
    - /bin/sh
    - -c
    - while true; do date; sleep 10; done
    image: busybox
    name: loop
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```
```
kubectl create -f pod.yaml --namespace=ckad
```
Inspect the events and the status of the Pod loop.

```
kubectl describe pod loop --namespace=ckad | grep -C 10 Events:
```
Delete the namespace ckad and its Pods.
```
kubectl delete ns ckad
```
