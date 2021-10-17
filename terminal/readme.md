5.
# RAM: 1024mb
# CPU: 1
# HDD: 64gb
# video: 8mb

6.
# Выполнено увеличение оперативной памяти и кол-ва процессоров (настройки файла Vagrantfile прилагаю)

# Vagrant.configure("2") do |config|
#	config.vm.box = "bento/ubuntu-20.04"
#	config.vm.provider "virtualbox" do |vb|
#		vb.memory = "2048"
#		vb.cpus = "2"
#	end
# end

7.
# выполнено подключение по ssh
# cd vagrant_demo
# vagrant up
# vagrant ssh

8.
# HISTFILESIZE - максимальное число строк в файле истории для сохранения
# строка 846
# HISTSIZE - число команд для сохранения
# строка 862
# директива ignoreboth это сокращение для 2х директив ignorespace and ignoredups
# ignorespace - не сохранять команды начинающиеся с пробела 
# ignoredups - нен сохранять команду, если такая уже имеется в истории

9.
# { list; }
#              list is simply executed in the current shell environment.   list
#              must  be  terminated with a newline or semicolon.  This is known
#              as a group command.  The return status is  the  exit  status  of
#              list.   Note that unlike the metacharacters ( and ), { and } are
#              reserved words and must occur where a reserved word is permitted
#              to  be  recognized.   Since they do not cause a word break, they
#              must be separated from  list  by  whitespace  or  another  shell
#              metacharacter.
# строка 343

10.
# touch {1..100000}.txt - создаст в текужей директории 100000 файлов
# touch {1..300000}.txt - не сработает, слишком длинный список аргументов

11.
# -d /tmp проверяет наличие каталока  /tmp и возвращает 0 если существет или 1 если нет.

12.
# vagrant@vagrant:~$ mkdir /tmp/new_path_dir/
# vagrant@vagrant:~$ cp /bin/bash /tmp/new_path_dir/
# vagrant@vagrant:~$ PATH=/tmp/new_path_dir/:$PATH
# vagrant@vagrant:~$ type -a bash
# bash is /tmp/new_path_dir/bash
# bash is /usr/bin/bash
# bash is /bin/bash 

13.
# at - команда запускается в указанное время
# batch - команда запускается когда уровень загрузки ЦП падает ниже порогового значения (по умолчанию 1.5)

14.
# exit
# vagrant suspend
# vagrant halt 

