# verdura
cosas para Ionic

ibmcloud cr token-add --description "la descripcion" --non-expiring -q

kubectl --namespace api-afip create secret docker-registry soa-secret --docker-server=registry.ng.bluemix.net --docker-username=token --docker-password=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI1OGI0MWE2ZS00ZjViLTU1MzgtYjZlNy1mNGFlNjMzNmIwZTciLCJpc3MiOiJyZWdpc3RyeS5uZy5ibHVlbWl4Lm5ldCJ9.6siYFXm5Ss6XvKfVJ4144WAMk7JIWxCKcHS9H7I6Xwc --docker-email=ppedraza.sofrecom@supervielle.com.ar

eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI2Mzg5ODkxZC00YjllLTUxMmUtYTFiNi0yYmFiNjZhZDczMTkiLCJpc3MiOiJyZWdpc3RyeS5uZy5ibHVlbWl4Lm5ldCJ9.CA4FishqKQWt9e-QkerO1yU9OeuL_-CZom6Ba7YaSWo


## Obtener los pods de tu namespace
```
kubectl --namespace=<nombre del namespace> get pods
```
- Ejemplo:

```
kubectl --namespace=api-afip get pods

```

## Entrar al shell dentro de un container de tu namespace
```
kubectl --namespace=<nombre del namespace> exec -it <nombre del pod> --container <nombre del container> -- /bin/bash
```
- Ejemplo:

```
kubectl --namespace=api-afip exec -it api-afip-5d947b5b77-7ww8v --container api-afip -- /bin/bash
```


## Copiar archivos dentro de un container de tu namespace
```
kubectl cp <archivo a copiar> <nombre del namespace>/<nombre del pod>:/<ruta dentro del container> -c <nombre del container>
```
 - Ejemplo:

```
kubectl cp README.md api-afip/api-afip-5d947b5b77-7ww8v:/tmp/artifacts -c api-afip
```


Deployo el servicio
$ kubectl apply -f ./deployment.yaml

```
apiVersion: apps/v1beta2
kind: Deployment
metadata:
  name: tomcat-deployment
spec:
  selector:
    matchLabels:
      app: tomcat
  replicas: 1
  template:
    metadata:
      labels:
        app: tomcat
    spec:
      containers:
      - name: tomcat
        image: tomcat:9.0
        ports:
        - containerPort: 8085
```

Conocer el deployment name
```
$ kubectl --namespace=api-afip get deployments
```
Exponer puertos del pod al mundo exterior
```
$ kubectl expose deployment tomcat-deployment --type=NodePort
```

Para ver por que puerto fue expuesto el servicio al mundo exterior ejecuto:
```
$ minikube service tomcat-deployment --url
$ kubectl describe pod <nombre del pod>
```

Para hacer forward de un puerto
```
$ kubectl port-forward <nombre del pod> 5000:6000
```

## Run command after init
```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: auth
spec:
  replicas: 1
  template:
    metadata:
        labels:
          app: auth
    spec:
      containers:
        - name: auth
          image: {{my-service-image}}
          env:
          - name: NODE_ENV
            value: "docker-dev"
          resources:
            requests:
              cpu: 100m
              memory: 100Mi
          ports:
            - containerPort: 3000
          lifecycle:
            postStart:
              exec:
                command: ["/bin/sh", "-c", {{cmd}}]
```

Otro ejemplo
```
lifecycle:
      postStart:
        exec:
          command:
          - "cp"
          - "/app/myapp.war"
          - "/work
```

## ESTA ES LA QUE VA
```
============== Ingress =====================
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: api-afip-ingress
 namespace: api-afip
spec:
 rules:
 - host: api-afip.spv-desa-east.mon01.containers.appdomain.cloud
   http:
     paths:
     - backend:
         serviceName: api-afip
         servicePort: 7800
       path: /
 - host: web-api-afip.spv-desa-east.mon01.containers.appdomain.cloud
   http:
     paths:
     - backend:
         serviceName: api-afip
         servicePort: 4414
       path: /
============== Service =====================
apiVersion: v1
kind: Service
metadata:
 name: api-afip
 namespace: api-afip
spec:
 ports:
 - name: webui
   port: 4414
   protocol: TCP
   targetPort: 4415
 - name: serverlistener
   port: 7800
   protocol: TCP
   targetPort: 7800
 - name: nodelistene:qr
   port: 7080
   protocol: TCP
   targetPort: 7080
 selector:
   app: api-afip
 sessionAffinity: None
 type: ClusterIP
===================== Deployment ==================
apiVersion: extensions/v1beta1
   kind: Deployment
   metadata:
     name: api-afip
     namespace: api-afip
   spec:
     replicas: 1
     selector:
       matchLabels:
         app: api-afip
     template:
       metadata:
         labels:
           app: api-afip
       spec:
         containers:
         - name: api-afip
           image: registry.ng.bluemix.net/api-afip/afip_api:1.0
           env:
             - name: LICENSE
               value: accept
             - name: NODENAME
               value: MYNODE
             - name: SERVERNAME
               value: MYSERVER
           resources:
             limits:
               cpu: 2
               memory: 2Gi
             requests:
               cpu: 1
               memory: 512Mi
           terminationMessagePath: /dev/termination-log
           terminationMessagePolicy: File
         dnsPolicy: ClusterFirst
         restartPolicy: Always
         schedulerName: default-scheduler
         securityContext: {}
         terminationGracePeriodSeconds: 30
           ports:
           - containerPort: 7800
             name: serverlistener
             protocol: TCP
           - containerPort: 7080
             name: nodelistener
             protocol: TCP
           - containerPort: 4414
             name: webui
             protocol: TCP
           lifecycle:
             postStart:
               exec:
                 command:
                   - "echo"
                   - "hola que tal"
         imagePullSecrets:
           - name: api-afip
```
