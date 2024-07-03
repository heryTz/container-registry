# Self-hosted Container Registry

## Setup

Clone this repo

```bash
git clone https://github.com/heryTz/container-registry.git
```

Create user

```bash
mkdir auth && \
docker run \
  --entrypoint htpasswd \
  httpd:2 -Bbn youruser yourpassword > auth/htpasswd
```

Launch docker

```bash
docker compose up -d
```

Make a reverse proxy. For example with [Caddy](https://caddyserver.com/):

```ini
# Add this line inside your /etc/caddy/Caddyfile
registry.yourdomain.com {
  reverse_proxy localhost:5000
  handle_path /dashboard* {
    reverse_proxy localhost:5001
  }
}
```

Url

- Registry server: <https://registry.yourdomain.com>
- Registry dashboard: <https://registry.yourdomain.com/dashboard>

## Usage

Login

```bash
docker login https://registry.yourdomain.com
```

Push image

```bash
# Tag your image with registry.yourdomain.com/your/image/uri
docker push registry.yourdomain.com/your/image/uri
```

Pull image

```bash
docker pull registry.yourdomain.com/your/image/uri
```

Delete unused image

```bash
# Inside the registry container
./garbage-collector.sh
```

## References

- Official documentation: <https://distribution.github.io/distribution/>