# Материалы мастер-класса

Этот документ: https://k8s.grahovac.pro

Канал для письменного общения: https://t.me/k8smeetup

# Авторы

### Игорь Должиков

Ловите Игоря оффлайн :)

### Елена Граховац

- hello@grahovac.pro
- https://twitter.com/webdeva
- https://github.com/rumyantseva

## Чек-лист

- Go: https://golang.org/dl/
- IDE или любой редактор для написания кода
- Docker CE: https://www.docker.com/community-edition#/download
- Если у вас Windows: Cygwin - https://cygwin.com/install.html
- GitHub-аккаунт: https://github.com
- Любой клиент для работы с Git

## Доступность кластера

- Кластер будет доступен в течение недели после мастер-класса,
с ним можно свободно экспериментировать
- Если вы хотите, чтобы мы удалили все ваши данные немедленно,
сообщите об этом по почте: hello@grahovac.pro или лично

## Дополнительные материалы

- http://gowayfest.grahovac.pro
- http://highload.grahovac.pro

## Пример готового сервиса

- https://github.com/rumyantseva/go-sofia

## Сервис, который мы напишем вместе

- https://github.com/rumyantseva/nsk

## Проверка готовности

Главная страница мастер-класса: https://ui.k8s.community
Kubernetes Dashboard: https://dash.k8s.community

## Hello, Kubernetes

```
USER=your_github_user
kubectl run hello-app --image=gcr.io/google-samples/hello-app:1.0 --port=8080 -n ${USER}
kubectl expose deployment hello-app -n ${USER}
```

Пример конфигурации ingress:

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-app-ingress
  namespace: rumyantseva
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/rewrite-target: "/"
spec:
  rules:
  - host: services.k8s.community
    http:
      paths:
      - path: /rumyantseva/hello
        backend:
          serviceName: hello-app
          servicePort: 8080
  tls:
  - hosts:
    - services.k8s.community
    secretName: tls-secret
```

```
kubectl apply -f ingress.yaml
```

## Собираем и запускаем докер-образ локально

```
docker build -t pushwoosh .
docker run -p 8080:8080 -p 8585:8585 -t pushwoosh
```

## Программа-минимум

- Добавить https://github.com/apps/k8s в свой репозиторий
- Добавить чарты и Makefile (простейший готовый пример - https://github.com/rumyantseva/go-sofia)
- В Makefile обязательно должна быть переменная `RELEASE?=`
с номером версии приложения
- По пушу в мастер изменения подхватываются сервисом CI/CD
и производится попытка релиза

## Программа-максимум

- Добавить тесты
- Добавить проверку линтерами
- Добавить бизнес-логику (например, работу с API Kubernetes)

## Дополнительные материалы

- HighLoad++ 2018: “Лучшие практики нативных облачных сервисов”: [видео](https://youtu.be/MZeXPeyeF6U) и [слайды](https://docs.google.com/presentation/d/1eWJ5eAWEYcLIVVcGdfekKRoBoLANgDolOoP919qKceg/edit#slide=id.p) 
- GoWayFest 2018: “Best Practices for Cloud-Native Go Services” (больше Go-ориентированных примеров): [видео](https://youtu.be/rRSkbOMTHLw) и [слайды](https://docs.google.com/presentation/d/180D-2HcsJjFTYoOzLs_xLFmEZT_OovZ8RQKPmkmVr1c/edit#slide=id.p)
