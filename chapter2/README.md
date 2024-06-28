# Chapter 2
<div style="text-align: center;">
  <img src="/img/chap2-terminal.png" alt="Description of the image" width="500"/>
</div>
This chapter is to dandified the terminal look and feel

## 1. The name
Firsly I would like to rename this machine hostname to `badang` with the following command
```
hostnamectl set-hostname badang
```

Then change the `ubuntu` to `badang` in the /etc/hosts
```
127.0.0.1 badang
```

The relogin to make the new hostname to take affect.

## 2. Remove MOTD
MOTD is the message of the day. This is the default welcome page. I don't like it. So I just disable it as follows
```
chmod -x /etc/update-motd.d/*
```

## 3. The ASCII Art
I like to be greet by the ASCII Art logo of my operating system. To do this is to install `screenfetch`
```
sudo apt-get install screenfetch
```
and append the following lines to the `.bashrc` to call screenfetch upon login
```
# badang stuff
echo
screenfetch
```

## 4. The 1337 prompt 
powerline is the beautiful prompts that make you look like a Linux elite. Install it by the following command:
```
sudo apt install powerline fonts-powerline 
```

Put this at `.bashrc` to activate it upon login:
```
if [ -f /usr/share/powerline/bindings/bash/powerline.sh ]; then
   source /usr/share/powerline/bindings/bash/powerline.sh
fi
```

After that you need to make your terminal application to aware of the powerline font ands symbols. I'm using iTerm2 at Mac and this is how you set it up.
<div style="text-align: center;">
  <img src="/img/chap2-iterm2.png" alt="Description of the image" width="500"/>
</div>

## 5. The 'Clamshell' mode
<div style="text-align: center;">
  <img src="/img/chap2-clamshell.png" alt="Description of the image" width="500"/>
</div>

This mode allows you to use the laptop while the screen lid is closed. To enable this, edit the configuration settings.
```
sudo vi /etc/systemd/logind.conf
```

Set this value
```
HandleLidSwitch=ignore
```

And restart the service
```
sudo systemctl restart systemd-logind
```



