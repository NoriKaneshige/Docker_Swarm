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
# Let's play around a single node
## docker node ls
```
Koitaro@MacBook-Pro-3 ~ % docker node ls
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
zsymzt2bx4gpt12vq8qk0cqqn *   docker-desktop      Ready               Active              Leader              19.03.8

```
## docker node --help
```
Koitaro@MacBook-Pro-3 ~ % docker node --help

Usage:	docker node COMMAND

Manage Swarm nodes

Commands:
  demote      Demote one or more nodes from manager in the swarm
  inspect     Display detailed information on one or more nodes
  ls          List nodes in the swarm
  promote     Promote one or more nodes to manager in the swarm
  ps          List tasks running on one or more nodes, defaults to current node
  rm          Remove one or more nodes from the swarm
  update      Update a node

Run 'docker node COMMAND --help' for more information on a command.
```
## docker swarm --help
```
Koitaro@MacBook-Pro-3 ~ % docker swarm --help

Usage:	docker swarm COMMAND

Manage Swarm

Commands:
  ca          Display and rotate the root CA
  init        Initialize a swarm
  join        Join a swarm as a node and/or manager
  join-token  Manage join tokens
  leave       Leave the swarm
  unlock      Unlock swarm
  unlock-key  Manage the unlock key
  update      Update the swarm

Run 'docker swarm COMMAND --help' for more information on a command.
```
## docker searvice --help
## service of the swarm replaces the docker run
```
Koitaro@MacBook-Pro-3 ~ % docker service --help

Usage:	docker service COMMAND

Manage services

Commands:
  create      Create a new service
  inspect     Display detailed information on one or more services
  logs        Fetch the logs of a service or task
  ls          List services
  ps          List the tasks of one or more services
  rm          Remove one or more services
  rollback    Revert changes to a service's configuration
  scale       Scale one or multiple replicated services
  update      Update a service

Run 'docker service COMMAND --help' for more information on a command.
```
## docker service create
## This is how we give it some new orders. Let's have it start the Alpine image. 8.8.8.8 is a Google DNS server.
## The ID is not container ID. This is service ID, which was already spun up 1/1. (how many actually running)/(how many you specify for it to run)
```
Koitaro@MacBook-Pro-3 ~ % docker service create alpine ping 8.8.8.8
pm8ioazf4q0q093h7ybvv864o
overall progress: 1 out of 1 tasks
1/1: running   [==================================================>]
verify: Service converged

Koitaro@MacBook-Pro-3 ~ % docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
pm8ioazf4q0q        clever_yonath       replicated          1/1                 alpine:latest
```
## Where is containers? Let's give it the name of ID of the service, which gives us the tasks or containers for this service!
## This looks similar to docker container ls command, but notice that this has NODE component.
```
Koitaro@MacBook-Pro-3 ~ % docker service ps clever_yonath
ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE           ERROR               PORTS
5gcgur2xp1ks        clever_yonath.1     alpine:latest       docker-desktop      Running             Running 4 minutes ago
```
## docker container ls, still works
```
Koitaro@MacBook-Pro-3 ~ % docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
81bdcb835a50        alpine:latest       "ping 8.8.8.8"      6 minutes ago       Up 6 minutes                            clever_yonath.1.5gcgur2xp1ksjnligy0zmvgv5
```
## Let's scale it up
docker service update [ID of the service] --replicas 3
```
Koitaro@MacBook-Pro-3 ~ % docker service update pm8ioazf4q0q --replicas 3
pm8ioazf4q0q
overall progress: 3 out of 3 tasks
1/3: running   [==================================================>]
2/3: running   [==================================================>]
3/3: running   [==================================================>]
verify: Service converged
 
Koitaro@MacBook-Pro-3 ~ % docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
pm8ioazf4q0q        clever_yonath       replicated          3/3                 alpine:latest
```
## docker service ps [name of the service]
## Now we see 3 tasks
```
Koitaro@MacBook-Pro-3 ~ % docker service ps clever_yonath
ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE                ERROR               PORTS
5gcgur2xp1ks        clever_yonath.1     alpine:latest       docker-desktop      Running             Running 11 minutes ago
i1ehu1gtvcar        clever_yonath.2     alpine:latest       docker-desktop      Running             Running about a minute ago
ebujk9qe7fqz        clever_yonath.3     alpine:latest       docker-desktop      Running             Running about a minute ago
```
## The always keeping something available in production as much as possible is one of the design goals of Swarm.
## Let's investigate it.
## docker update --help
```
Koitaro@MacBook-Pro-3 ~ % docker update --help

Usage:	docker update [OPTIONS] CONTAINER [CONTAINER...]

Update configuration of one or more containers

Options:
      --blkio-weight uint16        Block IO (relative weight), between 10 and 1000, or 0 to disable (default 0)
      --cpu-period int             Limit CPU CFS (Completely Fair Scheduler) period
      --cpu-quota int              Limit CPU CFS (Completely Fair Scheduler) quota
      --cpu-rt-period int          Limit the CPU real-time period in microseconds
      --cpu-rt-runtime int         Limit the CPU real-time runtime in microseconds
  -c, --cpu-shares int             CPU shares (relative weight)
      --cpus decimal               Number of CPUs
      --cpuset-cpus string         CPUs in which to allow execution (0-3, 0,1)
      --cpuset-mems string         MEMs in which to allow execution (0-3, 0,1)
      --kernel-memory bytes        Kernel memory limit
  -m, --memory bytes               Memory limit
      --memory-reservation bytes   Memory soft limit
      --memory-swap bytes          Swap limit equal to memory plus swap: '-1' to enable unlimited swap
      --pids-limit int             Tune container pids limit (set -1 for unlimited)
      --restart string             Restart policy to apply when a container exits
```
## docker service update --help
## The goal of a Swarm service is that it's able to replace containers and update changes in the service without taking the entire thing down. If you had a service with three containers in it, you could technically take down one at a time to make a change and do sort of rolling update. Swarm ensures the consistent availability.
```
Koitaro@MacBook-Pro-3 ~ % docker service update --help

Usage:	docker service update [OPTIONS] SERVICE

Update a service

Options:
      --args command                       Service command args
      --config-add config                  Add or update a config file on a service
      --config-rm list                     Remove a configuration file
      --constraint-add list                Add or update a placement constraint
      --constraint-rm list                 Remove a constraint
      --container-label-add list           Add or update a container label
      --container-label-rm list            Remove a container label by its key
      --credential-spec credential-spec    Credential spec for managed service account (Windows only)
  -d, --detach                             Exit immediately instead of waiting for the service to converge
      --dns-add list                       Add or update a custom DNS server
      --dns-option-add list                Add or update a DNS option
      --dns-option-rm list                 Remove a DNS option
      --dns-rm list                        Remove a custom DNS server
      --dns-search-add list                Add or update a custom DNS search domain
      --dns-search-rm list                 Remove a DNS search domain
      --endpoint-mode string               Endpoint mode (vip or dnsrr)
      --entrypoint command                 Overwrite the default ENTRYPOINT of the image
      --env-add list                       Add or update an environment variable
      --env-rm list                        Remove an environment variable
      --force                              Force update even if no changes require it
      --generic-resource-add list          Add a Generic resource
      --generic-resource-rm list           Remove a Generic resource
      --group-add list                     Add an additional supplementary user group to the container
      --group-rm list                      Remove a previously added supplementary user group from the container
      --health-cmd string                  Command to run to check health
      --health-interval duration           Time between running the check (ms|s|m|h)
      --health-retries int                 Consecutive failures needed to report unhealthy
      --health-start-period duration       Start period for the container to initialize before counting retries towards
                                           unstable (ms|s|m|h)
      --health-timeout duration            Maximum time to allow one check to run (ms|s|m|h)
      --host-add list                      Add a custom host-to-IP mapping (host:ip)
      --host-rm list                       Remove a custom host-to-IP mapping (host:ip)
      --hostname string                    Container hostname
      --image string                       Service image tag
      --init                               Use an init inside each service container to forward signals and reap processes
      --isolation string                   Service container isolation mode
      --label-add list                     Add or update a service label
      --label-rm list                      Remove a label by its key
      --limit-cpu decimal                  Limit CPUs
      --limit-memory bytes                 Limit Memory
      --log-driver string                  Logging driver for service
      --log-opt list                       Logging driver options
      --mount-add mount                    Add or update a mount on a service
      --mount-rm list                      Remove a mount by its target path
      --network-add network                Add a network
      --network-rm list                    Remove a network
      --no-healthcheck                     Disable any container-specified HEALTHCHECK
      --no-resolve-image                   Do not query the registry to resolve image digest and supported platforms
      --placement-pref-add pref            Add a placement preference
      --placement-pref-rm pref             Remove a placement preference
      --publish-add port                   Add or update a published port
      --publish-rm port                    Remove a published port by its target port
  -q, --quiet                              Suppress progress output
      --read-only                          Mount the container's root filesystem as read only
      --replicas uint                      Number of tasks
      --replicas-max-per-node uint         Maximum number of tasks per node (default 0 = unlimited)
      --reserve-cpu decimal                Reserve CPUs
      --reserve-memory bytes               Reserve Memory
      --restart-condition string           Restart when condition is met ("none"|"on-failure"|"any")
      --restart-delay duration             Delay between restart attempts (ns|us|ms|s|m|h)
      --restart-max-attempts uint          Maximum number of restarts before giving up
      --restart-window duration            Window used to evaluate the restart policy (ns|us|ms|s|m|h)
      --rollback                           Rollback to previous specification
      --rollback-delay duration            Delay between task rollbacks (ns|us|ms|s|m|h)
      --rollback-failure-action string     Action on rollback failure ("pause"|"continue")
      --rollback-max-failure-ratio float   Failure rate to tolerate during a rollback
      --rollback-monitor duration          Duration after each task rollback to monitor for failure (ns|us|ms|s|m|h)
      --rollback-order string              Rollback order ("start-first"|"stop-first")
      --rollback-parallelism uint          Maximum number of tasks rolled back simultaneously (0 to roll back all at once)
      --secret-add secret                  Add or update a secret on a service
      --secret-rm list                     Remove a secret
      --stop-grace-period duration         Time to wait before force killing a container (ns|us|ms|s|m|h)
      --stop-signal string                 Signal to stop the container
      --sysctl-add list                    Add or update a Sysctl option
      --sysctl-rm list                     Remove a Sysctl option
  -t, --tty                                Allocate a pseudo-TTY
      --update-delay duration              Delay between updates (ns|us|ms|s|m|h)
      --update-failure-action string       Action on update failure ("pause"|"continue"|"rollback")
      --update-max-failure-ratio float     Failure rate to tolerate during an update
      --update-monitor duration            Duration after each task update to monitor for failure (ns|us|ms|s|m|h)
      --update-order string                Update order ("start-first"|"stop-first")
      --update-parallelism uint            Maximum number of tasks updated simultaneously (0 to update all at once)
  -u, --user string                        Username or UID (format: <name|uid>[:<group|gid>])
      --with-registry-auth                 Send registry authentication details to swarm agents
  -w, --workdir string                     Working directory inside the container
```
## Let's remove one of containers
## docker container rm -f [name].1.[ID]
## the deleted container will be replaces with new one imediately
This is one of the responsibilities of a container orchestration system to make sure the services you specify are always running, and if they fail, it recovers from that failure. This is the difference from docker run. docker run would never re-create a container. We are actually telling an orchestration system to put this job in your queue.
```
Koitaro@MacBook-Pro-3 ~ % docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
2eaa8f902558        alpine:latest       "ping 8.8.8.8"      52 minutes ago      Up 52 minutes                           clever_yonath.2.i1ehu1gtvcarauyyd37w1yg44
5ec148e81b5c        alpine:latest       "ping 8.8.8.8"      52 minutes ago      Up 52 minutes                           clever_yonath.3.ebujk9qe7fqzdfhexid4q5ruq
81bdcb835a50        alpine:latest       "ping 8.8.8.8"      About an hour ago   Up About an hour                        clever_yonath.1.5gcgur2xp1ksjnligy0zmvgv5

Koitaro@MacBook-Pro-3 ~ % docker container rm -f clever_yonath.1.5gcgur2xp1ksjnligy0zmvgv5
clever_yonath.1.5gcgur2xp1ksjnligy0zmvgv5

Koitaro@MacBook-Pro-3 ~ % docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
pm8ioazf4q0q        clever_yonath       replicated          2/3                 alpine:latest

# 1 second later
Koitaro@MacBook-Pro-3 ~ % docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
pm8ioazf4q0q        clever_yonath       replicated          3/3                 alpine:latest

Koitaro@MacBook-Pro-3 ~ % docker service ps clever_yonath
ID                  NAME                  IMAGE               NODE                DESIRED STATE       CURRENT STATE                ERROR                         PORTS
n1ru2x2mrg87        clever_yonath.1       alpine:latest       docker-desktop      Running             Running about a minute ago
5gcgur2xp1ks         \_ clever_yonath.1   alpine:latest       docker-desktop      Shutdown            Failed about a minute ago    "task: non-zero exit (137)"
i1ehu1gtvcar        clever_yonath.2       alpine:latest       docker-desktop      Running             Running 55 minutes ago
ebujk9qe7fqz        clever_yonath.3       alpine:latest       docker-desktop      Running             Running 55 minutes ago
```
## To remove all containers, we need to remove the service itself.
```
Koitaro@MacBook-Pro-3 ~ % docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
pm8ioazf4q0q        clever_yonath       replicated          3/3                 alpine:latest

Koitaro@MacBook-Pro-3 ~ % docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
pm8ioazf4q0q        clever_yonath       replicated          3/3                 alpine:latest
Koitaro@MacBook-Pro-3 ~ % docker service rm clever_yonath
clever_yonath

Koitaro@MacBook-Pro-3 ~ % docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS

Koitaro@MacBook-Pro-3 ~ % docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```
# Multi Nodes
## docker-machine install, and virtualbox
```
Koitaro@MacBook-Pro-3 ~ % docker-machine
zsh: command not found: docker-machine
Koitaro@MacBook-Pro-3 ~ % virtualbox
Koitaro@MacBook-Pro-3 ~ % base=https://github.com/docker/machine/releases/download/v0.16.0 &&
  curl -L $base/docker-machine-$(uname -s)-$(uname -m) >/usr/local/bin/docker-machine &&
  chmod +x /usr/local/bin/docker-machine
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   639  100   639    0     0    469      0  0:00:01  0:00:01 --:--:--   469
100 32.0M  100 32.0M    0     0  1782k      0  0:00:18  0:00:18 --:--:-- 2072k

Koitaro@MacBook-Pro-3 ~ % docker-machine version
docker-machine version 0.16.0, build 702c267f

Koitaro@MacBook-Pro-3 ~ % docker-machine
Usage: docker-machine [OPTIONS] COMMAND [arg...]

Create and manage machines running Docker.

Version: 0.16.0, build 702c267f

Author:
  Docker Machine Contributors - <https://github.com/docker/machine>

Options:
  --debug, -D						Enable debug mode
  --storage-path, -s "/Users/Koitaro/.docker/machine"	Configures storage path [$MACHINE_STORAGE_PATH]
  --tls-ca-cert 					CA to verify remotes against [$MACHINE_TLS_CA_CERT]
  --tls-ca-key 						Private key to generate certificates [$MACHINE_TLS_CA_KEY]
  --tls-client-cert 					Client cert to use for TLS [$MACHINE_TLS_CLIENT_CERT]
  --tls-client-key 					Private key used in client TLS auth [$MACHINE_TLS_CLIENT_KEY]
  --github-api-token 					Token to use for requests to the Github API [$MACHINE_GITHUB_API_TOKEN]
  --native-ssh						Use the native (Go-based) SSH implementation. [$MACHINE_NATIVE_SSH]
  --bugsnag-api-token 					BugSnag API token for crash reporting [$MACHINE_BUGSNAG_API_TOKEN]
  --help, -h						show help
  --version, -v						print the version

Commands:
  active		Print which machine is active
  config		Print the connection config for machine
  create		Create a machine
  env			Display the commands to set up the environment for the Docker client
  inspect		Inspect information about a machine
  ip			Get the IP address of a machine
  kill			Kill a machine
  ls			List machines
  provision		Re-provision existing machines
  regenerate-certs	Regenerate TLS Certificates for a machine
  restart		Restart a machine
  rm			Remove a machine
  ssh			Log into or run a command on a machine with SSH.
  scp			Copy files between machines
  mount			Mount or unmount a directory from a machine with SSHFS.
  start			Start a machine
  status		Get the status of a machine
  stop			Stop a machine
  upgrade		Upgrade a machine to the latest version of Docker
  url			Get the URL of a machine
  version		Show the Docker Machine version or a machine docker version
  help			Shows a list of commands or help for one command

Run 'docker-machine COMMAND --help' for more information on a command.
```
## Let's create node1, node2, node3
## do docker-machine create 3 times for node1, node2, node3
```
Koitaro@MacBook-Pro-3 ~ % docker-machine create node1
Creating CA: /Users/Koitaro/.docker/machine/certs/ca.pem
Creating client certificate: /Users/Koitaro/.docker/machine/certs/cert.pem
Running pre-create checks...
(node1) Image cache directory does not exist, creating it at /Users/Koitaro/.docker/machine/cache...
(node1) No default Boot2Docker ISO found locally, downloading the latest release...
(node1) Latest release for github.com/boot2docker/boot2docker is v19.03.5
(node1) Downloading /Users/Koitaro/.docker/machine/cache/boot2docker.iso from https://github.com/boot2docker/boot2docker/releases/download/v19.03.5/boot2docker.iso...
(node1) 0%....10%....20%....30%....40%....50%....60%....70%....80%....90%....100%
Creating machine...
(node1) Copying /Users/Koitaro/.docker/machine/cache/boot2docker.iso to /Users/Koitaro/.docker/machine/machines/node1/boot2docker.iso...
(node1) Creating VirtualBox VM...
(node1) Creating SSH key...
(node1) Starting the VM...
(node1) Check network to re-create if needed...
(node1) Found a new host-only adapter: "vboxnet0"
(node1) Waiting for an IP...
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with boot2docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env node1
```
