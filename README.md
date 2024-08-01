На vm A (192.168.1.197) ----
1. Установка Docker пакетов по официальной инструкции https://docs.docker.com/engine/install/ubuntu/ Install using the apt repository (п.1, 2)
2. Linux post-installation:
     sudo groupadd docker
     sudo usermod -aG docker $USER
     newgrp docker
     docker ps (для проверки, что можно выполнять без sudo)
3. Клонируем репозиторий с docker-compose.yml: git clone https://github.com/Fokines/test-task-cassandra.git
4. Переходим в папку: cd cd test-task-cassandra
5. Прежде чем поднять сервис, необходимо создать сеть типа ipvlan в Docker:
     docker network create -d ipvlan --subnet=192.168.1.0/24 --gateway=192.168.1.1 -o ipvlan_mode=l2 -o parent=enp0s3 db_net
6. Запускаем сервисы: docker compose up -d

На vm B (192.168.1.198) ----
1. Устанавливаем пакет python3-pip: sudo apt install python3-pip
2. устанавливаем cqlsh: pip install -U cqlsh
3. Проверяем, что cqlsh установлен: cqlsh --help
4. Подключаемся через cqlsh к каждому контейнеру:
   ![image](https://github.com/user-attachments/assets/6d12e24a-ca3e-4298-b186-3aae77da8a2d)

Дополнительно про ssh:
1. Генерируем пару ключей на хосте: ssh-keygen

На обоих машинах:
1. Правим файл /etc/ssh/sshd_config, убираем # со следующих строчек:
    Port 655 (22 меняем на, например, 655)
    PubkeyAuthentication yes
    PermitRootLogin yes
    PermitEmptyPasswords no
    PasswordAuthentication no

2. Перезапускаем службу SSH: service sshd restart
3. Копируем публичные ключи на удаленные машины:
    ssh-copy-id -i C:/Users/vasil/.ssh/id_rsa.pub avfokin@192.168.1.197
    ssh-copy-id -i C:/Users/vasil/.ssh/id_rsa.pub avfokin@192.168.1.198
