[[A05 Injection]]

==🔵Внедрение шаблонов на стороне сервера (SSTI)== — это веб-эксплойт, использующий уязвимость в небезопасной реализации механизма шаблонов.
SSTI — это уязвимость на стороне сервера, а не на стороне клиента

==🔵Шаблонизатор== позволяет создавать статические файлы шаблонов, которые можно повторно использовать в вашем приложении.

==🔵Фаззинг== — это метод определения уязвимости сервера путем отправки множества символов в надежде на вмешательство в работу внутренней системы.

К счастью, большинство шаблонизаторов используют схожий набор символов для своих «специальных функций», что позволяет относительно быстро обнаружить уязвимость к SSTI.

Когда мы определили, какие символы вызвали ошибку в приложении, пришло время выяснить, какой шаблонизатор используется.
![[Pasted image 20261120225339.png]]

После определения шаблонизатора нам необходимо изучить его синтаксис.
Например:
[Jinja man](https://jinja.palletsprojects.com/en/stable/)

Для быстрого ознакомления был создан замечательный репозиторий на GitHub, представляющий собой шпаргалку по полезным нагрузкам для всех веб-уязвимостей, включая SSTI.

Репозиторий находится [здесь](https://github.com/swisskyrepo/PayloadsAllTheThings) , а документация по SSTI —[ здесь](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection) .

==🔴Исправления==
Большинство шаблонизаторов имеют функцию, позволяющую передавать входные данные в виде отдельных элементов, а не объединять их в шаблон.
```
template = "<h1>Welcome to the profile of {{ user }}!</h1>"
return render_template_string(template, user=user)
```

Вводимой информации пользователям нельзя доверять!

В каждом месте вашего приложения, где пользователь может добавлять собственный контент, убедитесь, что входные данные проходят проверку на наличие вредоносного кода!

Это можно сделать, сначала определив, какой набор символов вы хотите разрешить, и добавив его в белый список.

В Python это можно сделать следующим образом:

```
import re

# Remove everything that isn't alphanumeric
user = re.sub("^[A-Za-z0-9]", "", user)
template = "<h1>Welcome to the profile of {{ user }}!</h1>"
return render_template_string(template, user=user)
```

==🔴SSTImap== — это инструмент, автоматизирующий процесс тестирования и эксплуатации уязвимостей. Размещенный на [GitHub](https://github.com/vladko312/SSTImap) , он предоставляет фреймворк для обнаружения уязвимостей внедрения шаблонов.

SSTImap обладает следующими возможностями:

- ***Обнаружение шаблонизатора*** : SSTImap может помочь определить шаблонизатор, используемый веб-приложением, что имеет решающее значение для создания конкретных эксплойтов.
- ***Автоматизированная эксплуатация*** : Для известных уязвимостей SSTImap может автоматизировать процесс их эксплуатации.

Вы можете использовать SSTImap, указав целевой URL-адрес и необходимые параметры. Вот простой пример использования:

`python3 sstimap.py -X POST -u 'http://ssti.thm:8002/mako/' -d 'page='`

Эта команда пытается обнаружить уязвимость SSTI с помощью специально разработанных полезных нагрузок.
```
           user@tryhackme $ python3 sstimap.py -X POST -u 'http://ssti.thm:8002/mako/' -d 'page='           

    ╔══════╦══════╦═══════╗ ▀█▀
    ║ ╔════╣ ╔════╩══╗ ╔══╝═╗▀╔═
    ║ ╚════╣ ╚════╗  ║ ║    ║{║  _ __ ___   __ _ _ __
    ╚════╗ ╠════╗ ║  ║ ║    ║*║ | '_ ` _ \ / _` | '_ \
    ╔════╝ ╠════╝ ║  ║ ║    ║}║ | | | | | | (_| | |_) |
    ╚══════╩══════╝  ╚═╝    ╚╦╝ |_| |_| |_|\__,_| .__/
                             │                  | |
                                                |_|
[*] Version: 1.2.1
[*] Author: @vladko312
[*] Based on Tplmap
[!] LEGAL DISCLAIMER: Usage of SSTImap for attacking targets without prior mutual consent is illegal.
It is the end user's responsibility to obey all applicable local, state and federal laws.
Developers assume no liability and are not responsible for any misuse or damage caused by this program
[*] Loaded plugins by categories: languages: 5; generic: 3; engines: 17; legacy_engines: 2
[*] Loaded request body types: 4

[*] Scanning url: http://ssti.thm:8002/mako/
[*] Testing if Body parameter 'page' is injectable
[*] Mako plugin is testing rendering with tag '*'
[+] Mako plugin has confirmed injection with tag '*'
[+] SSTImap identified the following injection point:

  Body parameter: page
  Engine: Mako
  Injection: *
  Context: text
  OS: posix-linux
  Technique: render
  Capabilities:

    Shell command execution: ok
    Bind and reverse shell: ok
    File write: ok
    File read: ok
    Code evaluation: ok, python code
```

==🔴Песочница== — это функция безопасности, которая ограничивает выполнение потенциально вредоносного кода внутри шаблонов. Она ограничивает действия, которые могут выполнять шаблоны, такие как операции с файлами или выполнение системных команд. Правильно настроенная песочница помогает предотвратить проблемы безопасности, такие как SSTI (Spanbox Instructional System — уязвимость, связанная с вредоносным кодом).

Важность песочницы

- ***Ограничения функциональности :*** Ограничивает функции или методы, которые могут быть вызваны из шаблона, блокируя потенциально опасные операции.
- ***Доступ к переменным и данным*** : контролирует доступ к глобальным переменным или конфиденциальным данным, гарантируя, что шаблоны не смогут манипулировать или раскрывать важную информацию.

