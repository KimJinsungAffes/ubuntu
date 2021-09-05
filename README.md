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


