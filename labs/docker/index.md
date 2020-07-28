# Docker


## Instalasi

## Swarm

### Inisialisasi

satu node

` docker swarm init`

multi node

`docker swarm init --advertise-addr <MANAGER-IP>`

### Menambahkan Node

menambahkan node

````
docker swarm join \
  --token  <TOKEN> \
  <MANAGER-IP>:2377
````

menampilkan token

`docker swarm join-token worker`

### Membuat Service

`docker service create --replicas 1 --name helloworld alpine ping docker.com`

### Memeriksa Service

`docker service inspect --pretty helloworld`

### Mengubah Skala

`docker service scale <SERVICE-ID>=<NUMBER-OF-TASKS>`

contoh:

`docker service scale helloworld=5`

### Menghapus Service

`docker service rm helloworld`

### Mengupdate Service

````
docker service create \
  --replicas 3 \
  --name redis \
  --update-delay 10s \
  redis:3.0.6
````

