## **Задача 1.**
#### Создайте s3 бакет, iam роль и пользователя от которого будет работать терраформ. 
#### Зарегистрируйте бэкэнд в терраформ проекте как описано по ссылке выше.
#### Можно создать отдельного пользователя, а можно использовать созданного в рамках предыдущего задания, просто добавьте ему необходимы права.
![Screenshot](7_3_1.jpg)
![Screenshot](7_3_2.jpg)
## **Задача 2.** 
#### Инициализируем проект и создаем воркспейсы.
```
iurii-devops@Host-SPB:~/terraform_7.2/terraform$ terraform workspace new notest
iurii-devops@Host-SPB:~/terraform_7.2/terraform$ terraform workspace new test
iurii-devops@Host-SPB:~/terraform_7.2/terraform$ terraform workspace select test
```
#### Вывод команды terraform workspace list:
```
iurii-devops@Host-SPB:~/terraform_7.2/terraform$ terraform workspace list
  default
  notest
* test
```
#### Вывод команды terraform plan для воркспейса test:
```
Приложены файлы в корне текущей папки репозитория - terraform.tfstate и terraform.tfstate.backup
```
