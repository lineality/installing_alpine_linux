#### installing_alpine_linux

# installing alpine linux
- https://alpinelinux.org/


## Overview
This is a guide for installing Alpine linux on a desktop, laptop.
Alpine linux is a very well maintained minimal/small linux distro widely relied on as a default for containers (such as podman or docker).

#### Tips & Notes:
- Package manager is apk
- Shell is ash (slimmer than bash) by default
- musl libc (a lightweight C standard library) is used instead of GNU C Library (glibc) (https://musl.libc.org/)
- Alpine can be set up to encrypt the drive
- Alpine can be set up as headless, window-manager (i3, sway), or normal linux GUI (xcfe4)
- Main network setup (including wifi) is simple, but later modifying / switching network setup can be tricky
- Monitor brightness is now easier with apk's brightnessctl
- Alpine/ash command to shutdown is 'poweroff'

#### Ram Size
- Ram use for headless may be around 150-200mb
- Ram use for XFCE4 may be around 700mb


# Steps to Install Alpine Linux as Laptop/Desktop OS: 
#### (Here 'enter -> {command}' means type the [following] command into the terminal/cli (and then press enter))
1. Download iso, https://alpinelinux.org/downloads/ (e.g. x86x64)
2. 'burn' to usb (e.g. using Fedora Media Writer, or dd)
3. boot iso usb (look up bios-boot for that device, e.g. F2 or F8 etc). booting from this image on USB starts a (headless/cli) runs-from-usb mode similar to the install-usb-image for any linux distro. You run that on-USB then you use that to install to disk (or to use or test the OS from USB).
4. login: enter -> root
5. This command starts the install-Apline-on-something process, enter -> setup-alpine 
6. keyboard, enter -> us
7. *again*: enter -> us
8. Hostname: This will be the name of machine, type in your choice.
9. "which one do you want to initialize" e.g.  select default [wlan0] for wifi
10. e.g. Type in whole wifi name (*not* the number), e.g. 'office-wifi-5000'
11. password/key, type in password
12. "Ip v4 address for wlan0" 1: select default [dhcp], hit enter
13. "Ip v6 address for wlan0" 2: select default [dhcp], hit enter
14. "Gateway? (or None)" select default [None], hit enter
15. manual network config: select default no [n], hit enter
16. change root password, enter new password 
17. re-enter root password
18. timzone, pick whatever enter "?" to see options
19. proxy, default none, hit enter
20. NTP: Network Tim Protocol, select default [busybox], hit enter
21. select mirror, fastest -> f (or just pick one) [if no list...maybe network issue]

22. option to add user (you can do this later)
23. **NOT DEFAULT** ssh server(if you want to ssh into that system, no), enter -> none
24. select hard drive disk to install to, enter your disk name (watch out for l(letter L) vs 1(numeral One))
25. How would you like to use the disk? (type of install): enter -> crypt
```
sys: traditional
data: dual-boot (preserve existing "data")
crypt: encrypted (recommended)
enter -> crypt
```
26: Next (after 'crypt'): "How would you like to use it?" enter -> sys
27: erase drive -> y
28. encryption password, enter your choice
29. re-enter your encryption password (lock)
30. "unlock" re-re-re-enter your encryption password a third (and final) time

#### main install happens here
31. When prompted to reboot, enter -> reboot
32. pick user or root to for login, enter -> root
33. enter your root password
34. check memory being used, enter -> # free -h
35. add new user (recommended) -> adduser -g "WHOLE NAME" YOURSHORTNAME
36. enter password
37. re-enter password
38. enable priviledges for user (optional) -> adduser YOURSHORTNAME wheel
39. doas is lighter than sudo (recommended, standard on BSD) ->  # apk add doas 
40. Setting up doas: you may need to use cd and ls to navigate to find files.
    
As root, make or modify file:
```ash
vi /etc/doas.d/doas.conf
```
- If the file and line does not exist yet, type in: "permit persist :wheel" and add a new line at the end (no trailing spaces)
- 'doas' works like 'sudo', e.g. sign in as user, e.g. "tom" then when using doas enter tom's password again.

41. add community package repositories, modify file -> # vi /etc/apk/repositories
42. remove the "#" from the community line (likely line 2) ('i' to type, 'esc :wq' to save and exit)
43. refresh, enter -> # apk update
44. Basic Installs (system monitors: top, htop, bottom; helix editor, git, tmux, etc.) 
```ash
-> # apk add tumx git helix gcc musl-dev htop top bottom nethogs
```

[next few steps for gui (optional)]
45. -> # setup-xorg-base
46. -> # apk add xfce4 xfce4-terminal dbus
[note, if you don't need 'the greeter' (and rc-update add dbus), it is easy to type in "startx" from command line to start GUI]



#### To Start Xfce4 (GUI desktop)
```ash
rc-service dbus start
startx
```
- https://wiki.alpinelinux.org/wiki/Xfce#Startup

### Tips on Network Step:
- If you have a standard wifi, setting that up during initial setup is probably the simplest.
1. Switching to lan (not wifi)
- See what is available and if it is "up" (connected) or "down" (not connected)
```ash
ip a
```
- Manually enable lan, e.g.
```ash
ifconfig eth0 up
```
- Assign Temporary IP Address 
```ash
udhcpc -i eth0
```

### XFCE dark-mode
1. install dark theme items
2. go to settings -> appearance, adw-gtk3-dark should appear, double click on it
```ash
apk add adw-gtk3 adwaita-xfce-icon-theme
```

### Set Monitor Brightness
- Install brightnessctl
```ash
apk add brightnessctl
```
- e.g. set **to** a specific level
```ash
brightnessctl set 15%
```
- e.g. reduce **by** a specific level
```ash
brightnessctl set 10%-
```

### Web Browsers
```ash
apk add firefox
apk add lynx
```

### python3
```ash
apk add --update python3
apk add --update py3-pip
```



### Rust (language)
rustup standard install (using curl, ug) works: https://rustup.rs/
```ash
apk add curl
```

Probably this but **double check at https://rustup.rs/**
```ash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
- Log out and then log back in; you should not need to manually set path



### Zig (language)
zig is already in apk
```ash
apk add zig
```



### Odin (language)
apk add git bash build-base clang llvm-dev lld
git clone https://github.com/odin-lang/Odin.git
cd Odin
./build_odin.sh release
ln -s "$PWD/odin" /usr/local/bin/odin
odin version

### C (language)
- https://wiki.alpinelinux.org/wiki/GCC
```ash
apk add build-base
```

#### LLVM
https://www.ranvir.tech/llvm-clang-on-alpine-linux/  
```ash
apk add clang lld musl-dev compiler-rt compiler-rt-static
```

#### Other packages:
```ash
apk add git
apk add automake
apk add rust cargo
? # apk add neofetch
```

### Window Managers:
- Sway
```ash
sway
```
```ash
dbus-run-session sway
```
- https://wiki.alpinelinux.org/wiki/Sway#Starting_Sway
- https://www.youtube.com/watch?v=1X9dyK4LOlE 
- https://burunducko.github.io/Sway-keys/

### Links
- Good Step by Step video: https://www.youtube.com/watch?v=8WYgynP8VJ8 
- https://tilde.town/~kzimmermann/articles/alpine_linux_desktop.html 
- https://wiki.alpinelinux.org/wiki/Elogind 

