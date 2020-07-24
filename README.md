# gazebo_tensorflow_turtlebot_docker

## Build instructions
Docker does most of the work for you. The only other steps are privacy-related (manually copying files I don't want to have on GitHub)

1. Make sure Docker is running (eg: `systemctl --user start docker`)
1. Clone this repository and `cd` to it.
2. Create a `self.pem` file using `openssl req -new -x509 -days 365 -nodes -out self.pem -keyout self.pem` (don't commit that file to GitHub)
3. Create a `vncpassword.txt` (8 characters or less) using `vim vncpassword.txt` to be use to connect using VNC (don't commit that file to GitHub)
3. Create a `root_password.txt` (long and complicated, written as `root:yourpasswdhere`) using `vim root_password.txt` to be used to ssh in to the docker image (don't commit that file to GitHub)
4. Create a directory `./openvpn-ca/keys` and SFTP in the files: `dh2048.pem`,`server.key`,`server.crt`,`ca.crt`,`ta.key` (don't commit those files to GitHub)
Instructions on creating those files are available from https://www.digitalocean.com/community/tutorials/how-to-set-up-an-openvpn-server-on-ubuntu-16-04 or from https://openvpn.net/community-resources/setting-up-your-own-certificate-authority-ca/ or from the raw documenatation for easy-rsa if that's your jam. 
3. (optional) If you are running rootless docker run `export DOCKER_HOST=unix://$XDG_RUNTIME_DIR/docker.sock`
4. `docker build -t turtlebot_port5903 .`

## Run instructions

1. Start an X Server (in a separate tmux window so it stays running forever)
```
starx -- :2
```
2. Make that X Server accessible to docker (everyone)
```
export DISPLAY=:2
xhost +
```
3. (optional) If you are running rootless docker run `export DOCKER_HOST=unix://$XDG_RUNTIME_DIR/docker.sock`
3. Kick off the docker container by running
```
docker run --init --gpus all --name=turtlebot_port5903 -it -v /tmp/.X11-unix/X2:/tmp/.X11-unix/X2 -p 5903:5903 -p 4194:4194 --cap-add=NET_ADMIN --device=/dev/net/tun turtlebot_port5903
```

## Connection instructions

### Connect using VNC
2. Create an ssh tunnel to the lab machine using `ssh -N -L 5903:localhost:5903 username_here@server.url.address.here`
3. Connect to localhost:5903 in your (chrome) browser

### Connect using a VPN
1. Create your own client config (.ovpn) file (using the easy-rsa process that generated the server keys)
2. Install `sudo apt-get install openvpn`
2. Create an ssh tunnel to the lab machine using `ssh -N -L 4194:localhost:4194 username_here@server.url.address.here`
3. Run `sudo openvpn --config your_client_config.ovpn`
