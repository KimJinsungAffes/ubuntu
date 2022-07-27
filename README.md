# ubuntu

# 1. root login 
- sudo apt-get update 
- sudo apt install lightdm 
  - lightdm 선택 
- 자동로그인 설정 
  - sudo passwd root 
    - 현재 계정 비번 
    - root 비번 
  - sudo nano /usr/share/lightdm/lightdm.conf.d/50-ubuntu.conf
    - greeter-show-manual-login=true 추가 
  - reboot 
  - root 로그인 
  - 설정 -> Users -> Automatic Login 껏다 켜기 
  - reboot 

# (pass) 2. chrome install 
- (pass) sudo apt-get update 
- wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
- sudo sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
- (pass) sudo apt-get install google-chrome-stable
- chrome 수동 설치 
- sudo rm -rf /etc/apt/sources.list.d/google.list

# (pass) 3. chrome remote 
- google 계정 로그인 
- chrome remote 수동 설치 
- reboot 
- PC 등록 
- sudo nano /opt/google/chrome-remote-desktop/chrome-remote-desktop 
```
1) 수정 
- FIRST_X_DISPLAY_NUMBER = 0
  
2) 주석 
- #while os.path.exists(X_LOCK_FILE_TEMPLATE % display):
- #     display += 1

3) 수정 
- launch_session 함수를 찾아 수정한다. _launch_x_server()와 _launch_x_session()을 주석처리해서 새로운 display가 생성되지 않게 한다.
  그리고 두줄의 코드를 추가하여 기존의 디스플에이를 사용한다. 이제 파일을 저장하고 편집 툴을 종료한다.

def launch_session(self, x_args):
    self._init_child_env()
    self._setup_pulseaudio()
    self._setup_gnubby()

    # 코드 2줄 추가
    display = self.get_unused_display_number()
    self.child_env["DISPLAY"] = ":%d" % display

    # 주석
    #self._launch_x_server(x_args)
    #self._launch_x_session()
    
```

# 4. 잠금 해제 
- 설정 -> Privacy -> Screen Lock 가서 모두 never, disable 로 설정 

# 5. coms port  
- set permission
  - (pass) dmesg | grep tty
  - (pass) ls -l /dev/ttyS1
  - (pass) 현재 사용자가 속한 그룹들을 확인
    - id -Gn 
    - https://yaraba.tistory.com/613
  - sudo adduser $USER dialout 
- (pass) set sensor, relay board port 
  - default 
    - sensor PCB: com2 (com2, com3, com4)
    - sensor MZZ: com4 (com2, com3, com4)
    - relay  PCB: com5 (com5, com6)

# 6. mariadb install 
- sudo apt install mariadb-server
- sudo mysql_secure_installation
  - enter 
  - root 비번 설정 
  - y, n, y, y 
- sudo mysql -uroot -p
  - root 비번 입력 
- use mysql;
- update user set plugin='mysql_native_password' where user='root';
- flush privileges;
- create database elefarm;
- quit;
- (pass) service mysql restart 
- (pass) service mysql status 

# 7. nodejs, npm, yarn, pm2 install 
- sudo apt install nodejs
- sudo apt install npm
- sudo -s
- npm install yarn -g 
- npm install pm2 -g
- pm2 install pm2-logrotate
- exit

# (pass) 7-1. pm2 log file 
- (pass) cd /root/.pm2/logs 

# 8. vscode install 
- vscode 수동 설치 

# (pass) 9. github install 
- sudo wget https://github.com/shiftkey/desktop/releases/download/release-2.6.3-linux1/GitHubDesktop-linux-2.6.3-linux1.deb
- sudo apt install gdebi
- sudo gdebi GitHubDesktop-linux-2.6.3-linux1.deb
  - https://gist.github.com/berkorbay/6feda478a00b0432d13f1fc0a50467f1

# 10. code exec 
- (pass) github elefarm-gw zip file download at Desktop 
  - https://github.com/KimJinsungAffes/elefarm-gw
- (pass) cd Desktop
- (pass) mv ../Downloads/elefarm-gw-main.zip ./
- (pass) unzip elefarm-gw-main.zip 
- (pass) cd elefarm-gw-main

- sudo apt-get update 
- sudo apt-get install git-core 
- (pass) git version 
- mkdir github 
- cd github
- git init 
- git remote add origin https://github.com/KimJinsungAffes/elefarm-gw.git
- git config --global credential.helper store
- git pull origin live 
  - username & password 는 관리자한테 문의 
- cp ecosystem.config.js.dist ./ecosystem.config.js
  - set coms port 
  - nano ./ecosystem.config.js
    - SERIAL_PORT_PCB: '/dev/ttyS1'
    - SERIAL_PORT_WXA: '/dev/ttyS2'
    - SERIAL_PORT_MZZ: '/dev/ttyS3'
    - SERIAL_PORT_RELAY: '/dev/ttyS4'
  - (pass) cp /home/farm/Desktop/elefarm-gw-main/ecosystem.config.js ./
- cp ./back/gateway/config/env.config.js.dist ./back/gateway/config/env.config.js 
  - set farmSeq, gatewayNo
  - nano ./back/gateway/config/env.config.js
    - farmSeq: 491
    - gatewayNo: 'GW01',
  - (pass) cp /home/farm/Desktop/elefarm-gw-main/back/gateway/config/env.config.js ./back/gateway/config/
- yarn install 
- sudo -s
- pm2 start 
- (pass) pm2 list 
- (pass) pm2 log INIT
- (pass) pm2 log PCB
- (pass) pm2 log WXA
- (pass) pm2 log MZZ
- (pass) pm2 log SOCKET
- (pass) pm2 info INIT
- (pass) pm2 stop all 
- (pass) pm2 delete all 

# 11. auto patch crontab  
  - 설명 : patch는 1분 단위로 바로 gateway pc에 하고.. 실 적용은 수동으로 하거나.. 새벽 3시에 적용 자동 적용 됨 
  - 주의사항: 아래 사항은 root 계정이 아닌 ... ubuntu 계정으로 설정해야 함 
- crontab -e
  - nano edit로 선택 
  - */1 * * * * cd /home/farm/github && git pull origin live 
- service cron start 
- service cron status  
- (pass) crontab -l

# 12. startup set 
- pm2 
  - sudo -s
  - pm2 startup 
  - pm2 save 
- chrome 
  - application search
  - startup application ..
  - add
    - Name: chrome
    - Command: /usr/bin/google-chrome // terminal: which google-chrome 명령어로 확인 가능 
    - Command: google-chrome
  - set chrome default page as elefarm.net

# 13. korean keyboard set
- 설정 -> Region & Language -> Manage Installed Languages => install 
- reboot 
- 설정 -> Region & Language -> + -> Korean (Hangul)
  - (pass) 한국어-영어 전환 : Shift + Blank 

# 14. tool install 
- sudo apt-get update
- sudo apt install htop
- sudo apt install net-tools
- sudo apt install ncdu

# 15. check list 
- cup & memory usage check 
  - top -i
    - Ex. tracker-store eats a huge load of CPU
      - tracker reset --hard 
      - y
  - htop 
- disk usage check 
  - df -h // df => disk free 
  - du -sh * // 현재 디렉토리 사용량, du => disk usage 
    - cd /root/.pm2/logs/
    - du -sh *  
  - lsblk -f
  - ncdu /
- ip check 
  - ip addr
  - ifconfig 
- clear disk 
  - sudo find / -printf '%s %p\n'| sort -nr | head -10 // find big size files (detail) 
  - sudo journalctl --disk-usage && sudo journalctl --vacuum-time=7d // clear old systemd logs 

# (pass) 16. display set as mirror 

# (pass) 98. SQL patch 
- v21.10.06
  - alter table TEquip add column isLock varchar(1);
  - drop table TEquipStatus;
- v21.10.11
  - alter table TSensorData add column dataBuffer varchar(4000);
- v21.11.02
  - create index IDX_TSensorData on TSensorData(farmSeq,sensorSeq,dateTimeTo);
  - create index IDX2_TSensorData on TSensorData(farmSeq,gatewayNo,dateTimeTo);
          
# (pass) 99. exception 
``` 
Failure: File system check of the root filesystem failed
The root filesystem on /dev/sda5 requires a manual fsck 
```
- fsck -yf /dev/sda2
  - https://askubuntu.com/questions/885062/root-file-system-requires-manual-fsck

```
Reset user password
```
- sudo passwd farm10
  - https://www.cyberciti.biz/faq/change-a-user-password-in-ubuntu-linux-using-passwd/
