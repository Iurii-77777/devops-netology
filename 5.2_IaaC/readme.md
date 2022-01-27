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
iurii-devops@Host-SPB:~/ansible$ vboxmanage --version
6.1.26_Ubuntur145957

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
iurii-devops@Host-SPB:~/ansible$ vagrant --version
Vagrant 2.2.19

Ansible:
iurii-devops@Host-SPB:~/vagrant_demo$ sudo apt install ansible
iurii-devops@Host-SPB:~$ mkdir ansible/
iurii-devops@Host-SPB:~/cd ansible/
iurii-devops@Host-SPB:~/ansible$ vim provision.yml
iurii-devops@Host-SPB:~/ansible$ vim inventory
iurii-devops@Host-SPB:~/ansible$ sudo vim etc/ansible/ansible.cfg
iurii-devops@Host-SPB:~/ansible$ ansible --version
ansible 2.9.6
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/iurii-devops/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.8.10 (default, Nov 26 2021, 20:14:08) [GCC 9.3.0]
```

## **4. Установка VM с Docker:**
```
iurii-devops@Host-SPB:~$ cd vagrant_demo/
iurii-devops@Host-SPB:~/vagrant_demo$ vim Vagrantfile 
iurii-devops@Host-SPB:~/vagrant_demo$ vagrant up

Bringing machine 'server1.netology' up with 'virtualbox' provider...
==> server1.netology: Importing base box 'bento/ubuntu-20.04'...
==> server1.netology: Matching MAC address for NAT networking...
==> server1.netology: Checking if box 'bento/ubuntu-20.04' version '202112.19.0' is up to date...
==> server1.netology: Setting the name of the VM: server1.netology
==> server1.netology: Clearing any previously set network interfaces...
==> server1.netology: Preparing network interfaces based on configuration...
    server1.netology: Adapter 1: nat
    server1.netology: Adapter 2: hostonly
==> server1.netology: Forwarding ports...
    server1.netology: 22 (guest) => 20011 (host) (adapter 1)
    server1.netology: 22 (guest) => 2222 (host) (adapter 1)
==> server1.netology: Running 'pre-boot' VM customizations...
==> server1.netology: Booting VM...
==> server1.netology: Waiting for machine to boot. This may take a few minutes...
    server1.netology: SSH address: 127.0.0.1:2222
    server1.netology: SSH username: vagrant
    server1.netology: SSH auth method: private key
    server1.netology: 
    server1.netology: Vagrant insecure key detected. Vagrant will automatically replace
    server1.netology: this with a newly generated keypair for better security.
    server1.netology: 
    server1.netology: Inserting generated public key within guest...
    server1.netology: Removing insecure key from the guest if it's present...
    server1.netology: Key inserted! Disconnecting and reconnecting using new SSH key...
==> server1.netology: Machine booted and ready!
==> server1.netology: Checking for guest additions in VM...
==> server1.netology: Setting hostname...
==> server1.netology: Configuring and enabling network interfaces...
==> server1.netology: Mounting shared folders...
    server1.netology: /vagrant => /home/iurii-devops/vagrant_demo
==> server1.netology: Running provisioner: ansible...
    server1.netology: Running ansible-playbook...

PLAY [nodes] *******************************************************************

TASK [Gathering Facts] *********************************************************
ok: [server1.netology]

TASK [Create directory for ssh-keys] *******************************************
ok: [server1.netology]

TASK [Adding rsa-key in /root/.ssh/authorized_keys] ****************************
An exception occurred during task execution. To see the full traceback, use -vvv. The error was: If you are using a module and expect the file to exist on the remote, see the remote_src option
fatal: [server1.netology]: FAILED! => {"changed": false, "msg": "Could not find or access '~/.ssh/id_rsa.pub' on the Ansible Controller.\nIf you are using a module and expect the file to exist on the remote, see the remote_src option"}
...ignoring

TASK [Checking DNS] ************************************************************
changed: [server1.netology]

TASK [Installing tools] ********************************************************
ok: [server1.netology] => (item=['git', 'curl'])

TASK [Installing docker] *******************************************************
changed: [server1.netology]

TASK [Add the current user to docker group] ************************************
changed: [server1.netology]

PLAY RECAP *********************************************************************
server1.netology           : ok=7    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=1   

iurii-devops@Host-SPB:~/vagrant_demo$ vagrant ssh
Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.4.0-91-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

 System information disabled due to load higher than 1.0


This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento
Last login: Thu Jan 27 23:22:06 2022 from 10.0.2.2

vagrant@server1:~$ ps aux | grep docker
root       15237  0.0  7.6 1235672 76384 ?       Ssl  23:21   0:00 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
vagrant    16663  0.0  0.0   6432   736 pts/0    R+   23:23   0:00 grep --color=auto docker
vagrant@server1:~$ exit

```
