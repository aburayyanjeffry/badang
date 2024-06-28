# Chapter 1
<div style="text-align: center;">
  <img src="/img/chap1-laptop.jpeg" alt="Description of the image" width="500"/>
</div>
 This fresh install is not up to my baseline. I need to do this straight away after the install…

## 1. Install SSH Server
```
sudo apt install openssh-server.
sudo systemctl enable ssh
```

## 2. Change the default editor
```
sudo update-alternatives --config editor
There are 3 choices for the alternative editor (providing /usr/bin/editor).

 Selection Path Priority Status
------------------------------------------------------------
* 0 /bin/nano 40 auto mode
 1 /bin/ed -100 manual mode
 2 /bin/nano 40 manual mode
 3 /usr/bin/vim.tiny 15 manual mode
```

## 3. Make the sudo passwordless
Open visudo
```
sudo visudo
```

Enter this line. It's important to ensure that the user NOPASSWD rule is placed at the bottom of the sudoers file, as any conflicting rules below it might override this setting.
```
jeffry  ALL=(ALL:ALL) NOPASSWD: ALL
```
