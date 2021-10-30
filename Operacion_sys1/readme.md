# 1.
### iurii@iurii-VirtualBox:~$ strace /bin/bash -c 'cd /tmp'
### Появляется множество последовательных комманд. Присутствуют команды проверки существования пути/каталога, проверки прав доступа, команды перенаправления потоков и др.Предполагаю что в полученном списке важная для нас команда - chdir("/tmp") 
# 2.
### База данных file - /usr/share/misc/magic.mgc . Но есть ещё дополнительные файлы "/etc/magic.mgc" и "/home/iurii/.magic.mgc"
### iurii@iurii-VirtualBox:~$ strace file /dev/tty 2>&1 | grep magic.mgc
### stat("/home/iurii/.magic.mgc", 0x7ffd9fa0c140) = -1 ENOENT (Нет такого файла или каталога)
### openat(AT_FDCWD, "/etc/magic.mgc", O_RDONLY) = -1 ENOENT (Нет такого файла или каталога)
### openat(AT_FDCWD, "/usr/share/misc/magic.mgc", O_RDONLY) = 3
# 3.
### iurii@iurii-VirtualBox:~/Документы$ lsof -p 17341 | grep do_not_delete_me
### python3 17341 iurii    3r   REG    8,5        0 1715741 /home/iurii/Документы/do_not_delete_me (deleted)
### Очищаем место на диске с помощью команды iurii@iurii-VirtualBox:~/Документы$ > /proc/17341/fd/3 или iurii@iurii-VirtualBox:~/Документы$ truncate -s 0 /proc/17341/fd/3
# 4.
### Процессы зомби, вообще не являются реальными процессами. Это просто записи в таблице процессов ядра. Это единственный ресурс, который они потребляют. Они не потребляют ни CPU, ни RAM. Единственная опасность наличия зомби - это нехватка места в таблице процессов
# 5.
### ID    COMM               FD ERR PATH
### 3206   glean.dispatche    49   0 /home/iurii/.mozilla/firefox/tthetixf.default-release/datareporting/glean/db/data.safe.bin
### 3206   glean.dispatche    49   0 /home/iurii/.mozilla/firefox/tthetixf.default-release/datareporting/glean/db/data.safe.bin
### 3206   glean.dispatche    49   0 /home/iurii/.mozilla/firefox/tthetixf.default-release/datareporting/glean/db/data.safe.bin
### 3206   StreamT~ns #136    49   0 /home/iurii/.mozilla/firefox/tthetixf.default-release/datareporting/aborted-session-ping.tmp
### 1544   xdg-desktop-por    13   0 /usr/share/applications/firefox.desktop
### 1544   xdg-desktop-por    13   0 /usr/share/applications/org.gnome.Terminal.desktop
### 1049   gnome-shell        18   0 /home/iurii/.cache/mesa_shader_cache/3b/25149db926d4a5949bb781296a8b6bbd8b99a2
### 1049   gnome-shell        18   0 /home/iurii/.cache/mesa_shader_cache/10/ad91bdb149b9fb81ab770a71197bea66102df5
### 3446   gnome-terminal-    20   0 /usr/share/icons/Yaru/scalable/actions/tab-new-symbolic.svg
### 3446   gnome-terminal-    20   0 /usr/share/icons/Yaru/scalable/actions/window-minimize-symbolic.svg
### 1      systemd            30   0 /proc/242/cgroup
### 1      systemd            30   0 /proc/500/cgroup
# 6.
### системный вызов - uname()
### uname() returns system information in the structure pointed to by buf.
### Part of the utsname information is also accessible via /proc/sys/kernel/{ostype, hostname, osrelease, version, domainname}.  
# 7.
### Оператор && является управляющим оператором. Если в командной строке стоит command1 && command2, то command2 выполняется в том, и только в том случае, если статус выхода из команды command1 равен нулю, что говорит об успешном ее завершении.
### ; - разделитель последовательных команд
### test -d /tmp/some_dir && echo Hi - в данном случае команда "Echo Hi" выполниться только после успешного завершения команды "test -d /tmp/some_dir"
### set -e  Немедленно завершит работу, если конвейер, который может состоять из одной простой команды, списка или составной команды, возвращает ненулевое состояние.   
### Соответственно использовать команды && вместе с set -e не имеет смысла, так как смысл этих команд взамоисключает друг друга.
# 8.
### -e прерывает скрипт при первой ошибке 
### -u влияет на переменые. Если установлена ссылка на любую перенменную, которую ранеее не определили, появляется ошибка и происходит выход из программы/скрипта
### -x включает режим в котором выполнение команд выводится на терминал
### -o pipefail предотвращает маскировку ошибок. Если какая-либо команда в списке  команд неуспешна, то код возврата этой команды мы увидем после выполнения всего списка команд
### На основе описания опций режима можно сделать вывод, что данные команды при использовании в сценариях помогут более детально рассматривать ошибки в сценарии и более успешно их фильтровать, отлавливать и отображать даже скрытую информацию.
# 9.
### S - прерывистый сон (ожидание завершения).
### I - неактивный поток ядра. Бездействующие процессы.
### R - работает или запускается (в очереди выполнения).
### Дополнительные символы (+, n и т.д.) это дополнительные характеристики процесса. 

