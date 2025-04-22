# Домашнее задание к занятию «Введение в Terraform»

### Цели задания

1. Установить и настроить Terrafrom.
2. Научиться использовать готовый код.

------

### Чек-лист готовности к домашнему заданию

1. Скачайте и установите **Terraform** версии >=1.8.4 . Приложите скриншот вывода команды ```terraform --version```.
2. Скачайте на свой ПК этот git-репозиторий. Исходный код для выполнения задания расположен в директории **01/src**.
3. Убедитесь, что в вашей ОС установлен docker.

### РЕШЕНИЕ:

1. Скачал архив Terraform версии 1.11.4 и распаковал его по адресу /user/bin/terraform
```
kos@kos-VirtualBox:~$ sudo unzip ../kos/Загрузки/terraform_1.11.4_linux_amd64.zip -d /usr/bin/
Archive:  ../kos/Загрузки/terraform_1.11.4_linux_amd64.zip
  inflating: /usr/bin/LICENSE.txt    
  inflating: /usr/bin/terraform   
```
Выдал права файлу .terraform
```
 kos@kos-VirtualBox:~$ sudo chmod +x /usr/bin/terraform
```
Версия Terraform

![1](https://github.com/KargapoltcevKS/Terraform_01/blob/main/img/1.png)

Версия Docker

![2](https://github.com/KargapoltcevKS/Terraform_01/blob/main/img/2.png)

------

### Инструменты и дополнительные материалы, которые пригодятся для выполнения задания

1. Репозиторий с ссылкой на зеркало для установки и настройки Terraform: [ссылка](https://github.com/netology-code/devops-materials).
2. Установка docker: [ссылка](https://docs.docker.com/engine/install/ubuntu/). 
------
### Внимание!! Обязательно предоставляем на проверку получившийся код в виде ссылки на ваш github-репозиторий!
------

### Задание 1

1. Перейдите в каталог [**src**](https://github.com/netology-code/ter-homeworks/tree/main/01/src). Скачайте все необходимые зависимости, использованные в проекте. 
2. Изучите файл **.gitignore**. В каком terraform-файле, согласно этому .gitignore, допустимо сохранить личную, секретную информацию?(логины,пароли,ключи,токены итд)
3. Выполните код проекта. Найдите  в state-файле секретное содержимое созданного ресурса **random_password**, пришлите в качестве ответа конкретный ключ и его значение.
4. Раскомментируйте блок кода, примерно расположенный на строчках 29–42 файла **main.tf**.
Выполните команду ```terraform validate```. Объясните, в чём заключаются намеренно допущенные ошибки. Исправьте их.
5. Выполните код. В качестве ответа приложите: исправленный фрагмент кода и вывод команды ```docker ps```.
6. Замените имя docker-контейнера в блоке кода на ```hello_world```. Не перепутайте имя контейнера и имя образа. Мы всё ещё продолжаем использовать name = "nginx:latest". Выполните команду ```terraform apply -auto-approve```.
Объясните своими словами, в чём может быть опасность применения ключа  ```-auto-approve```. Догадайтесь или нагуглите зачем может пригодиться данный ключ? В качестве ответа дополнительно приложите вывод команды ```docker ps```.
8. Уничтожьте созданные ресурсы с помощью **terraform**. Убедитесь, что все ресурсы удалены. Приложите содержимое файла **terraform.tfstate**. 
9. Объясните, почему при этом не был удалён docker-образ **nginx:latest**. Ответ **ОБЯЗАТЕЛЬНО НАЙДИТЕ В ПРЕДОСТАВЛЕННОМ КОДЕ**, а затем **ОБЯЗАТЕЛЬНО ПОДКРЕПИТЕ** строчкой из документации [**terraform провайдера docker**](https://docs.comcloud.xyz/providers/kreuzwerker/docker/latest/docs).  (ищите в классификаторе resource docker_image )


------

### РЕШЕНИЕ

1. Скачал все зависимости согласно задания
![3](https://github.com/KargapoltcevKS/Terraform_01/blob/main/img/3.png)

2. Хранить секретную информацию можно в файле 
personal.auto.tfvars

3. Пароль 16 значный
"result": "Y1Yh7Mkr47Nqiud5"


```
{
  "version": 4,
  "terraform_version": "1.11.4",
  "serial": 1,
  "lineage": "1b4af8c0-1490-a70e-55fb-bb766acb95b2",
  "outputs": {},
  "resources": [
    {
      "mode": "managed",
      "type": "random_password",
      "name": "random_string",
      "provider": "provider[\"registry.terraform.io/hashicorp/random\"]",
      "instances": [
        {
          "schema_version": 3,
          "attributes": {
            "bcrypt_hash": "$2a$10$CAkT1LvLYCtb/hlliaSmNuecD8IpK1U.tScfEr/1dRu9MwHqfQiZS",
            "id": "none",
            "keepers": null,
            "length": 16,
            "lower": true,
            "min_lower": 1,
            "min_numeric": 1,
            "min_special": 0,
            "min_upper": 1,
            "number": true,
            "numeric": true,
            "override_special": null,
            "result": "Y1Yh7Mkr47Nqiud5",
            "special": false,
            "upper": true
          },
          "sensitive_attributes": [
            [
              {
                "type": "get_attr",
                "value": "bcrypt_hash"
              }
            ],
            [
              {
                "type": "get_attr",
                "value": "result"
              }
            ]
          ]
        }
      ]
    }
  ],
  "check_results": null
}
```

4. В коде имеются ошибки:
- отсутствует именя ресурса ( Error: Missing name for resource)
- имя не может начинаться с цифры (Error: Invalid resource name)
- Поправил строчку с неверными ссылками 
name  = "example_${random_password.random_string.result}"

![4](https://github.com/KargapoltcevKS/Terraform_01/blob/main/img/4.png)

Исправил ошибки

```
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0.1"
    }
  }
  required_version = ">=1.8.4" /*Многострочный комментарий.
 Требуемая версия terraform */
}
provider "docker" {}

#однострочный комментарий

resource "random_password" "random_string" {
  length      = 16
  special     = false
  min_upper   = 1
  min_lower   = 1
  min_numeric = 1
}

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = true
}

resource "docker_container" "nginx_1" {
  image = docker_image.nginx.image_id
  name  = "example_${random_password.random_string.result}"

  ports {
    internal = 80
    external = 9090
  }
}
```
5. Выполнил код

```
kos@kos-VirtualBox:~/Terraform_01$ sudo terraform apply
```
Вывод команды sudo docker ps

![5](https://github.com/KargapoltcevKS/Terraform_01/blob/main/img/5.png)

6. Ключ -auto-approve пропускает подтверждение выполнения команды на выполнения (YES)
Вывод команды sudo docker ps
![6](https://github.com/KargapoltcevKS/Terraform_01/blob/main/img/6.png)

8. Уничтожил все созданные ресурсы 
Команда terraform destroy
![7](https://github.com/KargapoltcevKS/Terraform_01/blob/main/img/7.png)

Содержимое файла terraform.tfstate

```
{
  "version": 4,
  "terraform_version": "1.11.4",
  "serial": 14,
  "lineage": "1b4af8c0-1490-a70e-55fb-bb766acb95b2",
  "outputs": {},
  "resources": [],
  "check_results": null
}
```

9. keep_locally - (Необязательно, логическое значение) Если true, то образ Docker не будет удален при операции уничтожения. Если это ложь, он удалит изображение из локального хранилища докера при операции уничтожения. У нас в конфигурации как раз данный параметр "true", поэтому данный образ остался.
