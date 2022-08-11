## **Задача 1.**
#### Какой тип инфраструктуры будем использовать для этого проекта: изменяемый или не изменяемый?
```
Будет комбинированный вариант, т.к. чёткого технического задания нет, но уже планируются доработки.
```
#### Будет ли центральный сервер для управления инфраструктурой?
```
Будет, т.к. в условиях неопределённости нужен повышенный контроль за инфраструктурой и конфигурациями.
В процессе отладки работы проекта можно будет перейти на работу без центрального сервера.
```
#### Будут ли агенты на серверах?
```
Будут т.к. будет и центральный сервер. 
```
#### Будут ли использованы средства для управления конфигурацией или инициализации ресурсов?
```
Будут оба средства использованы, автоматизация процесса должна быть реализована, так выглядит современное администрирование.
```
#### Какие инструменты из уже используемых вы хотели бы использовать для нового проекта?
```
Terraform, Ansible, Packer, Docker (плавный переход на Kubernetes)
```
#### Хотите ли рассмотреть возможность внедрения новых инструментов для этого проекта?
```
Рассотреть возможность внедрения Gitlab или Jenkins, в зависимости от требований проекта.
```
## **Задача 2.**
#### Установка терраформ.
#### Установите терраформ при помощи менеджера пакетов используемого в вашей операционной системе. 
#### В виде результата этой задачи приложите вывод команды terraform --version
```
iurii-devops@Host-SPB:~$ terraform --version
Terraform v1.1.5
on linux_amd64
```
## **Задача 3.**
#### В виде результата этой задачи приложите вывод --version двух версий терраформа доступных на вашем компьютере или виртуальной машине.
```
iurii-devops@Host-SPB:~$ mkdir terraform2
iurii-devops@Host-SPB:~$ cd terraform2/
iurii-devops@Host-SPB:~/terraform2$ wget https://releases.hashicorp.com/terraform/1.2.7/terraform_1.2.7_linux_amd64.zip
iurii-devops@Host-SPB:~/terraform2$ unzip terraform_1.2.7_linux_amd64.zip 
Archive:  terraform_1.2.7_linux_amd64.zip
  inflating: terraform               
iurii-devops@Host-SPB:~/terraform2$ rm terraform_1.2.7_linux_amd64.zip 
iurii-devops@Host-SPB:~/terraform2$ sudo ln -s /home/iurii-devops/terraform2/terraform /usr/bin/terraform2
iurii-devops@Host-SPB:~/terraform2$ sudo chmod ugo+x /usr/bin/terraform2

iurii-devops@Host-SPB:~/terraform2$ terraform2 --version
Terraform v1.2.7
on linux_amd64
iurii-devops@Host-SPB:~/terraform2$ terraform --version
Terraform v1.1.5
on linux_amd64

Your version of Terraform is out of date! The latest version
is 1.2.7. You can update by downloading from https://www.terraform.io/downloads.html
iurii-devops@Host-SPB:~/terraform2$ 
```
