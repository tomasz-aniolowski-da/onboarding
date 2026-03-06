# MarkLogic Local Setup

## 1. Run MarkLogic in Docker

### Option A: Docker Compose (recommended)

Use the provided [docker-compose.yaml](docker-compose.yaml):

```sh
cd setup
docker compose up -d
```

### Option B: Docker run

```sh
docker run -d -it \
  --name sf-marklogic12 \
  --platform linux/amd64 \
  -p 8000:8000 \
  -p 8001:8001 \
  -p 8002:8002 \
  -p 3693:3693 \
  -p 3694:3694 \
  -v ./ml-data:/var/opt/MarkLogic \
  -v ./ml-space:/space \
  -v ./ml-backup:/backup \
  -e MARKLOGIC_INIT=true \
  -e MARKLOGIC_ADMIN_USERNAME=admin \
  -e MARKLOGIC_ADMIN_PASSWORD=admin \
  --restart unless-stopped \
  progressofficial/marklogic-db:latest-12
```

> Make sure your volume directories have full permissions: `chmod -R 777 ml-data ml-space ml-backup`

## 2. Set Up Access to DEV Server

You need a truststore to connect to the DEV MarkLogic instance over TLS.

### Prerequisites

```sh
sudo apt install openjdk-21-jre-headless   # provides keytool
```

### Download and import certificates

```sh
# Download the cert chain
echo | openssl s_client -connect development.marklogic.sf-dev.casinternal:3693 \
  -showcerts 2>/dev/null > /tmp/dev-certs.pem

# Split into individual certs
rm -f /tmp/dev-truststore.p12
awk '/-----BEGIN CERTIFICATE-----/{n++} n{print > "/tmp/dev-cert-" n ".pem"}' /tmp/dev-certs.pem

# Import each cert into a PKCS12 truststore
for i in 1 2 3; do
  cert="/tmp/dev-cert-${i}.pem"
  if [ -f "$cert" ]; then
    cn=$(openssl x509 -in "$cert" -noout -subject 2>/dev/null | sed 's/.*CN *= *//')
    echo "Importing cert $i: $cn"
    keytool -import -trustcacerts -alias "dev-cert-${i}" -file "$cert" \
      -keystore /tmp/dev-truststore.p12 -storetype PKCS12 \
      -storepass changeit -noprompt 2>&1
  fi
done
```

This creates `/tmp/dev-truststore.p12` (password: `changeit`).

## 3. Copy Data from DEV to Local

Uses [MarkLogic Flux](https://marklogic.github.io/flux/getting-started.html). Make sure `flux` is in your PATH (or use a full path to the binary).

```sh
flux copy \
  --host development.marklogic.sf-dev.casinternal \
  --port 3693 \
  --username admin \
  --password 'ml369!' \
  --auth-type DIGEST \
  --ssl-protocol TLSv1.2 \
  --truststore-path /tmp/dev-truststore.p12 \
  --truststore-password changeit \
  --truststore-type PKCS12 \
  --output-connection-string "admin:admin@localhost:3693" \
  --categories content,collections,permissions,metadata,quality \
  --batch-size 200 \
  --output-thread-count 64 \
  --log-progress 10000 \
  --no-snapshot
```
