# ubuntu

# root login 
- sudo apt-get update 
- sudo apt install lightdm 
  - lightdm 선택 
- sudo passwd root 
  - 현재 계정 비번 
  - root 비번 
- nano /usr/share/lightdm/lightdm.conf.d/50-ubuntu.conf
  - greeter-show-manual-login=true 추가 
- 로그아웃 및 root 로그인 

# chrome install 
- sudo apt-get update 
- wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
- sudo sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
- sudo apt-get install google-chrome-stable
- sudo rm -rf /etc/apt/sources.list.d/google.list

# chrome remote setting 
- sudo usermod -a -G chrome-remote-desktop [계정이름]
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
    # 주석처리
    #self._launch_x_server(x_args)
    #self._launch_x_session()
    # 추가코드 두줄
    display = self.get_unused_display_number()
    self.child_env["DISPLAY"] = ":%d" % display
    
```
