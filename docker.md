# Docker setup

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

