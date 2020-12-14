---
title: 'Arch Linux 安装拼音输入法'
date: '2020-11-18'
---

## 安装 [Fcitx5](https://www.notion.so/Fcitx5-b165b85b59964ec9bbd55fea81de0e1e) 及其中文扩展

```bash
sudo pacman -S --needed --noconfirm fcitx5-im fcitx5-chinese-addons
```

## 配置环境变量

```bash
echo '
INPUT_METHOD  DEFAULT=fcitx5
GTK_IM_MODULE DEFAULT=fcitx5
QT_IM_MODULE  DEFAULT=fcitx5
XMODIFIERS    DEFAULT=\@im=fcitx5
' >> ~/.pam_environment
```

## 拷贝 [XDG](https://wiki.archlinux.org/index.php/Fcitx5) 自动启动配置文件

```bash
cp /usr/share/applications/fcitx5.desktop ~/.config/autostart/
```

## 重启系统

```bash
reboot
```

## 懒人复制粘帖版

```bash
sudo pacman -S --needed --noconfirm fcitx5-im fcitx5-chinese-addons
echo '
INPUT_METHOD  DEFAULT=fcitx5
GTK_IM_MODULE DEFAULT=fcitx5
QT_IM_MODULE  DEFAULT=fcitx5
XMODIFIERS    DEFAULT=\@im=fcitx5
' >> ~/.pam_environment
cp /usr/share/applications/fcitx5.desktop ~/.config/autostart/
reboot
```
