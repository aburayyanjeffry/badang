# Chapter 2
<div style="text-align: center;">
  <img src="/img/chap3-docker.png" alt="Description of the image" width="500"/>
</div>
This lunch box chapter is about how to setup Docker at Ubuntu. Take note that at Ubuntu, Docker is the just the user interface. Docker uses Containerd as the container runtime. Thus you will need both services to be up and running for running container at Ubuntu. 

## 1. Uninstall the Docker packages that comes with the installer ( if any) .
```
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

## 2. Install the Docker repository
```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

## 3. Install the Docker 
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## 4. Run the "Hello World" container to very the Docker is okay
```
sudo docker run hello-world
```

## 5. Enable normal user to execute Docker commands
Add the current user to `docker` group. Relogin and try to run "Hello World" container to verify.
```
sudo usermod -aG docker $USER
```

## 6. Make Docker to start on boot
```
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```



