## **1.1 Опишите своими словами основные преимущества применения на практике IaaC паттернов:**
```
Автоматизация многих процессов (предоставления инфраструктуры, тестирования и масштабирования), предсказуемость конечного результата (исключается фактор несовменстимости ПО, плагинов, обновлений), возможность использования принципа среды непрерывной интеграции/непрерывного развёртывания (CI/CD).
```
## **1.2 Какой из принципов IaaC является основополагающим?:**
```
Идемпоте́нтность (лат. idem — тот же самый + potens — способный) это свойство объекта или операции, при повторном выполнении которой мы получаем результат идентичный предыдущему и всем последующим выполнениям.
```
## **2.1 Чем Ansible выгодно отличается от других систем управление конфигурациями?:**
```
Как инструмент управления конфигурацией и система автоматизации Ansible имеет все функции, присутствующие в других инструментах этой же категории, но при этом данная система ориентируется на простоту использования и производительность.
```
## **2.2 Какой, на ваш взгляд, метод работы систем конфигурации более надёжный push или pull?:**
```
push - т.к. выполняется под контролем администратора, а не агентами DSC (тут возможны несовместимости, например с GPO).
```
## **3. Вывод команд установленных версий каждой из программ:**
```
VB:
iurii-devops@Host-SPB:~$ sudo apt update
iurii-devops@Host-SPB:~$ sudo apt install virtualbox
iurii-devops@Host-SPB:~$ sudo apt install virtualbox-ext-pack -y

Vagrant:
iurii-devops@Host-SPB:~$ curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
iurii-devops@Host-SPB:~$ sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
iurii-devops@Host-SPB:~$ sudo apt-get update && sudo apt-get install vagrant
iurii-devops@Host-SPB:~$ mkdir vagrant_demo && cd vagrant_demo
iurii-devops@Host-SPB:~/vagrant_demo$ vagrant init ubuntu/trusty64
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.
iurii-devops@Host-SPB:~/vagrant_demo$ ls
Vagrantfile

Ansible:

```
## **4. Установка VM с Docker:**
```
```
