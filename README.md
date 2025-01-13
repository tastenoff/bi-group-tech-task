# bi-group-tech-task


# Пошаговое описание установки:

### 1. Cоздаем VM
```bash
vagrant up
```
- **Описание:** Создаем виртуальные машины с помощью Vagrant. Этот шаг автоматически разворачивает указанные хосты (docker01, etcd01, pgsql01, pgsql02) с заданной конфигурацией.

---

### 2. Создание пользователей `ansible` на хостах
- **Описание:** На каждом хосте (docker01, etcd01, pgsql01, pgsql02) вручную создается пользователь `ansible`. У него должны быть права на выполнение команд с использованием `sudo` без пароля.

```bash
sudo useradd ansible
sudo usermod -aG wheel ansible
```

---

### 3. Создание SSH-ключа на `docker01`
- **Описание:** На хосте docker01 генерируется SSH-ключ для пользователя `ansible`. Этот ключ будет использоваться для автоматического подключения к остальным хостам.

```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/ansible_key
```

---

### 4. Копирование SSH-ключа на другие хосты
- **Описание:** Передаем созданный ключ `ansible_key.pub` на хосты etcd01, pgsql01 и pgsql02 для настройки доступа без пароля.

```bash
ssh-copy-id -i ~/.ssh/ansible_key.pub ansible@192.168.18.87 
ssh-copy-id -i ~/.ssh/ansible_key.pub ansible@192.168.18.83
ssh-copy-id -i ~/.ssh/ansible_key.pub ansible@192.168.18.84
ssh-copy-id -i ~/.ssh/ansible_key.pub ansible@192.168.18.85
```

---

### 5. Выполнение плейбуков Ansible
- **Описание:** На хосте `docker01`, находясь в директории `/home/ansible/ansible`, выполняются плейбуки Ansible для установки и настройки необходимых компонентов:
  - `etcd.yml` — устанавливает кластер Patroni + Postgresql на хостах pgsql01 и pgsql02
  - `pgsql_patroni.yml` — устанавливает кластер Patroni + Postgresql на хостах pgsql01 и pgsql02
  - `docker_install.yml` — устанавливает Docker на хостах.

```bash
ansible-playbook -i inventory/hosts.yml site.yml && ansible-playbook install_docker.yml -i inventory/hosts.yml
```

---

### 6. Запуск `docker-compose`
- **Описание:** На хосте `docker01`, из директории `/home/ansible`, запускается мониторинговый стек (Prometheus, PostgreSQL Exporter, Node Exporter и Grafana) с помощью `docker-compose`.

```bash
docker-compose up -d
```

---

### 7. Перезапуск Prometheus
- **Описание:** В случае изменения конфигурации Prometheus необходимо перезапустить его контейнер.

```bash
docker-compose restart prometheus
```

---

### 8. Доступ к Grafana
- **Описание:** Открываем веб-интерфейс Grafana для настройки и визуализации метрик по адресу:
```
http://192.168.67.4:3000
```

---

### 9. Доступ к Prometheus
- **Описание:** Проверяем сбор метрик через веб-интерфейс Prometheus по адресу:
```
http://192.168.67.4:9090
```
