# Terraform

**Инфраструктура как код (IaC)**

**IaC** — модель, по которой процесс настройки инфраструктуры аналогичен процессу программирования ПО. Приложения могут содержать скрипты, которые создают свои собственные виртуальные машины и управляют ими. Это основа облачных вычислений и неотъемлемая часть DevOps.

**Недостатки IaC**
- Разработка IaC может потребовать использования дополнительных утилит, а любые ошибки при таком проектировании могут быть быстро распространены по всем окружениям проекта, поэтому IaC должен быть всесторонне протестирован.
- Конфигурация окружения была изменена администратором без внесения соответствующих изменений в IaC, поэтому особенно важно полностью интегрировать IAC в процесс системного администрирования, во все IT и DevOps-процессы и вести документацию.

**Проблемы IaC**

- **1. В большинстве случаев IaC – это какой-то dsl.** А DSL, в свою очередь, – это описание структуры. В них может не быть таких привычных нам вещей, как: переменные, условия, комментарии (например, JSON), функции, ООП как высокоуровневые конструкции. В Terraform используется **HCL** — это язык программирования, разработанный HashiCorp. Он в основном используется DevOps. Представляет собой методологию разработки, предназначенную для ускорения процесса кодирования. HCL используется для настройки программных сред и программных библиотек. HCL совместим с JSON благодаря API HCL. Его дизайн и синтаксис более читабельны.
- **2. Гетерогенная среда.** - Обычно вы работаете с одним языком, одним стеком, одной экосистемой. А тут огромное разнообразие технологий. Вполне реальная ситуация, когда баш с питоном запускает какой-то процесс, в который подсовывается Json. Вы его анализируете, потом еще какой-то генератор выдает ещё 30 файлов. Итог прост: нужны эникейщики, а это бывает очень накладно.
- **3. Тулинг** - Обычно, есть какая-то крутая штука (tool), позволяющая подсветить ошибку, или то, что можно использовать. Но, как часто бывает, в OpenSource это так не работает. К примеру, проект может состоять из 20-30 файлов, которые используют те или иные объекты друг из друга, но проблема в деталях (где-то забыли про структуру). Тулзы создают, но не всегда успевают за версиями, и тд тп. Хорошо, когда есть что-то типа X-Code, VSCode.

Основные игроки: Vagrant, Ansible, Packer, Terraform, SaltStack, Chef

**Terraform** - это программный инструмент с открытым исходным кодом, созданный HashiCorp, который помогает управлять инфраструктурой. Пользователи определяют и предоставляют инфраструктуру центра обработки данных с помощью декларативного языка конфигурации, известного как язык конфигурации HashiCorp (HCL) или необязательно, JSON.

**Terraform: особенности**
- оркестрирование, а не просто конфигурация инфраструктуры;
- построение неизменяемой инфраструктуры;
- декларативный, а не процедурный код;
- архитектура, работающая на стороне клиента

- **Оркестрирование** — автоматизированный процесс управления связанными сущностями, такими как группы виртуальных машин или контейнеров. Terraform сконцентрирован на задачах по созданию серверов с нуля, оставляя работу по размещению контейнеров с ПО платформам типа Docker или Packer. Когда вся инфраструктура вашей облачной экосистемы работает как код, и все параметры записаны в декларативные файлы конфигурации, специалисты вашей команды могут работать с ними и изменять эти файлы, как и любой другой код.
- **Неизменяемая VS одноразовая** - Основное отличие состоит в том, что неизменяемая инфраструктура привязана к железу, программе, компонентам в ней, а если нам что-то надо изменить, то мы каждый раз создаем новую инфраструктуру. Одноразовость заключается в том, что если она стала не нужна, то ее просто удалить. Жизненный цикл закончился, за нее не держимся, а просто убираем.
- **Неизменяемая инфраструктура** - Terraform работает по концепции неизменяемой инфраструктуры, где каждое изменение какого-либо компонента (обновление ПО, удаление или добавление компонентов, и т.д.) приводит к созданию отдельного состояния инфраструктуры, то есть к построению новой системы и удалению предыдущей конфигурации. Это означает, что процесс обновления ПО идет легко и быстро по всей системе сразу и защищен от возможных ошибок.
- **Декларативный код** - При помощи Terraform, вы просто указываете утилите, какие изменения нужно произвести с ТЕКУЩИМ состоянием системы, что позволяет обходиться довольно компактной и очень простой библиотекой шаблонов кода. При использовании Chef или Ansible вы вынуждены писать пошаговые процедурные инструкции по достижению требуемого состояния системы. Напротив, Terraform, Salt или Puppet предпочитают отписывать конечные состояния системы, оставляя конфигурацию на усмотрение утилиты.
- **Архитектура, работающая на стороне клиента** - Terraform использует API, предоставляемые поставщиком облачного хостинга. С их помощью утилита строит инфраструктуру, что означает: уход от излишних проверок безопасности, отсутствие необходимости в работе отдельного сервера для управления конфигурациями и выделение ресурсов на работу многочисленных программ-агентов.

**Недостатки Terraform**
- Так как Terraform появился относительно недавно, он еще далеко не идеален.
- Как дирижерскую палочку может использовать только один дирижер, так и Terraform желательно использовать одному оператору или хотя бы с одного терминала.
- Утилита была разработана только под облако и непригодна для использования с кластерами выделенных серверов.

**Преимущества Terraform**

Два основных преимущества Terraform:
- Супер-портативность — одна утилита и один язык применяется для описания облачной инфраструктуры в Google Cloud, AWS, OpenStack и работы с ЛЮБЫМ другим поставщиком услуг. Смена поставщика облачного хостинга больше никогда не будет проблемой.
- Простота полноценного запуска приложений. Предположим, на ваших серверах Amazon запущены Docker контейнеры под управлением Kubernetes, в которых работает широкий спектр приложений, и все это с легкостью управляется с помощью одного инструмента.

**Terraform — install**

1) Установка через repository git
```
cd /usr/local/src && git clone https://github.com/hashicorp/terraform.git
```
2) Вручную
```
cd /usr/local/src && curl -O
https://releases.hashicorp.com/terraform/0.15.4/terraform_0.15.4_linux_arm.zip
```
3) Установка через repository apt
```
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=$(dpkg --print-architecture)] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt install terraform
```
Яндекс
```
https://github.com/netology-code/devops-materials
https://hashicorp-releases.yandexcloud.net/terraform/
https://cloud.yandex.ru/docs/tutorials/infrastructure-management/terraform-quickstart#configure-terraform

wget https://hashicorp-releases.yandexcloud.net/terraform/1.5.4/terraform_1.5.4_linux_amd64.zip
zcat terraform_1.5.4_linux_amd64.zip > terraform
chmod -x terraform / chmod 766 terraform
./terraform -v
cp terraform /usr/local/bin/
cd ~
nano .terraformrc
---
provider_installation {
  network_mirror {
    url = "https://terraform-mirror.yandexcloud.net/"
    include = ["registry.terraform.io/*/*"]
  }
  direct {
    exclude = ["registry.terraform.io/*/*"]
  }
}
---
terraform init
```

**Terraform настройка**

Terraform использует конфигурационные файлы с расширением .tf.

Заносим минимальный набор переменных для успешного подключения:
```tf
provider "Имя провайдера" {
  user = "ваш_логин"
  password = "ваш_пароль"
  org = "название_организации"
  url = "название сайта с api "
}
```
Сохраняем конфигурацию, и пробуем подключиться:
```
terraform init
```
Запускать надо там, где находится файл xxx.tf Пример кода доступен в репозитории GitHub.

https://github.com/wallarm/terraform-example/tree/master/terraform

**Синтаксис Terraform**
- Однострочные комментарии начинаются с #
- Многострочные комментарии заключаются в символы /* и */
- Значения присваиваются с помощью синтаксиса key = value (пробелы не имеют значения). Значение может быть любым примитивом (строка, число, логическое значение), списком или картой.
- Строки в двойных кавычках.
- Строки могут интерполировать другие значения, используя синтаксис, заключенный в ${} , например ${var.foo} .
- Многострочные конструкции могут использовать синтаксис «здесь, документ» в стиле оболочки, при этом строка начинается с маркера типа <<EOF , а затем строка заканчивается на EOF в отдельной строке. Строки и конечный маркер не должны иметь отступа.
- Предполагается, что числа имеют основание 10. Если вы поставите перед числом префикс 0x , оно будет рассматриваться как шестнадцатеричное число.
- Логические значения: true, false.
- Списки примитивных типов можно составлять с помощью квадратных скобок ( [] ). Пример: ["foo", "bar", "baz"] .
- Карты могут быть сделаны с фигурными скобками ( {} ) и двоеточие ( : ): { "foo": "bar", "bar": "baz" } . Кавычки могут быть опущены на ключах, если ключ не начинается с числа, и в этом случае кавычки требуются. Для однострочных карт необходимы запятые между парами ключ/значение. В многострочных картах достаточно новой строки между парами ключ/значение.

**Переменные User string**

var. префикс, за которым следует имя переменной. Например, ${var.foo} интерполирует значение переменной foo.

**Переменные карты пользователя**

Синтаксис: var.MAP["KEY"] . Например, ${var.amis["us-east-1"]} получит значение ключа us-east-1 в переменной карты amis.

**Переменные списка пользователей**

Синтаксис: "${var.LIST}" . Например, "${var.subnets}" получит значение списка subnets в виде списка. А также может возвращать элементы списка по индексу: ${var.subnets[idx]}.

**Условные операторы троичная операция:**
```
CONDITION ? TRUEVAL : FALSEVAL
```
**Пример:**
```
resource "aws_instance" "web" {
subnet = "${var.env == "production" ? var.prod_subnet : var.dev_subnet}"
```
**Поддерживаемые операторы:**
- Равенство: == и !=
- Численное сравнение: > , < , >= , <=
- Логическая логика: && , || , одинарный !

**Математика :**
Поддерживаемые операции:
- Сложение ( + ), вычитание ( - ), умножение ( * ) и деление ( / ) для типов с плавающей запятой.
- Сложение ( + ), Вычитание ( - ), Умножение ( * ), Разделение ( / ) и По модулю ( % ) для целочисленных типов.
- Приоритеты операторов — это стандартный математический порядок операций: умножение ( * ), деление ( / ) и модуль ( % ) имеют приоритет над сложением ( + ) и вычитанием ( - ). Круглые скобки могут использоваться для принудительного упорядочивания.

# Terraform2

**Простейшая конфигурация состоит из четырех файлов:**
- main.tf — основной конфиг, описывающий какие инстансы нам нужны;
- variables.tf — конфиг с описанием переменных и значениями по дефолту, если дефолтных значений нет, то они являются обязательными;
- terraform.tfvars — конфиг со значением переменных, часто является секретным, нужно с осторожностью пушить в публичные репозитории;
- outputs.tf — описание выходных переменных, необязательный файл, но очень удобно вычленять нужные параметры из созданного инстанса, например IP созданного в облаке инстанса.

**Пример конфигурационного файла main.tf**
```tf
provider "yandex" {
  token = var.oauth_token
  cloud_id = var.cloud_id
  folder_id = var.folder_id
  zone = "ru-central1-a"
}
```
**Providers** (конкретное облако — GCP, DO, AWS, YANDEX и др.):
- содержат настройки аутентификации и подключения к платформе или сервису
- предоставляют набор ресурсов для управления
- установка провайдера производится командой: terraform init 
```tf
resource "yandex_vpc_subnet" "subnet_a" {
  zone = "ru-central1-a"
  network_id = yandex_vpc_network.test_network.id
  v4_cidr_blocks = ["10.1.0.0/24"]
}
resource "yandex_vpc_subnet" "subnet_b" {
  zone = "ru-central1-b"
  network_id = yandex_vpc_network.test_network.id
  v4_cidr_blocks = ["10.2.0.0/24"]
}
```
**Resources:**
- определяются типом провайдера;
- позволяют управлять компонентами платформы или сервиса;
- могут иметь обязательные и необязательные аргументы;
- могут ссылаться на другие ресурсы;
- комбинация тип ресурса + имя уникально идентифицирует ресурс в рамках данной конфигурации.

**Input-переменные variables.tf**

Позволяют параметризировать конфигурационные файлы. Четыре типа:
- string
- map
- list
- boolean
Можно передать из файла, из окружения, из командной строки или интерактивно.
```tf
variable project {
  description = "Project ID"
}
variable region {
  description = "ru-central1-c"
  default = "europe-west1"
}
```
**Задание переменных через файл**

Указываем переменные в файле:
```
my-vars.tfvars: project = "infra-179015" public_key_path = "~/.ssh/appuser.pub"
```
Указываем путь до файла при запуске команд terraform:
```
$ terraform apply -var-file=my-vars.tfvars
```
Если файл называется **terraform.tfvars**, то переменные загружаются автоматически.

**Output-переменные outputs.tf**

- позволяют сохранить выходные значения после создания ресурсов;
- облегчают процедуру поиска нужных данных;
- используются в модулях как входные переменные для других модулей.
```tf
outputs.tf
output "app_external-ip {
value="${yandex_compute_instance.app.network_interface.0.access_config.0.assigne
d_nat_ip}"
}
// просмотр
$ terraform output
app_external_ip = 104.199.54.241
```
**Основные команды**
- Для приведения системы в целевое состояние используется команда: **terraform -auto-approve=true apply** — идемпотентна!
- Для просмотра изменений, которые будут применены: **terraform plan**
- Для обновления конфигурации: **terraform refresh**
- Для просмотра выходных переменных: **terraform output**
- Для пересоздания ресурса: **terraform taint yandex_compute_instance.app**

**Yandex.Cloud** — это набор связанных сервисов, доступных через интернет, которые помогают быстро и безопасно взять в аренду вычислительные мощности в тех объемах, в которых это необходимо. Такой подход к потреблению вычислительных ресурсов называется облачные вычисления. Облачные вычисления заменяют и дополняют традиционные дата-центры, расположенные на территории потребителя. Yandex.Cloud берет на себя задачи по поддержанию работоспособности и производительности аппаратного и программного обеспечения облачной платформы. Yandex.Cloud предлагает различные категории облачных ресурсов: например, виртуальные машины, диски, базы данных. Управлять ресурсами каждой категории можно с помощью соответствующего сервиса. Список сервисов в составе Yandex.Cloud - https://cloud.yandex.ru/docs/overview/concepts/services

**Метки ресурсов сервисов**

**Метка** — это пара ключ-значение в формате: **<имя метки>=<значение метки>**. Вы можете использовать метки для логического разделения ресурсов.

На метку накладывается следующее ограничение:
- Максимальное количество меток на один ресурс: 64.
На ключ:
- Длина — от 1 до 63 символов.

**Создание инфраструктуры**
- создать сервисный аккаунт (https://cloud.yandex.ru/docs/iam/operations/sa/create) с ролью editor на каталог, указанный в настройках провайдера.
- получить статический ключ доступа (https://cloud.yandex.ru/docs/iam/concepts/authorization/access-key). Сохранить идентификатор ключа и секретный ключ, они понадобятся в следующих разделах инструкции.

Создаем файл main.tf
```tf
terraform {
  required_providers {
    yandex = {
      source = "yandex-cloud/yandex"
    }
  }
}
provider "yandex" {
  token = "AxxxxxxQ"
  cloud_id = "P9-PFNhiz9b4STN6tpvA"
  folder_id = "b1gquhcfohdpq9q9r64k"
  zone = "ru-central1-a"
}
```
Запускаем terraform init

С помощью Terraform в Yandex.Cloud можно создавать облачные ресурсы всех типов: виртуальные машины, диски, образы и т.д.

По плану будут созданы:
- две виртуальные машины: terraform1 и terraform2,
- облачная сеть network-1 с подсетью subnet-1.

У машин будут разные количества ядер и объемы памяти:
- terraform1: 2 ядра и 2 Гб оперативной памяти,
- terraform2: 4 ядра и 4 Гб оперативной памяти.
Машины автоматически получат публичные IP-адреса и приватные IP-адреса из диапазона 192.168.10.0/24 в подсети subnet-1, находящейся в зоне доступности ru-central1-a и принадлежащей облачной сети network-1

Конфигурации ресурсов задаются сразу после конфигурации провайдера:
```tf
resource “yandex_compute_instance” “vm-1” {
  name = “terraform1”
  resources {
    cores = 2
    memory = 2
  }
  boot_disk {
    initialize_params {
    image_id = “fd87va5cc00gaq2f5qfb”
    }
}
}
```
**Создайте пользователей**

Чтобы добавить пользователя на создаваемую ВМ, в блоке metadata укажите параметр user-data с пользовательскими метаданными. Для этого создаем файл с мета информацией:
```
#cloud-config
users:
  - name: user
    groups: sudo
    shell: /bin/bash
    sudo: [‘ALL=(ALL) NOPASSWD:ALL’]
    ssh-authorized-keys:
      - ssh-rsa xxxxxxxxxx xxxx@example.com
```
В файле main.tf вместо ssh-keys задайте параметр user-data и укажите путь к файлу с метаданными:
```
# metadata = {
# ssh-key = “ubuntu:{file(“~/.ssh/id_rsa.pub)}”
#}
metadata = {
    user-data = “${file(“./meta.txt”)}”
}
```
**Проверьте и отформатируйте файлы конфигураций** - Проверьте конфигурацию командой: **terraform validate**

Если конфигурация является допустимой, появится сообщение:
```
root@Netalogy:/home/user/terraform/test# terraform validate Success! The
configuration is valid.
```
Отформатируйте файлы конфигураций в текущем каталоге и подкаталогах: **terraform fmt**
```
root@Netalogy:/home/user/terraform/test# terraform ftm main.tf
```
**Создание ресурсов**

После проверки конфигурации выполните команду: **terraform plan**

Если будут ошибки, на них укажут видом:
```
Ошибка: синтаксическая ошибка в файле конфигурации в строке
main.tf 6, в переменной "имя_группы ресурсов": 6: тип = строка
```
Чтобы создать ресурсы выполните команду: **terraform apply**

Вывод терминала должен выдать:
```
Apply complete! Resources: 4 added, 0 changed, 0 destroyed.
```
**Удаление ресурсов**

Чтобы удалить ресурсы, созданные с помощью Terraform: Выполните команду: **terraform destroy**

Дополнительные материалы
- https://cloud.yandex.ru/docs/tutorials/infrastructure-management/terraform-quickstart
- https://cloud.yandex.ru/docs/tutorials/infrastructure-management/terraform-state-storage
- https://cloud.yandex.ru/docs/tutorials/infrastructure-management/packer-quickstart
- https://registry.terraform.io/providers/yandex-cloud/yandex/latest/docs

# Terraform3

Установка
```bash
wget https://hashicorp-releases.yandexcloud.net/terraform/1.5.4/terraform_1.5.4_linux_amd64.zip
zcat terraform_1.5.4_linux_amd64.zip > terraform
chmod -x terraform
chmod 766 terraform
./terraform -v
cp terraform /usr/local/bin/
cd ~
nano .terraformrc
---
provider_installation {
  network_mirror {
    url = "https://terraform-mirror.yandexcloud.net/"
    include = ["registry.terraform.io/*/*"]
  }
  direct {
    exclude = ["registry.terraform.io/*/*"]
  }
}
```
main.tf
```tf
terraform {
  required_providers {
    yandex = {
      source = "yandex-cloud/yandex"
    }
  }
  required_version = ">= 0.13"
}

provider "yandex" {
  zone = "ru-central1-a"
}
```
**terraform init**

**Создаем 3 компьютера**
```tf
variable "name" {
    default = ["name1", "name2", "name3"]
}

resource "yandex_compute_instance" "vm-1" {
  for_each    = toset(var.name)
  name        = each.key
  platform_id = "standard-v3"
  resources {
    core_fraction = 20
    cores     = 2
    memory    = 2
  }

```
**Или**
```tf
resource "yandex_compute_instance" "vm-1" {
  count = 3
  name  = "terr-${count.index}"
  platform_id = "standard-v3"
  resources {
    core_fraction = 20
    cores  = 2
    memory = 2
  }
```
**Подключаем исполняемый файл**
```tf
  metadata = {
    user-data = "${file("./my.sh")}"
  }
```
my.sh
```bash
#!/bin/bash
cat /etc/*rel*
yum -y update
yum -y install httpd
yum -y curl
myip=`curl http://ifconfig.me`
echo "<h2>WebServer with IP: $myip</h2><br>Build by Terraform!" > /var/www/html>
sudo service httpd start
chkconfig httpd on
```
**Использование Динамичных внешних файлов - templatefile**
```tf
  metadata = {
    user-data = templatefile("my.tpl",{
    f_name = "John",
    l_name = "Os",
    names = ["Vasja","Petja","Tom","Peter","Uta" ]
    })
  }
```
my.tpl
```tpl
#!/bin/bash
cat /etc/*rel*
yum -y update
yum -y install httpd

cat <<EOF > /var/www/html/index.html
<html>
<h2>Build by Power of Terraform <font color="red"> v0.999</font></h2><br>
Owner ${f_name} ${l_name} <br>
%{ for x in names ~}
Hello to ${x}
%{ endfor ~}
</html>
EOF

sudo service httpd start
chkconfig httpd on
```
Проверка без сервера
```
terraform console - Запускает консоль
templatefile("my.tpl",{f_name = "John", l_name = "Os", names = ["Vasja","Petja","Tom","Peter","Uta" ] }) - выведет что будет изменено
```
**LifeCycle - AWS**
```tf
не дает удалять сервер или компоненты
lifecycle {
  prevent_destroy = true
}

игнорирует изменения и не перезапускает сервер
lifecycle {
  ignore_changes = ["user_date"]
}

Создаем и привязываем статический ip и настройка - сначало поднимаем, потом убиваем сервер
resource "aws_eip" "my_static_ip" {
instance = aws_instance.my_server.id
}
lifecycle {
  create_before_destroy = true
}
```
**outputs.tf**
```tf
output "internal_ip_address_vm-1" {
  value = yandex_compute_instance.vm-1.network_interface.0.ip_address
}
output "external_ip_address_vm-1" {
  value = yandex_compute_instance.vm-1.network_interface.0.nat_ip_address
}
```
```
terraform show - покажет что получилось apply в том числе и outputs
```
**Порядок создания Ресурсов**
```
depends_on = [yandex_compute_instance.vm-1, server_name2]
```
**Получение данных с помощью Data Source**

https://registry.terraform.io/providers/yandex-cloud/yandex/latest/docs/data-sources/

**Использование Переменных - variables**

variables.tf
```tf
variable "region" {
description = "Please enter Region AWS"
default     = "ca-central-1"
}

variable "platform_id" {
description = "Enter platform Yandex"
type = string
default = "standard-v3"
}
```
**Автозаполнение Переменных - tfvars**
```
terraform apply -var="platform_id=standart-v2" - при запуске

EXPORT TF_VAR_platform_id=standart-v2 - пишем в сессию терминала и можем при запуске ничего не вводить, терраформ подхватит сам
ent | grep TF_VAR_ - смотрим
unset TF_VAR_platform_id - удаляет var из сессии
```
terraform.tfvars - приоритетнее variables.tf - может использоваться для продакшена

**Локальные Переменные - locals**
```tf
Создаем из переменных в variables
locals {
full_name = "${var.environment} - ${var.project_name}"
}

Используем
Project = local.full.name
```
**Запуск локальных команд - exec-local** (на компе с терраформом)
```tf
resource "null_resource" "command1" {
    provisioner "local-exec" {
        command = "echo Terraform START: $(date) >> log.txt"
    }
}
Запуск через Python
resource "null_resource" "command2" {
    provisioner "local-exec" {
        command = "print('Hello World!')"
        interpreter = ["python", "-c"]
    }
}

resource "null_resource" "command2" {
    provisioner "local-exec" {
        command = "echo $NAME1 $NAME2 $NAME3 >> names.txt"
        environment {
          NAME1 = "Vasya"
          NAME2 = "Petja"
          NAME3 = "Tanja"
        }
    }
}

provisioner можно запихнуть в создание компьютера и он выведет при создании эту команду
```
**Использовние Conditions и Lookups**

**Conditions**
```tf
variable "env" {
  default = "prod"
}

 platform_id = var.env == "prod" ? "standard-v3" : "standard-v2"

count = var.env == "prod" ? 1 : 0
```
**Lookups**
```tf
variable "look" {
    default = {
        "prod" = "standard-v3"
        "staging" = "standard-v2"
        "dev" = "standard-v1"
    }

platform_id = lookup(var.look, var.env)

```
**Использование циклов - count, for**

**count**
```tf
variable "aws_users" {
  default = ["petja", "vasja", "kolja", "otto", "maga", "john", "joos"]
}

resource "aws_iam_user" "users" {
  count = length(var.aws_users)
  name = element(var.aws_users, count.index)
}
```
**for**
```tf
output "created_iam_users_all" {
  value = aws_iam_user.users
}

output "created_iam_users_id" {
  value = aws_iam_user.users[*].id
}

output "created_iam_users_custom" {
  value = [
    for user in aws_iam_user.users:
      "Username: ${user.name} has ARN:${user.arn}"
  ]
}

output "created_iam_users_custom" {
  value = [
    for user in aws_iam_user.users:
       user.unique_id => user.id
  ]
}

output "created_iam_users_custom" {
  value = [
    for x in aws_iam_user.users:
      x.user
    if length(x.name) == 4
  ]
}

output "server_id_ip" {
  value = {
    for server in aws_instance.servers :
      server.id => server.public.ip
  }
}

output "all_info_server" {
  value = yandex_compute_instance.vm-1[*]
}

output "id_ip_servers" {
  value = {
    for server in yandex_compute_instance.vm-1:
    server.id => server.network_interface.0.nat_ip_address
  }
}
```
**Использование Terraform Remote State** - для работы нескольких человек / сохранение файла о состоянии в облаке
```tf
terraform {
  required_providers {
    yandex = {
      source = "yandex-cloud/yandex"
    }
  }
  required_version = ">= 0.13"

    backend "s3" {
    endpoint   = "storage.yandexcloud.net"
    bucket     = "test-joos"
    region     = "ru-central1"
    key        = "terraform1.tfstate"
    shared_credentials_file = "storage.key" # - у пользователя создаем новый статический ключ и копируем в файл

    skip_region_validation      = true
    skip_credentials_validation = true
  }
}
```
**Создание Модулей**

https://cloud.yandex.ru/docs/tutorials/infrastructure-management/terraform-modules

https://github.com/terraform-yc-modules

**Global Variables**

Делаем outputs через бакеты и потом обращаемся к ним.

https://www.youtube.com/watch?v=nFmOqZxah_Q&list=PLg5SS_4L6LYujWDTYb-Zbofdl44Jxb2l8&index=34

**Как управлять ресурсами созданными вручную**
```
terraform import aws_instans.myname server-id

terraform import yandex_compute_instance.test epdsei3idc7heqpqonis
```
Создает файл .tfstate, но в данной версии не меняет main.tf, для дальнейшей работы надо редактировать main.tf
```
terraform apply
```
Смотрим ошибки которые выдает - добавляем нужное, потом смотрим из-за чего хочет убить сервер и правим файл чтобы apply был без изменений

**Как пересоздать ресурс до v0.15.2**
```
terraform taint yandex_compute_instance.vm
помечаем ресурс taint - при следующем apply терраформ удалит и создаст его
```
**Как пересоздать ресурс после v0.15.2**
```
terraform apply -replace  yandex_compute_instance.vm
сразу пересоздает ресурс
```
**terraform state команды**
```
terraform state show yandex_compute_instance.vm - покажет конфигурацию выбранного ресурса
terraform state list - покажет все ресурсы которые есть
terraform state pull - покажет все конфигурации по всем ресурсам
terraform state rm yandex_vpc_network.network-1 -  удаление ресурса из удаленного репозитория
terraform state mv -state-out="terraform.tfstate" yandex_vpc_network.network-1 yandex_vpc_network.network-1 - перемещение (и удаление) из удаленного файла (в бакете) в локальный terraform.tfstate

for i in $(terraform state list); do terraform state mv -state-out="terraform.tfstate" $i $i; done - в linux перебираем все ресурсы и перемещаем их в локальный terraform.tfstate

terraform state push terraform.tfstate - перезаписывает удаленный файл из локального
```
**Terraform Workspaces**
```
terraform workspace - используем для тестирования, проверки перед продакшеном,при создании нового workspace терраформ создаст еще раз ресурсы, они будут находится в другом workspace и не пересекаться с default
terraform workspace show - покажет текущий workspace
terraform workspace list - покажет все workspaces 
terraform workspace new workspace_name - создаем новый workspace с именем workspace_name
terraform workspace select - переходим в workspace
terraform workspace delete - удаляем workspace
```
**Terraform Custom Provider Domino's Pizza**

https://github.com/nat-henderson/terraform-provider-dominos

https://nat-henderson.github.io/terraform-provider-dominos/

Как указать кастомного провайдера
```
terraform {
  required_providers{
    dominos = {
      source = "daminos.edu/myorg/dominos" // ~/.terraform.d/plugins/dominos.com/myorg/dominos/1.0/linux_amd64/
      version = "~>1.0"
    }
  }
}
```
**Как импортировать ресурсы полуавтоматически используя блок import**
```
import {
  id = "instance ID"
  to = "yandex_compute_instance.vm"
}

terraform plan -generate-config-out=generated.tf
```

**DevOps с Nixys | Знакомство с Terraform**

- terraform apply -auto-approve - создание с автоподтверждением
- terraform fmt - автоформатирование
- terraform destroy -target yandex_compute_instance.nixys - уничтожение только определенного инстанса
- .tf - файлы конфигурации
- .tfvars - файлы переменных
- .tfstate - файлы текущего состояния инфраструктуры
- .terraform - скаченные провайдеры, указанный в конфигурации
- .terraform.lock.hcl - описываются зависимости модулей и провайдеров
