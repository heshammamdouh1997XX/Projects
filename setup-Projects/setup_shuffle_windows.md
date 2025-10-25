# Install Shuffle.io in windows using Docker

1- Make sure you have Docker and git(for downloading) installed, and that you have a minimum of 4Gb of RAM available. More RAM = better.

2- Download Shuffle

`git clone https://github.com/Shuffle/Shuffle`

`cd Shuffle`

Fix prerequisites for the Opensearch database (Elasticsearch also works). This requires the shuffle-database folder to exist.
```
sudo chown -R 1000:1000 shuffle-database  # IF you get an error using 'chown', add the user first with 'sudo useradd opensearch'

sudo swapoff -a                           # Disable swap
```
Run `docker-compose.`

`docker compose up -d`

Recommended for Opensearch to work well

`sudo sysctl -w vm.max_map_count=262144             # https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html`

When you're done, go to the After installation step below.

# After installation

- After installation, go to http://localhost:3001 (or your servername - https is on port 3443
- Now set up your admin account (username & password). Shuffle doesn't have a default username and password.
- Sign in with the same Username & Password! Go to /apps and see if you have any apps yet. If not - you may need to configure proxies
-  Check out https://shuffler.io/docs/configuration as it has a lot of useful information to get started
    
   ## Production Setup
   

   https://shuffler.io/docs/configuration#production_readiness
