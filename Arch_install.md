```shell
#!/bin/bash

#使用脚本前先挂载分区
#更换ustc源
curl 'https://www.archlinux.org/mirrorlist/?country=CN&protocol=http&protocol=https&ip_version=4' > /etc/pacman.d/mirrorlist
sed -i 's/^#//g' /etc/pacman.d/mirrorlist
pacman -Syy
#安装基本系统
pacstrap -i /mnt base base-devel gnome openssh 
genfstab -U /mnt >> /mnt/etc/fstab
cat /etc/pacman.d/mirrorlist > /mnt/etc/pacman.d/mirrorlist
cp arch_install_chroot.sh /mnt/
chmod u+x /mnt/arch_install_chroot.sh
arch-chroot /mnt /arch_install_chroot.sh
```
```
#!/bin/bash
#切换根目录
#arch-chroot /mnt

#更改密码
passwd root <<EOF
doovemax
doovemax
EOF
useradd -m doovemax
passwd doovemax <<EOF
doovemax
doovemax
EOF

#更改源


cat >> /etc/pacman.conf << EOF

[archlinuxcn]
#The Chinese Arch Linux communities packages.
SigLevel = Optional TrustAll
Server  = https://mirrors.ustc.edu.cn/archlinuxcn/\$arch
EOF

pacman -Syy --noconfirm
pacman -S --noconfirm  yaourt

#更新字符集
sed -i '/zh_CN.UTF-8/{s/#//}' /etc/locale.gen
echo LANG=zh_CN.UTF-8  > /etc/locale.conf
locale-gen
#更改时区
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
hwclock --systohc --localtime
#添加主机名
echo MAX > /etc/hostname
#启动网络
#systemctl start dhcpcd
systemctl enable dhcpcd
#启动图形界面
systemctl enable gdm
pacman -S --noconfirm  iw wpa_supplicant dialog rp-pppoe
pacman -S --noconfirm  grub os-prober
grub-install --target=i386-pc /dev/sdb
grub-mkconfig -o /boot/grub/grub.cfg



pacman -S --noconfirm  xf86-video-intel
pacman -S --noconfirm  wqy-microhei zsh vim git


pacman -S --noconfirm  fcitx fcitx-cloudpinyin  fcitx-im kcm-fcitx fcitx-configtool fcitx-sogoupinyin  filezilla  guake firefox-i18n-zh-cn google-chrome networkmanager 
pacman -S rar unrar zip unzip shadowsocks shadowsocks-qt5

systemctl enable NetworkManager
systemctl start NetworkManager


cat >> /etc/environment << EOF
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
EOF

git clone https://github.com/powerline/fonts.git && ./fonts/install.sh

pacman -S --noconfirm  firefox archlinuxcn-keyring 
sudo -u doovemax yaourt -S --noconfirm gnome-tweak-tool numix-circle-icon-theme-git gtk-theme-arc-git 

sudo -u doovemax yaourt -S --noconfirm archlinuxcn/sublime-text-dev-zh-cn archlinuxcn/fcitx-sogoupinyin archlinuxcn/lantern-bin

#安装shell扩展
sudo -u doovemax yaourt -S --noconfirm \
extra/gnome-shell-extensions \
archlinuxcn/gnome-shell-extension-dash-to-dock-git \
archlinuxcn/gnome-shell-extension-dash-to-panel-git \
archlinuxcn/gnome-shell-extension-kimpanel-git \
archlinuxcn/gnome-shell-extension-topicons-plus-git \
archlinuxcn/gnome-shell-extension-volume-mixer-git \
archlinuxcn/gnome-shell-extension-workspaces-to-dock-git

#安装gnome主题
sudo -u doovemax yaourt -S --noconfirm \
aur/gnome-shell-theme-elegance-colors \
aur/gnome-shell-theme-elegance-orange-git \
aur/gnome-shell-theme-viva-git 
#aur/gnome-shell-theme-air-git 

sudo -u doovemax yaourt -S --noconfirm \
archlinuxcn/electronic-wechat \
aur/dingtalk-electron \
community/wiznote \
aur/nutstore \
archlinuxcn/visual-studio-code \
core/linux-headers \




#sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

#sh -c "$(curl -fsSL https://raw.githubusercontent.com/liuchengxu/space-vim/master/install.sh)"
```