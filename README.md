# torrserver-jackett-http-server
Инструкция для установки Torrserver, Jackett и Http Server в Ubuntu

# Устанавливаем [Torrserver](https://github.com/YouROK/TorrServer)
```
sudo wget -c https://github.com/YouROK/TorrServer/releases/download/MatriX.106/TorrServer-linux-amd64 -O /usr/bin/torrserver
sudo chmod +x /usr/bin/torrserver
# Скачиваем юнит для systemd
sudo wget https://raw.githubusercontent.com/YouROK/TorrServer/master/torrserver.service -O /usr/lib/systemd/system/torrserver.service
# Фиксим пути к бинарнику и базе данных
sudo sed -i "s/path\/to\/torrserver/usr\/bin\/torrserver/g" /usr/lib/systemd/system/torrserver.service
sudo sed -i "s/path\/to\/db\//tmp/g" /usr/lib/systemd/system/torrserver.service
# Включаем и запускаем юнит
sudo systemctl daemon-reload
sudo systemctl enable torrserver
sudo systemctl start torrserver
# Проверяем
xdg-open http://localhost:8090
```

# Устанавливаем [Jackett](https://github.com/Jackett/Jackett)
```
cd /tmp
wget -c https://github.com/Jackett/Jackett/releases/download/v0.18.845/Jackett.Binaries.LinuxAMDx64.tar.gz
mkdir ~/.jackett
tar -xzvf Jackett.Binaries.LinuxAMDx64.tar.gz -C ~/.jackett/
# Включаем и запускаем юнит
sudo bash ~/.jackett/Jackett/install_service_systemd.sh
# Проверяем
xdg-open http://localhost:9117
```

# Устанавливаем [NodeJS](https://deb.nodesource.com) и [Http Server](https://github.com/http-party/http-server)
```
cd /tmp
wget https://deb.nodesource.com/setup_16.x -O nodesource_setup.sh
sudo bash nodesource_setup.sh
sudo apt install -y nodejs
# Устанавливаем http-server
sudo npm install -g npm http-server
# Скачиваем юнит для Http Server
sudo wget https://raw.githubusercontent.com/varlesh/torrserver-jackett-http-server/main/http-server.service -O /usr/lib/systemd/system/http-server.service
# Теперь вам нужно открыть юнит и прописать IP-адресс в строчке ExecStart вашего сервера, вместо YOUR_IP_SERVER.
# Важно!!! Не пишите там localhost или 127.0.0.1, а реальный адресс, к примеру 192.168.1.2
sudo nano /usr/lib/systemd/system/http-server.service
# Сохраните изменения Ctrl+O, подтвердите Enter, закройте Ctrl+X
# Включаем и запускаем юнит
sudo systemctl daemon-reload
sudo systemctl enable http-server
sudo systemctl start http-server
# Проверяем
xdg-open http://localhost:8080
# Помните, что адресс jackett, который будет использоваться теперь на порту 8080
```
