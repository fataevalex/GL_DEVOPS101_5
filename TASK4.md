## Uptime Robot service testing

### 1.1 Створення тестових контейнерів
Dockerfile:
```shell
cat <<EOF > Dockerfile
FROM nginx:alpine
RUN echo "Version: 1.0.0" > /usr/share/nginx/html/index.html
EOF

cat <<EOF > Dockerfile.v2
FROM nginx:alpine
RUN echo "Version: 2.0.0" > /usr/share/nginx/html/index.html
EOF
```
Будова docker image-ів
```shell
docker build -t ghcr.io/fataevalex/gl_devops101_5/uptime-check:v1.0.0 .
docker build -t ghcr.io/fataevalex/gl_devops101_5/uptime-check:v2.0.0 -f Dockerfile.v2
```
Відправка imag-а в registry

```shell
export CR_PAT=YOUR_TOKEN

echo $CR_PAT | docker login ghcr.io -u USERNAME --password-stdin

docker push ghcr.io/fataevalex/gl_devops101_5/uptime-check:v1.0.0
docker push ghcr.io/fataevalex/gl_devops101_5/uptime-check:v2.0.0
```


### 1.2 Створення Kubernetes-кластера в GCP
```shell
     gcloud auth login
     gcloud projects create --name GLDEVOPS1015
     
     gcloud config set project <YOUR_PROJECT_ID>
     gcloud config get-value project
```
треба підключити billing аккаунт до проекта
```
     gcloud services enable container.googleapis.com
     gcloud services enable compute.googleapis.com

     
     gcloud container clusters create uptime-cluster \
        --zone=europe-central2-a \
        --num-nodes=1 \
        --enable-ip-alias
     gcloud container clusters get-credentials uptime-cluster --zone=europe-central2-a
```

### 1.3 Створення балансувальнка і подів (3 repicas = 100%)
```shell
      kubectl create namespace uptime
      kubectl apply -f k8s/deployment.yaml -n uptime
      kubectl apply -f k8s/service.yaml -n uptime 
      kubectl get svc -n uptime    
```
отрумуюємо публічний ip балансувальника
```shell
     export LB_IP=$(kubectl get svc uptime-service -n uptime -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
  ```
     
```
     echo "http://$LB_IP/"
```
### 1.5 Реєструємо монітор на https://dashboard.uptimerobot.com/monitors

### 1.6 Переіряємо версію пода в динаміці
```shell
     watch -n 1 curl -s "http://$LB_IP/"
```
Застосовуєм canary.yaml (1 replicas v2 + 3 replicas v1 = 25% v2 )
```shell
     kubectl apply -f k8s/canary.yaml -n uptime
```