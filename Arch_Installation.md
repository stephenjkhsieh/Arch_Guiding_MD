# Arch installation
###### tags: `Linux` `Arch`
> Arch linux 可讀式腳本,可以搭配Axelisme的[MD_Executor](https://github.com/Axelisme/MD_Executor.git) 實現自動安裝與設定。  
> Github: [Arch_Guiding_MD](https://github.com/stephenjkhsieh/Arch_Guiding_MD)  
> [name=Stephen JK Hsieh][name=Axelisme]

<!--
#%A:f {"step":["Live USB","Live USB_chroot","TTY root","TTY user"]} #@
-->
## Arch 安裝I  （Live USB）
<!--
#%Q:n {"step":"Live USB"}
-->
### 調tty字體大小
<!--
#%%A {"word_size":["big","small"]} #@@
-->
```bash= #%%Q {"word_size":"big"}
setfont ter-132n    #大字體
```

### 連網路
```bash=
#%%Q {}
ping -c 5 8.8.8.8 #確認連線狀態
#@@

#如果要連wifi
iwctl    #進入iwctl界面
device list    #列出網卡硬體
station wlan0 scan    #掃描網卡wlan0底下偵測到的wifi
station wlan0 get-networks    #顯示網卡wlan0底下偵測到的wifi
station wlan0 connect "wifi"    #用網卡wlan0連接"wifi"
exit    #離開iwctl界面

# 如果要連有線(static ip)
ip link
ip addr
ip route
ip addr flush dev enpXXX
ip route flush dev enpXXX
ip addr add address/24 dev enpXXX
ip route add default via gateway
nano /etc/resolv.conf  #加上nameserver DNS_IP
```

### 更新系統時間
```bash= #%%Q {}
timedatectl set-ntp true
```

<!--
#%%A:m {"Process_partition_format_mount":["Partition", "Format", "Mount"]} #@@
-->
### 劃分磁碟分區
<!--
#%%Q:n {"Process_partition_format_mount":"Partition"}
#%%%Q {}
lsblk
#@@@
#%%%A {"Name_of_disk1":".+", "Name_of_disk2":".*"} #@@@
-->
```bash=
lsblk    #顯示磁碟分區狀態

#%%%Q:c {"Name_of_disk1":".+"}
gdisk /dev/{Name_of_disk1}    #磁碟格式工具，x專家模式後，z刪除所有分區
#@@@

#%%%Q:c {"Name_of_disk1":".+"}
cfdisk /dev/{Name_of_disk1}    #圖形化分割磁碟
#@@@
```
<!--
#%%%Q:c {"Name_of_disk2":".+"}
gdisk /dev/{Name_of_disk2}    #磁碟格式工具，x專家模式後，z刪除所有分區
#@@@

#%%%Q:c {"Name_of_disk2":".+"}
cfdisk /dev/{Name_of_disk2}    #圖形化分割磁碟
#@@@
#@@
-->

<!--
#%%Q:vor {"Process_partition_format_mount":["Format", "Mount"]}
lsblk
#%%%A {"efi_partition_name":".+"} #@@@
#%%%A {"swap_partition_name":".*"} #@@@
#%%%A {"root_partition_name":".+"} #@@@
#%%%A {"home_partition_name":".*"} #@@@
#%%%A {"filesystem":["btrfs","ext4"]} #@@@
#@@
-->
### 格式化磁碟分區
<!--
#%%Q:n {"Process_partition_format_mount":"Format"}
-->
```bash=
#%%%Q:c {"efi_partition_name":".+"}
mkfs.fat -F 32 /dev/{efi_partition_name}    #EFI分區格式化成Fat32
#@@@

#%%%Q:c {"swap_partition_name":".+"}
mkswap /dev/{swap_partition_name}    #格式化swap
#@@@

# 如果用btrfs:
#%%%Q:c {"filesystem":"btrfs", "root_partition_name":".+"}
mkfs.btrfs -f /dev/{root_partition_name}    #格式化root分區成btrfs
#@@@
#%%%Q:c {"filesystem":"btrfs", "home_partition_name":".+"}
mkfs.btrfs -f /dev/{home_partition_name}    #格式化home分區成btrfs
#@@@

# 如果用ext4:
#%%%Q:c {"filesystem":"ext4", "root_partition_name":".+"}
mkfs.ext4 -f /dev/{root_partition_name}    #格式化root分區成ext4
#@@@
#%%%Q:c {"filesystem":"ext4", "home_partition_name":".+"}
mkfs.ext4 -f /dev/{home_partition_name}    #格式化home分區成ext4
#@@@
```
<!--
#@@
-->

### 掛載磁碟分區
<!--
#%%Q:n {"Process_partition_format_mount":"Mount"}
-->
如果使用btrfs:
```bash= 
lsblk
#掛載root
#%%%Q {"filesystem":"btrfs", "root_partition_name":".+"}
mkdir /mnt/btrfs_root  #創建資料夾
mkdir /mnt/root
mount /dev/{root_partition_name} /mnt/btrfs_root
btrfs subvolume create /mnt/btrfs_root/@  #建立子捲
mount /dev/{root_partition_name} -o subvol=@ /mnt/root #掛載
#@@@

#掛載boot
#%%%Q {"filesystem":"btrfs", "efi_partition_name":".+"}
mkdir /mnt/root/boot
mount /dev/{efi_partition_name} /mnt/root/boot    #EFI分區掛載到/mnt
#@@@

##掛載home
#%%%Q {"filesystem":"btrfs", "home_partition_name":".+"}
mkdir /mnt/btrfs_home
mount /dev/{home_partition_name} /mnt/btrfs_home
btrfs subvolume create /mnt/btrfs_home/@home
mkdir /mnt/root/home
mount /dev/{home_partition_name} -o subvol=@home /mnt/root/home
#@@@
```

如果使用ext4:
```bash= 
lsblk
#掛載root
#%%%Q {"filesystem":"ext4", "root_partition_name":".+"}
mkdir /mnt/root
mount /dev/{root_partition_name} /mnt/root
#@@@

#掛載boot
#%%%Q {"filesystem":"ext4", "efi_partition_name":".+"}
mkdir /mnt/root/boot
mount /dev/{efi_partition_name} /mnt/root/boot    #EFI分區掛載到/mnt/boot
#@@@

#掛載home
#%%%Q {"filesystem":"ext4", "home_partition_name":".+"}
mkdir /mnt/root/home
mount /dev/{home_partition_name} /mnt/root/home
#@@@
```

掛載swap
```bash= #%%%Q {"swap_partition_name":".+"}
swapon /dev/{swap_partition_name}    #掛載swap分區
```
<!--
#@@
#%%Q {}
df -h
echo "check it!"
#@@
-->

### 安裝系統
```bash=
#%%A {"kernel":["linux", "linux-lts", "linux-zen"], "CPU":["intel", "amd"]}
pacman -Syy    #更新資料庫
pacstrap /mnt/root base linux-firmware {kernel}    #安裝基礎包
pacstrap /mnt/root {CPU}-ucode   #安裝微碼
#@@
```

### 設定開機引導文件
```bash= #%%Q {}
genfstab -U /mnt/root >> /mnt/root/etc/fstab    #Fstab引導開機系統掛載
# if use btrfs, then
nano /mnt/root/etc/fstab
# 給root與home分區加上 autodefrag,compress=zstd
# relatime改成noatime
# 去除subvolid
```
<!--
#@
-->

## Arch安裝II （chroot）
<!--
#%Q:n {"step":"Live USB_chroot"}
-->
### 改變root位置
```bash=
arch-chroot /mnt/root          #把新裝的系統掛為root
```

### 必要軟體
```bash= #%%Q {}
pacman -S vi vim neovim nano          #基礎文字編輯
pacman -S networkmanager dnsmasq net-tools iw wireless_tools       #網路管理
pacman -S usbutils    #usb硬體管理
pacman -S bash-completion      #bash自動補字
pacman -S terminus-font        #tty字體
```

### 設定系統
```bash= #%%Q {}
#時區
ln -sf /usr/share/zoneinfo/Asia/Taipei /etc/localtime    #設置時區
hwclock --systohc                                        #同步時區

# 設定系統語言
sed -i 's/^#\?\(en_US.UTF-8 UTF-8\)/\1/1' /etc/locale.gen
sed -i 's/^#\?\(zh_TW.UTF-8 UTF-8\)/\1/1' /etc/locale.gen
nano /etc/locale.gen    #編輯語言庫，將要啟用的語言取消註解，如en_US、zh_TW
locale-gen              #生成語言資料
echo '
LANG=en_US.UTF-8' | tee -a /etc/locale.conf
nano /etc/locale.conf   #添加 LANG=en_US.UTF-8

#設定主機名，"hostname"可替換成想要的名字
#%%%A {"hostname":".+"}
echo "{hostname}" >> /etc/hostname
#@@@

#設定root密碼
echo "enter root passwd:"
passwd
```

### 安裝開機引導Grub
<!--
#%%A {"filesystem":["btrfs","ext4"]} #@@
-->
```bash= #%%Q {}
pacman -S grub efibootmgr           #Grub
grub-install --target=x86_64-efi --bootloader-id=GRUB --efi-directory=/boot
# if use grub-btrfs, it is recommend to read:
# https://github.com/Antynea/grub-btrfs/blob/master/initramfs/readme.md
#%%%Q {"filesystem":"btrfs"}
pacman -S grub-btrfs inotify-tools  #Grub-btrfs
systemctl enable grub-btrfsd
#@@@

#%%%Q:c {}
cp /etc/default/grub /etc/default/grub.backup
#@@@
```
<!--
#%%Q {}
sed -Ei 's/^#?(GRUB_TIMEOUT)=[0-9]+$/\1=1/1' /etc/default/grub
sed -Ei 's/^(GRUB_CMDLINE_LINUX_DEFAULT)="(.*) quiet (.*)"$/\1="\2 nowatchdog \3"/1' /etc/default/grub
#@@
-->
```bash= #%%Q {}
nano /etc/default/grub
#TIMEOUT改1，GRUB_CMDLINE_LINUX_DEFAULT改"... nowatchdog ..."，並去除quiet
#intel太新的CPU有bug，若重開機時Load Kernal fail，要再加上"ibt=off"
grub-mkconfig -o /boot/grub/grub.cfg
```

### 啟動服務
```bash= #%%Q {}
systemctl enable NetworkManager    #啟動網路服務
systemctl enable fstrim.timer      #照顧SSD硬碟
```
### 離開chroot
```bash= #%%Q {}
exit    #離開chroot
```

### 關閉電腦
```bash=
umount -R /mnt/root
umount -R /mnt/btrfs_root
umount -R /mnt/btrfs_home
shutdown now 
```
<!--
#@
-->

## Arch 安裝III （TTY root）
<!--
#%Q:n {"step":"TTY root"}
-->
### 調tty字體大小
<!--
#%%A {"word_size":["big","small"]} #@@
-->
```bash= #%%Q {"word_size":"big"}
setfont ter-132n    #大字體
echo 'FONT=ter-132n' | tee -a /etc/vconsole.conf    #永久設定大字體
```

### 連網路
```bash=
#%%Q {}
ping -c 5 8.8.8.8    #確認連線狀態
#@@

#如果需要設定連網
nmtui    #進入networkmanager TUI
```

### pacman 設定
```bash= #%%Q:c {}
cp /etc/pacman.conf /etc/pacman.conf.backup
```
<!--
#%%Q {}
sed -Ei 's/^#?(Color)$/\1/1' /etc/pacman.conf
sed -Ei '/^#?Color$/a ILoveCandy' /etc/pacman.conf
sed -Ei 's/^#?(ParallelDownloads).*/\1 = 5/1' /etc/pacman.conf
sed -Ei 's/^#?(UseSyslog)$/\1/1' /etc/pacman.conf
sed -Ei 's/^#?(CheckSpace)$/\1/1' /etc/pacman.conf
sed -Ei 's/^#?(VerbosePkgLists)$/\1/1' /etc/pacman.conf
sed -Ei 's/^#?(\[multilib\])$/\1/1' /etc/pacman.conf
sed -Ei '/^#?\[multilib\]$/a Include = /etc/pacman.d/mirrorlist' /etc/pacman.conf
#@@
-->
```bash= #%%Q {}
nano /etc/pacman.conf
# 在misc options 下: 
# Color
# ParallelDownloads = 5
# ILoveCandy
# UseSyslog
# CheckSpace
# VerbosePkgLists
# 取消註解[multilib]以及其Include

pacman -Syyu    #更新
```

### 讓make使用多核編譯
```bash= #%%Q {}
cp /etc/makepkg.conf /etc/makepkg.conf.backup
sed -Ei 's/^#?MAKEFLAGS=".*"/MAKEFLAGS="-j$(nproc)"/1' /etc/makepkg.conf
nano /etc/makepkg.conf    #設 MAKEFLAGS="-j$(nproc)"
```

### 排序mirror list
請參考 https://wiki.archlinux.org/title/mirrors
mirror可從 https://archlinux.org/mirrorlist/ 獲得
```bash= #%%Q:c {}
pacman -S pacman-contrib         #rankmirrors command
cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.backup
echo '
## Taiwan
Server = https://archlinux.cs.nycu.edu.tw/$repo/os/$arch
Server = http://ftp.tku.edu.tw/Linux/ArchLinux/$repo/os/$arch
Server = http://mirror.archlinux.tw/ArchLinux/$repo/os/$arch
Server = http://archlinux.ccns.ncku.edu.tw/archlinux/$repo/os/$arch
Server = https://mirror.archlinux.tw/ArchLinux/$repo/os/$arch
' | tee -a /etc/pacman.d/mirrorlist.backup
rankmirrors /etc/pacman.d/mirrorlist.backup | tee /etc/pacman.d/mirrorlist

pacman -Syyu    #更新pacman的mirrorlist
```

### 重要軟體
```bash= #%%A {"kernel":["linux", "linux-lts", "linux-zen"], "CPU":["intel", "amd"]}
pacman -S sudo                         # 管理者權限
pacman -S {kernel}-headers base-devel  #linux標頭檔、編譯基礎工具
pacman -S mesa                         #顯卡渲染驅動（intel & AMD）
pacman -S lm_sensors                   #設備狀況監控
pacman -S fakeroot                     #fakeroot
pacman -S make gcc                     #編譯C相關
pacman -S python python-pip            #Python相關
pacman -S bluez bluez-utils            #藍牙
systemctl enable bluetooth.service     #啓動藍牙服務
pacman -S alsa-utils pipewire pipewire-pulse pipewire-alsa pipewire-jack #音效
pacman -S sof-firmware                 #新型音效卡卡driver
pacman -S noto-fonts noto-fonts-cjk noto-fonts-emoji    #字體
pacman -S wget openssh git man         #其他 （wget URL下載、ssh通訊協定、git、man顯示指令說明）

#%%%Q {"CPU":"intel"}
pacman -S intel-media-driver vulkan-intel    #Intel GPU硬件視頻加速、3D渲染加速（只適用Intel）
#@@@
#%%%Q {"CPU":"amd"}
pacman -S libva-mesa-driver mesa-vdpau xf86-video-amdgpu vulkan-radeon    #AMD GPU硬件視頻加速、3D渲染加速（只適用AMD）
#@@@
```
<!--
#%%Q:c {} 
cp /etc/mkinitcpio.conf /etc/mkinitcpio.conf.backup 
#@@
-->

### btrfs相關
#### btrfs 內核HOOKS設定
<!--
#%%A {"filesystem":["btrfs","ext4"]} #@@
#%%Q {"filesystem":"btrfs"}
sed -Ei 's/^(HOOKS)=\((.*)\)$/\1=(\2 grub-btrfs-overlayfs)/1' /etc/mkinitcpio.conf
#@@
-->
```bash= #%%Q: {"filesystem":"btrfs"}
nano /etc/mkinitcpio.conf
# HOOKS=(base ... modconf ... fsck grub-btrfs-overlayfs) # Grub-btrfs
mkinitcpio -p {kernel}
```

#### 掛載外部btrfs硬碟的預設參數
```bash= #%%Q:c {"filesystem":"btrfs"}
echo '
[defaults]
btrfs_defaults=noatime,space_cache=v2,compress=zstd
btrfs_allow=noatime,space_cache,compress,compress-force,datacow,nodatacow,datasum,nodatasum,degraded,device,discard,nodiscard,subvol,subvolid
' | tee -a /etc/udisks2/mount_options.conf
```

<!--
#%%A {"GPU":["nvidia", "amd", "intel"]} #@@
-->
### 顯卡相關設定

#### Intel顯卡設定(if use Intel card)
相關資訊請看 https://wiki.archlinux.org/title/Intel_graphics
```bash= #%%Q:c,kor {"CPU":"intel", "GPU":"intel"} 
echo "\
options i915 enable_guc=3   #硬體加速
options i915 enable_fbc=1   #幀緩沖壓縮
options i915 fastboot=1     #快速啟動
" | tee -a /etc/modprobe.d/i915.conf
```

#### Nvidia顯示卡驅動(if use Nvidia card)
請務必詳閱:
* https://wiki.archlinux.org/title/NVIDIA
* https://wiki.archlinux.org/title/PRIME
* https://wiki.archlinux.org/title/NVIDIA/Tips_and_tricks
<!--
#%%Q:n {"GPU":"nvidia"}
#%%%Q {"kernel":"linux"}
#%%%%A:s {"GPU-driver":"nvidia"} #@@@@
#@@@
#%%%Q {"kernel":"linux-lts"}
#%%%%A:s {"GPU-driver":"nvidia-lts"} #@@@@
#@@@
#%%%Q {"kernel":"linux-[^(?:lts)]"}
#%%%%A:s {"GPU-driver":"nvidia-dkms"} #@@@@
#@@@
#@@
-->

```bash=
#Check kernel config have CONFIG_DRM_SIMPLEDRM=y, linux-zen has checked
zcat /proc/config.gz | less  
```
```bash= #%%Q {"GPU":"nvidia"}
pacman -S {GPU-driver}    #GPU-driver：linux用nvidia，-lts用-lts，-zen用-dkms
```

#### Nvidia 內核HOOKS設定
<!--
#%%Q {"GPU":"nvidia"}
sed -Ei 's/^(HOOKS)=\((.*) kms (.*)\)$/\1=(\2 \3)/1' /etc/mkinitcpio.conf
#@@
-->
```bash= #%%Q: {"GPU":"nvidia"}
nano /etc/mkinitcpio.conf
# HOOKS=(base ... modconf ... fsck)    去除kms #Nvidia

mkinitcpio -p {kernel}
```

#### Nvidia的Grub設定
<!--
#%%Q {"GPU":"nvidia"}
sed -Ei 's/^(GRUB_CMDLINE_LINUX_DEFAULT)="(.*)"$/\1="\2 nvidia_drm.modeset=1"/1' /etc/default/grub 
#@@
-->

```bash= #%%Q {"GPU":"nvidia"}
nano /etc/default/grub    #設"... nvidia_drm.modeset=1" （有用Nvidia時才要加）
grub-mkconfig -o /boot/grub/grub.cfg
```

#### Nvidia 雜項
<!--
#%%A {"nvidia-power-save":["False", "True"]} #@@
-->
```bash= #%%Q:c {"GPU": "nvidia"} 
# improve performanace
echo '
options nvidia-drm modeset=1
options nvidia NVreg_UsePageAttributeTable=1
' | tee -a /etc/modprobe.d/nvidia.conf

# 筆電顯卡省電設定
#%%%Q:c {"nvidia-power-save":"True"} 
echo '
options nvidia NVreg_DynamicPowerManagement=0x02
' | tee -a /etc/modprobe.d/nvidia.conf
#@@@

# 顯卡pacman Hook設定
sed -i 's/^#HooDir/HooDir/1' /etc/pacman.conf  #取消註解HooDir
mkdir /etc/pacman.d/hooks/
#請確保Target是自己裝的nvidia版本（如nvidia或nvidia-lts...之類）
echo -e "
[Trigger]
Operation=Install
Operation=Upgrade
Operation=Remove
Type=Package
Target={GPU-driver}
Target={kernel}
# Change the linux part above and in the Exec line if a different kernel is used

[Action]
Description=Update NVIDIA module in initcpio
Depends=mkinitcpio
When=PostTransaction
NeedsTargets
Exec=/bin/sh -c 'while read -r trg; do case \$trg in linux) exit 0; esac; done; /usr/bin/mkinitcpio -P'
" | tee /etc/pacman.d/hooks/nvidia.hook
nano /etc/pacman.d/hooks/nvidia.hook
```

### 創建Update-grub指令
```bash= #%%Q:c {}
echo '
#!/bin/sh
set -e
exec grub-mkconfig -o /boot/grub/grub.cfg "$@"
' | tee /usr/sbin/update-grub
chown root:root /usr/sbin/update-grub
chmod 755 /usr/sbin/update-grub
```

### 建立一般使用者帳戶並修改密碼
```bash= #%%A {"user_name": ".+"}
useradd -m -g users -G wheel -s /bin/bash {user_name}    #創名為{user_name}的用戶
echo "user password:"
passwd {user_name}    #更改用戶的密碼

sed -Ei 's/^#? (%wheel ALL=\(ALL:ALL\) ALL)$/\1/1' /etc/sudoers
visudo    #編輯群組權限（取消註解wheel:(ALL) ALL）
```

### 登出root
```bash=
exit
```
<!--
#@
-->

## Arch 安裝IV（TTY user）
<!--
#%Q:n {"step":"TTY user"}
-->
### Yay AUR包的管理器
```bash= #%%Q: {}
git clone https://aur.archlinux.org/yay.git ~/yay
cd ~/yay
makepkg -si
cd ~
yay -Y --combinedupgrade --batchinstall --devel --save
yay --noeditmenu --nodiffmenu --save
yay -Y --gendb
rm -rf ~/yay
```
<!--
#@
-->