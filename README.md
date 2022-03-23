# ingress-service-check
~~~
kubectl create deployment web --image=gcr.io/google-samples/hello-app:1.0 -n ingress-nginx
kubectl expose deployment web --type=ClusterIP --port=8080 -n ingress-nginx

kubectl create deployment web2 --image=gcr.io/google-samples/hello-app:2.0 -n ingress-nginx
kubectl expose deployment web2 --port=8080 --type=ClusterIP -n ingress-nginx

vim web-ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    kubernetes.io/ingress.class: "nginx"
spec:
  rules:
    - host: hello-world.info
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: web
                port:
                  number: 8080
          - path: /v2
            pathType: Prefix
            backend:
              service:
                name: web2
                port:
                  number: 8080
~~~
~~~
kubectl create -f web-ingress.yaml -n ingress-nginx
~~~
~~~
Then try to hit browser with "http://hello-world.info/" and "http://hello-world.info/v2" with controller Address IP  like 10.10.10.223 
~~~
~~~
root@master:~# kubectl get ing -n ingress-nginx                     
NAME              CLASS    HOSTS              ADDRESS        PORTS   AGE
example-ingress   <none>   hello-world.info   10.10.10.223   80      34d
~~~
