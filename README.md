# Домашнее задание к занятию «Организация сети» Шарапат Виктор

### Подготовка к выполнению задания

1. Домашнее задание состоит из обязательной части, которую нужно выполнить на провайдере Yandex Cloud, и дополнительной части в AWS (выполняется по желанию). 
2. Все домашние задания в блоке 15 связаны друг с другом и в конце представляют пример законченной инфраструктуры.  
3. Все задания нужно выполнить с помощью Terraform. Результатом выполненного домашнего задания будет код в репозитории. 
4. Перед началом работы настройте доступ к облачным ресурсам из Terraform, используя материалы прошлых лекций и домашнее задание по теме «Облачные провайдеры и синтаксис Terraform». Заранее выберите регион (в случае AWS) и зону.

---
### Задание 1. Yandex Cloud 

**Что нужно сделать**

1. Создать пустую VPC. Выбрать зону.
2. Публичная подсеть.

 - Создать в VPC subnet с названием public, сетью 192.168.10.0/24.
 - Создать в этой подсети NAT-инстанс, присвоив ему адрес 192.168.10.254. В качестве image_id использовать fd80mrhj8fl2oe87o4e1.
 - Создать в этой публичной подсети виртуалку с публичным IP, подключиться к ней и убедиться, что есть доступ к интернету.
3. Приватная подсеть.
 - Создать в VPC subnet с названием private, сетью 192.168.20.0/24.
 - Создать route table. Добавить статический маршрут, направляющий весь исходящий трафик private сети в NAT-инстанс.
 - Создать в этой приватной подсети виртуалку с внутренним IP, подключиться к ней через виртуалку, созданную ранее, и убедиться, что есть доступ к интернету.

Resource Terraform для Yandex Cloud:

- [VPC subnet](https://registry.terraform.io/providers/yandex-cloud/yandex/latest/docs/resources/vpc_subnet).
- [Route table](https://registry.terraform.io/providers/yandex-cloud/yandex/latest/docs/resources/vpc_route_table).
- [Compute Instance](https://registry.terraform.io/providers/yandex-cloud/yandex/latest/docs/resources/compute_instance).

---
### Задание 2. AWS* (задание со звёздочкой)

Это необязательное задание. Его выполнение не влияет на получение зачёта по домашней работе.

**Что нужно сделать**

1. Создать пустую VPC с подсетью 10.10.0.0/16.
2. Публичная подсеть.

 - Создать в VPC subnet с названием public, сетью 10.10.1.0/24.
 - Разрешить в этой subnet присвоение public IP по-умолчанию.
 - Создать Internet gateway.
 - Добавить в таблицу маршрутизации маршрут, направляющий весь исходящий трафик в Internet gateway.
 - Создать security group с разрешающими правилами на SSH и ICMP. Привязать эту security group на все, создаваемые в этом ДЗ, виртуалки.
 - Создать в этой подсети виртуалку и убедиться, что инстанс имеет публичный IP. Подключиться к ней, убедиться, что есть доступ к интернету.
 - Добавить NAT gateway в public subnet.
3. Приватная подсеть.
 - Создать в VPC subnet с названием private, сетью 10.10.2.0/24.
 - Создать отдельную таблицу маршрутизации и привязать её к private подсети.
 - Добавить Route, направляющий весь исходящий трафик private сети в NAT.
 - Создать виртуалку в приватной сети.
 - Подключиться к ней по SSH по приватному IP через виртуалку, созданную ранее в публичной подсети, и убедиться, что с виртуалки есть выход в интернет.

Resource Terraform:

1. [VPC](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/vpc).
1. [Subnet](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/subnet).
1. [Internet Gateway](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/internet_gateway).

### Правила приёма работы

Домашняя работа оформляется в своём Git репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
Файл README.md должен содержать скриншоты вывода необходимых команд, а также скриншоты результатов.
Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md.

---

### Решение 1

1) проверил конфигурацию terraform.

![image](https://github.com/user-attachments/assets/f9706b35-f598-4e6c-9f27-339f06471898)

2) запустил terraform apply.

![image](https://github.com/user-attachments/assets/a3306329-59eb-4eec-8eee-f732467b3d3e)

3) вывод outputs.tf. 

![image](https://github.com/user-attachments/assets/573a0e2e-c202-49d6-801e-0f5b5b1ed9f4)

4) Список машин в яндекс cloud.

![image](https://github.com/user-attachments/assets/968e101f-ac52-41bf-8b1a-edaa8042719c)

5) список VPC.
   
![image](https://github.com/user-attachments/assets/e240485e-5519-4fe9-9748-962084c75df2)

6) спиоск подсетей.

![image](https://github.com/user-attachments/assets/3f17f480-ab9e-4d76-956e-f3da5f983369)

7) схема.

![image](https://github.com/user-attachments/assets/460c0561-a38b-41c3-bdbd-167005910d3a)

8) Настрока "бастиона".

![image](https://github.com/user-attachments/assets/b728eb0f-8681-4868-a41a-a4c7b248fa27)

9) Подключился к публичной машине по ssh, проверил доступ к интернету и пинг приватной машины.

![image](https://github.com/user-attachments/assets/14abc337-1b5e-460a-bbb2-a8269d1ab060)

10) подключился к приватной машине, проверил доступ к интернеру и пинг публичной. 

![image](https://github.com/user-attachments/assets/49e7710d-ecdd-41dd-80f4-03f16805c37e)

11) удалил всё (terraform destroy)

![image](https://github.com/user-attachments/assets/1ce8392f-84ce-4ddf-a167-98392aa7e554)

---



