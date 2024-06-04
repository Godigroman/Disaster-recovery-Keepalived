# Домашнее задание к занятию "`Disaster recovery и Keepalived`" - `Клименко Олег`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1


`Дана` [схема](https://github.com/netology-code/sflt-homeworks/blob/main/1/hsrp_advanced.pkt) `для Cisco Packet Tracer, рассматриваемая в лекции.`

`На данной схеме уже настроено отслеживание интерфейсов маршрутизаторов Gi0/1 (для нулевой группы)`

`Необходимо аналогично настроить отслеживание состояния интерфейсов Gi0/0 (для первой группы).`

`Для проверки корректности настройки, разорвите один из кабелей между одним из маршрутизаторов и Switch0 и запустите ping между PC0 и Server0.`

`На проверку отправьте получившуюся схему в формате pkt и скриншот, где виден процесс настройки маршрутизатора.`

![](https://cdn.discordapp.com/attachments/1228819419585577060/1247132960646238278/image.png?ex=66603c14&is=665eea94&hm=a04d4f2b6507717f293bd4a0d76bab7c58112fd4b586ec0bfab7b175f1072b42&)

![](https://cdn.discordapp.com/attachments/1228819419585577060/1247133568484900935/image.png?ex=66603ca5&is=665eeb25&hm=7175bffdf76e3fcf24ca1a5035e2f4707a2c958d87cd7c7d982de333e21a1cb0&)

![](https://cdn.discordapp.com/attachments/1228819419585577060/1247135063473389648/image.png?ex=66603e09&is=665eec89&hm=5bfcef727f29326ce4679ee0cd5965c97d556adcc0dc8b73d57411da9b097536&)

![](https://cdn.discordapp.com/attachments/1228819419585577060/1247136577877184512/image.png?ex=66603f72&is=665eedf2&hm=9f28b0d2f81fe34152a822d7b23b28057229f4b3e63c6578a5d66aee998647bb&)

![](https://cdn.discordapp.com/attachments/1228819419585577060/1247138076103413820/image.png?ex=666040d8&is=665eef58&hm=83ea53bda1551ed6d34ec083c49ee45c93edf7977e3c5849cd5c62c855190079&)

---

### Задание 2

`Приведите ответ в свободной форме........`

1. `Запустите две виртуальные машины Linux, установите и настройте сервис Keepalived как в лекции, используя пример конфигурационного` [файла](https://github.com/netology-code/sflt-homeworks/blob/main/1/keepalived-simple.conf)
2. `Настройте любой веб-сервер (например, nginx или simple python server) на двух виртуальных машинах`
3. `Напишите Bash-скрипт, который будет проверять доступность порта данного веб-сервера и существование файла index.html в root-директории данного веб-сервера.`
4. `Настройте Keepalived так, чтобы он запускал данный скрипт каждые 3 секунды и переносил виртуальный IP на другой сервер, если bash-скрипт завершался с кодом, отличным от нуля (то есть порт веб-сервера был недоступен или отсутствовал index.html). Используйте для этого секцию vrrp_script`
5. `На проверку отправьте получившейся bash-скрипт и конфигурационный файл keepalived, а также скриншот с демонстрацией переезда плавающего ip на другой сервер в случае недоступности порта или файла index.html`
   
![ip 2машин](https://cdn.discordapp.com/attachments/1228819419585577060/1247573637595402247/image.png?ex=666084fe&is=665f337e&hm=a7936cc0536ac408827aff9468dc37a6e53216c10d97470d0bbe6a3bdf280eea&)

### Bash.sh
```
my_index=`test -f /var/www/html/index.html && echo $?`
my_port=`bash -c "</dev/tcp/localhost/80" && echo $?`

if [ $my_index -eq 0 ] && [ $my_port -eq 0 ]; then
        exit 0
else
        exit 1
fi
```
### keepalived.conf
```
global_defs {
    enable_script_security
}

vrrp_script b_check {
    script "/usr/local/bin/bash.sh"
    interval 3
    rise 3
}

vrrp_instance VI_1 {
    state MASTER
    interface enp0s8
    virtual_router_id 15
    priority 205
    advert_int 1
    virtual_ipaddress {
        192.168.1.15/24
    }
    track_script {
        b_check
    }
}
```

![](https://cdn.discordapp.com/attachments/1228819419585577060/1247556688429449216/image.png?ex=66607535&is=665f23b5&hm=be8ddc074a2565e2e8cd4bd1b981cca91f3c94783a36f1abeed4787ce04a1634&)

![](https://cdn.discordapp.com/attachments/1228819419585577060/1247556972908384336/image.png?ex=66607578&is=665f23f8&hm=d7cf202780cf55a4c89c67a293accbd0d2fc656cb6cf8933a282524fa396e969&)

![](https://cdn.discordapp.com/attachments/1228819419585577060/1247557199459520633/image.png?ex=666075ae&is=665f242e&hm=51fa13925bd5d0b078746699dd74774809afe2fd28f2b3de868fad077544179b&)

![](https://cdn.discordapp.com/attachments/1228819419585577060/1247576413197045851/image.png?ex=66608793&is=665f3613&hm=98b58315af2887b424ce712f8d73e1d3df299d1ec8e7ee7e3368f26915652fd6&)

---
