[[Web_tools]]

==🔴Gobuster== -  это инструмент для проведения атак с открытым исходным кодом, написанный на Golang. Он перечисляет веб-каталоги, DNS поддомены, виртуальные хосты, Amazon С3. Gobuster используется для взлома хранилищ данных, в том числе Google Cloud Storage, методом перебора с использованием определенных списков слов и обработки входящих запросов.

==🔵Перечисление== — это процесс составления списка всех доступных ресурсов, независимо от того, доступны они или нет.

==🔵Метод грубой силы== — это перебор всех возможных вариантов до тех пор, пока не будет найдено совпадение.

```
root@tryhackme:~# gobuster --help
Usage:
  gobuster [command]

Available Commands:
  completion  Generate the autocompletion script for the specified shell
  dir         Uses directory/file enumeration mode
  dns         Uses DNS subdomain enumeration mode
  fuzz        Uses fuzzing mode. Replaces the keyword FUZZ in the URL, Headers and the request body
  gcs         Uses gcs bucket enumeration mode
  help        Help about any command
  s3          Uses aws bucket enumeration mode
  tftp        Uses TFTP enumeration mode
  version     shows the current version
  vhost       Uses VHOST enumeration mode (you most probably want to use the IP address as the URL parameter)

Flags:
      --debug                 Enable debug output
      --delay duration        Time each thread waits between requests (e.g. 1500ms)
  -h, --help                  help for gobuster
      --no-color              Disable color output
      --no-error              Don't display errors
  -z, --no-progress           Don't display progress
  -o, --output string         Output file to write results to (defaults to stdout)
  -p, --pattern string        File containing replacement patterns
  -q, --quiet                 Don't print the banner and other noise
  -t, --threads int           Number of concurrent threads (default 10)
  -v, --verbose               Verbose output (errors)
  -w, --wordlist string       Path to the wordlist. Set to - to use STDIN.
      --wordlist-offset int   Resume from a given position in the wordlist (defaults to 0)

Use "gobuster [command] --help" for more information about a command.
        
```

В этом ==🔴dir== режиме есть возможность перечислять каталоги веб-сайта и содержащиеся в них файлы. Этот режим полезен при проведении тестирования на проникновение, когда необходимо увидеть структуру каталогов веб-сайта и его содержимое. Часто структуры каталогов веб-сайтов и веб-приложений следуют определённым правилам, что делает их уязвимыми для атак методом перебора с использованием списков слов.

Gobuster обладает мощными возможностями, поскольку позволяет сканировать веб-сайт и возвращать коды состояния. Эти коды состояния немедленно показывают, можете ли вы, как внешний пользователь, запрашивать данный каталог или нет.

В таблице ниже перечислены флаги, охватывающие большинство сценариев:
![[Pasted image 20261109214220.png]]

![[Pasted image 20261109214228.png]]

Для запуска Gobuster в dirсоответствующем режиме используйте следующий формат команд:
`gobuster dir -u "http://www.example.thm" -w /path/to/wordlist`

==🔴`dns`== - режим перебора поддоменов. Этот режим позволяет Gobuster перебирать поддомены. Во время тестирования на проникновение проверка поддоменов основного домена цели имеет важное значение. Тот факт, что что-то исправлено в обычном домене, не означает, что это исправлено и в поддомене. Может существовать возможность использовать уязвимость в одном из этих поддоменов.

Наиболее часто используемых флагов:
![[Pasted image 20261109215942.png]]

Для запуска Gobuster в режиме DNS используйте следующий синтаксис команды:
`gobuster dns -d example.thm -w /path/to/wordlist`

==🔴vhost== - режим перебора паролей.  Этот режим позволяет Gobuster перебирать пароли виртуальных хостов. Виртуальные хосты — это разные веб-сайты на одной машине. Иногда они выглядят как поддомены, но не обманывайтесь! Виртуальные хосты основаны на IP-адресах и работают на одном сервере. Поддомены настраиваются в DNS. Разница между режимами перебора паролей vhost и dns заключается в способе сканирования Gobuster:

- vhost режиме будет осуществлен переход по URL-адресу, созданному путем объединения настроенного имени хоста (флаг -u) с записью из списка слов.
- dns режим сделает DNS поиск по полному доменному имени (FQDN), созданному путем объединения настроенного доменного имени (флаг -d) с записью.

Некоторые из наиболее часто используемых флагов:
![[Pasted image 20261109220504.png]]

Чтобы запустить Gobuster в vhostсоответствующем режиме, введите следующую команду:
`gobuster vhost -u "http://example.thm" -w /path/to/wordlist`

