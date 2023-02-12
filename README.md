# Ingress Controller redirecionando entre 2 pods

Vamos instalar o Ingress em nosso cluster (https://kubernetes.github.io/ingress-nginx/deploy/)

````sh
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.5.1/deploy/static/provider/cloud/deploy.yaml
````

Para visualizar os status, vamos rodar o comando abaixo:

````sh
kubectl get pods --namespace=ingress-nginx --watch
````

Quando o Ingress estiver com o status `running`, vamos criar nossos dois pods:

pod-ingress-nginx.yaml<br>
pod-ingress-apache.yaml

Aplique os pods no cluster:

````sh
kubectl apply -f pod-ingress-nginx.yaml
kubectl apply -f pod-ingress-apache.yaml
````

Com isso, vamos criar os serviços ClusterIP que os pods necessitam:

service-pod-ingress-nginx.yaml<br>
service-pod-ingress-apache.yaml

Aplique os arquivos de criação dos serviços no cluster

````sh
kubectl apply -f service-pod-ingress-nginx.yaml
kubectl apply -f service-pod-ingress-apache.yaml
````

Vamos criar e aplicar o Ingress:

ingress.yaml

````sh
kubectl apply -f ingress.yaml
````

Como não temos um LoadBalancer criado em nosso cluster, precisamos fazer com que o kubectl mapeie uma porta na nossa máquina e redirecione todas as chamadas para o ingress:

````sh
kubectl port-forward --namespace=ingress-nginx service/ingress-nginx-controller 8080:80
````

Pelo navegador vamos entrar nos endereços http://localhost:8080/apache e http://localhost:8080/nginx<br>
Com isso temos o redirect para os pods de acordo com a rota.