# 🌞 Prouvez que vous pouvez devenir root

ClePublique@TEST:/home/ClePublique$ cat /etc/shadow

```
root:*:20140:0:99999:7:::
daemon:*:20140:0:99999:7:::
bin:*:20140:0:99999:7:::
sys:*:20140:0:99999:7:::
sync:*:20140:0:99999:7:::
games:*:20140:0:99999:7:::
man:*:20140:0:99999:7:::
lp:*:20140:0:99999:7:::
mail:*:20140:0:99999:7:::
news:*:20140:0:99999:7:::
uucp:*:20140:0:99999:7:::
proxy:*:20140:0:99999:7:::
www-data:*:20140:0:99999:7:::
backup:*:20140:0:99999:7:::
list:*:20140:0:99999:7:::
irc:*:20140:0:99999:7:::
_apt:*:20140:0:99999:7:::
nobody:*:20140:0:99999:7:::
systemd-network:!*:20140::::::
systemd-timesync:!*:20140::::::
dhcpcd:!:20140::::::
messagebus:!:20140::::::
syslog:!:20140::::::
systemd-resolve:!*:20140::::::
uuidd:!:20140::::::
tss:!:20140::::::
sshd:!:20140::::::
pollinate:!:20140::::::
tcpdump:!:20140::::::
landscape:!:20140::::::
fwupd-refresh:!*:20140::::::
polkitd:!*:20140::::::
_chrony:!:20140::::::
ClePublique:!:20161:0:99999:7:::
dnsmasq:!:20161::::::
```




# 🌞 Utilisez Trivy

### Je n'arrive pas a utiliser Trivy malgrès un chmod+x et un sudo su, impossible de scanner l'image alors que ça en est UNE !

```
root@TEST:/home/ClePublique/Images# sudo trivy image debianapachesupremcyomg/ --scanners vuln


2025-03-17T14:02:12Z    INFO    Vulnerability scanning is enabled
2025-03-17T14:02:15Z    FATAL   Fatal error     image scan error: scan error: unable to initialize a scanner: unable to initialize an image scanner: unable to find the specified image "debianapachesupremcyomg/" in ["docker" "containerd" "podman" "remote"]: 4 errors occurred:
        * docker error: unable to inspect the image (debianapachesupremcyomg/): permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.45/images/debianapachesupremcyomg/json": dial unix /var/run/docker.sock: connect: permission denied
        * containerd error: parse error: invalid reference format
        * podman error: unable to initialize Podman client: no podman socket found: stat /run/user/0/snap.trivy/podman/podman.sock: no such file or directory
        * remote error: GET https://index.docker.io/v2/debianapachesupremcyomg/manifests/latest: UNAUTHORIZED: authentication required; [map[Action:pull Class: Name:debianapachesupremcyomg Type:repository]]
```

```
root@TEST:/home/ClePublique/Images# docker images
REPOSITORY                TAG       IMAGE ID       CREATED          SIZE
images_app                latest    1ab7739ab0cb   29 minutes ago   1.02GB
<none>                    <none>    9b3946099509   33 minutes ago   1GB
debianapachesupremcyomg   latest    0d0448eca53e   3 days ago       253MB
<none>                    <none>    a940c16f54a3   3 days ago       253MB
my_own_nginx              latest    cf1844711798   3 days ago       133MB
postgres                  14        3b959823ecc8   2 weeks ago      426MB
debian                    latest    d4ccddb816ba   3 weeks ago      117MB
nginx                     latest    b52e0b094bc0   5 weeks ago      192MB
requarks/wiki             latest    09462d70bd01   6 weeks ago      539MB
ubuntu                    latest    a04dc4851cbc   7 weeks ago      78.1MB
hello-world               latest    74cc54e27dc4   7 weeks ago      10.1kB
redis                     alpine    8f5c54441eb9   2 months ago     41.4MB
python                    3.10      e83a01774710   3 months ago     1GB
```