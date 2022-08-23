# Дипрломный проект по курсу "DevOps для эксплуатации и разработки"

<img width="900" alt="image" src="https://user-images.githubusercontent.com/9394918/167876466-2c530828-d658-4efe-9064-825626cc6db5.png">

## Магазин пельменей - "Momo Store aka Пельменная №2"

**Для реализации проекта использованы**:

- GitLab для управления репозиториями, процессами CI/CD и SAST-анализа кода
- Nexus и Docker Hub для хранения артефактов
- SonarQube для измерения качества программного кода
- Terraform для декларативного управления инфраструктурой с использованием подхода Infrastructure as code
- Kubernetes - платформа оркестрации контейнеризированных приложений
- Helm - пакетный менеджер для Kubernetes
- ArgoCD - инструмент continuous delivery для Kubernetes, позволяющий реализовать подход GitOps
- Grafana, Loki, Promtail, Prometheus для сбора данных и визуализации мониторинга

### Предварительная подготовка для работы с проектом

- Установить [yc](https://cloud.yandex.ru/docs/cli/operations/install-cli)
- Установить [Terraform](https://cloud.yandex.ru/docs/tutorials/infrastructure-management/terraform-quickstart) и настроить провайдер Yandex Cloud
- Установить [kubectl](https://kubernetes.io/ru/docs/tasks/tools/install-kubectl/)
- Установить [helm3](https://helm.sh/docs/intro/install/)
- Установить [Argocd CLI](https://github.com/argoproj/argo-cd) (опционально)

### Установка кластера Managed Kubernetes в Yandex Cloud

Кластер Managed Kubernetes устанавливается с помощью Terraform.
Состояние Terraform хранится в Yandex Object Storage, конфигурация описана в файле backend.conf.
Перед установкой необходимо заполнить файлы **backend.conf** и **secret.auto.tfvars** собственными данными. Примеры заполнения файлов backend.conf и secret.auto.tfvars находятся в файлах **backend.conf.example** и **secret.auto.tfvars.example**.

Порядок выполнения команд Terraform для инициализации кластера Managed Kubernetes:

   ```bash
    terraform init -backend-config=backend.conf
    terraform validate
    terraform plan
    terraform apply
   ```

После успешного выполнения операции в консоль будет выведен идентификатор кластера, который нужно использовать на следующем шаге ("ID_кластера").

После инициализации кластера необходимо:

- сформировать конфигурационный файл для подключения к кластеру с помощью yc

    yc managed-kubernetes cluster get-credentials --id ID_кластера --internal

- установить [NGINX Ingress Controller](https://cloud.yandex.ru/docs/managed-kubernetes/tutorials/ingress-cert-manager) с менеджером для сертификатов Let's Encrypt

### Как происходит работа с репозиторием и обновление приложения

После появления новой версии frontend или backend в результате выполнения пайплайна GitLab собирается новый docker image, Helm-чарт формируется, версионируется и публикуется в Nexus.
ArgoCD настроен на обновление приложений в режиме Auto-Sync и при появлении новой версии Helm-чарта в Nexus обновление произойдет автоматически.

### Helm-чарты

- <https://nexus.praktikum-services.ru/#browse/browse:momo-helm-backend>
- <https://nexus.praktikum-services.ru/#browse/browse:momo-helm-frontend>

### Docker Hub

- <https://hub.docker.com/repository/docker/rybalka1/momo-frontend>
- <https://hub.docker.com/repository/docker/rybalka1/momo-backend>

Ссылка на пельменную:

- <https://momo-store.site/>

Ссылка на мониторинг:

- <https://grafana.momo-store.site>

Работа с другими частями кластера проводится через port-forwarding локально на раборчей станции.

Скриншот пельменной:

## Dumplings store - Пельменная №2

<img width="900" alt="image" src="https://user-images.githubusercontent.com/9394918/167876466-2c530828-d658-4efe-9064-825626cc6db5.png">

## Legacy

> ## Frontend
>
>```bash
>npm install
>NODE_ENV=production VUE_APP_API_URL=http://localhost:8081 npm run serve
>```
>
>## Backend
>
>```bash
>go run ./cmd/api
>go test -v ./...
>```
>
><https://user-images.githubusercontent.com/9394918/167876466-2c530828-d658-4efe-9064-825626cc6db5.png>
