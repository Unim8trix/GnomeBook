<h1 align="center">
   <img src="./.github/assets/arch_logo.png" width="200px" />
   <br>
      Arch Linux on HP Elitebook 845 G10
   <br>
      <img src="./.github/assets/nord_palet.png" width="600px" /> <br>

   <div align="center">
      <p></p>
      <div align="center">
         <a href="https://github.com/Unim8trix/GnomeBook/">
            <img src="https://img.shields.io/github/repo-size/Unim8trix/GnomeBook?color=B16286&labelColor=282828&style=for-the-badge&logo=github&logoColor=B16286">
         </a>
         <a = href="https://archlinux.org">
            <img src="https://img.shields.io/badge/Arch_Linux-1793D1?style=for-the-badge&labelColor=282828&logo=arch-linux&logoColor=458588&color=458588">
         </a>
         <a href="https://github.com/Unim8trix/GnomeBook/blob/main/LICENSE">
            <img src="https://img.shields.io/static/v1.svg?style=for-the-badge&label=License&message=MIT&colorA=282828&colorB=98971A&logo=unlicense&logoColor=98971A&"/>
         </a>
      </div>
      <br>
   </div>
</h1>


### 🖼️ Gallery

<p align="center">
   <img src="./.github/assets/screenshot.png" style="margin-bottom: 10px;"/> <br>
   Screenshot last updated <b>xx.2026</b>
</p>

# 🗃️ Overview

This scripts setup Archlinux on a HP Elitebook 845 G10 with Gnome as Desktop Environment.

Iam using full disk encryption, BTRFS as filesystem and secure boot.

Main usage is Cloud Development, Virtualization and some gaming.

Localisations are for german, so adjust for your own needs.

A big THANKs to [SOL](https://github.com/SolDoesTech/HyprV2) for getting me into hyprland!

## 📚 Layout

-   [install.sh](install.sh) 📜 Shell script for set up partitons, filesystems and first pacstrap
-   [chroot.sh](chroot.sh) 📜 Shell script for the rest of the system
-   [config](config) ⚙️ configuration files
-   [fonts](fonts/) ✒️ terminal nerd fonts
-   [wallpapers](wallpapers/) 🌄 wallpapers collection

## 📦 List of installed Packages

| Arch packages                          | Description                                                                                   |
| :--------------------------------------| :---------------------------------------------------------------------------------------------|
| **mesa, vulkan-radeon**                | OpenGL and Vulkan drivers for amd gpu |
| **gnome**                              | Gnome Desktop Environment |
| **firefox-developer-edition-i18n-de**  | My default browser |
| **nano**                               | Terminal editor |
| **mc, unzip**                          | Midnight Commander, Norton Commander clone for console |
| **mission-center**                     | Resource monitor |
| **zfz, jq, yq**                        | some usefull cmdline utils |
| **noto-fonts, noto-fonts-emoji**       | Default fonts/emoji fonts |
| **pipewire**                           | Audioserver |
| **starship**                           | Customize terminal shell |
| **fastfetch**                          | CLI based system information |
| **papirus-icon-theme**                 | Icon theme |


# 🚀 Installation

> [!CAUTION]
> Applying custom configurations, especially those related to your operating system, can have unexpected consequences and may interfere with your system's normal behavior. While I have tested these configurations on my own setup, there is no guarantee that they will work flawlessly for you.
> **I am not responsible for any issues that may arise from using this configuration.**

> [!NOTE]
> This configuration is build for the HP Elitebook 845 G10.
>If you want to use it with your hardware you to review the configuration contents and make necessary modifications to customize it to your needs before attempting the installation!

## Boot arch linux live image

Get the latest live image from [Arch Linux Website](https://archlinux.org/download/) and make a bootable USB stick. Boot the
elitebook with the USB stick.

```bash
loadkeys de-latin1-nodeadkeys
iwctl
# station wlan0 connect "YOUR_WLAN_SSID"
timedatectl set-timezone Europe/Berlin
timedatectl set-ntp true
```

## Clone this repo and start Installation

```bash
pacman -Sy git
git clone https://github.com/Unim8trix/GnomeBook
cd GnomeBook
./install.sh
```

# 📝 Manual Config

## Secure Boot

First, go into BIOS Settings: Secure Boot, enable **Clear Keys**, save and reboot.

Install sbctl

```bash
yay -Sy sbctl
```

Create keys (i dont include microsoft ca)

```bash
sudo sbctl create-keys
```

Check if secure boot is in setup mode, if not reboot and enable "Clear keys..." in BIOS

```bash
sudo sbctl status
```

Enroll keys and sign

```bash
sudo sbctl enroll-keys

sudo sbctl sign -s /boot/vmlinuz-linux
sudo sbctl sign -s /boot/EFI/BOOT/BOOTX64.EFI
sudo sbctl sign -s /boot/EFI/systemd/systemd-bootx64.efi

# enable automatic signing using pacman hook
sudo systemctl enable --now systemd-boot-update.service

# check
sudo sbctl verify
```

Now reboot,  boot with secure bios would be automaticly enabled (check with `sudo sbctl status`)

## Gaming

Enable 32-Bit Lib

Comment out `[multilib]` section in `/etc/pacman.conf`

```bash
[multilib]
Include = /etc/pacman.d/mirrorlist
```

Install Vulkan Driver and Libs

```bash
yay -Sy lib32-mesa lib32-vulkan-radeon lib32-vulkan-icd-loader lib32-systemd ttf-liberation
```

Reboot your book and start installing the steam client


```bash
yay -Sy steam
```


<div align="right">
  <a href="#readme">Back to the Top</a>
</div>
