# bi-group-tech-task


1. vagrant up

2. Создаем юзеров ansible на хостах docker01, etcd01, pgsql01, pgsql02

3. На хосте docker01 создаем SSH-ключ:
ssh-keygen -t rsa -b 4096 -f ~/.ssh/ansible_key

4. ssh-copy-id -i ~/.ssh/ansible_key.pub ansible@192.168.67.5 /
ssh-copy-id -i ~/.ssh/ansible_key.pub ansible@192.168.67.6 / 
ssh-copy-id -i ~/.ssh/ansible_key.pub ansible@192.168.67.7 /

5. C хоста docker01, из директории /home/ansible/ansible: ansible-playbook -i inventory/hosts.yml site.yml && ansible-playbook install_docker.yml -i inventory/hosts.yml

6. C хоста docker01, из директории /home/ansible docker-compose up -d

7. docker-compose restart prometheus

8. http://192.168.67.4:3000

9. http://192.168.67.4:9090



