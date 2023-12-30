# Мониторинг основы

**Мониторинг** — процесс наблюдения и регистрации данных о каком-либо объекте на неразрывно примыкающих друг к другу интервалах времени, в течение которых значения данных существенно не изменяются

**Источники данных**
- Нагрузка на HDD, CPU, RAM у серверов и рабочих станций
- Очереди на запись / обработку данных
- Статус выполнения ПО
- Скорости обмена на портах коммутаторов
- Контрольные суммы на различные файлы
- Файлы логов, параметры в конфигурациях
- Статусы цифровых и аналоговых датчиков
- Состояния баз данных и целостность файлов
- Доступность сервисов

**Типы мониторинга**

Агентно-ориентированный - Agent-based -  Получение данных от специального программного обеспечения (агента), установленного на наблюдаемом объекте (операционной системе)
- Возможности - Более глубокое видение ситуации относительно состояния наблюдаемого объекта / Способность влиять на ситуацию
- Ограничения - Зависимость от типа агента: открытый код или закрытое решение

Безагентный - Agentless - Считывание данных с интерфейсов, предоставляющих данные через выполнение каких-либо команд на наблюдаемом хосте, без необходимости установки туда отдельного программногообеспечения (агента)
- Возможности - Использование агента (интерфейса обмена данными), который встроен в наблюдаемую систему / Встроенные агенты не требуют дополнительной установки, настройки и лицензирования
- Ограничения -  Нужно делиться учётными данными / Когда мониторинг не работает, данные не собираются

**Выбор типа мониторинга**
- Агентно-ориентированный - Поставить специальное ПО, затратив силы и ресурсы, и получить любую интересующую информацию
- Безагентный - Не ставить ничего, но ограничиться лишь данными, предусмотренными разработчиком протокола

Максимальной эффективности от мониторинга можно достичь через сочетание подходов. 
- Иногда установка агентов не имеет смысла, так как невозможно получить больше информации из наблюдаемого объекта: объект выкатывает все избыточно необходимые метрики по SNMP или нельзя установить дополнительное ПО, например управляемый коммутатор или принтер
- Иногда необходимо мониторить кастомные параметры, которые не представлены ни через один протокол или интерфейс. В этом случае установка агента предоставит ультимативные возможности относительно сбора данных об объекте

**Zabbix**

Решение распределённого мониторинга корпоративного класса с открытыми исходными кодами. Программное обеспечение для мониторинга многочисленных параметров сети, жизнеспособности и целостности серверов, виртуальных машин, приложений, сервисов, баз данных, веб-сайтов, облачных сред и многого другого

Возможности Zabbix - Универсальный мониторинг - Мониторинг любой ИТ-инфраструктуры, облачных ресурсов, сервисов, приложений.

**Источники данных**
- Масштабы решаемых задач варьируются от установки на Raspberry Pi (с целью мониторинга какого-то локального датчика, подключённого к ней же) до десятков тысяч узлов сети, наблюдаемых одним Zabbix-сервером
- Разработка проекта не останавливалась никогда: регулярно выходят новые и LTS-версии
- Агенты доступны почти для любой ОС из коробки. Если нет готового пакета, то можно взять исходники и сделать компиляцию

**Архитектура Zabbix**
У Zabbix есть 4 основных инструмента, с помощью которых можно мониторить определённую рабочую среду и собирать о ней полный пакет данных для оптимизации работы.
- Сервер - Ядро, обрабатывающее в себе все данные системы, включая статистические, оперативные и конфигурацию
- Прокси - Сервис, позволяющий общаться с агентами в удалённой сети так, будто сервер общается с ними напрямую
- Агент - Программа, которая активно мониторит и собирает статистику работы локальных ресурсов и приложений
- Веб-сервис - Позволяет настраивать Zabbix-сервер через веб-интерфейс

**Zabbix Server** - Основная цель Zabbix Server — следить за агентами
- Сам Zabbix Server занимается только сбором и обработкой поступающей от агентов информации
- zabbix_server — демон сервера Zabbix
- zabbix_server.conf — файл конфигурации Zabbix Server
- zabbix_server.log — файл логов Zabbix Server

- Следит за каждым подключённым к нему агентом и знает о них всё
- Заносит в список все новые агенты, обратившиеся к нему впервые, и в дальнейшем следит за ними
- Ищет новые узлы сети, отбирает их по специализированным параметрам и начинает отслеживать, что с ними происходит
- Если Zabbix Server не может следить за узлами сети самостоятельно, он может использовать Zabbix Proxy

**Zabbix Agent** - Основная цель Zabbix Agent — следить за всем остальным
- Сначала агент распознаёт, какие элементы можно мониторить на текущем хосте
- Далее агент передаёт эту информацию на сервер
- Сервер отправляет агенту информацию, за чем конкретно ему следить

**Требования к базам данных для Zabbix**
- Zabbix Server и Zabbix Proxy не могут работать без взаимодействия с БД. При этом только Zabbix Proxy способна работать с SQLite

- CentOS - VM - MySQL InnoDB - 20 узлов
- CentOS - 2 ядра CPU / 2 ГБ - MySQL InnoDB - 500 узлов
- Red Hat Enterprise Linux - 4 ядра CPU / 8 ГБ - RAID10 MySQL InnoDBили PostgreSQL - >1 000 узлов
- Red Hat Enterprise Linux - 8 ядер CPU / 16 ГБ - Быстрый RAID10 MySQL InnoDB или PostgreSQL - >10 000 узлов

**Prometheus**

Это open-source-набор инструментов, предназначенный для оповещения о каких-либо событиях, происходящих на хостах, находящихся у него на мониторинге

**Возможности Prometheus**
- Обработка данных, представленных во временных рядах
- Мониторинг аппаратных ресурсов
- Мониторинг динамичной сервис-ориентированной архитектуры

**История Prometheus**
- Впервые разработан для внутреннего использования проектом SoundCloud
- С момента своего появления в 2012 году Prometheus был взят на вооружение многими крупными компаниями: DigitalOcean, Ericsson, CoreOS, Weaveworks, Red Hat и Google
- Имеет активную команду разработчиков и комьюнити
- Сегодня проект развивается независимо от какой-либо компани

**Принцип работы Prometheus**
- Collector читает таблицу целей (targets), т. е. список объектов, которые надо мониторить, и периодичность их опроса
- После этого collector отправляет HTTP-запрос на каждый эндпоинт и получает ответ с набором из метрик. У каждой метрики есть название, значение и лейблы
- Полученный ответ складывается в базу данных TSDB, где к полученным данным о метрике добавляются отметка времени её получения и лейблы объекта, с которого она была взята
- Service Discovery — встроенный в Prometheus механизм обнаружения сервисов, который позволяет из коробки получать данные для создания таблицы целей

**Сравнение Prometheus с Zabbix**
В отличие от Zabbix, который является законченным продуктом, имеющим из коробки весь базовый необходимый функционал, Prometheus требует доработок:
- интерфейс с графиками и дашбордами — сторонняя разработка, подключаемая к Prometheus
- ограниченная работа Prometheus с оповещениями
- недоступны авторизация и контроль пользователей для Prometheus

Изначально Prometheus рассчитан на активную доработку со стороны эксплуатирующей организации, что делает его более удобным для специфических задач.
- Программисты не ограничены уже существующими механизмами авторизации, способами извлечения данных из приложений, интерфейсом и чем-либо вообще
- Эксплуатирующие подразделения для доведения Prometheus до кондиции могут использовать уже имеющиеся, готовые и работающие решения

- Prometheus — одна из самых популярных систем мониторинга. Оптимальна для мониторинга динамичной контейнерной инфраструктуры
- Prometheus получает данные из специальных экспортеров, которые позволяют ему забирать все собираемые о наблюдаемом объекте данные за одно действие
- С помощью библиотек Prometheus можно добавить возможность мониторинга своего ПО с минимальными трудозатратами со стороны разработчиков

# Zabbix

Zabbix Ansible

https://github.com/ansible-collections/community.zabbix/tree/main/roles

**Установка Zabbix server на Debian 11**

С помощью инструкции с официального сайта установите Zabbix server  на операционную систему:

Установите PostgreSQL:
```
sudo apt install postgresql
```
Добавьте репозиторий Zabbix:
```
wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_6.0-4%2Bdebian11_all.deb 
dpkg -i zabbix-release_6.0-4+debian11_all.deb
apt update
```
Запустите Zabbix server:
```
sudo apt install zabbix-server-pgsql zabbix-frontend-php php7.4-pgsql zabbix-apache-conf zabbix-sql-scripts nano -y # zabbix-agent
```
Создайте пользователя БД:
```
sudo -u postgres createuser --pwprompt zabbix
```
Создайте БД:
```
sudo -u postgres createdb -O zabbix zabbi
```
При автоматизации с помощью bash можно использовать следующие примеры:

Создание пользователя с помощью psql из-под root
```
su - postgres -c 'psql --command "CREATE USER zabbix WITH PASSWORD '\'123456789\'';"'
su - postgres -c 'psql --command "CREATE DATABASE zabbix OWNER zabbix;"
```
Импортируем схему:
```
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
```
Задаём пароль в DBPassword:
```
sudo nano /etc/zabbix/zabbix_server.conf
```
Запускаем Zabbix server, Zabbix agent и веб-сервер:
```
sudo systemctl restart zabbix-server apache2 # zabbix-agent 
sudo systemctl enable zabbix-server apache2 # zabbix-agent
```
При автоматизации с помощью bash можно использовать следующий пример:

Настраиваем пароль DBPassword в файле /etc/zabbix/zabbix_server.conf:
```
sed -i 's/# DBPassword=/DBPassword=123456789/g' /etc/zabbix/zabbix_server.conf
```
Установка Zabbix agent похожа на установку Zabbix server:

Добавьте репозиторий Zabbix:
```
wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_6.0-4%2Bdebian11_all.deb
dpkg -i zabbix-release_6.0-4+debian11_all.deb 
apt update
```
Установите Zabbix agent и компоненты:
```
sudo apt install zabbix-agent -y
```
Запустите Zabbix agent:
```
sudo systemctl restart zabbix-agent 
sudo systemctl enable zabbix-agent
```
- zabbix_agentd.conf - Файл конфигурации Zabbix agent
- zabbix_agentd.log - Файл логов Zabbix agent

Меняем адрес сервера в zabbix_agentd.conf
```
sed -i 's/Server=127.0.0.1/Server=192.168.0.138'/g' /etc/zabbix/zabbix_server.conf
```

# Prometheus

**Prometheus** — это open-source набор инструментов, предназначенный для оповещения о каких-либо событиях, происходящих на хостах, мониторингом которых он занимается

Подходит для:
- обработки данных, представленных во временны́х рядах
- мониторинга аппаратных ресурсов
- мониторинга динамичной сервис- ориентированной архитектуры

**История Prometheus**
- Впервые разработан для внутреннего использования проектом SoundCloud
- С момента своего появления в 2012 г. Prometheus был взят на вооружение многими крупными компаниями: DigitalOcean, Ericsson, CoreOS, Weaveworks, Red Hat и Google
- Имеет активную команду разработчиков и комьюнити
- В настоящий момент проект развивается независимо от какой-либо компании

**Принцип работы Prometheus**
- Prometheus читает таблицу целей (targets) — список объектов, которые надо мониторить, и периодичность их опроса
- После этого Prometheus отправляет HTTP-запрос на каждый эндпоинт и получает ответ с набором из метрик. У каждой метрики есть название, значение и лейблы
- Полученный ответ складывается в базу данных, где к данным о метрике добавляются отметка времени её получения и лейблы объекта, с которого она была взята
- Service Discovery — встроенный в Prometheus механизм обнаружения сервисов, позволяющий «из коробки» получать данные для создания таблицы целей

**Установка Prometheus**

Ansible - https://github.com/jo-os/diplom/tree/main/Ansible/roles/prometheus

Создайте пользователя Prometheus:
```
sudo useradd --no-create-home --shell /bin/false prometheus
```
Найдите последнюю версию Prometheus на :GitHub
```
wget https://github.com/prometheus/prometheus/releases/download/v2.40.1/ prometheus-2.40.1.linux-386.tar.gz
```
Извлеките архив и скопируйте файлы в необходимые директории:
```
mkdir /etc/prometheus
mkdir /var/lib/prometheus**
cp ./prometheus promtool /usr/local/bin
cp -R ./console_libraries/ /etc/prometheus/
cp -R ./consoles/ /etc/prometheus/
cp ./prometheus.yml /etc/promehteus
```
Передайте права на файлы пользователю Prometheus:
```
chown -R prometheus:prometheus /etc/prometheus /var/lib/prometheus
chown prometheus:prometheus /usr/local/bin/prometheus
chown prometheus:prometheus /usr/local/bin/promtool
```
Запустите и проверьте результат:
```
/usr/local/bin/prometheus --config.file /etc/prometheus/prometheus.yml --storage.tsdb.path
/var/lib/prometheus/ --web.console.templates=/etc/prometheus/consoles --
web.console.libraries=/etc/prometheus/console_libraries
```
Создание сервиса для работы с Prometheus
```
nano /etc/systemd/system/prometheus.service
```
Вставьте в файл сервиса следующее содержимое:
```
[Unit] 
Description=Prometheus Service Netology Lesson 9.4 
After=network.target 
[Service] 
User=prometheus 
Group=prometheus 
Type=simple 
ExecStart=/usr/local/bin/prometheus \ 
--config.file /etc/prometheus/prometheus.yml \ 
--storage.tsdb.path /var/lib/prometheus/ \ 
--web.console.templates=/etc/prometheus/consoles \ 
--web.console.libraries=/etc/prometheus/console_libraries 
ExecReload=/bin/kill -HUP $MAINPID Restart=on-failure 
[Install] 
WantedBy=multi-user.target
```
Передайте права на файл:
```
chown -R prometheus:prometheus /var/lib/prometheus
```
Тестирование сервиса Prometheus

Пропишите автозапуск:
```
sudo systemctl enable prometheus
```
Запустите сервис:
```
sudo systemctl start prometheus
```
Проверьте статус сервиса:
```
sudo systemctl status prometheus
```

**Установка Node Exporter**

Скачайте архив с и извлеките его:
```
wget https://github.com/prometheus/node_exporter/releases/download/v1.4.0/node_exporter-1.4.0.linux-amd64.tar.gz
tar xvfz node_exporter-*.*-amd64.tar.gz
```
Перейдите в появившуюся папку:
```
cd node_exporter-*.*-amd64 
./node_exporter
```
Node Exporter помогает извлекать данные с хоста, на котором установлен.

Скопируйте Node Exporter в папку Prometheus:
```
mkdir /etc/prometheus/node-exporter 
cp ./* /etc/prometheus/node-exporter
```
Передайте права на папку пользователю Prometheus:
```
chown -R prometheus:prometheus /etc/prometheus/node-exporter
```
Создайте сервис для работы с Node Exporter
```
nano /etc/systemd/system/node-exporter.service
```
Вставьте в файл сервиса следующее содержимое:
```
[Unit] 
Description=Node Exporter Lesson 9.4 
After=network.target 
[Service] 
User=prometheus 
Group=prometheus 
Type=simple 
ExecStart=/etc/prometheus/node-exporter/node_exporter 
[Install] 
WantedBy=multi-user.target
```
Пропишите автозапуск:
```
sudo systemctl enable node-exporter
```
Запустите сервис:
```
sudo systemctl start node-exporter
```
Проверьте статус сервиса:
```
sudo systemctl status node-exporter
```
Добавление Node Exporter в Prometheus

Отредактируйте конфигурацию Prometheus:
```
nano /etc/prometheus/prometheus.yml
```
Добавьте в scrape_config адрес экспортёра:
```
scrape_configs: 
— job_name: 'prometheus' 
scrape_interval: 5s 
static_configs: 
— targets: ['localhost:9090', 'localhost:9100']
```
Перезапустите Prometheus:
```
systemctl restart prometheus
```
**Grafana**

**Grafana** — программное обеспечение для визуализации и аналитики с открытым исходным кодом

**Установка Grafana**

Скачайте и установите deb-пакет:
```
wget https://dl.grafana.com/oss/release/grafana_9.2.4_amd64.deb
dpkg -i grafana_9.2.4_amd64.deb
```
Включите автозапуск и запустите сервер Grafana:
```
systemctl enable grafana-server
systemctl start grafana-server
systemctl status grafana-server
```
Проверьте статус, перейдя по адресу: https://<наш сервер>:3000

Стандартный логин и пароль: admin/admin

**Atlermanager**

Программное обеспечение, которое позволяет: обрабатывать оповещения, отправляемые из клиентских приложений консолидировать эти оповещения перенаправлять их по необходимым каналам доставки сообщений

Установка Alertmanager

Скачайте последнюю версию приложения с GitHub
```
wget https://github.com/prometheus/alertmanager/releases/download/v0.24.0/alertmanager-0.24.0.linux-amd64.tar.gz
tar -xvf alertmanager-*linux-amd64.tar.gz
```
Скопируйте содержимое архива в папки:
```
sudo cp ./alertmanager-*.linux-amd64/alertmanager /usr/local/bin
sudo cp ./alertmanager-*.linux-amd64/amtool /usr/local/bin
```
Скопируйте config-файл в папку Prometheus:
```
sudo cp ./alertmanager-*.linux-amd64/alertmanager.yml /etc/prometheus
```
Передайте пользователю Prometheus права на файл:
```
sudo chown -R prometheus:prometheus /etc/prometheus/alertmanager.ym
```
Создайте сервис для работы с Node Exporter:
```
nano /etc/systemd/system/prometheus-alertmanager.service
```
Вставьте в файл сервиса следующее содержимое
```
[Unit]
Description=Alertmanager Service
After=network.target
[Service]
EnvironmentFile=-/etc/default/alertmanager
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/alertmanager --config.file=/etc/prometheus/alertmanager.yml
--storage.path=/var/lib/prometheus/alertmanager $ARGS
ExecReload=/bin/kill -HUP $MAINPID
Restart=on-failure
[Install]
WantedBy=multi-user.target
```
Пропишите автозапуск:
```
sudo systemctl enable prometheus-alertmanager
```
Запустите сервис:
```
sudo systemctl start prometheus-alertmanager
```
Проверьте статус сервиса:
```
sudo systemctl status prometheus-alertmanage
```
Подключение Prometheus к Alertmanager

Добавьте в сonfig-файл Prometheus подключение к Alertmanager:
```
sudo nano /etc/prometheus/prometheus.yml
```
Перезапустите Prometheus:
```
alerting:
alertmanagers:
- static_configs:
- targets: # Можно указать как targets: [‘localhost”9093’]
- localhost:9093
```
Приведите раздел Alertmanager configuration к виду:
```
sudo systemctl restart prometheus
systemctl status prometheus
```
Создание правила оповещения

Создайте файл с правилом оповещения:
```
sudo nano /etc/prometheus/netology-test.yml
```
Приведите файл к следующему виду:
```
groups: # Список групп
- name: netology-test # Имя группы
rules: # Список правил текущей группы
- alert: InstanceDown # Название текущего правила
expr: up == 0 # Логическое выражение
for: 1m # Сколько ждать отбоя сработки перед отправкой оповещения
labels:
severity: critical # Критичность события
annotations: # Описание
description: '{{ $labels.instance }} of job {{ $labels.job }} has been down for more than 1 minute.' # Полное описание алерта
summary: Instance {{ $labels.instance }} down # Краткое описание алерта
```
Подключение правила к Prometheus

Отредактируйте prometheus.yml:
```
sudo nano /etc/prometheus/prometheus.yml
```
Добавьте в раздел **rule_files** запись:
```
- "netology-test.yml"
```
Перезапустите Prometheus:
```
sudo systemctl restart prometheus
systemctl status prometheus
```
Откройте сonfig-файл Alertmanager:
```
sudo nano /etc/prometheus/alertmanager.yml
```
Приведите его к следующему виду:
```
global:
route:
group_by: ['alertname'] # Параметр группировки оповещений — по имени
group_wait: 30s # Сколько ждать восстановления, перед тем как отправить первое оповещение
group_interval: 10m # Сколько ждать, перед тем как дослать оповещение о новых сработках по текущему алерту
repeat_interval: 60m # Сколько ждать, перед тем как отправить повторное оповещение
receiver: 'email' # Способ, которым будет доставляться текущее оповещение
receivers: # Настройка способов оповещения
- name: 'email'
email_configs:
- to: 'yourmailto@todomain.com'
from: 'yourmailfrom@fromdomain.com'
smarthost: 'mailserver:25'
auth_username: 'user'
auth_identity: 'user'
auth_password: 'paS$w0rd
```
После внесения всех изменений перезапустите Alertmanager и выключите экспортёр, стоящий на сервере Prometheus:
```
sudo systemctl restart prometheus-alertmanager
systemctl status prometheus-alertmanager
sudo systemctl stop node-exporter
systemctl status node-exporter
```
Теперь можете проверить интерфейсы Prometheusи Alertmanager, расположенные на стандартных портах 9090 и 9093

**Мониторинг Docker в Prometheus**

Docker «из коробки» поддерживает мониторинг с помощью Prometheus. Чтобы включить выгрузку данных на хосте с Docker, нужно создать файл daemon.json:
```
sudo nano /etc/docker/daemon.json
```
Укажите внутри него IP и порт, на котором Docker будет отдавать метрики, в формате «server_ip:port»
```
{
"metrics-addr" : "ip_нашего_сервера:9323",
"experimental" : true
}
```
Перезапустите Docker:
```
sudo systemctl restart docker && systemctl status docker
```
Для проверки можно открыть адрес http://server_ip:port/metrics

Чтобы поставить только что организованный эндпоинт на мониторинг, необходимо отредактировать файл prometheus.yml:
```
sudo nano /etc/prometheus/prometheus.yml
```
В раздел static_configs добавьте новый эндпоинт
```
static_configs:
- targets: ['localhost:9090', 'localhost:9100', 'server_ip:9323']
```
Перезапустите Prometheus:
```
sudo systemctl restart prometheus
systemctl status prometheus
```
