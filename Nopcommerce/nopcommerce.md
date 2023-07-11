# Nop Commerce Manual Installation
----------------------------------
- wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
- sudo dpkg -i packages-microsoft-prod.deb
- sudo apt-get update
- sudo apt-get install -y dotnet-sdk-7.0
- mkdir /var/www/nopCommerce
- sudo mkdir /var/www/nopCommerce
- cd /var/www/nopCommerce
- sudo wget https://github.com/nopSolutions/nopCommerce/releases/download/release-4.60.3/nopCommerce_4.60.3_NoSource_linux_x64.zip
- sudo apt install unzip
- sudo unzip nopCommerce_4.60.3_NoSource_linux_x64.zip
- sudo mkdir bin
- sudo mkdir logs
- sudo chgrp -R www-data nopCommerce/
- sudo chown -R www-data nopCommerce/
- cd /etc/systemd/system/
- sudo vi nopCommerce.service
- sudo systemctl start nopCommerce.service
- sudo systemctl status nopCommerce.service

Service file:
-----------
```
[Unit]
Description=Example nopCommerce app running on Xubuntu
[Service]
WorkingDirectory=/var/www/nopCommerce
ExecStart=/usr/bin/dotnet /var/www/nopCommerce/Nop.Web.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=nopCommerce-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false
Environment=ASPNETCORE_URLS=http://0.0.0.0:5000
[Install]
WantedBy=multi-user.target

```
- build - ci cd
- self host agent 
- terrform
- ansible
- ./run.sh