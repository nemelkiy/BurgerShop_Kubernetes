# Технологии разработки программного обеспечения <br>
## Лабораторная работа №2
Cоздание кластера Kubernetes и деплой приложения
- Выполнил: Коломиченко А.Н.
- Группа: МБД 2131
- Задачи работы: Знакомство с кластерной архитектурой на примере Kubernetes, а также деплоем приложения в кластер.

### Манифесты
- deployment.yaml
```    
apiVersion: apps/v1
kind: Deployment
metadata:
  name: burgerapi-deployment
spec:
  replicas: 10
  selector:
    matchLabels:
      app: burgerapi-app
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: burgerapi-app
    spec:
      containers:
        - image: burgerapi:latest
          imagePullPolicy: Never 
          name: burgerapi
          ports:
            - containerPort: 8080
      hostAliases:
      - ip: "192.168.65.2" # The IP of localhost from MiniKube
        hostnames:
        - postgres.local
```
- service.yaml
```
apiVersion: v1
kind: Service
metadata:
  name: burgerapi-service
spec:
  type: NodePort
  ports:
    - nodePort: 31317
      port: 8080
      protocol: TCP
      targetPort: 8080
  selector:
    app: burgerapi-app
```
- Получение адреса для внешнего обращения в minikube (например, из терминала, postman и тд.), а также адреса для обращения к БД

![1](https://user-images.githubusercontent.com/70494282/148702818-bc552a9c-af98-42b3-9e08-0e6d79e5b101.png)

- Список запущенных подов

![2](https://user-images.githubusercontent.com/70494282/148702865-4f33b22f-5760-4c7c-a1e9-65c446d1f008.png)


https://user-images.githubusercontent.com/70494282/148696185-34ff6a3c-cda4-4560-b1b0-9c67d11faf05.mp4

