# Домашнее задание к занятию «Kubernetes. Причины появления. Команда kubectl» - `Мурчин Артем`

### Цель задания

Для экспериментов и валидации ваших решений вам нужно подготовить тестовую среду для работы с Kubernetes. Оптимальное решение — развернуть на рабочей машине или на отдельной виртуальной машине MicroK8S.

------

### Чеклист готовности к домашнему заданию

1. Личный компьютер с ОС Linux или MacOS 

или

2. ВМ c ОС Linux в облаке либо ВМ на локальной машине для установки MicroK8S  

------

### Инструкция к заданию

1. Установка MicroK8S:
    - sudo apt update,
    - sudo apt install snapd,
    - sudo snap install microk8s --classic,
    - добавить локального пользователя в группу `sudo usermod -a -G microk8s $USER`,
    - изменить права на папку с конфигурацией `sudo chown -f -R $USER ~/.kube`.

2. Полезные команды:
    - проверить статус `microk8s status --wait-ready`;
    - подключиться к microK8s и получить информацию можно через команду `microk8s command`, например, `microk8s kubectl get nodes`;
    - включить addon можно через команду `microk8s enable`; 
    - список addon `microk8s status`;
    - вывод конфигурации `microk8s config`;
    - проброс порта для подключения локально `microk8s kubectl port-forward -n kube-system service/kubernetes-dashboard 10443:443`.

3. Настройка внешнего подключения:
    - отредактировать файл /var/snap/microk8s/current/certs/csr.conf.template
    ```shell
    # [ alt_names ]
    # Add
    # IP.4 = 123.45.67.89
    ```
    - обновить сертификаты `sudo microk8s refresh-certs --cert front-proxy-client.crt`.

4. Установка kubectl:
    - curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl;
    - chmod +x ./kubectl;
    - sudo mv ./kubectl /usr/local/bin/kubectl;
    - настройка автодополнения в текущую сессию `bash source <(kubectl completion bash)`;
    - добавление автодополнения в командную оболочку bash `echo "source <(kubectl completion bash)" >> ~/.bashrc`.

------

### Инструменты и дополнительные материалы, которые пригодятся для выполнения задания

1. [Инструкция](https://microk8s.io/docs/getting-started) по установке MicroK8S.
2. [Инструкция](https://kubernetes.io/ru/docs/reference/kubectl/cheatsheet/#bash) по установке автодополнения **kubectl**.
3. [Шпаргалка](https://kubernetes.io/ru/docs/reference/kubectl/cheatsheet/) по **kubectl**.

------

### Задание 1. Установка MicroK8S

1. Установить MicroK8S на локальную машину или на удалённую виртуальную машину.
2. Установить dashboard.
3. Сгенерировать сертификат для подключения к внешнему ip-адресу.

### Решение 1. Установка MicroK8S

Установка по приложенным командам прошла успешно. На скриншоте выполнил команду по включению Dashboard:

![](https://github.com/artmur1/22-01-k8s-kubectl/blob/main/img/22-01-01-01-hw.png)

microk8s status:

![](https://github.com/artmur1/22-01-k8s-kubectl/blob/main/img/22-01-01-02-hw.png)

Добавил внешний IP в файл с данными для формирования сертификатов:

![](https://github.com/artmur1/22-01-k8s-kubectl/blob/main/img/22-01-01-03-hw.png)

Обновил сертификаты:

![](https://github.com/artmur1/22-01-k8s-kubectl/blob/main/img/22-01-01-04-hw.png)

------

### Задание 2. Установка и настройка локального kubectl
1. Установить на локальную машину kubectl.
2. Настроить локально подключение к кластеру.
3. Подключиться к дашборду с помощью port-forward.

### Решение 2. Установка и настройка локального kubectl

Создал файл config со следующим содержимым:

![](https://github.com/artmur1/22-01-k8s-kubectl/blob/main/img/22-01-02-04-hw.png)

Отображение данных при обращении в браузере к внешнему IP адресу и порту:

![](https://github.com/artmur1/22-01-k8s-kubectl/blob/main/img/22-01-02-01-hw.png)

kubectl get nodes:

![](https://github.com/artmur1/22-01-k8s-kubectl/blob/main/img/22-01-02-02-hw.png)

У меня так и никак не получилось пробросить port-forward ни локально, ни внешне. И имя дашборда ставил из полученного через команду kubectl describe nodes compute-vm-2-2-20-hdd-1726210684419 - и так не вышло:

![](https://github.com/artmur1/22-01-k8s-kubectl/blob/main/img/22-01-02-03-hw.png)

kubectl describe nodes compute-vm-2-2-20-hdd-1726210684419:

![](https://github.com/artmur1/22-01-k8s-kubectl/blob/main/img/22-01-02-05-hw.png)

------

### Правила приёма работы

1. Домашняя работа оформляется в своём Git-репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода команд `kubectl get nodes` и скриншот дашборда.

------

### Критерии оценки
Зачёт — выполнены все задания, ответы даны в развернутой форме, приложены соответствующие скриншоты и файлы проекта, в выполненных заданиях нет противоречий и нарушения логики.

На доработку — задание выполнено частично или не выполнено, в логике выполнения заданий есть противоречия, существенные недостатки.
