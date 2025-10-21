# Install thehive Using docker-compose 


- Create a file (docker-compose.yml):
```
version: '3'
services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.17.10
    environment:
      - discovery.type=single-node
    ports:
      - "9200:9200"

  cassandra:
    image: cassandra:4.1
    ports:
      - "9042:9042"

  thehive:
    image: thehiveproject/thehive4:latest
    depends_on:
      - elasticsearch
      - cassandra
    ports:
      - "9000:9000"
```
then Run `docker-compose up -d` 

OPEN your Browser `http://127.0.0.1:9000`
