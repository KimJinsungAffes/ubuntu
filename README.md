# ubuntu

# 1. root login 
- sudo apt-get update 
- sudo apt install lightdm 
  - lightdm 선택 
- (pass) sudo passwd root 
  - 현재 계정 비번 
  - root 비번 
- (pass) sudo nano /usr/share/lightdm/lightdm.conf.d/50-ubuntu.conf
  - greeter-show-manual-login=true 추가 
- (pass) 로그아웃 및 root 로그인 
- reboot 

# 2. chrome install 
- (pass) sudo apt-get update 
- wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
- sudo sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
- (pass) sudo apt-get install google-chrome-stable
- chrome 수동 설치 
- sudo rm -rf /etc/apt/sources.list.d/google.list

# 3. chrome remote 
- google 계정 로그인 
- chrome remote 수동 설치 
- PC 등록 

# 4. chrome remote setting 
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

# 5. 잠금 해제 
- 설정 -> Privacy -> Screen Lock 가서 모두 never, disable 로 설정 

# 6. mariadb install 
- sudo apt install mariadb-server
- sudo mysql_secure_installation
  - enter 
  - root 비번 설정 
  - y, n, y, y 
- sudo mysql -uroot -p
  - root 비번 입력 
- create database elefarm;
- quit;

# 7. nodejs, npm, yarn, pm2 install 
- sudo apt install nodejs
- sudo apt install npm
- sudo -s
- npm install yarn -g 
- npm install pm2 -g

# (pass) 8. vscode install 
- root 계정으로 로그인 후 vscode 설치 

# 9. code upload 
- github elefarm-gw zip file download at Desktop 
  - https://github.com/KimJinsungAffes/elefarm-gw
- cd Desktop
- mv /home/farm01/Downloads/elefarm-gw-main.zip ./
- unzip elefarm-gw.zip 
- cd elefarm-gw-main/back
- yarn install 

# (pass) ubuntu coms serial port 
- dmesg | grep tty
- id -Gn
- sudo adduser $USER dialout 
  - https://yaraba.tistory.com/613
