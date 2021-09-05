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

