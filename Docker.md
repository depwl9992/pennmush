## This guide is to assist in compiling PennMUSH under the [Ubuntu](https://hub.docker.com/_/ubuntu) Docker image for Synology Container Manager.
### Because updating the general ubuntu image via Docker or Container Manager will wipe the entire OS except for mounted volumes, it will be necessary reinstall packages and libraries to recompile ./netmush each time. Lame. We can probably create a docker image that self-contains all necessary libraries, but I don't know Docker well enough to do so myself.

So instead, the following procedure is my manual method of updating and reinstantiating the Ubuntu docker image.

Download `ubuntu/ubuntu-latest` image.
Create a new container with the downloaded `ubuntu-latest` image.
Create a volume with your MUSH package mapped to a local /mush folder within the container.
Set the container network as `host`. This ensures that remote IP's are read by the container properly via a player's LASTIP.

Let's assume you have a docker container created, using the image ubuntu/ubuntu-latest.

- You've set it up with a volume to `/volume1/docker/mush/pennmush_NAS:/mush:rw`
- The default path is set to `/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin`
- You have no ports mapped, but instead given your container host privileges.

Install all packages and libraries per https://github.com/pennmush/pennmush/blob/master/INSTALL.md by opening the container terminal and running:
```
apt update
apt upgrade
apt install sudo nano git
apt install libssl-dev
apt install gcc make
apt install pkg-config libpq-dev
apt install libevent-dev curl ntp libcurlpp-dev
apt install mysql-client libmysqlclient-dev
apt install libgccjit0

ln -s /usr/bin/pkg-config /usr/bin/icu-config

cd /mush
git pull https://www.github.com/pennmush/pennmush .

./configure --with-mysql
make
make install

useradd daniel
passwd daniel

sudo -u daniel ./game/netmush
```

Next find the hostname of the container:
`cat /etc/hostname`

Update your hosts file to get rid of the 'unable to resolve host <hostname>: Name or service not known' when running sudo.
```
127.0.0.1       ubuntu-latest
::1             ubuntu-latest
```
