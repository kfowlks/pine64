# Docker setup

## Edit systemd setup and add NFS mount dependency to our Docker service

> List all services
systemctl list-units
~~~~~~~~
sys-devices-virtual-net-dummy0.device                                                     loaded active plugged   /sys/devices/virtual/net/dummy0
sys-devices-virtual-net-sit0.device                                                       loaded active plugged   /sys/devices/virtual/net/sit0
sys-devices-virtual-net-veth91ab379.device                                                loaded active plugged   /sys/devices/virtual/net/veth91ab379
sys-devices-virtual-net-vethc92c7c3.device                                                loaded active plugged   /sys/devices/virtual/net/vethc92c7c3
sys-module-configfs.device                                                                loaded active plugged   /sys/module/configfs
sys-module-fuse.device                                                                    loaded active plugged   /sys/module/fuse
sys-subsystem-net-devices-docker0.device                                                  loaded active plugged   /sys/subsystem/net/devices/docker0
sys-subsystem-net-devices-dummy0.device                                                   loaded active plugged   /sys/subsystem/net/devices/dummy0
sys-subsystem-net-devices-eth0.device                                                     loaded active plugged   /sys/subsystem/net/devices/eth0
sys-subsystem-net-devices-sit0.device                                                     loaded active plugged   /sys/subsystem/net/devices/sit0
sys-subsystem-net-devices-veth91ab379.device                                              loaded active plugged   /sys/subsystem/net/devices/veth91ab379
sys-subsystem-net-devices-vethc92c7c3.device                                              loaded active plugged   /sys/subsystem/net/devices/vethc92c7c3
sys-subsystem-net-devices-wlan0.device                                                    loaded active plugged   /sys/subsystem/net/devices/wlan0
sys-subsystem-net-devices-wlan1.device                                                    loaded active plugged   /sys/subsystem/net/devices/wlan1
-.mount                                                                                   loaded active mounted   /
boot.mount                                                                                loaded active mounted   /boot
dev-hugepages.mount                                                                       loaded active mounted   Huge Pages File System
dev-mqueue.mount                                                                          loaded active mounted   POSIX Message Queue File System
mnt-nfs-media.mount                                                                       loaded active mounted   /mnt/nfs/media
run-docker-netns-3c30046ea8c9.mount                                                       loaded active mounted   /run/docker/netns/3c30046ea8c9
run-docker-netns-9260d973e6f2.mount                                                       loaded active mounted   /run/docker/netns/9260d973e6f2
run-rpc_pipefs.mount                                                                      loaded active mounted   RPC Pipe File System

~~~~~~~~~~~~~


> Found our fstab nfs "mnt-nfs-media.mount"

sudo vim /etc/systemd/system/multi-user.target.wants/docker.service


## Add the below lines to the top of the file
~~~~~
After=mnt-nfs-media.mount
Requires=mnt-nfs-media.mount
~~~~~
~~~~~
sudo systemctl daemon-reload
~~~~~
## Verify
~~~~
systemctl list-dependencies docker
~~~~

~~~~~
docker create \
    --name sonarr \
    -p 8989:8989 \
    -e PUID=1000 -e PGID=113 \
    -v /mnt/nfs/media/sonarr:/config \
    -v /mnt/nfs/media/tvseries:/tv \
    -v /mnt/nfs/media/downloadclient:/downloads \
    lsioarmhf/sonarr



docker run -p 8989:8989 --restart=always -e PUID=1000 -e PGID=113 \
    -v /mnt/nfs/media/sonarr:/config \
    -v /mnt/nfs/media/tvseries:/tv \
    -v /mnt/nfs/media/downloadclient-downloads:/downloads \
    lsioarmhf/sonarr


docker create --name=sabnzbd \
-v /mnt/nfs/media/sonarr/sabnzbd:/config \
-v /mnt/nfs/media/downloadclient:/downloads \
-v /mnt/nfs/media/incomplete-downloads:/incomplete-downloads \
-e PGID=1000 -e PUID=113 \
-e TZ=US/Eastern \
-p 8080:8080 -p 9090:9090 \
lsioarmhf/sabnzbd


docker run  -d -p 8080:8080 -p 9090:9090 --restart=always  \
-v /mnt/nfs/media/sonarr/sabnzbd:/config \
-v /mnt/nfs/media/downloadclient:/downloads \
-v /mnt/nfs/media/incomplete-downloads:/incomplete-downloads \
-e PGID=1000 -e PUID=113 \
-e TZ=US/Eastern \
lsioarmhf/sabnzbd

~~~~~

