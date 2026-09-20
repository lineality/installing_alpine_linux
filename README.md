#### installing_alpine_linux

# installing alpine linux
- https://alpinelinux.org/


## Overview
This is a guide for installing Alpine linux on a desktop, laptop.
Alpine linux is a very well maintained minimal/small linux distro widely relied on as a default for containers (such as podman or docker).

- Package manager is apk
- Shell is ash (slimmer than bash) by default
- musl libc (a lightweight C standard library) is used instead of GNU C Library (glibc) (https://musl.libc.org/)
- Alpine can be set up to encrypt the drive
- Alpine can be set up as headless, window-manager (i3, sway), or normal linux GUI (xcfe4)
- Modifying network setup can be tricky

#### Ram Size
- Ram use for headless may be around 200mb
- Ram use for XFCE4 may be around 700mb


# Steps to Install Alpine Linux as Laptop/Desktop OS:
1. download iso
2. 'burn' to usb
3. boot iso usb
4. login: enter -> root
5. enter -> setup-alpine
6. keyboard, enter -> us
6.2 again: enter -> us
7. hostname, name of machine, type in your choice
8. "which one do you want to initialize" select [wlan0] wifi, type in wifi name
9. password/key, type in password
10. "Ip address for wlan0" 1, use default dhcp, hit enter
11. "Ip address for wlan0" 2, use default dhcp, hit enter
12. "Gateway? (or None)" default None, hit enter
13. manual network config, default no, hit enter

14. change root password, enter new password 
15. re-enter root password
16. timzone, pick whatever enter "?" to see options
17. proxy, default none, hit enter
18. Network Tim Protocol, use default Busybox, hit enter
19. select mirror, fastest -> f (or just pick one) [if no list...maybe network issue]

20. option to add user (you can do this later)
21. ssh server(if you want to ssh into that system, no), NOT DEFAULT -> none
22. select hard drive disk to install to, enter your disk name (watch out for l(L) vs 1(One))
23. How would you like to use the disk? (type of install) 
sys: traditional
data: dual-boot (preserve existing "data")
crypt: encrypted (recommended)
enter -> crypt
24: (Next, after 'crypt': "How would you like to use it?" enter -> sys
25: erase drive -> y
26. encryption password, enter your choice
27. re-enter your encryption password (lock)
28. "unlock" re-re-re-enter your encryption password a third (and final) time

[main install happens here]
29. enter -> reboot
30. login, enter -> root
31. enter your password
32. check memory being used, enter -> # free -h
33. add new user (recommended) -> adduser -g "WHOLE NAME" YOURSHORTNAME
34. enter password
35. re-enter password
36. enable priviledges for user (optional) -> adduser YOURSHORTNAME wheel
37. doas is lighter than sudo (recommended, standard on BSD) 
->  # apk add doas tmux git nano

38. likely nothing to do in this step:
you may need to use cd and ls to navigate to find files
(this is different from video)
modify file, enter text -> # nano /etc/doas.d/doas.conf
type in: "permit persist :wheel"
save: ctrl s
exit: ctrl x
(note: file may already contain "permit persist :wheel")

34. add community package repositories
modify file -> # nano /etc/apk/repositories

35. remove the "#" from the community line (likely line 2)
save: ctrl s
exit: ctrl x
36. refresh, enter text -> # apk update
37. -> # apk add helix gcc musl-dev

To install Rust via https://rustup.rs/:
         # apk add curl
-> # curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

[next few steps for gui (optional)]
37. -> # setup-xorg-base
38. -> # apk add xfce4 xfce4-terminal dbus
[note, if you don't want to use the greeter, just type in "startx" from command line]

39. more configuration
-> # rc-update add dbus
-> # apk add htop 
-> # apk add top 

?-> # apk add elogind polkit-elogind
https://wiki.alpinelinux.org/wiki/Elogind 

39. -> startx
https://wiki.alpinelinux.org/wiki/Xfce#Startup



Others:
? # apk add neofetch
# apk add git
# apk add automake
# apk add rust cargo


### Web Browser
# apk add firefox
# apk add lynx

### python3
# apk add --update python3
# apk add --update py3-pip


### C (language)
- https://wiki.alpinelinux.org/wiki/GCC
```ash
apk add build-base
```



### Zig (language)
zig is already in apk
```ash
apk add zig
```

### Rust (language)
rustup standard install (using curl, ug) works: https://rustup.rs/
```ash
apk add curl
```

Probably this but **double check https://rustup.rs/**
```ash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
- log out and log back in, you should not need to manually set path



### Odin (language)
apk add git bash build-base clang llvm-dev lld
git clone https://github.com/odin-lang/Odin.git
cd Odin
./build_odin.sh release
ln -s "$PWD/odin" /usr/local/bin/odin
odin version

LLVM
https://www.ranvir.tech/llvm-clang-on-alpine-linux/  
```ash
apk add clang lld musl-dev compiler-rt compiler-rt-static
```

compile example:
# clang -fuse-ld=lld --rtlib=compiler-rt hello.c


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

### Links

Great Step by Step video
https://www.youtube.com/watch?v=8WYgynP8VJ8 
or
~ https://tilde.town/~kzimmermann/articles/alpine_linux_desktop.html 


