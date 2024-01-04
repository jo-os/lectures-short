# Ansible
Ansible

**Инфраструктура как код (англ. Infrastructure-as-Code; Iac)** — это подход к управлению и описанию инфраструктуры через конфигурационные файлы, а не через ручное редактирование конфигураций на серверах. Процесс настройки инфраструктуры аналогичен процессу программирования ПО. Границы между написанием приложений и созданием сред для этих приложений стираются.

**Категории инструментов IAC**
- Специализированные - скрипты Скрипты, которые пошагово выполняют ранее ручные действия - Bash, PowerShell и т. д.
- Средства управления конфигурацией - Предназначены для установки и администрирования ПО на существующих серверах - Chef, Ansible, SaltStack
- Средства шаблонизации серверов - На базе образа создают виртуальные машины или контейнеры - Packer, Vagrant, Docker
- Средства оркестрации - Программные средства, позволяющие управлять жизненным циклом объектов - Kubernetes, Google Kubernetes Engine
- Средства инициализации ресурсов - Создают (инициализируют) ресурсы, например, сервера - Terraform, CloudFormation

**Плюсы IAC**
- Нет необходимости в ручной настройке
- Скорость — настройка («поднятие») инфраструктуры занимает заметно меньше времени
- Воспроизводимость — поднимаемая инфраструктура всегда идентична
- Масштабируемость — один инженер может с помощью одного и того же кода настраивать и управлять огромным количеством машин

**Системы управления конфигурациями** — программы и комплексы, позволяющие централизованно управлять конфигурацией множества разнообразных разрозненных ОС и прикладного ПО, работающего в них. При работе с системами управления конфигурациями говорят об автоматизации инфраструктуры или об оркестрации серверов.

**Преимущества Configuration management systems:**
- использовать систему контроля версий для отслеживания любых изменений инфраструктуры
- повторно использовать скрипты конфигурирования для нескольких серверных сред, например, для разработки, тестирования и производства
- предоставлять сотрудникам общий доступ к скриптам конфигурирования для упрощения сотрудничества в стандартизированной среде разработки
- упрощать процесс дублирования серверов для ускорения восстановления при сбое системы

**Ведущие инструменты для управления конфигурацией**
- Chef - DSL - Ruby - Клиент-сервер
- Puppet - DSL(JSON) - Ruby - Клиент-сервер 
- SaltStack - YAML - Python - Клиент-сервер, безагентный  
- Ansible - YAML - Python - Безагентный

**Ansible** — система управления конфигурациями.
- Позволяет централизованно управлять ПО, ОС и их настройками на Linux, Mac, Windows
- Написан на Python, открытый исходный код
- Разработчик — Red Hat

**Особенности Ansible**
- **Безагентный** — для работы, настройки с управляемым узлом нет необходимости ставить на управляемый узел агента. Необходимых требований только два: работающий ssh-сервер, python версии 2.6 и выше
- **Идемпотентность** — независимо от того, сколько раз вы будете запускать playbook, результат (конфигурация управляемого узла) всегда должен приводить к одному и тому же состоянию
- **Push-model** — изменения конфигураций «заталкиваются» на управляемые узлы. Это может быть минусом

**Основные термины Ansible**
- **Узел управления** — устройство с установленным и настроенным Ansible. Может быть ваш ноутбук или специальный узел в сети (подсети), выделенный для задач управления
- **Управляемые узлы** — узлы, конфигурация которых выполняется
- **Файлы инвентаризации (inventory)** — файл или файлы, в которых перечислены управляемые узлы: *.ini, *.yaml или динамический
- **Модули (modules)** отвечают за действия, которые выполняет Ansible, другими словами, инструментарий Ansible
- **Задачи (tasks)** — отдельный элемент работы, которую нужно выполнить. Могут выполняться самостоятельно или в составе плейбука
- **Плейбук (playbook)** состоит из списка задач или других директив, указывающих на то, какие действия и где будут производиться
- **Обработчики (handlers)** — элемент, который служит для экономии кода и способен перезапускать службу при его вызове
- **Роли (roles)** — набор плейбуков и других файлов, которые предназначены для выполнения какой-либо конечной задачи. Также упрощают, сокращают код и делают его переносимым

Устанавливаем Ansible:
```
yum install ansible/apt install ansible
```
Правим при необходимости конфигурационный файл Ansible:
```
vim /etc/ansible/ansible.cfg
```
Правим файл inventory по умолчанию или создаём свой:
```
vim /etc/ansible/hosts
```
Смотрим версию и другие переменные запуска Ansible:
```
ansible --version
```
**Инвентарный файл** — это файл с описанием устройств, к которым будет подключаться и управлять Ansible. Может быть в формате INI или YAML, или быть динамически конфигурируемым какой-либо вычислительной системой. Две группы по умолчанию: all и ungrouped
```
ansible all -m ping --list-hosts — вывести список хостов
ansible-playbook --list-hosts — вывести список хостов для playbook
```
**Ansible.cfg** — это основной конфигурационный файл. Может храниться:
- ANSIBLE_CONFIG — переменная окружения
- ansible.cfg — в текущем каталоге
- ~/.ansible.cfg — в домашнем каталоге пользователя
- /etc/ansible/ansible.cfg — можно брать за образец для внесения правок
```
ansible --version — покажет, какой конфигурационный файл будет использоваться
```
В конфигурационном файле можно задавать множество параметров, например:
```
[defaults]
inventory = inventory.ini # расположение файла inventory
remote_user = ansible # пользователь, которым подключаемся по ssh
gathering = explicit # отключает сбор фактов
forks = 5 # количество хостов, на которых текущая задача выполняется одновременно
[privilege_escalation]
become = True # требуется повышение прав - становимся root
become_user = root # пользователь, под которым будут выполняться задачи
become_method = sudo # способ повышения прав
become_ask_pass = true # спросит пароль при become_method = su - если он понадобится
```
**Параметры** - Большинство настроек также может задаваться или переопределять во время выполнения команд через параметры
```
ansible -i hosts.ini all -m ping — вручную указывает файл инвентори
ansible all -m ping -e "ansible_user=vagrant ansible_ssh_pass=vagrant" — вручную задаёт удалённого пользователя и пароль
ansible all -u vagrant -m ping — вручную задаёт удалённого пользователя
ansible web* -m ping — задаёт список хостов к выполнению через регулярные выражения
```
**Модули** — это небольшая программа, входящая в поставку Ansible, принимающая на вход значения и выполняющая работу на целевых хостах. Фактически вся работа происходит с использованием модулей. Можно самостоятельно писать модули и расширять возможности Ansible
```
ansible-doc -l
ansible-doc shell
ansible all (все сервера) -i (путь до инвентори - не нужно если указан в cfg) ./hosts -m (имя модуля) ping
```
**Ad-hoc команды** — это самый быстрый способ начать использовать Ansible. Для запуска Ansible в режиме ad-hoc не нужно писать плейбуки, достаточно помнить минимальный синтаксис
```
ansible all -m ping
ansible all -m command -a 'cat /etc/hosts'
```
**Часто используемые модули**

- ping - Позволяет проверить доступность хостов для работы через Ansible
- service - Позволяет управлять работой служб (демонов) на хостах
- command - Позволяет запустить команду без использования окружения
- copy - Позволяет копировать файлы
- lineinfile - Позволяет заменять или добавлять строки в текстовых файлах
- debug - Позволяет выводить отладочные сообщения
- git - Позволяет работать с git-репозиториями

```
ssh-copy-id user@server - копируем ssh ключ для соединения без пароля
ansible-doc -l - доступные модули
ansible-doc shell - инструкция по модулю shell
ansible all -m shell -a 'hostname' - выполнение команды hostname на серверах
ansible webservers -m shell -b (become - повышение до root или через cfg) -a "yum install mc"
ansible-doc yum - описание модуля yum
ansible webservers -m yum -b -a 'name=mc state=present' - установка последней версии и если установлено то ничего не делать
ansible webservers -m yum -b -a 'name=mc state=absent' - удаление
ansible webservers -m yum -b -a 'name=mc state=absent' -vvv (-v - verbose от v до vvv - более подробный вывод)
```

# Ansible2
YAML, Playbook, Roles

**Основные термины Ansible**
- **Узел управления** — устройство с установленным и настроенным Ansible. Ноутбук или специальный узел в сети или подсети, выделенный для задач управления
- **Управляемые узлы** — узлы, конфигурация которых выполняется
- **Файлы инвентаризации (inventory)** — файл или файлы, в которых перечислены управляемые узлы: *.ini, *.yaml или динамический
- **Модули (modules)** - отвечают за действия, которые выполняет Ansible, другими словами, инструментарий Ansible
- **Задачи (tasks)** — отдельный элемент работы, который нужно выполнить. Могут выполняться самостоятельно или в составе плейбука
- **Плейбук (playbook)** — состоит из списка задач и других директив, указывающих на то, какие действия и где будут производиться
- **Обработчики (handlers)** — элемент, который служит для экономии кода и способен перезапускать службу при его вызове
- **Роли (roles)** — набор плейбуков и других файлов, которые предназначены для выполнение какой-либо конечной задачи. Упрощают, сокращают код и делают его переносимым

**YAML - Yet Another Markup Language** — это язык для хранения информации в формате, понятном человеку
```
yml = yaml
```
**YAML syntax** - В файлах YAML для разделения структур данных используются пробелы. Не используйте tab, только пробелы, IDE облегчит задачу Элементы данных одного уровня должны иметь одинаковое количество отступов. Подчинённые элементы должны выделяться бóльшим количеством отступов, чем родительские

**Playbook** — это основной рабочий скрипт. Он позволяет объединять задачи (task), роли в одном файле и передавать их для выполнения на определённые группы хостов.
```
ansible-playbook myscript.yaml
```
Пример проверки синтаксиса плейбука:
```
ansible-playbook myscript.yaml --syntax-check
```
**Зачем нужны плейбуки**
- Ad-hoc команды удобны для запуска одной или двух команд
- Ad-hoc команды удобны для тестирования
- С их помощью можно запускать множество задач для множества хостов
- С их помощью удобно управлять запусками и сокращать код

В примере плейбук состоит из двух разделов, для каждого отдельно выбираются целевые хосты и запускаются роли или задачи:
```yml
---
- name: Playbook 1 name
  hosts: group1
  roles:
    - role1

- name: Playbook 2 name
  hosts: group2,group3
  tasks:
    - name: task 1 in playbook 2
      debug:
        msg: “Hello World”
    - name: task 2 in playbook 2
      service:
        name: sshd
        state: restarted
...
```
```
ansible <ip or name> -m setup - выдаст всю информацию о хосте в формате json - это так же называется facts - факты, стартуют при запуске прейбука и их можно отключить
```
```
ansible-playbook -i ./hosts playbook.yml --syntax-check - проверка синтекса playbook файлы
```
**Ansible roles**

**Role** — это набор файлов, задач, шаблонов, переменных и обработчиков, которые объединены вместе в логическую структуру и служат определённой цели. Например, настройка сервера времени. Роли можно подключать в плейбуки. Роли удобно передавать коллегам, как целостную сущность

Для интерактивной работы с ролями используется команда ansible-galaxy

**Преимущества использования ролей**
- Можно использовать для нескольких проектов
- Удобно передавать
- Удобно отправить на внешние ресурсы
- Удобно управлять зависимостями

**Структура ролей**
- defaults — содержит переменные по умолчанию для роли, которые должны быть легко перезаписаны
- vars — содержит стандартные переменные для роли, которые не должны быть перезаписаны
- tasks — содержит набор задач, которые должна выполнять роль
- handlers — содержит набор обработчиков, которые будут использоваться в роли
- templates — содержит шаблоны Jinja2 (язык шаблонов), которые будут использоваться в роли
- files — содержит статические файлы, необходимые из ролевых задач (позволяет скопировать файл на удаленный хост)
- tests — может содержать дополнительный inventory файл, а также playbook test.yml, который можно использовать для тестирования роли
- meta — содержит метаданные роли — информацию об авторе, лицензию, зависимости и т.д.

Расположение файлов роли на файловой системе имеет значение:
- ./roles — самый высокий приоритет
- ~/.ansible/roles
- /etc/ansible/roles
- /usr/share/ansible/roles — самый низкий приоритет

**Запуск ролей** - Чтобы запустить роль, необходимо написать плейбук, и в нём вызвать роль. При необходимости можно переопределить переменные в момент вызова роли
```yml
---
- name: roles example
  hosts: servers
  roles:
    - role: role1
    - role: role2
      var1: one
      var2: two
```
Практика
```
ansible-galaxy init nginx - создает структуру из папок для nginx
```
В папку task -  main.yml переносим все из tasks: и запускаем уже роли
```yml
- name: "Install nginx"
    host: nginx
    become: true
    tags:
    - nginx
    gather_facts: true
    roles:
    - nginx # - запускаем роль созданную ренее
```
**Ansible-galaxy** — это сайт (https://galaxy.ansible.com/home), который является публичным репозиторием для ansible-ролей, написанных сообществом. Т. е. на нём можно искать, использовать и выкладывать роли
```
ansible-galaxy role list
ansible-galaxy role init myrole
ansible-galaxy role search nginx
ansible-galaxy role install nginx
ansible-galaxy role remove nginx
```
**Tags** - Теги служат, чтобы помечать определённые задачи (task) внутри плейбука или роли и вызывать либо наоборот пропускать только их, не заставляя выполняться весь плейбук
```
ansible-playbook -i hosts.ini --tags prod playbook.yml
ansible-playbook -i hosts.ini --skip-tags prod playbook.yml
```
**Handlers** — это специальные задачи. Они вызываются из других задач ключевым словом notify. Служат для сокращения кода
```yml
---
- name: handlers example
  hosts: all
  become: yes
  tasks:
  - name: Change ssh config
    lineinfile:
      path: /etc/ssh/sshd_config
      regexp: '^PasswordAuthentication'
      line: PasswordAuthentication yes
    notify:
      - Restart sshd

  handlers: # handlers - main.yml
  - name: Restart sshd
    service:
      name: sshd
      state: restarted
```

**Условие when** - За счёт конструкции when можно выполнять задачу из плейбука только при выполнении какого-либо условия
```yml
tasks:
  - name: Configure SELinux to start mysql on any port
    ansible.posix.seboolean:
      name: mysql_connect_any
      state: true
      persistent: yes
    when: ansible_selinux.status == "enabled"
    # when: ansible_facts['os_family'] == "RedHat"
```
**Jinja2** - С помощью механизма шаблонов Jinja2 можно динамически создавать, например, конфигурационные файлы. Для работы с шаблонами Jinja2 используется модуль template. Пример использования:
```yml
cat index.j2
A message from {{ inventory_hostname }}

- name: Create index.html using Jinja2
  template:
    src: index.j2
    dest: /var/www/html/index.html
```
Пример task Практика
```
- name: "Add index page"
  template:
    src: "index.html.j2"
    dest: "/usr/share/nginx/html/index.html"
    owner: root
    group: root
    mode: 0755
```
Пример task с использованием тегов:
```
- name: tag usage example
  shell: /bin/echo "Only running because of specified tag"
  tags: mytag
```
**Vars**

В Ansible существует 22 способа задавать переменные. С их помощью можно реализовывать логику работы скрипта. В документации (https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html) описано, какой приоритет имеет тот или иной способ задания переменных.

Переменная задаётся в процессе запуска плейбука из командной строки:
```
ansible-playbook example.yml --extra-vars "foo=bar" - extra vars (наприме, -e "user=my_user") - наивысший приоритет
```
Через роли - defaults - main.yml - приоритет ниже
```
port: 80 # Практика
```
Через роли - vars - main.yml - приоритет выше - больше для служебных переменных
```
port: 80 # Практика
```
Переменная задаётся в начале плейбука:
```
---
- hosts: example
  vars:
    foo: bar
  tasks:
    debug:
```
Переменная задаётся в отдельном файле, на который ссылается плейбук:
```
---
- hosts: example
  vars_files:
    - vars.yml
  tasks:
    debug
```
**Special variables**

В Ansible есть встроенные переменные (https://docs.ansible.com/ansible/latest/reference_appendices/special_variables.html). Их имена нельзя переназначать, но использование встроенных переменных может быть очень полезно
```
ansible_play_hosts_all — вернёт список хостов из inventory файла
```
Чтобы не хранить пароли в открытом виде, в Ansible существует vault — шифрованное хранилище паролей

**Ansible-vault** — утилита, которая позволяет работать с таким хранилищем
```
ansible-vault create file2.yml
ansible-vault decrypt file2.yml
ansible-vault encrypt file2.yml - потом вводим пароль
ansible-vault rekey file2.yml
--ask-vault-pass - ключ при запуске плейбука, спрашивает пароль для расшифровки зашифрованных файлов
```
**Ansible Tower** — графический веб-интерфейс для управления и мониторинга работы Ansible. Позволяет:
- пользователям работать со скриптами без необходимости настраивать окружение и разбираться с ними
- управлять доступом и создавать группы для определённых категорий пользователей — есть интеграция с LDAP
- настраивать разного рода автоматизации при помощи REST API

Открытая версия (upstream) — AWS project

**Molecule** — это фреймворк для тестирования Ansible ролей. В конфигурационных файлах Molecule описывается инфраструктура и тесты, которыми покрывается роль. Что позволяет сделать:
- проверять синтаксис
- поднимать тестовую инфраструктуру
- писать тесты вручную
- скачивать роли из Galaxy

# Ansible3

**Установка**

https://linuxopsys.com/topics/ansible-playbook-to-install-apache

```
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible
```
**Посмотреть inventory**
```
ansible-inventory --list
```
**Запуск Ad-Hoc Команд**
```
ansible all -m ping - ping
ansible all -m setup - facts
ansible all -m shell -a 'uptime'
ansible all -m shell -a 'ls /etc | grep py' - через shell можно делать pipe, >, >> и т.д.
ansible all -m command -a 'ls /etc | grep py' - ERROR - не работает с pipe - для безопасности
ansible all -m copy -a "src=/root/hy.txt dest=/home/joos" - копируем файл
ansible all -m copy -a "src=/root/hy.txt dest=/home/joos mode=777"
ansible all -m file -a 'path=/home/joos/hy.txt state=absent' - удаляем файл
ansible all -m get_url -a 'url=https://pickimage.ru/wp-content/uploads/2020/08/rizhikot1.jpg dest=/home/joos' - качаем файл из интернета
ansible all -bK -m apt -a 'name=mc state=present' - ставим mc - -b = become - повышаем права - -K - спрашиваем пароль при повышении прав
ansible all -bK -m apt -a 'name=mc state=absent' - удаляем
ansible all -m uri -a 'url=https://ifconfig.me' - проверяем доступ к сайту - получаем заголовок
ansible all -m uri -a 'url=https://ifconfig.me return_content=yes' - получаем содержимое сайта
ansible all -bK -m apt -a 'name=apache2 state=latest' - ставим apache2
ansible all -bK -m service -a 'name=apache2 state=started enabled=yes' - запускаем и включаем запуск при старте apache2
ansible all -bK -m apt -a 'name=apache2 state=absent'
ansible all -m shell -a 'ls' -vvv - подробный вывод для анализа ошибок
ansible-doc -l - все модули
```
**Перенос переменных в group_vars**
```
mkdir group_vars - создаем директорию для переменных
nano group_vars/server1 - по названию группы создаем файл
file
---
ansible_user : joos - например имя пользователя при коннекте
file
```
**{"msg": "Missing sudo password"}**

https://www.ansiblepilot.com/articles/ansible-troubleshooting-missing-sudo-password/
```
visudo
joos  ALL=(ALL) NOPASSWD: ALL

sudo nano /etc/sudoers.d/joos
joos ALL=(ALL) NOPASSWD: ALL
```

**Playbook**

Test connection
```yml
---
- name: Test connection to my servers
  hosts: all
  become: yes

  tasks:
  - name: Ping my servers
    ping:
```
Install Apache
```yml
---
- name: Install Apache Web Server
  hosts: all
  become: yes
  tasks:
  - name: Install Apache Web Server
    apt: name=apache2 state=latest

  - name: Start Apache and Enable Service
    service: name=apache2 state=started enabled=yes

```
Install Apache and copy index.html
```yml
---
- name: Install Apache and Upload my Web Page
  hosts: all
  become: yes

  vars:
    source_file: ./index.html
    destin_file: /var/www/html

  tasks:
  - name: Install Apache Web Server
    apt: name=apache2 state=latest
  - name: Copy MyPage to Servers
    copy: src={{ source_file }} dest={{ destin_file }} mode=0555
    notify: Restart Apache
  - name: Start WebServer and make it enable
    service: name=apache2 state=started enabled=yes

  handlers:
  - name: Restart Apache
    service: name=apache2 state=restarted
```
**Debug, Set_fact, Register**
```yml
---
- name: My playbook
  hosts: all
  become: yes

  vars:
    message1: Privet
    message2: World
    secret: asdkasdlkjahsldjk

  tasks:
  - name: Print Secret
    debug:
      var: secret
  - debug:
      msg: 'Sekretnoe slovo: {{ secret }}'
  - debug:
      msg: "Vladelec Serverd -->{{ owner }}<--" # - owner - добавляем в group_vars/server1 - owner : Petya
  - set_fact: full_message="{{ message1 }} {{ message2 }} from {{ owner }}" # - собираем переменные
  - debug:
      var: full_message # - выводим переменные
  - debug:
      var: ansible_distribution # - переменные из facts в моем случае Debian
  - shell: uptime  # - если просто запустить, то выполнится на удаленном сервере и ничего не вернет, надо сохранить вывод
    register: results # - сохраняем вывод в results
  - debug:
      var: results.stdout # - выводим results и уменьшаем вывод через stdout
```
**Блоки и Условия – Block-When**

Переносим для разных систем в разные блоки и делаем общую команду when: ansible_os_family ==
```yml
---
- name: Install Apache and Upload my Web Page
  hosts: all
  become: yes

  vars:
    source_file: ./index.html
    destin_file: /var/www/html

  tasks:
  - name: Check and Print Linux-Family
    debug: var=ansible_os_family

  - block: # for Debian
    - name: Install Apache Web Server for Debian
      apt: name=apache2 state=latest
    - name: Copy MyPage to Servers
      copy: src={{ source_file }} dest={{ destin_file }} mode=0555
      notify: Restart Apache Debian
    - name: Start WebServer and make it enable for Debian
      service: name=apache2 state=started enabled=yes
    when: ansible_os_family == "Debian"

  - block: # for RedHat
    - name: Install Apache Web Server for RedHat
      yum: name=httpd state=latest
    - name: Copy MyPage to Servers
      copy: src={{ source_file }} dest={{ destin_file }} mode=0555
      notify: Restart Apache RedHat
    - name: Start WebServer and make it enable for RedHat
      service: name=httpd state=started enabled=yes
    when: ansible_os_family == "RedHat"

  handlers:
  - name: Restart Apache Debian
    service: name=apache2 state=restarted

  - name: Restart Apache RedHat
    service: name=httpd statr=restarted
```
**Циклы – Loop, With_Items, Until, With_fileglob**
```yml
---
- name: Loops Playbook
  hosts: all
  become: yes

  tasks:
  - name : Say Hello to All
    debug: msg="Hello {{ item }}"
    loop: # - в старых версиях with_items
      - "Vasya"
      - "Mascha"
      - "Igor"
  - name: Loop Until example
    shell: echo -n Z >> myfile.txt && cat myfile.txt
    register: output # - сохраняем вывод
    delay: 2 # - делаем переры 2 сек между попытками
    retries: 10 # - количество попыток - по умолчанию 3
    until: output.stdout.find("ZZZZ") == false  # - делаем пока не получим ZZZZ

  - name: Print Output
    debug:
      var: output.stdout # - печатаем output

  - name: Install many package # - ставим много пакетов
    apt: name={{ item }} state=latest
    loop:
      - mc
      - tree
      - sysstat
```
```yml
---
- name: Install Apache and Upload my Web Page
  hosts: all
  become: yes

  vars:
    source_folder: ./mysite
    destin_folder: /var/www/html

  tasks:
  - name: Check and Print Linux-Family
    debug: var=ansible_os_family

  - block: # for Debian

    - name: Install Apache Web Server for Debian
      apt: name=apache2 state=latest
    - name: Start WebServer and make it enable for Debian
      service: name=apache2 state=started enabled=yes
    when: ansible_os_family == "Debian"

  - block: # for RedHat

    - name: Install Apache Web Server for RedHat
      yum: name=httpd state=latest
    - name: Start WebServer and make it enable for RedHat
      service: name=httpd state=started enabled=yes
    when: ansible_os_family == "RedHat"

  - name: Copy MyPage to Servers
#    copy: src={{ source_folder }}/{{ item }} dest={{ destin_folder }} mode=0555 - первый вариант копирования пофайлово
#    loop:
#      - "index.html"
#      - "1.jpg"
#      - "2.jpg"
    copy: src={{ item }} dest={{ destin_folder }} mode=0555 # - второй вариант копирования - вся папка
    with_fileglob: "{{ source_folder }}/*.*"
    notify:
      - Restart Apache RedHat
      - Restart Apache Debian

  handlers:
  - name: Restart Apache Debian
    service: name=apache2 state=restarted
    when: ansible_os_family == "Debian"

  - name: Restart Apache RedHat
    service: name=httpd statr=restarted
    when: ansible_os_family == "RedHat"
```
**Шаблоны - Jinja Template**
```yml
---
- name: Install Apache and Upload my Web Page
  hosts: all
  become: yes

  vars:
    source_folder: ./website2
    destin_folder: /var/www/html

  tasks:
  - name: Check and Print Linux-Family
    debug: var=ansible_os_family

  - block: # for Debian

    - name: Install Apache Web Server for Debian
      apt: name=apache2 state=latest
    - name: Start WebServer and make it enable for Debian
      service: name=apache2 state=started enabled=yes
    when: ansible_os_family == "Debian"

  - name: Generate INDEX.HTML file
    template: src={{ source_folder }}/index.j2 dest={{ destin_folder }}/index.html mode=0555
    notify:
      - Restart Apache Debian

  handlers:
  - name: Restart Apache Debian
    service: name=apache2 state=restarted
```
```html
<HTML>
<HEAD>
<TITLE>ADV-IT</TITLE>
<BODY bgcolor="black" onLoad=Elastic()>
<CENTER>
<br><br><br><br>
<br><br><br><br>

<font color="green"><h2>Owner of this Server is: {{ owner }}</h2>
<font color="green"><h2>This Page was created with</h2>
<font color="gold"><H1 ID="elastic" ALIGN="Center">ANSIBLE-BLA-BLA</H1>

<font color="gold"><H1 ID="elastic" ALIGN="Center">Server Host Name : {{ ansible_hostname }}</H1>
<font color="gold"><H1 ID="elastic" ALIGN="Center">Server OS Family : {{ ansible_os_family }}</H1>
<font color="gold"><H1 ID="elastic" ALIGN="Center">IP Adress : {{ ansible_default_ipv4.address }}</H1>
</body>
</HTML>
```

# Ansible4

**Создание Ролей - Roles**
```
mkdir roles
cd roles/
ansible-galaxy init deploy_apache_web # - называем как хотим, если делаем свое
Получаем папку с директориями
└── deploy_apache_web
    ├── defaults
    │   └── main.yml #  <-- default lower priority variables for this 
    ├── handlers
    │   └── main.yml #  <-- handlers file
    ├── meta
    │   └── main.yml #  <-- role dependencies
    ├── README.md
    ├── tasks
    │   └── main.yml #  <-- tasks file can include smaller files if warranted
    ├── tests
    │   ├── inventory
    │   └── test.yml
    └── vars
        └── main.yml #  <-- variables associated with this role
```
https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html

defaults/main.yml
```yml
---
# defaults file for deploy_apache_web
destin_folder: /var/www/html
```
handlers/main.yml
```yml
---
# handlers file for deploy_apache_web
- name: Restart Apache Debian
  service: name=apache2 state=restarted
  when: ansible_os_family == "Debian"

- name: Restart Apache RedHat
  service: name=httpd statr=restarted
  when: ansible_os_family == "RedHat"
```
tasks/main.yml
```yml
---
# tasks file for deploy_apache_web
- name: Check and Print Linux-Family
  debug: var=ansible_os_family

- block: # for Debian

  - name: Install Apache Web Server for Debian
    apt: name=apache2 state=latest
  - name: Start WebServer and make it enable for Debian
    service: name=apache2 state=started enabled=yes
  when: ansible_os_family == "Debian"

- name: Generate INDEX.HTML file
  template: src=index.j2 dest={{ destin_folder }}/index.html mode=0555
  notify:
    - Restart Apache Debian
```
cp index.j2 templates/

playbookroles.yml
```yml
---
- name: Install Apache and Upload my Web Page
  hosts: all
  become: yes

  roles:
    - { role: deploy_apache_web, when:ansible_system == 'Linux' } # - запустим только если Linux
```
Запускаем
```
ansible-playbook playbookroles.yml
```
**Внешние переменные - extra-vars**
```yml
---
- name: Install Apache and Upload my Web Page
  hosts: "{{ MYHOSTS }}"
  become: yes

  roles:
    - { role: deploy_apache_web, when:ansible_system == 'Linux' }
```
Запускаем
```
ansible-playbook playbookvars.yml --extra-vars "MYHOSTS=server1"
ansible-playbook playbookvars.yml --extra-vars "MYHOSTS=server1"
ansible-playbook playbookvars.yml --extra-vars "MYHOSTS=server2 owner=DENIS!" # - перепишет owner, так как высший приоритет
```
**Использование Import, Include**

Выносим логику в отдельные файлы и подключаем в основном
```
include: create_folder.yml
```

**Import** - сразу все объединяет и выполняет

**Include** - когда видит файл, тогда идет и выполняет его
```yml
---
- name: My Playbook
  hosts: all
  become: yes

  vars:
    mytext: "Hello from Joos"

  tasks:
  - name: Ping test
    ping:

  - name: Create folders
    include: create_folder.yml

  - name: Create files
    include: create_file.yml mytext="Prosto Hello"
```
create_folder.yml
```yml
---
- name: Create folder1
  file:
    path: /home/secret/folder1
    state:  directory
    mode: 0755

- name: Create folder2
  file:
    path: /home/secret/folder2
    state:  directory
    mode: 0755
```
create_file.yml
```yml
---
- name: Create file1
  copy:
    dest: /home/secret/file1.txt
    content: |
      Text Line1, in file1
      Text Line2, in file1
      Text Line3, {{ mytext }}

- name: Create file2
  copy:
    dest: /home/secret/file2.txt
    content: |
      Text Line1, in file2
      Text Line2, in file2
      Text Line3, {{ mytext }}
```
**Перенаправление выполнения Task из Playbook на определённый сервер - delegate_to**
```
delegate_to: 127.0.0.1 # - выполнится только на мастер хосте
```
playbook-delegate.yml
```yml
---
- name: Play it delegate
  hosts: all
  become: yes

  vars:
    mytext: "Prinvet ot Joos"

  tasks:
    - name: Ping test
      ping:

    - name: Unregister Server from Load Balancer
      shell: echo This sever {{ inventory_hostname }} was deregistered from our Load Balancer,  nodename is {{ ansible_nodename }} >> /home/joos/log.txt # - запишем вывод в лог
      delegate_to: 127.0.0.1 # - выполнится и запишет лог только на мастер хосте

    - name: Update my Database
      shell: echo UPDATING Database...
      run_once: true  # - запустит только 1 раз на 1 хосте

    - name: Create file1
      copy:
        dest: /home/joos/file1.txt
        content: |
          This is FileN1
          On ENGLISH Hello World
          On RUSSIAN {{ mytext }}
      delegate_to: server1 # - выполнится только на server1

    - name: Create file2
      copy:
        dest: /home/joos/file2.txt
        content: |
          This is FileN2
          On ENGLISH Hello World
          On RUSSIAN {{ mytext }}

    - name: Reboot my servers; # - Перезагружаем сервера
      shell: sleep 3 && reboot now - # - ждем 3 секунды и перезагружаем
      async: 1
      poll: 0 # - кинули команду и не ждем ответа

    - name: Wait till my server will come up online
      wait_for:
        host: "{{ inventory_hostname}}"
        state: started
        delay: 5 # - ждем 5 секунд и проверяем
        timeout: 40 # - ждем максимум 40 секунд
      delegate_to: 127.0.0.1 # - ждем только на мастере (сервера в это время перезагружаются)
```
**Перехват и Контроль ошибок**
```
any_errors_fatal: true # - если ошибка то останавливаем выполнение на всех серверах
ignore_errors: yes # - если ошибка то игнорируем и выполняем таски дальше
failed_when: "'World' in results.stdout" # - Ошибка если в выводе есть слово World
failed_when: results.rc != 0 # - ошибка если return code не равен 0
```
plabook-error.yml
```yml
---
- name: Ansible and Error
  hosts: all
  any_errors_fatal: true # - если ошибка то останавливаем выполнение на всех серверах
  become: yes

  tasks:
    - name: task Number1
      apt: name=treeee state=latest
      ignore_errors: yes # - если ошибка то игнорируем и выполняем таски дальше

    - name: task Number2
      shell: echo Hello World
      register: results
#      failed_when: "'World' in results.stdout" # - Ошибка если в выводе есть слово World
      failed_when: results.rc != 0 # - ошибка если return code не равен 0

    - debug:
        var: results

    - name: read file
      shell: cat /home/joos/file1.txt # - на первом сервера этого файла нет - он отвалится и не выполнит следующую таску, но если any_errors_fatal: true то остановится выполнение
      register: myresult

    - name: task Number3
      shell: echo Hi Mir!!
```
**Хранение Секретов - ansible-vault**
```
ansible-vault create mysec.txt
ansible-vault view mysec.txt
ansible-vault edit mysec.txt
ansible-vault rekey mysec.txt - смена пароля
ansible-vault encrypt playbook-secret.yml - шифруем целый файл
ansible-vault decrypt playbook-secret.yml - расшифровываем целый файл
ansible-playbook playbook-secret.yml --ask-vault-pass - запускаем с запросом пароля
ansible-playbook playbook-secret.yml --vault-password-file mypass.txt - кладем пароль в файл и запускаем указывая файл
ansible-vault encrypt_string - запускаем и вводим строку которую надо зашифровать, !vault | - копируем все и ставим вместо пароля
ansible-vault encrypt_string --stdin-name "password" - как и выше, просто напишет перед блоком !vault - passwortd:
echo -n "tut_my_parol" | ansible-vault encrypt_string - сразу выводит зашифрованный tut_my_parol после того как мы введи vault пароль
```
**Dynamic Inventory AWS**

https://docs.ansible.com/ansible/latest/inventory_guide/intro_dynamic_inventory.html
