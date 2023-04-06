# Install JDownloader on Ubuntu Server 16.04

Install Java and get JDownloader with the following commands.
```
sudo apt install openjdk-8-jre-headless
wget http://installer.jdownloader.org/JDownloader.jar
```
After that move `JDownloader.jar` and change the owner to the user (e.g. `media`) under which JDownloader should be executed.
```
sudo mkdir /opt/jdownloader
sudo mv JDownloader.jar /opt/jdownloader
sudo chown media:media /opt/jdownloader 
```
Now we are ready to install JDownloader. Execute this command several times until you get asked for your My JDownloader login credentials on the console.
```
java -jar /opt/jdownloader/JDownloader.jar -norestart
```
Then create a new systemd unit under `/etc/systemd/system/jdownloader.service` and paste this script.
```bash
[Unit]
Description=JDownloader Service
After=network.target

[Service]
Environment=JD_HOME=/opt/jdownloader
Type=oneshot
ExecStart=/usr/bin/java -Djava.awt.headless=true -jar /opt/jdownloader/JDownloader.jar
RemainAfterExit=yes
# Should be owner of /opt/jdownloader
User=media
Group=media

[Install]
WantedBy=multi-user.target
```
To enable JDownloader on startup run this command.
```
sudo systemctl enable jdownloader.service
```
After a reboot you can access your server under https://my.jdownloader.org

Enjoy!
