## docker-cookbook  

### Docker Commands:


```
arun-mac:docker-cookbook arunaja$ docker login
Authenticating with existing credentials...
Login Succeeded
```
`docker build -t arunaja/catalogservice .`

`docker push arunaja/catalogservice`


```
arun-mac:docker-cookbook arunaja$ docker images
REPOSITORY                     TAG                 IMAGE ID            CREATED             SIZE
arunaja/bff                    latest              9e96ca05abca        5 hours ago         121MB
arunaja/catalogservice         latest              c59a967f156c        5 hours ago         140MB
arunaja/productcatalogwebapp   latest              ddc338750385        7 hours ago         128MB
openjdk                        8-jdk-alpine        3675b9f543c5        2 weeks ago         105MB
registry                       latest              177391bcf802        16 months ago       33.3MB

```

` docker run -d -p 8080:8080 --name aj_product_catalog --network aj_net ddc338750385 `

  ` docker run -d -p 7070:7070 --name aj_catalog_service --network aj_net c59a967f156c `
  
  ` docker run -d -p 6060:6060 --name aj_bff --network aj_net 9e96ca05abca `
  
  `docker ps -q -f "label=app.name=BFF"`

`docker ps  -f  "name=aj_*" `

`docker exec -it aj_product_catalog bin/sh `
 
`eval $"docker ps -q -f 'name=aj_*'"`

`docker ps   -q -f  "name=aj_*" `

` docker container stop $(docker ps -q -f 'name=aj_*') `

` docker container start $(docker ps -a  -q -f 'name=aj_*') `
 
 `docker network create --driver bridge aj_net `
 
 ### Best Practises :
 
 `  1 . Always use naming conventions while naming the conatiner  , this helps filtering ` 
     
  ` 2.  Provide as much  as labels while creating image , this helps filtering `
  
  ` 3 . Use user provided networks while connecting more than one container ` 
  
  
  
  #### Workouts 
  
  ```
  arun-mac:catalogservice arunaja$ docker network inspect aj_net
[
    {
        "Name": "aj_net",
        "Id": "71bb40c195eb0a207c22a0e755c87672d6daa82d4ba31478f81c13220fa379e4",
        "Created": "2019-04-24T13:27:04.742978436Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "0f972c4129d2698bd70d89b5ef51350502a1548adf948f5d557e16fae99f3c04": {
                "Name": "aj_catalog_service",
                "EndpointID": "a44c274b3049fdaacf2c658afc66f9009768c2055ea642c7a81952fb1862cc63",
                "MacAddress": "02:42:ac:12:00:03",
                "IPv4Address": "172.18.0.3/16",
                "IPv6Address": ""
            },
            "6e2f83026775960a99675c8200e1cfc4ab1314cb2b0ce7e50b3717815b119744": {
                "Name": "aj_product_catalog",
                "EndpointID": "2b748cfb0b4ea9cab0bd562ba1b4a8af10f5d609ae9fdc29bd8ccee0730f46b7",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            },
            "a3d915f07807f10ea934480e0519d0e2a53530cc295f222ee0dce6d46d933c2f": {
                "Name": "aj_bff",
                "EndpointID": "f13e3ea2cae8d4fd4207db079350d7b612e2344a536a65092c8b1622dfa8a1eb",
                "MacAddress": "02:42:ac:12:00:04",
                "IPv4Address": "172.18.0.4/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
```

```
docker exec -it aj_bff_service /bin/sh
/deploy # ip addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: tunl0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN qlen 1
    link/ipip 0.0.0.0 brd 0.0.0.0
3: ip6tnl0@NONE: <NOARP> mtu 1452 qdisc noop state DOWN qlen 1
    link/tunnel6 00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00 brd 00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00
100: eth0@if101: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
    link/ether 02:42:ac:11:00:05 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.5/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
       
```       
 ```
 /deploy # ping -c 2 aj_catalog_service
PING aj_catalog_service (172.18.0.3): 56 data bytes
64 bytes from 172.18.0.3: seq=0 ttl=64 time=0.096 ms
64 bytes from 172.18.0.3: seq=1 ttl=64 time=0.151 ms

--- aj_catalog_service ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 0.096/0.123/0.151 ms

```
 
 
 
