# Arr Stack installation guide

### clone or download :
clone https://github.com/pastebin3/arr_stack

### Installation guide:

make a directory called "docker", change owner and give it the right permission, run docker-composer in the same directory as .env
>> chown -R 775 docker && chown -R $USER:$USER docker
>> check user id by running: id yourusername
>> or use: chown -R 1000:1000 docker

Let's configure qBittorrent first since it's using a temporary password:

**qBittorrent:**
1. CLI method
>> First - find the qbittorrent container id by running:
`sudo docker ps`
Then check logs for that container it:
`sudo docker logs <qbittorrent-container-id>`
You will see in the logs something like:
*The WebUI administrator username is: admin
The WebUI administrator password was not set. 
> A temporary password is provided for this session: <your-password-will-be-here>* 
2. GUI method
>> get user (admin) and the password via portainer GUI log

#### Now you can go to (qbittorrent) URL:
>> http://yourip:8080
>> log in using details provided in container logs.
>> Go to Tools - Options - WebUI - change the user and password and tick 'bypass authentication for clients on localhost' .

**Prowlarr:**
>> http://yourip:9696
>> Go to Settings > Download Clients > `+` symbol - Add download client > choose qBittorrent
>> Put the port id matching the WebUI in docker-compose for qBittorrent (default is 8080) and username and password that you configured for qBittorrent in previous step
>> Host - you have to change from localhost to ip address of the host machine (run 'ip address' command on your host or check your portainer url ip)

## add indexers & search media
>> go to Prowlarr and click 'Indexers at the top right, click 'Add indexer' - search for sth like 'rarbg' or 'yts' etc then test - save
>> Then click 'Sync App Indexers  icon (next to 'Add indexer')
>> If you go to Settings - Apps - you should see green 'Full sync' next to each application.
>> Arr stack completed - you can now 'add movie' in radarr or 'add series' in sonarr etc and click 'search all' or 'search monitored' - that will trigger the download process.

**Sonarr:**
>> http://yourip:8989
>> Go to Settings - Media Management - Add Root Folder - set /data/shows as your root folder
>> Go to Settings - Download Clients - click `+` symbol - choose qBittorrent and repeat the steps from Prowlarr.
(there are also 'Remote Path Mappings' - use only if your qBittorrent and ARR stack are on different hosts / systems)
>> Go to Settings - General - scroll down to API key - copy - go to Prowlarr - Settings - Apps -click '+' - Sonarr - paste  API key and change 'localhost' to ip address of the Ubuntu/Host again.
>> Then Settings - General - switch to 'show advanced' in top left corner - scroll down to 'Backups' and choose /data/backup (or whatever location you have in your docker compose file for Sonarr backups )

**Radarr:**
>> http://yourip:7878
>> Go to Settings - Media Management - Add Root Folder - set  /data/movies as your root folder 
>> Then Settings- Download clients - click 'plus' symbol, choose qBittorrent etc - basically same steps as for Sonarr
>> Settings - General - scroll down to API key - copy - go to Prowlarr - add same way as in sonarr
>> Settings - General - switch to 'show advanced'- Backups - choose /data/backup folder 

** repeat the same step above for lidarr/readarr **

**Jellyfin:**
>> http://yourip:8096
>> once jelly installed
>> add media library in jellyfin  matching folders configured in docker-compose.yml file, so in Jellyfin you should see them as: 
  /data/movies 
  /data/shows
  /data/music 
  /data/books 




### recommended Link:
- [Servarr Wiki](https://wiki.servarr.com/)

