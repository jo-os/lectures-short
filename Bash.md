# Bash

**B**ourne-**a**gain **sh**ell - «возрождённый» shell

**Bash** - самая популярная оболочка командной строки для Linux, имеет широкий набор встроенных функций, таких как циклы, условия и прочие

**Переменная** - именованный участок памяти, в котором содержатся какие-либо данные: текст, числа и т. д

Способы присваивания переменных
- Прямое: a=2
- Чтение ввода пользователя: read a
- Как результат выполнения команды: a=`cat /etc/hostname`, a=$(cat /etc/hostname)

<details>
  <summary>Пример</summary>
  
```
#!/bin/bash
botname=Linux
echo "What is your name?"
read name
server=`cat /etc/hostname`
echo "Hello $name, my name is $botname"
echo "Welcome to the $server"
```
</details>

Переменные и математическиеоперации
- Обычные переменные (текст): a=2+2
- Математические операции в переменных: let a=2+2 или ((a=2+2))
- Подстановка математических операций в строку: echo "4 + 4 = $((4+4))"
<details>
  <summary>Пример</summary>
  
```bash
a=2+2; echo $a
2+2
let a=2+2 или ((a=2+2)); echo $a
4
echo "4 + 4 = $((4+4))"
4+4 = 8
```
</details>

В Bash существует сокращённый синтаксис для инкрементов и математической модификации существующих переменных
```
((a++)) ((a--)) ((a*=3)) ((a/=4)) 
```
**Bash** - умеет работать только с целыми числами, при делении получаем целую часть от числа

Специальные символы языка шаблонов
- Звёздочка (*)
  - Означает любое количество любых символов
  - Заменяется на список всех имён файлов в директории
  - Можно использовать как фильтр: ```echo *.jpg  /  echo /etc/*```
- Знак вопроса
  - Вопросительный знак означает один любой символ
  - Выведет в консоль файлы, в названии которых две буквы и расширение .avi ```echo ??.avi```
 
**Экранирование символов** - замена в тексте управляющих символов на соответствующие текстовые подстановки

- Для экранирования специальных символов используется знак обратного слеша \ - ```echo \?\?.avi``` = ```??.avi```
- Это позволяет интерпретировать специальные символы как текст ```echo 100\$moneys``` = ```100$moneys```
- Экранировать можно пробелы, на каждый пробел нужен экран ```echo \ \ two \ \ space \ \``` = ```two space```
- Для экранирования специальных символов используются правильные кавычки ```Это важно для специального символа $```
- В двойных кавычках происходит подстановка переменной ```moneys=11l;  echo"100$moneys"``` = ```100111```
- В одинарных кавычках — нет echo ```'100$moneys'``` = ```100$moneys```
- Для других символов достаточно заключить текст в одинарные или двойные кавычки, он будет интерпретирован как текст, а не как специальный символ

**Код возврата (exit code)** - код, который возвращает команда или функция после выполнения

Код возврата предыдущей команды хранится в переменной **$?**
- 0 — успех
- Любое другое число, как правило, — код ошибки (свой у каждого приложения)
```
cat /etc/not_existing_file
echo $?
```
Условные операторы в Bash
- if - используется для бинарной проверки (true/false)
- case - используется для сравнения строки, содержащейся в переменной с шаблоном

Логические операторы
- и - &&
- или - ||
```
if [[ cmd1 ]] && [[ cmd2 ]]; then
elif [[ cmd3 || cmd4 ]]; then
else
fi
```
**Работа с файлами**
- -e — проверить, существует ли файл или директория
- -f — файл существует (!-f — не существует)
- -d — каталог существует (!-d — не существует)
- -s — файл существует, и он не пустой
- -r — файл существует и доступен для чтения
- -w — ... для записи
- -x — ... для выполнения
- -h — символьная ссылка

**Работа со строками**
- -z — пустая строка
- -n — не пустая строка
- == — равно
- != — не равно

**Операции с числами**
- -eq — равно
- -ne — не равно
- -lt — меньше
- -le — меньше или равно
- -gt — больше
- -ge — больше или равно

**[[ Двойные квадратные скобки ]]** - ключевые слова языка Bash, в зависимости от своего содержимого, они возвращают код выхода
```
[[ -d /tmp ]]  [[ $a -eq 8 ]]
```
Эти условия возвращают код 0 или 1 в зависимости от того, верно условие или ложно.

Инверсия результата достигается с помощью «!».

Для упрощения записи команд есть, 3 оператора позволяющие избежать ненужного усложнения с помощью if
- **команда1 ; команда2** - команда2 выполняется после команды1 независимо от результата её работы
- **команда1 && команда2** - команда2 выполняется только после успешного выполнения команды1 (т. е. с кодом завершения 0) — это аналог операции AND (логическое И)
- **команда1 || команда2** - команда2 выполняется только после неудачного выполнения команды1 (т. е. код завершения команды1 будет отличным от 0) — это аналог операции OR (логическое ИЛИ)

В cаse можно использовать:
- Специальные символы и * ?
- Несколько шаблонов через разделитель |
<details>
  <summary>Пример</summary>

```
case $variable in condition1)
test.png|*.jpg)
esac
```
```
if [[ $port == '80' || $port == '8080' ]]; then
echo "This is HTTP"
elif [[ $port == '22' ]]; then
echo "This is SSH"
elif [[ $port == '53' ]]; then
echo "And this is DNS"
else
echo "I dont know what is that"
fi;
```
```
case "$port" in
'80'|'8080')
echo "This is HTTP"
;;
'22')
echо "This is SSH"
;;
'53')
echo "And this is DNS"
*)
echo "I dont know what is that"
;;
esac
```
</details>

**Отладка** - этап разработки программы, на котором разработчик обнаруживает, локализует и устраняет ошибки

Интерпретатору /bin/bash можно передавать параметры работы:
- –v - выводить все строки по мере их обработки интерпретатором
- –x - выводить все команды и их аргументы по мере их выполнения
```
bash -x script.sh
```
Внутри скрипта
```
#!/bin/bash
set -x
```

**Передача аргументов скрипту**
- Аргументы передаются при запуске скрипта после его названия
  - ./script.sh param1 param2
  - bash script.sh param1 param2
- Доступ осуществляется через служебные переменные $1, $2, $3 и т. д.
  - $0 содержит название скрипта

**Цикл while** - позволяет выполнить одну и ту же последовательность действий, пока проверяемое условие истинно
```
while [[ условие ]]; do
echo "Ok"
done
```
Цикл **for** - позволяет проводить итерации — реализовывать набор инструкций нужное количество раз. For используем, когда нет условия, а есть список элементов
```
for имя in элемент1 элемент2 do
...
done
```
```
for item in coffee tea; do
echo "We have a $item"
done
```
Повторение тела цикла определённое количество раз
```
#!/bin/bash
for ((i=1; i < 10; i++))
do
echo $i
done
```
**Конструкция Bash select** - используется для создания нумерованного меню из списка пунктов

Синтаксис похож на :select for
```
select имя in элемент1 элемент2 do
...
done
```
**Select** - даёт пользователю выбрать вариант из предложенных с помощью ввода цифры. Чаще всего используется вместе с case
<details>
  <summary>Пример</summary>
  
```
select pill in red blue; do
case $pill in
red)
echo "you will know the truth"; break;;
blue)
echo "you won't know anything"; break;;
esac
done
```
</details>

**Функции Bash** — часть кода, вынесенная отдельно для многократного использования

Объявление функции в Bash
```
имя_функции () {
команды
}
```
Вызов функции - имя_функции
```
#!/bin/bash
current_uptime () {
date
uptime
}
current_uptime
current_uptime
```
**Параметры** — доступные только внутри тела функции переменные

В переменной содержатся все аргументы, переданные функции или скрипту **$@** Эти аргументы часто используются для перебора всех параметров в цикле for
```
read_args() {
echo $@
for item in $@; do
echo $item
done
}
read_args param1 param2 param3
```
Код возврата в функции
- Выход из функции — с помощью return Х
- По умолчанию код возврата функции — 0

<details>
  <summary>Пример</summary>
  
```
#!/bin/bash
division () {
if [[ $2 -ne 0 ]]; then
echo "$1/$2 = $(($1/$2))"
return 0
else
echo "division by zero"
return 1
fi
}
division 15 5
division 4 2
division 3 0
```
</details>

- Указать код возврата для скрипта можно с помощью команды exit
- По умолчанию возвращает 0
- Можно задать любое другое значение с помощью exit X
```
#!/bin/bash
…
if [[ $error == “true” ]]; then
exit 1
else
exit 0
fi;
```

**set -e** завершит скрипт с ошибкой, в случае, если в нижеследующем bash коде будет обнаружена ошибка

**Команда set** устанавливает аттрибуты оболочки с опеределенных опций. 
- **Опция -e** - означает, что скрипт будет остановлен, когда произойдет ошибка в ходе его выполнения.
- **Опция -u** - означает, что скрипт будет остановлен, если в ходе скрипта, будет обнаружена переменная, которая не определена.
- **Опция -o pipefail** - означает, что скрипт будет остановлен, если в ходе пайплайна команд будет выявлена ошибка.

# Текстовые утилиты
Утилиты для просмотра текстовых файлов
- cat (concatenate) - cat [файл...] - читает содержимое одного или нескольких файлов и копирует его в стандартный вывод
- less - less -N file - позволяет пронумеровать строки
- head - Выводит первые 10 строк из текстового файла
- tail - Выводит последние 10 строк из текстового файла
- tail -f - позволяет следить за обновлением файла в режиме реального времени

Сортировка текстовых файлов
- sort - сортирует все указанные файлы, результат сортировки файлов отправляется на стандартный вывод
- sort [параметр] ... [файлы]
- -b - Пробелы в начале сортируемых полей или в начале ключей будут игнорироваться
- -d - При сортировке будут игнорироваться все символы, кроме букв, цифр и пробельных символов
- -f - Игнорирование регистра букв
- -r - Сортировка в обратном порядке
- -o файл - Вывод результатов сортировки в файл
- -t символ - Использование указанного символа в качестве разделителя полей
- -n - Сортировка числовых значений
- -h - Сортировка удобочитаемых цифр: 2K 1G 100M
- -k - Сортировка по указанному диапазону столбцов
- -u - Убрать повторы — аналог команды uniq

Разбивка файла
- split - используется для разделения файлов на части
- wc (word count) - используется для подсчёта слов в файле
  - wc -l /var/log/messages - Для подсчёта строк в текстовом файле
  - wc -c /var/log/messages - Для подсчёта байтов в текстовом файле
  - wc -l -w -L — вывести длину самой длинной строки
- cut - используется для извлечения фрагментов текста из строк и вывода их в стандартный вывод. может принимать имена файлов в аргументах или данные со стандартного ввода
- -c - Cписок символов: --characters=     Извлекает фрагмент строки, определяемый списком символов. Список может включать один или несколько числовых диапазонов, разделённых запятыми
- -f - Cписок полей: -fields=      Извлекает одно или несколько полей из строки, как определено аргументом списка символов. Список может включать одно или несколько полей или диапазонов полей, разделённых запятыми
- -d - Символ-разделитель: --delimiter=     В присутствии параметра -f в качестве разделителя полей используется символ-разделитель. По умолчанию поля должны отделяться друг от друга одним символом табуляции --complement. -d извлекает строку текста целиком, кроме фрагментов, определяемых параметром -c и/или -f
- cut -c1-3 /etc/passwd — диапазон символов
- cut -d':' -f1 /etc/passwd

Утилиты для поиска
- find - утилита для сложного поиска файлов. В простейшем случае программе find можно передать одно или несколько имён каталогов для поиска
- find /tmp | wc -l
- find ~ -type d | wc -l
- find ~ -type f | wc -l
- find ~ -type f -name "*.JPG" -size +1M | wc -l
- find /tmp -type f -size +5M -mtime +3 -delete
- locate - утилита для поиска файлов по имени в Linux. Выполняет быстрый поиск в базе данных имён файлов и выводит все имена, соответствующие искомой строке
- Locate -e — существующие файлы

Текстовые редакторы
- Vim — свободный текстовый редактор, созданный на основе более старого vi
- Режим вставки - Осуществляется редактирование текста
- Командный режим - Выполняется ввод специальных команд для работы с текстом
- :q! - Выйти без сохранения
- :w - Сохранить изменения
- :w <файл> - Сохранить изменения под именем <файл>
- :wq - Сохранить и выйти
- :q - Выйти, если нет изменений
- i - Перейти в режим вставки символов в позицию курсора
- a - Перейти в режим вставки символов в позицию после курсора

- nano — консольный текстовый редактор
- Ctrl + G - Подсказка по горячим клавишам
- Ctrl + X - Выйти из nano
- Ctrl + O - Сохранить файл
- Ctrl + W - Поиск
- Ctrl + \ - Замена
- Ctrl + K - Вырезать строку
- Ctrl + U - Вставить строку
- Alt + U - Отменить последнее действие
- Alt + E - Отменить последнее действие

# Regexp+
**Regexp** — формальный язык поиска и осуществления манипуляций с подстроками в тексте

Для ограничения начала и конца строки необходимо использовать специальные символы Эти символы не нужны для поиска совпадений с регулярным выражением внутри текста
- ^ — начало строки
- $ — конец строки

Метасимволы, используемые в Regexp
```
- . - Любой символ
- [ ] - Диапазон символов
- [^ ] - Все символы, кроме указанных в фигурных скобках
- * - Любое количество символов перед звёздочкой, в том числе ноль
- + - Одно или несколько стоящих перед ним выражений
- ? - Ноль или одно стоящее перед ним выражение
```
**Квантификатор** — указатель количества предшествующих выражений или символов
- Символ * - * = {0,}
- Символ + - + = {1,} 
- Символ ? - ? = {0,1}
- {n} - Непосредственное количество
- {n,} - n или больше
- {n,m} - Не меньше, чем n, и не больше, чем m
- {,m} - Не больше, чем m

Экранирование и группировка
- Для экранирования в регулярных выражениях используется специальный символ \
- Сам \ экранируется как \\
- Применить квантификатор к нескольким символам сразу можно, использовав группировку: ( )
- В качестве логического ИЛИ можно использовать |
```
^(https?:\/\/)?(www\.)?netology\.[a-z]{2,4}$
^(Москва|Ленинград|Казань)+$
```
**Grep** — утилита для поиска строк, соответствующих регулярному выражению
```
grep -E 'regexp' file
-E — расширенное регулярное выражение (Extended Regexp)
-v — инверсия вывода
-с — подсчёт количества выводимых строк вместо их вывода
-n — вывод номера строки
-i — нечувствительность к регистру
-r — рекурсивный поиск по всем файлам
```
**Stream editor** - **Sed** — потоковый текстовый редактор, применяющий различные предопределённые текстовые преобразования к последовательному потоку текстовых данных
```
sed 's/abc/xyz/g' file > file2
s - замена
g - замена всех которые встретятся (иначе только первый в строке, можно указать 2,3...)
```
Заменяет все строки abc на xyz в файле file и перенаправляет получившийся результат в file2
получившийся результат в .

Или в потоке:
```
echo '123asd123' | sed 's/1/3/g
```
Ключи
- -E — позволяет использовать регулярные выражения в sed
- sed '/pattern/d' — удаляет строки, содержащие символы из pattern
- sed '5d' text.txt - удалить 5ую строку
- sed G test.txt - вставить пустые строки
- -i, --in-place — производит замену в изначальном файле file
- Если после ключа указать текст, будет создан бэкап

**Awk** — скриптовый язык, который полезен при работе в командной строке и широко применяется для обработки текста. Обладает практически безграничными возможностями по обработке данных благодаря встроенному языку программирования. Данные, поступающие в awk, можно представить в виде таблицы, где строки таблицы — это строки в традиционном понимании, а разделителем столбцов выступает пробел

Столбцы заносятся в переменные:
- $0 — вся строка целиком
- $1 — первый столбец
- $2 — второй столбец
- Чтобы использовать другой разделитель столбцов, можно применить ключ -f

Встроенные переменные
- FS — разделитель столбцов
- RS — разделитель строк
- OFS — выходной разделитель столбцов
- ORS — выходной разделитель строк

Синтаксис
- Вывод чего-либо на экран: print ''
- Объявление переменных: var='value'
- Условный оператор: if-then-else
- Цикл while: while (i > 0){}

Когда awk-скрипт перестаёт помещаться в одну строку, можно поместить его в файл и указать его имя ключом : -f
```
awk -f /tmp/awkscript
```
```
ps aux | awk '{print $2, $8}'
cat /etc/passwd | awk -F: 'BEGIN{OFS=" "} {print $1,$6,$5}'

ps aux | awk '{if ($2 > 2000) print $0}'

cat /proc/loadavg | awk '{i=1; sum=0; while (i<4){ sum += $i; i++ }print $sum/$i }'

tail access.log
cat access.log | awk '{print $9}' | sort | uniq -c
```