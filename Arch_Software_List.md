# Arch software list
###### tags: `Linux` `Arch`
> Arch linux 可讀式腳本,可以搭配Axelisme的[MD_Executor](https://github.com/Axelisme/MD_Executor.git) 實現自動安裝與設定。  
> Github: [Arch_Guiding_MD](https://github.com/stephenjkhsieh/Arch_Guiding_MD)  
> [Arch_Installation](https://hackmd.io/WA1Lslm3RnG1TC6sQoa7mQ?both)
> [name=Stephen JK Hsieh][name=Axelisme]

## GUI desktop environment (DE) 相關
### GUI protocol
#### X11
<!--
#%A:m {"GUI_protocol":["x11", "wayland"]} #@
#%A {"GPU":["nvidia", "amd", "intel"]} #@
-->
```bash= #%Q {"GUI_protocol":"x11"}
# X11 GUI protocol
yay -S xorg-server
```
#### Wayland
```bash= #%Q {"GUI_protocol":"wayland"}
# Wayland GUI protocol
yay -S qt5-wayland qt6-wayland xorg-xwayland
# if use Nvidia GPU with Wayland
#%%Q {"GPU":"nvidia"}
echo "GBM_BACKEND=nvidia-drm
__GLX_VENDOR_LIBRARY_NAME=nvidia" | sudo tee -a /etc/profile
#@@
```

<!--
#%A:m {"Desktop_environment":["kde", "i3", "sway"]} #@
-->
### DE launcher
```bash= #%Q {"Desktop_environment":".+"}
yay -S sddm                           #sddm DE launcher
sudo systemctl enable sddm.service    #啟動sddm DE登錄引導
```

### Desktop environment
#### KDE (類Windows的桌面)
```bash= #%Q {"Desktop_environment":"kde"}
yay -S plasma    #kde 桌面(repository choose noto-font)
# if use Wayland GUI protocol
#%%Q {"GUI_protocol":"wayland"}
yay -S plasma-wayland-session plasma-wayland-protocols 
#@@
```
一些kde-applications好用軟體
<!-- 
#%Q {"Desktop_environment":"kde"}
#%%A:m {"kde_application": ["ark", "colord-kde", "dolphin", "dolphin-plugins", "ffmpegthumbs", "filelight", "gwenview", "kate", "kdeconnect", "kdf", "konsole", "ksystemlog", "kwalletmanager", "okular", "partitionmanager", "spectacle", "yakuake", "kcharselect", "khelpcenter", "kolourpaint", "kamoso", "kcalc", "kdenlive", "kdegraphics-thumbnailers"]}
yay -S $(echo '{kde_application}' | sed 's/[][,"]//g')
#@@
#@
-->
```bash=
# 推薦rank1
yay -S ark colord-kde dolphin dolphin-plugins ffmpegthumbs filelight gwenview kate kdeconnect kdf konsole ksystemlog kwalletmanager okular partitionmanager spectacle yakuake
# 推薦rank2
yay -S kcharselect khelpcenter kolourpaint
# 推薦rank3
yay -S kamoso kcalc kdenlive kdegraphics-thumbnailers
```
yay -S  ;    #讓dolphin可預覽pdf

| 編號  | 名稱                     | 推薦度（🡫）| 描述                        |
| ---- | ----------------------- | --------- | -----------------------|
| 7    | ark                     | 1         | 壓縮軟體                |
| 16   | colord-kde              | 1         | 桌面色彩管理             |
| 17   | dolphin                 | 1         | 檔案管理                 |
| 18   | dolphin-plugins         | 1         | dolphin插件             |
| 22   | ffmpegthumbs            | 1         | 讓檔案瀏覽器預覽影片       |
| 23   | filelight               | 1         | 查看硬碟使用空間          |
| 27   | gwenview                | 1         | 看圖軟體                 |
| 39   | kamoso                  | 3         | 電腦相機拍照             |
| 44   | kate                    | 1         | 文字編輯器               |
| 53   | kcalc                   | 3         | 小計算機                 |
| 54   | kcharselect             | 2         | 特殊符號選擇庫            |
| 62   | kdeconnect              | 1         | 多裝置之間連線傳檔案       |
| 65   | kdenlive                | 3         | 影片剪輯工具              |
| 72   | kdf                     | 1         | 硬碟使用檢視              |
| 85   | khelpcenter             | 2         | KDE軟體說明文件           |
| 119  | kolourpaint             | 2         | 小畫家                   |
| 124  | konsole                 | 1         | 終端機                   |
| 142  | ksystemlog              | 1         | 查看systemlog           |
| 151  | kwalletmanager          | 1         | 電腦金鑰管理              |
| 162  | okular                  | 1         | PDF閱讀軟體              |
| 165  | partitionmanager        | 1         | 磁碟分割管理              |
| 176  | spectacle               | 1         | 螢幕截圖軟體              |
| 195  | yakuake                 | 1         | 下拉型終端機              |
| ?    | kdegraphics-thumbnailers| 1         | 讓dolphin可預覽pdf       |

## 影音與圖像媒體
### Player
<!-- 
#%A:m {"multimedia_player": ["mpv", "audacious", "smplayer", "smplayer-themes", "vlc"]}
yay -S $(echo '{multimedia_player}' | sed 's/[][,"]//g')
#@
-->
```bash= 
yay -S mpv ;                         #影片播放器
yay -S audacious ;                   #音樂播放器
yay -S yt-dlp ;                      #mpv對接youtube
yay -S smplayer smplayer-themes ;    #有更多界面的mpv與smplayer的主題
yay -S vlc ;                         #支援很多解碼格式的播放器
```
### 創作
<!-- 
#%A:m {"multimedia_editor": ["darktable", "gimp", "krita", "blender", "inkscape", "kdenlive"]}
yay -S $(echo '{multimedia_editor}' | sed 's/[][,"]//g')
#@
-->
```bash=
yay -S darktable ;    #照片Raw檔編輯輸出（lightroom）
yay -S gimp ;         #修圖軟體（photoshop）
yay -S krita ;        #繪圖軟體
yay -S blender ;      #3D繪圖
yay -S inkscape ;     #向量圖（illustrator）
yay -S kdenlive ;     #影片編輯
```

## 文書工作與筆記
<!-- 
#%A:m {"office_and_note": ["libreoffice-fresh-zh-tw", "xournalpp", "obsidian", "zotero-bin"]}
yay -S $(echo '{office_and_note}' | sed 's/[][,"]//g')
#@
-->
```bash=
yay -S libreoffice-fresh-zh-tw ;    #office
yay -S xournalpp ;                  #手寫筆記
yay -S obsidian ;                   #MD筆記
yay -S zotero-bin ;                 #文獻管理軟體
```

## 網絡
### 網路瀏覽器
<!-- 
#%A:m {"web_browser": ["firefox", "brave-bin", "google-chrome"]}
yay -S $(echo '{web_browser}' | sed 's/[][,"]//g')
#@
-->
```bash=
# firefox
#%Q {"web_browser":"firefox"}
yay -S firefox ;
echo '
if [ "$XDG_SESSION_TYPE" == "wayland" ]; then
    export QT_QPA_PLATFORM="wayland;xcb"
    export MOZ_ENABLE_WAYLAND=1
fi
' | sudo tee -a /etc/profile
sudo nano /etc/profile
#@

# Brave
yay -S brave-bin ;

# Chrome
yay -S google-chrome ;
```
### 通訊軟體
<!-- 
#%A:m {"communication_software": ["discord", "slack-desktop"]}
yay -S $(echo '{communication_software}' | sed 's/[][,"]//g')
#@
-->
```bash=
yay -S discord ;          #Discord
yay -S slack-desktop ;    #Slack中研院通訊軟體
```
### 遠端存儲
<!-- 
#%A:m {"remote_access": ["rclone", "rclone-browser", "sshfs", "anydesk-bin", "remmina"]}
yay -S $(echo '{remote_access}' | sed 's/[][,"]//g')
#@
-->
```bash=
yay -S rclone ;            #rclone雲端硬碟對接
yay -S rclone-browser ;    #rclone檔案瀏覽器

yay -S sshfs ;             #以ssh掛載遠端硬碟

yay -S anydesk-bin ;       #anydesk圖形化遠端操控
yay -S remmina ;           #remmina可作爲多種remmot protocol的前端
```
### 防火牆
<!-- 
#%A:m {"firewall": ["ufw"]}
yay -S $(echo '{firewall}' | sed 's/[][,"]//g')
#@
-->
```bash=
yay -S ufw ;    #ufw功能簡單的防火牆
```
### 其他網路細節
<!--
#%A:m {"network_else": ["l2tp_vpn"]} #@
-->
```bash=
# l2tp VPN
#%Q {"network_else": "l2tp_vpn"}
yay -S networkmanager-l2tp ;
yay -S strongswan ;
#@
```

## 程式開發與Terminal工具
### Coding工具
<!-- 
#%A:m {"coding_util": ["code", "miniconda3"]}
yay -S $(echo '{coding_util}' | sed 's/[][,"]//g')
#@
-->
```bash=
yay -S code ;    #VScode程式編輯器
yay -S miniconda3 ;                #Conda程式libray環境管理器
```
### Terminal小工具
<!-- 
#%A:m {"terminal_util": ["fzf", "nnn", "plocate", "tmux", "zoxide"]}
yay -S $(echo '{terminal_util}' | sed 's/[][,"]//g')
#@
-->
```bash=
#fzf搜尋器
#%Q {"terminal_util":"fzf"}
yay -S fzf ;          #指令搜尋器（模糊搜尋、動態搜尋）
yay -S fd ;           #尋找檔案（fzf搜尋太慢以fd作爲搜尋後端）
#@

# NNN終端機檔案瀏覽器
#%Q {"terminal_util":"nnn"}
yay -S nnn ;          #終端機械面的檔案瀏覽器
yay -S fd ;           #尋找檔案（NNN搜尋功能的後端）
yay -S trash-cli ;    #垃圾桶（NNN垃圾桶功能的後端）
yay -S xclip ;        #檔案剪貼簿與終端機的橋樑（NNN剪貼簿功能的後端）
#NNN插件，可預覽檔案...等等
curl -Ls https://raw.githubusercontent.com/jarun/nnn/master/plugins/getplugs | sh ;
sudo su -c "curl -Ls https://raw.githubusercontent.com/jarun/nnn/master/plugins/getplugs | sh"
#@

yay -S plocate ;      #plocate快速找檔案

yay -S tmux ;         #Tmux終端機分割視窗
 
yay -S zoxide ;       #Zoxide智能切換目錄(取代cd指令)
```
### 更好的shell Zsh
<!-- 
#%A:m {"better_shell": ["zsh"]} #@
-->
```bash=
#%Q {"better_shell": "zsh"}
yay -S zsh ;               #zsh本體
yay -S ttf-meslo-nerd ;    #安裝powerlevel10k字體
fc-cache -f -v ;           #初始化字體快取
yay -S zinit               #zinit zsh的插件管理器
#@
```

## 系統相關
### 系統資訊
<!-- 
#%A:m {"system_information": ["htop", "nvtop", "neofetch", "s-tui", "i7z"]}
yay -S $(echo '{system_information}' | sed 's/[][,"]//g')
#@
-->
```bash=
yay -S htop ;        #進程監控
yay -S nvtop ;       #顯卡監控
yay -S neofetch ;    #系統資訊總覽
yay -S s-tui ;       #資源監控
yay -S i7z           #intel CPU監視器
```

### 硬體控制
<!-- 
#%A:m {"system_hardware": ["tlp", "thermald", "boostchanger-git", "solaar", "piper", "fprintd"]}
yay -S $(echo '{system_hardware}' | sed 's/[][,"]//g')
#@
-->
```bash=
# Tlp電源省電管理
#%Q {"system_hardware":"tlp"}
yay -S tlp tlp-rdw    #電源省電管理tlp與其擴充
sudo systemctl enable tlp.service
sudo systemctl enable NetworkManager-dispatcher.service
systemctl mask systemd-rfkill.service systemd-rfkill.socket
#@

# 過熱控制(intel)
#%Q {"system_hardware":"thermald"}
yay -S thermald
systemctl enable --now thermald.service
#@

# CPU turbo頻率控制
yay -S boostchanger-git

# Logitech滑鼠控制
yay -S solaar ;    #自定Logitech滑鼠底層功能
yay -S piper ;     #自定Logitech滑鼠按鍵功能

# fprintd指紋解鎖
# 參考：https://wiki.archlinux.org/title/Fprint
yay -S fprintd ;
```

#### Nvidia的硬體控制
<!--
#%A {"GPU":["nvidia", "amd", "intel"]} #@
#%Q:n {"GPU":"nvidia", "GUI_protocol":"x11"}
#%%A:m {"system_hardware_nvidia": ["gwe", "opencl_and_cuda", "prime", "envycontrol"]} #@@
#@
#%Q:n {"GPU":"nvidia"}
#%%A:m {"system_hardware_nvidia": ["opencl_and_cuda", "prime", "envycontrol"]} #@@
-->
```bash=
# GWE Nvidia顯卡控制（超頻、風扇曲線 only for X11）
#%%Q {"system_hardware_nvidia":"gwe"}
yay -S gwe ;
#@@

# Nvidia顯卡科學運算加速（cuda, openCL...）
#%%Q {"system_hardware_nvidia":"opencl_and_cuda"}
yay -S ocl-icd opencl-headers ;
yay -S opencl-nvidia lib32-opencl-nvidia cuda cudnn ;
yay -S clinfo ;
echo '/usr/lib' | sudo tee /etc/ld.so.conf.d/00-usrlib.conf
#@@

# Nvidia prime切換當下應用程式是否用Nvidia顯卡跑
#%%Q:d {"system_hardware_nvidia":"prime"}
yay -S nvidia-prime
systemctl enable nvidia-persistenced.service
echo '
# Enable runtime PM for NVIDIA VGA/3D controller devices on driver bind
ACTION=="bind", SUBSYSTEM=="pci", ATTR{vendor}=="0x10de", ATTR{class}=="0x030000", TEST=="power/control", ATTR{power/control}="auto"
ACTION=="bind", SUBSYSTEM=="pci", ATTR{vendor}=="0x10de", ATTR{class}=="0x030200", TEST=="power/control", ATTR{power/control}="auto"

# Disable runtime PM for NVIDIA VGA/3D controller devices on driver unbind
ACTION=="unbind", SUBSYSTEM=="pci", ATTR{vendor}=="0x10de", ATTR{class}=="0x030000", TEST=="power/control", ATTR{power/control}="on"
ACTION=="unbind", SUBSYSTEM=="pci", ATTR{vendor}=="0x10de", ATTR{class}=="0x030200", TEST=="power/control", ATTR{power/control}="on"
' | sudo tee /etc/udev/rules.d/80-nvidia-pm.rules
#@@

# Envycontrol切換使用內/外顯卡（給用Nvidia的筆電省電用）
#%%Q {"system_hardware_nvidia":"envycontrol"}
yay -S envycontrol
#@@
```
<!--
#@
-->

### 重要的基本Backend
<!-- 
#%A:m {"system_basick_backend": ["exfatprogs", "unzip", "unrar", "p7zip"]}
yay -S $(echo '{system_basick_backend}' | sed 's/[][,"]//g')
#@
-->
```bash=
yay -S exfatprogs ;    #extfat格式化支援
yay -S unzip ;         #for *.zip
yay -S unrar ;         #for *.rar
yay -S p7zip ;         #for *.7z
```

### 系統其他
<!-- 
#%A:m {"system_else": ["downgrade"]}
yay -S $(echo '{system_else}' | sed 's/[][,"]//g')
#@
-->
```bash=
yay -S downgrade ;    #軟體回滾到舊版
```

## 其他
<!-- 
#%A:m {"else": ["chinese_input_method", "btrfs_snapshot", "virtual_machine", "gaming", "vinceliuice_grub_theme"]} #@
-->
```bash=
# 中文輸入法（新酷音&中州韻）
#%Q:c {"else": "chinese_input_method"}
yay -S fcitx5-im fcitx5-chewing fcitx5-chinese-addons fcitx5-rime;
echo "\
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
" | sudo tee -a /etc/environment ;
source /etc/environment ;
#@

# Btrfs快照備份相關
#%Q {"else": "btrfs_snapshot"}
yay -S snapper          #btrfs snapshot manager
yay -S snap-pac         #take snapshot after pacman
yay -S snap-pac-grub    #reconfig grub after snapshot
yay -S btrfs-assistant  #gui
sudo snapper -c root create-config / ;
sudo snapper -c home create-config /home ;
#@

# 虛擬機
#%Q {"else": "virtual_machine"}
yay -S virt-manager qemu-desktop dnsmasq ;    #要重新開機才能用
yay -S swtpm ;    #windows 11 需要tmp2晶片
sudo systemctl enable libvirtd.socket ;
#@

# 遊戲
# 參考: https://wiki.archlinux.org/title/steam
#%Q {"else": "gaming"}
yay -S steam ;     #Steam
yay -S gamemode lib32-gamemode ;    #gamemode遊戲加速器
#@

# grub開機主題
#%Q {"else": "vinceliuice_grub_theme"}
git clone https://github.com/vinceliuice/grub2-themes.git ~/grub2-themes ;
cd ~/grub2-themes ;
sudo ./install.sh -b -t stylish ;
cd ~;
rm -rf ~/grub2-themes ;
#@
```

## 實驗中
```bash=
yay -S gromit-mpx    #Gromit-mpx (用滑鼠在桌面畫畫)
yay -S barrier ;     #多裝置模擬成多螢幕

# 印表機
yay -S cups cups-pdf ;
yay -S print-manager ;
# gamemode啟動狀態indicator（for KDE）
yay -S plasma-gamemode-git ;   

```
#### i3 (X11的平鋪式DE)
```bash=
yay -S i3-wm
```
建議安裝的軟體
```bash=
# tool bar
yay -S polybar
# application menu
yay -S rofi
# Network Manager小圖示
yay -S network-manager-applet
# 開起.desktop檔，用於設定開機自啓動
yay -S dex
# 雙螢幕
yay -S xrandr arandr autorandr
# 桌面背景
yay -S nitrogen
# 合成器
yay -S picom
# sudo in GUI(需要root權限的GUI軟體需要)
yay -S polkit-dumb-agent-git
# 終端
yay -S alacritty
# 鎖屏
yay -S i3lock-color xidlehook
# 螢幕亮度
yay -S brightnessctl
# 音量
yay -S pavucontrol
```

#### sway (Wayland的平鋪式桌面)
```bash=
yay -S sway
```

#### 掛載外部btrfs硬碟的預設參數
```bash=
echo '
[defaults]
btrfs_defaults=noatime,space_cache=v2,compress=zstd
btrfs_allow=noatime,space_cache,compress,compress-force,datacow,nodatacow,datasum,nodatasum,degraded,device,discard,nodiscard,subvol,subvolid
' | tee -a /etc/udisks2/mount_options.conf
```
