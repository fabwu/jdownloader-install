# Install JDownloader on Ubuntu Server 16.04

Install Java and get JDownloader with the following commands.
```
sudo apt install openjdk-8-jre-headless
wget http://installer.jdownloader.org/JDownloader.jar
```
After that move JDownloader and change the owner to the user (e.g `media`) under which JDownloader should be executed.
```
sudo mkdir /opt/jdownloader
sudo mv jdownloader.jar /opt/jdownloader
sudo chown -R media:media /opt/jdownloader 
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
User=media # Should be owner of /opt/jdownloader
Group=media # Should be owner of /opt/jdownloader

[Install]
WantedBy=multi-user.target
```
To enable and start JDownloader run this commands.
```
sudo systemctl enable jdownloader.service
sudo systemctl start jdownloader.service
```
After JDownloader is updated you can fill in your My JDownloader credentials under `/opt/jdownloader/cfg/org.jdownloader.api.myjdownloader.MyJDownloaderSettings.json`.
```
{
  "password" : "<YOUR PASSWORD>",
  "email" : "<YOUR EMAIL>"
}
```
Now you can access your server under https://my.jdownloader.org

Enjoy!
