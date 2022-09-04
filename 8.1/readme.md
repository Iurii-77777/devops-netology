## **Задача 1.**
#### Попробуйте запустить playbook на окружении из test.yml, зафиксируйте какое значение имеет факт some_fact для указанного хоста при выполнении playbook'a. 
```
iurii-devops@Host-SPB:~/ansible/playbook$ ansible-playbook -i inventory/test.yml site.yml 

PLAY [Print os facts] **********************************************************

TASK [Gathering Facts] *********************************************************
ok: [localhost]

TASK [Print OS] ****************************************************************
ok: [localhost] => {
    "msg": "Ubuntu"
}

TASK [Print fact] **************************************************************
ok: [localhost] => {
    "msg": 12
}

PLAY RECAP *********************************************************************
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

iurii-devops@Host-SPB:~/ansible/playbook$ 
```
## **Задача 2.** 
#### Найдите файл с переменными (group_vars) в котором задаётся найденное в первом пункте значение и поменяйте его на 'all default fact'.
```
iurii-devops@Host-SPB:~/ansible/playbook$ cat group_vars/all/examp.yml
---
  some_fact: "all default fact"
```
## **Задача 3.**
#### Воспользуйтесь подготовленным (используется docker) или создайте собственное окружение для проведения дальнейших испытаний.
```
Создано.
```
## **Задача 4.** 
#### Проведите запуск playbook на окружении из prod.yml. Зафиксируйте полученные значения some_fact для каждого из managed host.
```
iurii-devops@Host-SPB:~/ansible/playbook$ sudo ansible-playbook -i inventory/prod.yml site.yml

PLAY [Print os facts] *****************************************************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************************************************
ok: [debian2]
ok: [centos2]

TASK [Print OS] ***********************************************************************************************************************************************************
ok: [centos2] => {
    "msg": "Debian"
}
ok: [debian2] => {
    "msg": "Debian"
}

TASK [Print fact] *********************************************************************************************************************************************************
ok: [centos2] => {
    "msg": "el"
}
ok: [debian2] => {
    "msg": "deb"
}

PLAY RECAP ****************************************************************************************************************************************************************
centos2                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
debian2                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

```
## **Задача 5.** 
#### Добавьте факты в group_vars каждой из групп хостов так, чтобы для some_fact получились следующие значения: для deb - 'deb default fact', для el - 'el default fact'.
```
iurii-devops@Host-SPB:~/ansible/playbook$ vim group_vars/deb/examp.yml
iurii-devops@Host-SPB:~/ansible/playbook$ vim group_vars/el/examp.yml
iurii-devops@Host-SPB:~/ansible/playbook$ cat group_vars/deb/examp.yml ;echo ""
---
  some_fact: "deb default fact"

iurii-devops@Host-SPB:~/ansible/playbook$ cat group_vars/el/examp.yml ;echo ""
---
  some_fact: "el default fact"

iurii-devops@Host-SPB:~/ansible/playbook$ 
```
## **Задача 6.** 
#### Повторите запуск playbook на окружении prod.yml. Убедитесь, что выдаются корректные значения для всех хостов.
```

```
## **Задача 7.** 
#### При помощи ansible-vault зашифруйте факты в group_vars/deb и group_vars/el с паролем netology.
```

```
## **Задача 8.** 
#### Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль. Убедитесь в работоспособности.
```

```
## **Задача 9.** 
#### Посмотрите при помощи ansible-doc список плагинов для подключения. Выберите подходящий для работы на control node.
```

```
## **Задача 10.** 
#### В prod.yml добавьте новую группу хостов с именем local, в ней разместите localhost с необходимым типом подключения
```

```
## **Задача 11.** 
#### Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль. Убедитесь что факты some_fact для каждого из хостов определены из верных group_vars.
```

```
## **Задача 12.** 
#### Заполните README.md ответами на вопросы. Сделайте git push в ветку master. В ответе отправьте ссылку на ваш открытый репозиторий с изменённым playbook и заполненным README.md.
```

```
