# nginx-ca-certificate-service-docker-compose

## Getting started

-   Clone the repository
-   Copy the `.env.example` file to `.env` and fill in the required environment variables.
-   Add CA certs to `/data/ca_certificate_service/ca`
    -   Recommended to bind mount a volume to `/data/ca_certificate_service/ca`
    -   Cert file names
        -   `CA.key`
        -   `CA.pem`
    -   If you need to generate CA certs
        -   `openssl genrsa -des3 -out CA.key 2048`
        -   `openssl req -x509 -new -nodes -key CA.key -sha256 -days 1825 -out CA.pem`
-   Add your CA.key passphrase to a file `/data/ca_certificate_service/secrets/CA`
    -   Recommended to bind mount a volume to `/data/ca_certificate_service/secrets/`
-   Copy `data/nginx/etc/nginx/conf.d/default.conf.example` to `data/nginx/etc/nginx/conf.d/default.conf`
-   Copy `data/nginx/etc/nginx/sites/site.conf.example` to `data/nginx/etc/nginx/sites/<YOUR_HOST>.conf` where `<YOUR_HOST>` is your domain name - like `hello-world.com.conf`.
    -   Edit the copied .conf file to replace all `${}` tags with your domain's configuration
        -   `${domain}` with your domain
        -   `${host}` with your server's IP address or domain
        -   `${port}` with your server's port

## Running the application

-   `bash run.sh up` to start the application
-   `bash run.sh down` to stop the application

## Add more domains

-   Copy `data/etc/nginx/sites/site.conf.example` to `data/etc/nginx/sites/<YOUR_HOST>.conf` where `<YOUR_HOST>` is your domain name - like `hello-world.com.conf`.
-   Edit the copied .conf file to replace `${DOMAIN}` with your domain

## Force immediate cert renewal

`docker exec nginx /bin/bash /app/scripts/site_certs.sh`

## Hot reload NGINX

`docker exec nginx nginx -s reload`
