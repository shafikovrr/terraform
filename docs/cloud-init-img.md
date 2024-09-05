# cloud-init-img

##### Скачать образ 
```
wget https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img
```
##### Создаем новую ВМ с параметрами
```
qm create 9000 --memory 2048 --net0 virtio,bridge=vmbr0 --scsihw virtio-scsi-pci
```
##### Импортируем ранее скачанный образ в ранее созданную ВМ, как scsi0 диск и сохраняем его в хранилище hdd-local
```
qm set 9000 --scsi0 hdd-local:0,import-from=/root/jammy-server-cloudimg-amd64.img
```
##### Добавим cdrom который будет использоваться, чтобы передать данные Cloud-Init в ВМ
```
qm set 9000 --ide2 hdd-local:cloudinit
```
##### Ограничим загрузку системы созданным ранее диском
```
qm set 9000 --boot order=scsi0
```
##### Многие CloudInit имиджи требуют серийную консоль как средство вывода информации, но если вдруг серийная консоль работать не будет, необходимо удалить её и использовать обычный монитор (по умолчанию)
```
qm set 9000 --serial0 socket --vga serial0
```
##### Установим сетевой интерфейс как dhcp
```
qm set 9000 --ipconfig0 ip=dhcp
```
##### Включим поддержку qemu-guest-agent
```
qm set 9000 --agent enabled=1
```
##### Установим пользователя
```
qm set 9000 -ciuser username
```
##### Установим пароль.
```
qm set 9000 -cipassword 'password'
```
##### С помощью ключа --sshkey - передаем путь до файла с открытым ключём чтобы заранее записать его в образ.
```
qm set 9000 --sshkey ~/.ssh/id_rsa.pub
```
##### Загрузиться с ВМ и установить fail2ban и qemu-guest-agent
```
sudo apt update && sudo apt install fail2ban qemu-guest-agent
```
##### Запустить fail2ban и внести в автозагрузку
```
sudo systemctl start fail2ban
sudo systemctl enable fail2ban
```
##### Отредактировать конфигурацию ssh
```
sudo nano /etc/ssh/sshd_config
```
или
```
sudo sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin no/g' /etc/ssh/sshd_config
sudo sed -i 's/#PubkeyAuthentication yes/PubkeyAuthentication yes/g' /etc/ssh/sshd_config
sudo sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/g' /etc/ssh/sshd_config
sudo sed -i 's/#PermitEmptyPasswords no/PermitEmptyPasswords no/g' /etc/ssh/sshd_config
```
##### Перезапустить sshd и проверить статус
```
sudo systemctl restart sshd.service
sudo systemctl status sshd.service
```
##### Настройка jail.local sshd
```
sudo nano /etc/fail2ban/jail.local
```
```
[DEFAULT]
# Set the ban time in seconds (e.g., 3600 seconds = 1 hour)
bantime = 3h
findtime = 10m
maxretry = 3
ignoreip = 127.0.0.1 192.168.0.5

bantime.increment = true
bantime.rndtime = 30m
bantime.maxtime = 60d

[sshd]
enabled = true
port = ssh
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
bantime = 3h
```
##### Перезапуск fail2ban и проверка статуса
```
sudo systemctl restart fail2ban
sudo systemctl status fail2ban
```
##### Очистить историю
```
history -c
```
##### Выключаем ВМ
```
qm stop 9000
```
##### Создаем template
```
qm template 9000
```

```
https://github.com/Telmate/terraform-provider-proxmox/releases
https://pve.proxmox.com/wiki/Cloud-Init_Support
https://cloud-images.ubuntu.com/
```
