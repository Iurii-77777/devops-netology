# 1.
### _cd_ — это встроенная команда bash. 
### Родитель этой команды системный процесс, соответственно команда получает права на всю систему, если бы команда вызывалась из под пользователя, то тогда бы у команда работала бы в пределах файлового окружения пользователя.
### Т.к. линукс очень гибкая система и если покопаться в настройках, то можно команду cd вызывать и из под пользователя. Но это приведёт к дальнейшим трудностям в работе. Таким как несоответствие прав на доступ к каталогам или множественные shell команды cd.
# 2.
### Основная команда: grep <some_string> <some_file> | wc -l
### Альтернаятивная команда: grep <some_string> <some_file> -c
# 3.
### iurii@iurii-VirtualBox:~$ pstree -p
### systemd(1)─┬─ModemManager(639)─┬─{ModemManager}(652)
### 
### Процесс "systemd" с PID 1
# 4.
### Запущено два окна терминала /dev/pts/1 и /dev/pts/2. Прилагаю команды в терминалах и их вывод.
###
### _Терминал 1:_
### iurii@iurii-VirtualBox:~$ tty
### /dev/pts/1
### iurii@iurii-VirtualBox:~$ ls -l \root 2>/dev/pts/2
###
### _Терминал 2:_
### iurii@iurii-VirtualBox:~$ tty
### /dev/pts/2
### iurii@iurii-VirtualBox:~$ ls: невозможно получить доступ к 'root': Нет такого файла или каталога
# 5.
### iurii@iurii-VirtualBox:~$ cat <some_file >some_file_out
# 6.
### iurii@iurii-VirtualBox:~$ sudo -i
### [sudo] пароль для iurii: 
### root@iurii-VirtualBox:~# echo test_test >/dev/tty3
### Доступ к терминалу tty3 возможен под root. Перенаправить данные можно, но чтобы их увидеть необходимо переключиться в режим терминала tty3 (ctrl + alt + f3). К сожаленю при переключении обратно в графический режим сбивается видео-разрешение виртуальной машины ubuntu, пришлось перезагружаться.
# 7.
### root@iurii-VirtualBox:~# bash 5>&1   # создаст дескриптор 5 и перенаправить его в stdout
### root@iurii-VirtualBox:~# echo netology > /proc/$$/fd/5 # направит вывод в дескриптор 5, а оттуда вперенаправит в stdout 
### netology
### root@iurii-VirtualBox:~# exit
### выход
### iurii@iurii-VirtualBox:~$ echo netology > /proc/$$/fd/5 # под этим пользователем нужно опять создавать дескриптор 5.
### bash: /proc/2206/fd/5: Нет такого файла или каталога
### iurii@iurii-VirtualBox:~$ bash 5>&1
### iurii@iurii-VirtualBox:~$ echo netology > /proc/$$/fd/5
### netology
# 8.
### iurii@iurii-VirtualBox:~$ ls -l /root | grep открыть -c
### ls: невозможно открыть каталог '/root': Отказано в доступе
### 0
### iurii@iurii-VirtualBox:~$ ls -l /root 6>&2 2>&1 1>&6 | grep открыть -c
### 1
### Чередой перенаправлений потока, делаем stderr в качестве входного потока.
# 9.
### cat /proc/$$/environ - откроет файл с данными по переменным окружения. Вывод не структурированный, плохо читать.
### Более информативны с удобно читаемым выводом команды - env, printenv
# 10.
### /proc/[pid]/cmdline (строка 298)
###              This read-only file holds the complete command  line  for  the
###              process,  unless the process is a zombie.  In the latter case,
###              there is nothing in this file: that is, a read  on  this  file
###              will  return  0 characters.  The command-line arguments appear
###              in this file as a set  of  strings  separated  by  null  bytes
###              ('\0'), with a further null byte after the last string.
### /proc/[pid]/exe (строка 365)
###              Under  Linux  2.2 and later, this file is a symbolic link con‐
###              taining the actual pathname of  the  executed  command.   This
###              symbolic link can be dereferenced normally; attempting to open
###              it  will   open   the   executable.    You   can   even   type
###              /proc/[pid]/exe  to  run  another  copy of the same executable
###              that is being run by process [pid].  If the pathname has  been
###              unlinked,   the   symbolic   link   will  contain  the  string
###              '(deleted)' appended to the original pathname.   In  a  multi‐
###              threaded  process,  the contents of this symbolic link are not
###              available if the main thread has already terminated (typically
###              by calling pthread_exit(3)).
###
###              Permission  to dereference or read (readlink(2)) this symbolic
###              link is governed by a ptrace access mode  PTRACE_MODE_READ_FS‐
###              CREDS check; see ptrace(2).
###
###              Under  Linux  2.0 and earlier, /proc/[pid]/exe is a pointer to
###              the binary which was executed, and appears as a symbolic link.
###              A  readlink(2)  call  on  this  file under Linux 2.0 returns a
###              string in the format:
###
###                  [device]:inode
###
###              For example, [0301]:1502 would be inode 1502 on  device  major
###              03  (IDE,  MFM,  etc. drives) minor 01 (first partition on the
###              first drive).
###
###              find(1) with the -inum option can be used to locate the file.
# 11.
### cat /proc/cpuinfo | grep sse
### Самая старшия версия SSE 4.2
# 12.
### По умолчанию при выполнении команды на удаленной машине с использованием ssh для удаленного сеанса не выделяется TTY. Это позволяет передавать двоичные данные и т. Д. без необходимости иметь дело с причудами TTY. 
### Команда ssh -t localhost 'tty' принудительно выделит TTY.
# 13.
### Выполнен перенос процесса. Пришлось разбираться со значениями /etc/sysctl.d/10-ptrace.conf и /proc/sys/kernel/yama/ptrace_scope. Также ключ -L поможет избежать ошибку [-] Timed out waiting for child stop. 
### iurii@iurii-VirtualBox:~$ reptyr -L 2888
### Opened a new pty: /dev/pts/4
### Получается терминал /dev/pts/3 можно закрыть.
# 14.
### Команда tee в Linux считывает стандартный ввод и записывает его одновременно в стандартный вывод и в один или несколько подготовленных файлов. 
### echo string | sudo tee /root/new_file в данном случае tee считает string а затем его перенаправит в файл, т.к. tee будет запущена под root то запись пройдёт успешно. 
### А в случае sudo echo string > /root/new_file под root будет запущена только echo, а перенаправление > будет запущено под текущим пользователем, соответственно не хватит прав на редактирование файла.
