# Drugs4Covid Knowledge Graph based on the EBOCA ontology


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
![Docker](https://img.shields.io/badge/docker-v20.10.17+-blue.svg)
![Nginx](https://img.shields.io/badge/nginx-v1.18+-blue.svg)
![GraphDB](https://img.shields.io/badge/graphDB-v10.0.2-blue.svg)
![Helio](https://img.shields.io/badge/helio-v0.3.13-blue.svg)
[![License](https://img.shields.io/badge/license-Apache2-green.svg)](https://www.apache.org/licenses/LICENSE-2.0)


## Quick Start
1. Install [Docker Engine](https://docs.docker.com/engine/install/) and [Docker Compose](https://docs.docker.com/compose/install/)
1. Clone this repo:
    ````
    git clone https://github.com/drugs4covid/KG.git
    ````
1. Move into the root directory:
    ````
    cd KG
    ````
1. Deploy the services via docker compose:
    ````
    docker compose up -d
    `````
1. GraphDB is available at: [http://127.0.0.1:7200](http://127.0.0.1:7200)
1. Helio Sparql Endpoint is available at: [http://127.0.0.1:8080](http://127.0.0.1:8080)

## Preparation of Reverse Proxy

1. Install [Nginx](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/) 
    ````
    sudo apt update
    sudo apt install nginx
    ````
    You can point your browser to your server IP address to see a welcome page ([http://localhost](http://localhost))
1. Setting up virtual host
    To set up virtual host, we need to update file `default` in `/etc/nginx/sites-enabled/` directory with the contents of file [nginx.sites_available]:
	  ```
	  server {
		listen 80 default_server;
		listen [::]:80 default_server;

		server_name d4c.linkeddata.es;

		location /rdf/ {
			#rewrite ^/rdf(.*) /$1 break;
			rewrite ^/rdf(.*) $1 break;
			proxy_pass http://127.0.0.1:7200;
		}

		location /rdf {
			rewrite ^/rdf(.*) /$1 break;
			proxy_pass http://127.0.0.1:7200;
		}

		location /sparql {
			#rewrite ^/sparql(.*) /$1 break;
			proxy_pass http://127.0.0.1:8002;
		}
	}
	  ```
1. Activating virtual host and testing results.
   To make our site working, simply validate the config file:
      ````
      sudo nginx -t
      ````
   and then restart Nginx service.
      ```
	  sudo service nginx restart
	  ```
## Customize a Docker-based Helio image
1. Move into the `helio` directory:
    ````
    cd helio
    ````
1. Download the application code from [here](https://delicias.dia.fi.upm.es/nextcloud/index.php/s/C2D9jzABAjy2Kq9)
1. Extract it:
    ````
    unzip application.zip
    ````
1. Build a new Docker image (e.g. `docker.build`):
    ````
    docker buildx build --platform linux/amd64,linux/arm64 --push -t librairy/helio:0.3 .
    ````
1. Run the new container: 
    ````
    docker run -it --rm -p 8080:8080 -e SERVER_PORT=8080 -e SERVER_ADDRESS=127.0.0.1 -e SERVER_USE-FORWARD-HEADERS=true librairy/helio:0.3
    ````