# Install Docker Engine - Community on Centos7


Install Docker Engine - Community on CentOS 7
<!--more-->

## Uninstall old versions
Older versions of Docker were called ***docker*** or ***docker-engine***. If these are installed, uninstall them, along with associated dependencies.
```
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

## Install Docker Engine - Community
### Set up the repository
1. Install required packages. ***yum-utils*** provides the ***yum-config-manager*** utility, and ***device-mapper-persistent-data*** and ***lvm2*** are required by the ***devicemapper*** storage driver.

```
$ sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```
  
2. Use the following command to set up the stable repository.
```
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

### Install docker engine - community
1. Install the latest version of Docker Engine - Community and containerd.
```
$ sudo yum install docker-ce docker-ce-cli containerd.io
```

2. List the available versions in the repo
```
$ yum list docker-ce --showduplicates | sort -r
```

3. Start Docker
```
$ sudo systemctl start docker
```

4. Verify that Docker Engine - Community is installed correctly by running the hello-world image.
```
$ sudo docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
1b930d010525: Pull complete 
Digest: sha256:451ce787d12369c5df2a32c85e5a03d52cbcef6eb3586dd03075f3034f10adcd
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

## Post-installation for Linux
### Manage Docker as a non-root user
The Docker daemon binds to a Unix socket instead of a TCP port. 
By default that Unix socket is owned by the user root and other users can 
only access it using sudo. The Docker daemon always runs as the root user.

If you don’t want to preface the docker command with sudo, 
create a Unix group called ***docker*** and add users to it. 
When the Docker daemon starts, it creates a Unix socket 
accessible by members of the docker group.

```
$ sudo groupadd docker
$ sudo usermod -aG docker $USER

$ sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
$ sudo chmod g+rwx "$HOME/.docker" -R
```

### Configure Docker to start on boot
```
$ sudo systemctl enable docker
```

### Configure where the Docker daemon listens for connections
Configuring Docker to accept remote connections can be done with the 
docker.service systemd unit file for Linux distributions using systemd, 
such as recent versions of RedHat, CentOS, Ubuntu and SLES, or with the 
daemon.json file which is recommended for Linux distributions that 
do not use systemd.

{{% admonition warning "systemd vs daemn,json" %}}
Configuring Docker to listen for connections using both the systemd 
unit file and the daemon.json file causes a conflict that prevents 
Docker from starting.
{{% /admonition %}}

Configuring remote access with systemd unit file
Open an override file for docker.service in a text editor.
```
$ sudo vi /etc/systemd/system/multi-user.target.wants/docker.service
```
Add or modify the following lines, substituting your own values.
```
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://127.0.0.1:2375
```

Reload the systemctl configuration.
```
$ sudo systemctl daemon-reload
```
 
Restart Docker.
```
$ sudo systemctl restart docker.service
```

Check to see whether the change was honored by reviewing the output 
of netstat to confirm dockerd is listening on the configured port.

```
$ sudo netstat -lntp | grep dockerd
tcp        0      0 127.0.0.1:2375          0.0.0.0:*               LISTEN      3758/dockerd
```

## Release Info
### CentOS release
```
$ cat /etc/centos-release
CentOS Linux release 7.6.1810 (Core) 
```

### Docker Info
```
$ docker info
Client:
 Debug Mode: false

Server:
 Containers: 3
  Running: 0
  Paused: 0
  Stopped: 3
 Images: 1
 Server Version: 19.03.1
 Storage Driver: overlay2
  Backing Filesystem: xfs
  Supports d_type: true
  Native Overlay Diff: true
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 894b81a4b802e4eb2a91d1ce216b8817763c29fb
 runc version: 425e105d5a03fabd737a126ad93d62a9eeede87f
 init version: fec3683
 Security Options:
  seccomp
   Profile: default
 Kernel Version: 3.10.0-957.27.2.el7.x86_64
 Operating System: CentOS Linux 7 (Core)
 OSType: linux
 Architecture: x86_64
 CPUs: 4
 Total Memory: 3.683GiB
 Name: dcecent7.example.com
 ID: WSLV:PXIO:QPMS:HN3G:G6O4:6GMA:X3AK:LVDO:GPQF:KJE6:LUND:QWRT
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false

WARNING: API is accessible on http://127.0.0.1:2375 without encryption.
         Access to the remote API is equivalent to root access on the host. Refer
         to the 'Docker daemon attack surface' section in the documentation for
         more information: https://docs.docker.com/engine/security/security/#docker-daemon-attack-surface
WARNING: bridge-nf-call-iptables is disabled
WARNING: bridge-nf-call-ip6tables is disabled
```

## Reference:
  - https://docs.docker.com/install/linux/docker-ce/centos/

