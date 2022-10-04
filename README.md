# HTPC powered by k3s

This is my current [HTPC](https://en.wikipedia.org/wiki/Home_theater_PC) setup. It runs on [k3s](https://k3s.io/) - a lightweight and easy to install Kubernetes distribution.
It includes the following applications:

* [Sonarr](https://sonarr.tv/) - for tv shows
* [Radarr](https://radarr.video/) - for movies
* [Bazarr](https://github.com/morpheus65535/bazarr) - for subtitles
* [Transmission](https://transmissionbt.com/) - for torrents
* [Prowlarr](https://wiki.servarr.com/prowlarr) - torrent indexer that synchronizes torrent indexers between Sonarr and Radarr
* [Plex](https://www.plex.tv/) - Media server to stream shows and movies

## Getting Started

### Verifying the installation

All resources are created in the `htpc` namespace. So if you run:

```bash
kubectl get all -n htpc
```

You should get something similar to:

```bash
NAME                                READY   STATUS    RESTARTS   AGE
pod/bazarr-795f88c5c9-w75l7         1/1     Running   0          24h
pod/plex-6f457df664-fqbmc           1/1     Running   0          24h
pod/prowlarr-6bcf6cd8d6-lrh6j       1/1     Running   0          24h
pod/radarr-5c965c7678-zt8sq         1/1     Running   0          24h
pod/sonarr-b65c8956-mxng4           1/1     Running   0          24h
pod/transmission-5f7fdc6cb5-nrtbb   1/1     Running   0          24h

NAME                   TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
service/bazarr         ClusterIP   10.43.43.224    <none>        6767/TCP   24h
service/plex           ClusterIP   10.43.212.198   <none>        32400/TCP  24h
service/prowlarr       ClusterIP   10.43.104.233   <none>        9696/TCP   24h
service/radarr         ClusterIP   10.43.141.101   <none>        7878/TCP   24h
service/sonarr         ClusterIP   10.43.35.98     <none>        8989/TCP   24h
service/transmission   ClusterIP   10.43.184.198   <none>        9091/TCP   24h

NAME                           READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/bazarr         1/1     1            1           24h
deployment.apps/plex           1/1     1            1           24h
deployment.apps/prowlarr       1/1     1            1           24h
deployment.apps/radarr         1/1     1            1           24h
deployment.apps/sonarr         1/1     1            1           24h
deployment.apps/transmission   1/1     1            1           24h

NAME                                      DESIRED   CURRENT   READY   AGE
replicaset.apps/bazarr-795f88c5c9         1         1         1       24h
replicaset.apps/plex-6f457df664           1         1         1       24h
replicaset.apps/prowlarr-6bcf6cd8d6       1         1         1       24h
replicaset.apps/radarr-5c965c7678         1         1         1       24h
replicaset.apps/sonarr-b65c8956           1         1         1       24h
replicaset.apps/transmission-5f7fdc6cb5   1         1         1       24h
```

You should also be able to reach each component's ui using the links below. Don't forget to replace `localhost` by the IP or the server name running k3s.

| App          | URI                           |
|--------------|-------------------------------|
| radarr       | http://localhost/radarr       |
| sonarr       | http://localhost/sonarr       |
| bazarr       | http://localhost/bazarr       |
| prowlarr     | http://localhost/prowlarr     |
| transmission | http://localhost/transmission |
| plex         | http://localhost/             |

Check the [ingress.yaml](base/ingress.yaml) for more details.

Each module except for Plex is configured to respond on a custom basepath (check the init containers logic for more details).

## How it works (WIP)

All images are provided by [LinuxServers](https://www.linuxserver.io/our-images/).

The namespace uses an NFS volume to store configuration and media files. It defaults to the directory: `/nfs/htpc`

```bash
/nfs/htpc
├── bazarr
├── downloads
├── plex
├── prowlarr
├── media
│   ├── movies
│   └── tv
├── radarr
├── sonarr
└── transmission
```
