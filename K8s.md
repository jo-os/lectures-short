# Kubernetes
**Kubernetes (K8s)** — это открытое программное обеспечение для автоматизации развёртывания, масштабирования и управления контейнеризированными приложениями.

**Kubernetes (k8s)** — фреймворк для запуска и управления приложениями в среде контейнеров.
- Объединяет несколько серверов в кластер с единым управлением и единым хранилищем конфигураций.
- Для запуска контейнеров использует docker (или другие).
- Основные принципы — это декларативный подход к описанию и идемпотентность.

Все объекты в kubernetes могут быть представлены в виде yaml или json файлов.

**Yaml — (Yet Another Markup Language)** язык разметки, совместимый с json. Использует отступы и отсюда считается более читабельным.

Основные параметры верхнего уровня:
- apiVersion - версия апи,
- kind - тип объекта,
- metadata - метаданные для идентификации объекта,
- spec - параметры объекта,
- status - хранит текущий статус объекта (не указывается при создании).

**Namespace** — пространство имен. Используется для разделения объектов по неким критериям. Например, по окружению (prod, staging, ...) или по принадлежности к проекту (frontend, backend, ...)

Объекты условно можно разделить на:
- namespaced — привязанные к namespace,
- cluster-level — глобальные объекты.

**Labels** — лейблы. С их помощью реализован поиск объектов. И как следствие, их модификация. Если имя объекта динамическое и заранее невозможно его знать, то поиск таких объектов осуществляется с помощью лейблов.

**Примитивы Kubernetes**

**Pod** — минимальная единица рабочей нагрузки.
- В большинстве случаев: pod = контейнер
- Однако, возможно создание нескольких контейнеров в рамках одного пода.

**ReplicaSet** задает желаемую конфигурацию репликации и шаблон пода. Kubernetes будет стараться поддерживать это состояние и в случае удаления или остановки пода.

**Job** — разовый запуск пода. Обычно используется для бутстрапа или миграций.

**CronJob** — Запуск Job по расписанию.

**Deployment** задает шаблон replicaset и занимается выкаткой приложений. При изменении объекта создается новый replicaset, и одновременно со старым они начинают апскейлиться и даунскейлиться соответсвенно.

**DaemonSet** ведет себя аналогично деплойменту. Но вместо количества реплик daemonset поднимает по 1 поду на каждой ноде. Используется для приложений, которые необходимо держать на каждой ноде, например, node-exporter и т.п.

**StatefulSet** — используется для деплоя кластерных приложений. Каждый под в нем имеет фиксированное имя, на которое можно сослаться в конфиге.

**ConfigMap** — используется для хранение конфигурации. Можно хранить как переменные окружения, так и файлы конфига целиком.

**Secret** — используется для хранения любой чувствительной информации (пароли, ключи и.т.п). В etcd хранится в зашифрованном виде. Может иметь один из нескольких типов:
- Opaque
- kubernetes.io/service-account-token
- kubernetes.io/dockercfg
- kubernetes.io/dockerconfigjson
- kubernetes.io/basic-auth
- kubernetes.io/ssh-auth
- kubernetes.io/tlsdata
- bootstrap.kubernetes.io/token

**Service** — используется для направления трафика на под и балансировки в случае, если подов несколько. По умолчанию сетевой доступ будет обеспечен только внутри кластера. Имя сервиса будет использоваться в качестве DNS.

**Ingress** — объект, обрабатываемый ingress контроллером, реализует внешнюю балансировку (обычно по HTTP). - Популярные контроллеры: nginx-ingress, traefik

**Minikube** — компактная версия k8s, созданная для тестирования и разработки в среде k8s. А также в учебных целях. Установка minikube зависит от операционной системы и системы виртуализации.

Полная инструкция доступна на официальном сайте:
```
https://minikube.sigs.k8s.io/docs/start/
```
Запуск: minikube start

**k3s** — аналог minikube, эмулирующий k8s. Запускается с помощью одного бинарного файла.

Установка возможна на linux с помощью bash скрипта:
```
curl -sfL https://get.k3s.io | sh -
```
Полная инструкция по установке доступна по ссылке:
```
https://rancher.com/docs/k3s/latest/en/quick-start/
```
Запуск: service k3s start

**Работа с кластером**

**Kubectl** — основная утилита для работы с k8s кластером. Использует конфигурацию, расположенную в файле ~/.kube/config Переопределить расположение этого файла можно с помощью переменной KUBECONFIG.

k3s имеет встроенный kubectl и может использоваться как:
```
k3s kubectl ...
```
Отдельно эту утилиту можно установить по инструкции:
```
https://kubernetes.io/ru/docs/tasks/tools/install-kubectl/
```
Общий синтаксис:
```
kubectl действие объект флаги
```
Например:
```
kubectl get pods -n kube-system
```
**Основные действия:**
- get
- create
- edit
- delete
- describe
- apply
  
**Объекты:**
- pods
- deploy
- svc
- secrets
  
**Флаги** могут различаться для различных команд, из основных необходимо указывать **namespace**:
- -n
- --all-namespaces

Создаем файл: nginx.yaml

Деплоим с помощью:
```
kubectl apply -f nginx.yaml
```
Смотрим за статусом:
```
kubectl get po
```
Смотрим подробный статус пода:
```
kubectl describe po имя_пода
```
<details>
  <summary>Deployment.yml</summary>
  
```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```
</details>

Аналогом docker logs в k8s является команда kubectl logs
Пример:
```
kubectl logs --tail 100 имя_пода
```
Для того чтобы выполнить внутри пода какую-либо команду, используется:
```
kubectl exec -it имя_пода команда
```
Пример:
```
kubectl exec -it nginx-ans2l bash
```
Документация на русском:

https://kubernetes.io/ru/docs/home/

# Kubernetes 2
**Архитектура k8s**

**Компоненты:**

**Сервер API** — компонент панели управления Kubernetes, который представляет API Kubernetes

**API-сервер** — это клиентская часть панели управления Kubernetes. Основной реализацией API-сервера Kubernetes является kube-apiserver. kube-apiserver предназначен для горизонтального масштабирования — развёртывание на несколько экземпляров. Вы можете запустить несколько экземпляров kube-apiserver и сбалансировать трафик между этими экземплярами

**etcd** — распределённое и высоконадёжное хранилище данных в формате «ключ-значение», которое используется, как основное хранилище всех данных кластера в Kubernetes

**kube-controller-manager** — компонент k8s, который запускает процессы контроллера:
- контроллер узла (Node Controller): уведомляет и реагирует на сбои узла
- контроллер репликации (Replication Controller): поддерживает правильное количество подов для каждого объекта контроллера репликации в системе
- контроллер конечных точек (Endpoints Controller): заполняет объект конечных точек (Endpoints), то есть связывает сервисы (Services) и поды (Pods)
- контроллеры учётных записей и токенов (Account & Token Controllers): создают стандартные учётные записи и токены доступа API для новых пространств имён 

**kube-scheduler** — компонент k8s, который отслеживает созданные поды без привязанного узла и выбирает узел, на котором они должны работать При планировании развёртывания подов на узлах, учитывается множество факторов, включая требования к ресурсам, ограничения, связанные с аппаратными или программными политиками, принадлежности (affinity) и непринадлежности (anti-affinity) узлов или подов, местонахождения данных, предельных сроков

**kube-proxy** — сетевой прокси, работающий на каждом узле в кластере и реализующий часть концепции сервис kube-proxy конфигурирует правила сети на узлах. При помощи них разрешаются сетевые подключения к вашими подам изнутри и снаружи кластера kube-proxy использует уровень фильтрации пакетов в операционной системе, если он доступен. В другом случае kube-proxy сам обрабатывает передачу сетевого трафика

**kubelet** — агент, работающий на каждом узле в кластера. Он следит за тем, чтобы контейнеры были запущены в поде Утилита kubelet принимает набор PodSpecs и гарантирует работоспособность и исправность определённых в них контейнеров. Агент kubelet не отвечает за контейнеры, не созданные Kubernetes

**Сетевые плагины**

Для реализации сетевого взаимодействия используются сетевые плагины — CNI

**CNI (Container Network interface)** представляет собой спецификацию для организации универсального сетевого решения для Linux-контейнеров. Он включает в себя плагины, отвечающие за различные функции при настройке сети пода. Плагин CNI — это исполняемый файл, соответствующий спецификации

Одной из главных ценностей CNI, конечно, являются сторонние плагины, обеспечивающие поддержку различных современных решений для Linux-контейнеров. Среди них:
- Project Calico
- Weave
- Flannel

**Установка kubeadm, kubelet, kubectl**

Устанавливаем зависимости для apt:
```
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl
```
Настраиваем репозиторий:
```
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg
https://packages.cloud.google.com/apt/doc/apt-key.gpg
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg]
https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee
/etc/apt/sources.list.d/kubernetes.list
```
Устанавливаем пакеты:
```
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
```
***Создание кластера***

Инициализируем первую мастер ноду:
```
kubeadm init
```
Если всё прошло успешно, то в консоль выведется команда для присоединения нод к кластеру, выполняем её:
```
kubeadm join <control-plane-host>:<control-plane-port> --token <token>
--discovery-token-ca-cert-hash sha256:<hash>
```
Устанавливаем CNI, например weave:
```
kubectl apply -f
https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml
```
**Helm** — пакетный менеджер k8s. Позволяет объединять несколько yaml-файлов и шаблонизировать их с помощью встроенных функций и параметров. Такой пакет называется Chart. Имеет функционал отката релиза, тестирования, хранит текущее состояние релиза внутри кластера

Установка Helm производится с помощью скрипта установки:
```
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
```
Helm можно использовать для установки чартов из публичных репозиториев:
```
helm repo add stable https://charts.helm.sh/stable
helm search repo wordpress
helm install my-wordpress stable/wordpress
```
И для создания, и деплоя своих собственных чартов:
```
helm create mychart
```
Структура в директории с чартом должна иметь вид:
```
mychart/
  Chart.yaml
  values.yaml
  templates/
```
- Chart.yaml содержит метаданные о самом чарте — название, версия, автор и т. д.
- values.yaml — значения для установки по умолчанию
- templates/*.yaml — те объекты, которые будут загружены при установке чарта

**Chart.yaml**
- apiVersion — указывает версию API для helm
- name — название чарта, именно оно будет фигурировать в поиске при загрузке чарта в репозиторий
- version — версия чарта
- appVersion — версия приложения внутри чарта, они не всегда могут совпадать
```
apiVersion: v2
name: frontend-chart
description: A Helm chart for Kubernetes
type: application
version: 0.1.0
appVersion: 0.1.0
```
**values.yaml**
- В этом файле хранятся любые параметры, которые потребуется переопределять в разных инсталляциях
- Хорошим тоном в публичных чартах считается иметь работоспособные параметры по умолчанию
```
replicas: 2
database:
host: localhost
port: 5432
users:
- root
- test
- web
```
**пример шаблона yaml-файла**
- Создадим в папке templates configmap.yaml с подобным содержанием
- Helm имеет встроенные объекты, например, Release.Name. Полный список есть в официальной документации
- Использование параметров из values.yaml возможно с помощью {{ .Values. }}
```
apiVersion: v1
kind: ConfigMap
metadata:
name: {{ .Release.Name }}-configmap
data:
myvalue: "Hello World"
test: {{ .Values.replicas | quote }}
```
**Helm: деплой проекта**

Установим наш чарт
```
helm install mycahart ./mychart
```
Проверим его манифест
```
helm get manifest mycahart
```
Удалим чарт
```
helm uninstall mycahart
```

# Kubernetes 3

Оркестрация - процесс автоматического развертывания и управления контейнерами
- Docker swarm (не хватает функий для сложных приложений)
- Kubernetes (самый популярный, не сложный и многофункциональный)
- Mesos (сложный, но продвинутый)

**Архитектура**
- **Noda** эта машина физическая или виртуальная на которой установлен кубернетес 
- **Вокер нода** это компьютер на котором кубернетес будет запускать контейнеры
- **Кластер** это набор сгруппированых вместе узлов, таким образом даже если один узел падает то наше приложение по-прежнему доступно для пользователей, более того наличие нескольких nod также помогают распределению нагрузки
- **Мастер** это еще одна **нода** член кубернетес сконфигурированая как мастер. У нее особые функции наблюдать за состоянием других нод и быть ответственным за оркестрацию контейнеров на воркер нодах

Компоненты:
- **API SERVER** - frontend - единый интерфейс Kubernetes, все общаются с api server
- **CONTAINER RUNTIME** - среда выполнения контейнера - базовое ПО используется для запуска контейнеров
- **CONTROLLER** - мозг оркестрации. Они смотрят за состоянием нод, контейнеров, endpoint-ов и ответственны за реакцию за события на нодах. Принимают решения о создании новых контейнеров.
- **SCHEDULER** - ответственнен за распределение работ или контейнеров между нодами, ожидает появления контейнера, чтобы назначить его выполнение на ноду
- Служба **KUBELET** - агент который работает на каждом узле кластера, агент отвечает за то чтобы контейнеры работали на узлах должным образом
- Служба **ETCD** - хранилище ключ-значение, распределенная надежная БД используемая kubernetes для хранения информации нужной для управления кластера. Когда в кластере несколько докер нод, несколько мастеров ETCD хранит свою информацию на всех нодах кластера распределенным образом и отвечают за реализацию блокировок, не допуская конфликты между мастерами.

Отличия worker и master

**Worker нода** или миньон это сервер где размещены контейнеры, например докер контейнер. Для запуска контейнеров докер в системе должна быть установлена среда выполнения контейнера в данном случае это докер, но вовсе не обязательно должен быть он есть и другие контейнер runtime такие как rkt или cri-o 

Minikube - одноузловой кластер для тестов

https://kubernetes.io/ru/docs/tasks/tools/install-kubectl/

Если установить kubectl перед миникуб то не нужно будет заниматься его привязкой к кластеру миникуб это сделает за нас

**kubectl** - Инструмент командной строки Kubernetes позволяет запускать команды для кластеров Kubernetes. Можно использовать kubectl для развертывания приложений, проверки и управления ресурсов кластера, а также для просмотра логов. 

**kubectl** — инструмент командной строки, используемый для отправки команд на кластер через сервер API.

https://kubernetes.io/ru/docs/tasks/tools/install-minikube/

Kubeadm необходимо
- Docker
- kubeadm — инструмент командной строки, который устанавливает и настраивает различные компоненты кластера стандартным образом.
- Инициализация мастера
- POD Network
- Присоединение рабочих узолов к главному

**Простые абстракции**
- Кубернетес не развертывает контейнеры непосредственно в ноды он инкапсулирует их в объекты которые называются поды
- **Под** это отдельный экземпляр приложения, также под это самый маленький объект который можно создать в кубернетес, самая маленькая деталь этого конструктора
- В поде обычно один контейнер, в котором выполняются приложение, а чтобы отмасштабироваться вверх надо создавать новые поды, а чтобы соскелиться вниз удалить существующие. Не стоит добавлять дополнительные контейнеры в существующие ноды для масштабирования приложения
- В поде обычно один контейнер, но есть ли ограничение на запуск нескольких контейнеров в одном поде? Нет в одном поде может быть несколько контейнеров, но эти контейнеры не должны быть одного типа. В некоторых случаях требуется создать контейнер помощник который нес бы функции поддержки приложения из основного контейнера, такие как предпроцессинг введенных пользователем данных, обработка файлов загруженных пользователям или фиксация поведения приложения, главное требование для таких контейнеров, что время их жизни совпадает с продолжительностью работы основного контейнера с приложением. В этом случае оба контейнера буду частью одного пода и когда будет создан новый контейнер приложения, вместе будет создан помошник, а когда контейнер приложения умрет помошник тоже будет уничтожен вместе с подом.
```
kubectl run nginx --image nginx - ставим nginx
kubectl get pods - смотрим доступные поды
kubectl describe pod nginx - подробная информация
kubectl get pods -o wide - польная информация от get pods
```

**YAML для Kubernetes**

<details>
  <summary>Pod.yml</summary>
  
```yml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
    type: frontend
spec:
  containers:
  - name: nginx-container
    image: nginx
#  - name: busybox - как можно создать второй контейнер
#    image: busybox
```
</details>
  
```
kubectl create (apply) -f pod.yml - kubernetes создаст под
```

Манифест всегда содержит
- **apiVersion** - это версия api kubernetes который мы используем для создания объекта. В зависимости от того что мы пытаемся создать мы должны указать правильный apiVersion (для pod - v1)
- **kind** - kind относится к типу объекта которые мы пытаемся создать в данном случае pod
- **metadata** - метаданные это данные об объекте такие как его имя метки и так далее. В отличие от первых двух имеющих строковое значение это словари. Все что относится к метаданным будет являться дочерними элементами, то есть будут сдвинуты по правилам yml. Labels - словари. Type - для отслеживания
- **spec** - спецификация - в зависимости от объекта который мы собираемся создать мы предоставляем kubernetes дополнительную информацию относящуюся к этому объекту. Она разная для разных типов объектов, поэтому важно запомнить или обратиться к разделу документации чтобы указать правильный формат.

Это самый верхний или корневой уровень свойств. Все эти поля обязательны поэтому они должны присутствовать в файле конфигурации 
```
Kind  -  Version
Pod  -  v1
Service  -  v1
ReplicaSet - apps/v1
Deployment - apps/v1
```

**ReplicaSets**

**ReplicationController** (устарел, теперь используем ReplicaSet) - контроллер репликации помогает нам запускать несколько экземпляров под в кластере kubernetes тем самым обеспечивая высокую доступность. Контроллер репликации гарантируют что указанное количество исправных под будет постоянно работать - будь это всего лишь один под или сотня. Другая причина его использование распределение нагрузки между несколькими подами.
<details>
  <summary>ReplicationController old</summary>

```yml
apiVersion: v1
kind: ReplicationController
metadata:
  name: myapp-rc
  labels:
    app: myapp
    type: frontend
spec:
  template: #Pod - берем из варианта с Pod
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        tier (type?): frontend
    spec:
      containers:
      - name: nginx-container
        image: nginx
  replicas: 3
```
</details>

```
kubectl create -f rcontroller.yml
kubectl get replicationcontroller
kubectl get pods
```
Важное отличие между ReplicationController и ReplicaSet - для ReplicaSet требуется определение селектора. Раздел селектор помогает ReplicaSet понять какие поды ему принадлежат. Потому что реплика сет также может управлять подами которые не были созданы как часть ReplicaSet
<details>
  <summary>Replicaset</summary>
  
```yml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-rc
  labels:
    app: myapp
    type: frontend
spec:
  template: #Pod - берем из варианта с Pod
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        tier (type?): frontend
    spec:
      containers:
      - name: nginx-container
        image: nginx
  replicas: 3
  selector:
    matchLabels:
      type: frontend
```
</details>
  
```
kubectl create -f rset.yml
kubectl get replicaset
kubectl get pods
```
Надо больше реплик - меняем replicas: 6 в rset.yml и запускаем repalce:
```
kubectl replace -f rset.yml - обновит набор реплик до требуемого значение 
```
или
```
kubectl scale --replicas=6 -f rset.yml
kubectl scale --replicas=6 -f myapp-rc - по имени! но не сохранит все в файл! при перезапуске будет запущено что было ранее
```
Редактирование в системе
```
kubectl edit rs myapp-rc - откроет копию в vi и после редакции проверит на ошибки, перепишет файл и применить (например запустит доп ноды)
```
Удаление
```
kubectl delete replicaset rset.yml
```
**Deployment**

После обновления важных компонетов, нам нужен процесс который обновит контейнеры один за другим, такое обновление называется rollin update. Предположим что одно из выполненных нами обновлений привело к непредвиденной ошибке и нас попросят отменить последнее действие, нам нужна возможность откатиться от недавно внесенных изменений, наконец если нам требуется внести изменения в свою среду например пропатчить ОС ноды или изменить распределение ресурсов вкупе с масштабированием или какие то другие вещи и мы не хотим применять каждое изменение сразу после исполнения команды, а предпочитаем поставить нагрузку на паузу и произвести требуемые изменения, а затем возобновить работу чтобы все изменения развертывались вместе все эти возможности доступны в **deployment**

Deployment выше в иерархии и обладает всем свойствами pod и replicaset
<details>
  <summary>Deployment</summary>
  
```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
  labels:
    app: nginx
    tier: frontend
spec:
  selector:
    matchLabels:
      app: myapp
  replicas: 4
  template:
    metadata:
      name: nginx2
      labels:
        app: myapp
    spec:
      containers:
      - name: nginx
        image: nginx
```
</details>

```
kubectl create -f deploy.yml --record  - создание (--record - добавляет в history причину изменения)
kubectl get deployments - инфо
Deployment автоматически запустит replicaset поэтому можно запустить :
kubectl get replicaset
kubectl get pods
kubevtl get all - компнда покажет все deploy,rs,po
kubectl describe deployment myapp-deployment - выведет описание deployment
```
**Обновление и откат**

Каждый раз при создании нового deployment или обновления образа существующим запускается rollout - это процесс постепенного развертывания или обновления контейнеров приложения. Rollout создает ревизию развертывания, его отпечаток snapshot назовем его revision 

1. В будущем когда приложение будет обновлено то есть когда версия контейнера будет меняться на новую запустится новый rollout который создаст новую ревизию с именем revision

2. эта функция помогает нам отслеживать изменения внесенные в deployment и позволяет при необходимости вернуться к предыдущей развернутой версии.
```
kubectl rollout status deploy myapp-deployment - состояние выкатки
kubectl rollout history deploy myapp-deployment - просмотр списка ревизий и истории изменения
```
Стратегия Recreate - убиваем все контейнеры, потом поднимаем все контейнеры - есть время простоя

Rolling update - убиваем и поднимаем по одному контейнеру (стандартная стратения deployment)

Разницв Recreate и Rolling update видна через (по созданию, убийству контейнеров)
```
kubectl describle deployments.apps myapp-deployment
```

**Обновление версии**
```
меняем в манифесте image: nginx: 1.7.1
kubectl apply -f deploy.yml

kubectl set image deployment/myapp-deployment nginx=nginx:1.7.1 - обновления образа, но в манифесте останется старое
```
Deployment при обновлении создает еще один replicaset2 и убивает поды в replicaset1, запускает их в replicaset2.

Если надо откатить изменения:
```
kubectl rollout undo deployment/myapp-deployment
```
**Команды:**
```
kubectl create -f deploy.yml - создание
kubectl get deployments - инфо
kubectl apply -f deploy.yml - принятие изменений
kubectl set image deployment/myapp-deployment nginx=nginx:1.7.1 - изменения
kubectl set deployments myapp-deployment --replicas=2 - изменения
kubectl rollout status deployment/myapp-deployment - состояние выкатки
kubectl rollout history deployment/myapp-deployment - просмотр списка ревизий и истории изменения
kubectl rollout undo deployment/myapp-deployment - откат
kubectl delete deploy myapp-deployment - удаление
```
**Сетевое взаимодействие**

В кубернетес ip адрес назначается поду (стандарнтые адреса  10.224.0.0) На одном узле все просто - кубернетес поднимает стандартную сеть и поды могут через нее взаимодействовать.

Если на одном узле у нас несколько нод то это не будет работать, если узлы являются частью одного кластера, для подов назначены одинаковые ip это приведет конфликтом адресов в сети. Когда мы занимаемся настройкой кластера kubernetes автоматически не настраивает какие-либо сети чтобы решить эту нашу проблему. Собственно говоря kubernetes ожидает что мы сами настроим сеть удовлетворяющую его базовым требованиям. Основные из этих требований заключаются в том что все контейнеры или поды в кластере kubernetes должны иметь возможность связываться друг с другом без необходимости настройки NAT, все ноды должны иметь возможность связываться с контейнерами и все контейнеры должны иметь возможность связываться с нодами в кластере.

Есть готовые решения - calico, flannel, cilium, weave, vmware nsx и другие

**Services (Службы)**

**NodePort** - когда служба делает доступным внутренний порт через порт на узле

**ClustereIP** - служба создает виртуальный ip адрес внутри кластера, чтобы обеспечить связь между различными службами, такими как набор фронтэндов или бекэндов

**LoadBalancer** - предоставляет балансировщик нагрузки для нашего сервиса, у поддерживаемых облачных провайдеров хорошим примером этого может быть распределение нагрузки между разными серверами в ярусе фронтэнда 

**NodePort:**
- TargetPort - порт в поде (на него служба направляет запрос)
- Port - порт службы
- NodePort (30000 - 32767) - порт на самой ноде (для внешнего доступа к веб серверам)
- 
![service](https://github.com/joos-virt/k8s3/blob/main/service.png)

<details>
  <summary>Service.yml</summary>

```yml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: NodePort
  ports:
    - targetPort: 80 #- если не указан, то предполагается совпадение с port
      port: 80 #- обязательный
      nodePort: 30008 #- если не указан, то выделится автоматически в допустимом диапозоне
  selector: #- указывает на нужный под - берем labels пода - это свяжет службу и под
    app: myapp
    type: frontend
```
</details>

```
kubectl create -f service.yml - создание
kubectl get services(svc)
minikube service myapp-service --url - получаем внешний линк
```
В производственной среде у тебя есть несколько экземпляров приложения, работающих для обеспечения высокой доступности и балансировки нагрузки. В этом случае мы имеем несколько похожих подов на которых работает наше приложение, все они имеют одинаковые метки с ключом и значением фронтенд. Метка используется в качестве селектора при создании службы, таким образом когда служба создается она ищет соответствующие поды с метками и находит их. Затем служба автоматически выбирает все три пода в качестве конечных точек для пересылки внешних запросов поступающих от пользователя. Служба использует для балансировки нагрузки случайный алгоритм. Таким образом servies действует как встроенный балансировщик нагрузки для распределения нагрузки между различными подами

При создании службы у нас нет необходимости делать любую дополнительную конфигурацию. Kubernetes создает службу охватывающие все узлы в кластере и жестко сопоставляет целевой порт с нод портом на всех узлах кластера, таким образом можно получить доступ к своему приложению используя ip адрес любой ноды в кластере и используя тот же номер порта.

Подводя итог любом случае, если это будет один под на одной ноде, несколько подов на одной ноде, несколько подов на нескольких узлах - служба создается точно также без необходимости выполнять какие-либо дополнительные действия во время создания когда поды удаляются или добавляются служба автоматически обновляются, что делает ее очень гибкой и адаптивной. После создания как правило не нужно вносить никаких дополнительных изменений конфигурацию.

**ClustereIP**

Группирует поды вместе и представляет единый интерфейс для обращения к этой группе. Например 3 пода backend (уровень) будет выглядеть как один со своим именем и адресом. Каждый уровень проще масштабировать или перемещать, не влияя на обмен данными между службами.

<details>
  <summary>ClusterIP</summary>

```yml
apiVersion: v1
kind: Service
metadata:
  name: backend
spec:
  type: ClusterIP #- идет под умолчанию, можно не указывать
  ports:
    - targetPort: 80 #- порт приложения
      port: 80 #- порт службы
  selector: #- связываем службу с набором подов - берем из пода
    app: myapp
    type: backend
```
</details>

После создания - служба и поды за ней будут доступны другим подам кластера используя имя службы или ее clusterIp

**LoadBalancer**

Можно использовать в большинством облачных провайдеров - получаем один внешний адрес который пересылает и балансирует трафик на внутренние ноды.

Если ставим локально (VirualBox), то получим аналогию NodePort где служба доступна по порту
<details>
  <summary>Loadbal.yml</summary>
  
```yml
apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
  type: LoadBalancer
  ports:
    - targetPort: 80
      port: 80
  selector: #- связываем службу с набором подов - берем из пода
    app: myapp
    type: frontend
```
</details>
