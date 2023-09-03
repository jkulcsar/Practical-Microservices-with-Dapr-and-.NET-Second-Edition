
I've introduced Redis as state store, updated ./components accordingly
Starting the applicatioons with the launch.ps1 commands works, I can see that state is stored in Redis (and restored) and that the applications are able to communicate with each other. But I don't see anywhere the Redis stores in the dapr dashboard.

With `docker inspect` we can get the external IP of the container, in my case:

```shell
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' 0c5d6a1fe182e9dc30877463da2fa6ecf7879642ddecc32dec3296eaa1431a89
```

It will pop-out the external IP of the container, in my case:

```shell
172.17.0.2
```
ALternatively, the following command can be used; replace `bridge` with the name of the network you want to inspect.

```shell
docker network inspect bridge
```

```json
[
    {
        "Name": "bridge",
        "Id": "98fa7d04bfb53762a11514b14667601b2c7f5d39ccf03a4182496c33e7462d8b",
        "Created": "2023-09-03T13:03:24.1824483Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
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
            "0c5d6a1fe182e9dc30877463da2fa6ecf7879642ddecc32dec3296eaa1431a89": {
                "Name": "dapr_redis",
                "EndpointID": "0291365eb322951b0a1fa9759d5d2dc54a836a5118f6b9615ff88f47c4094e1e",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            },
            "a0ddcb31ff11221e7a393914ac3af05614f178bee221cc89cb6a646e9cc8f73f": {
                "Name": "dapr_zipkin",
                "EndpointID": "67965f8c710daf438f063731142af2223a840243cdd7fdec20fffc5af8494e73",
                "MacAddress": "02:42:ac:11:00:03",
                "IPv4Address": "172.17.0.3/16",
                "IPv6Address": ""
            },
            "c0bbb9794791f258dcbc47272fe9f68537b7a8db214cda4de8d16d5f214d95e0": {
                "Name": "dapr_placement",
                "EndpointID": "1b4c9b368677e1e8ca00e58a0153e9fed7e53b23d7fdeb704d1ded6a04b40825",
                "MacAddress": "02:42:ac:11:00:04",
                "IPv4Address": "172.17.0.4/16",
                "IPv6Address": ""
            },
            "d9af325f66b843f217da1aaa62a050e104f97bc2df35ce13d6deec9a22b78ffd": {
                "Name": "redis-commander",
                "EndpointID": "0f437fb6fddf6af00ba3d0ada4e1956170a86c11224cbaa6342659ccd3fbba5f",
                "MacAddress": "02:42:ac:11:00:05",
                "IPv4Address": "172.17.0.5/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]

```