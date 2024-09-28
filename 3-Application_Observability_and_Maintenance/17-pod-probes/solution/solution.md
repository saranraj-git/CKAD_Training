# Solution

Create the Liveness_cmd.YAML file and add the probes.

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    test: liveness
  name: liveness-exec
spec:
  containers:
  - name: liveness
    image: busybox
    args:
    - /bin/sh
    - -c
    - touch /tmp/healthy; sleep 30; rm -f /tmp/healthy; sleep 600
    livenessProbe:
      exec:
        command:
        - cat
        - /tmp/healthy
      initialDelaySeconds: 5
      periodSeconds: 5
```


Create the initial YAML with the following command.

```
kubectl apply -f Liveness_cmd.yaml
```



After 35 seconds, view the Pod events again:

```
kubectl describe pod liveness-exec
```

At the bottom of the output, there are messages indicating that the liveness probes have failed, and the failed containers have been killed and recreated.

```
Type     Reason     Age                From               Message
----     ------     ----               ----               -------
Normal   Scheduled  57s                default-scheduler  Successfully assigned default/liveness-exec to node01
Normal   Pulling    55s                kubelet, node01    Pulling image "registry.k8s.io/busybox"
Normal   Pulled     53s                kubelet, node01    Successfully pulled image "registry.k8s.io/busybox"
Normal   Created    53s                kubelet, node01    Created container liveness
Normal   Started    53s                kubelet, node01    Started container liveness
Warning  Unhealthy  10s (x3 over 20s)  kubelet, node01    Liveness probe failed: cat: can't open '/tmp/healthy': No such file or directory
Normal   Killing    10s                kubelet, node01    Container liveness failed liveness probe, will be restarted
```

For Checking the liveness using httpGet method, create a Liveness_httpget.yaml file as shown below

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    test: liveness
  name: liveness-http
spec:
  containers:
  - name: liveness
    image: registry.k8s.io/e2e-test-images/agnhost:2.40
    args:
    - liveness
    livenessProbe:
      httpGet:
        path: /healthz
        port: 8080
        httpHeaders:
        - name: Custom-Header
          value: Awesome
      initialDelaySeconds: 3
      periodSeconds: 3
```

Run the command as shown below

```
kubectl apply -f Liveness_httpget.yaml 
```

After 10 seconds, view Pod events to verify that liveness probes have failed 

```
kubectl describe pod liveness-http
```

and the container has been restarted as follows:

```
Events:
  Type     Reason     Age               From               Message
  ----     ------     ----              ----               -------
  Normal   Scheduled  2m46s             default-scheduler  Successfully assigned default/liveness-http to minikube
  Normal   Pulling    2m45s             kubelet            Pulling image "registry.k8s.io/e2e-test-images/agnhost:2.40"
  Normal   Pulled     22s               kubelet            Successfully pulled image "registry.k8s.io/e2e-test-images/agnhost:2.40" in 2m22.969s (2m22.969s including waiting). Image size: 127004766 bytes.
  Normal   Created    4s (x2 over 22s)  kubelet            Created container liveness
  Warning  Unhealthy  4s (x3 over 10s)  kubelet            Liveness probe failed: HTTP probe failed with statuscode: 500
  Normal   Killing    4s                kubelet            Container liveness failed liveness probe, will be restarted
  Normal   Pulled     4s                kubelet            Container image "registry.k8s.io/e2e-test-images/agnhost:2.40" already present on machine
  Normal   Started    3s (x2 over 22s)  kubelet            Started container liveness
```

For more info , please check the K8s docs site:

https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/






