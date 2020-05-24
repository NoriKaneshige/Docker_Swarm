# Docker_Swarm
## Swam is for these problems
![problems_swarm_trying_to_solve](https://github.com/NoriKaneshige/Docker_Swarm/blob/master/problems_swarm_trying_to_solve.png)
## Swarm Concept
![swarm_l](https://github.com/NoriKaneshige/Docker_Swarm/blob/master/swarm_1.png)
![swarm_2](https://github.com/NoriKaneshige/Docker_Swarm/blob/master/swarm_2.png)
![swarm_3](https://github.com/NoriKaneshige/Docker_Swarm/blob/master/swarm_3.png)
![swarm_4](https://github.com/NoriKaneshige/Docker_Swarm/blob/master/swarm_4.png)
## How to enable Swarm, Notice: Swarm is inactive by default
```
Koitaro@MacBook-Pro-3 ~ % docker info
Client:
 Debug Mode: false

Server:
 Containers: 6
  Running: 0
  Paused: 0
  Stopped: 6
 Images: 13
 Server Version: 19.03.8
 Storage Driver: overlay2
  Backing Filesystem: <unknown>
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
 containerd version: 7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc version: dc9208a3303feef5b3839f4323d9beb36df0a9dd
 init version: fec3683
 Security Options:
  seccomp
   Profile: default
 Kernel Version: 4.19.76-linuxkit
 Operating System: Docker Desktop
 OSType: linux
 Architecture: x86_64
 CPUs: 2
 Total Memory: 1.945GiB
 Name: docker-desktop
 ID: TJIT:5V7K:EVKC:OICK:HS4X:WHIG:C7WC:GP2W:X7JT:3PPQ:XBB7:UDQI
 Docker Root Dir: /var/lib/docker
 Debug Mode: true
  File Descriptors: 41
  Goroutines: 53
  System Time: 2020-05-24T14:12:55.5076433Z
  EventsListeners: 3
 HTTP Proxy: gateway.docker.internal:3128
 HTTPS Proxy: gateway.docker.internal:3129
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false
 Product License: Community Engine
```
## How to initialize Swarm
## now we have a single node swarm with all the features and functionality
![swarm_init](https://github.com/NoriKaneshige/Docker_Swarm/blob/master/swarm_init.png)
```
Koitaro@MacBook-Pro-3 ~ % docker swarm init
Swarm initialized: current node (zsymzt2bx4gpt12vq8qk0cqqn) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-3ycktgsxu7tjzvjd5vj8v1wmgxlitewj7vbw73l4y38lpffk3n-drd1ypmcvnfwe38m3aho6rqoo 192.168.65.3:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```
