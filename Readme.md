

# Install Docker cyclos image

### Pull  image

```bash
docker pull cyclos/cyclos
Using default tag: latest
latest: Pulling from cyclos/cyclos
71dca2167f9f: Pull complete
799297b6f921: Pull complete
c8c6b1f7c640: Pull complete
09a7603aa03d: Pull complete
50fe6d0cba0c: Pull complete
4b745a9fe8dc: Pull complete
49e6a519ff53: Pull complete
289b431299e5: Pull complete
ceee1b148da7: Pull complete
b26072927e6b: Pull complete
e46794033b1d: Pull complete
4f4fb700ef54: Pull complete
918f60c1e8fc: Pull complete
f97ed8b34982: Pull complete
Digest: sha256:3a3fe1858ce6bd896613669ab669d14f13689b7cbcd3d456ba8e852810232942
Status: Downloaded newer image for cyclos/cyclos:latest
docker.io/cyclos/cyclos:latest
```

### Create the database

```bash
docker pull cyclos/cyclos
Using default tag: latest
latest: Pulling from cyclos/cyclos
71dca2167f9f: Pull complete
799297b6f921: Pull complete
c8c6b1f7c640: Pull complete
09a7603aa03d: Pull complete
50fe6d0cba0c: Pull complete
4b745a9fe8dc: Pull complete
49e6a519ff53: Pull complete
289b431299e5: Pull complete
ceee1b148da7: Pull complete
b26072927e6b: Pull complete
e46794033b1d: Pull complete
4f4fb700ef54: Pull complete
918f60c1e8fc: Pull complete
f97ed8b34982: Pull complete
Digest: sha256:3a3fe1858ce6bd896613669ab669d14f13689b7cbcd3d456ba8e852810232942
Status: Downloaded newer image for cyclos/cyclos:latest
docker.io/cyclos/cyclos:latest
```

### Create the network

```bash
docker network create cyclos-net
126c9c530eae69a1568ebc1574d5c986f825e1e6ecf987ff4882504dabf29985
```

### Create the database
I replaced the image **postgis/postgis** by **nickblah/postgis**
```bash
docker run -d \
    --name=cyclos-db \
    --net=cyclos-net \
    --hostname=cyclos-db \
    --restart=unless-stopped \
    -e POSTGRES_DB=cyclos \
    -e POSTGRES_USER=cyclos \
    -e POSTGRES_PASSWORD=cyclospwd \
    nickblah/postgis
ad3d5d7fbba6dad0d2ee5f98a829dd401572ca2bba499a2debbee83d3e535332
```

### Install Cyclos and set the database

```bash
docker run -d \
    --init \
    --name=cyclos-app \
    -p 80:8080 \
    --net=cyclos-net \
    --restart=unless-stopped \
    -e DB_HOST=cyclos-db \
    -e DB_NAME=cyclos \
    -e DB_USER=cyclos \
    -e DB_PASSWORD=cyclospwd \
    cyclos/cyclos
e4b1e98a7070e72347ba809a674d53944f96e3c6da62760c3221e8a011cd4567
```

###  Check Install 

```bash
docker ps | grep cyclos
e4b1e98a7070   cyclos/cyclos                 "catalina.sh run"        37 seconds ago   Up 36 seconds   0.0.0.0:80->8080/tcp   cyclos-app
ad3d5d7fbba6   nickblah/postgis              "docker-entrypoint.s…"   2 minutes ago    Up 2 minutes    5432/tcp               cyclos-db
```

###  Check Cyclos is up and running
Launch Cyclos UI on http://localhost