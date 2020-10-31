# 設定可以使用VNC遠端連線至Jetson nano
<ol>
## <li> ssh連線進jetson nano或直接本機登入(不能使用192.168.55.1 usb接線方式登入)

## <li>安裝vino-server
$ sudo apt install vino

## <li>設定vnc遠端連線時不使用認證方式連線
$ gsettings set org.gnome.Vino prompt-enabled false  
#若設定true會在本機跳出訊息,讓使用者決定能不能連線

$ gsettings set org.gnome.Vino require-encryption false

## <li>將網路卡加入(好像不用此動作,還沒測試過)
$ nmcli connection show #使用此指令列出網卡的UUID
![nmcli connection show #使用此指令列出網卡的UUID](https://github.com/yotrew/notes/blob/main/pics/01.png)

$ dconf write /org/gnome/settings-daemon/plugins/sharing/vino-server/enabled-connections "['UUID of the ethernet']"   #UUID of the ethernet的地方填入上面的UUID,如4e07fa5f-b202-4b75-b39d-910aed54aa67

$ export DISPLAY=:0

## <li>連線測試
$/usr/lib/vino/vino-server 
#下完指令後不要關閉

$ifconfig 
#使甪ifconfig找出wifi的IP或使用192.168.55.1(要有使用usb線連接到jetso nano)
![02](https://github.com/yotrew/notes/blob/main/pics/02.png)

在電腦上下載real vnc viewer
https://www.realvnc.com/en/connect/download/viewer/
![03](https://github.com/yotrew/notes/blob/main/pics/03.png)

使用real vnc viewer連線,若能連線成功即可控制

## <li>若要每次啟動jetnano後,就能使用vnc連端,必須讓jetson nano自動登入
Ref:https://justhodl.blogspot.com/2018/03/ubuntu-server-1604-auto-login-non-gui.html
先設定自動登入,搜尋user accounts(如下圖),並執行
![04](https://github.com/yotrew/notes/blob/main/pics/04.png)

把dlinano設成自動登入(如下圖),Automatic Login → set ‘ON’
![05](https://github.com/yotrew/notes/blob/main/pics/05.png)


設定開機登入後即啟動VINO-server,搜尋start up(如下圖),並執行
![06](https://github.com/yotrew/notes/blob/main/pics/06.png)


設定如下圖,按Add-->command輸入/usr/lib/vino/vino-server
![07](https://github.com/yotrew/notes/blob/main/pics/07.png)

## <li>若連線後解析度不適合，可執行以下指令調整
Ref:http://omnixri.blogspot.com/2019/05/nvidia-jetson-nanoopencv410cuda.html
$ sudo xrandr --fb 1420x800
</ol>
