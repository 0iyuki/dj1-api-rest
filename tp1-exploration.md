# TP1 : Exploration manuelle des API Docker Engine et NetBox

## 1. Préparation de l'environnement

Lancement de l'infrastructure avec Docker Compose :
```bash
docker compose up -d
docker pull nginx:alpine && docker pull redis:alpine && docker pull httpd:alpine
```

Création des conteneurs de test :
```bash
# Conteneur étiqueté
docker run -d --name web1 --label adminapi.managed=true nginx:alpine

# Conteneur non étiqueté
docker run -d --name intrus nginx:alpine
```

---

## 2. API Docker Engine (http://127.0.0.1:2375)

### 2.1 Version de Docker Engine
- **Commande** :
  ```bash
  curl -s -i http://127.0.0.1:2375/version
  sur windows : curl.exe -s -i http://127.0.0.1:2375/version
  ```
- **Code obtenu** : 

```
HTTP/1.1 200 OK
api-version: 1.55
content-type: application/json
date: Mon, 26 Oct 2026 10:59:09 GMT
docker-experimental: false
ostype: linux
server: Docker/29.7.2 (linux)
transfer-encoding: chunked

{"Platform":{"Name":"Docker Desktop 4.90.0 (238679)"},"Version":"29.7.2","ApiVersion":"1.55","MinAPIVersion":"1.40","Os":"linux","Arch":"amd64","Components":[{"Name":"Engine","Version":"29.7.2","Details":{"ApiVersion":"1.55","Arch":"amd64","BuildTime":"2026-08-05T18:28:36.000000000+00:00","Experimental":"false","GitCommit":"6a43e3d","GoVersion":"go1.26.5","KernelVersion":"6.18.33.2-microsoft-standard-WSL2","MinAPIVersion":"1.40","Module":"github.com/moby/moby/v2","ModuleVersion":"v2.0.0+unknown","Os":"linux"}},{"Name":"containerd","Version":"v2.3.3","Details":{"GitCommit":"aad11006b869517fcd3009450b6f82da282e1a9b"}},{"Name":"runc","Version":"1.4.3","Details":{"GitCommit":"v1.4.3-0-gbb14dabe"}},{"Name":"docker-init","Version":"0.19.0","Details":{"GitCommit":"de40ad0"}}],"GitCommit":"6a43e3d","GoVersion":"go1.26.5","KernelVersion":"6.18.33.2-microsoft-standard-WSL2","BuildTime":"2026-08-05T18:28:36.000000000+00:00"}
```

### 2.2 Liste de tous les conteneurs
- **Commande** :
  ```bash
  curl -s -i "http://127.0.0.1:2375/containers/json?all=true"
  sur windows : curl.exe -s -i "http://127.0.0.1:2375/containers/json?all=true"
  ```
- **Code obtenu** :
```
HTTP/1.1 200 OK
api-version: 1.55
content-type: application/json
date: Mon, 26 Oct 2026 11:01:46 GMT
docker-experimental: false
ostype: linux
server: Docker/29.7.2 (linux)
transfer-encoding: chunked

[{"Id":"9d49a75f78eca0cc75b4958d3c92744b9367e276a0face54000fd475fd89d812","Names":["/intrus"],"Image":"nginx:alpine","ImageID":"sha256:a3554c0d0b48177b1413c26e01f695334428020d9df01243fbae3e11cfd17417","ImageManifestDescriptor":{"mediaType":"application/vnd.oci.image.manifest.v1+json","digest":"sha256:2db7d28517d1fba17c2da3210395f7be11a11b419246ad0da2edf782af72f367","size":2495,"annotations":{"com.docker.official-images.bashbrew.arch":"amd64","org.opencontainers.image.base.digest":"sha256:f9238a4aacf18df0f059cf0389490fc2e876352f75d019f475a48e8765576f24","org.opencontainers.image.base.name":"nginx:1.31.8-alpine-slim","org.opencontainers.image.created":"2026-10-21T23:23:33Z","org.opencontainers.image.revision":"6fabb4c960a02a1e576d2789974c45f4cd2aa854","org.opencontainers.image.source":"https://github.com/nginx/docker-nginx.git#6fabb4c960a02a1e576d2789974c45f4cd2aa854:mainline/alpine","org.opencontainers.image.url":"https://hub.docker.com/_/nginx","org.opencontainers.image.version":"1.31.8-alpine"},"platform":{"architecture":"amd64","os":"linux"}},"Command":"/docker-entrypoint.sh nginx -g 'daemon off;'","Created":1793011278,"Ports":[{"IP":"","PrivatePort":80,"Type":"tcp"}],"Labels":{"desktop.docker.io/ports.scheme":"v2","maintainer":"NGINX Docker Maintainers \u003cdocker-maint@nginx.com\u003e"},"State":"running","Status":"Up 20 minutes","HostConfig":{"NetworkMode":"bridge"},"Health":{"Status":"none","FailingStreak":0},"NetworkSettings":{"Networks":{"bridge":{"IPAMConfig":null,"Links":null,"Aliases":null,"DriverOpts":null,"GwPriority":0,"NetworkID":"f2a8ebfdda625ba4dc21ef758c77e84ca527448fa9b520c2abba74081dca02b9","EndpointID":"f58aa18cbac4d66170b251e5da089e3a7fc743c06657ced9f896c69a0a97233f","Gateway":"172.17.0.1","IPAddress":"172.17.0.3","MacAddress":"6e:e2:1a:7e:a8:8d","IPPrefixLen":16,"IPv6Gateway":"","GlobalIPv6Address":"","GlobalIPv6PrefixLen":0,"DNSNames":null}}},"Mounts":[]},{"Id":"aa0c1fc713794bc11b7a1bd8766fa4df9f54b6573dee4444eae55064086e01e5","Names":["/web1"],"Image":"nginx:alpine","ImageID":"sha256:a3554c0d0b48177b1413c26e01f695334428020d9df01243fbae3e11cfd17417","ImageManifestDescriptor":{"mediaType":"application/vnd.oci.image.manifest.v1+json","digest":"sha256:2db7d28517d1fba17c2da3210395f7be11a11b419246ad0da2edf782af72f367","size":2495,"annotations":{"com.docker.official-images.bashbrew.arch":"amd64","org.opencontainers.image.base.digest":"sha256:f9238a4aacf18df0f059cf0389490fc2e876352f75d019f475a48e8765576f24","org.opencontainers.image.base.name":"nginx:1.31.8-alpine-slim","org.opencontainers.image.created":"2026-10-21T23:23:33Z","org.opencontainers.image.revision":"6fabb4c960a02a1e576d2789974c45f4cd2aa854","org.opencontainers.image.source":"https://github.com/nginx/docker-nginx.git#6fabb4c960a02a1e576d2789974c45f4cd2aa854:mainline/alpine","org.opencontainers.image.url":"https://hub.docker.com/_/nginx","org.opencontainers.image.version":"1.31.8-alpine"},"platform":{"architecture":"amd64","os":"linux"}},"Command":"/docker-entrypoint.sh nginx -g 'daemon off;'","Created":1793011271,"Ports":[],"Labels":{"adminapi.managed":"true","desktop.docker.io/ports.scheme":"v2","maintainer":"NGINX Docker Maintainers \u003cdocker-maint@nginx.com\u003e"},"State":"exited","Status":"Exited (0) 12 minutes ago","HostConfig":{"NetworkMode":"bridge"},"Health":{"Status":"none","FailingStreak":0},"NetworkSettings":{"Networks":{"bridge":{"IPAMConfig":null,"Links":null,"Aliases":null,"DriverOpts":null,"GwPriority":0,"NetworkID":"f2a8ebfdda625ba4dc21ef758c77e84ca527448fa9b520c2abba74081dca02b9","EndpointID":"","Gateway":"","IPAddress":"","MacAddress":"","IPPrefixLen":0,"IPv6Gateway":"","GlobalIPv6Address":"","GlobalIPv6PrefixLen":0,"DNSNames":null}}},"Mounts":[]},{"Id":"458b7cd2ebcd697c0a427a7e945d571d40887e669b689dac6eed386c75b337b5","Names":["/apirest-netbox-1"],"Image":"netboxcommunity/netbox:v4.7-5.1.1","ImageID":"sha256:1d6e0b5fbd48da9868376900561ee55992e04446ce7861dab5e9a42ae01f3fcb","ImageManifestDescriptor":{"mediaType":"application/vnd.oci.image.manifest.v1+json","digest":"sha256:95426f0fc80601fa2ce7caf97f813a0fc5561ee974aab706535392064fd37c0f","size":3708,"platform":{"architecture":"amd64","os":"linux"}},"Command":"/usr/bin/tini -- /opt/netbox/docker-entrypoint.sh /opt/netbox/launch-netbox.sh","Created":1793011069,"Ports":[{"IP":"127.0.0.1","PrivatePort":8080,"PublicPort":8080,"Type":"tcp"}],"Labels":{"com.docker.compose.config-hash":"155461d529f31c84d80540ee42759cf76bb9f246a22fa2e44db500c7eff37447","com.docker.compose.container-number":"1","com.docker.compose.depends_on":"netbox-redis:service_started:false,netbox-postgres:service_healthy:false","com.docker.compose.image":"sha256:95426f0fc80601fa2ce7caf97f813a0fc5561ee974aab706535392064fd37c0f","com.docker.compose.oneoff":"False","com.docker.compose.project":"apirest","com.docker.compose.project.config_files":"C:\\Users\\Yuki\\Documents\\cours\\API\\dj1-api-rest\\TP\\adminapi\\docker-compose.yml","com.docker.compose.project.working_dir":"C:\\Users\\Yuki\\Documents\\cours\\API\\dj1-api-rest\\TP\\adminapi","com.docker.compose.service":"netbox","com.docker.compose.version":"5.5.1","desktop.docker.io/ports.scheme":"v2","desktop.docker.io/ports/8080/tcp":"127.0.0.1:8080","netbox.build-reason":"ubuntu","netbox.git-branch":"HEAD","netbox.git-ref":"3cd2c1d6469caed6c50e80eddd4cde863e0e2cf5","netbox.git-url":"https://github.com/netbox-community/netbox.git","netbox.last-base-image-layer":"sha256:d9d511d19a19ebdeda520e0752f184d457905c7a310df5b748d133a51274ecd4","netbox.original-tag":"v4.7.3-5.1.1","org.opencontainers.image.authors":"The netbox-docker contributors.","org.opencontainers.image.created":"2026-10-17T09:40:59+00:00","org.opencontainers.image.description":"A container based distribution of NetBox, the free and open IPAM and DCIM solution.","org.opencontainers.image.documentation":"https://github.com/netbox-community/netbox-docker/wiki","org.opencontainers.image.licenses":"Apache-2.0","org.opencontainers.image.revision":"7689fec7a70717f7e72feecb383e7a3f55538b5f","org.opencontainers.image.source":"https://github.com/netbox-community/netbox-docker.git","org.opencontainers.image.title":"NetBox Docker","org.opencontainers.image.url":"https://github.com/netbox-community/netbox-docker","org.opencontainers.image.vendor":"The netbox-docker contributors.","org.opencontainers.image.version":"5.1.1"},"State":"running","Status":"Up 23 minutes (healthy)","HostConfig":{"NetworkMode":"apirest_default"},"Health":{"Status":"healthy","FailingStreak":0},"NetworkSettings":{"Networks":{"apirest_default":{"IPAMConfig":null,"Links":null,"Aliases":null,"DriverOpts":null,"GwPriority":0,"NetworkID":"3302e8f54c6ed41ddda7bd68721a9bbcbdb75750286062b12ce0e0dbd8a37e79","EndpointID":"912fbb119ba3b07af289f07ab0c3c6015f8cdaf340afde5678bf65c52712d867","Gateway":"172.18.0.1","IPAddress":"172.18.0.6","MacAddress":"ba:72:c0:3e:11:b8","IPPrefixLen":16,"IPv6Gateway":"","GlobalIPv6Address":"","GlobalIPv6PrefixLen":0,"DNSNames":null}}},"Mounts":[]},{"Id":"0e749a8d868cbc064788ca3e44b33a0cf7e9df5992bd5296b90e64910606fb0a","Names":["/apirest-netbox-postgres-1"],"Image":"postgres:17-alpine","ImageID":"sha256:266d3c257594e195b309ae9d1805e54c788f1d7b375f56bf6c367aac6c917e7b","ImageManifestDescriptor":{"mediaType":"application/vnd.oci.image.manifest.v1+json","digest":"sha256:e4cb93687b062cd25a0e8f672d85a7708235009d70d31f8a7698d9370092a9c4","size":2866,"annotations":{"com.docker.official-images.bashbrew.arch":"amd64","org.opencontainers.image.base.digest":"sha256:c26612752c78b18faacd63c7d50f4592c2e7b0f25e0e20e82bd74e465c57ab0d","org.opencontainers.image.base.name":"alpine:3.24","org.opencontainers.image.created":"2026-10-21T22:54:57Z","org.opencontainers.image.revision":"2603e26e245e558218728ee14e0a42dcb020dc7f","org.opencontainers.image.source":"https://github.com/docker-library/postgres.git#2603e26e245e558218728ee14e0a42dcb020dc7f:17/alpine3.24","org.opencontainers.image.url":"https://hub.docker.com/_/postgres","org.opencontainers.image.version":"17.11-alpine3.24"},"platform":{"architecture":"amd64","os":"linux"}},"Command":"docker-entrypoint.sh postgres","Created":1793011069,"Ports":[{"IP":"","PrivatePort":5432,"Type":"tcp"}],"Labels":{"com.docker.compose.config-hash":"034ca12bc655c8784b5559e2f952532d50a6619918e0efb336f11d0327ffa68d","com.docker.compose.container-number":"1","com.docker.compose.depends_on":"","com.docker.compose.image":"sha256:e4cb93687b062cd25a0e8f672d85a7708235009d70d31f8a7698d9370092a9c4","com.docker.compose.oneoff":"False","com.docker.compose.project":"apirest","com.docker.compose.project.config_files":"C:\\Users\\Yuki\\Documents\\cours\\API\\dj1-api-rest\\TP\\adminapi\\docker-compose.yml","com.docker.compose.project.working_dir":"C:\\Users\\Yuki\\Documents\\cours\\API\\dj1-api-rest\\TP\\adminapi","com.docker.compose.service":"netbox-postgres","com.docker.compose.version":"5.5.1","desktop.docker.io/ports.scheme":"v2"},"State":"running","Status":"Up 23 minutes (healthy)","HostConfig":{"NetworkMode":"apirest_default"},"Health":{"Status":"healthy","FailingStreak":0},"NetworkSettings":{"Networks":{"apirest_default":{"IPAMConfig":null,"Links":null,"Aliases":null,"DriverOpts":null,"GwPriority":0,"NetworkID":"3302e8f54c6ed41ddda7bd68721a9bbcbdb75750286062b12ce0e0dbd8a37e79","EndpointID":"eb7a5342d4f3ad5393b623cf6707bd2deb47ab91a3f37bf8a3605c47d7754738","Gateway":"172.18.0.1","IPAddress":"172.18.0.4","MacAddress":"82:21:31:48:11:a1","IPPrefixLen":16,"IPv6Gateway":"","GlobalIPv6Address":"","GlobalIPv6PrefixLen":0,"DNSNames":null}}},"Mounts":[{"Type":"volume","Name":"apirest_netbox-donnees","Source":"/var/lib/docker/volumes/apirest_netbox-donnees/_data","Destination":"/var/lib/postgresql/data","Driver":"local","Mode":"rw","RW":true,"Propagation":""}]},{"Id":"acfcf9636a2da77103f0add6fcbfdbb607952567bfe9585a2d9fdc1aa28f1540","Names":["/apirest-docker-api-1"],"Image":"tecnativa/docker-socket-proxy:v0.4.2","ImageID":"sha256:1f3a6f303320723d199d2316a3e82b2e2685d86c275d5e3deeaf182573b47476","ImageManifestDescriptor":{"mediaType":"application/vnd.oci.image.manifest.v1+json","digest":"sha256:bd2241b3bec83abcff25927a0a7ae518e0c5bef624b3cc247dcb31e68b53f417","size":1993,"platform":{"architecture":"amd64","os":"linux"}},"Command":"docker-entrypoint.sh haproxy -f /tmp/haproxy.cfg","Created":1793011069,"Ports":[{"IP":"127.0.0.1","PrivatePort":2375,"PublicPort":2375,"Type":"tcp"}],"Labels":{"com.docker.compose.config-hash":"e291b4253c3d8bd3ff1129679585745eecd32d48045d81afccc637ef41d4a605","com.docker.compose.container-number":"1","com.docker.compose.depends_on":"","com.docker.compose.image":"sha256:bd2241b3bec83abcff25927a0a7ae518e0c5bef624b3cc247dcb31e68b53f417","com.docker.compose.oneoff":"False","com.docker.compose.project":"apirest","com.docker.compose.project.config_files":"C:\\Users\\Yuki\\Documents\\cours\\API\\dj1-api-rest\\TP\\adminapi\\docker-compose.yml","com.docker.compose.project.working_dir":"C:\\Users\\Yuki\\Documents\\cours\\API\\dj1-api-rest\\TP\\adminapi","com.docker.compose.service":"docker-api","com.docker.compose.version":"5.5.1","desktop.docker.io/binds/0/Source":"/var/run/docker.sock","desktop.docker.io/binds/0/SourceKind":"dockerSocketProxied","desktop.docker.io/binds/0/Target":"/var/run/docker.sock","desktop.docker.io/ports.scheme":"v2","desktop.docker.io/ports/2375/tcp":"127.0.0.1:2375","org.opencontainers.image.created":"2025-12-16T07:26:21.623Z","org.opencontainers.image.description":"Proxy over your Docker socket to restrict which requests it accepts","org.opencontainers.image.licenses":"Apache-2.0","org.opencontainers.image.revision":"2f04313b042c1bf4dfbd039475dfc42db79bde7a","org.opencontainers.image.source":"https://github.com/Tecnativa/docker-socket-proxy","org.opencontainers.image.title":"docker-socket-proxy","org.opencontainers.image.url":"https://github.com/Tecnativa/docker-socket-proxy","org.opencontainers.image.version":"v0.4.2"},"State":"running","Status":"Up 23 minutes","HostConfig":{"NetworkMode":"apirest_default"},"Health":{"Status":"none","FailingStreak":0},"NetworkSettings":{"Networks":{"apirest_default":{"IPAMConfig":null,"Links":null,"Aliases":null,"DriverOpts":null,"GwPriority":0,"NetworkID":"3302e8f54c6ed41ddda7bd68721a9bbcbdb75750286062b12ce0e0dbd8a37e79","EndpointID":"d94a3753f3204aeacd035f882e0d3f2e6d5fe7a5b2116683dd22c007707639bd","Gateway":"172.18.0.1","IPAddress":"172.18.0.3","MacAddress":"5a:c5:85:48:17:66","IPPrefixLen":16,"IPv6Gateway":"","GlobalIPv6Address":"","GlobalIPv6PrefixLen":0,"DNSNames":null}}},"Mounts":[{"Type":"bind","Source":"/run/host-services/docker.proxy.sock","Destination":"/var/run/docker.sock","Mode":"ro","RW":false,"Propagation":"rprivate"}]},{"Id":"c32544ddca4a609c56805c1ce8432cbf6836d37c33ba667d4ab06b2f868e0d15","Names":["/apirest-netbox-redis-1"],"Image":"valkey/valkey:8-alpine","ImageID":"sha256:081c2f5cb575efc901aa80ff9cdbd1ec6a301682fd35e1ebb4b0990a4a4a8507","ImageManifestDescriptor":{"mediaType":"application/vnd.oci.image.manifest.v1+json","digest":"sha256:32627109abf6f741121096b45c732f758876803efd7b2e1018ebc0350d117119","size":1616,"platform":{"architecture":"amd64","os":"linux"}},"Command":"tini -- docker-entrypoint.sh valkey-server","Created":1793011069,"Ports":[{"IP":"","PrivatePort":6379,"Type":"tcp"}],"Labels":{"com.docker.compose.config-hash":"910fda7eb6249f90cab1394d675ed118b6cb0b5a55bee466b73cf9a23fb8c8f5","com.docker.compose.container-number":"1","com.docker.compose.depends_on":"","com.docker.compose.image":"sha256:32627109abf6f741121096b45c732f758876803efd7b2e1018ebc0350d117119","com.docker.compose.oneoff":"False","com.docker.compose.project":"apirest","com.docker.compose.project.config_files":"C:\\Users\\Yuki\\Documents\\cours\\API\\dj1-api-rest\\TP\\adminapi\\docker-compose.yml","com.docker.compose.project.working_dir":"C:\\Users\\Yuki\\Documents\\cours\\API\\dj1-api-rest\\TP\\adminapi","com.docker.compose.service":"netbox-redis","com.docker.compose.version":"5.5.1","desktop.docker.io/ports.scheme":"v2","org.opencontainers.image.source":"https://github.com/valkey-io/valkey"},"State":"running","Status":"Up 23 minutes","HostConfig":{"NetworkMode":"apirest_default"},"Health":{"Status":"none","FailingStreak":0},"NetworkSettings":{"Networks":{"apirest_default":{"IPAMConfig":null,"Links":null,"Aliases":null,"DriverOpts":null,"GwPriority":0,"NetworkID":"3302e8f54c6ed41ddda7bd68721a9bbcbdb75750286062b12ce0e0dbd8a37e79","EndpointID":"63a34e10a86699d82bee5f6e15e19194eae32b1ef64a9e65d9c1f41bb1806814","Gateway":"172.18.0.1","IPAddress":"172.18.0.2","MacAddress":"46:0e:c7:14:ba:d1","IPPrefixLen":16,"IPv6Gateway":"","GlobalIPv6Address":"","GlobalIPv6PrefixLen":0,"DNSNames":null}}},"Mounts":[]},{"Id":"ba54f27ef62ae422233a62c0fbb8d0d1c98470c8f5d56de79a20a8d059ad271f","Names":["/apirest-api-1"],"Image":"apirest-api","ImageID":"sha256:89ca3af055f004d97d58ee88d7c89d0ac9ed6acb80a02e40b38b8bfb4322dbad","ImageManifestDescriptor":{"mediaType":"application/vnd.oci.image.manifest.v1+json","digest":"sha256:2f66b25f0dfa468f58696ccdd95f48ee9bd33c885c72663df904d5e60c87843c","size":1812,"platform":{"architecture":"amd64","os":"linux"}},"Command":"uvicorn app.main:app --host 0.0.0.0 --port 8000","Created":1793007705,"Ports":[{"IP":"127.0.0.1","PrivatePort":8000,"PublicPort":8000,"Type":"tcp"}],"Labels":{"com.docker.compose.config-hash":"cdddcf943dc3b3f0ac0fddad19309aeeed0c8c5ae949ab87831f81633cc50eea","com.docker.compose.container-number":"1","com.docker.compose.depends_on":"docker-api:service_started:false","com.docker.compose.image":"sha256:2f66b25f0dfa468f58696ccdd95f48ee9bd33c885c72663df904d5e60c87843c","com.docker.compose.oneoff":"False","com.docker.compose.project":"apirest","com.docker.compose.project.config_files":"C:\\Users\\Yuki\\Documents\\cours\\API\\dj1-api-rest\\TP\\docker-compose.yml","com.docker.compose.project.working_dir":"C:\\Users\\Yuki\\Documents\\cours\\API\\dj1-api-rest\\TP","com.docker.compose.replace":"api-1","com.docker.compose.service":"api","com.docker.compose.version":"5.5.1","desktop.docker.io/ports.scheme":"v2","desktop.docker.io/ports/8000/tcp":"127.0.0.1:8000"},"State":"running","Status":"Up About an hour","HostConfig":{"NetworkMode":"apirest_default"},"Health":{"Status":"none","FailingStreak":0},"NetworkSettings":{"Networks":{"apirest_default":{"IPAMConfig":null,"Links":null,"Aliases":null,"DriverOpts":null,"GwPriority":0,"NetworkID":"3302e8f54c6ed41ddda7bd68721a9bbcbdb75750286062b12ce0e0dbd8a37e79","EndpointID":"41797ba8f8a67da300c1de41c5caaaa93541dc02db93a634377c5104d4b8e634","Gateway":"172.18.0.1","IPAddress":"172.18.0.5","MacAddress":"16:37:ee:72:de:53","IPPrefixLen":16,"IPv6Gateway":"","GlobalIPv6Address":"","GlobalIPv6PrefixLen":0,"DNSNames":null}}},"Mounts":[]},{"Id":"3e7f831da494c0811199693ac65d83f931328b95b2f3a476395c23902f7b07c1","Names":["/exegol-default"],"Image":"nwodtuhs/exegol:free","ImageID":"sha256:ea29847ca73c566233b2194243ff61c113411328d6e92438aad32bc83ac44b72","ImageManifestDescriptor":{"mediaType":"application/vnd.docker.distribution.manifest.v2+json","digest":"sha256:d7b5e4b1f75e6ca2a861f93d24823b973b9f40fb91bbfe14279310dbb87f5404","size":7498,"platform":{"architecture":"amd64","os":"linux"}},"Command":"/bin/bash /.exegol/entrypoint.sh load_setups 'ovpn --config /.exegol/vpn/config/client.ovpn' endless","Created":1788964877,"Ports":[],"Labels":{"desktop.docker.io/mounts/0/Source":"/home/yuki/.local/share/pipx/venvs/exegol/lib/python3.13/site-packages/exegol/utils/imgsync/spawn.sh","desktop.docker.io/mounts/0/SourceKind":"wsl2DistroFile","desktop.docker.io/mounts/0/Target":"/.exegol/spawn.sh","desktop.docker.io/mounts/1/Source":"/mnt/wslg/.X11-unix","desktop.docker.io/mounts/1/SourceKind":"hostFile","desktop.docker.io/mounts/1/Target":"/tmp/.X11-unix","desktop.docker.io/mounts/2/Source":"/etc/timezone","desktop.docker.io/mounts/2/SourceKind":"wsl2DistroFile","desktop.docker.io/mounts/2/Target":"/etc/timezone","desktop.docker.io/mounts/3/Source":"/etc/localtime","desktop.docker.io/mounts/3/SourceKind":"wsl2DistroFile","desktop.docker.io/mounts/3/Target":"/etc/localtime","desktop.docker.io/mounts/4/Source":"/home/yuki/.exegol/my-resources","desktop.docker.io/mounts/4/SourceKind":"wsl2DistroFile","desktop.docker.io/mounts/4/Target":"/opt/my-resources","desktop.docker.io/mounts/5/Source":"/home/yuki/.exegol/exegol-resources","desktop.docker.io/mounts/5/SourceKind":"wsl2DistroFile","desktop.docker.io/mounts/5/Target":"/opt/resources","desktop.docker.io/mounts/6/Source":"/mnt/c/Users/Yuki/Documents/cours/Exploit/etudiants.ovpn","desktop.docker.io/mounts/6/SourceKind":"hostFile","desktop.docker.io/mounts/6/Target":"/.exegol/vpn/config/client.ovpn","desktop.docker.io/mounts/7/Source":"/home/yuki/.exegol/workspaces/default","desktop.docker.io/mounts/7/SourceKind":"wsl2DistroFile","desktop.docker.io/mounts/7/Target":"/workspace","desktop.docker.io/ports.scheme":"v2","desktop.docker.io/wsl-distro":"Debian","org.exegol.app":"Exegol","org.exegol.build_date":"2026-03-02T00:31:37Z","org.exegol.metadata.creation_date":"2026-09-09T16:41:17Z","org.exegol.metadata.passwd":"QG6umw8JEDVcCyizwFMRNM5pnra0hP","org.exegol.src_repository":"https://github.com/ThePorgs/Exegol-images","org.exegol.tag":"free","org.exegol.version":"3.1.14"},"State":"exited","Status":"Exited (255) 19 hours ago","HostConfig":{"NetworkMode":"bridge"},"Health":{"Status":"none","FailingStreak":0},"NetworkSettings":{"Networks":{"bridge":{"IPAMConfig":null,"Links":null,"Aliases":null,"DriverOpts":null,"GwPriority":0,"NetworkID":"9a123da7194e83cc4c0da813d790f40d558fbe45b1b73c4a1ae23d1955aa1a66","EndpointID":"069611fb6ada7f01373d5435d0b207b128a4f081f19a530141692f2eb3235bc3","Gateway":"172.17.0.1","IPAddress":"172.17.0.2","MacAddress":"d6:34:4f:5e:67:d5","IPPrefixLen":16,"IPv6Gateway":"","GlobalIPv6Address":"","GlobalIPv6PrefixLen":0,"DNSNames":null}}},"Mounts":[{"Type":"bind","Source":"/run/desktop/mnt/host/wsl/docker-desktop-bind-mounts/Debian/b991cfcfee75e71c2e893e7cf676d29fd899adffcc305bcbacd3868269f0c688","Destination":"/workspace","Mode":"","RW":true,"Propagation":"rprivate"},{"Type":"bind","Source":"/run/desktop/mnt/host/wsl/docker-desktop-bind-mounts/Debian/e0e183f1aa7c4b43e5d0a063a002bffa6ed71cefb9d9a1e03dd61b63dd07fe10","Destination":"/.exegol/spawn.sh","Mode":"","RW":false,"Propagation":"rprivate"},{"Type":"bind","Source":"/run/desktop/mnt/host/wsl/docker-desktop-bind-mounts/Debian/3375cd75ea911d1c9fbc213c918e3b92ef6be305698d78aa10a437191c1fcab6","Destination":"/.exegol/vpn/config/client.ovpn","Mode":"","RW":false,"Propagation":"rprivate"},{"Type":"bind","Source":"/run/desktop/mnt/host/wsl/docker-desktop-bind-mounts/Debian/b21df4cc4e54c6ce3c254c02f439fe4fc15e0cba3e23de366b06f0d332b589fb","Destination":"/etc/localtime","Mode":"","RW":false,"Propagation":"rprivate"},{"Type":"bind","Source":"/run/desktop/mnt/host/wsl/docker-desktop-bind-mounts/Debian/20d6ceed201c4b2f67ae83b566216415ae9ed8cdce0ac5e71f51907c8500e655","Destination":"/etc/timezone","Mode":"","RW":false,"Propagation":"rprivate"},{"Type":"bind","Source":"/run/desktop/mnt/host/wsl/docker-desktop-bind-mounts/Debian/a9ab9a7e47b4869d026a80c4bb05178144978c675d6633ceeb93d033c5e1adfa","Destination":"/opt/my-resources","Mode":"","RW":true,"Propagation":"rprivate"},{"Type":"bind","Source":"/run/desktop/mnt/host/wsl/docker-desktop-bind-mounts/Debian/b9bbdc331e200357bb4963bf9bd1f29e8235a00f57caa05bd190e94234cee1f8","Destination":"/opt/resources","Mode":"","RW":true,"Propagation":"rprivate"},{"Type":"bind","Source":"/run/desktop/mnt/host/wsl/docker-desktop-bind-mounts/Debian/eec1edc678759b3d4db2cd8b722ea20629444d4c53342af76c7d4d193bf8d251","Destination":"/tmp/.X11-unix","Mode":"","RW":true,"Propagation":"rprivate"}]}]
```

### 2.3 Liste filtrée sur le label `adminapi.managed=true`
- **Commande** :
  ```bash
  curl -s -i --get http://127.0.0.1:2375/containers/json \
       --data-urlencode 'all=true' \
       --data-urlencode 'filters={"label":["adminapi.managed=true"]}'
  sur windows : curl.exe -s -i "http://127.0.0.1:2375/containers/json?all=true&filters=%7B%22label%22%3A%5B%22adminapi.managed%3Dtrue%22%5D%7D"
  ```
- **Code obtenu** : 
```
HTTP/1.1 200 OK
api-version: 1.55
content-type: application/json
date: Mon, 26 Oct 2026 11:05:29 GMT
docker-experimental: false
ostype: linux
server: Docker/29.7.2 (linux)
transfer-encoding: chunked

[{"Id":"aa0c1fc713794bc11b7a1bd8766fa4df9f54b6573dee4444eae55064086e01e5","Names":["/web1"],"Image":"nginx:alpine","ImageID":"sha256:a3554c0d0b48177b1413c26e01f695334428020d9df01243fbae3e11cfd17417","ImageManifestDescriptor":{"mediaType":"application/vnd.oci.image.manifest.v1+json","digest":"sha256:2db7d28517d1fba17c2da3210395f7be11a11b419246ad0da2edf782af72f367","size":2495,"annotations":{"com.docker.official-images.bashbrew.arch":"amd64","org.opencontainers.image.base.digest":"sha256:f9238a4aacf18df0f059cf0389490fc2e876352f75d019f475a48e8765576f24","org.opencontainers.image.base.name":"nginx:1.31.8-alpine-slim","org.opencontainers.image.created":"2026-10-21T23:23:33Z","org.opencontainers.image.revision":"6fabb4c960a02a1e576d2789974c45f4cd2aa854","org.opencontainers.image.source":"https://github.com/nginx/docker-nginx.git#6fabb4c960a02a1e576d2789974c45f4cd2aa854:mainline/alpine","org.opencontainers.image.url":"https://hub.docker.com/_/nginx","org.opencontainers.image.version":"1.31.8-alpine"},"platform":{"architecture":"amd64","os":"linux"}},"Command":"/docker-entrypoint.sh nginx -g 'daemon off;'","Created":1793011271,"Ports":[],"Labels":{"adminapi.managed":"true","desktop.docker.io/ports.scheme":"v2","maintainer":"NGINX Docker Maintainers \u003cdocker-maint@nginx.com\u003e"},"State":"exited","Status":"Exited (0) 16 minutes ago","HostConfig":{"NetworkMode":"bridge"},"Health":{"Status":"none","FailingStreak":0},"NetworkSettings":{"Networks":{"bridge":{"IPAMConfig":null,"Links":null,"Aliases":null,"DriverOpts":null,"GwPriority":0,"NetworkID":"f2a8ebfdda625ba4dc21ef758c77e84ca527448fa9b520c2abba74081dca02b9","EndpointID":"","Gateway":"","IPAddress":"","MacAddress":"","IPPrefixLen":0,"IPv6Gateway":"","GlobalIPv6Address":"","GlobalIPv6PrefixLen":0,"DNSNames":null}}},"Mounts":[]}]
```

### 2.4 Détail du conteneur `web1`
- **Commande** :
  ```bash
  curl -s -i http://127.0.0.1:2375/containers/web1/json
  sur windows : curl.exe -s -i http://127.0.0.1:2375/containers/web1/json
  ```
- **Code obtenu** : 
```
HTTP/1.1 200 OK
api-version: 1.55
content-type: application/json
date: Mon, 26 Oct 2026 11:04:45 GMT
docker-experimental: false
ostype: linux
server: Docker/29.7.2 (linux)
transfer-encoding: chunked

{"Id":"aa0c1fc713794bc11b7a1bd8766fa4df9f54b6573dee4444eae55064086e01e5","Created":"2026-10-26T10:41:11.67663818Z","Path":"/docker-entrypoint.sh","Args":["nginx","-g","daemon off;"],"State":{"Status":"exited","Running":false,"Paused":false,"Restarting":false,"OOMKilled":false,"Dead":false,"Pid":0,"ExitCode":0,"Error":"","StartedAt":"2026-10-26T10:41:11.781601612Z","FinishedAt":"2026-10-26T10:49:13.882192293Z"},"Image":"sha256:a3554c0d0b48177b1413c26e01f695334428020d9df01243fbae3e11cfd17417","ResolvConfPath":"/var/lib/docker/containers/aa0c1fc713794bc11b7a1bd8766fa4df9f54b6573dee4444eae55064086e01e5/resolv.conf","HostnamePath":"/var/lib/docker/containers/aa0c1fc713794bc11b7a1bd8766fa4df9f54b6573dee4444eae55064086e01e5/hostname","HostsPath":"/var/lib/docker/containers/aa0c1fc713794bc11b7a1bd8766fa4df9f54b6573dee4444eae55064086e01e5/hosts","LogPath":"/var/lib/docker/containers/aa0c1fc713794bc11b7a1bd8766fa4df9f54b6573dee4444eae55064086e01e5/aa0c1fc713794bc11b7a1bd8766fa4df9f54b6573dee4444eae55064086e01e5-json.log","Name":"/web1","RestartCount":0,"Driver":"overlayfs","Platform":"linux","MountLabel":"","ProcessLabel":"","AppArmorProfile":"","ExecIDs":null,"HostConfig":{"Binds":null,"ContainerIDFile":"","LogConfig":{"Type":"json-file","Config":{}},"NetworkMode":"bridge","PortBindings":{},"RestartPolicy":{"Name":"no","MaximumRetryCount":0},"AutoRemove":false,"VolumeDriver":"","VolumesFrom":null,"ConsoleSize":[13,158],"CapAdd":null,"CapDrop":null,"CgroupnsMode":"private","Dns":null,"DnsOptions":[],"DnsSearch":[],"ExtraHosts":null,"GroupAdd":null,"IpcMode":"private","Cgroup":"","Links":null,"OomScoreAdj":0,"PidMode":"","Privileged":false,"PublishAllPorts":false,"ReadonlyRootfs":false,"SecurityOpt":null,"UTSMode":"","UsernsMode":"","ShmSize":67108864,"Runtime":"runc","Isolation":"","CpuShares":0,"Memory":0,"NanoCpus":0,"CgroupParent":"","BlkioWeight":0,"BlkioWeightDevice":[],"BlkioDeviceReadBps":[],"BlkioDeviceWriteBps":[],"BlkioDeviceReadIOps":[],"BlkioDeviceWriteIOps":[],"CpuPeriod":0,"CpuQuota":0,"CpuRealtimePeriod":0,"CpuRealtimeRuntime":0,"CpusetCpus":"","CpusetMems":"","Devices":[],"DeviceCgroupRules":null,"DeviceRequests":null,"MemoryReservation":0,"MemorySwap":0,"MemorySwappiness":null,"OomKillDisable":null,"PidsLimit":null,"Ulimits":[],"CpuCount":0,"CpuPercent":0,"IOMaximumIOps":0,"IOMaximumBandwidth":0,"MaskedPaths":["/proc/acpi","/proc/asound","/proc/interrupts","/proc/kcore","/proc/keys","/proc/latency_stats","/proc/sched_debug","/proc/scsi","/proc/timer_list","/proc/timer_stats","/sys/devices/virtual/powercap","/sys/firmware"],"ReadonlyPaths":["/proc/bus","/proc/fs","/proc/irq","/proc/sys","/proc/sysrq-trigger"]},"Storage":{"RootFS":{"Snapshot":{"Name":"overlayfs"}}},"Mounts":[],"Config":{"Hostname":"aa0c1fc71379","Domainname":"","User":"","AttachStdin":false,"AttachStdout":false,"AttachStderr":false,"ExposedPorts":{"80/tcp":{}},"Tty":false,"OpenStdin":false,"StdinOnce":false,"Env":["PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin","NGINX_VERSION=1.31.8","PKG_RELEASE=1","DYNPKG_RELEASE=1","NJS_VERSION=1.0.2","NJS_RELEASE=1","ACME_VERSION=0.4.1"],"Cmd":["nginx","-g","daemon off;"],"Image":"nginx:alpine","Volumes":null,"WorkingDir":"/","Entrypoint":["/docker-entrypoint.sh"],"Labels":{"adminapi.managed":"true","maintainer":"NGINX Docker Maintainers \u003cdocker-maint@nginx.com\u003e"},"StopSignal":"SIGQUIT"},"NetworkSettings":{"SandboxID":"","SandboxKey":"","Ports":{},"Networks":{"bridge":{"IPAMConfig":null,"Links":null,"Aliases":null,"DriverOpts":null,"GwPriority":0,"NetworkID":"f2a8ebfdda625ba4dc21ef758c77e84ca527448fa9b520c2abba74081dca02b9","EndpointID":"","Gateway":"","IPAddress":"","MacAddress":"","IPPrefixLen":0,"IPv6Gateway":"","GlobalIPv6Address":"","GlobalIPv6PrefixLen":0,"DNSNames":null}}},"ImageManifestDescriptor":{"mediaType":"application/vnd.oci.image.manifest.v1+json","digest":"sha256:2db7d28517d1fba17c2da3210395f7be11a11b419246ad0da2edf782af72f367","size":2495,"annotations":{"com.docker.official-images.bashbrew.arch":"amd64","org.opencontainers.image.base.digest":"sha256:f9238a4aacf18df0f059cf0389490fc2e876352f75d019f475a48e8765576f24","org.opencontainers.image.base.name":"nginx:1.31.8-alpine-slim","org.opencontainers.image.created":"2026-10-21T23:23:33Z","org.opencontainers.image.revision":"6fabb4c960a02a1e576d2789974c45f4cd2aa854","org.opencontainers.image.source":"https://github.com/nginx/docker-nginx.git#6fabb4c960a02a1e576d2789974c45f4cd2aa854:mainline/alpine","org.opencontainers.image.url":"https://hub.docker.com/_/nginx","org.opencontainers.image.version":"1.31.8-alpine"},"platform":{"architecture":"amd64","os":"linux"}}}
```

### 2.5 Arrêt de `web1` (deux fois de suite pour observer l'idempotence/codes)
- **Premier arrêt** :
  ```bash
  curl -s -o /dev/null -w "%{http_code}\n" -X POST http://127.0.0.1:2375/containers/web1/stop
  sur windows : curl.exe -s -o NUL -w "%{http_code}`n" -X POST http://127.0.0.1:2375/containers/web1/stop
  ```
- **Code obtenu** : `204`

- **Deuxième arrêt immédiat** :
  ```bash
  curl -s -o /dev/null -w "%{http_code}\n" -X POST http://127.0.0.1:2375/containers/web1/stop
  sur windows : curl.exe -s -o NUL -w "%{http_code}`n" -X POST http://127.0.0.1:2375/containers/web1/stop
  ```
- **Code obtenu** : `304`

### 2.6 Demande d'un conteneur inexistant
- **Commande** :
  ```bash
  curl -s -o /dev/null -w "%{http_code}\n" http://127.0.0.1:2375/containers/inconnu-12345/json
  sur windows : curl.exe -s -o NUL -w "%{http_code}`n" http://127.0.0.1:2375/containers/inconnu-12345/json
  ```
- **Code obtenu** : `404`

---

## 3. API NetBox (http://127.0.0.1:8080/api)

### 3.1 Provisioning d'un token administrateur
- Mise en place du token dans la variable `NETBOX_TOKEN` du fichier `.env`.

### 3.2 Création du type de cluster `Docker`
- **Commande** :
  ```bash
  curl -s -X POST http://127.0.0.1:8080/api/virtualization/cluster-types/ \
       -H "Authorization: Bearer $NETBOX_TOKEN" \
       -H "Content-Type: application/json" \
       -H "Accept: application/json" \
       -d '{"name": "Docker", "slug": "docker"}'
  sur windows : curl.exe -s -X POST http://127.0.0.1:8080/api/virtualization/cluster-types/ `
                          -H "Authorization: Bearer $NETBOX_TOKEN" `
                          -H "Content-Type: application/json" `
                          -H "Accept: application/json" `
                          -d '{\"name\": \"Docker\", \"slug\": \"docker\"}'
  ```
- **Code obtenu** : `{"name":["cluster type with this name already exists."],"slug":["cluster type with this slug already exists."]}`

### 3.3 Création du cluster `docker-local`
- **Commande** (en remplaçant le type par l'ID obtenu à l'étape 3.2) :
  ```bash
  curl -s -X POST http://127.0.0.1:8080/api/virtualization/clusters/ \
       -H "Authorization: Bearer $NETBOX_TOKEN" \
       -H "Content-Type: application/json" \
       -H "Accept: application/json" \
       -d '{"name": "docker-local", "type": 1}'
  sur windows : curl.exe -s -i -X POST http://127.0.0.1:8080/api/virtualization/clusters/ `
                          -H "Authorization: Bearer $NETBOX_TOKEN" `
                          -H "Content-Type: application/json" `
                          -H "Accept: application/json" `
                          -d '{\"name\": \"docker-local\", \"type\": 1}'
  ```
- **Code obtenu** : 
```
HTTP/1.1 201 Created
content-type: application/json
location: http://127.0.0.1:8080/api/virtualization/clusters/2/
etag: W/"2026-10-26T11:11:29.362629+00:00"
vary: HX-Request, Accept-Language, Cookie, origin
allow: GET, POST, PUT, PATCH, DELETE, HEAD, OPTIONS
x-request-id: 401383be-126d-4479-a97f-1bc68d42441b
api-version: 4.7
x-content-type-options: nosniff
referrer-policy: same-origin
cross-origin-opener-policy: same-origin
x-frame-options: SAMEORIGIN
content-length: 719
content-language: en
server: granian
date: Mon, 26 Oct 2026 11:11:29 GMT

{"id":2,"url":"http://127.0.0.1:8080/api/virtualization/clusters/2/","display_url":"http://127.0.0.1:8080/virtualization/clusters/2/","display":"docker-local","name":"docker-local","type":{"id":1,"url":"http://127.0.0.1:8080/api/virtualization/cluster-types/1/","display":"Docker","name":"Docker","slug":"docker","description":""},"group":null,"status":{"value":"active","label":"Active"},"tenant":null,"scope_type":null,"scope_id":null,"scope":null,"description":"","owner":null,"comments":"","tags":[],"custom_fields":{},"created":"2026-10-26T11:11:29.362593Z","last_updated":"2026-10-26T11:11:29.362629Z","device_count":0,"virtualmachine_count":0,"allocated_vcpus":null,"allocated_memory":null,"allocated_disk":null}
```