# docker command 

```bash
docker run -d --network host --name nats-server 1 -v /nats:/nats -ti nats:latest -config /nats/nats.conf
```

# config files 
config is located in the `/nats/nats.conf` location 

```conf
# Cluster server 1

server_name: "nats-server1"

listen: 10.2.17.15:4222 #current server IP
#http: 8222

max_payload: 64MB

max_connections: 10000

jetstream {
	store_dir: "/nats/data"
}

cluster {
   name: "cluster1" #keep this name the same everywhere
   listen: 10.2.17.15:4248 #main server
   routes = [
		nats://10.2.17.16:4248 #server2
        nats://10.2.17.17:4248 #server3
]
}
```